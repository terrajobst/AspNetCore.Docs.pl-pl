---
title: Utrwalaj dodatkowe oświadczenia i tokeny od zewnętrznych dostawców w ASP.NET Core
author: guardrex
description: Dowiedz się, jak ustanowić dodatkowe oświadczenia i tokeny od zewnętrznych dostawców.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 72710d249d3210208dd9b0356a700ba02a0b727a
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378884"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="60234-103">Utrwalaj dodatkowe oświadczenia i tokeny od zewnętrznych dostawców w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60234-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="60234-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="60234-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="60234-105">Aplikacja ASP.NET Core może nawiązać dodatkowe oświadczenia i tokeny od zewnętrznych dostawców uwierzytelniania, takich jak Facebook, Google, Microsoft i Twitter.</span><span class="sxs-lookup"><span data-stu-id="60234-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="60234-106">Każdy dostawca ujawnia różne informacje o użytkownikach na jego platformie, ale wzorzec do odbioru i przekształcania danych użytkownika w dodatkowe oświadczenia jest taki sam.</span><span class="sxs-lookup"><span data-stu-id="60234-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="60234-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="60234-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60234-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="60234-108">Prerequisites</span></span>

<span data-ttu-id="60234-109">Zdecyduj, które zewnętrzne dostawcy uwierzytelniania mają być obsługiwane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60234-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="60234-110">Dla każdego dostawcy Zarejestruj aplikację i uzyskaj identyfikator klienta i klucz tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="60234-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="60234-111">Aby uzyskać więcej informacji, zobacz temat <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="60234-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="60234-112">Przykładowa aplikacja używa [dostawcy uwierzytelniania Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="60234-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="60234-113">Ustawianie identyfikatora klienta i klucza tajnego klienta</span><span class="sxs-lookup"><span data-stu-id="60234-113">Set the client ID and client secret</span></span>

<span data-ttu-id="60234-114">Dostawca uwierzytelniania OAuth ustanawia relację zaufania z aplikacją przy użyciu identyfikatora klienta i klucza tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="60234-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="60234-115">Wartości identyfikator klienta i klucz tajny klienta są tworzone dla aplikacji przez zewnętrznego dostawcę uwierzytelniania, gdy aplikacja jest zarejestrowana w dostawcy.</span><span class="sxs-lookup"><span data-stu-id="60234-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="60234-116">Każdy dostawca zewnętrzny, którego używa aplikacja, musi być skonfigurowany niezależnie od identyfikatora klienta dostawcy i klucza tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="60234-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="60234-117">Aby uzyskać więcej informacji, zobacz tematy dotyczące zewnętrznego dostawcy uwierzytelniania, które dotyczą Twojego scenariusza:</span><span class="sxs-lookup"><span data-stu-id="60234-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="60234-118">Uwierzytelnianie przy użyciu usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="60234-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="60234-119">Uwierzytelnianie przy użyciu usługi Google</span><span class="sxs-lookup"><span data-stu-id="60234-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="60234-120">Uwierzytelnianie firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="60234-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="60234-121">Uwierzytelnianie przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="60234-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="60234-122">Inni dostawcy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="60234-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="60234-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="60234-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="60234-124">Przykładowa aplikacja konfiguruje dostawcę uwierzytelniania Google przy użyciu identyfikatora klienta i klucza tajnego klienta dostarczonego przez firmę Google:</span><span class="sxs-lookup"><span data-stu-id="60234-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="60234-125">Ustanów zakres uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="60234-125">Establish the authentication scope</span></span>

<span data-ttu-id="60234-126">Określ listę uprawnień do pobrania od dostawcy, określając <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="60234-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="60234-127">Zakresy uwierzytelniania dla typowych dostawców zewnętrznych są wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="60234-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="60234-128">Provider</span><span class="sxs-lookup"><span data-stu-id="60234-128">Provider</span></span>  | <span data-ttu-id="60234-129">Zakres</span><span class="sxs-lookup"><span data-stu-id="60234-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="60234-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="60234-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="60234-131">Google</span><span class="sxs-lookup"><span data-stu-id="60234-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="60234-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="60234-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="60234-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="60234-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="60234-134">W przykładowej aplikacji zakres `userinfo.profile` firmy Google jest automatycznie dodawany przez platformę, gdy <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> jest wywoływana w <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="60234-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="60234-135">Jeśli aplikacja wymaga dodatkowych zakresów, należy dodać je do opcji.</span><span class="sxs-lookup"><span data-stu-id="60234-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="60234-136">W poniższym przykładzie zostanie dodany zakres Google `https://www.googleapis.com/auth/user.birthday.read`, aby można było pobrać urodziny użytkownika:</span><span class="sxs-lookup"><span data-stu-id="60234-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="60234-137">Mapuj klucze danych użytkownika i Utwórz oświadczenia</span><span class="sxs-lookup"><span data-stu-id="60234-137">Map user data keys and create claims</span></span>

<span data-ttu-id="60234-138">W opcjach dostawcy Określ <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> lub <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> dla każdego klucza/podklucza w danych użytkownika JSON dostawcy zewnętrznego, aby można było odczytać tożsamość aplikacji podczas logowania.</span><span class="sxs-lookup"><span data-stu-id="60234-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="60234-139">Aby uzyskać więcej informacji na temat typów roszczeń, zobacz <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="60234-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="60234-140">Przykładowa aplikacja tworzy oświadczenia ustawień regionalnych (`urn:google:locale`) i zdjęcia (`urn:google:picture`) z kluczy `locale` i `picture` w danych użytkownika Google:</span><span class="sxs-lookup"><span data-stu-id="60234-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="60234-141">W `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`<xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) jest zalogowany do aplikacji z <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="60234-141">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="60234-142">W procesie logowania <xref:Microsoft.AspNetCore.Identity.UserManager%601> mogą przechowywać oświadczenia `ApplicationUser` dla danych użytkownika dostępnych w <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="60234-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="60234-143">W przykładowej aplikacji `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*) ustanawia oświadczenia ustawień regionalnych (`urn:google:locale`) i obrazu (`urn:google:picture`) dla zalogowanego `ApplicationUser`, w tym oświadczenie dla <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="60234-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="60234-144">Domyślnie oświadczenia użytkownika są przechowywane w pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="60234-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="60234-145">Jeśli plik cookie uwierzytelniania jest zbyt duży, może to spowodować niepowodzenie aplikacji, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="60234-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="60234-146">Przeglądarka wykryje, że nagłówek pliku cookie jest zbyt długi.</span><span class="sxs-lookup"><span data-stu-id="60234-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="60234-147">Całkowity rozmiar żądania jest zbyt duży.</span><span class="sxs-lookup"><span data-stu-id="60234-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="60234-148">Jeśli do przetwarzania żądań użytkowników są wymagane duże ilości danych użytkownika:</span><span class="sxs-lookup"><span data-stu-id="60234-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="60234-149">Ogranicz liczbę i rozmiar oświadczeń użytkowników do przetwarzania żądań tylko do wymaganej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60234-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="60234-150">Użyj niestandardowej <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> dla <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> oprogramowania pośredniczącego uwierzytelniania plików cookie do przechowywania tożsamości między żądaniami.</span><span class="sxs-lookup"><span data-stu-id="60234-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="60234-151">Na serwerze są zachowywane duże ilości informacji o tożsamości, a do klienta jest wysyłany tylko mały klucz identyfikatora sesji.</span><span class="sxs-lookup"><span data-stu-id="60234-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="60234-152">Zapisz token dostępu</span><span class="sxs-lookup"><span data-stu-id="60234-152">Save the access token</span></span>

<span data-ttu-id="60234-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> określa, czy tokeny dostępu i odświeżania mają być przechowywane w <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> po pomyślnej autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="60234-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="60234-154">`SaveTokens` jest domyślnie ustawiona na `false`, aby zmniejszyć rozmiar ostatniego pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="60234-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="60234-155">Aplikacja Przykładowa ustawia wartość `SaveTokens` do `true` w <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="60234-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="60234-156">Gdy `OnPostConfirmationAsync` jest wykonywane, należy przechowywać token dostępu ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) z zewnętrznego dostawcy w `AuthenticationProperties``ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="60234-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="60234-157">Aplikacja Przykładowa zapisuje token dostępu w `OnPostConfirmationAsync` (Rejestracja nowego użytkownika) i `OnGetCallbackAsync` (wcześniej zarejestrowany użytkownik) w ramach *konta/ExternalLogin. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="60234-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="60234-158">Jak dodać dodatkowe tokeny niestandardowe</span><span class="sxs-lookup"><span data-stu-id="60234-158">How to add additional custom tokens</span></span>

<span data-ttu-id="60234-159">Aby zademonstrować sposób dodawania niestandardowego tokenu, który jest przechowywany w ramach `SaveTokens`, przykładowa aplikacja dodaje <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> z bieżącą <xref:System.DateTime> dla [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="60234-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="60234-160">Tworzenie i Dodawanie oświadczeń</span><span class="sxs-lookup"><span data-stu-id="60234-160">Creating and adding claims</span></span>

<span data-ttu-id="60234-161">Struktura zawiera typowe akcje i metody rozszerzające do tworzenia i dodawania oświadczeń do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="60234-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="60234-162">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> i <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="60234-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="60234-163">Użytkownicy mogą definiować niestandardowe akcje, wyprowadzając z <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> i implementując metodę abstrakcyjną <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="60234-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="60234-164">Aby uzyskać więcej informacji, zobacz temat <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="60234-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="60234-165">Usuwanie akcji oświadczeń i oświadczeń</span><span class="sxs-lookup"><span data-stu-id="60234-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="60234-166">[ClaimActionCollection. Remove (ciąg)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) usuwa wszystkie akcje roszczeń dla danego <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> z kolekcji.</span><span class="sxs-lookup"><span data-stu-id="60234-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="60234-167">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) usuwa wniosek danego <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> z tożsamości.</span><span class="sxs-lookup"><span data-stu-id="60234-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="60234-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> jest używany głównie z [OpenID Connect Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) w celu usunięcia oświadczeń generowanych przez protokół.</span><span class="sxs-lookup"><span data-stu-id="60234-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="60234-169">Przykładowe dane wyjściowe aplikacji</span><span class="sxs-lookup"><span data-stu-id="60234-169">Sample app output</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="60234-170">Aplikacja ASP.NET Core może nawiązać dodatkowe oświadczenia i tokeny od zewnętrznych dostawców uwierzytelniania, takich jak Facebook, Google, Microsoft i Twitter.</span><span class="sxs-lookup"><span data-stu-id="60234-170">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="60234-171">Każdy dostawca ujawnia różne informacje o użytkownikach na jego platformie, ale wzorzec do odbioru i przekształcania danych użytkownika w dodatkowe oświadczenia jest taki sam.</span><span class="sxs-lookup"><span data-stu-id="60234-171">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="60234-172">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="60234-172">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60234-173">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="60234-173">Prerequisites</span></span>

<span data-ttu-id="60234-174">Zdecyduj, które zewnętrzne dostawcy uwierzytelniania mają być obsługiwane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60234-174">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="60234-175">Dla każdego dostawcy Zarejestruj aplikację i uzyskaj identyfikator klienta i klucz tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="60234-175">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="60234-176">Aby uzyskać więcej informacji, zobacz temat <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="60234-176">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="60234-177">Przykładowa aplikacja używa [dostawcy uwierzytelniania Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="60234-177">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="60234-178">Ustawianie identyfikatora klienta i klucza tajnego klienta</span><span class="sxs-lookup"><span data-stu-id="60234-178">Set the client ID and client secret</span></span>

<span data-ttu-id="60234-179">Dostawca uwierzytelniania OAuth ustanawia relację zaufania z aplikacją przy użyciu identyfikatora klienta i klucza tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="60234-179">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="60234-180">Wartości identyfikator klienta i klucz tajny klienta są tworzone dla aplikacji przez zewnętrznego dostawcę uwierzytelniania, gdy aplikacja jest zarejestrowana w dostawcy.</span><span class="sxs-lookup"><span data-stu-id="60234-180">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="60234-181">Każdy dostawca zewnętrzny, którego używa aplikacja, musi być skonfigurowany niezależnie od identyfikatora klienta dostawcy i klucza tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="60234-181">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="60234-182">Aby uzyskać więcej informacji, zobacz tematy dotyczące zewnętrznego dostawcy uwierzytelniania, które dotyczą Twojego scenariusza:</span><span class="sxs-lookup"><span data-stu-id="60234-182">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="60234-183">Uwierzytelnianie przy użyciu usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="60234-183">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="60234-184">Uwierzytelnianie przy użyciu usługi Google</span><span class="sxs-lookup"><span data-stu-id="60234-184">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="60234-185">Uwierzytelnianie firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="60234-185">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="60234-186">Uwierzytelnianie przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="60234-186">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="60234-187">Inni dostawcy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="60234-187">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="60234-188">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="60234-188">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="60234-189">Przykładowa aplikacja konfiguruje dostawcę uwierzytelniania Google przy użyciu identyfikatora klienta i klucza tajnego klienta dostarczonego przez firmę Google:</span><span class="sxs-lookup"><span data-stu-id="60234-189">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="60234-190">Ustanów zakres uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="60234-190">Establish the authentication scope</span></span>

<span data-ttu-id="60234-191">Określ listę uprawnień do pobrania od dostawcy, określając <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="60234-191">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="60234-192">Zakresy uwierzytelniania dla typowych dostawców zewnętrznych są wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="60234-192">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="60234-193">Provider</span><span class="sxs-lookup"><span data-stu-id="60234-193">Provider</span></span>  | <span data-ttu-id="60234-194">Zakres</span><span class="sxs-lookup"><span data-stu-id="60234-194">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="60234-195">Facebook</span><span class="sxs-lookup"><span data-stu-id="60234-195">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="60234-196">Google</span><span class="sxs-lookup"><span data-stu-id="60234-196">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="60234-197">Microsoft</span><span class="sxs-lookup"><span data-stu-id="60234-197">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="60234-198">Twitter</span><span class="sxs-lookup"><span data-stu-id="60234-198">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="60234-199">W przykładowej aplikacji zakres `userinfo.profile` firmy Google jest automatycznie dodawany przez platformę, gdy <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> jest wywoływana w <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="60234-199">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="60234-200">Jeśli aplikacja wymaga dodatkowych zakresów, należy dodać je do opcji.</span><span class="sxs-lookup"><span data-stu-id="60234-200">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="60234-201">W poniższym przykładzie zostanie dodany zakres Google `https://www.googleapis.com/auth/user.birthday.read`, aby można było pobrać urodziny użytkownika:</span><span class="sxs-lookup"><span data-stu-id="60234-201">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="60234-202">Mapuj klucze danych użytkownika i Utwórz oświadczenia</span><span class="sxs-lookup"><span data-stu-id="60234-202">Map user data keys and create claims</span></span>

<span data-ttu-id="60234-203">W opcjach dostawcy Określ <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> lub <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> dla każdego klucza/podklucza w danych użytkownika JSON dostawcy zewnętrznego, aby można było odczytać tożsamość aplikacji podczas logowania.</span><span class="sxs-lookup"><span data-stu-id="60234-203">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="60234-204">Aby uzyskać więcej informacji na temat typów roszczeń, zobacz <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="60234-204">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="60234-205">Przykładowa aplikacja tworzy oświadczenia ustawień regionalnych (`urn:google:locale`) i zdjęcia (`urn:google:picture`) z kluczy `locale` i `picture` w danych użytkownika Google:</span><span class="sxs-lookup"><span data-stu-id="60234-205">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="60234-206">W `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`<xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) jest zalogowany do aplikacji z <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="60234-206">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="60234-207">W procesie logowania <xref:Microsoft.AspNetCore.Identity.UserManager%601> mogą przechowywać oświadczenia `ApplicationUser` dla danych użytkownika dostępnych w <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="60234-207">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="60234-208">W przykładowej aplikacji `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*) ustanawia oświadczenia ustawień regionalnych (`urn:google:locale`) i obrazu (`urn:google:picture`) dla zalogowanego `ApplicationUser`, w tym oświadczenie dla <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="60234-208">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="60234-209">Domyślnie oświadczenia użytkownika są przechowywane w pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="60234-209">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="60234-210">Jeśli plik cookie uwierzytelniania jest zbyt duży, może to spowodować niepowodzenie aplikacji, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="60234-210">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="60234-211">Przeglądarka wykryje, że nagłówek pliku cookie jest zbyt długi.</span><span class="sxs-lookup"><span data-stu-id="60234-211">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="60234-212">Całkowity rozmiar żądania jest zbyt duży.</span><span class="sxs-lookup"><span data-stu-id="60234-212">The overall size of the request is too large.</span></span>

<span data-ttu-id="60234-213">Jeśli do przetwarzania żądań użytkowników są wymagane duże ilości danych użytkownika:</span><span class="sxs-lookup"><span data-stu-id="60234-213">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="60234-214">Ogranicz liczbę i rozmiar oświadczeń użytkowników do przetwarzania żądań tylko do wymaganej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60234-214">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="60234-215">Użyj niestandardowej <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> dla <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> oprogramowania pośredniczącego uwierzytelniania plików cookie do przechowywania tożsamości między żądaniami.</span><span class="sxs-lookup"><span data-stu-id="60234-215">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="60234-216">Na serwerze są zachowywane duże ilości informacji o tożsamości, a do klienta jest wysyłany tylko mały klucz identyfikatora sesji.</span><span class="sxs-lookup"><span data-stu-id="60234-216">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="60234-217">Zapisz token dostępu</span><span class="sxs-lookup"><span data-stu-id="60234-217">Save the access token</span></span>

<span data-ttu-id="60234-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> określa, czy tokeny dostępu i odświeżania mają być przechowywane w <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> po pomyślnej autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="60234-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="60234-219">`SaveTokens` jest domyślnie ustawiona na `false`, aby zmniejszyć rozmiar ostatniego pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="60234-219">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="60234-220">Aplikacja Przykładowa ustawia wartość `SaveTokens` do `true` w <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="60234-220">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="60234-221">Gdy `OnPostConfirmationAsync` jest wykonywane, należy przechowywać token dostępu ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) z zewnętrznego dostawcy w `AuthenticationProperties``ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="60234-221">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="60234-222">Aplikacja Przykładowa zapisuje token dostępu w `OnPostConfirmationAsync` (Rejestracja nowego użytkownika) i `OnGetCallbackAsync` (wcześniej zarejestrowany użytkownik) w ramach *konta/ExternalLogin. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="60234-222">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="60234-223">Jak dodać dodatkowe tokeny niestandardowe</span><span class="sxs-lookup"><span data-stu-id="60234-223">How to add additional custom tokens</span></span>

<span data-ttu-id="60234-224">Aby zademonstrować sposób dodawania niestandardowego tokenu, który jest przechowywany w ramach `SaveTokens`, przykładowa aplikacja dodaje <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> z bieżącą <xref:System.DateTime> dla [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="60234-224">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="60234-225">Tworzenie i Dodawanie oświadczeń</span><span class="sxs-lookup"><span data-stu-id="60234-225">Creating and adding claims</span></span>

<span data-ttu-id="60234-226">Struktura zawiera typowe akcje i metody rozszerzające do tworzenia i dodawania oświadczeń do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="60234-226">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="60234-227">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> i <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="60234-227">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="60234-228">Użytkownicy mogą definiować niestandardowe akcje, wyprowadzając z <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> i implementując metodę abstrakcyjną <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="60234-228">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="60234-229">Aby uzyskać więcej informacji, zobacz temat <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="60234-229">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="60234-230">Usuwanie akcji oświadczeń i oświadczeń</span><span class="sxs-lookup"><span data-stu-id="60234-230">Removal of claim actions and claims</span></span>

<span data-ttu-id="60234-231">[ClaimActionCollection. Remove (ciąg)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) usuwa wszystkie akcje roszczeń dla danego <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> z kolekcji.</span><span class="sxs-lookup"><span data-stu-id="60234-231">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="60234-232">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) usuwa wniosek danego <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> z tożsamości.</span><span class="sxs-lookup"><span data-stu-id="60234-232">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="60234-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> jest używany głównie z [OpenID Connect Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) w celu usunięcia oświadczeń generowanych przez protokół.</span><span class="sxs-lookup"><span data-stu-id="60234-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="60234-234">Przykładowe dane wyjściowe aplikacji</span><span class="sxs-lookup"><span data-stu-id="60234-234">Sample app output</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="60234-235">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="60234-235">Additional resources</span></span>

* <span data-ttu-id="60234-236">[aplikacja AspNetCore Engineering SocialSample](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) , która &ndash; połączonej przykładowej aplikacji, znajduje się w gałęzi inżyniera `master`a w [repozytorium GitHub/AspNetCore](https://github.com/aspnet/AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="60234-236">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="60234-237">Gałąź `master` zawiera kod w ramach aktywnego programowania dla następnej wersji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60234-237">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="60234-238">Aby wyświetlić wersję przykładowej aplikacji dla wydanej wersji ASP.NET Core, Użyj listy rozwijanej **rozgałęzienie** , aby wybrać gałąź wydania (na przykład `release/{X.Y}`).</span><span class="sxs-lookup"><span data-stu-id="60234-238">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
