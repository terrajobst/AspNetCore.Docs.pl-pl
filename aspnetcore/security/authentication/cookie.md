---
title: "Przy użyciu uwierzytelniania plików Cookie bez tożsamości platformy ASP.NET Core"
author: rick-anderson
description: "Wyjaśnienie przy użyciu uwierzytelniania plików cookie bez ASP.NET Core Identity"
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: bbc49a0d3ede66ad07ec3f1dea055cae5fec39ff
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Przy użyciu uwierzytelniania plików Cookie bez tożsamości platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)

Jak przedstawiono w starszych tematy dotyczące uwierzytelniania [ASP.NET Core Identity](xref:security/authentication/identity) jest dostawcą uwierzytelniania pełną, kompletne za tworzenie i obsługę logowania. Można jednak logiki niestandardowe uwierzytelnianie za pomocą pliku cookie uwierzytelniania w czasie. Jako autonomiczny dostawca uwierzytelniania bez ASP.NET Core Identity, można użyć uwierzytelniania opartego na pliku cookie.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Aby uzyskać informacje dotyczące migrowania uwierzytelniania opartego na pliku cookie z platformy ASP.NET Core 1.x 2.0, zobacz [Migrowanie uwierzytelnianie i tożsamość do tematu ASP.NET Core 2.0 (uwierzytelnianie na podstawie plików Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

## <a name="configuration"></a>Konfiguracja

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Jeśli nie używasz [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), zainstaluj w wersji 2.0 + [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu NuGet.

W `ConfigureServices` metody, Utwórz usługę oprogramowania pośredniczącego uwierzytelniania za pomocą `AddAuthentication` i `AddCookie` metod:

[!code-csharp[](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme` przekazany do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji. `AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzacji z określonym schematem](xref:security/authorization/limitingidentitybyscheme). Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" schematu. Możesz podać dowolną wartość ciągu, która odróżnia schemat.

W `Configure` metody, użyj `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości. Wywołanie `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:

[!code-csharp[](cookie/sample/Startup.cs?name=snippet2)]

**Opcje AddCookie**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.

| Opcja | Opis |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania), gdy wyzwalana przez `HttpContext.ForbidAsync`. Wartość domyślna to `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość wszelkie oświadczenia utworzone przez usługę uwierzytelniania pliku cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Nazwa domeny, w której plik cookie jest obsługiwana. Domyślnie jest to nazwa hosta z żądania. Przeglądarka wysyła tylko plik cookie w żądaniach pasujące nazwy hosta. Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie. Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia do `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | Pobiera lub ustawia informacje o czasie pliku cookie. Obecnie to opcja ops nie i staną się nieaktualne w ASP.NET Core 2.1 +. Użyj `ExpireTimeSpan` opcję, aby określić datę ważności pliku cookie. Aby uzyskać więcej informacji, zobacz [wyjaśnić zachowanie CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293). |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Flaga wskazująca, czy plik cookie powinien być dostępny tylko na serwerach. Zmiana tej wartości do `false` zezwala na dostęp do pliku cookie skryptów po stronie klienta i może otworzyć aplikację, aby kradzieży plików cookie powinien mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luki w zabezpieczeniach. Wartość domyślna to `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Ustawia nazwę pliku cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Używane do izolowania aplikacji działających na takiej samej nazwie hosta. Jeśli masz aplikację, w którym uruchomiono `/app1` i chcesz ograniczyć plików cookie do tej aplikacji, ustaw `CookiePath` właściwości `/app1`. W ten sposób plik cookie jest dostępny tylko dla żądań wysyłanych na `/app1` i wszystkich aplikacji znajdujących się pod nim. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Wskazuje, czy przeglądarka powinna zezwalać plik cookie jest dołączony do tej samej lokacji tylko żądania (`SameSiteMode.Strict`) lub przy użyciu tej samej lokacji żądań i bezpieczne metody HTTP żądania między witrynami (`SameSiteMode.Lax`). Jeśli wartość `SameSiteMode.None`, nie jest ustawiona wartość nagłówka pliku cookie. Należy pamiętać, że [oprogramowaniu pośredniczącym pliku Cookie zasad](#cookie-policy-middleware) może zastąpić wartość, którą należy podać. Aby zapewnić obsługę uwierzytelniania OAuth, wartością domyślną jest `SameSiteMode.Lax`. Aby uzyskać więcej informacji, zobacz [uwierzytelniania OAuth przerwane z powodu zasad pliku cookie SameSite](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Flaga wskazująca, czy utworzony plik cookie powinien być ograniczony do HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`). Wartość domyślna to `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Ustawia `DataProtectionProvider` używany do tworzenia domyślnie `TicketDataFormat`. Jeśli `TicketDataFormat` właściwość jest ustawiona, `DataProtectionProvider` nie jest używana opcja. Jeśli nie zostanie podana, jest używany domyślny dostawca ochrony danych aplikacji. |
| [Zdarzenia](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | Program obsługi wywołuje metody dla dostawcy, który mieć kontrolę aplikacji w niektórych punktach przetwarzania. Jeśli `Events` nie są dostarczane podano domyślnego wystąpienia, które nie działają, gdy metody są wywoływane. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Używane jako typ usługi, aby uzyskać `Events` wystąpienia zamiast właściwości. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania przechowywany w pliku cookie. `ExpireTimeSpan` zostanie dodany do bieżącego czasu można utworzyć czas wygaśnięcia biletu. `ExpiredTimeSpan` Wartość zawsze przechodzi w stan AuthTicket zaszyfrowane, zweryfikowane przez serwer. Może ona także przejść do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) nagłówka, ale tylko wtedy, gdy `IsPersistent` jest ustawiona. Aby ustawić `IsPersistent` do `true`, skonfiguruj [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) przekazany do `SignInAsync`. Wartość domyślna `ExpireTimeSpan` 14 dni. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania), gdy wyzwalana przez `HttpContext.ChallengeAsync`. Bieżący adres URL, który wygenerował kod 401, jest dodawany do `LoginPath` jako parametr ciągu zapytania o nazwie `ReturnUrlParameter`. Raz żądanie `LoginPath` nadaje nową tożsamość logowania, `ReturnUrlParameter` wartość jest używana do przekierowania przeglądarki z powrotem do adresu URL, który spowodował oryginalnego kodu stanu nieautoryzowanego. Wartość domyślna to `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Jeśli `LogoutPath` podano do programu obsługi, a następnie przekierowuje żądanie do tej ścieżki na podstawie wartości z `ReturnUrlParameter`. Wartość domyślna to `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Określa nazwę parametru ciągu zapytania, która jest dołączana przez procedurę obsługi dla odpowiedzi 302 Found (adres URL przekierowania). `ReturnUrlParameter` jest używany, gdy żądanie dociera na `LoginPath` lub `LogoutPath` aby wrócić do oryginalnego adresu URL w przeglądarce, po wykonaniu akcji logowanie lub wylogowanie. Wartość domyślna to `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Opcjonalny kontener używany do przechowywania tożsamości dla żądań. W przypadku identyfikatora sesji są wysyłane do klienta. `SessionStore` można wyeliminować potencjalne problemy z tożsamościami duże. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Flaga wskazująca, czy nowy plik cookie z czasem wygaśnięcia zaktualizowane powinien zostać wystawiony dynamicznie. Może to się zdarzyć na każde żądanie, w którym bieżącego okresu wygaśnięcia pliku cookie jest więcej niż 50% wygasł. Nowa data wygaśnięcia została przeniesiona do przodu do była bieżąca data i `ExpireTimespan`. [Czas wygaśnięcia bezwzględne cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić przy użyciu `AuthenticationProperties` klasy podczas wywoływania metody `SignInAsync`. Czas wygaśnięcia bezwzględne zwiększyć bezpieczeństwo aplikacji poprzez ograniczenie ilość czasu, który jest prawidłowy plik cookie uwierzytelniania. Wartość domyślna to `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat` Służy do ustawiania i usuwania ochrony tożsamości i innych właściwości, które są przechowywane w wartości pliku cookie. Jeśli nie zostanie podana, `TicketDataFormat` jest tworzony przy użyciu [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Sprawdzanie poprawności](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Metoda, która sprawdza, czy ustawienia są prawidłowe. |

Ustaw `CookieAuthenticationOptions` w konfiguracji usługi dla uwierzytelniania w `ConfigureServices` metody:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Plik cookie używa 1.x platformy ASP.NET Core [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) serializujący głównej nazwy użytkownika do zaszyfrowanego pliku cookie. Kolejne żądania plik cookie jest weryfikowana i ponownie utworzyć podmiot zabezpieczeń i ma przypisaną do `HttpContext.User` właściwości.

Zainstaluj [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu NuGet w projekcie. Ten pakiet zawiera oprogramowaniu pośredniczącym pliku cookie.

Użyj `UseCookieAuthentication` metody w `Configure` metody w Twojej *Startup.cs* pliku przed `UseMvc` lub `UseMvcWithDefaultRoute`:

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

**Opcje CookieAuthenticationOptions**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.

| Opcja | Opis |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Ustawia schematu uwierzytelniania. `AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania i chcesz [autoryzacji z określonym schematem](xref:security/authorization/limitingidentitybyscheme). Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" schematu. Możesz podać dowolną wartość ciągu, która odróżnia schemat. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Ustawia wartość oznacza, że uwierzytelniania plików cookie należy uruchomić na każde żądanie i próbę zweryfikowania i rekonstrukcji serializacji podmiot zabezpieczeń, utworzonych. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | Jeśli PRAWDA, oprogramowanie pośredniczące uwierzytelniania obsługuje automatyczne wyzwania. Jeśli ma wartość FAŁSZ, oprogramowanie pośredniczące uwierzytelniania zmienia tylko odpowiedzi, gdy jest to wyraźnie wskazane przez `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość wszelkie oświadczenia utworzone przez oprogramowanie pośredniczące uwierzytelniania plików cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Nazwa domeny, w której plik cookie jest obsługiwana. Domyślnie jest to nazwa hosta z żądania. Przeglądarka służy tylko pliku cookie do dopasowania nazwy hosta. Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie. Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia do `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Flaga wskazująca, czy plik cookie powinien być dostępny tylko na serwerach. Zmiana tej wartości do `false` zezwala na dostęp do pliku cookie skryptów po stronie klienta i może otworzyć aplikację, aby kradzieży plików cookie powinien mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luki w zabezpieczeniach. Wartość domyślna to `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Używane do izolowania aplikacji działających na takiej samej nazwie hosta. Jeśli masz aplikację, w którym uruchomiono `/app1` i chcesz ograniczyć plików cookie do tej aplikacji, ustaw `CookiePath` właściwości `/app1`. W ten sposób plik cookie jest dostępny tylko dla żądań wysyłanych na `/app1` i wszystkich aplikacji znajdujących się pod nim. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Flaga wskazująca, czy utworzony plik cookie powinien być ograniczony do HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`). Wartość domyślna to `CookieSecurePolicy.SameAsRequest`. |
| [Opis](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Dodatkowe informacje o typie uwierzytelniania, który ma zostać udostępnione do aplikacji. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania. Jest ona dodawana do bieżącego czasu, aby utworzyć czas wygaśnięcia biletu. Aby użyć `ExpireTimeSpan`, należy ustawić `IsPersistent` do `true` w `AuthenticationProperties` przekazany do `SignInAsync`. Wartość domyślna to 14 dni. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Flaga wskazująca, czy data wygaśnięcia pliku cookie resetuje kiedy więcej niż połowy `ExpireTimeSpan` upływie interwału. Nowy termin exipiration została przeniesiona do przodu do była bieżąca data i `ExpireTimespan`. [Czas wygaśnięcia bezwzględne cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić przy użyciu `AuthenticationProperties` klasy podczas wywoływania metody `SignInAsync`. Czas wygaśnięcia bezwzględne zwiększyć bezpieczeństwo aplikacji poprzez ograniczenie ilość czasu, który jest prawidłowy plik cookie uwierzytelniania. Wartość domyślna to `true`. |

Ustaw `CookieAuthenticationOptions` dla oprogramowania pośredniczącego uwierzytelniania pliku Cookie w `Configure` metody:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Oprogramowanie pośredniczące zasad pliku cookie

[Oprogramowanie pośredniczące zasad pliku cookie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) włącza funkcje zasad pliku cookie w aplikacji. Dodawanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest kolejność liter; wpływa tylko na składniki zarejestrowane po nim w potoku.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) dostarczony do pliku Cookie oprogramowanie pośredniczące zasady umożliwiają kontrolowanie globalnych właściwości przetwarzania pliku cookie i haku do obsługi przetwarzania pliku cookie, gdy pliki cookie są dołączane lub usuwane.

| Właściwość | Opis |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Dotyczy, czy pliki cookie musi być HttpOnly, który jest flagę wskazującą, jeśli plik cookie powinien być dostępny tylko na serwerach. Wartość domyślna to `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Wpływa na plik cookie tej samej lokacji atrybutu (patrz poniżej). Wartość domyślna to `SameSiteMode.Lax`. Ta opcja jest dostępna dla platformy ASP.NET Core 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Wywoływane, gdy jest dołączany plik cookie. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Wywoływane, gdy plik cookie zostanie usunięty. |
| [Zabezpieczenia](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Wpływa na pliki cookie musi być zabezpieczona. Wartość domyślna to `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)

Wartość domyślna `MinimumSameSitePolicy` wartość jest `SameSiteMode.Lax` umożliwiające uwierzytelniania OAuth2. Ściśle wymuszać zasady tej samej lokacji `SameSiteMode.Strict`ustaw `MinimumSameSitePolicy`. Mimo że to ustawienie dzieli OAuth2 i inne schematy uwierzytelniania między źródłami, eksponuje poziom zabezpieczeń plików cookie dla innych typów aplikacji, które nie należy polegać na przetwarzanie żądań cross-origin.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Ustawienie oprogramowaniu pośredniczącym pliku Cookie zasad `MinimumSameSitePolicy` mogą mieć wpływ na ustawienia z `Cookie.SameSite` w `CookieAuthenticationOptions` ustawienia zgodnie z poniższym macierzy.

| MinimumSameSitePolicy | Cookie.SameSite | Wynikowe ustawienie Cookie.SameSite |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="creating-an-authentication-cookie"></a>Tworzenie pliku cookie uwierzytelniania

Aby utworzyć plik cookie zawierający informacje o użytkownikach, należy tworzyć [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). Informacje o użytkowniku są serializowane i przechowywane w pliku cookie. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Utwórz [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) z każdego wymaganego [oświadczeń](/dotnet/api/system.security.claims.claim)s i wywołanie [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) do logowania użytkownika:

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wywołanie [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) do logowania użytkownika:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync` Tworzy zaszyfrowanego pliku cookie i dodaje go do bieżącej odpowiedzi. Jeśli nie określisz `AuthenticationScheme`, używany jest domyślny schemat.

W obszarze obejmuje szyfrowania używany jest platformy ASP.NET Core [ochrony danych](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) systemu. Jeśli przechowujesz aplikacji na wiele komputerów, równoważenia obciążenia w aplikacjach lub przy użyciu farmy sieci web, a następnie należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) korzystać z tej samej pierścień klucz i identyfikator aplikacji.

## <a name="signing-out"></a>Wylogowywanie

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wyloguj z bieżącego użytkownika i usuń ich pliku cookie, wywołaj [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wyloguj z bieżącego użytkownika i usuń ich pliku cookie, wywołaj [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

Jeśli nie używasz `CookieAuthenticationDefaults.AuthenticationScheme` (lub "Plików cookie") jako systemu (na przykład "ContosoCookie"), podaj Schemat używany podczas konfigurowania dostawcy uwierzytelniania. W przeciwnym razie jest używany domyślny schemat.

## <a name="reacting-to-back-end-changes"></a>Reagowanie na zmiany zaplecza

Po utworzeniu pliku cookie, staje się pojedynczym źródłem tożsamości. Nawet wtedy, gdy użytkownik jest wyłączone w przez systemy zaplecza, system plików cookie uwierzytelniania nie ma informacji o tym, a użytkownik pozostaje zalogowany, tak długo, jak ich plik cookie jest prawidłowy.

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) zdarzenie platformy ASP.NET Core 2.x lub [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metoda platformy ASP.NET Core 1.x, można przechwytywać i zastąpić weryfikacji tożsamości pliku cookie. Takie podejście zmniejsza ryzyko odwołane użytkowników uzyskujących dostęp do aplikacji.

Jeden ze sposobów sprawdzania poprawności pliku cookie jest oparta na rejestrowanie informacji o bazie danych użytkownika po zmianie. Jeśli bazy danych nie został zmodyfikowany od czasu plików cookie użytkownika został wystawiony, jest niepotrzebna ponownego uwierzytelnienia użytkownika, jeśli ich plik cookie jest nadal ważny. Aby zaimplementować bazy danych, która jest zaimplementowana w tym scenariuszu `IUserRepository` w tym przykładzie przechowuje `LastChanged` wartość. Po zaktualizowaniu każdy użytkownik w bazie danych, `LastChanged` wartość jest ustawiona na bieżącą godzinę.

W celu unieważnienia pliku cookie, gdy na podstawie zmian w bazie danych `LastChanged` wartość, Utwórz plik cookie z `LastChanged` oświadczeń, zawierający bieżącą `LastChanged` wartości z bazy danych:

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aby zaimplementować zastąpienie `ValidatePrincipal` zdarzeń, zapisu, a metoda z następującą sygnaturą w klasie, która pochodzi z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Przykład wygląda następująco:

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

Zarejestruj wystąpienie zdarzenia podczas rejestracji usługi plików cookie w `ConfigureServices` metody. Podaj rejestracji usługi w zakresie sieci `CustomCookieAuthenticationEvents` klasy:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aby zaimplementować zastąpienie `ValidateAsync` zdarzeń, zapisu, a metoda z następującą sygnaturą:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core Identity implementuje ten test jako część jej [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Przykład wygląda następująco:

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

Zarejestruj zdarzenie podczas konfigurowania uwierzytelniania plików cookie w `Configure` metody:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Rozważmy sytuację, w którym nazwa użytkownika jest aktualizowana &mdash; decyzja, który nie ma wpływu na zabezpieczenia w dowolny sposób. Jeśli chcesz zaktualizować bezpieczny głównej nazwy użytkownika, należy wywołać `context.ReplacePrincipal` i ustaw `context.ShouldRenew` właściwości `true`.

> [!WARNING]
> W sposób opisany poniżej jest wyzwalane na każde żądanie. Może to spowodować zmniejszenie wydajności dużych aplikacji.

## <a name="persistent-cookies"></a>Trwałe pliki cookie

Możesz pliku cookie do utrwalenia w wielu sesjach przeglądarki. Ta trwałości powinna być włączona tylko za zgodą użytkownika jawne z checkbox "Zapamiętaj moje" na nazwy logowania lub podobny mechanizm. 

Poniższy fragment kodu tworzy tożsamość i odpowiedniego pliku cookie, który przeżyje za pośrednictwem zamknięcia przeglądarki. Ustawienia wygaśnięcia przesuwanego wcześniej skonfigurowane są uznawane. Jeśli plik cookie wygasa, gdy nastąpi zamknięcie okna przeglądarki, przeglądarki usuwa plik cookie po ponownym uruchomieniu.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

---

## <a name="absolute-cookie-expiration"></a>Datę ważności pliku cookie bezwzględne

Można ustawić czas wygaśnięcia bezwzględne z `ExpiresUtc`. Należy także ustawić `IsPersistent`; w przeciwnym razie `ExpiresUtc` jest ignorowany i jest tworzony plik cookie sesji jednego. Podczas `ExpiresUtc` jest ustawiona na `SignInAsync`, zastępuje on wartość `ExpireTimeSpan` opcji `CookieAuthenticationOptions`, jeśli ustawiona.

Poniższy fragment kodu tworzy tożsamość i odpowiedniego pliku cookie, który jest ważny przez 20 minut. To ignoruje wszystkie przesuwanego wcześniej skonfigurowane ustawienia wygaśnięcia.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

---

## <a name="see-also"></a>Zobacz także

* [Zmiany uwierzytelniania 2.0 / migracji anonsu](https://github.com/aspnet/Announcements/issues/262)
* [Ograniczanie tożsamości według schematu](xref:security/authorization/limitingidentitybyscheme)
* [Autoryzacja oparta na oświadczeniach](xref:security/authorization/claims)
* [Rola oparta na zasadach kontroli](xref:security/authorization/roles#policy-based-role-checks)
