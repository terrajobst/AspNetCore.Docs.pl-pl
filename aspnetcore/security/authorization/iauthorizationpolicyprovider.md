---
title: Autoryzacja Niestandardowa zasada dostawców w programie ASP.NET Core
author: mjrousos
description: Dowiedz się, jak używać niestandardowego IAuthorizationPolicyProvider w aplikacji ASP.NET Core można dynamicznie wygenerować zasady autoryzacji.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: e17372bb0ec9091c385a70b1e907eaa3cff24003
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614412"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="53707-103">Niestandardowi dostawcy zasad autoryzacji przy użyciu IAuthorizationPolicyProvider w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53707-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="53707-104">Przez [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="53707-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="53707-105">Zazwyczaj przy użyciu [autoryzacji opartej na zasadach](xref:security/authorization/policies), zasady są rejestrowane przez wywołanie metody `AuthorizationOptions.AddPolicy` jako część konfiguracji usługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="53707-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="53707-106">W niektórych scenariuszach może nie być możliwe (lub pożądane) do rejestrowania wszystkich zasad autoryzacji w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="53707-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="53707-107">W takich przypadkach można używać niestandardowego `IAuthorizationPolicyProvider` do kontrolowania, jak podano zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="53707-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="53707-108">Przykładowe scenariusze, w przypadku, gdy niestandardowego [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) może być przydatne, obejmują:</span><span class="sxs-lookup"><span data-stu-id="53707-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="53707-109">Podaj oceny zasad przy użyciu usługi zewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="53707-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="53707-110">Przy użyciu dużych wielu zasad (w przypadku liczb różne pomieszczenia lub wieku, na przykład), więc nie ma sensu do dodania każdej zasady autoryzacji poszczególnych z `AuthorizationOptions.AddPolicy` wywołania.</span><span class="sxs-lookup"><span data-stu-id="53707-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="53707-111">Tworzenie zasad w oparciu o informacje z zewnętrznego źródła danych (np. bazy danych) w czasie wykonywania, lub określanie wymagań autoryzacji dynamicznie za pośrednictwem innego mechanizmu.</span><span class="sxs-lookup"><span data-stu-id="53707-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="53707-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) z [repozytorium AspNetCore GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="53707-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="53707-113">Pobierz plik ZIP repozytorium aspnet/AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="53707-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="53707-114">Rozpakuj plik.</span><span class="sxs-lookup"><span data-stu-id="53707-114">Unzip the file.</span></span> <span data-ttu-id="53707-115">Przejdź do *src/Security/samples/CustomPolicyProvider* folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="53707-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="53707-116">Dostosowywanie pobierania zasad</span><span class="sxs-lookup"><span data-stu-id="53707-116">Customize policy retrieval</span></span>

<span data-ttu-id="53707-117">Aplikacje platformy ASP.NET Core korzystać z implementacji `IAuthorizationPolicyProvider` interfejsu można pobrać zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="53707-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="53707-118">Domyślnie [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) jest zarejestrowany i używane.</span><span class="sxs-lookup"><span data-stu-id="53707-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="53707-119">`DefaultAuthorizationPolicyProvider` Zwraca zasad z `AuthorizationOptions` podawany `IServiceCollection.AddAuthorization` wywołania.</span><span class="sxs-lookup"><span data-stu-id="53707-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="53707-120">To zachowanie można dostosować przez zarejestrowanie innego `IAuthorizationPolicyProvider` implementacji aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="53707-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="53707-121">`IAuthorizationPolicyProvider` Interfejs zawiera dwa interfejsy API:</span><span class="sxs-lookup"><span data-stu-id="53707-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="53707-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) zwraca zasady autoryzacji dla danej nazwy.</span><span class="sxs-lookup"><span data-stu-id="53707-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="53707-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) zwraca domyślne zasady autoryzacji (zasady stosowane do `[Authorize]` atrybutów bez określone zasady).</span><span class="sxs-lookup"><span data-stu-id="53707-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="53707-124">Wdrażając te dwa interfejsy API, można dostosować, jak zasady autoryzacji są dostarczane.</span><span class="sxs-lookup"><span data-stu-id="53707-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="53707-125">Sparametryzowane autoryzować przykład atrybutu</span><span class="sxs-lookup"><span data-stu-id="53707-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="53707-126">Jeden scenariusz gdzie `IAuthorizationPolicyProvider` jest przydatne, jest zapewnienie niestandardowe `[Authorize]` atrybutów, którego wymagania zależą od parametru.</span><span class="sxs-lookup"><span data-stu-id="53707-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="53707-127">Na przykład w [autoryzacji opartej na zasadach](xref:security/authorization/policies) dokumentacji, na podstawie wieku ("AtLeast21") zasad została użyta jako przykładu.</span><span class="sxs-lookup"><span data-stu-id="53707-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="53707-128">Jeśli kontroler różnych akcji w aplikacji należy udostępniane użytkownikom *różnych* wieku, warto mieć wiele różnych zasad na podstawie wieku.</span><span class="sxs-lookup"><span data-stu-id="53707-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="53707-129">Zamiast rejestracji wszystkie różne na podstawie wieku zasady wymagające aplikacji w `AuthorizationOptions`, można wygenerować zasady dynamicznie przy użyciu niestandardowego `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="53707-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="53707-130">Aby upewnić się, przy użyciu zasad jest łatwiejsze, może dodawać adnotacje akcji z atrybutem autoryzacja niestandardowa, takich jak `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="53707-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="53707-131">Niestandardowe atrybuty autoryzacji</span><span class="sxs-lookup"><span data-stu-id="53707-131">Custom Authorization attributes</span></span>

<span data-ttu-id="53707-132">Zasady autoryzacji są identyfikowane przez ich nazwy.</span><span class="sxs-lookup"><span data-stu-id="53707-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="53707-133">Niestandardowy `MinimumAgeAuthorizeAttribute` opisanego wcześniej potrzebuje do mapowania argumentów na ciąg, który może służyć do pobierania odpowiednie zasady autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="53707-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="53707-134">Możesz to zrobić, wynikające z `AuthorizeAttribute` i dokonując `Age` zawijania właściwość `AuthorizeAttribute.Policy` właściwości.</span><span class="sxs-lookup"><span data-stu-id="53707-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="53707-135">Ten typ atrybutu ma `Policy` ciągu na podstawie ustaloną prefiksu (`"MinimumAge"`) i całkowitą przekazanych za pośrednictwem konstruktora.</span><span class="sxs-lookup"><span data-stu-id="53707-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="53707-136">Można go zastosować do akcji w taki sam sposób jak inne `Authorize` atrybutów, z tą różnicą, że wykorzystuje całkowitą jako parametr.</span><span class="sxs-lookup"><span data-stu-id="53707-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="53707-137">Custom IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="53707-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="53707-138">Niestandardowy `MinimumAgeAuthorizeAttribute` ułatwia zasady autoryzacji żądania dla dowolnego minimalny wiek żądanego.</span><span class="sxs-lookup"><span data-stu-id="53707-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="53707-139">Następny problem do rozwiązania jest upewnienie się, że zasady autoryzacji są dostępne dla wszystkich tych różnych wieku.</span><span class="sxs-lookup"><span data-stu-id="53707-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="53707-140">Jest to miejsce `IAuthorizationPolicyProvider` przydaje się.</span><span class="sxs-lookup"><span data-stu-id="53707-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="53707-141">Korzystając z `MinimumAgeAuthorizationAttribute`, nazwy zasad autoryzacji będą oparte na wzorcu `"MinimumAge" + Age`, więc niestandardowej `IAuthorizationPolicyProvider` powinien wygenerować zasady autoryzacji przez:</span><span class="sxs-lookup"><span data-stu-id="53707-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="53707-142">Analizowanie wieku na podstawie nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="53707-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="53707-143">Za pomocą `AuthorizationPolicyBuilder` do tworzenia nowego `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="53707-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="53707-144">Dodawanie wymagań do zasady oparte na wiek za pomocą `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="53707-144">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="53707-145">W innych sytuacjach, można użyć `RequireClaim`, `RequireRole`, lub `RequireUserName` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="53707-145">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="53707-146">Wielu dostawców zasad autoryzacji</span><span class="sxs-lookup"><span data-stu-id="53707-146">Multiple authorization policy providers</span></span>

<span data-ttu-id="53707-147">Korzystając z niestandardowego `IAuthorizationPolicyProvider` implementacji, należy pamiętać, który platformy ASP.NET Core używa tylko jedno wystąpienie `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="53707-147">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="53707-148">Jeśli niestandardowego dostawcy nie jest w stanie zapewnić zasady autoryzacji dla wszystkich nazw zasad, które będą używane, jego powinien wrócić do tworzenia kopii zapasowych dostawcy.</span><span class="sxs-lookup"><span data-stu-id="53707-148">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="53707-149">Rozważmy na przykład aplikację, która wymaga bardziej tradycyjny pobierania zasad oparta na rolach i zasady dotyczące wieku niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="53707-149">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="53707-150">Takiej aplikacji można użyć niestandardowego zasad dostawcę autoryzacji który:</span><span class="sxs-lookup"><span data-stu-id="53707-150">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="53707-151">Próbuje przeanalizować nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="53707-151">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="53707-152">Wywołania do dostawcy inne zasady (takich jak `DefaultAuthorizationPolicyProvider`) Jeśli nazwa zasad nie zawiera wieku.</span><span class="sxs-lookup"><span data-stu-id="53707-152">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="53707-153">Przykład `IAuthorizationPolicyProvider` implementacji powyżej można zaktualizować w celu zastosowania `DefaultAuthorizationPolicyProvider` , tworząc dostawcy zasad rezerwowej w jego konstruktorze (do użycia w przypadku, gdy nazwa zasad nie odpowiada jego oczekiwanym wzorcem "MinimumAge" + wiek).</span><span class="sxs-lookup"><span data-stu-id="53707-153">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="53707-154">Następnie `GetPolicyAsync` metoda można zaktualizować w celu zastosowania `FallbackPolicyProvider` zamiast zwracać wartość null:</span><span class="sxs-lookup"><span data-stu-id="53707-154">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="53707-155">Zasady domyślne</span><span class="sxs-lookup"><span data-stu-id="53707-155">Default policy</span></span>

<span data-ttu-id="53707-156">Oprócz zapewniania zasady autoryzacji dla nazwanych, niestandardowe `IAuthorizationPolicyProvider` należy zaimplementować `GetDefaultPolicyAsync` zapewnienie zasady autoryzacji dla `[Authorize]` atrybutów bez określonej nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="53707-156">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="53707-157">W wielu przypadkach, ten atrybut autoryzacji wymaga tylko uwierzytelnionego użytkownika, dzięki czemu można zmieniać wymaganych zasad z wywołaniem `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="53707-157">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="53707-158">Podobnie jak w przypadku wszystkich aspektów niestandardowego `IAuthorizationPolicyProvider`, to ustawienie można dostosować, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="53707-158">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="53707-159">W niektórych przypadkach może być pożądane, można pobrać domyślnej zasady z rezerwowe `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="53707-159">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="53707-160">Wymagane zasady</span><span class="sxs-lookup"><span data-stu-id="53707-160">Required policy</span></span>

<span data-ttu-id="53707-161">Niestandardowy `IAuthorizationPolicyProvider` należy zaimplementować `GetRequiredPolicyAsync` zapewnienie, opcjonalnie, zasady, które jest zawsze wymagana.</span><span class="sxs-lookup"><span data-stu-id="53707-161">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="53707-162">Jeśli `GetRequiredPolicyAsync` przywraca zasadom inną niż null, te zasady zostaną połączone z innymi (o nazwie lub domyślny) zasad, które są wymagane.</span><span class="sxs-lookup"><span data-stu-id="53707-162">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="53707-163">W razie potrzeby nie wymagane zasady dostawca może po prostu zwracać wartość null lub odroczone do rezerwowego dostawcy:</span><span class="sxs-lookup"><span data-stu-id="53707-163">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="53707-164">Użyj niestandardowego IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="53707-164">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="53707-165">Korzystanie z niestandardowych zasad z `IAuthorizationPolicyProvider`, należy:</span><span class="sxs-lookup"><span data-stu-id="53707-165">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="53707-166">Zarejestruj odpowiednie `AuthorizationHandler` typy za pomocą iniekcji zależności (opisanego w [autoryzacji opartej na zasadach](xref:security/authorization/policies#authorization-handlers)), podobnie jak w przypadku wszystkich scenariuszy na podstawie zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="53707-166">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="53707-167">Rejestrowanie niestandardowego `IAuthorizationPolicyProvider` typu w kolekcji usługi iniekcji zależności aplikacji (w `Startup.ConfigureServices`) aby zastąpić domyślny dostawca zasad.</span><span class="sxs-lookup"><span data-stu-id="53707-167">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="53707-168">Niestandardowe pełną `IAuthorizationPolicyProvider` próbka jest dostępna w [repozytorium GitHub aspnet/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="53707-168">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
