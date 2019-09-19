---
title: Niestandardowi dostawcy zasad autoryzacji w ASP.NET Core
author: mjrousos
description: Dowiedz się, jak używać niestandardowych IAuthorizationPolicyProvider w aplikacji ASP.NET Core do dynamicznego generowania zasad autoryzacji.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: b11f7f94e1042e65d5f2ff8ab0fe9ea7838bebeb
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108064"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="f356d-103">Niestandardowi dostawcy zasad autoryzacji korzystający z usługi IAuthorizationPolicyProvider w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f356d-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="f356d-104">Według [Jan Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="f356d-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="f356d-105">Zwykle podczas korzystania z [autoryzacji opartej na zasadach](xref:security/authorization/policies)zasady są rejestrowane przez `AuthorizationOptions.AddPolicy` wywołanie w ramach konfiguracji usługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f356d-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="f356d-106">W niektórych scenariuszach może nie być możliwe (lub pożądane) rejestrowanie wszystkich zasad autoryzacji w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="f356d-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="f356d-107">W takich przypadkach można użyć niestandardowych `IAuthorizationPolicyProvider` do kontrolowania sposobu dostarczania zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f356d-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="f356d-108">Przykłady scenariuszy, w których może być przydatne niestandardowe [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) :</span><span class="sxs-lookup"><span data-stu-id="f356d-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="f356d-109">Korzystanie z usługi zewnętrznej w celu zapewnienia oceny zasad.</span><span class="sxs-lookup"><span data-stu-id="f356d-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="f356d-110">Użycie dużego zakresu zasad (na przykład w przypadku różnych numerów pomieszczeń lub wieku), dlatego nie ma sensu dodania poszczególnych zasad `AuthorizationOptions.AddPolicy` autoryzacji do wywołania.</span><span class="sxs-lookup"><span data-stu-id="f356d-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="f356d-111">Tworzenie zasad w czasie wykonywania na podstawie informacji w zewnętrznym źródle danych (np. bazy danych) lub Określanie wymagań dotyczących autoryzacji w sposób dynamiczny przez inny mechanizm.</span><span class="sxs-lookup"><span data-stu-id="f356d-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="f356d-112">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) z [repozytorium GitHub AspNetCore](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="f356d-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="f356d-113">Pobierz plik ZIP repozytorium ASPNET/AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="f356d-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="f356d-114">Rozpakuj plik.</span><span class="sxs-lookup"><span data-stu-id="f356d-114">Unzip the file.</span></span> <span data-ttu-id="f356d-115">Przejdź do folderu *src/Security/Samples/CustomPolicyProvider* Project.</span><span class="sxs-lookup"><span data-stu-id="f356d-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="f356d-116">Dostosuj pobieranie zasad</span><span class="sxs-lookup"><span data-stu-id="f356d-116">Customize policy retrieval</span></span>

<span data-ttu-id="f356d-117">Aplikacje ASP.NET Core korzystają z implementacji `IAuthorizationPolicyProvider` interfejsu w celu pobierania zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f356d-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="f356d-118">Domyślnie [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) jest zarejestrowany i używany.</span><span class="sxs-lookup"><span data-stu-id="f356d-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="f356d-119">`DefaultAuthorizationPolicyProvider`zwraca zasady z `AuthorizationOptions` podanego `IServiceCollection.AddAuthorization` w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="f356d-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="f356d-120">To zachowanie można dostosować, rejestrując inną `IAuthorizationPolicyProvider` implementację w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f356d-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="f356d-121">`IAuthorizationPolicyProvider` Interfejs zawiera dwa interfejsy API:</span><span class="sxs-lookup"><span data-stu-id="f356d-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="f356d-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) zwraca zasady autoryzacji dla danej nazwy.</span><span class="sxs-lookup"><span data-stu-id="f356d-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="f356d-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) zwraca domyślne zasady autoryzacji (zasady używane dla `[Authorize]` atrybutów bez określonych zasad).</span><span class="sxs-lookup"><span data-stu-id="f356d-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="f356d-124">Implementując te dwa interfejsy API, można dostosowywać sposób zapewniania zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f356d-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="f356d-125">Przykładowy atrybut autoryzacji sparametryzowanej</span><span class="sxs-lookup"><span data-stu-id="f356d-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="f356d-126">Jednym z scenariuszy `IAuthorizationPolicyProvider` , gdzie jest przydatna, `[Authorize]` jest włączenie atrybutów niestandardowych, których wymagania zależą od parametru.</span><span class="sxs-lookup"><span data-stu-id="f356d-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="f356d-127">Na przykład w dokumentacji dotyczącej [autoryzacji opartej na zasadach](xref:security/authorization/policies) , jako przykład użyto zasad opartych na wieku ("AtLeast21").</span><span class="sxs-lookup"><span data-stu-id="f356d-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="f356d-128">Jeśli różne akcje kontrolera w aplikacji powinny być dostępne dla użytkowników z *różnych* okresów obowiązywania, może być przydatne w wielu różnych zasadach opartych na wieku.</span><span class="sxs-lookup"><span data-stu-id="f356d-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="f356d-129">Zamiast zarejestrowania wszystkich zasad opartych na wieku, do których aplikacja będzie potrzebna `AuthorizationOptions`, możesz dynamicznie generować zasady za pomocą niestandardowej. `IAuthorizationPolicyProvider`</span><span class="sxs-lookup"><span data-stu-id="f356d-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="f356d-130">Aby ułatwić korzystanie z zasad, można dodawać adnotacje do akcji przy użyciu niestandardowego atrybutu autoryzacji `[MinimumAgeAuthorize(20)]`, takiego jak.</span><span class="sxs-lookup"><span data-stu-id="f356d-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="f356d-131">Niestandardowe atrybuty autoryzacji</span><span class="sxs-lookup"><span data-stu-id="f356d-131">Custom Authorization attributes</span></span>

<span data-ttu-id="f356d-132">Zasady autoryzacji są identyfikowane przez ich nazwy.</span><span class="sxs-lookup"><span data-stu-id="f356d-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="f356d-133">Niestandardowe `MinimumAgeAuthorizeAttribute` opisane wcześniej muszą mapować argumenty na ciąg, którego można użyć do pobrania odpowiednich zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f356d-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="f356d-134">Można to zrobić, wyprowadzając z `AuthorizeAttribute` i `Age` ustawiając właściwość jako przewinięcie `AuthorizeAttribute.Policy` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f356d-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="f356d-135">Ten typ atrybutu ma `Policy` ciąg oparty na zakodowanym prefiksie (`"MinimumAge"`) i liczbie całkowitej przekazanej za pośrednictwem konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f356d-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="f356d-136">Można zastosować go do akcji w taki sam sposób, jak inne `Authorize` atrybuty, z wyjątkiem tego, że przyjmuje jako parametr liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="f356d-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="f356d-137">Niestandardowy IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="f356d-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="f356d-138">Niestandardowe `MinimumAgeAuthorizeAttribute` ułatwiają żądania zasad autoryzacji w przypadku dowolnych minimalnych wieku.</span><span class="sxs-lookup"><span data-stu-id="f356d-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="f356d-139">Następnym problemem do rozwiązania jest upewnienie się, że zasady autoryzacji są dostępne dla wszystkich tych różnych okresów.</span><span class="sxs-lookup"><span data-stu-id="f356d-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="f356d-140">Jest to miejsce, `IAuthorizationPolicyProvider` w którym jest to przydatne.</span><span class="sxs-lookup"><span data-stu-id="f356d-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="f356d-141">W przypadku `MinimumAgeAuthorizationAttribute`korzystania z programu nazwy zasad autoryzacji będą zgodne ze `"MinimumAge" + Age`wzorcem, więc `IAuthorizationPolicyProvider` niestandardowe powinny generować zasady autoryzacji, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="f356d-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="f356d-142">Analizowanie wieku z nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="f356d-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="f356d-143">`AuthorizationPolicyBuilder` Tworzenie nowego`AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="f356d-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="f356d-144">W tym i następujących przykładach zostanie przyjęty, że użytkownik jest uwierzytelniany za pośrednictwem pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="f356d-144">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="f356d-145">`AuthorizationPolicyBuilder` Należy utworzyć co najmniej jedną nazwę schematu autoryzacji lub zawsze powiodła się.</span><span class="sxs-lookup"><span data-stu-id="f356d-145">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="f356d-146">W przeciwnym razie nie ma informacji o sposobie zapewnienia użytkownikowi wyzwania i zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="f356d-146">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="f356d-147">Dodawanie wymagań do zasad na podstawie wieku w programie `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="f356d-147">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="f356d-148">W innych scenariuszach można użyć `RequireClaim`, `RequireRole`lub `RequireUserName` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="f356d-148">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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
            var policy = new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme);
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="f356d-149">Wielu dostawców zasad autoryzacji</span><span class="sxs-lookup"><span data-stu-id="f356d-149">Multiple authorization policy providers</span></span>

<span data-ttu-id="f356d-150">W przypadku korzystania `IAuthorizationPolicyProvider` z implementacji niestandardowych należy pamiętać, że ASP.NET Core używa tylko jednego `IAuthorizationPolicyProvider`wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="f356d-150">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="f356d-151">Jeśli dostawca niestandardowy nie jest w stanie dostarczyć zasad autoryzacji dla wszystkich nazw zasad, które będą używane, należy powracać do dostawcy kopii zapasowych.</span><span class="sxs-lookup"><span data-stu-id="f356d-151">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="f356d-152">Rozważmy na przykład aplikację, która wymaga zarówno niestandardowych zasad wieku, jak i bardziej tradycyjnych operacji pobierania zasad opartych na rolach.</span><span class="sxs-lookup"><span data-stu-id="f356d-152">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="f356d-153">Taka aplikacja może użyć niestandardowego dostawcy zasad autoryzacji, który:</span><span class="sxs-lookup"><span data-stu-id="f356d-153">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="f356d-154">Próbuje przeanalizować nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="f356d-154">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="f356d-155">Wywołuje innego dostawcę zasad (na przykład `DefaultAuthorizationPolicyProvider`), jeśli nazwa zasad nie zawiera wieku.</span><span class="sxs-lookup"><span data-stu-id="f356d-155">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="f356d-156">Przykładowa `IAuthorizationPolicyProvider` implementacja pokazana powyżej może zostać zaktualizowana w `DefaultAuthorizationPolicyProvider` celu użycia przez utworzenie dostawcy zasad rezerwowych w konstruktorze (do użycia w przypadku, gdy nazwa zasad nie jest zgodna z oczekiwanym wzorcem "minimalna" + wiek).</span><span class="sxs-lookup"><span data-stu-id="f356d-156">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="f356d-157">Następnie można zaktualizować `GetPolicyAsync` metodę, aby `FallbackPolicyProvider` użyć zamiast zwracanej wartości null:</span><span class="sxs-lookup"><span data-stu-id="f356d-157">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="f356d-158">Zasady domyślne</span><span class="sxs-lookup"><span data-stu-id="f356d-158">Default policy</span></span>

<span data-ttu-id="f356d-159">Oprócz dostarczania nazwanych zasad autoryzacji, niestandardową `IAuthorizationPolicyProvider` potrzebą jest `GetDefaultPolicyAsync` wdrożenie w celu udostępnienia zasad `[Authorize]` autoryzacji dla atrybutów bez określonej nazwy zasad.</span><span class="sxs-lookup"><span data-stu-id="f356d-159">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="f356d-160">W wielu przypadkach ten atrybut autoryzacji wymaga tylko uwierzytelnionego użytkownika, więc można wykonać niezbędne zasady z wywołaniem `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="f356d-160">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="f356d-161">Podobnie jak w przypadku wszystkich aspektów `IAuthorizationPolicyProvider`niestandardowych, można je dostosować w razie konieczności.</span><span class="sxs-lookup"><span data-stu-id="f356d-161">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="f356d-162">W niektórych przypadkach może być wskazane pobranie domyślnych zasad z rezerwy `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="f356d-162">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="f356d-163">Wymagane zasady</span><span class="sxs-lookup"><span data-stu-id="f356d-163">Required policy</span></span>

<span data-ttu-id="f356d-164">Niestandardowa `IAuthorizationPolicyProvider` konieczna `GetRequiredPolicyAsync` jest implementacja do, opcjonalnie, zapewnienie zasad, które są zawsze wymagane.</span><span class="sxs-lookup"><span data-stu-id="f356d-164">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="f356d-165">Jeśli `GetRequiredPolicyAsync` zwraca zasady o wartości innej niż null, te zasady zostaną połączone z innymi (nazwanymi lub domyślnymi) zasadami, które są żądane.</span><span class="sxs-lookup"><span data-stu-id="f356d-165">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="f356d-166">Jeśli wymagane zasady nie są potrzebne, dostawca może po prostu zwrócić wartość null lub przełożyć na dostawcę rezerwy:</span><span class="sxs-lookup"><span data-stu-id="f356d-166">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="f356d-167">Użyj niestandardowego IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="f356d-167">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="f356d-168">Aby używać zasad niestandardowych z poziomu `IAuthorizationPolicyProvider`programu, należy:</span><span class="sxs-lookup"><span data-stu-id="f356d-168">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="f356d-169">Zarejestruj odpowiednie `AuthorizationHandler` typy z iniekcją zależności (zgodnie z opisem w [autoryzacji opartej na zasadach](xref:security/authorization/policies#authorization-handlers)), podobnie jak w przypadku wszystkich scenariuszy autoryzacji opartych na zasadach.</span><span class="sxs-lookup"><span data-stu-id="f356d-169">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="f356d-170">Zarejestruj typ niestandardowy `IAuthorizationPolicyProvider` w kolekcji usług iniekcji zależności aplikacji (w programie `Startup.ConfigureServices`), aby zastąpić domyślnego dostawcę zasad.</span><span class="sxs-lookup"><span data-stu-id="f356d-170">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="f356d-171">W `IAuthorizationPolicyProvider` [repozytorium GitHub/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)można uzyskać pełny niestandardowy przykład.</span><span class="sxs-lookup"><span data-stu-id="f356d-171">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
