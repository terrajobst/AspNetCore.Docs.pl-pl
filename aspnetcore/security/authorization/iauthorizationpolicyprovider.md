---
title: Autoryzacja Niestandardowa zasada dostawców w programie ASP.NET Core
author: mjrousos
description: Dowiedz się, jak używać niestandardowego IAuthorizationPolicyProvider w aplikacji ASP.NET Core można dynamicznie wygenerować zasady autoryzacji.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: fdd8f9232c4332aa8307b9dbdfba6af48dfafa72
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045500"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="44cea-103">Niestandardowi dostawcy zasad autoryzacji przy użyciu IAuthorizationPolicyProvider w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44cea-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="44cea-104">Przez [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="44cea-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="44cea-105">Zazwyczaj przy użyciu [autoryzacji opartej na zasadach](xref:security/authorization/policies), zasady są rejestrowane przez wywołanie metody `AuthorizationOptions.AddPolicy` jako część konfiguracji usługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="44cea-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="44cea-106">W niektórych scenariuszach może nie być możliwe (lub pożądane) do rejestrowania wszystkich zasad autoryzacji w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="44cea-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="44cea-107">W takich przypadkach można używać niestandardowego `IAuthorizationPolicyProvider` do kontrolowania, jak podano zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="44cea-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="44cea-108">Przykładowe scenariusze, w przypadku, gdy niestandardowego [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) może być przydatne, obejmują:</span><span class="sxs-lookup"><span data-stu-id="44cea-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="44cea-109">Podaj oceny zasad przy użyciu usługi zewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="44cea-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="44cea-110">Przy użyciu dużych wielu zasad (w przypadku liczb różne pomieszczenia lub wieku, na przykład), więc nie ma sensu do dodania każdej zasady autoryzacji poszczególnych z `AuthorizationOptions.AddPolicy` wywołania.</span><span class="sxs-lookup"><span data-stu-id="44cea-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="44cea-111">Tworzenie zasad w oparciu o informacje z zewnętrznego źródła danych (np. bazy danych) w czasie wykonywania, lub określanie wymagań autoryzacji dynamicznie za pośrednictwem innego mechanizmu.</span><span class="sxs-lookup"><span data-stu-id="44cea-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="44cea-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider) z [repozytorium GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples).</span><span class="sxs-lookup"><span data-stu-id="44cea-112">[View or download sample code](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider) from the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples).</span></span> <span data-ttu-id="44cea-113">Pobierz plik ZIP repozytorium aspnet/AuthSamples.</span><span class="sxs-lookup"><span data-stu-id="44cea-113">Download the aspnet/AuthSamples repository ZIP file.</span></span>
<span data-ttu-id="44cea-114">Rozpakuj *AuthSamples-master.zip* pliku.</span><span class="sxs-lookup"><span data-stu-id="44cea-114">Unzip the *AuthSamples-master.zip* file.</span></span> <span data-ttu-id="44cea-115">Przejdź do *samples/CustomPolicyProvider* folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="44cea-115">Navigate to the *samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="44cea-116">Dostosowywanie pobierania zasad</span><span class="sxs-lookup"><span data-stu-id="44cea-116">Customize policy retrieval</span></span>

<span data-ttu-id="44cea-117">Aplikacje platformy ASP.NET Core korzystać z implementacji `IAuthorizationPolicyProvider` interfejsu można pobrać zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="44cea-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="44cea-118">Domyślnie [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) jest zarejestrowany i używane.</span><span class="sxs-lookup"><span data-stu-id="44cea-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="44cea-119">`DefaultAuthorizationPolicyProvider` Zwraca zasad z `AuthorizationOptions` podawany `IServiceCollection.AddAuthorization` wywołania.</span><span class="sxs-lookup"><span data-stu-id="44cea-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="44cea-120">To zachowanie można dostosować przez zarejestrowanie innego `IAuthorizationPolicyProvider` implementacji aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="44cea-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="44cea-121">`IAuthorizationPolicyProvider` Interfejs zawiera dwa interfejsy API:</span><span class="sxs-lookup"><span data-stu-id="44cea-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="44cea-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) zwraca zasady autoryzacji dla danej nazwy.</span><span class="sxs-lookup"><span data-stu-id="44cea-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="44cea-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) zwraca domyślne zasady autoryzacji (zasady stosowane do `[Authorize]` atrybutów bez określone zasady).</span><span class="sxs-lookup"><span data-stu-id="44cea-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="44cea-124">Wdrażając te dwa interfejsy API, można dostosować, jak zasady autoryzacji są dostarczane.</span><span class="sxs-lookup"><span data-stu-id="44cea-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="44cea-125">Sparametryzowane autoryzować przykład atrybutu</span><span class="sxs-lookup"><span data-stu-id="44cea-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="44cea-126">Jeden scenariusz gdzie `IAuthorizationPolicyProvider` jest przydatne, jest zapewnienie niestandardowe `[Authorize]` atrybutów, którego wymagania zależą od parametru.</span><span class="sxs-lookup"><span data-stu-id="44cea-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="44cea-127">Na przykład w [autoryzacji opartej na zasadach](xref:security/authorization/policies) dokumentacji, na podstawie wieku ("AtLeast21") zasad została użyta jako przykładu.</span><span class="sxs-lookup"><span data-stu-id="44cea-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="44cea-128">Jeśli kontroler różnych akcji w aplikacji należy udostępniane użytkownikom *różnych* wieku, warto mieć wiele różnych zasad na podstawie wieku.</span><span class="sxs-lookup"><span data-stu-id="44cea-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="44cea-129">Zamiast rejestracji wszystkie różne na podstawie wieku zasady wymagające aplikacji w `AuthorizationOptions`, można wygenerować zasady dynamicznie przy użyciu niestandardowego `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="44cea-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="44cea-130">Aby upewnić się, przy użyciu zasad jest łatwiejsze, może dodawać adnotacje akcji z atrybutem autoryzacja niestandardowa, takich jak `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="44cea-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="44cea-131">Autoryzacja niestandardowa atrybutów</span><span class="sxs-lookup"><span data-stu-id="44cea-131">Custom Authorization Attributes</span></span>

<span data-ttu-id="44cea-132">Zasady autoryzacji są identyfikowane przez ich nazwy.</span><span class="sxs-lookup"><span data-stu-id="44cea-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="44cea-133">Niestandardowy `MinimumAgeAuthorizeAttribute` opisanego wcześniej potrzebuje do mapowania argumentów na ciąg, który może służyć do pobierania odpowiednie zasady autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="44cea-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="44cea-134">Możesz to zrobić, wynikające z `AuthorizeAttribute` i dokonując `Age` zawijania właściwość `AuthorizeAttribute.Policy` właściwości.</span><span class="sxs-lookup"><span data-stu-id="44cea-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```csharp
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

<span data-ttu-id="44cea-135">Ten typ atrybutu ma `Policy` ciągu na podstawie ustaloną prefiksu (`"MinimumAge"`) i całkowitą przekazanych za pośrednictwem konstruktora.</span><span class="sxs-lookup"><span data-stu-id="44cea-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="44cea-136">Można go zastosować do akcji w taki sam sposób jak inne `Authorize` atrybutów, z tą różnicą, że wykorzystuje całkowitą jako parametr.</span><span class="sxs-lookup"><span data-stu-id="44cea-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="44cea-137">Niestandardowe IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="44cea-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="44cea-138">Niestandardowy `MinimumAgeAuthorizeAttribute` ułatwia zasady autoryzacji żądania dla dowolnego minimalny wiek żądanego.</span><span class="sxs-lookup"><span data-stu-id="44cea-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="44cea-139">Następny problem do rozwiązania jest upewnienie się, że zasady autoryzacji są dostępne dla wszystkich tych różnych wieku.</span><span class="sxs-lookup"><span data-stu-id="44cea-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="44cea-140">Jest to miejsce `IAuthorizationPolicyProvider` przydaje się.</span><span class="sxs-lookup"><span data-stu-id="44cea-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="44cea-141">Korzystając z `MinimumAgeAuthorizationAttribute`, nazwy zasad autoryzacji będą oparte na wzorcu `"MinimumAge" + Age`, więc niestandardowej `IAuthorizationPolicyProvider` powinien wygenerować zasady autoryzacji przez:</span><span class="sxs-lookup"><span data-stu-id="44cea-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="44cea-142">Analizowanie wieku na podstawie nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="44cea-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="44cea-143">Za pomocą `AuthorizationPolicyBuilder` do tworzenia nowego `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="44cea-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="44cea-144">Dodawanie wymagań do zasady oparte na wiek za pomocą `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="44cea-144">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="44cea-145">W innych sytuacjach, można użyć `RequireClaim`, `RequireRole`, lub `RequireUserName` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="44cea-145">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```csharp
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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="44cea-146">Wielu dostawców zasad autoryzacji</span><span class="sxs-lookup"><span data-stu-id="44cea-146">Multiple authorization policy providers</span></span>

<span data-ttu-id="44cea-147">Korzystając z niestandardowego `IAuthorizationPolicyProvider` implementacji, należy pamiętać, który platformy ASP.NET Core używa tylko jedno wystąpienie `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="44cea-147">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="44cea-148">Jeśli niestandardowego dostawcy nie jest zapewnienie zasady autoryzacji dla wszystkich nazw zasady, jej należy wrócić do dostawcę kopii zapasowych.</span><span class="sxs-lookup"><span data-stu-id="44cea-148">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="44cea-149">Nazwy zasad mogą obejmować te, które pochodzą z zasady domyślne dla `[Authorize]` atrybutów bez nazwy.</span><span class="sxs-lookup"><span data-stu-id="44cea-149">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="44cea-150">Na przykład należy rozważyć aplikacji wymagane zasady dotyczące wieku niestandardowych i bardziej tradycyjny pobierania zasad oparta na rolach.</span><span class="sxs-lookup"><span data-stu-id="44cea-150">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="44cea-151">Takiej aplikacji można użyć niestandardowego zasad dostawcę autoryzacji który:</span><span class="sxs-lookup"><span data-stu-id="44cea-151">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="44cea-152">Próbuje przeanalizować nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="44cea-152">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="44cea-153">Wywołania do dostawcy inne zasady (takich jak `DefaultAuthorizationPolicyProvider`) Jeśli nazwa zasad nie zawiera wieku.</span><span class="sxs-lookup"><span data-stu-id="44cea-153">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="44cea-154">Zasady domyślne</span><span class="sxs-lookup"><span data-stu-id="44cea-154">Default policy</span></span>

<span data-ttu-id="44cea-155">Oprócz zapewniania zasady autoryzacji dla nazwanych, niestandardowe `IAuthorizationPolicyProvider` należy zaimplementować `GetDefaultPolicyAsync` zapewnienie zasady autoryzacji dla `[Authorize]` atrybutów bez określonej nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="44cea-155">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="44cea-156">W wielu przypadkach, ten atrybut autoryzacji wymaga tylko uwierzytelnionego użytkownika, dzięki czemu można zmieniać wymaganych zasad z wywołaniem `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="44cea-156">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="44cea-157">Podobnie jak w przypadku wszystkich aspektów niestandardowego `IAuthorizationPolicyProvider`, to ustawienie można dostosować, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="44cea-157">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="44cea-158">W niektórych przypadkach:</span><span class="sxs-lookup"><span data-stu-id="44cea-158">In some cases:</span></span>

* <span data-ttu-id="44cea-159">Domyślne zasady autoryzacji nie mogą być używane.</span><span class="sxs-lookup"><span data-stu-id="44cea-159">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="44cea-160">Pobiera zasady domyślne mogą być delegowane do rezerwowe `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="44cea-160">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="44cea-161">Użyj niestandardowego IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="44cea-161">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="44cea-162">Korzystanie z niestandardowych zasad z `IAuthorizationPolicyProvider`, należy:</span><span class="sxs-lookup"><span data-stu-id="44cea-162">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="44cea-163">Zarejestruj odpowiednie `AuthorizationHandler` typy za pomocą iniekcji zależności (opisanego w [autoryzacji opartej na zasadach](xref:security/authorization/policies#authorization-handlers)), podobnie jak w przypadku wszystkich scenariuszy na podstawie zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="44cea-163">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="44cea-164">Rejestrowanie niestandardowego `IAuthorizationPolicyProvider` typu w kolekcji usługi iniekcji zależności aplikacji (w `Startup.ConfigureServices`) aby zastąpić domyślny dostawca zasad.</span><span class="sxs-lookup"><span data-stu-id="44cea-164">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="44cea-165">Niestandardowe pełną `IAuthorizationPolicyProvider` próbka jest dostępna w [repozytorium GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="44cea-165">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).</span></span>
