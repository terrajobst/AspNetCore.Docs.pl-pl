---
title: Autoryzacja Niestandardowa zasada dostawców w programie ASP.NET Core
author: mjrousos
description: Dowiedz się, jak używać niestandardowego IAuthorizationPolicyProvider w aplikacji ASP.NET Core można dynamicznie wygenerować zasady autoryzacji.
ms.author: riande
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: ca57a9fd8e3c11f15fe14bbe4538bc748c4c84b6
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444158"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Niestandardowi dostawcy zasad autoryzacji przy użyciu IAuthorizationPolicyProvider w programie ASP.NET Core 

Przez [Mike Rousos](https://github.com/mjrousos)

Zazwyczaj przy użyciu [autoryzacji opartej na zasadach](xref:security/authorization/policies), zasady są rejestrowane przez wywołanie metody `AuthorizationOptions.AddPolicy` jako część konfiguracji usługi autoryzacji. W niektórych scenariuszach może nie być możliwe (lub pożądane) do rejestrowania wszystkich zasad autoryzacji w ten sposób. W takich przypadkach można używać niestandardowego `IAuthorizationPolicyProvider` do kontrolowania, jak podano zasad autoryzacji.

Przykładowe scenariusze, w przypadku, gdy niestandardowego [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) może być przydatne, obejmują:

* Podaj oceny zasad przy użyciu usługi zewnętrznej.
* Przy użyciu dużych wielu zasad (w przypadku liczb różne pomieszczenia lub wieku, na przykład), więc nie ma sensu do dodania każdej zasady autoryzacji poszczególnych z `AuthorizationOptions.AddPolicy` wywołania.
* Tworzenie zasad w oparciu o informacje z zewnętrznego źródła danych (np. bazy danych) w czasie wykonywania, lub określanie wymagań autoryzacji dynamicznie za pośrednictwem innego mechanizmu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) z [repozytorium AspNetCore GitHub](https://github.com/aspnet/AspNetCore). Pobierz plik ZIP repozytorium aspnet/AspNetCore. Rozpakuj plik. Przejdź do *src/Security/samples/CustomPolicyProvider* folderu projektu.

## <a name="customize-policy-retrieval"></a>Dostosowywanie pobierania zasad

Aplikacje platformy ASP.NET Core korzystać z implementacji `IAuthorizationPolicyProvider` interfejsu można pobrać zasad autoryzacji. Domyślnie [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) jest zarejestrowany i używane. `DefaultAuthorizationPolicyProvider` Zwraca zasad z `AuthorizationOptions` podawany `IServiceCollection.AddAuthorization` wywołania.

To zachowanie można dostosować przez zarejestrowanie innego `IAuthorizationPolicyProvider` implementacji aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera. 

`IAuthorizationPolicyProvider` Interfejs zawiera dwa interfejsy API:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) zwraca zasady autoryzacji dla danej nazwy.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) zwraca domyślne zasady autoryzacji (zasady stosowane do `[Authorize]` atrybutów bez określone zasady). 

Wdrażając te dwa interfejsy API, można dostosować, jak zasady autoryzacji są dostarczane.

## <a name="parameterized-authorize-attribute-example"></a>Sparametryzowane autoryzować przykład atrybutu

Jeden scenariusz gdzie `IAuthorizationPolicyProvider` jest przydatne, jest zapewnienie niestandardowe `[Authorize]` atrybutów, którego wymagania zależą od parametru. Na przykład w [autoryzacji opartej na zasadach](xref:security/authorization/policies) dokumentacji, na podstawie wieku ("AtLeast21") zasad została użyta jako przykładu. Jeśli kontroler różnych akcji w aplikacji należy udostępniane użytkownikom *różnych* wieku, warto mieć wiele różnych zasad na podstawie wieku. Zamiast rejestracji wszystkie różne na podstawie wieku zasady wymagające aplikacji w `AuthorizationOptions`, można wygenerować zasady dynamicznie przy użyciu niestandardowego `IAuthorizationPolicyProvider`. Aby upewnić się, przy użyciu zasad jest łatwiejsze, może dodawać adnotacje akcji z atrybutem autoryzacja niestandardowa, takich jak `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Autoryzacja niestandardowa atrybutów

Zasady autoryzacji są identyfikowane przez ich nazwy. Niestandardowy `MinimumAgeAuthorizeAttribute` opisanego wcześniej potrzebuje do mapowania argumentów na ciąg, który może służyć do pobierania odpowiednie zasady autoryzacji. Możesz to zrobić, wynikające z `AuthorizeAttribute` i dokonując `Age` zawijania właściwość `AuthorizeAttribute.Policy` właściwości.

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

Ten typ atrybutu ma `Policy` ciągu na podstawie ustaloną prefiksu (`"MinimumAge"`) i całkowitą przekazanych za pośrednictwem konstruktora.

Można go zastosować do akcji w taki sam sposób jak inne `Authorize` atrybutów, z tą różnicą, że wykorzystuje całkowitą jako parametr.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Custom IAuthorizationPolicyProvider

Niestandardowy `MinimumAgeAuthorizeAttribute` ułatwia zasady autoryzacji żądania dla dowolnego minimalny wiek żądanego. Następny problem do rozwiązania jest upewnienie się, że zasady autoryzacji są dostępne dla wszystkich tych różnych wieku. Jest to miejsce `IAuthorizationPolicyProvider` przydaje się.

Korzystając z `MinimumAgeAuthorizationAttribute`, nazwy zasad autoryzacji będą oparte na wzorcu `"MinimumAge" + Age`, więc niestandardowej `IAuthorizationPolicyProvider` powinien wygenerować zasady autoryzacji przez:

* Analizowanie wieku na podstawie nazwy zasad.
* Za pomocą `AuthorizationPolicyBuilder` do tworzenia nowego `AuthorizationPolicy`
* Dodawanie wymagań do zasady oparte na wiek za pomocą `AuthorizationPolicyBuilder.AddRequirements`. W innych sytuacjach, można użyć `RequireClaim`, `RequireRole`, lub `RequireUserName` zamiast tego.

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

## <a name="multiple-authorization-policy-providers"></a>Wielu dostawców zasad autoryzacji

Korzystając z niestandardowego `IAuthorizationPolicyProvider` implementacji, należy pamiętać, który platformy ASP.NET Core używa tylko jedno wystąpienie `IAuthorizationPolicyProvider`. Jeśli niestandardowego dostawcy nie jest zapewnienie zasady autoryzacji dla wszystkich nazw zasady, jej należy wrócić do dostawcę kopii zapasowych. Nazwy zasad mogą obejmować te, które pochodzą z zasady domyślne dla `[Authorize]` atrybutów bez nazwy.

Na przykład należy rozważyć aplikacji wymagane zasady dotyczące wieku niestandardowych i bardziej tradycyjny pobierania zasad oparta na rolach. Takiej aplikacji można użyć niestandardowego zasad dostawcę autoryzacji który:

* Próbuje przeanalizować nazwy zasad. 
* Wywołania do dostawcy inne zasady (takich jak `DefaultAuthorizationPolicyProvider`) Jeśli nazwa zasad nie zawiera wieku.

## <a name="default-policy"></a>Zasady domyślne

Oprócz zapewniania zasady autoryzacji dla nazwanych, niestandardowe `IAuthorizationPolicyProvider` należy zaimplementować `GetDefaultPolicyAsync` zapewnienie zasady autoryzacji dla `[Authorize]` atrybutów bez określonej nazwy zasad.

W wielu przypadkach, ten atrybut autoryzacji wymaga tylko uwierzytelnionego użytkownika, dzięki czemu można zmieniać wymaganych zasad z wywołaniem `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Podobnie jak w przypadku wszystkich aspektów niestandardowego `IAuthorizationPolicyProvider`, to ustawienie można dostosować, zgodnie z potrzebami. W niektórych przypadkach:

* Domyślne zasady autoryzacji nie mogą być używane.
* Pobiera zasady domyślne mogą być delegowane do rezerwowe `IAuthorizationPolicyProvider`.

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Użyj niestandardowego IAuthorizationPolicyProvider

Korzystanie z niestandardowych zasad z `IAuthorizationPolicyProvider`, należy:

* Zarejestruj odpowiednie `AuthorizationHandler` typy za pomocą iniekcji zależności (opisanego w [autoryzacji opartej na zasadach](xref:security/authorization/policies#authorization-handlers)), podobnie jak w przypadku wszystkich scenariuszy na podstawie zasad autoryzacji.
* Rejestrowanie niestandardowego `IAuthorizationPolicyProvider` typu w kolekcji usługi iniekcji zależności aplikacji (w `Startup.ConfigureServices`) aby zastąpić domyślny dostawca zasad.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Niestandardowe pełną `IAuthorizationPolicyProvider` próbka jest dostępna w [repozytorium GitHub aspnet/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).
