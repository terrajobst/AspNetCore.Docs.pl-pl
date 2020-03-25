---
title: Korzystanie z uwierzytelniania plików cookie bez tożsamości ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać uwierzytelniania przy użyciu plików cookie bez tożsamości ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: b7c8b2cccb27dd6818330b17439675e41bfef013
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219210"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="f45fe-103">Korzystanie z uwierzytelniania plików cookie bez tożsamości ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f45fe-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="f45fe-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f45fe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f45fe-105">ASP.NET Core Identity to kompletny, w pełni funkcjonalny Dostawca uwierzytelniania do tworzenia i obsługiwania logowań.</span><span class="sxs-lookup"><span data-stu-id="f45fe-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="f45fe-106">Jednak można użyć dostawcy uwierzytelniania opartego na plikach cookie bez tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f45fe-106">However, a cookie-based authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="f45fe-107">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="f45fe-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="f45fe-108">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f45fe-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f45fe-109">W celach demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika, Maria Rodriguez jest stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="f45fe-110">Użyj adresu **e-mail** `maria.rodriguez@contoso.com` i hasła do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f45fe-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="f45fe-111">Użytkownik jest uwierzytelniany w metodzie `AuthenticateUser` w pliku *Pages/Account/Login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="f45fe-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="f45fe-112">W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f45fe-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="f45fe-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="f45fe-113">Configuration</span></span>

<span data-ttu-id="f45fe-114">Jeśli aplikacja nie korzysta z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla pakietu [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="f45fe-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="f45fe-115">W metodzie `Startup.ConfigureServices` Utwórz usługi pośredniczące uwierzytelniania przy użyciu metod <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:</span><span class="sxs-lookup"><span data-stu-id="f45fe-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="f45fe-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> przeszedł do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="f45fe-117">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzować z określonym schematem](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="f45fe-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="f45fe-118">Ustawienie `AuthenticationScheme` na [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) zapewnia wartość "cookies" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="f45fe-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="f45fe-119">Można podać dowolną wartość ciągu odróżniającą schemat.</span><span class="sxs-lookup"><span data-stu-id="f45fe-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="f45fe-120">Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania plików cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="f45fe-121">Gdy schemat uwierzytelniania plików cookie nie jest dostarczany do <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa `CookieAuthenticationDefaults.AuthenticationScheme` ("pliki cookie").</span><span class="sxs-lookup"><span data-stu-id="f45fe-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="f45fe-122">Właściwość <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> pliku cookie uwierzytelniania jest domyślnie ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="f45fe-123">Pliki cookie uwierzytelniania są dozwolone, gdy osoby odwiedzające witrynę nie wyraziły zgody na zbieranie danych.</span><span class="sxs-lookup"><span data-stu-id="f45fe-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="f45fe-124">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="f45fe-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="f45fe-125">W `Startup.Configure`Wywołaj `UseAuthentication` i `UseAuthorization`, aby ustawić właściwość `HttpContext.User` i uruchomić oprogramowanie pośredniczące autoryzacji dla żądań.</span><span class="sxs-lookup"><span data-stu-id="f45fe-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="f45fe-126">Przed wywołaniem `UseEndpoints`wywołaj metody `UseAuthentication` i `UseAuthorization`:</span><span class="sxs-lookup"><span data-stu-id="f45fe-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="f45fe-127">Klasa <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> służy do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="f45fe-128">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi na potrzeby uwierzytelniania w `Startup.ConfigureServices` metodzie:</span><span class="sxs-lookup"><span data-stu-id="f45fe-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="f45fe-129">Oprogramowanie pośredniczące zasad dotyczących plików cookie</span><span class="sxs-lookup"><span data-stu-id="f45fe-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="f45fe-130">[Oprogramowanie pośredniczące zasad plików cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) umożliwia korzystanie z funkcji zasad dotyczących plików cookie.</span><span class="sxs-lookup"><span data-stu-id="f45fe-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="f45fe-131">Dodanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest zależne od kolejności&mdash;ma wpływ tylko na składniki podrzędne zarejestrowane w potoku.</span><span class="sxs-lookup"><span data-stu-id="f45fe-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="f45fe-132">Użyj <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> dostarczanego do programu pośredniczącego zasad dotyczących plików cookie, aby kontrolować globalne właściwości przetwarzania plików cookie i podłączać się do programów obsługi przetwarzania plików cookie, gdy pliki cookie są dodawane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="f45fe-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="f45fe-133">Domyślna wartość <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> jest `SameSiteMode.Lax`, aby zezwalać na uwierzytelnianie OAuth2.</span><span class="sxs-lookup"><span data-stu-id="f45fe-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="f45fe-134">Aby ściśle wymusić zasady dotyczące tej samej lokacji `SameSiteMode.Strict`, ustaw `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="f45fe-135">Mimo że to ustawienie powoduje przerwanie OAuth2 i innych schematów uwierzytelniania między źródłami, podnosi poziom bezpieczeństwa plików cookie dla innych typów aplikacji, które nie polegają na przetwarzaniu żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="f45fe-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="f45fe-136">Ustawienie ustawienia pośredniczące zasad plików cookie dla `MinimumSameSitePolicy` może wpływać na ustawienie `Cookie.SameSite` w ustawieniach `CookieAuthenticationOptions` zgodnie z poniższą macierzą.</span><span class="sxs-lookup"><span data-stu-id="f45fe-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="f45fe-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="f45fe-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="f45fe-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="f45fe-138">Cookie.SameSite</span></span> | <span data-ttu-id="f45fe-139">Wynikowy plik cookie. SameSite ustawienie</span><span class="sxs-lookup"><span data-stu-id="f45fe-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="f45fe-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-140">SameSiteMode.None</span></span>     | <span data-ttu-id="f45fe-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-141">SameSiteMode.None</span></span><br><span data-ttu-id="f45fe-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="f45fe-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-144">SameSiteMode.None</span></span><br><span data-ttu-id="f45fe-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="f45fe-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="f45fe-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-148">SameSiteMode.None</span></span><br><span data-ttu-id="f45fe-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="f45fe-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="f45fe-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="f45fe-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-155">SameSiteMode.None</span></span><br><span data-ttu-id="f45fe-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="f45fe-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="f45fe-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="f45fe-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="f45fe-161">Utwórz plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="f45fe-161">Create an authentication cookie</span></span>

<span data-ttu-id="f45fe-162">Aby utworzyć plik cookie przechowujący informacje o użytkowniku, Utwórz <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="f45fe-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="f45fe-163">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="f45fe-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="f45fe-164">Utwórz <xref:System.Security.Claims.ClaimsIdentity> z dowolnym wymaganym <xref:System.Security.Claims.Claim>s i Wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>, aby zalogować użytkownika:</span><span class="sxs-lookup"><span data-stu-id="f45fe-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

<span data-ttu-id="f45fe-165">`SignInAsync` tworzy zaszyfrowany plik cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f45fe-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="f45fe-166">Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="f45fe-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="f45fe-167">System [ochrony danych](xref:security/data-protection/using-data-protection) ASP.NET Core jest używany do szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="f45fe-168">W przypadku aplikacji hostowanej na wielu komputerach, równoważenia obciążenia między aplikacjami lub korzystania z kolektywu serwerów sieci Web należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) tak, aby używała tego samego dzwonka klucza i identyfikatora aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="f45fe-169">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="f45fe-169">Sign out</span></span>

<span data-ttu-id="f45fe-170">Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="f45fe-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="f45fe-171">Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "cookies") nie jest używany jako schemat (na przykład "ContosoCookie"), należy podać schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="f45fe-172">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="f45fe-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="f45fe-173">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="f45fe-173">React to back-end changes</span></span>

<span data-ttu-id="f45fe-174">Po utworzeniu pliku cookie plik cookie jest pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f45fe-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="f45fe-175">Jeśli konto użytkownika jest wyłączone w systemach zaplecza:</span><span class="sxs-lookup"><span data-stu-id="f45fe-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="f45fe-176">System uwierzytelniania plików cookie aplikacji kontynuuje przetwarzanie żądań na podstawie pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="f45fe-177">Użytkownik pozostaje zalogowany do aplikacji, o ile plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="f45fe-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="f45fe-178">Zdarzenia <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> mogą służyć do przechwycenia i zastąpienia weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="f45fe-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="f45fe-179">Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko odwołujący się użytkowników, którzy uzyskują dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="f45fe-180">Jedno z metod weryfikacji plików cookie polega na śledzeniu zmian w bazie danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f45fe-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="f45fe-181">Jeśli baza danych nie została zmieniona od czasu wystawienia pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="f45fe-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="f45fe-182">W przykładowej aplikacji baza danych jest zaimplementowana w `IUserRepository` i przechowuje `LastChanged` wartość.</span><span class="sxs-lookup"><span data-stu-id="f45fe-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="f45fe-183">Gdy użytkownik zostanie zaktualizowany w bazie danych, wartość `LastChanged` jest ustawiana na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="f45fe-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="f45fe-184">Aby unieważnić plik cookie, gdy baza danych zmienia się w oparciu o wartość `LastChanged`, Utwórz plik cookie z `LastChanged`m, zawierającym wartość bieżącą `LastChanged` z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="f45fe-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="f45fe-185">Aby zaimplementować przesłonięcie dla zdarzenia `ValidatePrincipal`, napisz metodę o następującej sygnaturze w klasie, która pochodzi od <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="f45fe-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="f45fe-186">Poniżej przedstawiono przykładową implementację `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="f45fe-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="f45fe-187">Zarejestruj wystąpienie zdarzeń podczas rejestracji usługi plików cookie w metodzie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f45fe-188">Podaj [rejestrację usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes) dla swojej klasy `CustomCookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="f45fe-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="f45fe-189">Rozważ sytuację, w której nazwa użytkownika jest aktualizowana&mdash;decyzji, która nie ma wpływu na zabezpieczenia w jakikolwiek sposób.</span><span class="sxs-lookup"><span data-stu-id="f45fe-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="f45fe-190">Jeśli chcesz nieszkodliwej aktualizacji podmiotu zabezpieczeń, wywołaj `context.ReplacePrincipal` i ustaw właściwość `context.ShouldRenew` na `true`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="f45fe-191">Opisane tutaj podejście jest wyzwalane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="f45fe-192">Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników w każdym żądaniu może spowodować spadek wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="f45fe-193">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="f45fe-193">Persistent cookies</span></span>

<span data-ttu-id="f45fe-194">Plik cookie może być trwały między sesjami przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f45fe-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="f45fe-195">Tę trwałość należy włączyć tylko w przypadku jawnej zgody użytkownika przy użyciu pola wyboru "Zapamiętaj mnie" przy logowaniu lub podobnym mechanizmie.</span><span class="sxs-lookup"><span data-stu-id="f45fe-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="f45fe-196">Poniższy fragment kodu tworzy tożsamość i odpowiadający jej plik cookie, który przeżyje przez zamknięcia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f45fe-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="f45fe-197">Wszystkie skonfigurowane wcześniej ustawienia wygasania są honorowane.</span><span class="sxs-lookup"><span data-stu-id="f45fe-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="f45fe-198">Jeśli plik cookie wygaśnie, gdy przeglądarka zostanie ZAMKNIĘTA, przeglądarka czyści plik cookie po jego ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="f45fe-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="f45fe-199">Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> na `true` w <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="f45fe-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="f45fe-200">Bezwzględne wygaśnięcie pliku cookie</span><span class="sxs-lookup"><span data-stu-id="f45fe-200">Absolute cookie expiration</span></span>

<span data-ttu-id="f45fe-201">Bezwzględny czas wygaśnięcia można ustawić za pomocą <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="f45fe-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="f45fe-202">Aby utworzyć trwały plik cookie, należy również ustawić `IsPersistent`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="f45fe-203">W przeciwnym razie plik cookie jest tworzony przy użyciu okresu istnienia sesji i może wygasnąć przed lub po posiadanym przez niego biletem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="f45fe-204">Gdy `ExpiresUtc` jest ustawiona, zastępuje wartość opcji <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f45fe-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="f45fe-205">Poniższy fragment kodu tworzy tożsamość i odpowiedni plik cookie, który trwa przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="f45fe-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="f45fe-206">Ignoruje to wszystkie ustawienia wygasania, które zostały wcześniej skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="f45fe-206">This ignores any sliding expiration settings previously configured.</span></span>

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

<span data-ttu-id="f45fe-207">ASP.NET Core Identity to kompletny, w pełni funkcjonalny Dostawca uwierzytelniania do tworzenia i obsługiwania logowań.</span><span class="sxs-lookup"><span data-stu-id="f45fe-207">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="f45fe-208">Jednak można użyć dostawcy uwierzytelniania uwierzytelniania opartego na plikach cookie bez tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f45fe-208">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="f45fe-209">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="f45fe-209">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="f45fe-210">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f45fe-210">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f45fe-211">W celach demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika, Maria Rodriguez jest stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-211">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="f45fe-212">Użyj adresu **e-mail** `maria.rodriguez@contoso.com` i hasła do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f45fe-212">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="f45fe-213">Użytkownik jest uwierzytelniany w metodzie `AuthenticateUser` w pliku *Pages/Account/Login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="f45fe-213">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="f45fe-214">W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f45fe-214">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="f45fe-215">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="f45fe-215">Configuration</span></span>

<span data-ttu-id="f45fe-216">Jeśli aplikacja nie korzysta z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla pakietu [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="f45fe-216">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="f45fe-217">W metodzie `Startup.ConfigureServices` Utwórz usługę pośredniczącą uwierzytelniania przy użyciu metod <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:</span><span class="sxs-lookup"><span data-stu-id="f45fe-217">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="f45fe-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> przeszedł do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="f45fe-219">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzować z określonym schematem](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="f45fe-219">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="f45fe-220">Ustawienie `AuthenticationScheme` na [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) zapewnia wartość "cookies" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="f45fe-220">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="f45fe-221">Można podać dowolną wartość ciągu odróżniającą schemat.</span><span class="sxs-lookup"><span data-stu-id="f45fe-221">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="f45fe-222">Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania plików cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-222">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="f45fe-223">Gdy schemat uwierzytelniania plików cookie nie jest dostarczany do <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa `CookieAuthenticationDefaults.AuthenticationScheme` ("pliki cookie").</span><span class="sxs-lookup"><span data-stu-id="f45fe-223">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="f45fe-224">Właściwość <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> pliku cookie uwierzytelniania jest domyślnie ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-224">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="f45fe-225">Pliki cookie uwierzytelniania są dozwolone, gdy osoby odwiedzające witrynę nie wyraziły zgody na zbieranie danych.</span><span class="sxs-lookup"><span data-stu-id="f45fe-225">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="f45fe-226">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="f45fe-226">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="f45fe-227">W metodzie `Startup.Configure` Wywołaj metodę `UseAuthentication`, aby wywołać oprogramowanie pośredniczące uwierzytelniania, które ustawia właściwość `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-227">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="f45fe-228">Wywołaj metodę `UseAuthentication` przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="f45fe-228">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="f45fe-229">Klasa <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> służy do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-229">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="f45fe-230">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi na potrzeby uwierzytelniania w `Startup.ConfigureServices` metodzie:</span><span class="sxs-lookup"><span data-stu-id="f45fe-230">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="f45fe-231">Oprogramowanie pośredniczące zasad dotyczących plików cookie</span><span class="sxs-lookup"><span data-stu-id="f45fe-231">Cookie Policy Middleware</span></span>

<span data-ttu-id="f45fe-232">[Oprogramowanie pośredniczące zasad plików cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) umożliwia korzystanie z funkcji zasad dotyczących plików cookie.</span><span class="sxs-lookup"><span data-stu-id="f45fe-232">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="f45fe-233">Dodanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest zależne od kolejności&mdash;ma wpływ tylko na składniki podrzędne zarejestrowane w potoku.</span><span class="sxs-lookup"><span data-stu-id="f45fe-233">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="f45fe-234">Użyj <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> dostarczanego do programu pośredniczącego zasad dotyczących plików cookie, aby kontrolować globalne właściwości przetwarzania plików cookie i podłączać się do programów obsługi przetwarzania plików cookie, gdy pliki cookie są dodawane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="f45fe-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="f45fe-235">Domyślna wartość <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> jest `SameSiteMode.Lax`, aby zezwalać na uwierzytelnianie OAuth2.</span><span class="sxs-lookup"><span data-stu-id="f45fe-235">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="f45fe-236">Aby ściśle wymusić zasady dotyczące tej samej lokacji `SameSiteMode.Strict`, ustaw `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-236">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="f45fe-237">Mimo że to ustawienie powoduje przerwanie OAuth2 i innych schematów uwierzytelniania między źródłami, podnosi poziom bezpieczeństwa plików cookie dla innych typów aplikacji, które nie polegają na przetwarzaniu żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="f45fe-237">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="f45fe-238">Ustawienie ustawienia pośredniczące zasad plików cookie dla `MinimumSameSitePolicy` może wpływać na ustawienie `Cookie.SameSite` w ustawieniach `CookieAuthenticationOptions` zgodnie z poniższą macierzą.</span><span class="sxs-lookup"><span data-stu-id="f45fe-238">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="f45fe-239">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="f45fe-239">MinimumSameSitePolicy</span></span> | <span data-ttu-id="f45fe-240">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="f45fe-240">Cookie.SameSite</span></span> | <span data-ttu-id="f45fe-241">Wynikowy plik cookie. SameSite ustawienie</span><span class="sxs-lookup"><span data-stu-id="f45fe-241">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="f45fe-242">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-242">SameSiteMode.None</span></span>     | <span data-ttu-id="f45fe-243">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-243">SameSiteMode.None</span></span><br><span data-ttu-id="f45fe-244">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-244">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-245">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-245">SameSiteMode.Strict</span></span> | <span data-ttu-id="f45fe-246">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-246">SameSiteMode.None</span></span><br><span data-ttu-id="f45fe-247">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-247">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-248">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-248">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="f45fe-249">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-249">SameSiteMode.Lax</span></span>      | <span data-ttu-id="f45fe-250">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-250">SameSiteMode.None</span></span><br><span data-ttu-id="f45fe-251">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-251">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-252">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-252">SameSiteMode.Strict</span></span> | <span data-ttu-id="f45fe-253">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-254">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-254">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-255">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-255">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="f45fe-256">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-256">SameSiteMode.Strict</span></span>   | <span data-ttu-id="f45fe-257">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="f45fe-257">SameSiteMode.None</span></span><br><span data-ttu-id="f45fe-258">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="f45fe-258">SameSiteMode.Lax</span></span><br><span data-ttu-id="f45fe-259">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-259">SameSiteMode.Strict</span></span> | <span data-ttu-id="f45fe-260">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-260">SameSiteMode.Strict</span></span><br><span data-ttu-id="f45fe-261">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-261">SameSiteMode.Strict</span></span><br><span data-ttu-id="f45fe-262">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="f45fe-262">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="f45fe-263">Utwórz plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="f45fe-263">Create an authentication cookie</span></span>

<span data-ttu-id="f45fe-264">Aby utworzyć plik cookie przechowujący informacje o użytkowniku, Utwórz <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="f45fe-264">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="f45fe-265">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="f45fe-265">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="f45fe-266">Utwórz <xref:System.Security.Claims.ClaimsIdentity> z dowolnym wymaganym <xref:System.Security.Claims.Claim>s i Wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>, aby zalogować użytkownika:</span><span class="sxs-lookup"><span data-stu-id="f45fe-266">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="f45fe-267">`SignInAsync` tworzy zaszyfrowany plik cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f45fe-267">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="f45fe-268">Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="f45fe-268">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="f45fe-269">System [ochrony danych](xref:security/data-protection/using-data-protection) ASP.NET Core jest używany do szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-269">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="f45fe-270">W przypadku aplikacji hostowanej na wielu komputerach, równoważenia obciążenia między aplikacjami lub korzystania z kolektywu serwerów sieci Web należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) tak, aby używała tego samego dzwonka klucza i identyfikatora aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-270">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="f45fe-271">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="f45fe-271">Sign out</span></span>

<span data-ttu-id="f45fe-272">Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="f45fe-272">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="f45fe-273">Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "cookies") nie jest używany jako schemat (na przykład "ContosoCookie"), należy podać schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-273">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="f45fe-274">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="f45fe-274">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="f45fe-275">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="f45fe-275">React to back-end changes</span></span>

<span data-ttu-id="f45fe-276">Po utworzeniu pliku cookie plik cookie jest pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f45fe-276">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="f45fe-277">Jeśli konto użytkownika jest wyłączone w systemach zaplecza:</span><span class="sxs-lookup"><span data-stu-id="f45fe-277">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="f45fe-278">System uwierzytelniania plików cookie aplikacji kontynuuje przetwarzanie żądań na podstawie pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-278">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="f45fe-279">Użytkownik pozostaje zalogowany do aplikacji, o ile plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="f45fe-279">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="f45fe-280">Zdarzenia <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> mogą służyć do przechwycenia i zastąpienia weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="f45fe-280">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="f45fe-281">Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko odwołujący się użytkowników, którzy uzyskują dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-281">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="f45fe-282">Jedno z metod weryfikacji plików cookie polega na śledzeniu zmian w bazie danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f45fe-282">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="f45fe-283">Jeśli baza danych nie została zmieniona od czasu wystawienia pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="f45fe-283">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="f45fe-284">W przykładowej aplikacji baza danych jest zaimplementowana w `IUserRepository` i przechowuje `LastChanged` wartość.</span><span class="sxs-lookup"><span data-stu-id="f45fe-284">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="f45fe-285">Gdy użytkownik zostanie zaktualizowany w bazie danych, wartość `LastChanged` jest ustawiana na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="f45fe-285">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="f45fe-286">Aby unieważnić plik cookie, gdy baza danych zmienia się w oparciu o wartość `LastChanged`, Utwórz plik cookie z `LastChanged`m, zawierającym wartość bieżącą `LastChanged` z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="f45fe-286">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="f45fe-287">Aby zaimplementować przesłonięcie dla zdarzenia `ValidatePrincipal`, napisz metodę o następującej sygnaturze w klasie, która pochodzi od <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="f45fe-287">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="f45fe-288">Poniżej przedstawiono przykładową implementację `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="f45fe-288">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="f45fe-289">Zarejestruj wystąpienie zdarzeń podczas rejestracji usługi plików cookie w metodzie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-289">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f45fe-290">Podaj [rejestrację usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes) dla swojej klasy `CustomCookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="f45fe-290">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="f45fe-291">Rozważ sytuację, w której nazwa użytkownika jest aktualizowana&mdash;decyzji, która nie ma wpływu na zabezpieczenia w jakikolwiek sposób.</span><span class="sxs-lookup"><span data-stu-id="f45fe-291">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="f45fe-292">Jeśli chcesz nieszkodliwej aktualizacji podmiotu zabezpieczeń, wywołaj `context.ReplacePrincipal` i ustaw właściwość `context.ShouldRenew` na `true`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-292">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="f45fe-293">Opisane tutaj podejście jest wyzwalane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-293">The approach described here is triggered on every request.</span></span> <span data-ttu-id="f45fe-294">Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników w każdym żądaniu może spowodować spadek wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45fe-294">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="f45fe-295">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="f45fe-295">Persistent cookies</span></span>

<span data-ttu-id="f45fe-296">Plik cookie może być trwały między sesjami przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f45fe-296">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="f45fe-297">Tę trwałość należy włączyć tylko w przypadku jawnej zgody użytkownika przy użyciu pola wyboru "Zapamiętaj mnie" przy logowaniu lub podobnym mechanizmie.</span><span class="sxs-lookup"><span data-stu-id="f45fe-297">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="f45fe-298">Poniższy fragment kodu tworzy tożsamość i odpowiadający jej plik cookie, który przeżyje przez zamknięcia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f45fe-298">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="f45fe-299">Wszystkie skonfigurowane wcześniej ustawienia wygasania są honorowane.</span><span class="sxs-lookup"><span data-stu-id="f45fe-299">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="f45fe-300">Jeśli plik cookie wygaśnie, gdy przeglądarka zostanie ZAMKNIĘTA, przeglądarka czyści plik cookie po jego ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="f45fe-300">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="f45fe-301">Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> na `true` w <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="f45fe-301">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="f45fe-302">Bezwzględne wygaśnięcie pliku cookie</span><span class="sxs-lookup"><span data-stu-id="f45fe-302">Absolute cookie expiration</span></span>

<span data-ttu-id="f45fe-303">Bezwzględny czas wygaśnięcia można ustawić za pomocą <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="f45fe-303">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="f45fe-304">Aby utworzyć trwały plik cookie, należy również ustawić `IsPersistent`.</span><span class="sxs-lookup"><span data-stu-id="f45fe-304">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="f45fe-305">W przeciwnym razie plik cookie jest tworzony przy użyciu okresu istnienia sesji i może wygasnąć przed lub po posiadanym przez niego biletem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f45fe-305">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="f45fe-306">Gdy `ExpiresUtc` jest ustawiona, zastępuje wartość opcji <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f45fe-306">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="f45fe-307">Poniższy fragment kodu tworzy tożsamość i odpowiedni plik cookie, który trwa przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="f45fe-307">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="f45fe-308">Ignoruje to wszystkie ustawienia wygasania, które zostały wcześniej skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="f45fe-308">This ignores any sliding expiration settings previously configured.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="f45fe-309">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f45fe-309">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="f45fe-310">Kontrole ról oparte na zasadach</span><span class="sxs-lookup"><span data-stu-id="f45fe-310">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
