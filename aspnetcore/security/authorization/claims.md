---
title: "Autoryzacji opartej na oświadczeniach"
author: rick-anderson
description: "Tym dokumencie opisano sposób dodawania oświadczeń sprawdzeń autoryzacji w aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: 870bdf79abc2c94745ab5da6997a37ed0e4ea4e2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="claims-based-authorization"></a><span data-ttu-id="3a080-103">Autoryzacji opartej na oświadczeniach</span><span class="sxs-lookup"><span data-stu-id="3a080-103">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="3a080-104">Po utworzeniu tożsamości może być przypisana co najmniej jednego oświadczenia wystawione przez zaufany.</span><span class="sxs-lookup"><span data-stu-id="3a080-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="3a080-105">Oświadczenie para reprezentujący jakie podmiot jest wartość nazwy, nie jakie tematu może.</span><span class="sxs-lookup"><span data-stu-id="3a080-105">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="3a080-106">Na przykład masz prawa jazdy, wystawiony przez urząd lokalny licencji jazdy.</span><span class="sxs-lookup"><span data-stu-id="3a080-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="3a080-107">W sterowniku licencji znajdują się datę urodzenia.</span><span class="sxs-lookup"><span data-stu-id="3a080-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="3a080-108">W takim przypadku nazwa oświadczenia jest `DateOfBirth`, wartość oświadczenia będą datę urodzenia, na przykład `8th June 1970` i Wystawca byłaby pobudzenie urzędu licencji.</span><span class="sxs-lookup"><span data-stu-id="3a080-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="3a080-109">Autoryzacji oświadczeń, w najprostszym sprawdza wartość oświadczenia i zezwala na dostęp do zasobu na podstawie tej wartości.</span><span class="sxs-lookup"><span data-stu-id="3a080-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="3a080-110">Na przykład, jeśli chcesz uzyskać dostęp do klub nocy proces autoryzacji. może to być:</span><span class="sxs-lookup"><span data-stu-id="3a080-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="3a080-111">Drzwi urzędnika oceni wartość daty urodzenia oświadczeń i określa, czy ufają wystawcy (pobudzenie urzędu licencji) zanim zostanie przyznany dostęp.</span><span class="sxs-lookup"><span data-stu-id="3a080-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="3a080-112">Tożsamość może zawierać wielu oświadczeń z wieloma wartościami i może zawierać wielu oświadczenia tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="3a080-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="3a080-113">Dodawanie oświadczenia kontroli</span><span class="sxs-lookup"><span data-stu-id="3a080-113">Adding claims checks</span></span>

<span data-ttu-id="3a080-114">Oświadczenia deklaratywne sprawdzeń autoryzacji na podstawie — dewelopera umieszcza je ich kodem, kontrolera lub akcji w obrębie kontrolera, określając oświadczenia, które bieżący użytkownik musi posiadać i opcjonalnie wartość oświadczenia musi posiadać dostępu żądany zasób.</span><span class="sxs-lookup"><span data-stu-id="3a080-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="3a080-115">Oświadczeń wymagania są oparte na zasadach, deweloper musi kompilacji i zarejestruj zasad, przedstawiając wymagania oświadczeń.</span><span class="sxs-lookup"><span data-stu-id="3a080-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="3a080-116">Najprostszy typ oświadczenia zasad szuka obecności oświadczenia i nie zostanie zaewidencjonowane wartość.</span><span class="sxs-lookup"><span data-stu-id="3a080-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="3a080-117">Należy najpierw skompilować i zarejestrować zasad.</span><span class="sxs-lookup"><span data-stu-id="3a080-117">First you need to build and register the policy.</span></span> <span data-ttu-id="3a080-118">Ma to miejsce w ramach konfiguracji usługi autoryzacji, które zwykle bierze udział w `ConfigureServices()` w Twojej *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="3a080-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

<span data-ttu-id="3a080-119">W takim przypadku `EmployeeOnly` zasad sprawdza obecność `EmployeeNumber` oświadczeń w bieżącej tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3a080-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="3a080-120">Następnie Zastosuj zasad przy użyciu opcji `Policy` właściwość `AuthorizeAttribute` atrybutu, aby określić nazwę zasady;</span><span class="sxs-lookup"><span data-stu-id="3a080-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="3a080-121">`AuthorizeAttribute` Atrybut można stosować do całego kontrolera, w tym wystąpieniu tylko tożsamości pasujących zasady będą miały dostęp do dowolnej akcji na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="3a080-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="3a080-122">Jeśli masz kontroler, która jest chroniona przez `AuthorizeAttribute` atrybutu, ale chcesz zezwolić na dostęp anonimowy do określonej akcji, należy zastosować `AllowAnonymousAttribute` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3a080-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

<span data-ttu-id="3a080-123">Większość oświadczenia mają wartość.</span><span class="sxs-lookup"><span data-stu-id="3a080-123">Most claims come with a value.</span></span> <span data-ttu-id="3a080-124">Można określić listę dozwolonych wartości podczas tworzenia zasad.</span><span class="sxs-lookup"><span data-stu-id="3a080-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="3a080-125">Poniższy przykład tylko powiedzie się dla pracowników, których liczba pracowników była 1, 2, 3, 4 lub 5.</span><span class="sxs-lookup"><span data-stu-id="3a080-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="3a080-126">Wiele ocena zasad</span><span class="sxs-lookup"><span data-stu-id="3a080-126">Multiple Policy Evaluation</span></span>

<span data-ttu-id="3a080-127">Jeśli wiele zasad można zastosować do kontrolera lub akcji, wszystkie zasady muszą upłynąć, zanim zostanie przyznany dostęp.</span><span class="sxs-lookup"><span data-stu-id="3a080-127">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="3a080-128">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3a080-128">For example:</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

<span data-ttu-id="3a080-129">W powyższym przykładzie wszystkie tożsamości, która spełnia `EmployeeOnly` zasady mogą uzyskiwać dostęp do `Payslip` akcji, jak te zasady są wymuszane na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="3a080-129">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="3a080-130">Jednak aby można było wywołać `UpdateSalary` akcji, które należy spełnić tożsamość *zarówno* `EmployeeOnly` zasad i `HumanResources` zasad.</span><span class="sxs-lookup"><span data-stu-id="3a080-130">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="3a080-131">Jeśli chcesz bardziej złożone zasady, takie jak pobieranie daty urodzenia oświadczenia, obliczanie wieku z niego, a następnie sprawdzanie wiek to 21 lub starsze, a następnie należy napisać [zasady niestandardowe programy obsługi](policies.md).</span><span class="sxs-lookup"><span data-stu-id="3a080-131">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
