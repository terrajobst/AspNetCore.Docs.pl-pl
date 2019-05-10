---
title: Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity
author: rick-anderson
description: Wyjaśnienie przy użyciu uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: 2bc3e16b1b32816b14ad5bb4ca905ae5ed51ab87
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903154"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="9e028-103">Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="9e028-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="9e028-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9e028-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9e028-105">Jak przedstawiono w wcześniejszych tematach uwierzytelniania [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jest dostawcy uwierzytelniania pełne, kompleksowe za utworzenie i utrzymywanie logowania.</span><span class="sxs-lookup"><span data-stu-id="9e028-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="9e028-106">Można jednak logika uwierzytelniania niestandardowego za pomocą na podstawie plików cookie uwierzytelniania w czasie.</span><span class="sxs-lookup"><span data-stu-id="9e028-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="9e028-107">Uwierzytelnianie oparte na plikach cookie służy jako dostawcy uwierzytelniania autonomicznych, bez tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e028-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="9e028-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9e028-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9e028-109">Dla celów demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika Maria Rodriguez jest zapisane na stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e028-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="9e028-110">Użyj nazwy użytkownika wiadomości E-mail "maria.rodriguez@contoso.com" i wszystkie hasła do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9e028-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="9e028-111">Użytkownik jest uwierzytelniany w `AuthenticateUser` method in Class metoda *Pages/Account/Login.cshtml.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="9e028-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="9e028-112">Przykład rzeczywistych użytkownika może być uwierzytelniani względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9e028-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="9e028-113">Instrukcje dotyczące migrowania uwierzytelniania na podstawie plików cookie z platformą ASP.NET Core 1.x do 2.0, zobacz [migracji uwierzytelnianie i tożsamość do tematu platformy ASP.NET Core 2.0 (uwierzytelnianie na podstawie plików Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="9e028-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="9e028-114">Aby użyć tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity) tematu.</span><span class="sxs-lookup"><span data-stu-id="9e028-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="9e028-115">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="9e028-115">Configuration</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9e028-116">Jeśli aplikacja nie używa [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakiet (wersja 2.1.0 lub później).</span><span class="sxs-lookup"><span data-stu-id="9e028-116">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="9e028-117">W `ConfigureServices` metody tworzenia usługi oprogramowania pośredniczącego uwierzytelniania za pomocą `AddAuthentication` i `AddCookie` metody:</span><span class="sxs-lookup"><span data-stu-id="9e028-117">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="9e028-118">`AuthenticationScheme` przekazany do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e028-118">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="9e028-119">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień pliku cookie uwierzytelniania, a użytkownik chce [autoryzacji przy użyciu określonego schematu](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="9e028-119">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="9e028-120">Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="9e028-120">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="9e028-121">Możesz podać dowolną wartość ciągu, która odróżnia systemu.</span><span class="sxs-lookup"><span data-stu-id="9e028-121">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="9e028-122">Schemat uwierzytelniania aplikacji różni się od schematu uwierzytelniania pliku cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e028-122">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="9e028-123">Gdy nie jest udostępniane schematu uwierzytelniania pliku cookie <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, używa ona [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("plików cookie" ").</span><span class="sxs-lookup"><span data-stu-id="9e028-123">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="9e028-124">Plik cookie uwierzytelniania <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> właściwość jest ustawiona na `true` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="9e028-124">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="9e028-125">Pliki cookie uwierzytelniania są dozwolone, gdy użytkownik nie wyraził zgodę, do zbierania danych.</span><span class="sxs-lookup"><span data-stu-id="9e028-125">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="9e028-126">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="9e028-126">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="9e028-127">W `Configure` metody, użyj `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9e028-127">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="9e028-128">Wywołaj `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="9e028-128">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="9e028-129">**AddCookie Options**</span><span class="sxs-lookup"><span data-stu-id="9e028-129">**AddCookie Options**</span></span>

<span data-ttu-id="9e028-130">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="9e028-130">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="9e028-131">Opcja</span><span class="sxs-lookup"><span data-stu-id="9e028-131">Option</span></span> | <span data-ttu-id="9e028-132">Opis</span><span class="sxs-lookup"><span data-stu-id="9e028-132">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="9e028-133">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="9e028-133">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="9e028-134">Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania) po wyzwoleniu przez `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e028-134">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="9e028-135">Wartość domyślna to `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="9e028-135">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="9e028-136">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="9e028-136">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="9e028-137">Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość żadnych oświadczeń, utworzone przez usługę uwierzytelniania pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-137">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="9e028-138">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="9e028-138">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="9e028-139">Nazwa domeny, w którym plik cookie jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="9e028-139">The domain name where the cookie is served.</span></span> <span data-ttu-id="9e028-140">Domyślnie jest to nazwa hosta żądania.</span><span class="sxs-lookup"><span data-stu-id="9e028-140">By default, this is the host name of the request.</span></span> <span data-ttu-id="9e028-141">Przeglądarka wysyła tylko pliki cookie w żądaniach do takiej samej nazwie hosta.</span><span class="sxs-lookup"><span data-stu-id="9e028-141">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="9e028-142">Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie.</span><span class="sxs-lookup"><span data-stu-id="9e028-142">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="9e028-143">Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia je zadaniom `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="9e028-143">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="9e028-144">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="9e028-144">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="9e028-145">Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów.</span><span class="sxs-lookup"><span data-stu-id="9e028-145">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="9e028-146">Zmiana tej wartości do `false` zezwala na skrypty po stronie klienta do dostępu do pliku cookie i mogą otwierać aplikację, aby kradzieży plików cookie, powinny mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="9e028-146">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="9e028-147">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="9e028-147">The default value is `true`.</span></span> |
| [<span data-ttu-id="9e028-148">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="9e028-148">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="9e028-149">Określa nazwę pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-149">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="9e028-150">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="9e028-150">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="9e028-151">Używane do izolowania aplikacji uruchomionych na tej samej nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="9e028-151">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="9e028-152">Jeśli masz aplikację, w którym uruchomiono `/app1` i ograniczenie liczby plików cookie do tej aplikacji, ustaw `CookiePath` właściwość `/app1`.</span><span class="sxs-lookup"><span data-stu-id="9e028-152">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="9e028-153">W ten sposób plik cookie jest dostępny tylko dla żądań `/app1` oraz dowolnej aplikacji znajdujących się pod nim.</span><span class="sxs-lookup"><span data-stu-id="9e028-153">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="9e028-154">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="9e028-154">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="9e028-155">Wskazuje, czy przeglądarka powinna zezwalać pliku cookie do podłączenia do tej samej lokacji żądania tylko (`SameSiteMode.Strict`) lub żądań między witrynami przy użyciu bezpiecznych metod HTTP i tej samej lokacji żądania (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="9e028-155">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="9e028-156">Po ustawieniu `SameSiteMode.None`, nie jest ustawiona wartość nagłówka pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-156">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="9e028-157">Należy pamiętać, że [oprogramowaniu pośredniczącym pliku Cookie zasad](#cookie-policy-middleware) może zastąpić tę wartość, przez Ciebie.</span><span class="sxs-lookup"><span data-stu-id="9e028-157">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="9e028-158">Aby zapewnić obsługę uwierzytelniania OAuth, wartość domyślna to `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="9e028-158">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="9e028-159">Aby uzyskać więcej informacji, zobacz [uwierzytelniania OAuth, przerwane z powodu zasad plików cookie SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="9e028-159">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="9e028-160">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="9e028-160">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="9e028-161">Flaga wskazująca, jeśli utworzony plik cookie powinien być ograniczony do protokołu HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="9e028-161">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="9e028-162">Wartość domyślna to `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="9e028-162">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="9e028-163">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="9e028-163">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="9e028-164">Zestawy `DataProtectionProvider` umożliwiający tworzenie domyślnie `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="9e028-164">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="9e028-165">Jeśli `TicketDataFormat` ustawiono właściwość `DataProtectionProvider` nie jest używana opcja.</span><span class="sxs-lookup"><span data-stu-id="9e028-165">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="9e028-166">Jeśli nie zostanie podany, domyślny dostawca ochrony danych aplikacji jest używany.</span><span class="sxs-lookup"><span data-stu-id="9e028-166">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="9e028-167">Zdarzenia</span><span class="sxs-lookup"><span data-stu-id="9e028-167">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="9e028-168">Program obsługi wyjątku wywołuje metody dla dostawcy, które zapewniają kontroli aplikacji w pewnych punktach, przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="9e028-168">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="9e028-169">Jeśli `Events` nie są podane wystąpienie domyślne jest podany, które nie działają, gdy metody są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="9e028-169">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="9e028-170">EventsType</span><span class="sxs-lookup"><span data-stu-id="9e028-170">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="9e028-171">Używane jako typ usługi, aby uzyskać `Events` wystąpieniem, a nie właściwości.</span><span class="sxs-lookup"><span data-stu-id="9e028-171">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="9e028-172">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="9e028-172">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="9e028-173">`TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-173">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="9e028-174">`ExpireTimeSpan` jest dodawany do bieżącego czasu, aby utworzyć czas wygaśnięcia biletu.</span><span class="sxs-lookup"><span data-stu-id="9e028-174">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="9e028-175">`ExpiredTimeSpan` Wartość zawsze przechodzi w stan AuthTicket zaszyfrowane, zweryfikowane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="9e028-175">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="9e028-176">Również może przejść do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) nagłówka, ale tylko wtedy, gdy `IsPersistent` jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="9e028-176">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="9e028-177">Aby ustawić `IsPersistent` do `true`, skonfiguruj [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) przekazany do `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e028-177">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="9e028-178">Wartość domyślna `ExpireTimeSpan` to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="9e028-178">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="9e028-179">LoginPath</span><span class="sxs-lookup"><span data-stu-id="9e028-179">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="9e028-180">Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania) po wyzwoleniu przez `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e028-180">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="9e028-181">Bieżący adres URL, który wygenerował 401 jest dodawany do `LoginPath` jako o nazwie określonej przez parametr ciągu zapytania `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="9e028-181">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="9e028-182">Gdy żądanie `LoginPath` nadaje nową tożsamość logowania, `ReturnUrlParameter` wartość jest używana do przekierowania wyszukiwarki do adresu URL, który spowodował oryginalnego kodu stanu nieautoryzowanego.</span><span class="sxs-lookup"><span data-stu-id="9e028-182">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="9e028-183">Wartość domyślna to `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="9e028-183">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="9e028-184">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="9e028-184">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="9e028-185">Jeśli `LogoutPath` znajduje się do programu obsługi, a następnie przekierowuje żądanie do tej ścieżki na podstawie wartości `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="9e028-185">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="9e028-186">Wartość domyślna to `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="9e028-186">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="9e028-187">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="9e028-187">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="9e028-188">Określa nazwę parametru ciągu zapytania, która jest dołączana przez program obsługi odpowiedzi 302 Found (adres URL przekierowania).</span><span class="sxs-lookup"><span data-stu-id="9e028-188">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="9e028-189">`ReturnUrlParameter` jest używany, gdy żądanie dociera `LoginPath` lub `LogoutPath` aby powrócić do oryginalnego adresu URL w przeglądarce, po wykonaniu akcji logowania lub wylogowywania.</span><span class="sxs-lookup"><span data-stu-id="9e028-189">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="9e028-190">Wartość domyślna to `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="9e028-190">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="9e028-191">SessionStore</span><span class="sxs-lookup"><span data-stu-id="9e028-191">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="9e028-192">Opcjonalny kontener do przechowywania tożsamości na żądań.</span><span class="sxs-lookup"><span data-stu-id="9e028-192">An optional container used to store identity across requests.</span></span> <span data-ttu-id="9e028-193">W przypadku użycia, identyfikator sesji jest wysyłany do klienta.</span><span class="sxs-lookup"><span data-stu-id="9e028-193">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="9e028-194">`SessionStore` można wyeliminować potencjalne problemy z tożsamościami dużych.</span><span class="sxs-lookup"><span data-stu-id="9e028-194">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="9e028-195">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="9e028-195">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="9e028-196">Flaga wskazująca, czy nowy plik cookie z czasem wygaśnięcia zaktualizowane powinny być wydawane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="9e028-196">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="9e028-197">Można to zrobić na każde żądanie, gdy bieżący okres ważności pliku cookie więcej niż 50% wygasł.</span><span class="sxs-lookup"><span data-stu-id="9e028-197">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="9e028-198">Nowa data ważności jest przenoszony do przodu do bieżącej daty plus `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="9e028-198">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="9e028-199">[Czas wygaśnięcia bezwzględną pliku cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić za pomocą `AuthenticationProperties` klasy podczas wywoływania `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e028-199">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="9e028-200">Czas wygaśnięcia bezwzględny może zwiększyć zabezpieczenia aplikacji, ograniczając ilość czasu, który plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="9e028-200">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="9e028-201">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="9e028-201">The default value is `true`.</span></span> |
| [<span data-ttu-id="9e028-202">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="9e028-202">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="9e028-203">`TicketDataFormat` Służy do włączania i wyłączania ochrony tożsamości i innych właściwości, które są przechowywane w wartościach plików cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-203">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="9e028-204">Jeśli nie zostanie podana, `TicketDataFormat` jest tworzony przy użyciu [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="9e028-204">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="9e028-205">Sprawdzanie poprawności</span><span class="sxs-lookup"><span data-stu-id="9e028-205">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="9e028-206">Metoda, która sprawdza, czy opcje są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="9e028-206">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="9e028-207">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi uwierzytelniania `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="9e028-207">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9e028-208">ASP.NET Core 1.x korzysta z plików cookie [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) serializujący głównej nazwy użytkownika w zaszyfrowanym pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-208">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="9e028-209">Kolejne żądania plik cookie jest weryfikowana, a podmiot zabezpieczeń jest odtwarzane i przypisane do `HttpContext.User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9e028-209">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="9e028-210">Zainstaluj [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu NuGet w projekcie.</span><span class="sxs-lookup"><span data-stu-id="9e028-210">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="9e028-211">Ten pakiet zawiera oprogramowaniu pośredniczącym pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-211">This package contains the cookie middleware.</span></span>

<span data-ttu-id="9e028-212">Użyj `UseCookieAuthentication` method in Class metoda `Configure` method in Class metoda swoje *Startup.cs* pliku przed `UseMvc` lub `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="9e028-212">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="9e028-213">**CookieAuthenticationOptions Options**</span><span class="sxs-lookup"><span data-stu-id="9e028-213">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="9e028-214">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="9e028-214">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="9e028-215">Opcja</span><span class="sxs-lookup"><span data-stu-id="9e028-215">Option</span></span> | <span data-ttu-id="9e028-216">Opis</span><span class="sxs-lookup"><span data-stu-id="9e028-216">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="9e028-217">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="9e028-217">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="9e028-218">Ustawia schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="9e028-218">Sets the authentication scheme.</span></span> <span data-ttu-id="9e028-219">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania, a użytkownik chce [autoryzacji przy użyciu określonego schematu](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="9e028-219">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="9e028-220">Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="9e028-220">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="9e028-221">Możesz podać dowolną wartość ciągu, która odróżnia systemu.</span><span class="sxs-lookup"><span data-stu-id="9e028-221">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="9e028-222">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="9e028-222">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="9e028-223">Ustawia wartość, aby wskazać, że uwierzytelniania plików cookie na każde żądanie i uruchamiania próba weryfikacji i ponownie skonstruuj Zserializowany podmiot zabezpieczeń, utworzone przez siebie.</span><span class="sxs-lookup"><span data-stu-id="9e028-223">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="9e028-224">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="9e028-224">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="9e028-225">W przypadku opcji true oprogramowania pośredniczącego uwierzytelniania obsługuje automatyczne wyzwania.</span><span class="sxs-lookup"><span data-stu-id="9e028-225">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="9e028-226">Jeśli ma wartość FAŁSZ, oprogramowanie pośredniczące uwierzytelniania zmienia tylko odpowiedzi, gdy jest to wyraźnie wskazane przez `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="9e028-226">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="9e028-227">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="9e028-227">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="9e028-228">Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość żadnych oświadczeń, utworzone przez oprogramowanie pośredniczące uwierzytelniania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-228">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="9e028-229">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="9e028-229">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="9e028-230">Nazwa domeny, w którym plik cookie jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="9e028-230">The domain name where the cookie is served.</span></span> <span data-ttu-id="9e028-231">Domyślnie jest to nazwa hosta żądania.</span><span class="sxs-lookup"><span data-stu-id="9e028-231">By default, this is the host name of the request.</span></span> <span data-ttu-id="9e028-232">Przeglądarka służy tylko pliku cookie do takiej samej nazwie hosta.</span><span class="sxs-lookup"><span data-stu-id="9e028-232">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="9e028-233">Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie.</span><span class="sxs-lookup"><span data-stu-id="9e028-233">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="9e028-234">Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia je zadaniom `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="9e028-234">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="9e028-235">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="9e028-235">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="9e028-236">Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów.</span><span class="sxs-lookup"><span data-stu-id="9e028-236">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="9e028-237">Zmiana tej wartości do `false` zezwala na skrypty po stronie klienta do dostępu do pliku cookie i mogą otwierać aplikację, aby kradzieży plików cookie, powinny mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="9e028-237">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="9e028-238">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="9e028-238">The default value is `true`.</span></span> |
| [<span data-ttu-id="9e028-239">CookiePath</span><span class="sxs-lookup"><span data-stu-id="9e028-239">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="9e028-240">Używane do izolowania aplikacji uruchomionych na tej samej nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="9e028-240">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="9e028-241">Jeśli masz aplikację, w którym uruchomiono `/app1` i ograniczenie liczby plików cookie do tej aplikacji, ustaw `CookiePath` właściwość `/app1`.</span><span class="sxs-lookup"><span data-stu-id="9e028-241">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="9e028-242">W ten sposób plik cookie jest dostępny tylko dla żądań `/app1` oraz dowolnej aplikacji znajdujących się pod nim.</span><span class="sxs-lookup"><span data-stu-id="9e028-242">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="9e028-243">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="9e028-243">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="9e028-244">Flaga wskazująca, jeśli utworzony plik cookie powinien być ograniczony do protokołu HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="9e028-244">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="9e028-245">Wartość domyślna to `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="9e028-245">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="9e028-246">Opis</span><span class="sxs-lookup"><span data-stu-id="9e028-246">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="9e028-247">Dodatkowe informacje o typie uwierzytelniania, który ma zostać udostępnione do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e028-247">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="9e028-248">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="9e028-248">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="9e028-249">`TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="9e028-249">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="9e028-250">Jest dodawany do bieżącego czasu, aby utworzyć czas wygaśnięcia biletu.</span><span class="sxs-lookup"><span data-stu-id="9e028-250">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="9e028-251">Aby użyć `ExpireTimeSpan`, należy ustawić `IsPersistent` do `true` w `AuthenticationProperties` przekazany do `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e028-251">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="9e028-252">Wartość domyślna to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="9e028-252">The default value is 14 days.</span></span> |
| [<span data-ttu-id="9e028-253">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="9e028-253">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="9e028-254">Flaga wskazująca, czy data wygaśnięcia pliku cookie resetuje, gdy więcej niż połowy `ExpireTimeSpan` upływie interwału.</span><span class="sxs-lookup"><span data-stu-id="9e028-254">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="9e028-255">Nową wartość czasu wygaśnięcia jest przenoszony do przodu do bieżącej daty plus `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="9e028-255">The new expiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="9e028-256">[Czas wygaśnięcia bezwzględną pliku cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić za pomocą `AuthenticationProperties` klasy podczas wywoływania `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e028-256">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="9e028-257">Czas wygaśnięcia bezwzględny może zwiększyć zabezpieczenia aplikacji, ograniczając ilość czasu, który plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="9e028-257">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="9e028-258">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="9e028-258">The default value is `true`.</span></span> |

<span data-ttu-id="9e028-259">Ustaw `CookieAuthenticationOptions` dla oprogramowania pośredniczącego uwierzytelniania pliku Cookie w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="9e028-259">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a><span data-ttu-id="9e028-260">Oprogramowaniu pośredniczącym pliku cookie zasad</span><span class="sxs-lookup"><span data-stu-id="9e028-260">Cookie Policy Middleware</span></span>

<span data-ttu-id="9e028-261">[Oprogramowaniu pośredniczącym pliku cookie zasad](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) oferuje możliwości zasad plików cookie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e028-261">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="9e028-262">Dodawanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest kolejność liter; dotyczy tylko składniki zarejestrowane po nim w potoku.</span><span class="sxs-lookup"><span data-stu-id="9e028-262">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="9e028-263">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) dostarczane z oprogramowaniem pośredniczącym pliku Cookie zasady umożliwiają kontrolowanie globalne właściwości przetwarzania plików cookie oraz podłączania do obsługi przetwarzania plików cookie, pliki cookie są dołączane lub usunięciu.</span><span class="sxs-lookup"><span data-stu-id="9e028-263">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="9e028-264">Właściwość</span><span class="sxs-lookup"><span data-stu-id="9e028-264">Property</span></span> | <span data-ttu-id="9e028-265">Opis</span><span class="sxs-lookup"><span data-stu-id="9e028-265">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="9e028-266">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="9e028-266">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="9e028-267">Ma wpływ na, czy pliki cookie musi być HttpOnly, który jest Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów.</span><span class="sxs-lookup"><span data-stu-id="9e028-267">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="9e028-268">Wartość domyślna to `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="9e028-268">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="9e028-269">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="9e028-269">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="9e028-270">Wpływa na atrybut o tej samej lokacji plik cookie (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="9e028-270">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="9e028-271">Wartość domyślna to `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="9e028-271">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="9e028-272">Ta opcja jest dostępna dla platformy ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="9e028-272">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="9e028-273">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="9e028-273">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="9e028-274">Wywołuje się, gdy jest dołączany plik cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-274">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="9e028-275">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="9e028-275">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="9e028-276">Wywołuje się, gdy plik cookie zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="9e028-276">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="9e028-277">Bezpieczeństwo</span><span class="sxs-lookup"><span data-stu-id="9e028-277">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="9e028-278">Wpływa na pliki cookie musi być bezpieczny.</span><span class="sxs-lookup"><span data-stu-id="9e028-278">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="9e028-279">Wartość domyślna to `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="9e028-279">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="9e028-280">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span><span class="sxs-lookup"><span data-stu-id="9e028-280">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="9e028-281">Wartość domyślna `MinimumSameSitePolicy` wartość `SameSiteMode.Lax` pozwalające uwierzytelniania OAuth2.</span><span class="sxs-lookup"><span data-stu-id="9e028-281">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="9e028-282">Ściśle wymusić zasady tej samej lokacji `SameSiteMode.Strict`ustaw `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="9e028-282">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="9e028-283">Mimo że to ustawienie dzieli OAuth2 i innych schematów uwierzytelniania między źródłami, eksponuje poziom bezpieczeństwa pliku cookie dla innych typów aplikacji, które nie należy polegać na przetwarzanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="9e028-283">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="9e028-284">Ustawienie oprogramowaniu pośredniczącym pliku Cookie zasad `MinimumSameSitePolicy` mogą mieć wpływ na Twoje ustawienia `Cookie.SameSite` w `CookieAuthenticationOptions` ustawienia zgodnie z poniższym macierzy.</span><span class="sxs-lookup"><span data-stu-id="9e028-284">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="9e028-285">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="9e028-285">MinimumSameSitePolicy</span></span> | <span data-ttu-id="9e028-286">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="9e028-286">Cookie.SameSite</span></span> | <span data-ttu-id="9e028-287">Wynikowe ustawienie Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="9e028-287">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="9e028-288">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="9e028-288">SameSiteMode.None</span></span>     | <span data-ttu-id="9e028-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="9e028-289">SameSiteMode.None</span></span><br><span data-ttu-id="9e028-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="9e028-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e028-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-291">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e028-292">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="9e028-292">SameSiteMode.None</span></span><br><span data-ttu-id="9e028-293">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="9e028-293">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e028-294">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-294">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="9e028-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="9e028-295">SameSiteMode.Lax</span></span>      | <span data-ttu-id="9e028-296">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="9e028-296">SameSiteMode.None</span></span><br><span data-ttu-id="9e028-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="9e028-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e028-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-298">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e028-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="9e028-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e028-300">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="9e028-300">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e028-301">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-301">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="9e028-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-302">SameSiteMode.Strict</span></span>   | <span data-ttu-id="9e028-303">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="9e028-303">SameSiteMode.None</span></span><br><span data-ttu-id="9e028-304">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="9e028-304">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e028-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-305">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e028-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-306">SameSiteMode.Strict</span></span><br><span data-ttu-id="9e028-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-307">SameSiteMode.Strict</span></span><br><span data-ttu-id="9e028-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="9e028-308">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="9e028-309">Tworzenie pliku cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="9e028-309">Create an authentication cookie</span></span>

<span data-ttu-id="9e028-310">Aby utworzyć plik cookie zawierający informacje o użytkowniku, należy tworzyć [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="9e028-310">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="9e028-311">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-311">The user information is serialized and stored in the cookie.</span></span> 

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9e028-312">Tworzenie [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) z każdego wymaganego [oświadczenia](/dotnet/api/system.security.claims.claim)s, a następnie wywołać [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) do logowania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="9e028-312">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9e028-313">Wywołaj [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) do logowania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="9e028-313">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

<span data-ttu-id="9e028-314">`SignInAsync` Tworzy zaszyfrowanego pliku cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9e028-314">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="9e028-315">Jeśli nie określisz `AuthenticationScheme`, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="9e028-315">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="9e028-316">Dzieje się w tle, szyfrowanie, używana jest platforma ASP.NET Core [ochrony danych](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) systemu.</span><span class="sxs-lookup"><span data-stu-id="9e028-316">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="9e028-317">Jeśli hostujesz aplikację na wielu maszynach, równoważenia obciążenia w aplikacjach lub przy użyciu farmy sieci web, a następnie należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) korzystać z tej samej pierścień klucz i identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e028-317">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="9e028-318">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="9e028-318">Sign out</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9e028-319">Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="9e028-319">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9e028-320">Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="9e028-320">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

<span data-ttu-id="9e028-321">Jeśli nie używasz `CookieAuthenticationDefaults.AuthenticationScheme` (lub "Plików cookie" ") jako systemu (na przykład" ContosoCookie"), podaj Schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="9e028-321">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="9e028-322">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="9e028-322">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="9e028-323">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="9e028-323">React to back-end changes</span></span>

<span data-ttu-id="9e028-324">Po utworzeniu pliku cookie staje się pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="9e028-324">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="9e028-325">Nawet jeśli użytkownik zostanie wyłączone w systemach zaplecza, system plików cookie uwierzytelniania nie ma informacji o tym, a użytkownik pozostaje zalogowany, tak długo, jak ich plik cookie jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="9e028-325">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="9e028-326">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) zdarzeń w programie ASP.NET Core 2.x lub [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metodę w programie ASP.NET Core 1.x może służyć do przechwycenia, a następnie zastąpić weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="9e028-326">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="9e028-327">Takie podejście zmniejsza ryzyko odwołanych użytkownikom dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e028-327">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="9e028-328">Jedno z podejść do weryfikacji opiera się na rejestrowanie informacji o bazie danych użytkownika po zmianie.</span><span class="sxs-lookup"><span data-stu-id="9e028-328">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="9e028-329">Jeśli baza danych nie zostały zmodyfikowane, ponieważ został wystawiony przez użytkownika w pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli ich plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="9e028-329">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="9e028-330">Aby zrealizować ten scenariusz, w bazie danych, która jest zaimplementowana w `IUserRepository` w tym przykładzie przechowuje `LastChanged` wartość.</span><span class="sxs-lookup"><span data-stu-id="9e028-330">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="9e028-331">Po zaktualizowaniu dowolnego użytkownika w bazie danych `LastChanged` wartość jest ustawiona na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="9e028-331">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="9e028-332">W celu unieważnienia pliku cookie, gdy na podstawie zmian w bazie danych `LastChanged` wartość, należy utworzyć plik cookie z `LastChanged` oświadczenia, zawierającego bieżącego `LastChanged` wartość z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="9e028-332">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9e028-333">Aby zaimplementować zastąpienie `ValidatePrincipal` zdarzeń, napisz metody z następującą sygnaturą w klasie, która pochodzi z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="9e028-333">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="9e028-334">Przykład wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="9e028-334">An example looks like the following:</span></span>

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

<span data-ttu-id="9e028-335">Rejestrowanie wystąpienia zdarzenia podczas rejestracji usługa plików cookie w `ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="9e028-335">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="9e028-336">Podaj rejestracji usługi o określonym zakresie z `CustomCookieAuthenticationEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="9e028-336">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9e028-337">Aby zaimplementować zastąpienie `ValidateAsync` zdarzeń, napisz metody z następującą sygnaturą:</span><span class="sxs-lookup"><span data-stu-id="9e028-337">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="9e028-338">Tożsamości platformy ASP.NET Core to sprawdzenie są implementowane jako część jej [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="9e028-338">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="9e028-339">Przykład wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="9e028-339">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="9e028-340">Rejestrowanie zdarzeń podczas konfigurowania uwierzytelniania plików cookie w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="9e028-340">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

<span data-ttu-id="9e028-341">Rozważmy sytuację, w którym nazwa użytkownika jest aktualizowana &mdash; decyzji, który nie ma wpływu na zabezpieczenia w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="9e028-341">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="9e028-342">Jeśli chcesz zaktualizować bezpieczny głównej nazwy użytkownika, wywołaj `context.ReplacePrincipal` i ustaw `context.ShouldRenew` właściwość `true`.</span><span class="sxs-lookup"><span data-stu-id="9e028-342">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="9e028-343">W sposób opisany poniżej są wyzwalane na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="9e028-343">The approach described here is triggered on every request.</span></span> <span data-ttu-id="9e028-344">Może to spowodować spadek wydajności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e028-344">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="9e028-345">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="9e028-345">Persistent cookies</span></span>

<span data-ttu-id="9e028-346">Możesz chcieć plików cookie, aby zachować w wielu sesjach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="9e028-346">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="9e028-347">Ten stan trwały można włączyć tylko za zgodą użytkownika z polem wyboru "Pamiętaj mnie" logowania lub podobny mechanizm.</span><span class="sxs-lookup"><span data-stu-id="9e028-347">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on login or a similar mechanism.</span></span> 

<span data-ttu-id="9e028-348">Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który przeżyje za pośrednictwem przeglądarki zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="9e028-348">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="9e028-349">Ustawienia wygaśnięcia przewijania wcześniej skonfigurowane są uznawane.</span><span class="sxs-lookup"><span data-stu-id="9e028-349">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="9e028-350">Jeśli plik cookie wygasa, gdy nastąpi zamknięcie okna przeglądarki, przeglądarki usuwa plik cookie po ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="9e028-350">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="9e028-351">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) klasa znajduje się w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="9e028-351">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="9e028-352">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) klasa znajduje się w `Microsoft.AspNetCore.Http.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="9e028-352">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

::: moniker-end

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="9e028-353">Datę ważności pliku cookie bezwzględne</span><span class="sxs-lookup"><span data-stu-id="9e028-353">Absolute cookie expiration</span></span>

<span data-ttu-id="9e028-354">Można ustawić czasu wygaśnięcia bezwzględne za pomocą `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="9e028-354">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="9e028-355">Aby utworzyć trwały plik cookie, należy także ustawić `IsPersistent`; w przeciwnym wypadku plik cookie jest tworzony o okresie istnienia, oparte na sesji i może wygaśnie przed lub po biletu uwierzytelniania, który przechowuje.</span><span class="sxs-lookup"><span data-stu-id="9e028-355">To create a persistent cookie, you must also set `IsPersistent`; otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="9e028-356">Gdy `ExpiresUtc` jest ustawiona na `SignInAsync`, zastępuje ona wartość `ExpireTimeSpan` opcji `CookieAuthenticationOptions`, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="9e028-356">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="9e028-357">Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który jest ważny przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="9e028-357">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="9e028-358">To ignoruje wszelkie przewijania wcześniej skonfigurowane ustawienia wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="9e028-358">This ignores any sliding expiration settings previously configured.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
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

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9e028-359">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9e028-359">Additional resources</span></span>

* [<span data-ttu-id="9e028-360">Zmiany uwierzytelniania w wersji 2.0 / migracji ogłoszenia</span><span class="sxs-lookup"><span data-stu-id="9e028-360">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="9e028-361">Rola oparta na zasadach kontroli</span><span class="sxs-lookup"><span data-stu-id="9e028-361">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
