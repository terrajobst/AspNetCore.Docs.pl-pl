---
title: Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity
author: rick-anderson
description: Informacje o sposobie używania uwierzytelniania plików cookie bez tożsamości platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622748"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)

Tożsamości platformy ASP.NET Core to dostawca uwierzytelniania pełne, kompleksowe za utworzenie i utrzymywanie logowania. Jednak można dostawcy uwierzytelniania na podstawie plików cookie uwierzytelniania, bez tożsamości platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Dla celów demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika Maria Rodriguez jest zapisane na stałe do aplikacji. Użyj **E-mail** nazwy użytkownika `maria.rodriguez@contoso.com` i wszystkie hasła do logowania użytkownika. Użytkownik jest uwierzytelniany w `AuthenticateUser` method in Class metoda *Pages/Account/Login.cshtml.cs* pliku. Przykład rzeczywistych użytkownika może być uwierzytelniani względem bazy danych.

## <a name="configuration"></a>Konfiguracja

Jeśli aplikacja nie używa [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu.

W `Startup.ConfigureServices` metody tworzenia usługi oprogramowania pośredniczącego uwierzytelniania za pomocą <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> metody:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> przekazany do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji. `AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień pliku cookie uwierzytelniania, a użytkownik chce [autoryzacji przy użyciu określonego schematu](xref:security/authorization/limitingidentitybyscheme). Ustawienie `AuthenticationScheme` do [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) zawiera wartość "Plików cookie" dla schematu. Możesz podać dowolną wartość ciągu, która odróżnia systemu.

Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania pliku cookie aplikacji. Gdy nie jest udostępniane schematu uwierzytelniania pliku cookie <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa ona `CookieAuthenticationDefaults.AuthenticationScheme` ("plików cookie" ").

Plik cookie uwierzytelniania <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> właściwość jest ustawiona na `true` domyślnie. Pliki cookie uwierzytelniania są dozwolone, gdy użytkownik nie wyraził zgodę, do zbierania danych. Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.

W `Startup.Configure` metody, wywołanie `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości. Wywołaj `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.

Ustaw `CookieAuthenticationOptions` w konfiguracji usługi uwierzytelniania `Startup.ConfigureServices` metody:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Oprogramowaniu pośredniczącym pliku cookie zasad

[Oprogramowaniu pośredniczącym pliku cookie zasad](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) oferuje możliwości zasad pliku cookie. Dodawanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest kolejność liter&mdash;dotyczy tylko transmisji składników zarejestrowanych w potoku.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Użyj <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> udostępnionych plików Cookie zasad oprogramowania pośredniczącego, aby sterować globalne właściwości przetwarzania plików cookie oraz podłączania do obsługi przetwarzania plików cookie, gdy pliki cookie są dołączane lub usuwane.

Wartość domyślna <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> wartość `SameSiteMode.Lax` pozwalające uwierzytelniania OAuth2. Ściśle wymusić zasady tej samej lokacji `SameSiteMode.Strict`ustaw `MinimumSameSitePolicy`. Mimo że to ustawienie dzieli OAuth2 i innych schematów uwierzytelniania między źródłami, eksponuje poziom bezpieczeństwa pliku cookie dla innych typów aplikacji, które nie należy polegać na przetwarzanie żądań cross-origin.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Ustawienie oprogramowaniu pośredniczącym pliku Cookie zasad `MinimumSameSitePolicy` mogą mieć wpływ na ustawienie `Cookie.SameSite` w `CookieAuthenticationOptions` ustawienia zgodnie z poniższym macierzy.

| MinimumSameSitePolicy | Cookie.SameSite | Wynikowe ustawienie Cookie.SameSite |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Tworzenie pliku cookie uwierzytelniania

Aby utworzyć plik cookie zawierający informacje o użytkowniku, należy utworzyć <xref:System.Security.Claims.ClaimsPrincipal>. Informacje o użytkowniku są serializowane i przechowywane w pliku cookie. 

Tworzenie <xref:System.Security.Claims.ClaimsIdentity> z każdego wymaganego <xref:System.Security.Claims.Claim>s, a następnie wywołać <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> do logowania użytkownika:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` Tworzy zaszyfrowanego pliku cookie i dodaje go do bieżącej odpowiedzi. Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.

Platforma ASP.NET Core [ochrony danych](xref:security/data-protection/using-data-protection) systemu jest używany do szyfrowania. Dla aplikacji hostowanych na wielu komputerach, obciążenia równoważenia w aplikacjach lub przy użyciu farmy sieci web [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) korzystać z tej samej pierścień klucz i identyfikator aplikacji.

## <a name="sign-out"></a>Wyloguj

Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "Plików cookie" ") nie jest używany jako schematu (na przykład" ContosoCookie"), podaj Schemat używany podczas konfigurowania dostawcy uwierzytelniania. W przeciwnym razie używany jest domyślny schemat.

## <a name="react-to-back-end-changes"></a>Reagowanie na zmiany zaplecza

Po utworzeniu pliku cookie, plik cookie jest jedynym źródłem tożsamości. Jeśli konto użytkownika jest wyłączona w systemów zaplecza:

* System uwierzytelniania pliku cookie aplikacji w dalszym ciągu przetwarzać żądania oparte na pliku cookie uwierzytelniania.
* Użytkownik pozostaje zalogowany do aplikacji, tak długo, jak plik cookie uwierzytelniania jest prawidłowy.

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Zdarzeń może służyć do przechwycenia, a następnie zastąpić weryfikacji tożsamości pliku cookie. Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko związane odwołanych użytkowników, dostęp do aplikacji.

Jedno z podejść do weryfikacji opiera się na rejestrowanie informacji o zmianach w bazie danych użytkownika. Jeśli baza danych nie zostały zmodyfikowane, ponieważ został wystawiony przez użytkownika w pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli ich plik cookie jest nadal ważny. W przykładową aplikację, baza danych jest realizowane w `IUserRepository` i przechowuje `LastChanged` wartość. Gdy użytkownik zostanie zaktualizowany w bazie danych, `LastChanged` wartość jest ustawiona na bieżącą godzinę.

W celu unieważnienia pliku cookie, gdy na podstawie zmian w bazie danych `LastChanged` wartość, należy utworzyć plik cookie z `LastChanged` oświadczenia, zawierającego bieżącego `LastChanged` wartość z bazy danych:

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

Aby zaimplementować zastąpienie `ValidatePrincipal` zdarzeń, napisz metody z następującą sygnaturą w klasie, która pochodzi od klasy <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Poniżej przedstawiono przykład implementacji `CookieAuthenticationEvents`:

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Rejestrowanie wystąpienia zdarzenia podczas rejestracji usługa plików cookie w `Startup.ConfigureServices` metody. Podaj [zakresie rejestracji usługi](xref:fundamentals/dependency-injection#service-lifetimes) dla Twojego `CustomCookieAuthenticationEvents` klasy:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Rozważmy sytuację, w którym nazwa użytkownika jest aktualizowana&mdash;decyzji, który nie ma wpływu na zabezpieczenia w dowolny sposób. Jeśli chcesz zaktualizować bezpieczny głównej nazwy użytkownika, wywołaj `context.ReplacePrincipal` i ustaw `context.ShouldRenew` właściwość `true`.

> [!WARNING]
> W sposób opisany poniżej są wyzwalane na każde żądanie. Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników na każde żądanie może spowodować zmniejszenie wydajności dużych aplikacji.

## <a name="persistent-cookies"></a>Trwałe pliki cookie

Możesz chcieć plików cookie, aby zachować w wielu sesjach przeglądarki. Ten stan trwały można włączyć tylko za zgodą użytkownika z polem wyboru "Pamiętaj mnie" na temat logowania się lub podobny mechanizm. 

Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który przeżyje za pośrednictwem przeglądarki zamknięcia. Ustawienia wygaśnięcia przewijania wcześniej skonfigurowane są uznawane. Jeśli plik cookie wygasa, gdy nastąpi zamknięcie okna przeglądarki, przeglądarki usuwa plik cookie po ponownym uruchomieniu.

Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> do `true` w <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>Datę ważności pliku cookie bezwzględne

Czas wygaśnięcia bezwzględny, można ustawić za pomocą <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Aby utworzyć trwały plik cookie `IsPersistent` również musi być ustawiona. W przeciwnym razie plik cookie jest tworzony o okresie istnienia, oparte na sesji i można wycofać, ani przed ani po bilet uwierzytelniania, który przechowuje. Gdy `ExpiresUtc` jest ustawiony, zastępuje ona wartość <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opcji <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, jeśli ustawiona.

Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który jest ważny przez 20 minut. To ignoruje wszelkie przewijania wcześniej skonfigurowane ustawienia wygaśnięcia.

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Rola oparta na zasadach kontroli](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
