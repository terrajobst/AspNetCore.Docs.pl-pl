---
title: Migracja z ClaimsPrincipal.Current
author: mjrousos
description: Dowiedz się, jak przeprowadzić migrację od ClaimsPrincipal.Current do pobrania aktualnego użytkownika uwierzytelnionego tożsamości i oświadczenia w programie ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 35c3389798041e141c45bf0a76fa9d7285212768
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757307"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migracja z ClaimsPrincipal.Current

W projektach programów ASP.NET 4.x, było często używa się [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) można pobrać bieżącego uwierzytelnienia oświadczenia i tożsamości użytkownika. W programie ASP.NET Core ta właściwość jest już ustawiona. Kod, który został z niego zależne musi zostać zaktualizowany tożsamości aktualnego użytkownika uwierzytelnionego realizowane na różne sposoby.

## <a name="context-specific-data-instead-of-static-data"></a>Dane zależne od kontekstu, zamiast danych statycznych

Korzystając z platformy ASP.NET Core, wartości obu `ClaimsPrincipal.Current` i `Thread.CurrentPrincipal` nie są ustawione. Te właściwości, oba reprezentują Państwo statyczne, które zazwyczaj pozwala uniknąć platformy ASP.NET Core. Zamiast tego architektury programu ASP.NET Core jest pobranie zależności (np. tożsamość bieżącego użytkownika) z usługi zależne od kontekstu kolekcji (przy użyciu jego [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) modelu (DI)). Co to jest więcej, `Thread.CurrentPrincipal` jest wątek statyczne, więc może mogą się zmian w niektórych scenariuszach asynchroniczne (i `ClaimsPrincipal.Current` po prostu wywołuje funkcję `Thread.CurrentPrincipal` domyślnie).

Aby poznać różne rodzaje problemów wątku statycznych elementów członkowskich może prowadzić do na scenariusze asynchroniczne, należy wziąć pod uwagę następujący fragment kodu:

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

Poprzedni zestawów kodu przykładowej `Thread.CurrentPrincipal` i sprawdza, czy jego wartości przed i po nim oczekujące na wywołania asynchronicznego. `Thread.CurrentPrincipal` dotyczy *wątku* na którym jest ustawiona, a metoda ta jest prawdopodobne wznowić wykonywanie w innym wątku po await. W związku z tym `Thread.CurrentPrincipal` jest obecny, kiedy go najpierw jest zaznaczone, ale ma wartość null, po wywołaniu `await Task.Yield()`.

Pobieranie tożsamość bieżącego użytkownika z kolekcji usługi DI aplikacji jest bardziej sprawdzalnego działa zgodnie, ponieważ test tożsamości można łatwo wprowadzony.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Pobieranie bieżącego użytkownika w aplikacji ASP.NET Core

Dostępnych jest kilka opcji do pobierania aktualnego użytkownika uwierzytelnionego `ClaimsPrincipal` w programie ASP.NET Core, zamiast `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Kontrolerów MVC mogą uzyskiwać dostęp do aktualnego użytkownika uwierzytelnionego przy użyciu ich [użytkownika](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) właściwości.
* **HttpContext.User**. Składniki z dostępem do bieżącego `HttpContext` (oprogramowanie pośredniczące, na przykład) można uzyskać bieżącego użytkownika `ClaimsPrincipal` z [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Przekazywane z obiektu wywołującego**. Biblioteki bez dostępu do bieżącego `HttpContext` są często wywoływane z kontrolerami lub składników oprogramowania pośredniczącego i może mieć tożsamość bieżącego użytkownika przekazywany jako argument.
* **IHttpContextAccessor**. Projekt migracji do platformy ASP.NET Core może być zbyt duża, aby łatwo przekazywać tożsamość bieżącego użytkownika do wszystkich wymaganych lokalizacjach. W takich przypadkach [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) mogą być używane jako obejście tego problemu. `IHttpContextAccessor` możliwość dostępu bieżącego `HttpContext` (jeśli istnieje). Rozwiązaniem krótkoterminowym pobranie tożsamość bieżącego użytkownika w kodzie, który jeszcze nie zostały zaktualizowane do pracy przy użyciu architektury oparte na DI platformy ASP.NET Core będzie:

  * Wprowadź `IHttpContextAccessor` dostępne w kontenerze DI przez wywołanie metody [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) w `Startup.ConfigureServices`.
  * Pobierz wystąpienia `IHttpContextAccessor` podczas uruchamiania i zapisz go w zmiennej statycznej. Wystąpienia staje się dostępny do kodu, który został wcześniej pobieranie bieżącego użytkownika z właściwości statycznej.
  * Pobieranie bieżącego użytkownika `ClaimsPrincipal` przy użyciu `HttpContextAccessor.HttpContext?.User`. Jeśli ten kod jest używany poza kontekstem żądania HTTP `HttpContext` ma wartość null.

Końcowe opcji, za pomocą `IHttpContextAccessor`, jest sprzeczne z zasadami platformy ASP.NET Core (preferowanie wprowadzonego zależności zależności statyczne). Planowanie ostatecznie wyeliminować zależność od statycznej `IHttpContextAccessor` pomocnika. Można go przydatne most, jednak podczas migrowania dużych istniejących aplikacji programu ASP.NET, które były wcześniej używane `ClaimsPrincipal.Current`.
