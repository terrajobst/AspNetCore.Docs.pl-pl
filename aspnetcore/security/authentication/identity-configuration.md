---
title: Konfigurowanie tożsamości platformy ASP.NET Core
author: AdrienTorris
description: Zrozumienie ASP.NET Core Identity wartości domyślne i Dowiedz się, jak skonfigurować właściwości tożsamości, aby użyć niestandardowej wartości.
ms.author: scaddie
ms.date: 03/06/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 914e9b22ed52b560366fdff1f2430d3dd66454c3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276265"
---
# <a name="configure-aspnet-core-identity"></a>Konfigurowanie tożsamości platformy ASP.NET Core

Tożsamość platformy ASP.NET Core używa domyślnej konfiguracji ustawień, takich jak zasady haseł, czasu blokady i ustawienia plików cookie. Te ustawienia można nadpisać w aplikacji `Startup` klasy.

## <a name="identity-options"></a>Opcje tożsamości

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) klasa reprezentuje opcje, które mogą służyć do konfigurowania systemu tożsamości.

### <a name="claims-identity"></a>Tożsamość oświadczeń

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Określa [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) z właściwości wyświetlane w tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Pobiera lub ustawia typ oświadczenia używany dla oświadczeń roli. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Pobiera lub ustawia typ oświadczenia używany dla oświadczeń sygnatury bezpieczeństwa. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Pobiera lub ustawia typ oświadczenia używany dla oświadczeń identyfikator użytkownika. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Pobiera lub ustawia typ oświadczenia używany dla oświadczeń nazwy użytkownika. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Blokady

Blokuje użytkownika w danym okresie czasu po daną liczbę nieudanych prób dostępu (domyślne: 5 minut blokady po 5 nieudanych prób dostępu). Po pomyślnym uwierzytelnieniu Resetuje licznik nieudanych prób dostępu prób i zresetowanie zegara.

W poniższym przykładzie przedstawiono wartości domyślne:

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

Upewnij się, że [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ustawia `lockoutOnFailure` do `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Określa [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) z właściwości wyświetlane w tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Określa, czy nowego użytkownika można zablokować. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Czas użytkownika zostanie zablokowane po wystąpieniu blokady. | 5 minut |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona. | 5 |

### <a name="password"></a>Hasło

Domyślnie tożsamości wymaga haseł zawierających wielką literę, małą literę, cyfrę i znaków innych niż alfanumeryczne. Hasło musi mieć co najmniej sześciu znaków. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) można zmienić w programie `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Platformy ASP.NET Core 2.0 dodane [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) właściwości. W przeciwnym razie opcje są takie same jak platformy ASP.NET Core 1.x.

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Określa [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) z właściwości wyświetlane w tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Wymaga liczbą z zakresu od 0 do 9 w haśle. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Minimalna długość hasła. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Ma zastosowanie tylko do wersji platformy ASP.NET Core 2.0 lub nowszej.<br><br> Wymaga liczby unikatowych znaków w haśle. | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Wymaga małych znaków w haśle. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Wymaga inne niż alfanumeryczne znaków w haśle. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Wymaga wielką literę w haśle. | `true` |

### <a name="sign-in"></a>Logowanie

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Określa [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) z właściwości wyświetlane w tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Wymaga potwierdzone poczty e-mail do logowania. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Wymaga numeru telefonu potwierdzone do logowania. | `false` |

### <a name="tokens"></a>Tokeny

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Określa [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) z właściwości wyświetlane w tabeli.


|                                                        Właściwość                                                         |                                                                                      Opis                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Pobiera lub ustawia `AuthenticatorTokenProvider` używany do sprawdzania poprawności logowania dwuskładnikowego z wystawcy uwierzytelnienia.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Pobiera lub ustawia `ChangeEmailTokenProvider` służący do generowania tokenów używanych w wiadomości e-mail z potwierdzeniem zmiany poczty e-mail.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Pobiera lub ustawia `ChangePhoneNumberTokenProvider` służący do generowania tokenów używanych w przypadku zmiany numerów telefonów.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Pobiera lub Ustawia dostawcę tokenu służącego do generowania tokenów używanych w wiadomości e-mail z potwierdzeniem konta.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Pobiera lub ustawia [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) używane do generowania tokenów używany w wiadomościach e-mail resetowania hasła. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Używany do budowy [dostawcy tokenu użytkownika](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) z kluczem używana jako nazwa dostawcy.                 |

### <a name="user"></a>Użytkownik

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Określa [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) z właściwości wyświetlane w tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Dozwolonych znaków nazwy użytkownika. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Wymaga unikatowego adresu e-mail każdego użytkownika. | `false` |

## <a name="cookie-settings"></a>Ustawienia plików cookie

Konfigurowanie pliku cookie aplikacji w `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) ma następujące właściwości:

|                                                               Właściwość                                                               |                                                                                                                                                           Opis                                                                                                                                                            |
|--------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)       |                                                                 Program obsługi informuje, że powinno zmienić wychodzący <em>403 Zabroniony</em> kod stanu do <em>przekierowania 302</em> na podanej ścieżce.<br><br>Wartość domyślna to `/Account/AccessDenied`.                                                                  |
|             [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme)              |                                                                                                                Ma zastosowanie tylko do platformy ASP.NET Core 1.x.<br><br> Nazwa logiczna schemat danego uwierzytelniania.                                                                                                                |
|            [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate)             |                                                                       Ma zastosowanie tylko do platformy ASP.NET Core 1.x.<br><br> W przypadku wartości true, uruchom na każde żądanie uwierzytelniania plików cookie i próbę zweryfikowania i rekonstrukcji serializacji podmiot zabezpieczeń, z którym utworzony.                                                                        |
|               [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge)                |                                              Ma zastosowanie tylko do platformy ASP.NET Core 1.x.<br><br> Jeśli PRAWDA, oprogramowanie pośredniczące uwierzytelniania obsługuje automatyczne wyzwania. Jeśli ma wartość FAŁSZ, oprogramowanie pośredniczące uwierzytelniania zmienia tylko odpowiedzi, gdy jest to wyraźnie wskazane przez `AuthenticationScheme`.                                               |
|               [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer)               |                                                             Pobiera lub ustawia wystawcy, które mają być używane dla wszelkie oświadczenia, które są tworzone (dziedziczone z [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)).                                                             |
|                             [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)                              |                                                                                                                                             Domena do skojarzenia z plikiem cookie.                                                                                                                                             |
|                         [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration)                          |                 Pobiera lub ustawia informacje o czasie pliku cookie HTTP (nie pliku cookie uwierzytelniania). Ta właściwość jest zastępowana [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan). Nie można używać w kontekście CookieAuthentication.                  |
|                           [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly)                            |                                                                                                               Wskazuje, czy plik cookie jest dostępny za pomocą skryptu po stronie klienta.<br><br>Wartość domyślna to `true`.                                                                                                                |
|                               [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name)                                |                                                                                                                            Nazwa pliku cookie.<br><br>Wartość domyślna to `.AspNetCore.Cookies`.                                                                                                                            |
|                               [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path)                                |                                                                                                                                                         Ścieżka pliku cookie.                                                                                                                                                         |
|                           [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite)                            |                                                                                           `SameSite` Atrybut pliku cookie.<br><br>Wartość domyślna to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode).                                                                                            |
|                       [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy)                        |                                                   [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) konfiguracji.<br><br>Wartość domyślna to [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy).                                                    |
|                  [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain)                   |                                                                                                                      Ma zastosowanie tylko do platformy ASP.NET Core 1.x.<br><br> Nazwa domeny, w której plik cookie jest obsługiwana.                                                                                                                       |
|                [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly)                 |                                                                                       Ma zastosowanie tylko do platformy ASP.NET Core 1.x.<br><br> Flaga wskazująca, czy plik cookie powinien być dostępny tylko na serwerach.<br><br>Wartość domyślna to `true`.                                                                                        |
|                    [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath)                     |                                                                                                                  Ma zastosowanie tylko do platformy ASP.NET Core 1.x.<br><br> Używane do izolowania aplikacji działających na takiej samej nazwie hosta.                                                                                                                   |
|                  [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure)                   | Ma zastosowanie tylko do platformy ASP.NET Core 1.x.<br><br> Flaga wskazująca, czy utworzony plik cookie powinien być ograniczony do HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`).<br><br>Wartość domyślna to `CookieSecurePolicy.SameAsRequest`. |
|          [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager)          |                                                                                                                         Składnik służący do pobierania plików cookie z żądania lub ustawiania ich w odpowiedzi.                                                                                                                          |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) |                                                                             Jeśli ustawiono, dostawca używane przez [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) ochrony danych.                                                                             |
|                      [Opis](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description)                       |                                                                                                Ma zastosowanie tylko do platformy ASP.NET Core 1.x.<br><br> Dodatkowe informacje o typie uwierzytelniania, który ma zostać udostępnione do aplikacji.                                                                                                |
|                 [Zdarzenia](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events)                 |                                                                                                      Program obsługi wywołuje metody dla dostawcy dające kontroli aplikacji w pewnych punktach, gdzie występuje przetwarzanie.                                                                                                       |
|                 [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype)                 |                                                            Jeśli ustawiona, usługi wpisz, aby pobrać `Events` wystąpienia zamiast właściwości (dziedziczone z [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)).                                                            |
|         [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan)         |                                                                                      Steruje ile czasu biletu uwierzytelniania przechowywane w ważny od punktu, w którym jest tworzona pozostaje pliku cookie.<br><br>Wartość domyślna to 14 dni.                                                                                       |
|              [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)              |                                                                                                       Użytkownik nie ma autoryzacji, jest przekierowywany do tej ścieżki do logowania.<br><br>Wartość domyślna to `/Account/Login`.                                                                                                       |
|             [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)             |                                                                                                            Użytkownik jest zalogowany, jest przekierowywany do tej ścieżki.<br><br>Wartość domyślna to `/Account/Logout`.                                                                                                            |
|     [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter)     |                                              Określa nazwę parametru ciągu zapytania, która jest dołączana przez oprogramowanie pośredniczące podczas <em>401 nieautoryzowane</em> kod stanu jest zmieniana na <em>przekierowania 302</em> na ścieżkę logowania.<br><br>Wartość domyślna to `ReturnUrl`.                                              |
|           [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore)           |                                                                                                                              Opcjonalny kontener, w którym ma być przechowywana tożsamość żądań.                                                                                                                               |
|      [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration)      |                                                                           W przypadku wartości true nowy plik cookie jest wystawiony nową godzinę wygaśnięcia po bieżącym plikiem cookie jest przekroczyło połowę okna wygaśnięcia.<br><br>Wartość domyślna to `true`.                                                                           |
|       [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat)       |                                                                                                 `TicketDataFormat` Służy do ustawiania i usuwania ochrony tożsamości i innych właściwości, które są przechowywane w wartości pliku cookie.                                                                                                  |

