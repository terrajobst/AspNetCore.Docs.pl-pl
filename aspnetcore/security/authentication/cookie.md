---
title: Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity
author: rick-anderson
description: Informacje o sposobie używania uwierzytelniania plików cookie bez tożsamości platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622748"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="5577c-103">Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="5577c-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="5577c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5577c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5577c-105">Tożsamości platformy ASP.NET Core to dostawca uwierzytelniania pełne, kompleksowe za utworzenie i utrzymywanie logowania.</span><span class="sxs-lookup"><span data-stu-id="5577c-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="5577c-106">Jednak można dostawcy uwierzytelniania na podstawie plików cookie uwierzytelniania, bez tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5577c-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="5577c-107">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="5577c-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="5577c-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5577c-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5577c-109">Dla celów demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika Maria Rodriguez jest zapisane na stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5577c-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="5577c-110">Użyj **E-mail** nazwy użytkownika `maria.rodriguez@contoso.com` i wszystkie hasła do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5577c-110">Use the **Email** user name `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="5577c-111">Użytkownik jest uwierzytelniany w `AuthenticateUser` method in Class metoda *Pages/Account/Login.cshtml.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="5577c-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="5577c-112">Przykład rzeczywistych użytkownika może być uwierzytelniani względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5577c-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="5577c-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="5577c-113">Configuration</span></span>

<span data-ttu-id="5577c-114">Jeśli aplikacja nie używa [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="5577c-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="5577c-115">W `Startup.ConfigureServices` metody tworzenia usługi oprogramowania pośredniczącego uwierzytelniania za pomocą <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> i <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> metody:</span><span class="sxs-lookup"><span data-stu-id="5577c-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="5577c-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> przekazany do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5577c-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="5577c-117">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień pliku cookie uwierzytelniania, a użytkownik chce [autoryzacji przy użyciu określonego schematu](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="5577c-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="5577c-118">Ustawienie `AuthenticationScheme` do [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) zawiera wartość "Plików cookie" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="5577c-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="5577c-119">Możesz podać dowolną wartość ciągu, która odróżnia systemu.</span><span class="sxs-lookup"><span data-stu-id="5577c-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="5577c-120">Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania pliku cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5577c-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="5577c-121">Gdy nie jest udostępniane schematu uwierzytelniania pliku cookie <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa ona `CookieAuthenticationDefaults.AuthenticationScheme` ("plików cookie" ").</span><span class="sxs-lookup"><span data-stu-id="5577c-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="5577c-122">Plik cookie uwierzytelniania <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> właściwość jest ustawiona na `true` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="5577c-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="5577c-123">Pliki cookie uwierzytelniania są dozwolone, gdy użytkownik nie wyraził zgodę, do zbierania danych.</span><span class="sxs-lookup"><span data-stu-id="5577c-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="5577c-124">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="5577c-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="5577c-125">W `Startup.Configure` metody, wywołanie `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="5577c-125">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="5577c-126">Wywołaj `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="5577c-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="5577c-127"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="5577c-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="5577c-128">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi uwierzytelniania `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="5577c-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="5577c-129">Oprogramowaniu pośredniczącym pliku cookie zasad</span><span class="sxs-lookup"><span data-stu-id="5577c-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="5577c-130">[Oprogramowaniu pośredniczącym pliku cookie zasad](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) oferuje możliwości zasad pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="5577c-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="5577c-131">Dodawanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest kolejność liter&mdash;dotyczy tylko transmisji składników zarejestrowanych w potoku.</span><span class="sxs-lookup"><span data-stu-id="5577c-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="5577c-132">Użyj <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> udostępnionych plików Cookie zasad oprogramowania pośredniczącego, aby sterować globalne właściwości przetwarzania plików cookie oraz podłączania do obsługi przetwarzania plików cookie, gdy pliki cookie są dołączane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="5577c-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="5577c-133">Wartość domyślna <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> wartość `SameSiteMode.Lax` pozwalające uwierzytelniania OAuth2.</span><span class="sxs-lookup"><span data-stu-id="5577c-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="5577c-134">Ściśle wymusić zasady tej samej lokacji `SameSiteMode.Strict`ustaw `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="5577c-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="5577c-135">Mimo że to ustawienie dzieli OAuth2 i innych schematów uwierzytelniania między źródłami, eksponuje poziom bezpieczeństwa pliku cookie dla innych typów aplikacji, które nie należy polegać na przetwarzanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="5577c-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="5577c-136">Ustawienie oprogramowaniu pośredniczącym pliku Cookie zasad `MinimumSameSitePolicy` mogą mieć wpływ na ustawienie `Cookie.SameSite` w `CookieAuthenticationOptions` ustawienia zgodnie z poniższym macierzy.</span><span class="sxs-lookup"><span data-stu-id="5577c-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="5577c-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="5577c-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="5577c-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="5577c-138">Cookie.SameSite</span></span> | <span data-ttu-id="5577c-139">Wynikowe ustawienie Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="5577c-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="5577c-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5577c-140">SameSiteMode.None</span></span>     | <span data-ttu-id="5577c-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5577c-141">SameSiteMode.None</span></span><br><span data-ttu-id="5577c-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5577c-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="5577c-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="5577c-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5577c-144">SameSiteMode.None</span></span><br><span data-ttu-id="5577c-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5577c-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="5577c-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="5577c-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5577c-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="5577c-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5577c-148">SameSiteMode.None</span></span><br><span data-ttu-id="5577c-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5577c-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="5577c-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="5577c-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5577c-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="5577c-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5577c-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="5577c-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="5577c-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="5577c-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="5577c-155">SameSiteMode.None</span></span><br><span data-ttu-id="5577c-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="5577c-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="5577c-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="5577c-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="5577c-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="5577c-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="5577c-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="5577c-161">Tworzenie pliku cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="5577c-161">Create an authentication cookie</span></span>

<span data-ttu-id="5577c-162">Aby utworzyć plik cookie zawierający informacje o użytkowniku, należy utworzyć <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="5577c-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="5577c-163">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="5577c-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="5577c-164">Tworzenie <xref:System.Security.Claims.ClaimsIdentity> z każdego wymaganego <xref:System.Security.Claims.Claim>s, a następnie wywołać <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> do logowania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="5577c-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="5577c-165">`SignInAsync` Tworzy zaszyfrowanego pliku cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5577c-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="5577c-166">Jeśli `AuthenticationScheme` nie jest określony, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="5577c-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="5577c-167">Platforma ASP.NET Core [ochrony danych](xref:security/data-protection/using-data-protection) systemu jest używany do szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="5577c-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="5577c-168">Dla aplikacji hostowanych na wielu komputerach, obciążenia równoważenia w aplikacjach lub przy użyciu farmy sieci web [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) korzystać z tej samej pierścień klucz i identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5577c-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="5577c-169">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="5577c-169">Sign out</span></span>

<span data-ttu-id="5577c-170">Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="5577c-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="5577c-171">Jeśli `CookieAuthenticationDefaults.AuthenticationScheme` (lub "Plików cookie" ") nie jest używany jako schematu (na przykład" ContosoCookie"), podaj Schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="5577c-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="5577c-172">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="5577c-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="5577c-173">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="5577c-173">React to back-end changes</span></span>

<span data-ttu-id="5577c-174">Po utworzeniu pliku cookie, plik cookie jest jedynym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="5577c-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="5577c-175">Jeśli konto użytkownika jest wyłączona w systemów zaplecza:</span><span class="sxs-lookup"><span data-stu-id="5577c-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="5577c-176">System uwierzytelniania pliku cookie aplikacji w dalszym ciągu przetwarzać żądania oparte na pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="5577c-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="5577c-177">Użytkownik pozostaje zalogowany do aplikacji, tak długo, jak plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="5577c-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="5577c-178"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Zdarzeń może służyć do przechwycenia, a następnie zastąpić weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="5577c-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="5577c-179">Sprawdzanie poprawności pliku cookie na każde żądanie zmniejsza ryzyko związane odwołanych użytkowników, dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5577c-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="5577c-180">Jedno z podejść do weryfikacji opiera się na rejestrowanie informacji o zmianach w bazie danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5577c-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="5577c-181">Jeśli baza danych nie zostały zmodyfikowane, ponieważ został wystawiony przez użytkownika w pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli ich plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="5577c-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="5577c-182">W przykładową aplikację, baza danych jest realizowane w `IUserRepository` i przechowuje `LastChanged` wartość.</span><span class="sxs-lookup"><span data-stu-id="5577c-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="5577c-183">Gdy użytkownik zostanie zaktualizowany w bazie danych, `LastChanged` wartość jest ustawiona na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="5577c-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="5577c-184">W celu unieważnienia pliku cookie, gdy na podstawie zmian w bazie danych `LastChanged` wartość, należy utworzyć plik cookie z `LastChanged` oświadczenia, zawierającego bieżącego `LastChanged` wartość z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="5577c-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="5577c-185">Aby zaimplementować zastąpienie `ValidatePrincipal` zdarzeń, napisz metody z następującą sygnaturą w klasie, która pochodzi od klasy <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="5577c-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="5577c-186">Poniżej przedstawiono przykład implementacji `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="5577c-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="5577c-187">Rejestrowanie wystąpienia zdarzenia podczas rejestracji usługa plików cookie w `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="5577c-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="5577c-188">Podaj [zakresie rejestracji usługi](xref:fundamentals/dependency-injection#service-lifetimes) dla Twojego `CustomCookieAuthenticationEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="5577c-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="5577c-189">Rozważmy sytuację, w którym nazwa użytkownika jest aktualizowana&mdash;decyzji, który nie ma wpływu na zabezpieczenia w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="5577c-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="5577c-190">Jeśli chcesz zaktualizować bezpieczny głównej nazwy użytkownika, wywołaj `context.ReplacePrincipal` i ustaw `context.ShouldRenew` właściwość `true`.</span><span class="sxs-lookup"><span data-stu-id="5577c-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="5577c-191">W sposób opisany poniżej są wyzwalane na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="5577c-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="5577c-192">Sprawdzanie poprawności plików cookie uwierzytelniania dla wszystkich użytkowników na każde żądanie może spowodować zmniejszenie wydajności dużych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5577c-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="5577c-193">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="5577c-193">Persistent cookies</span></span>

<span data-ttu-id="5577c-194">Możesz chcieć plików cookie, aby zachować w wielu sesjach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="5577c-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="5577c-195">Ten stan trwały można włączyć tylko za zgodą użytkownika z polem wyboru "Pamiętaj mnie" na temat logowania się lub podobny mechanizm.</span><span class="sxs-lookup"><span data-stu-id="5577c-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="5577c-196">Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który przeżyje za pośrednictwem przeglądarki zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="5577c-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="5577c-197">Ustawienia wygaśnięcia przewijania wcześniej skonfigurowane są uznawane.</span><span class="sxs-lookup"><span data-stu-id="5577c-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="5577c-198">Jeśli plik cookie wygasa, gdy nastąpi zamknięcie okna przeglądarki, przeglądarki usuwa plik cookie po ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="5577c-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="5577c-199">Ustaw <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> do `true` w <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="5577c-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="5577c-200">Datę ważności pliku cookie bezwzględne</span><span class="sxs-lookup"><span data-stu-id="5577c-200">Absolute cookie expiration</span></span>

<span data-ttu-id="5577c-201">Czas wygaśnięcia bezwzględny, można ustawić za pomocą <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="5577c-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="5577c-202">Aby utworzyć trwały plik cookie `IsPersistent` również musi być ustawiona.</span><span class="sxs-lookup"><span data-stu-id="5577c-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="5577c-203">W przeciwnym razie plik cookie jest tworzony o okresie istnienia, oparte na sesji i można wycofać, ani przed ani po bilet uwierzytelniania, który przechowuje.</span><span class="sxs-lookup"><span data-stu-id="5577c-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="5577c-204">Gdy `ExpiresUtc` jest ustawiony, zastępuje ona wartość <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opcji <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="5577c-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="5577c-205">Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który jest ważny przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="5577c-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="5577c-206">To ignoruje wszelkie przewijania wcześniej skonfigurowane ustawienia wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="5577c-206">This ignores any sliding expiration settings previously configured.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="5577c-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5577c-207">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="5577c-208">Rola oparta na zasadach kontroli</span><span class="sxs-lookup"><span data-stu-id="5577c-208">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
