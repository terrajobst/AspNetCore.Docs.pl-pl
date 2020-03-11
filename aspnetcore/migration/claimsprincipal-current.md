---
title: Migruj z ClaimsPrincipal. Current
author: mjrousos
description: Dowiedz się, jak przeprowadzić migrację z ClaimsPrincipal. Current, aby pobrać tożsamość i oświadczenia uwierzytelnionego użytkownika w ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: f7472f5b851d3869da3d26b881e276ce4ca004fb
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659314"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migruj z ClaimsPrincipal. Current

W projektach ASP.NET 4. x często było używane [ClaimsPrincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) do pobrania tożsamości i oświadczeń aktualnie uwierzytelnionego użytkownika. W ASP.NET Core ta właściwość nie jest już ustawiona. Kod, w zależności od tego, musi zostać zaktualizowany, aby można było uzyskać aktualną tożsamość uwierzytelnionego użytkownika w inny sposób.

## <a name="context-specific-data-instead-of-static-data"></a>Dane specyficzne dla kontekstu zamiast danych statycznych

W przypadku korzystania z ASP.NET Core nie są ustawiane wartości `ClaimsPrincipal.Current` i `Thread.CurrentPrincipal`. Te właściwości reprezentują stan statyczny, który ASP.NET Core zwykle unika. Zamiast tego architektura ASP.NET Core ma pobierać zależności (takie jak tożsamość bieżącego użytkownika) z kolekcji usług specyficznych dla kontekstu (przy użyciu modelu [iniekcji zależności](xref:fundamentals/dependency-injection) (di)). Co więcej, `Thread.CurrentPrincipal` jest statyczny wątku, dlatego może nie utrzymywać zmian w niektórych scenariuszach asynchronicznych (i `ClaimsPrincipal.Current` tylko wywołania `Thread.CurrentPrincipal` domyślnie).

Aby zrozumieć, jakie problemy członkowie statyczni mogą prowadzić do scenariuszy asynchronicznych, należy wziąć pod uwagę następujący fragment kodu:

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

Poprzedni przykładowy kod ustawia `Thread.CurrentPrincipal` i sprawdza jego wartość przed i po oczekiwaniu na wywołanie asynchroniczne. `Thread.CurrentPrincipal` jest specyficzny dla *wątku* , w którym jest ustawiony, a metoda może wznowić wykonywanie w innym wątku po oczekiwania. W związku z tym `Thread.CurrentPrincipal` jest obecny, gdy jest ona najpierw zaznaczona, ale ma wartość null po wywołaniu `await Task.Yield()`.

Uzyskanie informacji o tożsamości bieżącego użytkownika z kolekcji DI Service jest bardziej weryfikowalnee, ponieważ tożsamości testowe mogą być łatwo wstrzykiwane.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Pobierz bieżącego użytkownika w aplikacji ASP.NET Core

Istnieje kilka opcji pobierania `ClaimsPrincipal` bieżącego uwierzytelnionego użytkownika w ASP.NET Core zamiast `ClaimsPrincipal.Current`:

* **ControllerBase. User**. Kontrolery MVC mogą uzyskać dostęp do bieżącego uwierzytelnionego użytkownika przy użyciu ich właściwości [użytkownika](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) .
* **HttpContext. User**. Składniki z dostępem do bieżącego `HttpContext` (na przykład oprogramowania pośredniczącego) mogą pobrać `ClaimsPrincipal` bieżącego użytkownika z elementu [HttpContext. User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Przeszedł z elementu wywołującego**. Biblioteki bez dostępu do bieżącego `HttpContext` są często wywoływane z poziomu kontrolerów lub składników pośredniczących, a tożsamość bieżącego użytkownika jest przenoszona jako argument.
* **IHttpContextAccessor**. Projekt migrowany do ASP.NET Core może być zbyt duży, aby można było łatwo przekazać tożsamość bieżącego użytkownika do wszystkich potrzebnych lokalizacji. W takich przypadkach [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) może służyć jako obejście problemu. `IHttpContextAccessor` jest w stanie uzyskać dostęp do bieżącego `HttpContext` (jeśli taki istnieje). Jeśli jest używany przycisk DI, zobacz <xref:fundamentals/httpcontext>. Krótkoterminowe rozwiązanie do uzyskiwania informacji o tożsamości bieżącego użytkownika w kodzie, który nie został jeszcze zaktualizowany do pracy z ASP.NET Core architekturą o regulowanej mocy, będzie:

  * Udostępnij `IHttpContextAccessor` w kontenerze DI przez wywołanie [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) w `Startup.ConfigureServices`.
  * Pobierz wystąpienie `IHttpContextAccessor` podczas uruchamiania i Zapisz je w zmiennej statycznej. Wystąpienie jest udostępniane dla kodu, który wcześniej pobiera bieżącego użytkownika z właściwości statycznej.
  * Pobierz `ClaimsPrincipal` bieżącego użytkownika przy użyciu `HttpContextAccessor.HttpContext?.User`. Jeśli ten kod jest używany poza kontekstem żądania HTTP, `HttpContext` ma wartość null.

Ostatnia opcja, przy użyciu wystąpienia `IHttpContextAccessor` przechowywanego w zmiennej statycznej, jest sprzeczna z regułami ASP.NET Core (przed przyłożeniem przywołujących się do zależności statycznych). Zaplanuj ostatecznie pobranie wystąpień `IHttpContextAccessor` z iniekcji zależności. Pomocnik statyczny może być przydatnego mostka, chociaż podczas migrowania dużych istniejących aplikacji ASP.NET, które wcześniej korzystały z `ClaimsPrincipal.Current`.
