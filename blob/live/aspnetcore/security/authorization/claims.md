---
title: "Autoryzacji opartej na oświadczeniach"
author: rick-anderson
description: "Tym dokumencie opisano sposób dodawania oświadczeń sprawdzeń autoryzacji w aplikacji platformy ASP.NET Core."
keywords: "Platformy ASP.NET Core autoryzacji oświadczeń"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 737be5cd-3511-4f1c-b0ce-65403fb5eed3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: eebaddabdd360f34b6ff44e8f4f9f1f10fda6406
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="claims-based-authorization"></a><span data-ttu-id="8218d-104">Autoryzacji opartej na oświadczeniach</span><span class="sxs-lookup"><span data-stu-id="8218d-104">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="8218d-105">Po utworzeniu tożsamości może być przypisana co najmniej jednego oświadczenia wystawione przez zaufany.</span><span class="sxs-lookup"><span data-stu-id="8218d-105">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="8218d-106">Oświadczenie para reprezentujący jakie podmiot jest wartość nazwy, nie jakie tematu może.</span><span class="sxs-lookup"><span data-stu-id="8218d-106">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="8218d-107">Na przykład masz prawa jazdy, wystawiony przez urząd lokalny licencji jazdy.</span><span class="sxs-lookup"><span data-stu-id="8218d-107">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="8218d-108">W sterowniku licencji znajdują się datę urodzenia.</span><span class="sxs-lookup"><span data-stu-id="8218d-108">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="8218d-109">W takim przypadku nazwa oświadczenia jest `DateOfBirth`, wartość oświadczenia będą datę urodzenia, na przykład `8th June 1970` i Wystawca byłaby pobudzenie urzędu licencji.</span><span class="sxs-lookup"><span data-stu-id="8218d-109">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="8218d-110">Autoryzacji oświadczeń, w najprostszym sprawdza wartość oświadczenia i zezwala na dostęp do zasobu na podstawie tej wartości.</span><span class="sxs-lookup"><span data-stu-id="8218d-110">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="8218d-111">Na przykład, jeśli chcesz uzyskać dostęp do klub nocy proces autoryzacji. może to być:</span><span class="sxs-lookup"><span data-stu-id="8218d-111">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="8218d-112">Drzwi urzędnika oceni wartość daty urodzenia oświadczeń i określa, czy ufają wystawcy (pobudzenie urzędu licencji) zanim zostanie przyznany dostęp.</span><span class="sxs-lookup"><span data-stu-id="8218d-112">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="8218d-113">Tożsamość może zawierać wielu oświadczeń z wieloma wartościami i może zawierać wielu oświadczenia tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="8218d-113">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="8218d-114">Dodawanie oświadczenia kontroli</span><span class="sxs-lookup"><span data-stu-id="8218d-114">Adding claims checks</span></span>

<span data-ttu-id="8218d-115">Oświadczenia deklaratywne sprawdzeń autoryzacji na podstawie — dewelopera umieszcza je ich kodem, kontrolera lub akcji w obrębie kontrolera, określając oświadczenia, które bieżący użytkownik musi posiadać i opcjonalnie wartość oświadczenia musi posiadać dostępu żądany zasób.</span><span class="sxs-lookup"><span data-stu-id="8218d-115">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="8218d-116">Oświadczeń wymagania są oparte na zasadach, deweloper musi kompilacji i zarejestruj zasad, przedstawiając wymagania oświadczeń.</span><span class="sxs-lookup"><span data-stu-id="8218d-116">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="8218d-117">Najprostszy typ oświadczenia zasad szuka obecności oświadczenia, nie sprawdzając wartość.</span><span class="sxs-lookup"><span data-stu-id="8218d-117">The simplest type of claim policy looks for the presence of a claim and does not check the value.</span></span>

<span data-ttu-id="8218d-118">Należy najpierw skompilować i zarejestrować zasad.</span><span class="sxs-lookup"><span data-stu-id="8218d-118">First you need to build and register the policy.</span></span> <span data-ttu-id="8218d-119">Ma to miejsce w ramach konfiguracji usługi autoryzacji, które zwykle bierze udział w `ConfigureServices()` w Twojej *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="8218d-119">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="8218d-120">W takim przypadku `EmployeeOnly` zasad sprawdza obecność `EmployeeNumber` oświadczeń w bieżącej tożsamości.</span><span class="sxs-lookup"><span data-stu-id="8218d-120">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="8218d-121">Następnie Zastosuj zasad przy użyciu opcji `Policy` właściwość `AuthorizeAttribute` atrybutu, aby określić nazwę zasady;</span><span class="sxs-lookup"><span data-stu-id="8218d-121">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="8218d-122">`AuthorizeAttribute` Atrybut można stosować do całego kontrolera, w tym wystąpieniu tylko tożsamości pasujących zasady będą miały dostęp do dowolnej akcji na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="8218d-122">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="8218d-123">Jeśli masz kontroler, która jest chroniona przez `AuthorizeAttribute` atrybutu, ale chcesz zezwolić na dostęp anonimowy do określonej akcji, należy zastosować `AllowAnonymousAttribute` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8218d-123">If you have a controller that is protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="8218d-124">Większość oświadczenia mają wartość.</span><span class="sxs-lookup"><span data-stu-id="8218d-124">Most claims come with a value.</span></span> <span data-ttu-id="8218d-125">Można określić listę dozwolonych wartości podczas tworzenia zasad.</span><span class="sxs-lookup"><span data-stu-id="8218d-125">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="8218d-126">Poniższy przykład tylko powiedzie się dla pracowników, których liczba pracowników była 1, 2, 3, 4 lub 5.</span><span class="sxs-lookup"><span data-stu-id="8218d-126">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="8218d-127">Wiele ocena zasad</span><span class="sxs-lookup"><span data-stu-id="8218d-127">Multiple Policy Evaluation</span></span>

<span data-ttu-id="8218d-128">Jeśli wiele zasad można zastosować do kontrolera lub akcji, wszystkie zasady muszą upłynąć, zanim zostanie przyznany dostęp.</span><span class="sxs-lookup"><span data-stu-id="8218d-128">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="8218d-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8218d-129">For example:</span></span>

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

<span data-ttu-id="8218d-130">W powyższym przykładzie wszystkie tożsamości, która spełnia `EmployeeOnly` zasady mogą uzyskiwać dostęp do `Payslip` akcji, jak te zasady są wymuszane na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="8218d-130">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="8218d-131">Jednak aby można było wywołać `UpdateSalary` akcji, które należy spełnić tożsamość *zarówno* `EmployeeOnly` zasad i `HumanResources` zasad.</span><span class="sxs-lookup"><span data-stu-id="8218d-131">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="8218d-132">Jeśli chcesz bardziej złożone zasady, takie jak pobieranie daty urodzenia oświadczenia, obliczanie wieku z niego, a następnie sprawdzanie wiek to 21 lub starsze, a następnie należy napisać [zasady niestandardowe programy obsługi](policies.md).</span><span class="sxs-lookup"><span data-stu-id="8218d-132">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
