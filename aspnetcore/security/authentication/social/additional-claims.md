---
title: Utrwalanie dodatkowe oświadczenia i tokeny od dostawców zewnętrznych w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak zadbać o dodatkowe oświadczenia i tokeny od dostawców zewnętrznych.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: dc8b3e32141466a12e4eff0c86d2d4bed689afe5
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206360"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="f56f6-103">Utrwalanie dodatkowe oświadczenia i tokeny od dostawców zewnętrznych w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f56f6-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="f56f6-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f56f6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f56f6-105">Aplikacji ASP.NET Core można ustanowić dodatkowe oświadczenia i tokeny od dostawców uwierzytelniania zewnętrznych, takich jak Facebook, Google, Microsoft i Twitter.</span><span class="sxs-lookup"><span data-stu-id="f56f6-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="f56f6-106">Każdy dostawca, co spowoduje wyświetlenie różne informacje o użytkownikach na swojej platformie, ale wzorzec otrzymywania i przekształcania danych użytkownika w dodatkowe oświadczenia jest taki sam.</span><span class="sxs-lookup"><span data-stu-id="f56f6-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="f56f6-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f56f6-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisite"></a><span data-ttu-id="f56f6-108">Wymaganie wstępne</span><span class="sxs-lookup"><span data-stu-id="f56f6-108">Prerequisite</span></span>

<span data-ttu-id="f56f6-109">Zdecyduj, które dostawcy uwierzytelniania zewnętrznego do obsługi w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f56f6-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="f56f6-110">Dla każdego dostawcy rejestrowanie aplikacji i uzyskać identyfikator klienta oraz klucz tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="f56f6-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="f56f6-111">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="f56f6-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="f56f6-112">[Przykładową aplikację](#sample-app-instructions) używa [dostawcę uwierzytelniania serwisu Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="f56f6-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="authentication-provider-configuration"></a><span data-ttu-id="f56f6-113">Konfiguracja dostawcy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="f56f6-113">Authentication provider configuration</span></span>

### <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="f56f6-114">Ustaw identyfikator klienta i klucz tajny klienta</span><span class="sxs-lookup"><span data-stu-id="f56f6-114">Set the client ID and client secret</span></span>

<span data-ttu-id="f56f6-115">Dostawca uwierzytelniania OAuth ustanawia relację zaufania z aplikacji przy użyciu Identyfikatora klienta oraz klucz tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="f56f6-115">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="f56f6-116">Identyfikator klienta i wartości klucza tajnego klienta są tworzone dla aplikacji przez dostawcę uwierzytelniania zewnętrznych podczas rejestracji aplikacji za pomocą dostawcy.</span><span class="sxs-lookup"><span data-stu-id="f56f6-116">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="f56f6-117">Każdego dostawcy zewnętrznego przez aplikację muszą być konfigurowane niezależnie z Identyfikatorem klienta i klucz tajny klienta dostawcy.</span><span class="sxs-lookup"><span data-stu-id="f56f6-117">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="f56f6-118">Aby uzyskać więcej informacji zobacz Tematy dostawcy uwierzytelniania zewnętrznego, które mają zastosowanie do danego scenariusza:</span><span class="sxs-lookup"><span data-stu-id="f56f6-118">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="f56f6-119">Uwierzytelnianie przy użyciu usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="f56f6-119">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="f56f6-120">Uwierzytelnianie przy użyciu usługi Google</span><span class="sxs-lookup"><span data-stu-id="f56f6-120">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="f56f6-121">Uwierzytelnianie firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="f56f6-121">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="f56f6-122">Uwierzytelnianie przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="f56f6-122">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="f56f6-123">Inni dostawcy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="f56f6-123">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="f56f6-124">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="f56f6-124">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="f56f6-125">Przykładowa aplikacja konfiguruje dostawcę uwierzytelniania serwisu Google z Identyfikatorem klienta i klucz tajny klienta dostarczane przez firmę Google:</span><span class="sxs-lookup"><span data-stu-id="f56f6-125">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a><span data-ttu-id="f56f6-126">Określa zakres uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="f56f6-126">Establish the authentication scope</span></span>

<span data-ttu-id="f56f6-127">Określ listę uprawnień do pobierania od dostawcy, określając <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="f56f6-127">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="f56f6-128">Zakresy uwierzytelniania dla typowych dostawców zewnętrznych są wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="f56f6-128">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="f56f6-129">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="f56f6-129">Provider</span></span>  | <span data-ttu-id="f56f6-130">Zakres</span><span class="sxs-lookup"><span data-stu-id="f56f6-130">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="f56f6-131">Facebook</span><span class="sxs-lookup"><span data-stu-id="f56f6-131">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="f56f6-132">Google</span><span class="sxs-lookup"><span data-stu-id="f56f6-132">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="f56f6-133">Microsoft</span><span class="sxs-lookup"><span data-stu-id="f56f6-133">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="f56f6-134">Twitter</span><span class="sxs-lookup"><span data-stu-id="f56f6-134">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="f56f6-135">Przykładowa aplikacja dodaje Google `plus.login` zakres żądania logowania Google + uprawnień:</span><span class="sxs-lookup"><span data-stu-id="f56f6-135">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="f56f6-136">Mapy kluczy danych użytkownika i tworzą oświadczenia</span><span class="sxs-lookup"><span data-stu-id="f56f6-136">Map user data keys and create claims</span></span>

<span data-ttu-id="f56f6-137">W oknie dialogowym Opcje dostawcy należy określić <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> dla każdego klucza dane użytkownika JSON zewnętrznego dostawcy tożsamości aplikacji do odczytu na rejestracji.</span><span class="sxs-lookup"><span data-stu-id="f56f6-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="f56f6-138">Aby uzyskać więcej informacji na temat typów oświadczeń, zobacz <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="f56f6-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="f56f6-139">Przykładowa aplikacja tworzy <xref:System.Security.Claims.ClaimTypes.Gender> oświadczeń z `gender` kluczowe dane użytkownika Google:</span><span class="sxs-lookup"><span data-stu-id="f56f6-139">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="f56f6-140">W <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) jest zalogowany w aplikacji za pomocą <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="f56f6-140">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="f56f6-141">Podczas logowania w procesie <xref:Microsoft.AspNetCore.Identity.UserManager`1> można przechowywać `ApplicationUser` oświadczenia dla danych użytkownika z <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="f56f6-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="f56f6-142">W przykładowej aplikacji `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) ustanawia <xref:System.Security.Claims.ClaimTypes.Gender> oświadczenia dla podpisanej w `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="f56f6-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a><span data-ttu-id="f56f6-143">Zapisywanie tokenu dostępu</span><span class="sxs-lookup"><span data-stu-id="f56f6-143">Save the access token</span></span>

<span data-ttu-id="f56f6-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Określa, czy tokeny dostępu i Odśwież powinny być przechowywane w <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> po pomyślnej autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f56f6-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="f56f6-145">`SaveTokens` ustawiono `false` domyślnie, aby zmniejszyć rozmiar pliku cookie uwierzytelniania końcowej.</span><span class="sxs-lookup"><span data-stu-id="f56f6-145">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="f56f6-146">Przykładowa aplikacja ustawia wartość `SaveTokens` do `true` w <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="f56f6-146">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="f56f6-147">Gdy `OnPostConfirmationAsync` wykonuje, przechowywania tokenu dostępu ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) z zewnętrznego dostawcy w `ApplicationUser`firmy `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="f56f6-147">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="f56f6-148">Przykładowa aplikacja zapisuje tokenu dostępu w:</span><span class="sxs-lookup"><span data-stu-id="f56f6-148">The sample app saves the access token in:</span></span>

* <span data-ttu-id="f56f6-149">`OnPostConfirmationAsync` &ndash; Wykonuje rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f56f6-149">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="f56f6-150">`OnGetCallbackAsync` &ndash; Wykonuje, gdy wcześniej zarejestrowany użytkownik zaloguje się do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f56f6-150">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="f56f6-151">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f56f6-151">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="f56f6-152">Jak dodać dodatkowe tokeny niestandardowe</span><span class="sxs-lookup"><span data-stu-id="f56f6-152">How to add additional custom tokens</span></span>

<span data-ttu-id="f56f6-153">Aby zademonstrować, jak dodać niestandardowy token, który jest przechowywany jako część `SaveTokens`, przykładowa aplikacja dodaje <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> z bieżącymi <xref:System.DateTime> dla [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) z `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="f56f6-153">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="f56f6-154">Instrukcje dotyczące aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="f56f6-154">Sample app instructions</span></span>

<span data-ttu-id="f56f6-155">Przykładowa aplikacja pokazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="f56f6-155">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="f56f6-156">Uzyskaj płeć użytkownika z usługi Google i przechowuje oświadczenie z wartością płci.</span><span class="sxs-lookup"><span data-stu-id="f56f6-156">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="f56f6-157">Store Google token dostępu użytkownika `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="f56f6-157">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="f56f6-158">Aby użyć przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f56f6-158">To use the sample app:</span></span>

1. <span data-ttu-id="f56f6-159">Rejestrowanie aplikacji i uzyskać prawidłowy identyfikator klienta oraz klucz tajny klienta dla uwierzytelniania serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="f56f6-159">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="f56f6-160">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/google-logins>.</span><span class="sxs-lookup"><span data-stu-id="f56f6-160">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="f56f6-161">Podanie Identyfikatora klienta oraz klucz tajny klienta aplikacji w <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> z `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f56f6-161">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="f56f6-162">Uruchom aplikację i żądania na stronie Moje oświadczeń.</span><span class="sxs-lookup"><span data-stu-id="f56f6-162">Run the app and request the My Claims page.</span></span> <span data-ttu-id="f56f6-163">Gdy użytkownik nie jest zalogowany, aplikacja przekierowuje do firmy Google.</span><span class="sxs-lookup"><span data-stu-id="f56f6-163">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="f56f6-164">Zaloguj się przy użyciu Google.</span><span class="sxs-lookup"><span data-stu-id="f56f6-164">Sign in with Google.</span></span> <span data-ttu-id="f56f6-165">Google przekierowuje użytkownika do aplikacji (`/Home/MyClaims`).</span><span class="sxs-lookup"><span data-stu-id="f56f6-165">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="f56f6-166">Użytkownik jest uwierzytelniony, a na stronie Moje oświadczeń jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="f56f6-166">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="f56f6-167">Oświadczenie płeć znajduje się w katalogu **oświadczenia użytkownika** z wartością uzyskaną z usługi Google.</span><span class="sxs-lookup"><span data-stu-id="f56f6-167">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="f56f6-168">Token dostępu zostanie wyświetlony w **właściwości uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="f56f6-168">The access token appears in the **Authentication Properties**.</span></span>

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
