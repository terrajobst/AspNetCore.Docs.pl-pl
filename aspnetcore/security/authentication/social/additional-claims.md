---
title: Utrwalanie dodatkowe oświadczenia i tokeny od dostawców zewnętrznych w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak zadbać o dodatkowe oświadczenia i tokeny od dostawców zewnętrznych.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874843"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Utrwalanie dodatkowe oświadczenia i tokeny od dostawców zewnętrznych w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Aplikacji ASP.NET Core można ustanowić dodatkowe oświadczenia i tokeny od dostawców uwierzytelniania zewnętrznych, takich jak Facebook, Google, Microsoft i Twitter. Każdy dostawca, co spowoduje wyświetlenie różne informacje o użytkownikach na swojej platformie, ale wzorzec otrzymywania i przekształcania danych użytkownika w dodatkowe oświadczenia jest taki sam.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

Zdecyduj, które dostawcy uwierzytelniania zewnętrznego do obsługi w aplikacji. Dla każdego dostawcy rejestrowanie aplikacji i uzyskać identyfikator klienta oraz klucz tajny klienta. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/index>. Ta aplikacja używa przykładowych [dostawcę uwierzytelniania serwisu Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Ustaw identyfikator klienta i klucz tajny klienta

Dostawca uwierzytelniania OAuth ustanawia relację zaufania z aplikacji przy użyciu Identyfikatora klienta oraz klucz tajny klienta. Identyfikator klienta i wartości klucza tajnego klienta są tworzone dla aplikacji przez dostawcę uwierzytelniania zewnętrznych podczas rejestracji aplikacji za pomocą dostawcy. Każdego dostawcy zewnętrznego przez aplikację muszą być konfigurowane niezależnie z Identyfikatorem klienta i klucz tajny klienta dostawcy. Aby uzyskać więcej informacji zobacz Tematy dostawcy uwierzytelniania zewnętrznego, które mają zastosowanie do danego scenariusza:

* [Uwierzytelnianie przy użyciu usługi Facebook](xref:security/authentication/facebook-logins)
* [Uwierzytelnianie przy użyciu usługi Google](xref:security/authentication/google-logins)
* [Uwierzytelnianie firmy Microsoft](xref:security/authentication/microsoft-logins)
* [Uwierzytelnianie przy użyciu usługi Twitter](xref:security/authentication/twitter-logins)
* [Inni dostawcy uwierzytelniania](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Przykładowa aplikacja konfiguruje dostawcę uwierzytelniania serwisu Google z Identyfikatorem klienta i klucz tajny klienta dostarczane przez firmę Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Określa zakres uwierzytelniania

Określ listę uprawnień do pobierania od dostawcy, określając <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Zakresy uwierzytelniania dla typowych dostawców zewnętrznych są wyświetlane w poniższej tabeli.

| Dostawca  | Scope                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

W przykładowej aplikacji, Google `userinfo.profile` zakres jest automatycznie dodawany przez platformę, gdy <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> jest wywoływana w <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>. Jeśli aplikacja wymaga dodatkowe zakresy, należy je dodać do opcji. W poniższym przykładzie Google `https://www.googleapis.com/auth/user.birthday.read` zakres zostanie dodany w celu pobrania datę urodzin użytkownika:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Mapy kluczy danych użytkownika i tworzą oświadczenia

W oknie dialogowym Opcje dostawcy należy określić <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> lub <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> dla każdego klucza/podklucza dane użytkownika JSON zewnętrznego dostawcy tożsamości aplikacji do odczytu na rejestracji. Aby uzyskać więcej informacji na temat typów oświadczeń, zobacz <xref:System.Security.Claims.ClaimTypes>.

Przykładowa aplikacja tworzy ustawienia regionalne (`urn:google:locale`) i obraz (`urn:google:picture`) oświadczeń od `locale` i `picture` klucze Google dane użytkownika:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

W <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) jest zalogowany w aplikacji za pomocą <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Podczas logowania w procesie <xref:Microsoft.AspNetCore.Identity.UserManager%601> można przechowywać `ApplicationUser` oświadczenia dla danych użytkownika z <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

W przykładowej aplikacji `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) określa ustawienia regionalne (`urn:google:locale`) i obraz (`urn:google:picture`) oświadczenia dla podpisanej w `ApplicationUser`, w tym oświadczenia dla <xref:System.Security.Claims.ClaimTypes.GivenName> :

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

Domyślnie oświadczenia użytkownika są przechowywane w pliku cookie uwierzytelniania. Jeśli plik cookie uwierzytelniania jest za duży, może to spowodować aplikację, aby zakończyć się niepowodzeniem, ponieważ:

* Przeglądarka wykrywa, że nagłówek cookie jest za długa.
* Całkowity rozmiar żądania jest zbyt duży.

Jeśli dużą ilość danych użytkownika jest wymagany do przetwarzania żądań użytkowników:

* Ogranicz liczbę i rozmiar oświadczenia użytkownika dla żądania przetwarzania tylko co wymaga aplikacja.
* Użyj niestandardowego <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> pośredniczącego uwierzytelniania pliku Cookie <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> do przechowywania tożsamości na żądań. Zachowaj dużych ilości informacji o tożsamości na serwerze podczas tylko wysyłania klucza sesji małej identyfikator do klienta.

## <a name="save-the-access-token"></a>Zapisywanie tokenu dostępu

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Określa, czy tokeny dostępu i Odśwież powinny być przechowywane w <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> po pomyślnej autoryzacji. `SaveTokens` ustawiono `false` domyślnie, aby zmniejszyć rozmiar pliku cookie uwierzytelniania końcowej.

Przykładowa aplikacja ustawia wartość `SaveTokens` do `true` w <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

Gdy `OnPostConfirmationAsync` wykonuje, przechowywania tokenu dostępu ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) z zewnętrznego dostawcy w `ApplicationUser`firmy `AuthenticationProperties`.

Przykładowa aplikacja zapisuje tokenu dostępu w `OnPostConfirmationAsync` (rejestrowanie nowego użytkownika) i `OnGetCallbackAsync` (wcześniej zarejestrowanego użytkownika) w *Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Jak dodać dodatkowe tokeny niestandardowe

Aby zademonstrować, jak dodać niestandardowy token, który jest przechowywany jako część `SaveTokens`, przykładowa aplikacja dodaje <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> z bieżącymi <xref:System.DateTime> dla [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) z `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a>Tworzenie i dodawanie oświadczeń

Struktura zawiera typowe akcje i metody rozszerzenia dla tworzenie i dodawanie oświadczenia do kolekcji. Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> i <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.

Użytkownicy mogą definiować niestandardowe akcje, wynikające z <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> i wdrażanie abstrakcyjnej <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> metody.

Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Usuwanie działań oświadczeń i oświadczenia

[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) usuwa wszystkie oświadczenia akcje w przypadku danego <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> z kolekcji. [ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) usuwa oświadczenie danego <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> z tożsamości. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> jest używany głównie z [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) można usunąć wygenerowanych przez protokół oświadczeń.

## <a name="sample-app-output"></a>Przykładowe dane wyjściowe z aplikacji

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [inżynierów aplikacji SocialSample ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; połączonej przykładowej aplikacji znajduje się na [repozytorium GitHub aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` inżynierów gałęzi. `master` Gałąź zawiera kod w opracowaniu aktywne w następnej wersji platformy ASP.NET Core. Aby wyświetlić wersję Przykładowa aplikacja dla pełnej wersji platformy ASP.NET Core, użyj **gałęzi** listę rozwijaną listę, aby wybrać gałąź wydania (na przykład `release/2.2`).
