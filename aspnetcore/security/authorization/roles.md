---
title: Autoryzacja oparta na rolach
author: rick-anderson
description: "Ten dokument pokazuje, jak ograniczyć dostęp kontroler i akcja MVC ASP.NET Core przez przekazanie do atrybutu autoryzacji ról."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: 764d1fcc384fc8370d1a536f9609333de6bd4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="role-based-authorization"></a>Autoryzacja oparta na rolach

<a name="security-authorization-role-based"></a>

Po utworzeniu tożsamości może należeć do jednej lub więcej ról. Na przykład Tracy może należeć do ról administratora i użytkownika, przy jednoczesnym Scott może należeć tylko do roli użytkownika. Jak te role są tworzone i zarządzane zależy od magazynu zapasowego procesu autoryzacji. Role są widoczne dla projektanta za pośrednictwem [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) metoda [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) klasy.

## <a name="adding-role-checks"></a>Dodawanie ról kontroli

Sprawdzanie autoryzacji opartej na rolach są deklaratywne&mdash;dewelopera umieszcza je w ramach ich kodu, kontrolera lub akcji w obrębie kontrolera, określanie ról, które bieżący użytkownik musi należeć do dostępu do żądanego zasobu.

Na przykład następujący kod ogranicza dostęp do wszystkich akcji w `AdministrationController` dla użytkowników, którzy są członkami `Administrator` roli:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Możesz określić wiele ról rozdzielana przecinkami lista:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Ten kontroler będą tylko przez użytkowników, którzy są członkami dostępne z `HRManager` roli lub `Finance` roli.

W przypadku zastosowania wielu atrybutów podczas uzyskiwania dostępu do użytkownika musi być członkiem wszystkich ról określony; Poniższy przykład wymaga, aby użytkownik musi być elementem członkowskim `PowerUser` i `ControlPanelUser` roli.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Można jeszcze bardziej ograniczyć dostęp przez zastosowanie dodatkowych ról autoryzacji atrybuty na poziomie akcji:

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

W poprzednich członków fragment kodu z `Administrator` roli lub `PowerUser` roli mogą uzyskiwać dostęp do kontrolera i `SetTime` akcji, ale tylko członkowie `Administrator` dostęp `ShutDown` akcji.

Można również zablokować kontrolerem, ale zezwalaj na anonimowe, nieuwierzytelnione dostęp do poszczególnych działań.

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>Sprawdza rolę oparte na zasadach

Wymaganiami dotyczącymi roli można również wyrazić za pomocą nowej składni zasady, których projektant rejestruje zasad podczas uruchamiania w ramach konfiguracji usługi autoryzacji. Zwykle występuje to `ConfigureServices()` w Twojej *Startup.cs* pliku.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

Zasady są stosowane przy użyciu `Policy` właściwość `AuthorizeAttribute` atrybutu:

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Jeśli chcesz określić wiele ról dozwolonych w wymaganie, określ je jako parametry `RequireRole` metody:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

W tym przykładzie autoryzuje użytkowników, którzy należą do `Administrator`, `PowerUser` lub `BackupAdministrator` ról.
