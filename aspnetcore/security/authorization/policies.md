---
title: Niestandardowe autoryzacji opartych na zasadach
author: rick-anderson
description: "Tym dokumencie opisano sposób tworzenia i używania programów obsługi zasad autoryzacji niestandardowej w aplikacji platformy ASP.NET Core."
keywords: Platformy ASP.NET Core autoryzacji, niestandardowych zasad, zasady autoryzacji
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a>Niestandardowe autoryzacji opartych na zasadach

<a name="security-authorization-policies-based"></a>

Poniżej obejmuje [autoryzacji ról](roles.md) i [oświadczeń autoryzacji](claims.md) wprowadzić korzystanie z wymaganiem, obsługa wymagania i wstępnie skonfigurowanych zasad. Te bloki konstrukcyjne umożliwiają express ocen autoryzacji w kodzie, co pozwala na pełniejsze, wielokrotnego użytku i struktury łatwością testować autoryzacji.

Zasady autoryzacji jest składają się z co najmniej jednego wymagania i zarejestrowane podczas uruchamiania aplikacji w ramach konfiguracji usługi autoryzacji, w `ConfigureServices` w *Startup.cs* pliku.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

W tym miejscu można zauważyć, że z jednym wymaganie, że o minimalnym wieku, która jest przekazywana jako parametr wymogiem tworzone są zasady "Over21".

Zasady są stosowane przy użyciu `Authorize` atrybutu, określając nazwę zasady, na przykład;

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a>Wymagania

Wymaganie autoryzacji to zbiór parametrów danych, które zasady można użyć do oceny, bieżący podmiot zabezpieczeń użytkownika. W zasadach minimalnego wieku wymaganie, który mamy jest jeden parametr, minimalnym wieku. Wymaganie musi implementować `IAuthorizationRequirement`. To jest pusta, interfejsem znacznika. Sparametryzowane minimalny wymagany wiek mogą zostać zaimplementowane w następujący sposób:

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

Wymagania nie musi być danych lub właściwości.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Programy obsługi autoryzacji

Program obsługi autoryzacji jest odpowiedzialny za oceny wszystkie właściwości wymagane. Program obsługi autoryzacji musi ich oceny na podstawie podanych `AuthorizationHandlerContext` zdecydować, czy jest dozwolone autoryzacji. Wymóg może mieć [wielu obsług](policies.md#security-authorization-policies-based-multiple-handlers). Programy obsługi musi dziedziczyć `AuthorizationHandler<T>` gdzie T jest konieczność obsługi.

<a name="security-authorization-handler-example"></a>

Program obsługi minimalnego wieku może wyglądać następująco:

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

W powyższym kodzie najpierw szukamy aby zobaczyć, czy bieżący użytkownik główna ma datę urodzenia oświadczenia, które zostały wystawione przez wystawcę wiemy i zaufania. Jeśli brakuje oświadczenie tak, aby firma Microsoft nie można autoryzować, dlatego zostanie zwrócona. Jeśli mamy oświadczenia, możemy ustalić przyczynę wiek użytkownika i, jeżeli spełniają minimalnego wieku przekazany przez wymaganie następnie autoryzacji przebiegło pomyślnie. Po pomyślnej autoryzacji nazywamy `context.Succeed()` przekazywanie wymaganie, która została pomyślnie jako parametr.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Rejestracja programu obsługi
Programy obsługi musi być zarejestrowany w kolekcji usługi podczas konfiguracji, na przykład;

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

Każdy program obsługi jest dodawany do kolekcji usług przy użyciu `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` przekazywanie w klasie obsługi.

## <a name="what-should-a-handler-return"></a>Co powinna zwracać obsługi?

Można zobaczyć w naszym [przykład obsługi](policies.md#security-authorization-handler-example) który `Handle()` — metoda nie ma zwracanych wartości, w jaki sposób możemy oznaczają powodzenie lub niepowodzenie?

* Program obsługi oznacza Powodzenie przez wywołanie metody `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, która została pomyślnie zweryfikowana.

* Program obsługi nie musi do obsługi błędów ogólnie rzecz biorąc, zgodnie z innych programów obsługi na ten sam wymóg może się powieść.

* Aby zagwarantować awarii, nawet w przypadku pomyślnego innych programów obsługi dla wymagania, należy wywołać `context.Fail`.

Niezależnie od wywołana wewnątrz obsługi sieci obsługi wszystkie wymagane będzie wywoływany, gdy zasady wymaga to wymaganie. Dzięki temu wymagania dotyczące efekty uboczne, takich jak rejestrowanie, które będą zawsze miały miejsce nawet wtedy, gdy `context.Fail()` została wywołana w innego programu obsługi.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Dlaczego może być wielu obsług dla wymagania?

W przypadkach, w którym ma oceny na **lub** podstawy wdrożenia wielu obsług dla pojedynczego wymagania. Na przykład firma Microsoft ma drzwi, które mogły być otwierane tylko w przypadku kart klucza. Jeśli karta klucza pozostanie w domu recepcjonista drukuje tymczasowego naklejce i otwiera drzwi. W tym scenariuszu należy wymaganiem pojedynczego *EnterBuilding*, ale wielu obsług, każda z nich badanie pojedynczego wymaganie.

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Teraz, zakładając, że oba programy obsługi są [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) po ocenia zasady `EnterBuildingRequirement` Jeśli albo obsługi zakończy się powodzeniem oceny zasad powiedzie się.

## <a name="using-a-func-to-fulfill-a-policy"></a>Przy użyciu func do zrealizowania zasady

Może być zmieniana spełniających zasady w przypadku prostego można wyrazić w kodzie. Można podać po prostu `Func<AuthorizationHandlerContext, bool>` przy konfigurowaniu zasad z `RequireAssertion` konstruktora zasad.

Na przykład poprzedniej `BadgeEntryHandler` można ponownie zapisać w następujący sposób:

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a>Uzyskiwanie dostępu do kontekstu żądania MVC w obsłudze

`Handle` Metoda musi implementować obsługi autoryzacji ma dwa parametry `AuthorizationContext` i `Requirement` są obsługi. Struktury, np. MVC lub Jabbr są także dodać dowolny obiekt, aby `Resource` właściwość `AuthorizationContext` do przekazywania dodatkowych informacji.

Na przykład MVC przekazuje wystąpienie `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` we właściwości zasobu, którego uzyskano dostęp element HttpContext RouteData i wszystko, co zapewnia else MVC.

Korzystanie z `Resource` właściwość jest tylko dla struktury. Korzystając z informacji w `Resource` właściwości ograniczy zasad autoryzacji do określonej struktury. Należy rzutować `Resource` za pomocą właściwości `as` — słowo kluczowe i sprawdź rzutowanie ma się powieść, aby upewnić się, kod nie awarii z `InvalidCastExceptions` uruchomienia na innych platformach;

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
