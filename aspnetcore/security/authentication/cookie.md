---
title: Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity
author: rick-anderson
description: Wyjaśnienie przy użyciu uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: f3e02b357a83cf5fc4b9fcdc79b2fbe80da98507
ms.sourcegitcommit: 9691b742134563b662948b0ed63f54ef7186801e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2019
ms.locfileid: "66824753"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)

Jak przedstawiono w wcześniejszych tematach uwierzytelniania [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jest dostawcy uwierzytelniania pełne, kompleksowe za utworzenie i utrzymywanie logowania. Można jednak logika uwierzytelniania niestandardowego za pomocą na podstawie plików cookie uwierzytelniania w czasie. Uwierzytelnianie oparte na plikach cookie służy jako dostawcy uwierzytelniania autonomicznych, bez tożsamości platformy ASP.NET Core.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Dla celów demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika Maria Rodriguez jest zapisane na stałe do aplikacji. Użyj nazwy użytkownika wiadomości E-mail "maria.rodriguez@contoso.com" i wszystkie hasła do logowania użytkownika. Użytkownik jest uwierzytelniany w `AuthenticateUser` method in Class metoda *Pages/Account/Login.cshtml.cs* pliku. Przykład rzeczywistych użytkownika może być uwierzytelniani względem bazy danych.

Instrukcje dotyczące migrowania uwierzytelniania na podstawie plików cookie z platformą ASP.NET Core 1.x do 2.0, zobacz [migracji uwierzytelnianie i tożsamość do tematu platformy ASP.NET Core 2.0 (uwierzytelnianie na podstawie plików Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

Aby użyć tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity) tematu.

## <a name="configuration"></a>Konfiguracja

::: moniker range=">= aspnetcore-2.0"

Jeśli aplikacja nie używa [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakiet (wersja 2.1.0 lub później).

W `ConfigureServices` metody tworzenia usługi oprogramowania pośredniczącego uwierzytelniania za pomocą `AddAuthentication` i `AddCookie` metody:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AuthenticationScheme` przekazany do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji. `AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień pliku cookie uwierzytelniania, a użytkownik chce [autoryzacji przy użyciu określonego schematu](xref:security/authorization/limitingidentitybyscheme). Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" dla schematu. Możesz podać dowolną wartość ciągu, która odróżnia systemu.

Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania pliku cookie aplikacji. Gdy nie jest udostępniane schematu uwierzytelniania pliku cookie <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa ona [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("plików cookie" ").

Plik cookie uwierzytelniania <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> właściwość jest ustawiona na `true` domyślnie. Pliki cookie uwierzytelniania są dozwolone, gdy użytkownik nie wyraził zgodę, do zbierania danych. Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.

W `Configure` metody, użyj `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości. Wywołaj `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

**AddCookie Options**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.

| Opcja | Opis |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania) po wyzwoleniu przez `HttpContext.ForbidAsync`. Wartość domyślna to `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość żadnych oświadczeń, utworzone przez usługę uwierzytelniania pliku cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Nazwa domeny, w którym plik cookie jest obsługiwany. Domyślnie jest to nazwa hosta żądania. Przeglądarka wysyła tylko pliki cookie w żądaniach do takiej samej nazwie hosta. Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie. Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia je zadaniom `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów. Zmiana tej wartości do `false` zezwala na skrypty po stronie klienta do dostępu do pliku cookie i mogą otwierać aplikację, aby kradzieży plików cookie, powinny mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luk w zabezpieczeniach. Wartość domyślna to `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Określa nazwę pliku cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Używane do izolowania aplikacji uruchomionych na tej samej nazwy hosta. Jeśli masz aplikację, w którym uruchomiono `/app1` i ograniczenie liczby plików cookie do tej aplikacji, ustaw `CookiePath` właściwość `/app1`. W ten sposób plik cookie jest dostępny tylko dla żądań `/app1` oraz dowolnej aplikacji znajdujących się pod nim. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Wskazuje, czy przeglądarka powinna zezwalać pliku cookie do podłączenia do tej samej lokacji żądania tylko (`SameSiteMode.Strict`) lub żądań między witrynami przy użyciu bezpiecznych metod HTTP i tej samej lokacji żądania (`SameSiteMode.Lax`). Po ustawieniu `SameSiteMode.None`, nie jest ustawiona wartość nagłówka pliku cookie. Należy pamiętać, że [oprogramowaniu pośredniczącym pliku Cookie zasad](#cookie-policy-middleware) może zastąpić tę wartość, przez Ciebie. Aby zapewnić obsługę uwierzytelniania OAuth, wartość domyślna to `SameSiteMode.Lax`. Aby uzyskać więcej informacji, zobacz [uwierzytelniania OAuth, przerwane z powodu zasad plików cookie SameSite](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Flaga wskazująca, jeśli utworzony plik cookie powinien być ograniczony do protokołu HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`). Wartość domyślna to `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Zestawy `DataProtectionProvider` umożliwiający tworzenie domyślnie `TicketDataFormat`. Jeśli `TicketDataFormat` ustawiono właściwość `DataProtectionProvider` nie jest używana opcja. Jeśli nie zostanie podany, domyślny dostawca ochrony danych aplikacji jest używany. |
| [Zdarzenia](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | Program obsługi wyjątku wywołuje metody dla dostawcy, które zapewniają kontroli aplikacji w pewnych punktach, przetwarzanie. Jeśli `Events` nie są podane wystąpienie domyślne jest podany, które nie działają, gdy metody są wywoływane. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Używane jako typ usługi, aby uzyskać `Events` wystąpieniem, a nie właściwości. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania przechowywany w pliku cookie. `ExpireTimeSpan` jest dodawany do bieżącego czasu, aby utworzyć czas wygaśnięcia biletu. `ExpiredTimeSpan` Wartość zawsze przechodzi w stan AuthTicket zaszyfrowane, zweryfikowane przez serwer. Również może przejść do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) nagłówka, ale tylko wtedy, gdy `IsPersistent` jest ustawiona. Aby ustawić `IsPersistent` do `true`, skonfiguruj [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) przekazany do `SignInAsync`. Wartość domyślna `ExpireTimeSpan` to 14 dni. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania) po wyzwoleniu przez `HttpContext.ChallengeAsync`. Bieżący adres URL, który wygenerował 401 jest dodawany do `LoginPath` jako o nazwie określonej przez parametr ciągu zapytania `ReturnUrlParameter`. Gdy żądanie `LoginPath` nadaje nową tożsamość logowania, `ReturnUrlParameter` wartość jest używana do przekierowania wyszukiwarki do adresu URL, który spowodował oryginalnego kodu stanu nieautoryzowanego. Wartość domyślna to `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Jeśli `LogoutPath` znajduje się do programu obsługi, a następnie przekierowuje żądanie do tej ścieżki na podstawie wartości `ReturnUrlParameter`. Wartość domyślna to `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Określa nazwę parametru ciągu zapytania, która jest dołączana przez program obsługi odpowiedzi 302 Found (adres URL przekierowania). `ReturnUrlParameter` jest używany, gdy żądanie dociera `LoginPath` lub `LogoutPath` aby powrócić do oryginalnego adresu URL w przeglądarce, po wykonaniu akcji logowania lub wylogowywania. Wartość domyślna to `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Opcjonalny kontener do przechowywania tożsamości na żądań. W przypadku użycia, identyfikator sesji jest wysyłany do klienta. `SessionStore` można wyeliminować potencjalne problemy z tożsamościami dużych. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Flaga wskazująca, czy nowy plik cookie z czasem wygaśnięcia zaktualizowane powinny być wydawane dynamicznie. Można to zrobić na każde żądanie, gdy bieżący okres ważności pliku cookie więcej niż 50% wygasł. Nowa data ważności jest przenoszony do przodu do bieżącej daty plus `ExpireTimespan`. [Czas wygaśnięcia bezwzględną pliku cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić za pomocą `AuthenticationProperties` klasy podczas wywoływania `SignInAsync`. Czas wygaśnięcia bezwzględny może zwiększyć zabezpieczenia aplikacji, ograniczając ilość czasu, który plik cookie uwierzytelniania jest prawidłowy. Wartość domyślna to `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat` Służy do włączania i wyłączania ochrony tożsamości i innych właściwości, które są przechowywane w wartościach plików cookie. Jeśli nie zostanie podana, `TicketDataFormat` jest tworzony przy użyciu [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Sprawdzanie poprawności](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Metoda, która sprawdza, czy opcje są prawidłowe. |

Ustaw `CookieAuthenticationOptions` w konfiguracji usługi uwierzytelniania `ConfigureServices` metody:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.x korzysta z plików cookie [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) serializujący głównej nazwy użytkownika w zaszyfrowanym pliku cookie. Kolejne żądania plik cookie jest weryfikowana, a podmiot zabezpieczeń jest odtwarzane i przypisane do `HttpContext.User` właściwości.

Zainstaluj [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu NuGet w projekcie. Ten pakiet zawiera oprogramowaniu pośredniczącym pliku cookie.

Użyj `UseCookieAuthentication` method in Class metoda `Configure` method in Class metoda swoje *Startup.cs* pliku przed `UseMvc` lub `UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**CookieAuthenticationOptions Options**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.

| Opcja | Opis |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Ustawia schematu uwierzytelniania. `AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania, a użytkownik chce [autoryzacji przy użyciu określonego schematu](xref:security/authorization/limitingidentitybyscheme). Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" dla schematu. Możesz podać dowolną wartość ciągu, która odróżnia systemu. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Ustawia wartość, aby wskazać, że uwierzytelniania plików cookie na każde żądanie i uruchamiania próba weryfikacji i ponownie skonstruuj Zserializowany podmiot zabezpieczeń, utworzone przez siebie. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | W przypadku opcji true oprogramowania pośredniczącego uwierzytelniania obsługuje automatyczne wyzwania. Jeśli ma wartość FAŁSZ, oprogramowanie pośredniczące uwierzytelniania zmienia tylko odpowiedzi, gdy jest to wyraźnie wskazane przez `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość żadnych oświadczeń, utworzone przez oprogramowanie pośredniczące uwierzytelniania plików cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Nazwa domeny, w którym plik cookie jest obsługiwany. Domyślnie jest to nazwa hosta żądania. Przeglądarka służy tylko pliku cookie do takiej samej nazwie hosta. Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie. Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia je zadaniom `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów. Zmiana tej wartości do `false` zezwala na skrypty po stronie klienta do dostępu do pliku cookie i mogą otwierać aplikację, aby kradzieży plików cookie, powinny mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luk w zabezpieczeniach. Wartość domyślna to `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Używane do izolowania aplikacji uruchomionych na tej samej nazwy hosta. Jeśli masz aplikację, w którym uruchomiono `/app1` i ograniczenie liczby plików cookie do tej aplikacji, ustaw `CookiePath` właściwość `/app1`. W ten sposób plik cookie jest dostępny tylko dla żądań `/app1` oraz dowolnej aplikacji znajdujących się pod nim. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Flaga wskazująca, jeśli utworzony plik cookie powinien być ograniczony do protokołu HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`). Wartość domyślna to `CookieSecurePolicy.SameAsRequest`. |
| [Opis](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Dodatkowe informacje o typie uwierzytelniania, który ma zostać udostępnione do aplikacji. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania. Jest dodawany do bieżącego czasu, aby utworzyć czas wygaśnięcia biletu. Aby użyć `ExpireTimeSpan`, należy ustawić `IsPersistent` do `true` w `AuthenticationProperties` przekazany do `SignInAsync`. Wartość domyślna to 14 dni. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Flaga wskazująca, czy data wygaśnięcia pliku cookie resetuje, gdy więcej niż połowy `ExpireTimeSpan` upływie interwału. Nową wartość czasu wygaśnięcia jest przenoszony do przodu do bieżącej daty plus `ExpireTimespan`. [Czas wygaśnięcia bezwzględną pliku cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić za pomocą `AuthenticationProperties` klasy podczas wywoływania `SignInAsync`. Czas wygaśnięcia bezwzględny może zwiększyć zabezpieczenia aplikacji, ograniczając ilość czasu, który plik cookie uwierzytelniania jest prawidłowy. Wartość domyślna to `true`. |

Ustaw `CookieAuthenticationOptions` dla oprogramowania pośredniczącego uwierzytelniania pliku Cookie w `Configure` metody:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a>Oprogramowaniu pośredniczącym pliku cookie zasad

[Oprogramowaniu pośredniczącym pliku cookie zasad](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) oferuje możliwości zasad plików cookie w aplikacji. Dodawanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest kolejność liter; dotyczy tylko składniki zarejestrowane po nim w potoku.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) dostarczane z oprogramowaniem pośredniczącym pliku Cookie zasady umożliwiają kontrolowanie globalne właściwości przetwarzania plików cookie oraz podłączania do obsługi przetwarzania plików cookie, pliki cookie są dołączane lub usunięciu.

| Właściwość | Opis |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Ma wpływ na, czy pliki cookie musi być HttpOnly, który jest Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów. Wartość domyślna to `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Wpływa na atrybut o tej samej lokacji plik cookie (patrz poniżej). Wartość domyślna to `SameSiteMode.Lax`. Ta opcja jest dostępna dla platformy ASP.NET Core 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Wywołuje się, gdy jest dołączany plik cookie. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Wywołuje się, gdy plik cookie zostanie usunięty. |
| [Bezpieczeństwo](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Wpływa na pliki cookie musi być bezpieczny. Wartość domyślna to `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)

Wartość domyślna `MinimumSameSitePolicy` wartość `SameSiteMode.Lax` pozwalające uwierzytelniania OAuth2. Ściśle wymusić zasady tej samej lokacji `SameSiteMode.Strict`ustaw `MinimumSameSitePolicy`. Mimo że to ustawienie dzieli OAuth2 i innych schematów uwierzytelniania między źródłami, eksponuje poziom bezpieczeństwa pliku cookie dla innych typów aplikacji, które nie należy polegać na przetwarzanie żądań cross-origin.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Ustawienie oprogramowaniu pośredniczącym pliku Cookie zasad `MinimumSameSitePolicy` mogą mieć wpływ na Twoje ustawienia `Cookie.SameSite` w `CookieAuthenticationOptions` ustawienia zgodnie z poniższym macierzy.

| MinimumSameSitePolicy | Cookie.SameSite | Wynikowe ustawienie Cookie.SameSite |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Tworzenie pliku cookie uwierzytelniania

Aby utworzyć plik cookie zawierający informacje o użytkowniku, należy tworzyć [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal). Informacje o użytkowniku są serializowane i przechowywane w pliku cookie. 

::: moniker range=">= aspnetcore-2.0"

Tworzenie [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) z każdego wymaganego [oświadczenia](/dotnet/api/system.security.claims.claim)s, a następnie wywołać [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) do logowania użytkownika:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Wywołaj [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) do logowania użytkownika:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

`SignInAsync` Tworzy zaszyfrowanego pliku cookie i dodaje go do bieżącej odpowiedzi. Jeśli nie określisz `AuthenticationScheme`, używany jest domyślny schemat.

Dzieje się w tle, szyfrowanie, używana jest platforma ASP.NET Core [ochrony danych](xref:security/data-protection/using-data-protection) systemu. Jeśli hostujesz aplikację na wielu maszynach, równoważenia obciążenia w aplikacjach lub przy użyciu farmy sieci web, a następnie należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) korzystać z tej samej pierścień klucz i identyfikator aplikacji.

## <a name="sign-out"></a>Wyloguj

::: moniker range=">= aspnetcore-2.0"

Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

Jeśli nie używasz `CookieAuthenticationDefaults.AuthenticationScheme` (lub "Plików cookie" ") jako systemu (na przykład" ContosoCookie"), podaj Schemat używany podczas konfigurowania dostawcy uwierzytelniania. W przeciwnym razie używany jest domyślny schemat.

## <a name="react-to-back-end-changes"></a>Reagowanie na zmiany zaplecza

Po utworzeniu pliku cookie staje się pojedynczym źródłem tożsamości. Nawet jeśli użytkownik zostanie wyłączone w systemach zaplecza, system plików cookie uwierzytelniania nie ma informacji o tym, a użytkownik pozostaje zalogowany, tak długo, jak ich plik cookie jest prawidłowy.

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) zdarzeń w programie ASP.NET Core 2.x lub [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metodę w programie ASP.NET Core 1.x może służyć do przechwycenia, a następnie zastąpić weryfikacji tożsamości pliku cookie. Takie podejście zmniejsza ryzyko odwołanych użytkownikom dostęp do aplikacji.

Jedno z podejść do weryfikacji opiera się na rejestrowanie informacji o bazie danych użytkownika po zmianie. Jeśli baza danych nie zostały zmodyfikowane, ponieważ został wystawiony przez użytkownika w pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli ich plik cookie jest nadal ważny. Aby zrealizować ten scenariusz, w bazie danych, która jest zaimplementowana w `IUserRepository` w tym przykładzie przechowuje `LastChanged` wartość. Po zaktualizowaniu dowolnego użytkownika w bazie danych `LastChanged` wartość jest ustawiona na bieżącą godzinę.

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

::: moniker range=">= aspnetcore-2.0"

Aby zaimplementować zastąpienie `ValidatePrincipal` zdarzeń, napisz metody z następującą sygnaturą w klasie, która pochodzi z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Przykład wygląda podobnie do poniższego:

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

Rejestrowanie wystąpienia zdarzenia podczas rejestracji usługa plików cookie w `ConfigureServices` metody. Podaj rejestracji usługi o określonym zakresie z `CustomCookieAuthenticationEvents` klasy:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aby zaimplementować zastąpienie `ValidateAsync` zdarzeń, napisz metody z następującą sygnaturą:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

Tożsamości platformy ASP.NET Core to sprawdzenie są implementowane jako część jej [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Przykład wygląda podobnie do poniższego:

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Rejestrowanie zdarzeń podczas konfigurowania uwierzytelniania plików cookie w `Configure` metody:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

Rozważmy sytuację, w którym nazwa użytkownika jest aktualizowana &mdash; decyzji, który nie ma wpływu na zabezpieczenia w dowolny sposób. Jeśli chcesz zaktualizować bezpieczny głównej nazwy użytkownika, wywołaj `context.ReplacePrincipal` i ustaw `context.ShouldRenew` właściwość `true`.

> [!WARNING]
> W sposób opisany poniżej są wyzwalane na każde żądanie. Może to spowodować spadek wydajności w aplikacji.

## <a name="persistent-cookies"></a>Trwałe pliki cookie

Możesz chcieć plików cookie, aby zachować w wielu sesjach przeglądarki. Ten stan trwały można włączyć tylko za zgodą użytkownika z polem wyboru "Pamiętaj mnie" logowania lub podobny mechanizm. 

Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który przeżyje za pośrednictwem przeglądarki zamknięcia. Ustawienia wygaśnięcia przewijania wcześniej skonfigurowane są uznawane. Jeśli plik cookie wygasa, gdy nastąpi zamknięcie okna przeglądarki, przeglądarki usuwa plik cookie po ponownym uruchomieniu.

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) klasa znajduje się w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) klasa znajduje się w `Microsoft.AspNetCore.Http.Authentication` przestrzeni nazw.

::: moniker-end

## <a name="absolute-cookie-expiration"></a>Datę ważności pliku cookie bezwzględne

Można ustawić czasu wygaśnięcia bezwzględne za pomocą `ExpiresUtc`. Aby utworzyć trwały plik cookie, należy także ustawić `IsPersistent`; w przeciwnym wypadku plik cookie jest tworzony o okresie istnienia, oparte na sesji i może wygaśnie przed lub po biletu uwierzytelniania, który przechowuje. Gdy `ExpiresUtc` jest ustawiona na `SignInAsync`, zastępuje ona wartość `ExpireTimeSpan` opcji `CookieAuthenticationOptions`, jeśli ustawiona.

Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który jest ważny przez 20 minut. To ignoruje wszelkie przewijania wcześniej skonfigurowane ustawienia wygaśnięcia.

::: moniker range=">= aspnetcore-2.0"

```csharp
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

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
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

* [Zmiany uwierzytelniania w wersji 2.0 / migracji ogłoszenia](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Rola oparta na zasadach kontroli](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
