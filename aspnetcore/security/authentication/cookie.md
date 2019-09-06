---
title: Korzystanie z uwierzytelniania plików cookie bez tożsamości ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać uwierzytelniania przy użyciu plików cookie bez tożsamości ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/20/2019
uid: security/authentication/cookie
ms.openlocfilehash: 76c7fc20c8870668ca7c65d975e2ed59f40f7dc8
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384830"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Korzystanie z uwierzytelniania plików cookie bez tożsamości ASP.NET Core

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core Identity to kompletny, w pełni funkcjonalny Dostawca uwierzytelniania do tworzenia i obsługiwania logowań. Jednak można użyć dostawcy uwierzytelniania uwierzytelniania opartego na plikach cookie bez tożsamości ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

W celach demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika, Maria Rodriguez jest stałe do aplikacji. Zaloguj się przy użyciu `maria.rodriguez@contoso.com` adresu e-mail i hasła. Użytkownik jest uwierzytelniany w `AuthenticateUser` metodzie w pliku *Pages/Account/Login. cshtml. cs* . W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.

## <a name="configuration"></a>Konfiguracja

Jeśli aplikacja nie korzysta z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla pakietu [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .

W metodzie Utwórz usługi pośredniczące uwierzytelniania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> przy użyciu metod i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>: `Startup.ConfigureServices`

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>przeszedł `AddAuthentication` do ustawia domyślny schemat uwierzytelniania dla aplikacji. `AuthenticationScheme`jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzować z określonym schematem](xref:security/authorization/limitingidentitybyscheme). Ustawienie parametru na CookieAuthenticationDefaults. AuthenticationScheme zapewnia wartość "cookies" dla schematu. [](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) `AuthenticationScheme` Można podać dowolną wartość ciągu odróżniającą schemat.

Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania plików cookie aplikacji. Gdy schemat uwierzytelniania plików cookie nie jest dostarczany <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa `CookieAuthenticationDefaults.AuthenticationScheme` go ("pliki cookie").

<xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> Właściwość pliku cookie uwierzytelniania jest domyślnie ustawiona na `true` wartość. Pliki cookie uwierzytelniania są dozwolone, gdy osoby odwiedzające witrynę nie wyraziły zgody na zbieranie danych. Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.

W `Startup.Configure`, wywołaj `UseAuthentication` i `UseAuthorization` , aby `HttpContext.User` ustawić właściwość i uruchamiać oprogramowanie pośredniczące autoryzacji dla żądań. Wywołaj metody `UseAuthorization` `UseEndpoints`i przed `UseAuthentication` wywołaniem:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Klasa służy do konfigurowania opcji dostawcy uwierzytelniania.

Ustaw `CookieAuthenticationOptions` w konfiguracji usługi na potrzeby uwierzytelniania `Startup.ConfigureServices` w metodzie:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Oprogramowanie pośredniczące zasad dotyczących plików cookie

[Oprogramowanie pośredniczące zasad plików cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) umożliwia korzystanie z funkcji zasad dotyczących plików cookie. Dodanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest zgodne z&mdash;kolejnością, ma wpływ tylko na składniki podrzędne zarejestrowane w potoku.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Zastosowanie <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> do oprogramowania pośredniczącego zasad dotyczących plików cookie pozwala kontrolować globalne właściwości przetwarzania plików cookie i przełączać się do programów obsługi przetwarzania plików cookie, gdy pliki cookie są dodawane lub usuwane.

Wartością domyślną <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> jest `SameSiteMode.Lax` umożliwienie uwierzytelniania OAuth2. Aby ściśle wymuszać zasady dotyczące `SameSiteMode.Strict`tej samej lokacji, należy `MinimumSameSitePolicy`ustawić. Mimo że to ustawienie powoduje przerwanie OAuth2 i innych schematów uwierzytelniania między źródłami, podnosi poziom bezpieczeństwa plików cookie dla innych typów aplikacji, które nie polegają na przetwarzaniu żądań między źródłami.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Ustawienie ustawienia pośredniczące zasad dotyczących plików `MinimumSameSitePolicy` cookie dla programu może mieć `Cookie.SameSite` wpływ `CookieAuthenticationOptions` na ustawienia w ustawieniach zgodnie z poniższą macierzą.

| MinimumSameSitePolicy | Cookie.SameSite | Wynikowy plik cookie. SameSite ustawienie |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Utwórz plik cookie uwierzytelniania

Aby utworzyć plik cookie przechowujący informacje o użytkowniku <xref:System.Security.Claims.ClaimsPrincipal>, konstrukcja a. Informacje o użytkowniku są serializowane i przechowywane w pliku cookie. 

Utwórz z <xref:System.Security.Claims.ClaimsIdentity> dowolnym wymaganym <xref:System.Security.Claims.Claim>elementem s i <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> Wywołaj, aby zalogować użytkownika:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync`tworzy zaszyfrowany plik cookie i dodaje go do bieżącej odpowiedzi. Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.

System [ochrony danych](xref:security/data-protection/using-data-protection) ASP.NET Core jest używany do szyfrowania. W przypadku aplikacji hostowanej na wielu komputerach, równoważenia obciążenia między aplikacjami lub korzystania z kolektywu serwerów sieci Web należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) tak, aby używała tego samego dzwonka klucza i identyfikatora aplikacji.

## <a name="sign-out"></a>Wyloguj

Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "cookie") nie jest używany jako schemat (na przykład "ContosoCookie"), należy podać schemat używany podczas konfigurowania dostawcy uwierzytelniania. W przeciwnym razie używany jest domyślny schemat.

## <a name="react-to-back-end-changes"></a>Reagowanie na zmiany zaplecza

Po utworzeniu pliku cookie plik cookie jest pojedynczym źródłem tożsamości. Jeśli konto użytkownika jest wyłączone w systemach zaplecza:

* System uwierzytelniania plików cookie aplikacji kontynuuje przetwarzanie żądań na podstawie pliku cookie uwierzytelniania.
* Użytkownik pozostaje zalogowany do aplikacji, o ile plik cookie uwierzytelniania jest prawidłowy.

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Zdarzenia można użyć do przechwycenia i zastąpienia weryfikacji tożsamości pliku cookie. Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko odwołujący się użytkowników, którzy uzyskują dostęp do aplikacji.

Jedno z metod weryfikacji plików cookie polega na śledzeniu zmian w bazie danych użytkownika. Jeśli baza danych nie została zmieniona od czasu wystawienia pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli plik cookie jest nadal ważny. W przykładowej aplikacji baza danych jest zaimplementowana `IUserRepository` w i `LastChanged` przechowuje wartość. Gdy użytkownik zostanie zaktualizowany w bazie danych, `LastChanged` wartość jest ustawiana na bieżącą godzinę.

Aby unieważnić plik cookie, gdy baza danych zmienia się na podstawie `LastChanged` wartości, Utwórz plik cookie `LastChanged` z zastrzeżeniem zawierającym bieżącą `LastChanged` wartość z bazy danych:

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

Aby zaimplementować przesłonięcie dla `ValidatePrincipal` zdarzenia, napisz metodę o następującym podpisie w klasie, która pochodzi od: <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Poniżej przedstawiono przykładową implementację programu `CookieAuthenticationEvents`:

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

Zarejestruj wystąpienie zdarzeń podczas rejestracji usługi plików cookie w `Startup.ConfigureServices` metodzie. Podaj [rejestrację usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes) dla swojej `CustomCookieAuthenticationEvents` klasy:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Rozważ sytuację, w której nazwa użytkownika jest aktualizowana&mdash;decyzją, która nie ma wpływu na zabezpieczenia w jakikolwiek sposób. Jeśli chcesz nieszkodliwej aktualizacji podmiotu zabezpieczeń, wywołaj `context.ReplacePrincipal` i `context.ShouldRenew` ustaw właściwość na `true`.

> [!WARNING]
> Opisane tutaj podejście jest wyzwalane dla każdego żądania. Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników w każdym żądaniu może spowodować spadek wydajności aplikacji.

## <a name="persistent-cookies"></a>Trwałe pliki cookie

Plik cookie może być trwały między sesjami przeglądarki. Tę trwałość należy włączyć tylko w przypadku jawnej zgody użytkownika przy użyciu pola wyboru "Zapamiętaj mnie" przy logowaniu lub podobnym mechanizmie. 

Poniższy fragment kodu tworzy tożsamość i odpowiadający jej plik cookie, który przeżyje przez zamknięcia przeglądarki. Wszystkie skonfigurowane wcześniej ustawienia wygasania są honorowane. Jeśli plik cookie wygaśnie, gdy przeglądarka zostanie ZAMKNIĘTA, przeglądarka czyści plik cookie po jego ponownym uruchomieniu.

Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> na `true` wartość w<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

## <a name="absolute-cookie-expiration"></a>Bezwzględne wygaśnięcie pliku cookie

Bezwzględny czas wygaśnięcia można ustawić za <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>pomocą. Aby utworzyć trwały plik cookie `IsPersistent` , należy również ustawić opcję. W przeciwnym razie plik cookie jest tworzony przy użyciu okresu istnienia sesji i może wygasnąć przed lub po posiadanym przez niego biletem uwierzytelniania. Gdy `ExpiresUtc` jest ustawiona, zastępuje wartość <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opcji <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, jeśli jest ustawiona.

Poniższy fragment kodu tworzy tożsamość i odpowiedni plik cookie, który trwa przez 20 minut. Ignoruje to wszystkie ustawienia wygasania, które zostały wcześniej skonfigurowane.

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core Identity to kompletny, w pełni funkcjonalny Dostawca uwierzytelniania do tworzenia i obsługiwania logowań. Jednak można użyć dostawcy uwierzytelniania uwierzytelniania opartego na plikach cookie bez tożsamości ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

W celach demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika, Maria Rodriguez jest stałe do aplikacji. Zaloguj się przy użyciu `maria.rodriguez@contoso.com` adresu e-mail i hasła. Użytkownik jest uwierzytelniany w `AuthenticateUser` metodzie w pliku *Pages/Account/Login. cshtml. cs* . W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.

## <a name="configuration"></a>Konfiguracja

Jeśli aplikacja nie korzysta z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla pakietu [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .

W metodzie Utwórz usługę pośredniczącą uwierzytelniania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> przy użyciu metod i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>: `Startup.ConfigureServices`

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>przeszedł `AddAuthentication` do ustawia domyślny schemat uwierzytelniania dla aplikacji. `AuthenticationScheme`jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzować z określonym schematem](xref:security/authorization/limitingidentitybyscheme). Ustawienie parametru na CookieAuthenticationDefaults. AuthenticationScheme zapewnia wartość "cookies" dla schematu. [](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) `AuthenticationScheme` Można podać dowolną wartość ciągu odróżniającą schemat.

Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania plików cookie aplikacji. Gdy schemat uwierzytelniania plików cookie nie jest dostarczany <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa `CookieAuthenticationDefaults.AuthenticationScheme` go ("pliki cookie").

<xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> Właściwość pliku cookie uwierzytelniania jest domyślnie ustawiona na `true` wartość. Pliki cookie uwierzytelniania są dozwolone, gdy osoby odwiedzające witrynę nie wyraziły zgody na zbieranie danych. Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.

W metodzie `UseAuthentication` Wywołaj metodę, aby wywołać `HttpContext.User` oprogramowanie pośredniczące uwierzytelniania, które ustawia właściwość. `Startup.Configure` Wywołaj `UseMvcWithDefaultRoute` metodęprzedwywołaniemlub:`UseMvc` `UseAuthentication`

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Klasa służy do konfigurowania opcji dostawcy uwierzytelniania.

Ustaw `CookieAuthenticationOptions` w konfiguracji usługi na potrzeby uwierzytelniania `Startup.ConfigureServices` w metodzie:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Oprogramowanie pośredniczące zasad dotyczących plików cookie

[Oprogramowanie pośredniczące zasad plików cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) umożliwia korzystanie z funkcji zasad dotyczących plików cookie. Dodanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest zgodne z&mdash;kolejnością, ma wpływ tylko na składniki podrzędne zarejestrowane w potoku.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Zastosowanie <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> do oprogramowania pośredniczącego zasad dotyczących plików cookie pozwala kontrolować globalne właściwości przetwarzania plików cookie i przełączać się do programów obsługi przetwarzania plików cookie, gdy pliki cookie są dodawane lub usuwane.

Wartością domyślną <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> jest `SameSiteMode.Lax` umożliwienie uwierzytelniania OAuth2. Aby ściśle wymuszać zasady dotyczące `SameSiteMode.Strict`tej samej lokacji, należy `MinimumSameSitePolicy`ustawić. Mimo że to ustawienie powoduje przerwanie OAuth2 i innych schematów uwierzytelniania między źródłami, podnosi poziom bezpieczeństwa plików cookie dla innych typów aplikacji, które nie polegają na przetwarzaniu żądań między źródłami.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Ustawienie ustawienia pośredniczące zasad dotyczących plików `MinimumSameSitePolicy` cookie dla programu może mieć `Cookie.SameSite` wpływ `CookieAuthenticationOptions` na ustawienia w ustawieniach zgodnie z poniższą macierzą.

| MinimumSameSitePolicy | Cookie.SameSite | Wynikowy plik cookie. SameSite ustawienie |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Utwórz plik cookie uwierzytelniania

Aby utworzyć plik cookie przechowujący informacje o użytkowniku <xref:System.Security.Claims.ClaimsPrincipal>, konstrukcja a. Informacje o użytkowniku są serializowane i przechowywane w pliku cookie. 

Utwórz z <xref:System.Security.Claims.ClaimsIdentity> dowolnym wymaganym <xref:System.Security.Claims.Claim>elementem s i <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> Wywołaj, aby zalogować użytkownika:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync`tworzy zaszyfrowany plik cookie i dodaje go do bieżącej odpowiedzi. Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.

System [ochrony danych](xref:security/data-protection/using-data-protection) ASP.NET Core jest używany do szyfrowania. W przypadku aplikacji hostowanej na wielu komputerach, równoważenia obciążenia między aplikacjami lub korzystania z kolektywu serwerów sieci Web należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) tak, aby używała tego samego dzwonka klucza i identyfikatora aplikacji.

## <a name="sign-out"></a>Wyloguj

Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "cookie") nie jest używany jako schemat (na przykład "ContosoCookie"), należy podać schemat używany podczas konfigurowania dostawcy uwierzytelniania. W przeciwnym razie używany jest domyślny schemat.

## <a name="react-to-back-end-changes"></a>Reagowanie na zmiany zaplecza

Po utworzeniu pliku cookie plik cookie jest pojedynczym źródłem tożsamości. Jeśli konto użytkownika jest wyłączone w systemach zaplecza:

* System uwierzytelniania plików cookie aplikacji kontynuuje przetwarzanie żądań na podstawie pliku cookie uwierzytelniania.
* Użytkownik pozostaje zalogowany do aplikacji, o ile plik cookie uwierzytelniania jest prawidłowy.

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Zdarzenia można użyć do przechwycenia i zastąpienia weryfikacji tożsamości pliku cookie. Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko odwołujący się użytkowników, którzy uzyskują dostęp do aplikacji.

Jedno z metod weryfikacji plików cookie polega na śledzeniu zmian w bazie danych użytkownika. Jeśli baza danych nie została zmieniona od czasu wystawienia pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli plik cookie jest nadal ważny. W przykładowej aplikacji baza danych jest zaimplementowana `IUserRepository` w i `LastChanged` przechowuje wartość. Gdy użytkownik zostanie zaktualizowany w bazie danych, `LastChanged` wartość jest ustawiana na bieżącą godzinę.

Aby unieważnić plik cookie, gdy baza danych zmienia się na podstawie `LastChanged` wartości, Utwórz plik cookie `LastChanged` z zastrzeżeniem zawierającym bieżącą `LastChanged` wartość z bazy danych:

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

Aby zaimplementować przesłonięcie dla `ValidatePrincipal` zdarzenia, napisz metodę o następującym podpisie w klasie, która pochodzi od: <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Poniżej przedstawiono przykładową implementację programu `CookieAuthenticationEvents`:

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

Zarejestruj wystąpienie zdarzeń podczas rejestracji usługi plików cookie w `Startup.ConfigureServices` metodzie. Podaj [rejestrację usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes) dla swojej `CustomCookieAuthenticationEvents` klasy:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Rozważ sytuację, w której nazwa użytkownika jest aktualizowana&mdash;decyzją, która nie ma wpływu na zabezpieczenia w jakikolwiek sposób. Jeśli chcesz nieszkodliwej aktualizacji podmiotu zabezpieczeń, wywołaj `context.ReplacePrincipal` i `context.ShouldRenew` ustaw właściwość na `true`.

> [!WARNING]
> Opisane tutaj podejście jest wyzwalane dla każdego żądania. Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników w każdym żądaniu może spowodować spadek wydajności aplikacji.

## <a name="persistent-cookies"></a>Trwałe pliki cookie

Plik cookie może być trwały między sesjami przeglądarki. Tę trwałość należy włączyć tylko w przypadku jawnej zgody użytkownika przy użyciu pola wyboru "Zapamiętaj mnie" przy logowaniu lub podobnym mechanizmie. 

Poniższy fragment kodu tworzy tożsamość i odpowiadający jej plik cookie, który przeżyje przez zamknięcia przeglądarki. Wszystkie skonfigurowane wcześniej ustawienia wygasania są honorowane. Jeśli plik cookie wygaśnie, gdy przeglądarka zostanie ZAMKNIĘTA, przeglądarka czyści plik cookie po jego ponownym uruchomieniu.

Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> na `true` wartość w<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

## <a name="absolute-cookie-expiration"></a>Bezwzględne wygaśnięcie pliku cookie

Bezwzględny czas wygaśnięcia można ustawić za <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>pomocą. Aby utworzyć trwały plik cookie `IsPersistent` , należy również ustawić opcję. W przeciwnym razie plik cookie jest tworzony przy użyciu okresu istnienia sesji i może wygasnąć przed lub po posiadanym przez niego biletem uwierzytelniania. Gdy `ExpiresUtc` jest ustawiona, zastępuje wartość <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opcji <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, jeśli jest ustawiona.

Poniższy fragment kodu tworzy tożsamość i odpowiedni plik cookie, który trwa przez 20 minut. Ignoruje to wszystkie ustawienia wygasania, które zostały wcześniej skonfigurowane.

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

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Kontrole ról oparte na zasadach](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
