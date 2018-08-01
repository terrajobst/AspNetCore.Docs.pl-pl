---
title: Autoryzacja oparta na rolach w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ograniczyć dostęp do akcji i kontrolerów platformy ASP.NET Core, przekazując ról z atrybutem autoryzacji.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 59753b90d3196b0bc16d4963f45b995f5108bc8b
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356678"
---
# <a name="role-based-authorization-in-aspnet-core"></a>Autoryzacja oparta na rolach w programie ASP.NET Core

<a name="security-authorization-role-based"></a>

Po utworzeniu tożsamości może on należeć do co najmniej jedną rolę. Na przykład Tracy mogą należeć do ról administratora i użytkownika, o ile Scott może wyłącznie należeć do roli użytkownika. Jak te role są tworzone i zarządzane zależy od magazyn zapasowy procesu autoryzacji. Role są widoczne dla deweloperów za pośrednictwem [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) metody [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) klasy.

::: moniker range=">= aspnetcore-2.0"

> [!IMPORTANT]
> W tym temacie jest **nie** dotyczą stron Razor. Strony razor obsługuje [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) i [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter). Aby uzyskać więcej informacji, zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter).

::: moniker-end

## <a name="adding-role-checks"></a>Dodawanie kontroli roli

Sprawdzanie autoryzacji opartej na rolach są deklaratywne&mdash;Deweloper umieszcza je w ramach ich kodować, kontrolera lub akcji w kontrolerze, określając role, które bieżący użytkownik musi należeć do dostępu do żądanego zasobu.

Na przykład, poniższy kod ogranicza dostęp do wszystkich działań na `AdministrationController` dla użytkowników, którzy są członkami `Administrator` roli:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Można określić wiele ról jako listę rozdzielonych przecinkami:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Ten kontroler będą tylko dostępne przez użytkowników, którzy są członkami z `HRManager` roli lub `Finance` roli.

Jeśli zastosujesz wiele atrybutów, a następnie uzyskiwanie dostępu do użytkownika musi być członkiem wszystkich ról, które są określone; Poniższy przykład wymaga, że użytkownik musi być członkiem zarówno `PowerUser` i `ControlPanelUser` roli.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Można zawęzić dostęp przez zastosowanie dodatkowych ról autoryzacji atrybuty na poziomie akcji:

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

W poprzednich składowych fragment kodu z `Administrator` roli lub `PowerUser` rola umożliwia dostęp do kontrolera i `SetTime` akcji, ale tylko członkowie `Administrator` rola umożliwia dostęp do `ShutDown` akcji.

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

## <a name="policy-based-role-checks"></a>Uprawnienia ról oparte na zasadach

Wymagania roli można również wyrazić za pomocą nowej składni zasad, których Deweloper rejestruje zasady przy uruchomieniu jako część konfiguracji usługi autoryzacji. Zwykle dzieje się to `ConfigureServices()` w swojej *Startup.cs* pliku.

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

Jeśli chcesz określić wiele ról dozwolonych w zapotrzebowania, a następnie określić je jako parametry `RequireRole` metody:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

W tym przykładzie autoryzowania użytkowników, którzy należą do `Administrator`, `PowerUser` lub `BackupAdministrator` ról.
