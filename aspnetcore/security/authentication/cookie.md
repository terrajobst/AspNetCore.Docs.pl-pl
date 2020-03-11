---
title: Korzystanie z uwierzytelniania plików cookie bez tożsamości ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać uwierzytelniania przy użyciu plików cookie bez tożsamości ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: 64f881441a7a7f9a5529cb6ee5ce81142ccd69e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663003"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="84c6d-103">Korzystanie z uwierzytelniania plików cookie bez tożsamości ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84c6d-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="84c6d-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84c6d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="84c6d-105">ASP.NET Core Identity to kompletny, w pełni funkcjonalny Dostawca uwierzytelniania do tworzenia i obsługiwania logowań.</span><span class="sxs-lookup"><span data-stu-id="84c6d-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="84c6d-106">Jednak można użyć dostawcy uwierzytelniania opartego na plikach cookie bez tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84c6d-106">However, a cookie-based authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="84c6d-107">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="84c6d-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="84c6d-108">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84c6d-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="84c6d-109">W celach demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika, Maria Rodriguez jest stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="84c6d-110">Użyj adresu **e-mail** `maria.rodriguez@contoso.com` i hasła do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="84c6d-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="84c6d-111">Użytkownik jest uwierzytelniany w metodzie `AuthenticateUser` w pliku *Pages/Account/Login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="84c6d-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="84c6d-112">W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="84c6d-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="84c6d-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="84c6d-113">Configuration</span></span>

<span data-ttu-id="84c6d-114">Jeśli aplikacja nie korzysta z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla pakietu [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="84c6d-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="84c6d-115">W metodzie `Startup.ConfigureServices` Utwórz usługi pośredniczące uwierzytelniania przy użyciu metod <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:</span><span class="sxs-lookup"><span data-stu-id="84c6d-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="84c6d-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> przeszedł do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="84c6d-117">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzować z określonym schematem](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="84c6d-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="84c6d-118">Ustawienie `AuthenticationScheme` na [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) zapewnia wartość "cookies" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="84c6d-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="84c6d-119">Można podać dowolną wartość ciągu odróżniającą schemat.</span><span class="sxs-lookup"><span data-stu-id="84c6d-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="84c6d-120">Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania plików cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="84c6d-121">Gdy schemat uwierzytelniania plików cookie nie jest dostarczany do <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa `CookieAuthenticationDefaults.AuthenticationScheme` ("pliki cookie").</span><span class="sxs-lookup"><span data-stu-id="84c6d-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="84c6d-122">Właściwość <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> pliku cookie uwierzytelniania jest domyślnie ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="84c6d-123">Pliki cookie uwierzytelniania są dozwolone, gdy osoby odwiedzające witrynę nie wyraziły zgody na zbieranie danych.</span><span class="sxs-lookup"><span data-stu-id="84c6d-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="84c6d-124">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="84c6d-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="84c6d-125">W `Startup.Configure`Wywołaj `UseAuthentication` i `UseAuthorization`, aby ustawić właściwość `HttpContext.User` i uruchomić oprogramowanie pośredniczące autoryzacji dla żądań.</span><span class="sxs-lookup"><span data-stu-id="84c6d-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="84c6d-126">Przed wywołaniem `UseEndpoints`wywołaj metody `UseAuthentication` i `UseAuthorization`:</span><span class="sxs-lookup"><span data-stu-id="84c6d-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="84c6d-127">Klasa <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> służy do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="84c6d-128">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi na potrzeby uwierzytelniania w `Startup.ConfigureServices` metodzie:</span><span class="sxs-lookup"><span data-stu-id="84c6d-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="84c6d-129">Oprogramowanie pośredniczące zasad dotyczących plików cookie</span><span class="sxs-lookup"><span data-stu-id="84c6d-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="84c6d-130">[Oprogramowanie pośredniczące zasad plików cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) umożliwia korzystanie z funkcji zasad dotyczących plików cookie.</span><span class="sxs-lookup"><span data-stu-id="84c6d-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="84c6d-131">Dodanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest zależne od kolejności&mdash;ma wpływ tylko na składniki podrzędne zarejestrowane w potoku.</span><span class="sxs-lookup"><span data-stu-id="84c6d-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="84c6d-132">Użyj <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> dostarczanego do programu pośredniczącego zasad dotyczących plików cookie, aby kontrolować globalne właściwości przetwarzania plików cookie i podłączać się do programów obsługi przetwarzania plików cookie, gdy pliki cookie są dodawane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="84c6d-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="84c6d-133">Domyślna wartość <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> jest `SameSiteMode.Lax`, aby zezwalać na uwierzytelnianie OAuth2.</span><span class="sxs-lookup"><span data-stu-id="84c6d-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="84c6d-134">Aby ściśle wymusić zasady dotyczące tej samej lokacji `SameSiteMode.Strict`, ustaw `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="84c6d-135">Mimo że to ustawienie powoduje przerwanie OAuth2 i innych schematów uwierzytelniania między źródłami, podnosi poziom bezpieczeństwa plików cookie dla innych typów aplikacji, które nie polegają na przetwarzaniu żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="84c6d-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="84c6d-136">Ustawienie ustawienia pośredniczące zasad plików cookie dla `MinimumSameSitePolicy` może wpływać na ustawienie `Cookie.SameSite` w ustawieniach `CookieAuthenticationOptions` zgodnie z poniższą macierzą.</span><span class="sxs-lookup"><span data-stu-id="84c6d-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="84c6d-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="84c6d-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="84c6d-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="84c6d-138">Cookie.SameSite</span></span> | <span data-ttu-id="84c6d-139">Wynikowy plik cookie. SameSite ustawienie</span><span class="sxs-lookup"><span data-stu-id="84c6d-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="84c6d-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-140">SameSiteMode.None</span></span>     | <span data-ttu-id="84c6d-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-141">SameSiteMode.None</span></span><br><span data-ttu-id="84c6d-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="84c6d-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-144">SameSiteMode.None</span></span><br><span data-ttu-id="84c6d-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="84c6d-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="84c6d-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-148">SameSiteMode.None</span></span><br><span data-ttu-id="84c6d-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="84c6d-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="84c6d-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="84c6d-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-155">SameSiteMode.None</span></span><br><span data-ttu-id="84c6d-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="84c6d-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="84c6d-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="84c6d-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="84c6d-161">Utwórz plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="84c6d-161">Create an authentication cookie</span></span>

<span data-ttu-id="84c6d-162">Aby utworzyć plik cookie przechowujący informacje o użytkowniku, Utwórz <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="84c6d-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="84c6d-163">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="84c6d-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="84c6d-164">Utwórz <xref:System.Security.Claims.ClaimsIdentity> z dowolnym wymaganym <xref:System.Security.Claims.Claim>s i Wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>, aby zalogować użytkownika:</span><span class="sxs-lookup"><span data-stu-id="84c6d-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="84c6d-165">`SignInAsync` tworzy zaszyfrowany plik cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="84c6d-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="84c6d-166">Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="84c6d-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="84c6d-167">System [ochrony danych](xref:security/data-protection/using-data-protection) ASP.NET Core jest używany do szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="84c6d-168">W przypadku aplikacji hostowanej na wielu komputerach, równoważenia obciążenia między aplikacjami lub korzystania z kolektywu serwerów sieci Web należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) tak, aby używała tego samego dzwonka klucza i identyfikatora aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="84c6d-169">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="84c6d-169">Sign out</span></span>

<span data-ttu-id="84c6d-170">Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="84c6d-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="84c6d-171">Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "cookies") nie jest używany jako schemat (na przykład "ContosoCookie"), należy podać schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="84c6d-172">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="84c6d-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="84c6d-173">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="84c6d-173">React to back-end changes</span></span>

<span data-ttu-id="84c6d-174">Po utworzeniu pliku cookie plik cookie jest pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="84c6d-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="84c6d-175">Jeśli konto użytkownika jest wyłączone w systemach zaplecza:</span><span class="sxs-lookup"><span data-stu-id="84c6d-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="84c6d-176">System uwierzytelniania plików cookie aplikacji kontynuuje przetwarzanie żądań na podstawie pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="84c6d-177">Użytkownik pozostaje zalogowany do aplikacji, o ile plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="84c6d-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="84c6d-178">Zdarzenia <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> mogą służyć do przechwycenia i zastąpienia weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="84c6d-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="84c6d-179">Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko odwołujący się użytkowników, którzy uzyskują dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="84c6d-180">Jedno z metod weryfikacji plików cookie polega na śledzeniu zmian w bazie danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="84c6d-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="84c6d-181">Jeśli baza danych nie została zmieniona od czasu wystawienia pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="84c6d-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="84c6d-182">W przykładowej aplikacji baza danych jest zaimplementowana w `IUserRepository` i przechowuje `LastChanged` wartość.</span><span class="sxs-lookup"><span data-stu-id="84c6d-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="84c6d-183">Gdy użytkownik zostanie zaktualizowany w bazie danych, wartość `LastChanged` jest ustawiana na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="84c6d-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="84c6d-184">Aby unieważnić plik cookie, gdy baza danych zmienia się w oparciu o wartość `LastChanged`, Utwórz plik cookie z `LastChanged`m, zawierającym wartość bieżącą `LastChanged` z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="84c6d-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="84c6d-185">Aby zaimplementować przesłonięcie dla zdarzenia `ValidatePrincipal`, napisz metodę o następującej sygnaturze w klasie, która pochodzi od <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="84c6d-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="84c6d-186">Poniżej przedstawiono przykładową implementację `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="84c6d-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="84c6d-187">Zarejestruj wystąpienie zdarzeń podczas rejestracji usługi plików cookie w metodzie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="84c6d-188">Podaj [rejestrację usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes) dla swojej klasy `CustomCookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="84c6d-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="84c6d-189">Rozważ sytuację, w której nazwa użytkownika jest aktualizowana&mdash;decyzji, która nie ma wpływu na zabezpieczenia w jakikolwiek sposób.</span><span class="sxs-lookup"><span data-stu-id="84c6d-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="84c6d-190">Jeśli chcesz nieszkodliwej aktualizacji podmiotu zabezpieczeń, wywołaj `context.ReplacePrincipal` i ustaw właściwość `context.ShouldRenew` na `true`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="84c6d-191">Opisane tutaj podejście jest wyzwalane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="84c6d-192">Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników w każdym żądaniu może spowodować spadek wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="84c6d-193">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="84c6d-193">Persistent cookies</span></span>

<span data-ttu-id="84c6d-194">Plik cookie może być trwały między sesjami przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="84c6d-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="84c6d-195">Tę trwałość należy włączyć tylko w przypadku jawnej zgody użytkownika przy użyciu pola wyboru "Zapamiętaj mnie" przy logowaniu lub podobnym mechanizmie.</span><span class="sxs-lookup"><span data-stu-id="84c6d-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="84c6d-196">Poniższy fragment kodu tworzy tożsamość i odpowiadający jej plik cookie, który przeżyje przez zamknięcia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="84c6d-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="84c6d-197">Wszystkie skonfigurowane wcześniej ustawienia wygasania są honorowane.</span><span class="sxs-lookup"><span data-stu-id="84c6d-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="84c6d-198">Jeśli plik cookie wygaśnie, gdy przeglądarka zostanie ZAMKNIĘTA, przeglądarka czyści plik cookie po jego ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="84c6d-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="84c6d-199">Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> na `true` w <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="84c6d-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="84c6d-200">Bezwzględne wygaśnięcie pliku cookie</span><span class="sxs-lookup"><span data-stu-id="84c6d-200">Absolute cookie expiration</span></span>

<span data-ttu-id="84c6d-201">Bezwzględny czas wygaśnięcia można ustawić za pomocą <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="84c6d-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="84c6d-202">Aby utworzyć trwały plik cookie, należy również ustawić `IsPersistent`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="84c6d-203">W przeciwnym razie plik cookie jest tworzony przy użyciu okresu istnienia sesji i może wygasnąć przed lub po posiadanym przez niego biletem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="84c6d-204">Gdy `ExpiresUtc` jest ustawiona, zastępuje wartość opcji <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="84c6d-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="84c6d-205">Poniższy fragment kodu tworzy tożsamość i odpowiedni plik cookie, który trwa przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="84c6d-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="84c6d-206">Ignoruje to wszystkie ustawienia wygasania, które zostały wcześniej skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="84c6d-206">This ignores any sliding expiration settings previously configured.</span></span>

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

<span data-ttu-id="84c6d-207">ASP.NET Core Identity to kompletny, w pełni funkcjonalny Dostawca uwierzytelniania do tworzenia i obsługiwania logowań.</span><span class="sxs-lookup"><span data-stu-id="84c6d-207">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="84c6d-208">Jednak można użyć dostawcy uwierzytelniania uwierzytelniania opartego na plikach cookie bez tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84c6d-208">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="84c6d-209">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="84c6d-209">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="84c6d-210">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84c6d-210">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="84c6d-211">W celach demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika, Maria Rodriguez jest stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-211">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="84c6d-212">Użyj adresu **e-mail** `maria.rodriguez@contoso.com` i hasła do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="84c6d-212">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="84c6d-213">Użytkownik jest uwierzytelniany w metodzie `AuthenticateUser` w pliku *Pages/Account/Login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="84c6d-213">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="84c6d-214">W świecie rzeczywistym użytkownik zostanie uwierzytelniony w odniesieniu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="84c6d-214">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="84c6d-215">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="84c6d-215">Configuration</span></span>

<span data-ttu-id="84c6d-216">Jeśli aplikacja nie korzysta z [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla pakietu [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="84c6d-216">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="84c6d-217">W metodzie `Startup.ConfigureServices` Utwórz usługę pośredniczącą uwierzytelniania przy użyciu metod <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:</span><span class="sxs-lookup"><span data-stu-id="84c6d-217">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="84c6d-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> przeszedł do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="84c6d-219">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzować z określonym schematem](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="84c6d-219">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="84c6d-220">Ustawienie `AuthenticationScheme` na [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) zapewnia wartość "cookies" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="84c6d-220">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="84c6d-221">Można podać dowolną wartość ciągu odróżniającą schemat.</span><span class="sxs-lookup"><span data-stu-id="84c6d-221">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="84c6d-222">Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania plików cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-222">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="84c6d-223">Gdy schemat uwierzytelniania plików cookie nie jest dostarczany do <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa `CookieAuthenticationDefaults.AuthenticationScheme` ("pliki cookie").</span><span class="sxs-lookup"><span data-stu-id="84c6d-223">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="84c6d-224">Właściwość <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> pliku cookie uwierzytelniania jest domyślnie ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-224">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="84c6d-225">Pliki cookie uwierzytelniania są dozwolone, gdy osoby odwiedzające witrynę nie wyraziły zgody na zbieranie danych.</span><span class="sxs-lookup"><span data-stu-id="84c6d-225">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="84c6d-226">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="84c6d-226">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="84c6d-227">W metodzie `Startup.Configure` Wywołaj metodę `UseAuthentication`, aby wywołać oprogramowanie pośredniczące uwierzytelniania, które ustawia właściwość `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-227">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="84c6d-228">Wywołaj metodę `UseAuthentication` przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="84c6d-228">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="84c6d-229">Klasa <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> służy do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-229">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="84c6d-230">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi na potrzeby uwierzytelniania w `Startup.ConfigureServices` metodzie:</span><span class="sxs-lookup"><span data-stu-id="84c6d-230">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="84c6d-231">Oprogramowanie pośredniczące zasad dotyczących plików cookie</span><span class="sxs-lookup"><span data-stu-id="84c6d-231">Cookie Policy Middleware</span></span>

<span data-ttu-id="84c6d-232">[Oprogramowanie pośredniczące zasad plików cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) umożliwia korzystanie z funkcji zasad dotyczących plików cookie.</span><span class="sxs-lookup"><span data-stu-id="84c6d-232">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="84c6d-233">Dodanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest zależne od kolejności&mdash;ma wpływ tylko na składniki podrzędne zarejestrowane w potoku.</span><span class="sxs-lookup"><span data-stu-id="84c6d-233">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="84c6d-234">Użyj <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> dostarczanego do programu pośredniczącego zasad dotyczących plików cookie, aby kontrolować globalne właściwości przetwarzania plików cookie i podłączać się do programów obsługi przetwarzania plików cookie, gdy pliki cookie są dodawane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="84c6d-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="84c6d-235">Domyślna wartość <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> jest `SameSiteMode.Lax`, aby zezwalać na uwierzytelnianie OAuth2.</span><span class="sxs-lookup"><span data-stu-id="84c6d-235">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="84c6d-236">Aby ściśle wymusić zasady dotyczące tej samej lokacji `SameSiteMode.Strict`, ustaw `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-236">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="84c6d-237">Mimo że to ustawienie powoduje przerwanie OAuth2 i innych schematów uwierzytelniania między źródłami, podnosi poziom bezpieczeństwa plików cookie dla innych typów aplikacji, które nie polegają na przetwarzaniu żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="84c6d-237">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="84c6d-238">Ustawienie ustawienia pośredniczące zasad plików cookie dla `MinimumSameSitePolicy` może wpływać na ustawienie `Cookie.SameSite` w ustawieniach `CookieAuthenticationOptions` zgodnie z poniższą macierzą.</span><span class="sxs-lookup"><span data-stu-id="84c6d-238">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="84c6d-239">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="84c6d-239">MinimumSameSitePolicy</span></span> | <span data-ttu-id="84c6d-240">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="84c6d-240">Cookie.SameSite</span></span> | <span data-ttu-id="84c6d-241">Wynikowy plik cookie. SameSite ustawienie</span><span class="sxs-lookup"><span data-stu-id="84c6d-241">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="84c6d-242">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-242">SameSiteMode.None</span></span>     | <span data-ttu-id="84c6d-243">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-243">SameSiteMode.None</span></span><br><span data-ttu-id="84c6d-244">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-244">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-245">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-245">SameSiteMode.Strict</span></span> | <span data-ttu-id="84c6d-246">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-246">SameSiteMode.None</span></span><br><span data-ttu-id="84c6d-247">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-247">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-248">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-248">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="84c6d-249">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-249">SameSiteMode.Lax</span></span>      | <span data-ttu-id="84c6d-250">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-250">SameSiteMode.None</span></span><br><span data-ttu-id="84c6d-251">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-251">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-252">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-252">SameSiteMode.Strict</span></span> | <span data-ttu-id="84c6d-253">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-254">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-254">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-255">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-255">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="84c6d-256">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-256">SameSiteMode.Strict</span></span>   | <span data-ttu-id="84c6d-257">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="84c6d-257">SameSiteMode.None</span></span><br><span data-ttu-id="84c6d-258">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="84c6d-258">SameSiteMode.Lax</span></span><br><span data-ttu-id="84c6d-259">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-259">SameSiteMode.Strict</span></span> | <span data-ttu-id="84c6d-260">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-260">SameSiteMode.Strict</span></span><br><span data-ttu-id="84c6d-261">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-261">SameSiteMode.Strict</span></span><br><span data-ttu-id="84c6d-262">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="84c6d-262">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="84c6d-263">Utwórz plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="84c6d-263">Create an authentication cookie</span></span>

<span data-ttu-id="84c6d-264">Aby utworzyć plik cookie przechowujący informacje o użytkowniku, Utwórz <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="84c6d-264">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="84c6d-265">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="84c6d-265">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="84c6d-266">Utwórz <xref:System.Security.Claims.ClaimsIdentity> z dowolnym wymaganym <xref:System.Security.Claims.Claim>s i Wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>, aby zalogować użytkownika:</span><span class="sxs-lookup"><span data-stu-id="84c6d-266">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="84c6d-267">`SignInAsync` tworzy zaszyfrowany plik cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="84c6d-267">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="84c6d-268">Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="84c6d-268">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="84c6d-269">System [ochrony danych](xref:security/data-protection/using-data-protection) ASP.NET Core jest używany do szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-269">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="84c6d-270">W przypadku aplikacji hostowanej na wielu komputerach, równoważenia obciążenia między aplikacjami lub korzystania z kolektywu serwerów sieci Web należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) tak, aby używała tego samego dzwonka klucza i identyfikatora aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-270">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="84c6d-271">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="84c6d-271">Sign out</span></span>

<span data-ttu-id="84c6d-272">Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="84c6d-272">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="84c6d-273">Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "cookies") nie jest używany jako schemat (na przykład "ContosoCookie"), należy podać schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-273">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="84c6d-274">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="84c6d-274">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="84c6d-275">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="84c6d-275">React to back-end changes</span></span>

<span data-ttu-id="84c6d-276">Po utworzeniu pliku cookie plik cookie jest pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="84c6d-276">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="84c6d-277">Jeśli konto użytkownika jest wyłączone w systemach zaplecza:</span><span class="sxs-lookup"><span data-stu-id="84c6d-277">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="84c6d-278">System uwierzytelniania plików cookie aplikacji kontynuuje przetwarzanie żądań na podstawie pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-278">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="84c6d-279">Użytkownik pozostaje zalogowany do aplikacji, o ile plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="84c6d-279">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="84c6d-280">Zdarzenia <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> mogą służyć do przechwycenia i zastąpienia weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="84c6d-280">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="84c6d-281">Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko odwołujący się użytkowników, którzy uzyskują dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-281">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="84c6d-282">Jedno z metod weryfikacji plików cookie polega na śledzeniu zmian w bazie danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="84c6d-282">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="84c6d-283">Jeśli baza danych nie została zmieniona od czasu wystawienia pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="84c6d-283">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="84c6d-284">W przykładowej aplikacji baza danych jest zaimplementowana w `IUserRepository` i przechowuje `LastChanged` wartość.</span><span class="sxs-lookup"><span data-stu-id="84c6d-284">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="84c6d-285">Gdy użytkownik zostanie zaktualizowany w bazie danych, wartość `LastChanged` jest ustawiana na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="84c6d-285">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="84c6d-286">Aby unieważnić plik cookie, gdy baza danych zmienia się w oparciu o wartość `LastChanged`, Utwórz plik cookie z `LastChanged`m, zawierającym wartość bieżącą `LastChanged` z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="84c6d-286">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="84c6d-287">Aby zaimplementować przesłonięcie dla zdarzenia `ValidatePrincipal`, napisz metodę o następującej sygnaturze w klasie, która pochodzi od <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="84c6d-287">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="84c6d-288">Poniżej przedstawiono przykładową implementację `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="84c6d-288">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="84c6d-289">Zarejestruj wystąpienie zdarzeń podczas rejestracji usługi plików cookie w metodzie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-289">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="84c6d-290">Podaj [rejestrację usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes) dla swojej klasy `CustomCookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="84c6d-290">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="84c6d-291">Rozważ sytuację, w której nazwa użytkownika jest aktualizowana&mdash;decyzji, która nie ma wpływu na zabezpieczenia w jakikolwiek sposób.</span><span class="sxs-lookup"><span data-stu-id="84c6d-291">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="84c6d-292">Jeśli chcesz nieszkodliwej aktualizacji podmiotu zabezpieczeń, wywołaj `context.ReplacePrincipal` i ustaw właściwość `context.ShouldRenew` na `true`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-292">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="84c6d-293">Opisane tutaj podejście jest wyzwalane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-293">The approach described here is triggered on every request.</span></span> <span data-ttu-id="84c6d-294">Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników w każdym żądaniu może spowodować spadek wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84c6d-294">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="84c6d-295">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="84c6d-295">Persistent cookies</span></span>

<span data-ttu-id="84c6d-296">Plik cookie może być trwały między sesjami przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="84c6d-296">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="84c6d-297">Tę trwałość należy włączyć tylko w przypadku jawnej zgody użytkownika przy użyciu pola wyboru "Zapamiętaj mnie" przy logowaniu lub podobnym mechanizmie.</span><span class="sxs-lookup"><span data-stu-id="84c6d-297">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="84c6d-298">Poniższy fragment kodu tworzy tożsamość i odpowiadający jej plik cookie, który przeżyje przez zamknięcia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="84c6d-298">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="84c6d-299">Wszystkie skonfigurowane wcześniej ustawienia wygasania są honorowane.</span><span class="sxs-lookup"><span data-stu-id="84c6d-299">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="84c6d-300">Jeśli plik cookie wygaśnie, gdy przeglądarka zostanie ZAMKNIĘTA, przeglądarka czyści plik cookie po jego ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="84c6d-300">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="84c6d-301">Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> na `true` w <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="84c6d-301">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="84c6d-302">Bezwzględne wygaśnięcie pliku cookie</span><span class="sxs-lookup"><span data-stu-id="84c6d-302">Absolute cookie expiration</span></span>

<span data-ttu-id="84c6d-303">Bezwzględny czas wygaśnięcia można ustawić za pomocą <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="84c6d-303">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="84c6d-304">Aby utworzyć trwały plik cookie, należy również ustawić `IsPersistent`.</span><span class="sxs-lookup"><span data-stu-id="84c6d-304">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="84c6d-305">W przeciwnym razie plik cookie jest tworzony przy użyciu okresu istnienia sesji i może wygasnąć przed lub po posiadanym przez niego biletem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="84c6d-305">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="84c6d-306">Gdy `ExpiresUtc` jest ustawiona, zastępuje wartość opcji <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="84c6d-306">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="84c6d-307">Poniższy fragment kodu tworzy tożsamość i odpowiedni plik cookie, który trwa przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="84c6d-307">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="84c6d-308">Ignoruje to wszystkie ustawienia wygasania, które zostały wcześniej skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="84c6d-308">This ignores any sliding expiration settings previously configured.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="84c6d-309">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="84c6d-309">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="84c6d-310">Kontrole ról oparte na zasadach</span><span class="sxs-lookup"><span data-stu-id="84c6d-310">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
