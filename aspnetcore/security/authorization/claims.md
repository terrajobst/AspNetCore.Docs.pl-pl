---
title: Autoryzacja oparta na oświadczeniach w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać sprawdzanie oświadczenia dotyczące autoryzacji w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086151"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="9da80-103">Autoryzacja oparta na oświadczeniach w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9da80-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="9da80-104">Po utworzeniu tożsamości może być przypisany co najmniej jeden oświadczeń wystawione przez zaufany.</span><span class="sxs-lookup"><span data-stu-id="9da80-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="9da80-105">Oświadczenie to pary nazwa-wartość reprezentującą jakie podmiot, nie jakie podmiotu można zrobić.</span><span class="sxs-lookup"><span data-stu-id="9da80-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="9da80-106">Na przykład masz prawa jazdy, wystawiony przez urząd lokalny opracowuje licencji.</span><span class="sxs-lookup"><span data-stu-id="9da80-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="9da80-107">Twoje prawa jazdy znajdują się informacje o dacie urodzenia.</span><span class="sxs-lookup"><span data-stu-id="9da80-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="9da80-108">W tym przypadku wyniesie Nazwa oświadczenia `DateOfBirth`, wartość oświadczenia będą informacje o dacie urodzenia, na przykład `8th June 1970` i wystawca, którego będzie kierowania organu licencji.</span><span class="sxs-lookup"><span data-stu-id="9da80-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="9da80-109">Autoryzacji oświadczeń, w najprostszym sprawdza wartość oświadczenia i umożliwia dostęp do zasobu na podstawie tej wartości.</span><span class="sxs-lookup"><span data-stu-id="9da80-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="9da80-110">Na przykład jeśli chcesz, aby uzyskać dostęp do club nocy proces autoryzacji mogą być:</span><span class="sxs-lookup"><span data-stu-id="9da80-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="9da80-111">Ze specjalistą ds. zabezpieczeń drzwi biblioteki używane do oceny wartość Twojej daty urodzenia oświadczeń i czy ufają one wystawcy (opracowuje urzędu licencji) przed udzieleniem im dostępu.</span><span class="sxs-lookup"><span data-stu-id="9da80-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="9da80-112">Tożsamość może zawierać wielu oświadczeń z wieloma wartościami i może zawierać wielu oświadczeń tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="9da80-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="9da80-113">Dodawanie oświadczenia kontroli</span><span class="sxs-lookup"><span data-stu-id="9da80-113">Adding claims checks</span></span>

<span data-ttu-id="9da80-114">Oświadczenia deklaratywne sprawdzanie autoryzacji na podstawie — Deweloper umieszcza je w ramach ich kodować, kontrolera lub akcji w kontrolerze, określając oświadczenia, które musi posiadać bieżącego użytkownika i opcjonalnie wartość oświadczenia musi posiadać dostęp do żądany zasób.</span><span class="sxs-lookup"><span data-stu-id="9da80-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="9da80-115">Oświadczenia, że wymagania są oparte na zasadach, deweloper musi kompilacji i zarejestrowania zasadę wyrażania wymagań oświadczeń.</span><span class="sxs-lookup"><span data-stu-id="9da80-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="9da80-116">Najprostszy typ oświadczenia zasad szuka obecności oświadczenia i nie sprawdza wartość.</span><span class="sxs-lookup"><span data-stu-id="9da80-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="9da80-117">Najpierw należy do kompilacji i zarejestrowania zasad.</span><span class="sxs-lookup"><span data-stu-id="9da80-117">First you need to build and register the policy.</span></span> <span data-ttu-id="9da80-118">Ma to miejsce w ramach konfiguracji usługi autoryzacji, które normalnie bierze udział w `ConfigureServices()` w swojej *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="9da80-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="9da80-119">W tym przypadku `EmployeeOnly` zasad sprawdza obecność `EmployeeNumber` oświadczenia na bieżącej tożsamości.</span><span class="sxs-lookup"><span data-stu-id="9da80-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="9da80-120">Następnie zastosować zasady za pomocą `Policy` właściwość `AuthorizeAttribute` atrybutu, aby określić nazwę zasad;</span><span class="sxs-lookup"><span data-stu-id="9da80-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="9da80-121">`AuthorizeAttribute` Atrybut można stosować do całego kontrolera, w tym wystąpieniu tylko do tożsamości dopasowania zasad będą miały dostęp do dowolnej akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="9da80-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="9da80-122">Jeśli masz kontrolera, który jest chroniony przez `AuthorizeAttribute` atrybutu, ale chcesz zezwolić na dostęp anonimowy do określonej akcji, które można zastosować `AllowAnonymousAttribute` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9da80-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="9da80-123">Większość oświadczenia pochodzą z wartością.</span><span class="sxs-lookup"><span data-stu-id="9da80-123">Most claims come with a value.</span></span> <span data-ttu-id="9da80-124">Podczas tworzenia zasad, można określić listę dozwolonych wartości.</span><span class="sxs-lookup"><span data-stu-id="9da80-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="9da80-125">Poniższy przykład tylko powiedzie się dla pracowników, których liczba pracowników wyniosła 1, 2, 3, 4 lub 5.</span><span class="sxs-lookup"><span data-stu-id="9da80-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="9da80-126">Dodaj sprawdzanie oświadczenia ogólny</span><span class="sxs-lookup"><span data-stu-id="9da80-126">Add a generic claim check</span></span>

<span data-ttu-id="9da80-127">Jeśli wartość oświadczenia nie jest pojedynczą wartość lub transformacji jest wymagany, użyj [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span><span class="sxs-lookup"><span data-stu-id="9da80-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="9da80-128">Aby uzyskać więcej informacji, zobacz [spełniają zasady za pomocą func](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span><span class="sxs-lookup"><span data-stu-id="9da80-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="9da80-129">Wielokrotne obliczenie zasad</span><span class="sxs-lookup"><span data-stu-id="9da80-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="9da80-130">Jeśli wiele zasad są stosowane do kontrolera lub akcji, wszystkie zasady muszą minąć, zanim można udzielić dostępu.</span><span class="sxs-lookup"><span data-stu-id="9da80-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="9da80-131">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9da80-131">For example:</span></span>

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

<span data-ttu-id="9da80-132">W powyższym przykładzie dowolnej tożsamości, która spełnia `EmployeeOnly` zasady mogą uzyskiwać dostęp do `Payslip` akcji, jak te zasady są wymuszane na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="9da80-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="9da80-133">Jednak aby można było wywołać `UpdateSalary` akcji, które należy spełnić tożsamości *zarówno* `EmployeeOnly` zasad i `HumanResources` zasad.</span><span class="sxs-lookup"><span data-stu-id="9da80-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="9da80-134">Jeśli chcesz bardziej złożone zasady, takie jak biorąc datę urodzenia oświadczenia, obliczenia wieku z niego, a następnie sprawdzając wiek to 21 lub starszy, a następnie należy napisać [zasady niestandardowe programy obsługi](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="9da80-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
