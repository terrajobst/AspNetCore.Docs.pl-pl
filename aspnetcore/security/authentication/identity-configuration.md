---
title: Konfigurowanie tożsamości platformy ASP.NET Core
author: AdrienTorris
description: Zrozumienie wartości domyślne tożsamości platformy ASP.NET Core i Dowiedz się, jak skonfigurować właściwości tożsamości, aby użyć niestandardowej wartości.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 3213f669cbfccdcda7cc7c0142b8101e696678e6
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159516"
---
# <a name="configure-aspnet-core-identity"></a>Konfigurowanie tożsamości platformy ASP.NET Core

Tożsamość platformy ASP.NET Core używa domyślnych wartości dla ustawienia, takie jak zasady haseł, blokowanie i plik cookie konfiguracji. Te ustawienia mogą zostać zastąpione w `Startup` klasy.

## <a name="identity-options"></a>Opcji tożsamości

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) klasa przedstawia opcje, które mogą być używane do konfigurowania systemu tożsamości. `IdentityOptions` należy ustawić **po** wywoływania `AddIdentity` lub `AddDefaultIdentity`.

### <a name="claims-identity"></a>Tożsamość oświadczeń

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Określa [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) za pomocą właściwości pokazane w poniższej tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Pobiera lub ustawia typ oświadczenia używany dla oświadczeń roli. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Pobiera lub ustawia typ oświadczenia używany dla oświadczeń sygnatury bezpieczeństwa. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Pobiera lub ustawia typ oświadczenia używany dla oświadczeń identyfikatora użytkownika. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Pobiera lub ustawia typ oświadczenia używany dla oświadczenia nazwy użytkownika. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Blokady

Blokada jest ustawiana w [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) metody:

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

Powyższy kod opiera się na `Login` tożsamość szablonu. 

Opcje blokady są ustawiane w `StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

Poprzedni zestawów kodu [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) z wartościami domyślnymi.

Po pomyślnym uwierzytelnieniu resetuje liczbę prób dostępu nie powiodło się i zresetowanie zegara.

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Określa [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) przy użyciu właściwości wyświetlane w tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Określa, jeśli nowy użytkownik może być zablokowane. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Czas blokady użytkownika po wystąpieniu blokady. | 5 minut |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona. | 5 |

### <a name="password"></a>Hasło

Domyślnie tożsamości wymaga haseł zawierających wielkiej litery, małej litery, cyfry i znak inny niż alfanumeryczny. Hasło musi mieć co najmniej 6 znaków. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) można ustawić w `Startup.ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Określa [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) przy użyciu właściwości wyświetlane w tabeli.

::: moniker range=">= aspnetcore-2.0"

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Wymaga liczbą z zakresu od 0 do 9, w haśle. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Minimalna długość hasła. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Wymaga małą literę w haśle. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Wymaga znaków niealfanumerycznych w haśle. | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Ma zastosowanie tylko do ASP.NET Core 2.0 lub nowszej.<br><br> Musi zawierać cyfry, odrębnych znaków w haśle. | 1 |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Wymaga wielkiej litery w haśle. | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Wymaga liczbą z zakresu od 0 do 9, w haśle. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Minimalna długość hasła. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Wymaga małą literę w haśle. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Wymaga znaków niealfanumerycznych w haśle. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Wymaga wielkiej litery w haśle. | `true` |

::: moniker-end

### <a name="sign-in"></a>Zaloguj się

Poniższy kod ustawia `SignIn` ustawień (wartości domyślne):

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Określa [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) przy użyciu właściwości wyświetlane w tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Wymaga potwierdzony adres e-mail do logowania. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Wymaga numeru telefonu potwierdzone zalogować się. | `false` |

### <a name="tokens"></a>Tokeny

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Określa [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) przy użyciu właściwości wyświetlane w tabeli.


|                                                        Właściwość                                                         |                                                                                      Opis                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Pobiera lub ustawia `AuthenticatorTokenProvider` służący do weryfikowania logowania dwuskładnikowego z wystawcy uwierzytelnienia.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Pobiera lub ustawia `ChangeEmailTokenProvider` używane do generowania tokenów używanych w wiadomości e-mail z potwierdzeniem zmiany wiadomości e-mail.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Pobiera lub ustawia `ChangePhoneNumberTokenProvider` używane do generowania tokenów w przypadku zmiany numerów telefonów.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Pobiera lub Ustawia dostawcę tokenu służącego do generowania tokenów używanych w wiadomości e-mail z potwierdzeniem konta.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Pobiera lub ustawia [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) używane do generowania tokenów używane w wiadomościach e-mail resetowania hasła. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Używany do budowy [dostawcy tokenu użytkownika](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) przy użyciu klucza używana jako nazwa dostawcy.                 |

### <a name="user"></a>Użytkownik

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Określa [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) przy użyciu właściwości wyświetlane w tabeli.

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Dozwolonych znaków nazwy użytkownika. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Wymaga każdy użytkownik musi mieć unikatowy adres e-mail. | `false` |

### <a name="cookie-settings"></a>Ustawienia plików cookie

Konfigurowanie aplikacji plik cookie, `Startup.ConfigureServices`. [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) musi zostać wywołana **po** wywoływania `AddIdentity` lub `AddDefaultIdentity`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

Aby uzyskać więcej informacji, zobacz [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).

## <a name="password-hasher-options"></a>Opcje Hasher hasła

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> pobiera i ustawia opcje tworzenia skrótu hasła.

| Opcja | Opis |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | Tryb zgodności używany podczas mieszania nowych haseł. Wartość domyślna to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. Pierwszy bajt skrótem hasła, o nazwie *format znacznika*, określa numer wersji to algorytm mieszania używany do tworzenia skrótu hasła. Podczas weryfikowania haseł przed skrót, <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> metoda wybiera poprawny algorytm, oparty na pierwszy bajt. Klient jest w stanie do uwierzytelniania niezależnie od tego z nich użyto wersję algorytmu wyznaczania wartości skrótu hasła. Ustawienie trybu zgodności dotyczy mieszanie *nowych haseł*. |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | Liczba iteracji, używane podczas tworzenia skrótów haseł przy użyciu PBKDF2. Ta wartość jest tylko wtedy, gdy używane <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> ustawiono <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. Wartość musi być dodatnią liczbą całkowitą i wartość domyślna to `10000`. |

W poniższym przykładzie <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> ustawiono `12000` w `Startup.ConfigureServices`:

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
