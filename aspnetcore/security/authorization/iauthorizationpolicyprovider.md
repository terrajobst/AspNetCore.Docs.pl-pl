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
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Niestandardowi dostawcy zasad autoryzacji korzystający z usługi IAuthorizationPolicyProvider w ASP.NET Core 

Według [Jan Rousos](https://github.com/mjrousos)

Zwykle podczas korzystania z [autoryzacji opartej na zasadach](xref:security/authorization/policies)zasady są rejestrowane przez `AuthorizationOptions.AddPolicy` wywołanie w ramach konfiguracji usługi autoryzacji. W niektórych scenariuszach może nie być możliwe (lub pożądane) rejestrowanie wszystkich zasad autoryzacji w ten sposób. W takich przypadkach można użyć niestandardowych `IAuthorizationPolicyProvider` do kontrolowania sposobu dostarczania zasad autoryzacji.

Przykłady scenariuszy, w których może być przydatne niestandardowe [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) :

* Korzystanie z usługi zewnętrznej w celu zapewnienia oceny zasad.
* Użycie dużego zakresu zasad (na przykład w przypadku różnych numerów pomieszczeń lub wieku), dlatego nie ma sensu dodania poszczególnych zasad `AuthorizationOptions.AddPolicy` autoryzacji do wywołania.
* Tworzenie zasad w czasie wykonywania na podstawie informacji w zewnętrznym źródle danych (np. bazy danych) lub Określanie wymagań dotyczących autoryzacji w sposób dynamiczny przez inny mechanizm.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) z [repozytorium GitHub AspNetCore](https://github.com/aspnet/AspNetCore). Pobierz plik ZIP repozytorium ASPNET/AspNetCore. Rozpakuj plik. Przejdź do folderu *src/Security/Samples/CustomPolicyProvider* Project.

## <a name="customize-policy-retrieval"></a>Dostosuj pobieranie zasad

Aplikacje ASP.NET Core korzystają z implementacji `IAuthorizationPolicyProvider` interfejsu w celu pobierania zasad autoryzacji. Domyślnie [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) jest zarejestrowany i używany. `DefaultAuthorizationPolicyProvider`zwraca zasady z `AuthorizationOptions` podanego `IServiceCollection.AddAuthorization` w wywołaniu.

To zachowanie można dostosować, rejestrując inną `IAuthorizationPolicyProvider` implementację w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji. 

`IAuthorizationPolicyProvider` Interfejs zawiera dwa interfejsy API:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) zwraca zasady autoryzacji dla danej nazwy.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) zwraca domyślne zasady autoryzacji (zasady używane dla `[Authorize]` atrybutów bez określonych zasad). 

Implementując te dwa interfejsy API, można dostosowywać sposób zapewniania zasad autoryzacji.

## <a name="parameterized-authorize-attribute-example"></a>Przykładowy atrybut autoryzacji sparametryzowanej

Jednym z scenariuszy `IAuthorizationPolicyProvider` , gdzie jest przydatna, `[Authorize]` jest włączenie atrybutów niestandardowych, których wymagania zależą od parametru. Na przykład w dokumentacji dotyczącej [autoryzacji opartej na zasadach](xref:security/authorization/policies) , jako przykład użyto zasad opartych na wieku ("AtLeast21"). Jeśli różne akcje kontrolera w aplikacji powinny być dostępne dla użytkowników z *różnych* okresów obowiązywania, może być przydatne w wielu różnych zasadach opartych na wieku. Zamiast zarejestrowania wszystkich zasad opartych na wieku, do których aplikacja będzie potrzebna `AuthorizationOptions`, możesz dynamicznie generować zasady za pomocą niestandardowej. `IAuthorizationPolicyProvider` Aby ułatwić korzystanie z zasad, można dodawać adnotacje do akcji przy użyciu niestandardowego atrybutu autoryzacji `[MinimumAgeAuthorize(20)]`, takiego jak.

## <a name="custom-authorization-attributes"></a>Niestandardowe atrybuty autoryzacji

Zasady autoryzacji są identyfikowane przez ich nazwy. Niestandardowe `MinimumAgeAuthorizeAttribute` opisane wcześniej muszą mapować argumenty na ciąg, którego można użyć do pobrania odpowiednich zasad autoryzacji. Można to zrobić, wyprowadzając z `AuthorizeAttribute` i `Age` ustawiając właściwość jako przewinięcie `AuthorizeAttribute.Policy` właściwości.

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

Można zastosować go do akcji w taki sam sposób, jak inne `Authorize` atrybuty, z wyjątkiem tego, że przyjmuje jako parametr liczbę całkowitą.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Niestandardowy IAuthorizationPolicyProvider

Niestandardowe `MinimumAgeAuthorizeAttribute` ułatwiają żądania zasad autoryzacji w przypadku dowolnych minimalnych wieku. Następnym problemem do rozwiązania jest upewnienie się, że zasady autoryzacji są dostępne dla wszystkich tych różnych okresów. Jest to miejsce, `IAuthorizationPolicyProvider` w którym jest to przydatne.

W przypadku `MinimumAgeAuthorizationAttribute`korzystania z programu nazwy zasad autoryzacji będą zgodne ze `"MinimumAge" + Age`wzorcem, więc `IAuthorizationPolicyProvider` niestandardowe powinny generować zasady autoryzacji, wykonując następujące czynności:

* Analizowanie wieku z nazwy zasad.
* `AuthorizationPolicyBuilder` Tworzenie nowego`AuthorizationPolicy`
* W tym i następujących przykładach zostanie przyjęty, że użytkownik jest uwierzytelniany za pośrednictwem pliku cookie. `AuthorizationPolicyBuilder` Należy utworzyć co najmniej jedną nazwę schematu autoryzacji lub zawsze powiodła się. W przeciwnym razie nie ma informacji o sposobie zapewnienia użytkownikowi wyzwania i zostanie zgłoszony wyjątek.
* Dodawanie wymagań do zasad na podstawie wieku w programie `AuthorizationPolicyBuilder.AddRequirements`. W innych scenariuszach można użyć `RequireClaim`, `RequireRole`lub `RequireUserName` zamiast tego.

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

W przypadku korzystania `IAuthorizationPolicyProvider` z implementacji niestandardowych należy pamiętać, że ASP.NET Core używa tylko jednego `IAuthorizationPolicyProvider`wystąpienia. Jeśli dostawca niestandardowy nie jest w stanie dostarczyć zasad autoryzacji dla wszystkich nazw zasad, które będą używane, należy powracać do dostawcy kopii zapasowych. 

Rozważmy na przykład aplikację, która wymaga zarówno niestandardowych zasad wieku, jak i bardziej tradycyjnych operacji pobierania zasad opartych na rolach. Taka aplikacja może użyć niestandardowego dostawcy zasad autoryzacji, który:

* Próbuje przeanalizować nazwy zasad. 
* Wywołuje innego dostawcę zasad (na przykład `DefaultAuthorizationPolicyProvider`), jeśli nazwa zasad nie zawiera wieku.

Przykładowa `IAuthorizationPolicyProvider` implementacja pokazana powyżej może zostać zaktualizowana w `DefaultAuthorizationPolicyProvider` celu użycia przez utworzenie dostawcy zasad rezerwowych w konstruktorze (do użycia w przypadku, gdy nazwa zasad nie jest zgodna z oczekiwanym wzorcem "minimalna" + wiek).

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Następnie można zaktualizować `GetPolicyAsync` metodę, aby `FallbackPolicyProvider` użyć zamiast zwracanej wartości null:

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Zasady domyślne

Oprócz dostarczania nazwanych zasad autoryzacji, niestandardową `IAuthorizationPolicyProvider` potrzebą jest `GetDefaultPolicyAsync` wdrożenie w celu udostępnienia zasad `[Authorize]` autoryzacji dla atrybutów bez określonej nazwy zasad.

W wielu przypadkach ten atrybut autoryzacji wymaga tylko uwierzytelnionego użytkownika, więc można wykonać niezbędne zasady z wywołaniem `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

Podobnie jak w przypadku wszystkich aspektów `IAuthorizationPolicyProvider`niestandardowych, można je dostosować w razie konieczności. W niektórych przypadkach może być wskazane pobranie domyślnych zasad z rezerwy `IAuthorizationPolicyProvider`.

## <a name="required-policy"></a>Wymagane zasady

Niestandardowa `IAuthorizationPolicyProvider` konieczna `GetRequiredPolicyAsync` jest implementacja do, opcjonalnie, zapewnienie zasad, które są zawsze wymagane. Jeśli `GetRequiredPolicyAsync` zwraca zasady o wartości innej niż null, te zasady zostaną połączone z innymi (nazwanymi lub domyślnymi) zasadami, które są żądane.

Jeśli wymagane zasady nie są potrzebne, dostawca może po prostu zwrócić wartość null lub przełożyć na dostawcę rezerwy:

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Użyj niestandardowego IAuthorizationPolicyProvider

Aby używać zasad niestandardowych z poziomu `IAuthorizationPolicyProvider`programu, należy:

* Zarejestruj odpowiednie `AuthorizationHandler` typy z iniekcją zależności (zgodnie z opisem w [autoryzacji opartej na zasadach](xref:security/authorization/policies#authorization-handlers)), podobnie jak w przypadku wszystkich scenariuszy autoryzacji opartych na zasadach.
* Zarejestruj typ niestandardowy `IAuthorizationPolicyProvider` w kolekcji usług iniekcji zależności aplikacji (w programie `Startup.ConfigureServices`), aby zastąpić domyślnego dostawcę zasad.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

W `IAuthorizationPolicyProvider` [repozytorium GitHub/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)można uzyskać pełny niestandardowy przykład.
