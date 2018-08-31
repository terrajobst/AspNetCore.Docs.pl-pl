---
title: Utrwalanie dodatkowe oświadczenia i tokeny od dostawców zewnętrznych w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak zadbać o dodatkowe oświadczenia i tokeny od dostawców zewnętrznych.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 6386dd5f0bd901be01cf56081a6b9ca7f408f9f9
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336220"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Utrwalanie dodatkowe oświadczenia i tokeny od dostawców zewnętrznych w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Aplikacji ASP.NET Core można ustanowić dodatkowe oświadczenia i tokeny od dostawców uwierzytelniania zewnętrznych, takich jak Facebook, Google, Microsoft i Twitter. Każdy dostawca, co spowoduje wyświetlenie różne informacje o użytkownikach na swojej platformie, ale wzorzec otrzymywania i przekształcania danych użytkownika w dodatkowe oświadczenia jest taki sam.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisite"></a>Wymaganie wstępne

Zdecyduj, które dostawcy uwierzytelniania zewnętrznego do obsługi w aplikacji. Dla każdego dostawcy rejestrowanie aplikacji i uzyskać identyfikator klienta oraz klucz tajny klienta. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/index>. [Przykładową aplikację](#sample-app-instructions) używa [dostawcę uwierzytelniania serwisu Google](xref:security/authentication/google-logins).

## <a name="authentication-provider-configuration"></a>Konfiguracja dostawcy uwierzytelniania

### <a name="set-the-client-id-and-client-secret"></a>Ustaw identyfikator klienta i klucz tajny klienta

Dostawca uwierzytelniania OAuth ustanawia relację zaufania z aplikacji przy użyciu Identyfikatora klienta oraz klucz tajny klienta. Identyfikator klienta i wartości klucza tajnego klienta są tworzone dla aplikacji przez dostawcę uwierzytelniania zewnętrznych podczas rejestracji aplikacji za pomocą dostawcy. Każdego dostawcy zewnętrznego przez aplikację muszą być konfigurowane niezależnie z Identyfikatorem klienta i klucz tajny klienta dostawcy. Aby uzyskać więcej informacji zobacz Tematy dostawcy uwierzytelniania zewnętrznego, które mają zastosowanie do danego scenariusza:

* [Uwierzytelnianie przy użyciu usługi Facebook](xref:security/authentication/facebook-logins)
* [Uwierzytelnianie przy użyciu usługi Google](xref:security/authentication/google-logins)
* [Uwierzytelnianie firmy Microsoft](xref:security/authentication/microsoft-logins)
* [Uwierzytelnianie przy użyciu usługi Twitter](xref:security/authentication/twitter-logins)
* [Inni dostawcy uwierzytelniania](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Przykładowa aplikacja konfiguruje dostawcę uwierzytelniania serwisu Google z Identyfikatorem klienta i klucz tajny klienta dostarczane przez firmę Google:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a>Określa zakres uwierzytelniania

Określ listę uprawnień do pobierania od dostawcy, określając <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Zakresy uwierzytelniania dla typowych dostawców zewnętrznych są wyświetlane w poniższej tabeli.

| Dostawcy  | Zakres                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/plus.login`                     |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

Przykładowa aplikacja dodaje Google `plus.login` zakres żądania logowania Google + uprawnień:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a>Mapy kluczy danych użytkownika i tworzą oświadczenia

W oknie dialogowym Opcje dostawcy należy określić <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> dla każdego klucza dane użytkownika JSON zewnętrznego dostawcy tożsamości aplikacji do odczytu na rejestracji. Aby uzyskać więcej informacji na temat typów oświadczeń, zobacz <xref:System.Security.Claims.ClaimTypes>.

Przykładowa aplikacja tworzy <xref:System.Security.Claims.ClaimTypes.Gender> oświadczeń z `gender` kluczowe dane użytkownika Google:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

W <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) jest zalogowany w aplikacji za pomocą <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>. Podczas logowania w procesie <xref:Microsoft.AspNetCore.Identity.UserManager`1> można przechowywać `ApplicationUser` oświadczenia dla danych użytkownika z <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

W przykładowej aplikacji `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) ustanawia <xref:System.Security.Claims.ClaimTypes.Gender> oświadczenia dla podpisanej w `ApplicationUser`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a>Zapisywanie tokenu dostępu

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Określa, czy tokeny dostępu i Odśwież powinny być przechowywane w <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> po pomyślnej autoryzacji. `SaveTokens` ustawiono `false` domyślnie, aby zmniejszyć rozmiar pliku cookie uwierzytelniania końcowej.

Przykładowa aplikacja ustawia wartość `SaveTokens` do `true` w <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

Gdy `OnPostConfirmationAsync` wykonuje, przechowywania tokenu dostępu ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) z zewnętrznego dostawcy w `ApplicationUser`firmy `AuthenticationProperties`.

Przykładowa aplikacja zapisuje tokenu dostępu w:

* `OnPostConfirmationAsync` &ndash; Wykonuje rejestrowanie nowego użytkownika.
* `OnGetCallbackAsync` &ndash; Wykonuje, gdy wcześniej zarejestrowany użytkownik zaloguje się do aplikacji.

*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a>Jak dodać dodatkowe tokeny niestandardowe

Aby zademonstrować, jak dodać niestandardowy token, który jest przechowywany jako część `SaveTokens`, przykładowa aplikacja dodaje <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> z bieżącymi <xref:System.DateTime> dla [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) z `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a>Instrukcje dotyczące aplikacji przykładowej

Przykładowa aplikacja pokazuje, jak:

* Uzyskaj płeć użytkownika z usługi Google i przechowuje oświadczenie z wartością płci.
* Store Google token dostępu użytkownika `AuthenticationProperties`.

Aby użyć przykładowej aplikacji:

1. Rejestrowanie aplikacji i uzyskać prawidłowy identyfikator klienta oraz klucz tajny klienta dla uwierzytelniania serwisu Google. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/google-logins>.
1. Podanie Identyfikatora klienta oraz klucz tajny klienta aplikacji w <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> z `Startup.ConfigureServices`.
1. Uruchom aplikację i żądania na stronie Moje oświadczeń. Gdy użytkownik nie jest zalogowany, aplikacja przekierowuje do firmy Google. Zaloguj się przy użyciu Google. Google przekierowuje użytkownika do aplikacji (`/Home/MyClaims`). Użytkownik jest uwierzytelniony, a na stronie Moje oświadczeń jest ładowany. Oświadczenie płeć znajduje się w katalogu **oświadczenia użytkownika** z wartością uzyskaną z usługi Google. Token dostępu zostanie wyświetlony w **właściwości uwierzytelniania**.

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
