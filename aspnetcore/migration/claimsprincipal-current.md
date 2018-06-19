---
title: Migracja z ClaimsPrincipal.Current
author: mjrousos
description: Dowiedz się, jak przeprowadzić migrację od ClaimsPrincipal.Current można pobrać bieżącego użytkownika uwierzytelnionego tożsamości i oświadczeń z platformy ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851595"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migracja z ClaimsPrincipal.Current

Projekty programu ASP.NET został wspólnego do użycia [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) można pobrać bieżącego uwierzytelnić tożsamości i oświadczeń użytkownika. W ASP.NET Core nie jest już wartość tej właściwości. Kod, który został od niego zależne wymaga aktualizacji można uzyskać tożsamości bieżący użytkownik uwierzytelniony w inny sposób.

## <a name="context-specific-data-instead-of-static-data"></a>Dane specyficzne dla kontekstu zamiast danych statycznych

Korzystając z platformy ASP.NET Core wartości obu `ClaimsPrincipal.Current` i `Thread.CurrentPrincipal` nie są skonfigurowane. Te właściwości zarówno reprezentują Państwo statycznych, które zazwyczaj pozwala uniknąć platformy ASP.NET Core. Zamiast tego jest architektura platformy ASP.NET Core można pobrać zależności (np. Bieżąca tożsamość użytkownika) z usługi zależne od kontekstu kolekcji (przy użyciu jego [iniekcji zależności](xref:fundamentals/dependency-injection) modelu (Podpisane)). Co to jest więcej, `Thread.CurrentPrincipal` jest wątku statyczne, więc może zmienić się zmiany w niektórych scenariuszach asynchroniczne (i `ClaimsPrincipal.Current` po prostu wywołuje `Thread.CurrentPrincipal` domyślnie).

Zrozumienie gamy problemów wątku statycznych elementów członkowskich może prowadzić do w scenariuszach asynchronicznego, rozważ poniższy fragment kodu:

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

Poprzedni zestawy kod przykładowy `Thread.CurrentPrincipal` i sprawdza jego wartość przed i po oczekiwanie na wywołanie asynchroniczne. `Thread.CurrentPrincipal` dotyczy *wątku* na którym jest ustawiona, a metoda jest prawdopodobne wznowić pracę w innym wątku po await. W rezultacie `Thread.CurrentPrincipal` jest obecna, gdy po raz pierwszy jest wyewidencjonowany ale ma wartość null po wywołaniu `await Task.Yield()`.

Pobieranie Bieżąca tożsamość użytkownika z kolekcji usługi Podpisane aplikacji jest więcej testować, ponieważ test tożsamości mogą łatwo dodane.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Pobieranie bieżącego użytkownika w aplikacji platformy ASP.NET Core

Dostępnych jest kilka opcji pobierania bieżącego użytkownika uwierzytelnionego `ClaimsPrincipal` w ASP.NET Core zamiast `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Kontrolerów MVC mogą uzyskiwać dostęp do bieżącej uwierzytelnionego użytkownika z ich [użytkownika](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) właściwości.
* **HttpContext.User**. Składniki z dostępem do bieżącego `HttpContext` (oprogramowanie pośredniczące, na przykład) można pobrać bieżącego użytkownika `ClaimsPrincipal` z [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Przekazano z wywołującego**. Biblioteki bez dostępu do bieżącego `HttpContext` są często nazywane z kontrolerów lub składników oprogramowania pośredniczącego i może zawierać przekazanego jako argument tożsamości bieżącego użytkownika.
* **IHttpContextAccessor**. Projekt ASP.NET jest migrowany do platformy ASP.NET Core może być zbyt duża, aby łatwo przekazywać Bieżąca tożsamość użytkownika do wszystkich niezbędnych lokalizacji. W takich przypadkach [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) mogą być używane jako obejście tego problemu. `IHttpContextAccessor` jest w stanie uzyskać dostępu do bieżącego `HttpContext` (jeśli istnieje). Krótkoterminowe rozwiązania do uzyskiwania tożsamości bieżącego użytkownika w kodzie, które jeszcze nie zostały zaktualizowane do pracy z architekturą oparte na Podpisane platformy ASP.NET Core będzie:

  * Wprowadź `IHttpContextAccessor` dostępne w kontenerze Podpisane przez wywołanie metody [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) w `Startup.ConfigureServices`.
  * Pobranie wystąpienia `IHttpContextAccessor` podczas uruchamiania i zapisz go w zmienna statyczna. Wystąpienie jest udostępniana do kodu, który został wcześniej pobieranie bieżącego użytkownika z właściwości statycznej.
  * Pobieranie bieżącego użytkownika `ClaimsPrincipal` przy użyciu `HttpContextAccessor.HttpContext?.User`. Jeśli ten kod jest używana poza kontekstem żądanie HTTP `HttpContext` ma wartość null.

Ostatni opcję przy użyciu `IHttpContextAccessor`, jest to sprzeczne z zasadami platformy ASP.NET Core (preferowanie wprowadzony zależności statycznej zależności). Planowanie ostatecznie usunięcie zależności od statycznych `IHttpContextAccessor` pomocnika. Można ją mostka przydatne, jednak podczas migrowania dużej istniejące aplikacje ASP.NET, które były wcześniej używane `ClaimsPrincipal.Current`.
