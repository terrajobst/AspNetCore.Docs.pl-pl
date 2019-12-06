---
title: Niestandardowi dostawcy zasad autoryzacji w ASP.NET Core
author: mjrousos
description: Dowiedz się, jak używać niestandardowych IAuthorizationPolicyProvider w aplikacji ASP.NET Core do dynamicznego generowania zasad autoryzacji.
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: fe07a113a29ed3e14679e3f3f2249b0810c17593
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880705"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Niestandardowi dostawcy zasad autoryzacji korzystający z usługi IAuthorizationPolicyProvider w ASP.NET Core 

Według [Jan Rousos](https://github.com/mjrousos)

Zwykle podczas korzystania z [autoryzacji opartej na zasadach](xref:security/authorization/policies)zasady są rejestrowane przez wywołanie `AuthorizationOptions.AddPolicy` w ramach konfiguracji usługi autoryzacji. W niektórych scenariuszach może nie być możliwe (lub pożądane) rejestrowanie wszystkich zasad autoryzacji w ten sposób. W takich przypadkach można użyć niestandardowego `IAuthorizationPolicyProvider` do kontrolowania sposobu dostarczania zasad autoryzacji.

Przykłady scenariuszy, w których może być przydatne niestandardowe [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) :

* Korzystanie z usługi zewnętrznej w celu zapewnienia oceny zasad.
* Użycie dużego zakresu zasad (na przykład dla różnych numerów pomieszczeń lub wieku), dlatego nie ma sensu dodania poszczególnych zasad autoryzacji z wywołaniem `AuthorizationOptions.AddPolicy`.
* Tworzenie zasad w czasie wykonywania na podstawie informacji w zewnętrznym źródle danych (np. bazy danych) lub Określanie wymagań dotyczących autoryzacji w sposób dynamiczny przez inny mechanizm.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) z [repozytorium GitHub AspNetCore](https://github.com/aspnet/AspNetCore). Pobierz plik ZIP repozytorium ASPNET/AspNetCore. Rozpakuj plik. Przejdź do folderu *src/Security/Samples/CustomPolicyProvider* Project.

## <a name="customize-policy-retrieval"></a>Dostosuj pobieranie zasad

Aplikacje ASP.NET Core używają implementacji interfejsu `IAuthorizationPolicyProvider` do pobierania zasad autoryzacji. Domyślnie [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) jest zarejestrowany i używany. `DefaultAuthorizationPolicyProvider` zwraca zasady z `AuthorizationOptions` podanego w wywołaniu `IServiceCollection.AddAuthorization`.

Dostosuj to zachowanie, rejestrując inną implementację `IAuthorizationPolicyProvider` w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji. 

Interfejs `IAuthorizationPolicyProvider` zawiera trzy interfejsy API:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) zwraca zasady autoryzacji dla danej nazwy.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) zwraca domyślne zasady autoryzacji (zasady używane dla atrybutów `[Authorize]` bez określonych zasad). 
* [GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) zwraca rezerwowe zasady autoryzacji (zasady używane przez oprogramowanie pośredniczące autoryzacji, gdy nie określono żadnych zasad). 

Implementując te interfejsy API, można dostosować sposób, w jaki są udostępniane zasady autoryzacji.

## <a name="parameterized-authorize-attribute-example"></a>Przykładowy atrybut autoryzacji sparametryzowanej

Jeden scenariusz, w którym `IAuthorizationPolicyProvider` jest przydatna, włącza niestandardowe atrybuty `[Authorize]`, których wymagania zależą od parametru. Na przykład w dokumentacji dotyczącej [autoryzacji opartej na zasadach](xref:security/authorization/policies) , jako przykład użyto zasad opartych na wieku ("AtLeast21"). Jeśli różne akcje kontrolera w aplikacji powinny być dostępne dla użytkowników z *różnych* okresów obowiązywania, może być przydatne w wielu różnych zasadach opartych na wieku. Zamiast rejestrować wszystkie różne zasady oparte na wieku, których aplikacja będzie potrzebować w `AuthorizationOptions`, możesz dynamicznie generować zasady za pomocą niestandardowego `IAuthorizationPolicyProvider`. Aby ułatwić korzystanie z zasad, można dodawać adnotacje do akcji z niestandardowym atrybutem autoryzacji, takim jak `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Niestandardowe atrybuty autoryzacji

Zasady autoryzacji są identyfikowane przez ich nazwy. Niestandardowe `MinimumAgeAuthorizeAttribute` opisane wcześniej muszą mapować argumenty na ciąg, którego można użyć do pobrania odpowiednich zasad autoryzacji. Można to zrobić, wyprowadzając z `AuthorizeAttribute` i ustawiając właściwość `Age`, zawijając Właściwość `AuthorizeAttribute.Policy`.

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

Ten typ atrybutu ma `Policy` ciąg oparty na zakodowanym prefiksie (`"MinimumAge"`) i liczbie całkowitej przekazanej za pośrednictwem konstruktora.

Można go zastosować do akcji w taki sam sposób jak inne atrybuty `Authorize`, z tą różnicą, że przyjmuje liczbę całkowitą jako parametr.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Niestandardowy IAuthorizationPolicyProvider

Niestandardowa `MinimumAgeAuthorizeAttribute` ułatwia żądanie zasad autoryzacji w dowolnym minimalnym wieku. Następnym problemem do rozwiązania jest upewnienie się, że zasady autoryzacji są dostępne dla wszystkich tych różnych okresów. Jest to miejsce, w którym `IAuthorizationPolicyProvider` jest przydatna.

W przypadku korzystania z `MinimumAgeAuthorizationAttribute`nazwy zasad autoryzacji będą zgodne ze wzorcem `"MinimumAge" + Age`, dzięki czemu niestandardowe `IAuthorizationPolicyProvider` powinny generować zasady autoryzacji, wykonując następujące czynności:

* Analizowanie wieku z nazwy zasad.
* Tworzenie nowego `AuthorizationPolicy` przy użyciu `AuthorizationPolicyBuilder`
* W tym i następujących przykładach zostanie przyjęty, że użytkownik jest uwierzytelniany za pośrednictwem pliku cookie. `AuthorizationPolicyBuilder` należy utworzyć przy użyciu co najmniej jednej nazwy schematu autoryzacji lub zawsze powiedzie się. W przeciwnym razie nie ma informacji o sposobie zapewnienia użytkownikowi wyzwania i zostanie zgłoszony wyjątek.
* Dodawanie wymagań do zasad na podstawie wieku z `AuthorizationPolicyBuilder.AddRequirements`. W innych scenariuszach można zamiast tego użyć `RequireClaim`, `RequireRole`lub `RequireUserName`.

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

## <a name="multiple-authorization-policy-providers"></a>Wielu dostawców zasad autoryzacji

W przypadku korzystania z niestandardowych implementacji `IAuthorizationPolicyProvider` należy pamiętać, że ASP.NET Core używa tylko jednego wystąpienia `IAuthorizationPolicyProvider`. Jeśli dostawca niestandardowy nie jest w stanie zapewnić zasad autoryzacji dla wszystkich nazw zasad, które będą używane, należy odroczyć dostawcę kopii zapasowych. 

Rozważmy na przykład aplikację, która wymaga zarówno niestandardowych zasad wieku, jak i bardziej tradycyjnych operacji pobierania zasad opartych na rolach. Taka aplikacja może użyć niestandardowego dostawcy zasad autoryzacji, który:

* Próbuje przeanalizować nazwy zasad. 
* Wywołuje innego dostawcę zasad (na przykład `DefaultAuthorizationPolicyProvider`), jeśli nazwa zasad nie zawiera wieku.

Powyższa Przykładowa implementacja `IAuthorizationPolicyProvider` może zostać zaktualizowana w celu użycia `DefaultAuthorizationPolicyProvider` przez utworzenie dostawcy zasad kopii zapasowych w konstruktorze (do użycia w przypadku, gdy nazwa zasad nie jest zgodna z oczekiwanym wzorcem "minimalnie" + wiek).

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Następnie można zaktualizować metodę `GetPolicyAsync`, aby użyć `BackupPolicyProvider` zamiast zwracać wartości null:

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Zasady domyślne

Oprócz zapewnienia zasad autoryzacji Nazwa niestandardowa `IAuthorizationPolicyProvider` musi implementować `GetDefaultPolicyAsync` w celu zapewnienia zasad autoryzacji dla atrybutów `[Authorize]` bez określonej nazwy zasad.

W wielu przypadkach ten atrybut autoryzacji wymaga tylko uwierzytelnionego użytkownika, więc można wykonać niezbędne zasady z wywołaniem do `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

Podobnie jak w przypadku wszystkich aspektów niestandardowych `IAuthorizationPolicyProvider`, w razie konieczności można je dostosować. W niektórych przypadkach może być wskazane pobranie domyślnych zasad z `IAuthorizationPolicyProvider`rezerwowej.

## <a name="fallback-policy"></a>Zasady powrotu

Niestandardowa `IAuthorizationPolicyProvider` może opcjonalnie zaimplementować `GetFallbackPolicyAsync`, aby określić zasady, które są używane podczas [łączenia zasad](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) i gdy nie określono żadnych zasad. Jeśli `GetFallbackPolicyAsync` zwraca zasadę o wartości innej niż null, zwracane zasady są używane przez oprogramowanie pośredniczące autoryzacji, gdy dla żądania nie określono żadnych zasad.

Jeśli nie są wymagane żadne zasady powrotu dostawcy, dostawca może zwrócić `null` lub odroczyć dostawcę rezerwowego:

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Użyj niestandardowego IAuthorizationPolicyProvider

Aby używać zasad niestandardowych z poziomu `IAuthorizationPolicyProvider`, należy:

* Zarejestrowanie odpowiednich typów `AuthorizationHandler` z iniekcją zależności (opisanymi w temacie [Autoryzacja oparta na zasadach](xref:security/authorization/policies#authorization-handlers)), podobnie jak w przypadku wszystkich scenariuszy autoryzacji opartych na zasadach.
* Zarejestruj niestandardowy typ `IAuthorizationPolicyProvider` w kolekcji usługi iniekcji zależności aplikacji (w `Startup.ConfigureServices`), aby zastąpić domyślnego dostawcę zasad.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

W [repozytorium GitHub/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)można uzyskać pełny niestandardowy przykład `IAuthorizationPolicyProvider`.
