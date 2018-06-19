---
title: Autoryzacja Niestandardowa zasada dostawców w platformy ASP.NET Core
author: mjrousos
description: Dowiedz się, jak używać niestandardowych IAuthorizationPolicyProvider w aplikacji platformy ASP.NET Core można dynamicznie wygenerować zasad autoryzacji.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: a5bad88b37d38b819b960b1eb27808d891268c01
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233446"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Niestandardowych dostawców zasad autoryzacji przy użyciu IAuthorizationPolicyProvider w ASP.NET Core 

Przez [Rousos Jan](https://github.com/mjrousos)

Zazwyczaj przy użyciu [autoryzacji na podstawie zasad](xref:security/authorization/policies), zasady są rejestrowane przez wywołanie metody `AuthorizationOptions.AddPolicy` w ramach konfiguracji usługi autoryzacji. W niektórych scenariuszach może nie być możliwe (lub pożądane) do rejestrowania wszystkich zasad autoryzacji w ten sposób. W takich przypadkach można użyć niestandardowego `IAuthorizationPolicyProvider` do kontrolowania, jak podano zasad autoryzacji.

Przykładowe scenariusze, w przypadku, gdy niestandardowego [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) mogą być przydatne, obejmują:

* Aby zapewnić oceny zasad za pomocą zewnętrznej usługi.
* Przy użyciu duży zakres zasad (w przypadku liczb różne pomieszczenia lub wieku, na przykład), więc go nie ma sensu dodać każdej zasady autoryzacji poszczególnych z `AuthorizationOptions.AddPolicy` wywołania.
* Tworzenie zasad w czasie wykonywania na podstawie informacji zewnętrznego źródła danych (takich jak bazy danych) lub określanie wymagań autoryzacji dynamicznie za pośrednictwem innego mechanizmu.

## <a name="customizing-policy-retrieval"></a>Dostosowywanie pobieranie zasad

Aplikacje platformy ASP.NET Core korzystać z implementacji `IAuthorizationPolicyProvider` interfejs do pobrania zasad autoryzacji. Domyślnie [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) jest zarejestrowany i używane. `DefaultAuthorizationPolicyProvider` Zwraca zasad z `AuthorizationOptions` w `IServiceCollection.AddAuthorization` wywołania.

To zachowanie można dostosować, rejestrując innej `IAuthorizationPolicyProvider` wdrożenia w aplikacji [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera. 

`IAuthorizationPolicyProvider` Interfejs zawiera dwa interfejsy API:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) zwraca zasady autoryzacji dla podanej nazwie.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) zwraca domyślne zasady autoryzacji (zasady dla `[Authorize]` atrybuty bez określone zasady). 

Zaimplementowanie tych dwóch interfejsów API, można dostosować, jak podano zasad autoryzacji.

## <a name="parameterized-authorize-attribute-example"></a>Sparametryzowane autoryzować przykład atrybutu

Jednym ze scenariuszy gdzie `IAuthorizationPolicyProvider` przydaje się jest włączenie niestandardowe `[Authorize]` atrybutów, których wymagania zależą od parametru. Na przykład w [autoryzacji na podstawie zasad](xref:security/authorization/policies) dokumentacji, na podstawie wieku ("AtLeast21") zasady została użyta jako przykład. Jeśli inny kontroler akcji w aplikacji powinna zostać udostępniona użytkownikom *różnych* rzadziej, może być warto mieć wiele różnych zasad na podstawie wieku. Zamiast zarejestrować wszystkie różne na podstawie wieku zasady wymagające aplikacji w `AuthorizationOptions`, można wygenerować zasady dynamicznie z niestandardowego `IAuthorizationPolicyProvider`. Aby przy użyciu zasady jest łatwiejsze, może dodawać adnotacje do działania z atrybutem przeprowadzania niestandardowej autoryzacji, takich jak `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Atrybuty niestandardowe autoryzacji

Zasady autoryzacji są identyfikowane przez ich nazwy. Niestandardowa `MinimumAgeAuthorizeAttribute` opisanego wcześniej należy mapować argumenty na ciąg, który może służyć do pobierania odpowiednie zasady autoryzacji. Aby to zrobić, wynikające z `AuthorizeAttribute` i `Age` zawijania właściwości `AuthorizeAttribute.Policy` właściwości.

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

Ten typ atrybutu ma `Policy` ciąg opartą na prefiksie ustalony (`"MinimumAge"`) i całkowitą przekazane za pośrednictwem konstruktora.

Można go zastosować do akcji w taki sam sposób jak inne `Authorize` atrybutów z tą różnicą, że trwa całkowitą jako parametr.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Niestandardowe IAuthorizationPolicyProvider

Niestandardowa `MinimumAgeAuthorizeAttribute` ułatwia zasad autoryzacji żądania dla dowolnego minimalnego wieku potrzebne. Następny problem do rozwiązania jest upewnienie się, zasady autoryzacji są dostępne dla wszystkich tych różnym wieku. Jest to, gdy `IAuthorizationPolicyProvider` jest użyteczne.

Korzystając z `MinimumAgeAuthorizationAttribute`, nazwy zasad autoryzacji będzie zgodne ze wzorcem `"MinimumAge" + Age`, więc niestandardowego `IAuthorizationPolicyProvider` powinna generować zasad autoryzacji przez:

* Podczas analizowania wieku z nazwę zasady.
* Przy użyciu `AuthorizationPolicyBuiler` do tworzenia nowego `AuthorizationPolicy`
* Dodawanie wymagań do zasady oparte na wiek za pomocą `AuthorizationPolicyBuilder.AddRequirements`. W innych sytuacjach można użyć `RequireClaim`, `RequireRole`, lub `RequireUserName` zamiast tego.

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

## <a name="multiple-authorization-policy-providers"></a>Wielu dostawców zasad autoryzacji

Podczas korzystania z niestandardowego `IAuthorizationPolicyProvider` implementacji, należy pamiętać, które platformy ASP.NET Core używa tylko jedno wystąpienie `IAuthorizationPolicyProvider`. Jeśli niestandardowego dostawcy nie jest zapewnienie zasady autoryzacji dla wszystkich nazw zasad, powinien on może wrócić do dostawcę kopii zapasowej. Nazwy zasad może obejmować te, które pochodzą z domyślnych zasad dla `[Authorize]` atrybuty bez nazwy.

Na przykład należy rozważyć aplikacji wymagane zarówno zasady dotyczące wieku niestandardowych, jak i bardziej tradycyjnej pobieranie zasad opartych na rolach. Takich aplikacji można użyć dostawcy zasad autoryzacji niestandardowej który:

* Próbuje przeanalizować nazwy zasad. 
* Wywołuje dostawcę różnych zasad (takie jak `DefaultAuthorizationPolicyProvider`), jeśli nazwa zasad nie zawiera wieku.

## <a name="default-policy"></a>Domyślne zasady

Oprócz zasad autoryzacji nazwanego, niestandardowego `IAuthorizationPolicyProvider` należy zaimplementować `GetDefaultPolicyAsync` zapewnienie zasady autoryzacji dla `[Authorize]` atrybuty bez określonej nazwy zasad.

W wielu przypadkach ten atrybut autoryzacji tylko wymaga uwierzytelnionego użytkownika, dzięki czemu można zmieniać niezbędne zasad wywołaniem `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

W przypadku wszystkich aspektów niestandardowego `IAuthorizationPolicyProvider`, to ustawienie można dostosować, zgodnie z potrzebami. W niektórych przypadkach:

* Domyślne zasady autoryzacji nie mogą być używane.
* Pobiera zasady domyślne można delegować do rezerwowe `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>Przy użyciu niestandardowych IAuthorizationPolicyProvider

Stosowanie niestandardowych zasad z `IAuthorizationPolicyProvider`, musisz:

* Zarejestruj odpowiedni `AuthorizationHandler` typy z iniekcji zależności (opisany w [autoryzacji na podstawie zasad](xref:security/authorization/policies#authorization-handlers)), podobnie jak w przypadku wszystkich scenariuszy opartych na zasadach autoryzacji.
* Rejestrowanie niestandardowe `IAuthorizationPolicyProvider` typu w kolekcji usługi iniekcji zależności aplikacji (w `Startup.ConfigureServices`) aby zastąpić domyślny dostawca zasad.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Niestandardowe pełną `IAuthorizationPolicyProvider` przykład jest dostępny w [repozytorium GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
