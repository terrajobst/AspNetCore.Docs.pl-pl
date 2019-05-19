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
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="aeffc-103">Utrwalanie dodatkowe oświadczenia i tokeny od dostawców zewnętrznych w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aeffc-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="aeffc-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="aeffc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="aeffc-105">Aplikacji ASP.NET Core można ustanowić dodatkowe oświadczenia i tokeny od dostawców uwierzytelniania zewnętrznych, takich jak Facebook, Google, Microsoft i Twitter.</span><span class="sxs-lookup"><span data-stu-id="aeffc-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="aeffc-106">Każdy dostawca, co spowoduje wyświetlenie różne informacje o użytkownikach na swojej platformie, ale wzorzec otrzymywania i przekształcania danych użytkownika w dodatkowe oświadczenia jest taki sam.</span><span class="sxs-lookup"><span data-stu-id="aeffc-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="aeffc-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aeffc-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aeffc-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="aeffc-108">Prerequisites</span></span>

<span data-ttu-id="aeffc-109">Zdecyduj, które dostawcy uwierzytelniania zewnętrznego do obsługi w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aeffc-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="aeffc-110">Dla każdego dostawcy rejestrowanie aplikacji i uzyskać identyfikator klienta oraz klucz tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="aeffc-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="aeffc-111">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="aeffc-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="aeffc-112">Ta aplikacja używa przykładowych [dostawcę uwierzytelniania serwisu Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="aeffc-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="aeffc-113">Ustaw identyfikator klienta i klucz tajny klienta</span><span class="sxs-lookup"><span data-stu-id="aeffc-113">Set the client ID and client secret</span></span>

<span data-ttu-id="aeffc-114">Dostawca uwierzytelniania OAuth ustanawia relację zaufania z aplikacji przy użyciu Identyfikatora klienta oraz klucz tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="aeffc-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="aeffc-115">Identyfikator klienta i wartości klucza tajnego klienta są tworzone dla aplikacji przez dostawcę uwierzytelniania zewnętrznych podczas rejestracji aplikacji za pomocą dostawcy.</span><span class="sxs-lookup"><span data-stu-id="aeffc-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="aeffc-116">Każdego dostawcy zewnętrznego przez aplikację muszą być konfigurowane niezależnie z Identyfikatorem klienta i klucz tajny klienta dostawcy.</span><span class="sxs-lookup"><span data-stu-id="aeffc-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="aeffc-117">Aby uzyskać więcej informacji zobacz Tematy dostawcy uwierzytelniania zewnętrznego, które mają zastosowanie do danego scenariusza:</span><span class="sxs-lookup"><span data-stu-id="aeffc-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="aeffc-118">Uwierzytelnianie przy użyciu usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="aeffc-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="aeffc-119">Uwierzytelnianie przy użyciu usługi Google</span><span class="sxs-lookup"><span data-stu-id="aeffc-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="aeffc-120">Uwierzytelnianie firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="aeffc-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="aeffc-121">Uwierzytelnianie przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="aeffc-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="aeffc-122">Inni dostawcy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="aeffc-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="aeffc-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="aeffc-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="aeffc-124">Przykładowa aplikacja konfiguruje dostawcę uwierzytelniania serwisu Google z Identyfikatorem klienta i klucz tajny klienta dostarczane przez firmę Google:</span><span class="sxs-lookup"><span data-stu-id="aeffc-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="aeffc-125">Określa zakres uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="aeffc-125">Establish the authentication scope</span></span>

<span data-ttu-id="aeffc-126">Określ listę uprawnień do pobierania od dostawcy, określając <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="aeffc-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="aeffc-127">Zakresy uwierzytelniania dla typowych dostawców zewnętrznych są wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="aeffc-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="aeffc-128">Dostawca</span><span class="sxs-lookup"><span data-stu-id="aeffc-128">Provider</span></span>  | <span data-ttu-id="aeffc-129">Scope</span><span class="sxs-lookup"><span data-stu-id="aeffc-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="aeffc-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="aeffc-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="aeffc-131">Google</span><span class="sxs-lookup"><span data-stu-id="aeffc-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="aeffc-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="aeffc-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="aeffc-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="aeffc-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="aeffc-134">W przykładowej aplikacji, Google `userinfo.profile` zakres jest automatycznie dodawany przez platformę, gdy <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> jest wywoływana w <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="aeffc-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="aeffc-135">Jeśli aplikacja wymaga dodatkowe zakresy, należy je dodać do opcji.</span><span class="sxs-lookup"><span data-stu-id="aeffc-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="aeffc-136">W poniższym przykładzie Google `https://www.googleapis.com/auth/user.birthday.read` zakres zostanie dodany w celu pobrania datę urodzin użytkownika:</span><span class="sxs-lookup"><span data-stu-id="aeffc-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="aeffc-137">Mapy kluczy danych użytkownika i tworzą oświadczenia</span><span class="sxs-lookup"><span data-stu-id="aeffc-137">Map user data keys and create claims</span></span>

<span data-ttu-id="aeffc-138">W oknie dialogowym Opcje dostawcy należy określić <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> lub <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> dla każdego klucza/podklucza dane użytkownika JSON zewnętrznego dostawcy tożsamości aplikacji do odczytu na rejestracji.</span><span class="sxs-lookup"><span data-stu-id="aeffc-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="aeffc-139">Aby uzyskać więcej informacji na temat typów oświadczeń, zobacz <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="aeffc-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="aeffc-140">Przykładowa aplikacja tworzy ustawienia regionalne (`urn:google:locale`) i obraz (`urn:google:picture`) oświadczeń od `locale` i `picture` klucze Google dane użytkownika:</span><span class="sxs-lookup"><span data-stu-id="aeffc-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="aeffc-141">W <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) jest zalogowany w aplikacji za pomocą <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="aeffc-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="aeffc-142">Podczas logowania w procesie <xref:Microsoft.AspNetCore.Identity.UserManager%601> można przechowywać `ApplicationUser` oświadczenia dla danych użytkownika z <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="aeffc-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="aeffc-143">W przykładowej aplikacji `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) określa ustawienia regionalne (`urn:google:locale`) i obraz (`urn:google:picture`) oświadczenia dla podpisanej w `ApplicationUser`, w tym oświadczenia dla <xref:System.Security.Claims.ClaimTypes.GivenName> :</span><span class="sxs-lookup"><span data-stu-id="aeffc-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="aeffc-144">Domyślnie oświadczenia użytkownika są przechowywane w pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="aeffc-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="aeffc-145">Jeśli plik cookie uwierzytelniania jest za duży, może to spowodować aplikację, aby zakończyć się niepowodzeniem, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="aeffc-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="aeffc-146">Przeglądarka wykrywa, że nagłówek cookie jest za długa.</span><span class="sxs-lookup"><span data-stu-id="aeffc-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="aeffc-147">Całkowity rozmiar żądania jest zbyt duży.</span><span class="sxs-lookup"><span data-stu-id="aeffc-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="aeffc-148">Jeśli dużą ilość danych użytkownika jest wymagany do przetwarzania żądań użytkowników:</span><span class="sxs-lookup"><span data-stu-id="aeffc-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="aeffc-149">Ogranicz liczbę i rozmiar oświadczenia użytkownika dla żądania przetwarzania tylko co wymaga aplikacja.</span><span class="sxs-lookup"><span data-stu-id="aeffc-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="aeffc-150">Użyj niestandardowego <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> pośredniczącego uwierzytelniania pliku Cookie <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> do przechowywania tożsamości na żądań.</span><span class="sxs-lookup"><span data-stu-id="aeffc-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="aeffc-151">Zachowaj dużych ilości informacji o tożsamości na serwerze podczas tylko wysyłania klucza sesji małej identyfikator do klienta.</span><span class="sxs-lookup"><span data-stu-id="aeffc-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="aeffc-152">Zapisywanie tokenu dostępu</span><span class="sxs-lookup"><span data-stu-id="aeffc-152">Save the access token</span></span>

<span data-ttu-id="aeffc-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Określa, czy tokeny dostępu i Odśwież powinny być przechowywane w <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> po pomyślnej autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="aeffc-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="aeffc-154">`SaveTokens` ustawiono `false` domyślnie, aby zmniejszyć rozmiar pliku cookie uwierzytelniania końcowej.</span><span class="sxs-lookup"><span data-stu-id="aeffc-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="aeffc-155">Przykładowa aplikacja ustawia wartość `SaveTokens` do `true` w <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="aeffc-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="aeffc-156">Gdy `OnPostConfirmationAsync` wykonuje, przechowywania tokenu dostępu ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) z zewnętrznego dostawcy w `ApplicationUser`firmy `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="aeffc-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="aeffc-157">Przykładowa aplikacja zapisuje tokenu dostępu w `OnPostConfirmationAsync` (rejestrowanie nowego użytkownika) i `OnGetCallbackAsync` (wcześniej zarejestrowanego użytkownika) w *Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="aeffc-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="aeffc-158">Jak dodać dodatkowe tokeny niestandardowe</span><span class="sxs-lookup"><span data-stu-id="aeffc-158">How to add additional custom tokens</span></span>

<span data-ttu-id="aeffc-159">Aby zademonstrować, jak dodać niestandardowy token, który jest przechowywany jako część `SaveTokens`, przykładowa aplikacja dodaje <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> z bieżącymi <xref:System.DateTime> dla [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) z `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="aeffc-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="aeffc-160">Tworzenie i dodawanie oświadczeń</span><span class="sxs-lookup"><span data-stu-id="aeffc-160">Creating and adding claims</span></span>

<span data-ttu-id="aeffc-161">Struktura zawiera typowe akcje i metody rozszerzenia dla tworzenie i dodawanie oświadczenia do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="aeffc-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="aeffc-162">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> i <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="aeffc-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="aeffc-163">Użytkownicy mogą definiować niestandardowe akcje, wynikające z <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> i wdrażanie abstrakcyjnej <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> metody.</span><span class="sxs-lookup"><span data-stu-id="aeffc-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="aeffc-164">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="aeffc-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="aeffc-165">Usuwanie działań oświadczeń i oświadczenia</span><span class="sxs-lookup"><span data-stu-id="aeffc-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="aeffc-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) usuwa wszystkie oświadczenia akcje w przypadku danego <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> z kolekcji.</span><span class="sxs-lookup"><span data-stu-id="aeffc-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="aeffc-167">[ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) usuwa oświadczenie danego <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> z tożsamości.</span><span class="sxs-lookup"><span data-stu-id="aeffc-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="aeffc-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> jest używany głównie z [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) można usunąć wygenerowanych przez protokół oświadczeń.</span><span class="sxs-lookup"><span data-stu-id="aeffc-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="aeffc-169">Przykładowe dane wyjściowe z aplikacji</span><span class="sxs-lookup"><span data-stu-id="aeffc-169">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="aeffc-170">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="aeffc-170">Additional resources</span></span>

* <span data-ttu-id="aeffc-171">[inżynierów aplikacji SocialSample ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; połączonej przykładowej aplikacji znajduje się na [repozytorium GitHub aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` inżynierów gałęzi.</span><span class="sxs-lookup"><span data-stu-id="aeffc-171">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="aeffc-172">`master` Gałąź zawiera kod w opracowaniu aktywne w następnej wersji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aeffc-172">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="aeffc-173">Aby wyświetlić wersję Przykładowa aplikacja dla pełnej wersji platformy ASP.NET Core, użyj **gałęzi** listę rozwijaną listę, aby wybrać gałąź wydania (na przykład `release/2.2`).</span><span class="sxs-lookup"><span data-stu-id="aeffc-173">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/2.2`).</span></span>
