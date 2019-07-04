---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania bez tożsamości platformy ASP.NET Core
author: rick-anderson
description: Omówienie usługi Facebook, Google, Twitter, itp. konto użytkownika uwierzytelniania bez tożsamości platformy ASP.NET Core.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 1e7124e8b07c0faf2d005ec3ef55c0414a697d64
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561565"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="89d42-103">Użyj uwierzytelniania społecznościowych dostawcy logowania bez tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89d42-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="89d42-104"><xref:security/authentication/social/index> w tym artykule opisano, jak umożliwić użytkownikom na logowanie przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="89d42-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="89d42-105">Podejście opisane w tym temacie zawiera tożsamości platformy ASP.NET Core jako dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="89d42-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="89d42-106">W tym przykładzie przedstawiono sposób użycia dostawcy uwierzytelniania zewnętrznych **bez** tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="89d42-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="89d42-107">Jest to przydatne w przypadku aplikacji, które nie wymagają wszystkich funkcji tożsamości platformy ASP.NET Core, ale nadal wymagają integracji z zaufanego zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="89d42-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="89d42-108">W tym przykładzie użyto [uwierzytelniania serwisu Google](xref:security/authentication/google-logins) do uwierzytelniania użytkowników.</span><span class="sxs-lookup"><span data-stu-id="89d42-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="89d42-109">Za pomocą Google uwierzytelniania przenosi wiele złożoności zarządzania proces logowania do usługi Google.</span><span class="sxs-lookup"><span data-stu-id="89d42-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="89d42-110">Aby zintegrować przy użyciu innego zewnętrznego dostawcę uwierzytelniania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="89d42-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="89d42-111">Uwierzytelnianie przy użyciu usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="89d42-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="89d42-112">Uwierzytelnianie firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="89d42-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="89d42-113">Uwierzytelnianie przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="89d42-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="89d42-114">Inni dostawcy</span><span class="sxs-lookup"><span data-stu-id="89d42-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="89d42-115">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="89d42-115">Configuration</span></span>

<span data-ttu-id="89d42-116">W `ConfigureServices` metody, skonfiguruj schematów uwierzytelniania aplikacji za pomocą `AddAuthentication`, `AddCookie` i `AddGoogle` metody:</span><span class="sxs-lookup"><span data-stu-id="89d42-116">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie` and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="89d42-117">Wywołanie [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) ustawia aplikacji [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span><span class="sxs-lookup"><span data-stu-id="89d42-117">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="89d42-118">`DefaultScheme` Jest domyślny schemat używany przez następujące `HttpContext` metody rozszerzenia dla uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="89d42-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="89d42-119">Ustawienie aplikacji `DefaultScheme` do [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("plików cookie" ") umożliwia skonfigurowanie aplikacji na używanie plików cookie jako domyślny schemat dotyczącej tych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="89d42-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="89d42-120">Ustawienie aplikacji <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> do [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") umożliwia skonfigurowanie aplikacji do użytku jako domyślny schemat wywołania Google `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="89d42-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="89d42-121">`DefaultChallengeScheme` zastępuje `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="89d42-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="89d42-122">Zobacz <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> dodatkowe właściwości, które zastępują `DefaultScheme` po ustawieniu.</span><span class="sxs-lookup"><span data-stu-id="89d42-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="89d42-123">W `Configure` metody, wywołanie `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="89d42-123">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="89d42-124">Wywołaj `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="89d42-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="89d42-125">Aby dowiedzieć się więcej na temat uwierzytelniania plików cookie i schematy uwierzytelniania, zobacz <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="89d42-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="applying-authorization"></a><span data-ttu-id="89d42-126">Stosowanie autoryzacji</span><span class="sxs-lookup"><span data-stu-id="89d42-126">Applying authorization</span></span>

<span data-ttu-id="89d42-127">Testowanie konfigurację uwierzytelniania aplikacji, stosując `AuthorizeAttribute` atrybutu kontrolerze, akcji lub strony.</span><span class="sxs-lookup"><span data-stu-id="89d42-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="89d42-128">Poniższy kod ogranicza dostęp do *zachowania* strony do użytkowników uwierzytelnionych:</span><span class="sxs-lookup"><span data-stu-id="89d42-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="89d42-129">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="89d42-129">Sign out</span></span>

<span data-ttu-id="89d42-130">Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="89d42-130">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span></span> <span data-ttu-id="89d42-131">Poniższy kod dodaje `Logout` obsługi strony ma *indeksu* strony:</span><span class="sxs-lookup"><span data-stu-id="89d42-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="89d42-132">Należy zauważyć, że wywołanie `SignOutAsync` nie określono schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="89d42-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="89d42-133">Aplikacja `DefaultScheme` z `CookieAuthenticationDefaults.AuthenticationScheme` jest używany jako bazowy.</span><span class="sxs-lookup"><span data-stu-id="89d42-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="89d42-134">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="89d42-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
