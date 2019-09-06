---
title: Korzystanie z uwierzytelniania plików cookie bez tożsamości ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać uwierzytelniania przy użyciu plików cookie bez tożsamości ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/20/2019
uid: security/authentication/cookie
ms.openlocfilehash: 76c7fc20c8870668ca7c65d975e2ed59f40f7dc8
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384830"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="82aac-103">Korzystanie z uwierzytelniania plików cookie bez tożsamości ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82aac-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="82aac-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="82aac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="82aac-105">ASP.NET Core Identity to kompletny, w pełni funkcjonalny Dostawca uwierzytelniania do tworzenia i obsługiwania logowań.</span><span class="sxs-lookup"><span data-stu-id="82aac-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="82aac-106">Jednak można użyć dostawcy uwierzytelniania uwierzytelniania opartego na plikach cookie bez tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82aac-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="82aac-107">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="82aac-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="82aac-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="82aac-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="82aac-109">W celach demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika, Maria Rodriguez jest stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="82aac-110">Zaloguj się przy użyciu `maria.rodriguez@contoso.com` adresu e-mail i hasła.</span><span class="sxs-lookup"><span data-stu-id="82aac-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="82aac-111">Użytkownik jest uwierzytelniany w `AuthenticateUser` metodzie w pliku *Pages/Account/Login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="82aac-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="82aac-112">W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82aac-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="82aac-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="82aac-113">Configuration</span></span>

<span data-ttu-id="82aac-114">Jeśli aplikacja nie korzysta z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla pakietu [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="82aac-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="82aac-115">W metodzie Utwórz usługi pośredniczące uwierzytelniania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> przy użyciu metod i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="82aac-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="82aac-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>przeszedł `AddAuthentication` do ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="82aac-117">`AuthenticationScheme`jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzować z określonym schematem](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="82aac-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="82aac-118">Ustawienie parametru na CookieAuthenticationDefaults. AuthenticationScheme zapewnia wartość "cookies" dla schematu. [](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) `AuthenticationScheme`</span><span class="sxs-lookup"><span data-stu-id="82aac-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="82aac-119">Można podać dowolną wartość ciągu odróżniającą schemat.</span><span class="sxs-lookup"><span data-stu-id="82aac-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="82aac-120">Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania plików cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="82aac-121">Gdy schemat uwierzytelniania plików cookie nie jest dostarczany <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa `CookieAuthenticationDefaults.AuthenticationScheme` go ("pliki cookie").</span><span class="sxs-lookup"><span data-stu-id="82aac-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="82aac-122"><xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> Właściwość pliku cookie uwierzytelniania jest domyślnie ustawiona na `true` wartość.</span><span class="sxs-lookup"><span data-stu-id="82aac-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="82aac-123">Pliki cookie uwierzytelniania są dozwolone, gdy osoby odwiedzające witrynę nie wyraziły zgody na zbieranie danych.</span><span class="sxs-lookup"><span data-stu-id="82aac-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="82aac-124">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="82aac-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="82aac-125">W `Startup.Configure`, wywołaj `UseAuthentication` i `UseAuthorization` , aby `HttpContext.User` ustawić właściwość i uruchamiać oprogramowanie pośredniczące autoryzacji dla żądań.</span><span class="sxs-lookup"><span data-stu-id="82aac-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="82aac-126">Wywołaj metody `UseAuthorization` `UseEndpoints`i przed `UseAuthentication` wywołaniem:</span><span class="sxs-lookup"><span data-stu-id="82aac-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="82aac-127"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Klasa służy do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="82aac-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="82aac-128">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi na potrzeby uwierzytelniania `Startup.ConfigureServices` w metodzie:</span><span class="sxs-lookup"><span data-stu-id="82aac-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="82aac-129">Oprogramowanie pośredniczące zasad dotyczących plików cookie</span><span class="sxs-lookup"><span data-stu-id="82aac-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="82aac-130">[Oprogramowanie pośredniczące zasad plików cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) umożliwia korzystanie z funkcji zasad dotyczących plików cookie.</span><span class="sxs-lookup"><span data-stu-id="82aac-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="82aac-131">Dodanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest zgodne z&mdash;kolejnością, ma wpływ tylko na składniki podrzędne zarejestrowane w potoku.</span><span class="sxs-lookup"><span data-stu-id="82aac-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="82aac-132">Zastosowanie <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> do oprogramowania pośredniczącego zasad dotyczących plików cookie pozwala kontrolować globalne właściwości przetwarzania plików cookie i przełączać się do programów obsługi przetwarzania plików cookie, gdy pliki cookie są dodawane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="82aac-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="82aac-133">Wartością domyślną <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> jest `SameSiteMode.Lax` umożliwienie uwierzytelniania OAuth2.</span><span class="sxs-lookup"><span data-stu-id="82aac-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="82aac-134">Aby ściśle wymuszać zasady dotyczące `SameSiteMode.Strict`tej samej lokacji, należy `MinimumSameSitePolicy`ustawić.</span><span class="sxs-lookup"><span data-stu-id="82aac-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="82aac-135">Mimo że to ustawienie powoduje przerwanie OAuth2 i innych schematów uwierzytelniania między źródłami, podnosi poziom bezpieczeństwa plików cookie dla innych typów aplikacji, które nie polegają na przetwarzaniu żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="82aac-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="82aac-136">Ustawienie ustawienia pośredniczące zasad dotyczących plików `MinimumSameSitePolicy` cookie dla programu może mieć `Cookie.SameSite` wpływ `CookieAuthenticationOptions` na ustawienia w ustawieniach zgodnie z poniższą macierzą.</span><span class="sxs-lookup"><span data-stu-id="82aac-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="82aac-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="82aac-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="82aac-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="82aac-138">Cookie.SameSite</span></span> | <span data-ttu-id="82aac-139">Wynikowy plik cookie. SameSite ustawienie</span><span class="sxs-lookup"><span data-stu-id="82aac-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="82aac-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-140">SameSiteMode.None</span></span>     | <span data-ttu-id="82aac-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-141">SameSiteMode.None</span></span><br><span data-ttu-id="82aac-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="82aac-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-144">SameSiteMode.None</span></span><br><span data-ttu-id="82aac-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="82aac-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="82aac-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-148">SameSiteMode.None</span></span><br><span data-ttu-id="82aac-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="82aac-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="82aac-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="82aac-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-155">SameSiteMode.None</span></span><br><span data-ttu-id="82aac-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="82aac-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="82aac-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="82aac-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="82aac-161">Utwórz plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="82aac-161">Create an authentication cookie</span></span>

<span data-ttu-id="82aac-162">Aby utworzyć plik cookie przechowujący informacje o użytkowniku <xref:System.Security.Claims.ClaimsPrincipal>, konstrukcja a.</span><span class="sxs-lookup"><span data-stu-id="82aac-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="82aac-163">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="82aac-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="82aac-164">Utwórz z <xref:System.Security.Claims.ClaimsIdentity> dowolnym wymaganym <xref:System.Security.Claims.Claim>elementem s i <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> Wywołaj, aby zalogować użytkownika:</span><span class="sxs-lookup"><span data-stu-id="82aac-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="82aac-165">`SignInAsync`tworzy zaszyfrowany plik cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="82aac-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="82aac-166">Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="82aac-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="82aac-167">System [ochrony danych](xref:security/data-protection/using-data-protection) ASP.NET Core jest używany do szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="82aac-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="82aac-168">W przypadku aplikacji hostowanej na wielu komputerach, równoważenia obciążenia między aplikacjami lub korzystania z kolektywu serwerów sieci Web należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) tak, aby używała tego samego dzwonka klucza i identyfikatora aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="82aac-169">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="82aac-169">Sign out</span></span>

<span data-ttu-id="82aac-170">Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="82aac-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="82aac-171">Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "cookie") nie jest używany jako schemat (na przykład "ContosoCookie"), należy podać schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="82aac-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="82aac-172">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="82aac-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="82aac-173">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="82aac-173">React to back-end changes</span></span>

<span data-ttu-id="82aac-174">Po utworzeniu pliku cookie plik cookie jest pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="82aac-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="82aac-175">Jeśli konto użytkownika jest wyłączone w systemach zaplecza:</span><span class="sxs-lookup"><span data-stu-id="82aac-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="82aac-176">System uwierzytelniania plików cookie aplikacji kontynuuje przetwarzanie żądań na podstawie pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="82aac-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="82aac-177">Użytkownik pozostaje zalogowany do aplikacji, o ile plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="82aac-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="82aac-178"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Zdarzenia można użyć do przechwycenia i zastąpienia weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="82aac-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="82aac-179">Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko odwołujący się użytkowników, którzy uzyskują dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="82aac-180">Jedno z metod weryfikacji plików cookie polega na śledzeniu zmian w bazie danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="82aac-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="82aac-181">Jeśli baza danych nie została zmieniona od czasu wystawienia pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="82aac-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="82aac-182">W przykładowej aplikacji baza danych jest zaimplementowana `IUserRepository` w i `LastChanged` przechowuje wartość.</span><span class="sxs-lookup"><span data-stu-id="82aac-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="82aac-183">Gdy użytkownik zostanie zaktualizowany w bazie danych, `LastChanged` wartość jest ustawiana na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="82aac-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="82aac-184">Aby unieważnić plik cookie, gdy baza danych zmienia się na podstawie `LastChanged` wartości, Utwórz plik cookie `LastChanged` z zastrzeżeniem zawierającym bieżącą `LastChanged` wartość z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="82aac-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="82aac-185">Aby zaimplementować przesłonięcie dla `ValidatePrincipal` zdarzenia, napisz metodę o następującym podpisie w klasie, która pochodzi od: <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents></span><span class="sxs-lookup"><span data-stu-id="82aac-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="82aac-186">Poniżej przedstawiono przykładową implementację programu `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="82aac-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="82aac-187">Zarejestruj wystąpienie zdarzeń podczas rejestracji usługi plików cookie w `Startup.ConfigureServices` metodzie.</span><span class="sxs-lookup"><span data-stu-id="82aac-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="82aac-188">Podaj [rejestrację usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes) dla swojej `CustomCookieAuthenticationEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="82aac-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="82aac-189">Rozważ sytuację, w której nazwa użytkownika jest aktualizowana&mdash;decyzją, która nie ma wpływu na zabezpieczenia w jakikolwiek sposób.</span><span class="sxs-lookup"><span data-stu-id="82aac-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="82aac-190">Jeśli chcesz nieszkodliwej aktualizacji podmiotu zabezpieczeń, wywołaj `context.ReplacePrincipal` i `context.ShouldRenew` ustaw właściwość na `true`.</span><span class="sxs-lookup"><span data-stu-id="82aac-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="82aac-191">Opisane tutaj podejście jest wyzwalane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="82aac-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="82aac-192">Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników w każdym żądaniu może spowodować spadek wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="82aac-193">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="82aac-193">Persistent cookies</span></span>

<span data-ttu-id="82aac-194">Plik cookie może być trwały między sesjami przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="82aac-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="82aac-195">Tę trwałość należy włączyć tylko w przypadku jawnej zgody użytkownika przy użyciu pola wyboru "Zapamiętaj mnie" przy logowaniu lub podobnym mechanizmie.</span><span class="sxs-lookup"><span data-stu-id="82aac-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="82aac-196">Poniższy fragment kodu tworzy tożsamość i odpowiadający jej plik cookie, który przeżyje przez zamknięcia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="82aac-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="82aac-197">Wszystkie skonfigurowane wcześniej ustawienia wygasania są honorowane.</span><span class="sxs-lookup"><span data-stu-id="82aac-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="82aac-198">Jeśli plik cookie wygaśnie, gdy przeglądarka zostanie ZAMKNIĘTA, przeglądarka czyści plik cookie po jego ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="82aac-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="82aac-199">Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> na `true` wartość w<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="82aac-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="82aac-200">Bezwzględne wygaśnięcie pliku cookie</span><span class="sxs-lookup"><span data-stu-id="82aac-200">Absolute cookie expiration</span></span>

<span data-ttu-id="82aac-201">Bezwzględny czas wygaśnięcia można ustawić za <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>pomocą.</span><span class="sxs-lookup"><span data-stu-id="82aac-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="82aac-202">Aby utworzyć trwały plik cookie `IsPersistent` , należy również ustawić opcję.</span><span class="sxs-lookup"><span data-stu-id="82aac-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="82aac-203">W przeciwnym razie plik cookie jest tworzony przy użyciu okresu istnienia sesji i może wygasnąć przed lub po posiadanym przez niego biletem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="82aac-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="82aac-204">Gdy `ExpiresUtc` jest ustawiona, zastępuje wartość <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opcji <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="82aac-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="82aac-205">Poniższy fragment kodu tworzy tożsamość i odpowiedni plik cookie, który trwa przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="82aac-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="82aac-206">Ignoruje to wszystkie ustawienia wygasania, które zostały wcześniej skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="82aac-206">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

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

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="82aac-207">ASP.NET Core Identity to kompletny, w pełni funkcjonalny Dostawca uwierzytelniania do tworzenia i obsługiwania logowań.</span><span class="sxs-lookup"><span data-stu-id="82aac-207">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="82aac-208">Jednak można użyć dostawcy uwierzytelniania uwierzytelniania opartego na plikach cookie bez tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82aac-208">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="82aac-209">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="82aac-209">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="82aac-210">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="82aac-210">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="82aac-211">W celach demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika, Maria Rodriguez jest stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-211">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="82aac-212">Zaloguj się przy użyciu `maria.rodriguez@contoso.com` adresu e-mail i hasła.</span><span class="sxs-lookup"><span data-stu-id="82aac-212">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="82aac-213">Użytkownik jest uwierzytelniany w `AuthenticateUser` metodzie w pliku *Pages/Account/Login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="82aac-213">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="82aac-214">W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82aac-214">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="82aac-215">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="82aac-215">Configuration</span></span>

<span data-ttu-id="82aac-216">Jeśli aplikacja nie korzysta z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla pakietu [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="82aac-216">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="82aac-217">W metodzie Utwórz usługę pośredniczącą uwierzytelniania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> przy użyciu metod i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="82aac-217">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="82aac-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>przeszedł `AddAuthentication` do ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="82aac-219">`AuthenticationScheme`jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzować z określonym schematem](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="82aac-219">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="82aac-220">Ustawienie parametru na CookieAuthenticationDefaults. AuthenticationScheme zapewnia wartość "cookies" dla schematu. [](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) `AuthenticationScheme`</span><span class="sxs-lookup"><span data-stu-id="82aac-220">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="82aac-221">Można podać dowolną wartość ciągu odróżniającą schemat.</span><span class="sxs-lookup"><span data-stu-id="82aac-221">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="82aac-222">Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania plików cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-222">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="82aac-223">Gdy schemat uwierzytelniania plików cookie nie jest dostarczany <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa `CookieAuthenticationDefaults.AuthenticationScheme` go ("pliki cookie").</span><span class="sxs-lookup"><span data-stu-id="82aac-223">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="82aac-224"><xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> Właściwość pliku cookie uwierzytelniania jest domyślnie ustawiona na `true` wartość.</span><span class="sxs-lookup"><span data-stu-id="82aac-224">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="82aac-225">Pliki cookie uwierzytelniania są dozwolone, gdy osoby odwiedzające witrynę nie wyraziły zgody na zbieranie danych.</span><span class="sxs-lookup"><span data-stu-id="82aac-225">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="82aac-226">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="82aac-226">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="82aac-227">W metodzie `UseAuthentication` Wywołaj metodę, aby wywołać `HttpContext.User` oprogramowanie pośredniczące uwierzytelniania, które ustawia właściwość. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="82aac-227">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="82aac-228">Wywołaj `UseMvcWithDefaultRoute` metodęprzedwywołaniemlub:`UseMvc` `UseAuthentication`</span><span class="sxs-lookup"><span data-stu-id="82aac-228">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="82aac-229"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Klasa służy do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="82aac-229">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="82aac-230">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi na potrzeby uwierzytelniania `Startup.ConfigureServices` w metodzie:</span><span class="sxs-lookup"><span data-stu-id="82aac-230">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="82aac-231">Oprogramowanie pośredniczące zasad dotyczących plików cookie</span><span class="sxs-lookup"><span data-stu-id="82aac-231">Cookie Policy Middleware</span></span>

<span data-ttu-id="82aac-232">[Oprogramowanie pośredniczące zasad plików cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) umożliwia korzystanie z funkcji zasad dotyczących plików cookie.</span><span class="sxs-lookup"><span data-stu-id="82aac-232">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="82aac-233">Dodanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest zgodne z&mdash;kolejnością, ma wpływ tylko na składniki podrzędne zarejestrowane w potoku.</span><span class="sxs-lookup"><span data-stu-id="82aac-233">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="82aac-234">Zastosowanie <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> do oprogramowania pośredniczącego zasad dotyczących plików cookie pozwala kontrolować globalne właściwości przetwarzania plików cookie i przełączać się do programów obsługi przetwarzania plików cookie, gdy pliki cookie są dodawane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="82aac-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="82aac-235">Wartością domyślną <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> jest `SameSiteMode.Lax` umożliwienie uwierzytelniania OAuth2.</span><span class="sxs-lookup"><span data-stu-id="82aac-235">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="82aac-236">Aby ściśle wymuszać zasady dotyczące `SameSiteMode.Strict`tej samej lokacji, należy `MinimumSameSitePolicy`ustawić.</span><span class="sxs-lookup"><span data-stu-id="82aac-236">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="82aac-237">Mimo że to ustawienie powoduje przerwanie OAuth2 i innych schematów uwierzytelniania między źródłami, podnosi poziom bezpieczeństwa plików cookie dla innych typów aplikacji, które nie polegają na przetwarzaniu żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="82aac-237">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="82aac-238">Ustawienie ustawienia pośredniczące zasad dotyczących plików `MinimumSameSitePolicy` cookie dla programu może mieć `Cookie.SameSite` wpływ `CookieAuthenticationOptions` na ustawienia w ustawieniach zgodnie z poniższą macierzą.</span><span class="sxs-lookup"><span data-stu-id="82aac-238">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="82aac-239">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="82aac-239">MinimumSameSitePolicy</span></span> | <span data-ttu-id="82aac-240">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="82aac-240">Cookie.SameSite</span></span> | <span data-ttu-id="82aac-241">Wynikowy plik cookie. SameSite ustawienie</span><span class="sxs-lookup"><span data-stu-id="82aac-241">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="82aac-242">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-242">SameSiteMode.None</span></span>     | <span data-ttu-id="82aac-243">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-243">SameSiteMode.None</span></span><br><span data-ttu-id="82aac-244">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-244">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-245">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-245">SameSiteMode.Strict</span></span> | <span data-ttu-id="82aac-246">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-246">SameSiteMode.None</span></span><br><span data-ttu-id="82aac-247">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-247">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-248">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-248">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="82aac-249">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-249">SameSiteMode.Lax</span></span>      | <span data-ttu-id="82aac-250">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-250">SameSiteMode.None</span></span><br><span data-ttu-id="82aac-251">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-251">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-252">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-252">SameSiteMode.Strict</span></span> | <span data-ttu-id="82aac-253">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-254">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-254">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-255">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-255">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="82aac-256">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-256">SameSiteMode.Strict</span></span>   | <span data-ttu-id="82aac-257">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="82aac-257">SameSiteMode.None</span></span><br><span data-ttu-id="82aac-258">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="82aac-258">SameSiteMode.Lax</span></span><br><span data-ttu-id="82aac-259">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-259">SameSiteMode.Strict</span></span> | <span data-ttu-id="82aac-260">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-260">SameSiteMode.Strict</span></span><br><span data-ttu-id="82aac-261">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-261">SameSiteMode.Strict</span></span><br><span data-ttu-id="82aac-262">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="82aac-262">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="82aac-263">Utwórz plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="82aac-263">Create an authentication cookie</span></span>

<span data-ttu-id="82aac-264">Aby utworzyć plik cookie przechowujący informacje o użytkowniku <xref:System.Security.Claims.ClaimsPrincipal>, konstrukcja a.</span><span class="sxs-lookup"><span data-stu-id="82aac-264">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="82aac-265">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="82aac-265">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="82aac-266">Utwórz z <xref:System.Security.Claims.ClaimsIdentity> dowolnym wymaganym <xref:System.Security.Claims.Claim>elementem s i <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> Wywołaj, aby zalogować użytkownika:</span><span class="sxs-lookup"><span data-stu-id="82aac-266">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="82aac-267">`SignInAsync`tworzy zaszyfrowany plik cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="82aac-267">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="82aac-268">Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="82aac-268">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="82aac-269">System [ochrony danych](xref:security/data-protection/using-data-protection) ASP.NET Core jest używany do szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="82aac-269">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="82aac-270">W przypadku aplikacji hostowanej na wielu komputerach, równoważenia obciążenia między aplikacjami lub korzystania z kolektywu serwerów sieci Web należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) tak, aby używała tego samego dzwonka klucza i identyfikatora aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-270">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="82aac-271">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="82aac-271">Sign out</span></span>

<span data-ttu-id="82aac-272">Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="82aac-272">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="82aac-273">Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "cookie") nie jest używany jako schemat (na przykład "ContosoCookie"), należy podać schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="82aac-273">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="82aac-274">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="82aac-274">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="82aac-275">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="82aac-275">React to back-end changes</span></span>

<span data-ttu-id="82aac-276">Po utworzeniu pliku cookie plik cookie jest pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="82aac-276">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="82aac-277">Jeśli konto użytkownika jest wyłączone w systemach zaplecza:</span><span class="sxs-lookup"><span data-stu-id="82aac-277">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="82aac-278">System uwierzytelniania plików cookie aplikacji kontynuuje przetwarzanie żądań na podstawie pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="82aac-278">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="82aac-279">Użytkownik pozostaje zalogowany do aplikacji, o ile plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="82aac-279">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="82aac-280"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Zdarzenia można użyć do przechwycenia i zastąpienia weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="82aac-280">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="82aac-281">Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko odwołujący się użytkowników, którzy uzyskują dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-281">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="82aac-282">Jedno z metod weryfikacji plików cookie polega na śledzeniu zmian w bazie danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="82aac-282">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="82aac-283">Jeśli baza danych nie została zmieniona od czasu wystawienia pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="82aac-283">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="82aac-284">W przykładowej aplikacji baza danych jest zaimplementowana `IUserRepository` w i `LastChanged` przechowuje wartość.</span><span class="sxs-lookup"><span data-stu-id="82aac-284">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="82aac-285">Gdy użytkownik zostanie zaktualizowany w bazie danych, `LastChanged` wartość jest ustawiana na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="82aac-285">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="82aac-286">Aby unieważnić plik cookie, gdy baza danych zmienia się na podstawie `LastChanged` wartości, Utwórz plik cookie `LastChanged` z zastrzeżeniem zawierającym bieżącą `LastChanged` wartość z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="82aac-286">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="82aac-287">Aby zaimplementować przesłonięcie dla `ValidatePrincipal` zdarzenia, napisz metodę o następującym podpisie w klasie, która pochodzi od: <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents></span><span class="sxs-lookup"><span data-stu-id="82aac-287">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="82aac-288">Poniżej przedstawiono przykładową implementację programu `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="82aac-288">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="82aac-289">Zarejestruj wystąpienie zdarzeń podczas rejestracji usługi plików cookie w `Startup.ConfigureServices` metodzie.</span><span class="sxs-lookup"><span data-stu-id="82aac-289">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="82aac-290">Podaj [rejestrację usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes) dla swojej `CustomCookieAuthenticationEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="82aac-290">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="82aac-291">Rozważ sytuację, w której nazwa użytkownika jest aktualizowana&mdash;decyzją, która nie ma wpływu na zabezpieczenia w jakikolwiek sposób.</span><span class="sxs-lookup"><span data-stu-id="82aac-291">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="82aac-292">Jeśli chcesz nieszkodliwej aktualizacji podmiotu zabezpieczeń, wywołaj `context.ReplacePrincipal` i `context.ShouldRenew` ustaw właściwość na `true`.</span><span class="sxs-lookup"><span data-stu-id="82aac-292">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="82aac-293">Opisane tutaj podejście jest wyzwalane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="82aac-293">The approach described here is triggered on every request.</span></span> <span data-ttu-id="82aac-294">Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników w każdym żądaniu może spowodować spadek wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82aac-294">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="82aac-295">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="82aac-295">Persistent cookies</span></span>

<span data-ttu-id="82aac-296">Plik cookie może być trwały między sesjami przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="82aac-296">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="82aac-297">Tę trwałość należy włączyć tylko w przypadku jawnej zgody użytkownika przy użyciu pola wyboru "Zapamiętaj mnie" przy logowaniu lub podobnym mechanizmie.</span><span class="sxs-lookup"><span data-stu-id="82aac-297">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="82aac-298">Poniższy fragment kodu tworzy tożsamość i odpowiadający jej plik cookie, który przeżyje przez zamknięcia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="82aac-298">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="82aac-299">Wszystkie skonfigurowane wcześniej ustawienia wygasania są honorowane.</span><span class="sxs-lookup"><span data-stu-id="82aac-299">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="82aac-300">Jeśli plik cookie wygaśnie, gdy przeglądarka zostanie ZAMKNIĘTA, przeglądarka czyści plik cookie po jego ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="82aac-300">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="82aac-301">Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> na `true` wartość w<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="82aac-301">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="82aac-302">Bezwzględne wygaśnięcie pliku cookie</span><span class="sxs-lookup"><span data-stu-id="82aac-302">Absolute cookie expiration</span></span>

<span data-ttu-id="82aac-303">Bezwzględny czas wygaśnięcia można ustawić za <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>pomocą.</span><span class="sxs-lookup"><span data-stu-id="82aac-303">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="82aac-304">Aby utworzyć trwały plik cookie `IsPersistent` , należy również ustawić opcję.</span><span class="sxs-lookup"><span data-stu-id="82aac-304">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="82aac-305">W przeciwnym razie plik cookie jest tworzony przy użyciu okresu istnienia sesji i może wygasnąć przed lub po posiadanym przez niego biletem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="82aac-305">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="82aac-306">Gdy `ExpiresUtc` jest ustawiona, zastępuje wartość <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opcji <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="82aac-306">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="82aac-307">Poniższy fragment kodu tworzy tożsamość i odpowiedni plik cookie, który trwa przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="82aac-307">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="82aac-308">Ignoruje to wszystkie ustawienia wygasania, które zostały wcześniej skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="82aac-308">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

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

## <a name="additional-resources"></a><span data-ttu-id="82aac-309">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="82aac-309">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="82aac-310">Kontrole ról oparte na zasadach</span><span class="sxs-lookup"><span data-stu-id="82aac-310">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
