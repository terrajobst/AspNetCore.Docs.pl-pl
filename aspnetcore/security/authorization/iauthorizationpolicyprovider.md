---
title: Autoryzacja Niestandardowa zasada dostawców w platformy ASP.NET Core
author: mjrousos
description: Dowiedz się, jak używać niestandardowych IAuthorizationPolicyProvider w aplikacji platformy ASP.NET Core można dynamicznie wygenerować zasad autoryzacji.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 524928a5b291e02556d11a762d86430a6dc94660
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277260"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="e9e66-103">Niestandardowych dostawców zasad autoryzacji przy użyciu IAuthorizationPolicyProvider w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9e66-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="e9e66-104">Przez [Rousos Jan](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="e9e66-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="e9e66-105">Zazwyczaj przy użyciu [autoryzacji na podstawie zasad](xref:security/authorization/policies), zasady są rejestrowane przez wywołanie metody `AuthorizationOptions.AddPolicy` w ramach konfiguracji usługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e9e66-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="e9e66-106">W niektórych scenariuszach może nie być możliwe (lub pożądane) do rejestrowania wszystkich zasad autoryzacji w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="e9e66-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="e9e66-107">W takich przypadkach można użyć niestandardowego `IAuthorizationPolicyProvider` do kontrolowania, jak podano zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e9e66-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="e9e66-108">Przykładowe scenariusze, w przypadku, gdy niestandardowego [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) mogą być przydatne, obejmują:</span><span class="sxs-lookup"><span data-stu-id="e9e66-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="e9e66-109">Aby zapewnić oceny zasad za pomocą zewnętrznej usługi.</span><span class="sxs-lookup"><span data-stu-id="e9e66-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="e9e66-110">Przy użyciu duży zakres zasad (w przypadku liczb różne pomieszczenia lub wieku, na przykład), więc go nie ma sensu dodać każdej zasady autoryzacji poszczególnych z `AuthorizationOptions.AddPolicy` wywołania.</span><span class="sxs-lookup"><span data-stu-id="e9e66-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="e9e66-111">Tworzenie zasad w czasie wykonywania na podstawie informacji zewnętrznego źródła danych (takich jak bazy danych) lub określanie wymagań autoryzacji dynamicznie za pośrednictwem innego mechanizmu.</span><span class="sxs-lookup"><span data-stu-id="e9e66-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="e9e66-112">Dostosowywanie pobieranie zasad</span><span class="sxs-lookup"><span data-stu-id="e9e66-112">Customizing policy retrieval</span></span>

<span data-ttu-id="e9e66-113">Aplikacje platformy ASP.NET Core korzystać z implementacji `IAuthorizationPolicyProvider` interfejs do pobrania zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e9e66-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="e9e66-114">Domyślnie [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) jest zarejestrowany i używane.</span><span class="sxs-lookup"><span data-stu-id="e9e66-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="e9e66-115">`DefaultAuthorizationPolicyProvider` Zwraca zasad z `AuthorizationOptions` w `IServiceCollection.AddAuthorization` wywołania.</span><span class="sxs-lookup"><span data-stu-id="e9e66-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="e9e66-116">To zachowanie można dostosować, rejestrując innej `IAuthorizationPolicyProvider` wdrożenia w aplikacji [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="e9e66-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="e9e66-117">`IAuthorizationPolicyProvider` Interfejs zawiera dwa interfejsy API:</span><span class="sxs-lookup"><span data-stu-id="e9e66-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="e9e66-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) zwraca zasady autoryzacji dla podanej nazwie.</span><span class="sxs-lookup"><span data-stu-id="e9e66-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="e9e66-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) zwraca domyślne zasady autoryzacji (zasady dla `[Authorize]` atrybuty bez określone zasady).</span><span class="sxs-lookup"><span data-stu-id="e9e66-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="e9e66-120">Zaimplementowanie tych dwóch interfejsów API, można dostosować, jak podano zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e9e66-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="e9e66-121">Sparametryzowane autoryzować przykład atrybutu</span><span class="sxs-lookup"><span data-stu-id="e9e66-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="e9e66-122">Jednym ze scenariuszy gdzie `IAuthorizationPolicyProvider` przydaje się jest włączenie niestandardowe `[Authorize]` atrybutów, których wymagania zależą od parametru.</span><span class="sxs-lookup"><span data-stu-id="e9e66-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="e9e66-123">Na przykład w [autoryzacji na podstawie zasad](xref:security/authorization/policies) dokumentacji, na podstawie wieku ("AtLeast21") zasady została użyta jako przykład.</span><span class="sxs-lookup"><span data-stu-id="e9e66-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="e9e66-124">Jeśli inny kontroler akcji w aplikacji powinna zostać udostępniona użytkownikom *różnych* rzadziej, może być warto mieć wiele różnych zasad na podstawie wieku.</span><span class="sxs-lookup"><span data-stu-id="e9e66-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="e9e66-125">Zamiast zarejestrować wszystkie różne na podstawie wieku zasady wymagające aplikacji w `AuthorizationOptions`, można wygenerować zasady dynamicznie z niestandardowego `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="e9e66-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="e9e66-126">Aby przy użyciu zasady jest łatwiejsze, może dodawać adnotacje do działania z atrybutem przeprowadzania niestandardowej autoryzacji, takich jak `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="e9e66-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="e9e66-127">Atrybuty niestandardowe autoryzacji</span><span class="sxs-lookup"><span data-stu-id="e9e66-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="e9e66-128">Zasady autoryzacji są identyfikowane przez ich nazwy.</span><span class="sxs-lookup"><span data-stu-id="e9e66-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="e9e66-129">Niestandardowa `MinimumAgeAuthorizeAttribute` opisanego wcześniej należy mapować argumenty na ciąg, który może służyć do pobierania odpowiednie zasady autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e9e66-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="e9e66-130">Aby to zrobić, wynikające z `AuthorizeAttribute` i `Age` zawijania właściwości `AuthorizeAttribute.Policy` właściwości.</span><span class="sxs-lookup"><span data-stu-id="e9e66-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="e9e66-131">Ten typ atrybutu ma `Policy` ciąg opartą na prefiksie ustalony (`"MinimumAge"`) i całkowitą przekazane za pośrednictwem konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e9e66-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="e9e66-132">Można go zastosować do akcji w taki sam sposób jak inne `Authorize` atrybutów z tą różnicą, że trwa całkowitą jako parametr.</span><span class="sxs-lookup"><span data-stu-id="e9e66-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="e9e66-133">Niestandardowe IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="e9e66-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="e9e66-134">Niestandardowa `MinimumAgeAuthorizeAttribute` ułatwia zasad autoryzacji żądania dla dowolnego minimalnego wieku potrzebne.</span><span class="sxs-lookup"><span data-stu-id="e9e66-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="e9e66-135">Następny problem do rozwiązania jest upewnienie się, zasady autoryzacji są dostępne dla wszystkich tych różnym wieku.</span><span class="sxs-lookup"><span data-stu-id="e9e66-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="e9e66-136">Jest to, gdy `IAuthorizationPolicyProvider` jest użyteczne.</span><span class="sxs-lookup"><span data-stu-id="e9e66-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="e9e66-137">Korzystając z `MinimumAgeAuthorizationAttribute`, nazwy zasad autoryzacji będzie zgodne ze wzorcem `"MinimumAge" + Age`, więc niestandardowego `IAuthorizationPolicyProvider` powinna generować zasad autoryzacji przez:</span><span class="sxs-lookup"><span data-stu-id="e9e66-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="e9e66-138">Podczas analizowania wieku z nazwę zasady.</span><span class="sxs-lookup"><span data-stu-id="e9e66-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="e9e66-139">Przy użyciu `AuthorizationPolicyBuiler` do tworzenia nowego `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="e9e66-139">Using `AuthorizationPolicyBuiler` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="e9e66-140">Dodawanie wymagań do zasady oparte na wiek za pomocą `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="e9e66-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="e9e66-141">W innych sytuacjach można użyć `RequireClaim`, `RequireRole`, lub `RequireUserName` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="e9e66-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="e9e66-142">Wielu dostawców zasad autoryzacji</span><span class="sxs-lookup"><span data-stu-id="e9e66-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="e9e66-143">Podczas korzystania z niestandardowego `IAuthorizationPolicyProvider` implementacji, należy pamiętać, które platformy ASP.NET Core używa tylko jedno wystąpienie `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="e9e66-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="e9e66-144">Jeśli niestandardowego dostawcy nie jest zapewnienie zasady autoryzacji dla wszystkich nazw zasad, powinien on może wrócić do dostawcę kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="e9e66-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="e9e66-145">Nazwy zasad może obejmować te, które pochodzą z domyślnych zasad dla `[Authorize]` atrybuty bez nazwy.</span><span class="sxs-lookup"><span data-stu-id="e9e66-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="e9e66-146">Na przykład należy rozważyć aplikacji wymagane zarówno zasady dotyczące wieku niestandardowych, jak i bardziej tradycyjnej pobieranie zasad opartych na rolach.</span><span class="sxs-lookup"><span data-stu-id="e9e66-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="e9e66-147">Takich aplikacji można użyć dostawcy zasad autoryzacji niestandardowej który:</span><span class="sxs-lookup"><span data-stu-id="e9e66-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="e9e66-148">Próbuje przeanalizować nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="e9e66-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="e9e66-149">Wywołuje dostawcę różnych zasad (takie jak `DefaultAuthorizationPolicyProvider`), jeśli nazwa zasad nie zawiera wieku.</span><span class="sxs-lookup"><span data-stu-id="e9e66-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="e9e66-150">Domyślne zasady</span><span class="sxs-lookup"><span data-stu-id="e9e66-150">Default policy</span></span>

<span data-ttu-id="e9e66-151">Oprócz zasad autoryzacji nazwanego, niestandardowego `IAuthorizationPolicyProvider` należy zaimplementować `GetDefaultPolicyAsync` zapewnienie zasady autoryzacji dla `[Authorize]` atrybuty bez określonej nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="e9e66-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="e9e66-152">W wielu przypadkach ten atrybut autoryzacji tylko wymaga uwierzytelnionego użytkownika, dzięki czemu można zmieniać niezbędne zasad wywołaniem `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="e9e66-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="e9e66-153">W przypadku wszystkich aspektów niestandardowego `IAuthorizationPolicyProvider`, to ustawienie można dostosować, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="e9e66-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="e9e66-154">W niektórych przypadkach:</span><span class="sxs-lookup"><span data-stu-id="e9e66-154">In some cases:</span></span>

* <span data-ttu-id="e9e66-155">Domyślne zasady autoryzacji nie mogą być używane.</span><span class="sxs-lookup"><span data-stu-id="e9e66-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="e9e66-156">Pobiera zasady domyślne można delegować do rezerwowe `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="e9e66-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="e9e66-157">Przy użyciu niestandardowych IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="e9e66-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="e9e66-158">Stosowanie niestandardowych zasad z `IAuthorizationPolicyProvider`, musisz:</span><span class="sxs-lookup"><span data-stu-id="e9e66-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="e9e66-159">Zarejestruj odpowiedni `AuthorizationHandler` typy z iniekcji zależności (opisany w [autoryzacji na podstawie zasad](xref:security/authorization/policies#authorization-handlers)), podobnie jak w przypadku wszystkich scenariuszy opartych na zasadach autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e9e66-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="e9e66-160">Rejestrowanie niestandardowe `IAuthorizationPolicyProvider` typu w kolekcji usługi iniekcji zależności aplikacji (w `Startup.ConfigureServices`) aby zastąpić domyślny dostawca zasad.</span><span class="sxs-lookup"><span data-stu-id="e9e66-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="e9e66-161">Niestandardowe pełną `IAuthorizationPolicyProvider` przykład jest dostępny w [repozytorium GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="e9e66-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
