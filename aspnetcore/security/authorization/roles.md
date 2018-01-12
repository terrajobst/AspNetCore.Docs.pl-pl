---
title: Autoryzacja oparta na rolach
author: rick-anderson
description: "Ten dokument pokazuje, jak ograniczyć dostęp kontroler i akcja MVC ASP.NET Core przez przekazanie do atrybutu autoryzacji ról."
keywords: Platformy ASP.NET Core autoryzacji, role
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 26babef1a296aaa1fa11f36d30c4d911d73808ce
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="role-based-authorization"></a><span data-ttu-id="37c79-104">Autoryzacja oparta na rolach</span><span class="sxs-lookup"><span data-stu-id="37c79-104">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="37c79-105">Po utworzeniu tożsamości może należeć do jednej lub więcej ról.</span><span class="sxs-lookup"><span data-stu-id="37c79-105">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="37c79-106">Na przykład Tracy może należeć do ról administratora i użytkownika, przy jednoczesnym Scott może należeć tylko do roli użytkownika.</span><span class="sxs-lookup"><span data-stu-id="37c79-106">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="37c79-107">Jak te role są tworzone i zarządzane zależy od magazynu zapasowego procesu autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="37c79-107">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="37c79-108">Role są widoczne dla projektanta za pośrednictwem [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) metoda [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) klasy.</span><span class="sxs-lookup"><span data-stu-id="37c79-108">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="37c79-109">Dodawanie ról kontroli</span><span class="sxs-lookup"><span data-stu-id="37c79-109">Adding role checks</span></span>

<span data-ttu-id="37c79-110">Sprawdzanie autoryzacji opartej na rolach są deklaratywne&mdash;dewelopera umieszcza je w ramach ich kodu, kontrolera lub akcji w obrębie kontrolera, określanie ról, które bieżący użytkownik musi należeć do dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="37c79-110">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="37c79-111">Na przykład następujący kod ogranicza dostęp do wszystkich akcji w `AdministrationController` dla użytkowników, którzy są członkami `Administrator` roli:</span><span class="sxs-lookup"><span data-stu-id="37c79-111">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="37c79-112">Możesz określić wiele ról rozdzielana przecinkami lista:</span><span class="sxs-lookup"><span data-stu-id="37c79-112">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="37c79-113">Ten kontroler będą tylko przez użytkowników, którzy są członkami dostępne z `HRManager` roli lub `Finance` roli.</span><span class="sxs-lookup"><span data-stu-id="37c79-113">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="37c79-114">W przypadku zastosowania wielu atrybutów podczas uzyskiwania dostępu do użytkownika musi być członkiem wszystkich ról określony; Poniższy przykład wymaga, aby użytkownik musi być elementem członkowskim `PowerUser` i `ControlPanelUser` roli.</span><span class="sxs-lookup"><span data-stu-id="37c79-114">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="37c79-115">Można jeszcze bardziej ograniczyć dostęp przez zastosowanie dodatkowych ról autoryzacji atrybuty na poziomie akcji:</span><span class="sxs-lookup"><span data-stu-id="37c79-115">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="37c79-116">W poprzednich członków fragment kodu z `Administrator` roli lub `PowerUser` roli mogą uzyskiwać dostęp do kontrolera i `SetTime` akcji, ale tylko członkowie `Administrator` dostęp `ShutDown` akcji.</span><span class="sxs-lookup"><span data-stu-id="37c79-116">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="37c79-117">Można również zablokować kontrolerem, ale zezwalaj na anonimowe, nieuwierzytelnione dostęp do poszczególnych działań.</span><span class="sxs-lookup"><span data-stu-id="37c79-117">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

## <a name="policy-based-role-checks"></a><span data-ttu-id="37c79-118">Sprawdza rolę oparte na zasadach</span><span class="sxs-lookup"><span data-stu-id="37c79-118">Policy based role checks</span></span>

<span data-ttu-id="37c79-119">Wymaganiami dotyczącymi roli można również wyrazić za pomocą nowej składni zasady, których projektant rejestruje zasad podczas uruchamiania w ramach konfiguracji usługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="37c79-119">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="37c79-120">Zwykle występuje to `ConfigureServices()` w Twojej *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="37c79-120">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="37c79-121">Zasady są stosowane przy użyciu `Policy` właściwość `AuthorizeAttribute` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="37c79-121">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="37c79-122">Jeśli chcesz określić wiele ról dozwolonych w wymaganie, określ je jako parametry `RequireRole` metody:</span><span class="sxs-lookup"><span data-stu-id="37c79-122">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="37c79-123">W tym przykładzie autoryzuje użytkowników, którzy należą do `Administrator`, `PowerUser` lub `BackupAdministrator` ról.</span><span class="sxs-lookup"><span data-stu-id="37c79-123">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
