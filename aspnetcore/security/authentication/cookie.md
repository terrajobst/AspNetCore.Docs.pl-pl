---
title: Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity
author: rick-anderson
description: Wyjaśnienie przy użyciu uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: ac1eec297d3efd1403990722f59c414ba4e5ddd9
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095804"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="7bd6b-103">Używania uwierzytelniania plików cookie bez użycia produktu ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="7bd6b-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="7bd6b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7bd6b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7bd6b-105">Jak przedstawiono w wcześniejszych tematach uwierzytelniania [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jest dostawcy uwierzytelniania pełne, kompleksowe za utworzenie i utrzymywanie logowania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="7bd6b-106">Można jednak logika uwierzytelniania niestandardowego za pomocą na podstawie plików cookie uwierzytelniania w czasie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="7bd6b-107">Uwierzytelnianie oparte na plikach cookie służy jako dostawcy uwierzytelniania autonomicznych, bez tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="7bd6b-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7bd6b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7bd6b-109">Dla celów demonstracyjnych w przykładowej aplikacji konto użytkownika dla hipotetycznego użytkownika Maria Rodriguez jest zapisane na stałe do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="7bd6b-110">Użyj nazwy użytkownika wiadomości E-mail "maria.rodriguez@contoso.com" i wszystkie hasła do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="7bd6b-111">Użytkownik jest uwierzytelniany w `AuthenticateUser` method in Class metoda *Pages/Account/Login.cshtml.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="7bd6b-112">Przykład rzeczywistych użytkownika może być uwierzytelniani względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="7bd6b-113">Instrukcje dotyczące migrowania uwierzytelniania na podstawie plików cookie z platformą ASP.NET Core 1.x do 2.0, zobacz [migracji uwierzytelnianie i tożsamość do tematu platformy ASP.NET Core 2.0 (uwierzytelnianie na podstawie plików Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="7bd6b-114">Aby użyć tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity) tematu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="7bd6b-115">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="7bd6b-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bd6b-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7bd6b-117">Jeśli aplikacja nie używa [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakiet (wersja 2.1.0 lub później).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="7bd6b-118">W `ConfigureServices` metody tworzenia usługi oprogramowania pośredniczącego uwierzytelniania za pomocą `AddAuthentication` i `AddCookie` metody:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="7bd6b-119">`AuthenticationScheme` przekazany do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="7bd6b-120">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień pliku cookie uwierzytelniania, a użytkownik chce [autoryzacji przy użyciu określonego schematu](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="7bd6b-121">Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="7bd6b-122">Możesz podać dowolną wartość ciągu, która odróżnia systemu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="7bd6b-123">W `Configure` metody, użyj `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-123">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="7bd6b-124">Wywołaj `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="7bd6b-125">**Opcje AddCookie**</span><span class="sxs-lookup"><span data-stu-id="7bd6b-125">**AddCookie Options**</span></span>

<span data-ttu-id="7bd6b-126">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-126">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="7bd6b-127">Opcja</span><span class="sxs-lookup"><span data-stu-id="7bd6b-127">Option</span></span> | <span data-ttu-id="7bd6b-128">Opis</span><span class="sxs-lookup"><span data-stu-id="7bd6b-128">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="7bd6b-129">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="7bd6b-129">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-130">Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania) po wyzwoleniu przez `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-130">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="7bd6b-131">Wartość domyślna to `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-131">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="7bd6b-132">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="7bd6b-132">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-133">Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość żadnych oświadczeń, utworzone przez usługę uwierzytelniania pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-133">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="7bd6b-134">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="7bd6b-134">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-135">Nazwa domeny, w którym plik cookie jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-135">The domain name where the cookie is served.</span></span> <span data-ttu-id="7bd6b-136">Domyślnie jest to nazwa hosta żądania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-136">By default, this is the host name of the request.</span></span> <span data-ttu-id="7bd6b-137">Przeglądarka wysyła tylko pliki cookie w żądaniach do takiej samej nazwie hosta.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-137">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="7bd6b-138">Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-138">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="7bd6b-139">Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia je zadaniom `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-139">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="7bd6b-140">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="7bd6b-140">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-141">Pobiera lub ustawia cyklem życia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-141">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="7bd6b-142">Obecnie ta opcja żadnych operacji i staną się przestarzałe w programie ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-142">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="7bd6b-143">Użyj `ExpireTimeSpan` opcję, aby ustawić datę ważności pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-143">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="7bd6b-144">Aby uzyskać więcej informacji, zobacz [wyjaśnić zachowanie CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-144">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="7bd6b-145">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="7bd6b-145">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-146">Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-146">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="7bd6b-147">Zmiana tej wartości do `false` zezwala na skrypty po stronie klienta do dostępu do pliku cookie i mogą otwierać aplikację, aby kradzieży plików cookie, powinny mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-147">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="7bd6b-148">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-148">The default value is `true`.</span></span> |
| [<span data-ttu-id="7bd6b-149">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="7bd6b-149">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-150">Określa nazwę pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-150">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="7bd6b-151">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="7bd6b-151">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-152">Używane do izolowania aplikacji uruchomionych na tej samej nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-152">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="7bd6b-153">Jeśli masz aplikację, w którym uruchomiono `/app1` i ograniczenie liczby plików cookie do tej aplikacji, ustaw `CookiePath` właściwość `/app1`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-153">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="7bd6b-154">W ten sposób plik cookie jest dostępny tylko dla żądań `/app1` oraz dowolnej aplikacji znajdujących się pod nim.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-154">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="7bd6b-155">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="7bd6b-155">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-156">Wskazuje, czy przeglądarka powinna zezwalać pliku cookie do podłączenia do tej samej lokacji żądania tylko (`SameSiteMode.Strict`) lub żądań między witrynami przy użyciu bezpiecznych metod HTTP i tej samej lokacji żądania (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-156">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="7bd6b-157">Po ustawieniu `SameSiteMode.None`, nie jest ustawiona wartość nagłówka pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-157">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="7bd6b-158">Należy pamiętać, że [oprogramowaniu pośredniczącym pliku Cookie zasad](#cookie-policy-middleware) może zastąpić tę wartość, przez Ciebie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-158">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="7bd6b-159">Aby zapewnić obsługę uwierzytelniania OAuth, wartość domyślna to `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-159">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="7bd6b-160">Aby uzyskać więcej informacji, zobacz [uwierzytelniania OAuth, przerwane z powodu zasad plików cookie SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-160">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="7bd6b-161">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="7bd6b-161">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-162">Flaga wskazująca, jeśli utworzony plik cookie powinien być ograniczony do protokołu HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-162">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="7bd6b-163">Wartość domyślna to `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-163">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="7bd6b-164">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="7bd6b-164">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-165">Zestawy `DataProtectionProvider` umożliwiający tworzenie domyślnie `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-165">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="7bd6b-166">Jeśli `TicketDataFormat` ustawiono właściwość `DataProtectionProvider` nie jest używana opcja.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-166">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="7bd6b-167">Jeśli nie zostanie podany, domyślny dostawca ochrony danych aplikacji jest używany.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-167">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="7bd6b-168">Zdarzenia</span><span class="sxs-lookup"><span data-stu-id="7bd6b-168">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-169">Program obsługi wyjątku wywołuje metody dla dostawcy, które zapewniają kontroli aplikacji w pewnych punktach, przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-169">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="7bd6b-170">Jeśli `Events` nie są podane wystąpienie domyślne jest podany, które nie działają, gdy metody są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-170">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="7bd6b-171">EventsType</span><span class="sxs-lookup"><span data-stu-id="7bd6b-171">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-172">Używane jako typ usługi, aby uzyskać `Events` wystąpieniem, a nie właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-172">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="7bd6b-173">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="7bd6b-173">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-174">`TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-174">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="7bd6b-175">`ExpireTimeSpan` jest dodawany do bieżącego czasu, aby utworzyć czas wygaśnięcia biletu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-175">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="7bd6b-176">`ExpiredTimeSpan` Wartość zawsze przechodzi w stan AuthTicket zaszyfrowane, zweryfikowane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-176">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="7bd6b-177">Również może przejść do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) nagłówka, ale tylko wtedy, gdy `IsPersistent` jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-177">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="7bd6b-178">Aby ustawić `IsPersistent` do `true`, skonfiguruj [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) przekazany do `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-178">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="7bd6b-179">Wartość domyślna `ExpireTimeSpan` to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-179">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="7bd6b-180">LoginPath</span><span class="sxs-lookup"><span data-stu-id="7bd6b-180">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-181">Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania) po wyzwoleniu przez `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-181">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="7bd6b-182">Bieżący adres URL, który wygenerował 401 jest dodawany do `LoginPath` jako o nazwie określonej przez parametr ciągu zapytania `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-182">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="7bd6b-183">Gdy żądanie `LoginPath` nadaje nową tożsamość logowania, `ReturnUrlParameter` wartość jest używana do przekierowania wyszukiwarki do adresu URL, który spowodował oryginalnego kodu stanu nieautoryzowanego.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-183">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="7bd6b-184">Wartość domyślna to `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-184">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="7bd6b-185">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="7bd6b-185">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-186">Jeśli `LogoutPath` znajduje się do programu obsługi, a następnie przekierowuje żądanie do tej ścieżki na podstawie wartości `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-186">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="7bd6b-187">Wartość domyślna to `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-187">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="7bd6b-188">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="7bd6b-188">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-189">Określa nazwę parametru ciągu zapytania, która jest dołączana przez program obsługi odpowiedzi 302 Found (adres URL przekierowania).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-189">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="7bd6b-190">`ReturnUrlParameter` jest używany, gdy żądanie dociera `LoginPath` lub `LogoutPath` aby powrócić do oryginalnego adresu URL w przeglądarce, po wykonaniu akcji logowania lub wylogowywania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-190">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="7bd6b-191">Wartość domyślna to `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-191">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="7bd6b-192">SessionStore</span><span class="sxs-lookup"><span data-stu-id="7bd6b-192">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-193">Opcjonalny kontener do przechowywania tożsamości na żądań.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-193">An optional container used to store identity across requests.</span></span> <span data-ttu-id="7bd6b-194">W przypadku użycia, identyfikator sesji jest wysyłany do klienta.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-194">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="7bd6b-195">`SessionStore` można wyeliminować potencjalne problemy z tożsamościami dużych.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-195">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="7bd6b-196">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="7bd6b-196">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-197">Flaga wskazująca, czy nowy plik cookie z czasem wygaśnięcia zaktualizowane powinny być wydawane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-197">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="7bd6b-198">Można to zrobić na każde żądanie, gdy bieżący okres ważności pliku cookie więcej niż 50% wygasł.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-198">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="7bd6b-199">Nowa data ważności jest przenoszony do przodu do bieżącej daty plus `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-199">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="7bd6b-200">[Czas wygaśnięcia bezwzględną pliku cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić za pomocą `AuthenticationProperties` klasy podczas wywoływania `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-200">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="7bd6b-201">Czas wygaśnięcia bezwzględny może zwiększyć zabezpieczenia aplikacji, ograniczając ilość czasu, który plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-201">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="7bd6b-202">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-202">The default value is `true`.</span></span> |
| [<span data-ttu-id="7bd6b-203">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="7bd6b-203">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-204">`TicketDataFormat` Służy do włączania i wyłączania ochrony tożsamości i innych właściwości, które są przechowywane w wartościach plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-204">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="7bd6b-205">Jeśli nie zostanie podana, `TicketDataFormat` jest tworzony przy użyciu [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-205">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="7bd6b-206">Sprawdzanie poprawności</span><span class="sxs-lookup"><span data-stu-id="7bd6b-206">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="7bd6b-207">Metoda, która sprawdza, czy opcje są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-207">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="7bd6b-208">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi uwierzytelniania `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-208">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bd6b-209">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-209">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7bd6b-210">ASP.NET Core 1.x korzysta z plików cookie [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) serializujący głównej nazwy użytkownika w zaszyfrowanym pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="7bd6b-211">Kolejne żądania plik cookie jest weryfikowana, a podmiot zabezpieczeń jest odtwarzane i przypisane do `HttpContext.User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="7bd6b-212">Zainstaluj [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu NuGet w projekcie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="7bd6b-213">Ten pakiet zawiera oprogramowaniu pośredniczącym pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="7bd6b-214">Użyj `UseCookieAuthentication` method in Class metoda `Configure` method in Class metoda swoje *Startup.cs* pliku przed `UseMvc` lub `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="7bd6b-215">**Opcje CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="7bd6b-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="7bd6b-216">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="7bd6b-217">Opcja</span><span class="sxs-lookup"><span data-stu-id="7bd6b-217">Option</span></span> | <span data-ttu-id="7bd6b-218">Opis</span><span class="sxs-lookup"><span data-stu-id="7bd6b-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="7bd6b-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="7bd6b-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-220">Ustawia schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-220">Sets the authentication scheme.</span></span> <span data-ttu-id="7bd6b-221">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania, a użytkownik chce [autoryzacji przy użyciu określonego schematu](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="7bd6b-222">Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" dla schematu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="7bd6b-223">Możesz podać dowolną wartość ciągu, która odróżnia systemu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="7bd6b-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="7bd6b-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-225">Ustawia wartość, aby wskazać, że uwierzytelniania plików cookie na każde żądanie i uruchamiania próba weryfikacji i ponownie skonstruuj Zserializowany podmiot zabezpieczeń, utworzone przez siebie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="7bd6b-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="7bd6b-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-227">W przypadku opcji true oprogramowania pośredniczącego uwierzytelniania obsługuje automatyczne wyzwania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="7bd6b-228">Jeśli ma wartość FAŁSZ, oprogramowanie pośredniczące uwierzytelniania zmienia tylko odpowiedzi, gdy jest to wyraźnie wskazane przez `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="7bd6b-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="7bd6b-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-230">Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość żadnych oświadczeń, utworzone przez oprogramowanie pośredniczące uwierzytelniania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="7bd6b-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="7bd6b-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-232">Nazwa domeny, w którym plik cookie jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="7bd6b-233">Domyślnie jest to nazwa hosta żądania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="7bd6b-234">Przeglądarka służy tylko pliku cookie do takiej samej nazwie hosta.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="7bd6b-235">Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="7bd6b-236">Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia je zadaniom `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="7bd6b-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="7bd6b-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-238">Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="7bd6b-239">Zmiana tej wartości do `false` zezwala na skrypty po stronie klienta do dostępu do pliku cookie i mogą otwierać aplikację, aby kradzieży plików cookie, powinny mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="7bd6b-240">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="7bd6b-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="7bd6b-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-242">Używane do izolowania aplikacji uruchomionych na tej samej nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="7bd6b-243">Jeśli masz aplikację, w którym uruchomiono `/app1` i ograniczenie liczby plików cookie do tej aplikacji, ustaw `CookiePath` właściwość `/app1`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="7bd6b-244">W ten sposób plik cookie jest dostępny tylko dla żądań `/app1` oraz dowolnej aplikacji znajdujących się pod nim.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="7bd6b-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="7bd6b-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-246">Flaga wskazująca, jeśli utworzony plik cookie powinien być ograniczony do protokołu HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="7bd6b-247">Wartość domyślna to `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="7bd6b-248">Opis</span><span class="sxs-lookup"><span data-stu-id="7bd6b-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-249">Dodatkowe informacje o typie uwierzytelniania, który ma zostać udostępnione do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="7bd6b-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="7bd6b-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-251">`TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="7bd6b-252">Jest dodawany do bieżącego czasu, aby utworzyć czas wygaśnięcia biletu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="7bd6b-253">Aby użyć `ExpireTimeSpan`, należy ustawić `IsPersistent` do `true` w `AuthenticationProperties` przekazany do `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="7bd6b-254">Wartość domyślna to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="7bd6b-255">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="7bd6b-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="7bd6b-256">Flaga wskazująca, czy data wygaśnięcia pliku cookie resetuje, gdy więcej niż połowy `ExpireTimeSpan` upływie interwału.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="7bd6b-257">Nowy termin exipiration jest przenoszony do przodu do bieżącej daty plus `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="7bd6b-258">[Czas wygaśnięcia bezwzględną pliku cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić za pomocą `AuthenticationProperties` klasy podczas wywoływania `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="7bd6b-259">Czas wygaśnięcia bezwzględny może zwiększyć zabezpieczenia aplikacji, ograniczając ilość czasu, który plik cookie uwierzytelniania jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="7bd6b-260">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-260">The default value is `true`.</span></span> |

<span data-ttu-id="7bd6b-261">Ustaw `CookieAuthenticationOptions` dla oprogramowania pośredniczącego uwierzytelniania pliku Cookie w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="7bd6b-262">Oprogramowaniu pośredniczącym pliku cookie zasad</span><span class="sxs-lookup"><span data-stu-id="7bd6b-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="7bd6b-263">[Oprogramowaniu pośredniczącym pliku cookie zasad](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) oferuje możliwości zasad plików cookie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="7bd6b-264">Dodawanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest kolejność liter; dotyczy tylko składniki zarejestrowane po nim w potoku.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="7bd6b-265">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) dostarczane z oprogramowaniem pośredniczącym pliku Cookie zasady umożliwiają kontrolowanie globalne właściwości przetwarzania plików cookie oraz podłączania do obsługi przetwarzania plików cookie, pliki cookie są dołączane lub usunięciu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="7bd6b-266">Właściwość</span><span class="sxs-lookup"><span data-stu-id="7bd6b-266">Property</span></span> | <span data-ttu-id="7bd6b-267">Opis</span><span class="sxs-lookup"><span data-stu-id="7bd6b-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="7bd6b-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="7bd6b-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="7bd6b-269">Ma wpływ na, czy pliki cookie musi być HttpOnly, który jest Flaga wskazująca, jeśli plik cookie ma być dostępne tylko dla serwerów.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="7bd6b-270">Wartość domyślna to `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="7bd6b-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="7bd6b-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="7bd6b-272">Wpływa na atrybut o tej samej lokacji plik cookie (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="7bd6b-273">Wartość domyślna to `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="7bd6b-274">Ta opcja jest dostępna dla platformy ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="7bd6b-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="7bd6b-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="7bd6b-276">Wywołuje się, gdy jest dołączany plik cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="7bd6b-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="7bd6b-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="7bd6b-278">Wywołuje się, gdy plik cookie zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="7bd6b-279">Zabezpieczanie</span><span class="sxs-lookup"><span data-stu-id="7bd6b-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="7bd6b-280">Wpływa na pliki cookie musi być bezpieczny.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="7bd6b-281">Wartość domyślna to `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="7bd6b-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span><span class="sxs-lookup"><span data-stu-id="7bd6b-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="7bd6b-283">Wartość domyślna `MinimumSameSitePolicy` wartość `SameSiteMode.Lax` pozwalające uwierzytelniania OAuth2.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="7bd6b-284">Ściśle wymusić zasady tej samej lokacji `SameSiteMode.Strict`ustaw `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="7bd6b-285">Mimo że to ustawienie dzieli OAuth2 i innych schematów uwierzytelniania między źródłami, eksponuje poziom bezpieczeństwa pliku cookie dla innych typów aplikacji, które nie należy polegać na przetwarzanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="7bd6b-286">Ustawienie oprogramowaniu pośredniczącym pliku Cookie zasad `MinimumSameSitePolicy` mogą mieć wpływ na Twoje ustawienia `Cookie.SameSite` w `CookieAuthenticationOptions` ustawienia zgodnie z poniższym macierzy.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="7bd6b-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="7bd6b-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="7bd6b-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="7bd6b-288">Cookie.SameSite</span></span> | <span data-ttu-id="7bd6b-289">Wynikowe ustawienie Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="7bd6b-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="7bd6b-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7bd6b-290">SameSiteMode.None</span></span>     | <span data-ttu-id="7bd6b-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7bd6b-291">SameSiteMode.None</span></span><br><span data-ttu-id="7bd6b-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7bd6b-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="7bd6b-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="7bd6b-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7bd6b-294">SameSiteMode.None</span></span><br><span data-ttu-id="7bd6b-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7bd6b-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="7bd6b-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="7bd6b-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7bd6b-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="7bd6b-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7bd6b-298">SameSiteMode.None</span></span><br><span data-ttu-id="7bd6b-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7bd6b-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="7bd6b-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="7bd6b-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7bd6b-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="7bd6b-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7bd6b-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="7bd6b-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="7bd6b-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="7bd6b-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7bd6b-305">SameSiteMode.None</span></span><br><span data-ttu-id="7bd6b-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7bd6b-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="7bd6b-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="7bd6b-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="7bd6b-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="7bd6b-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7bd6b-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="7bd6b-311">Tworzenie pliku cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="7bd6b-311">Create an authentication cookie</span></span>

<span data-ttu-id="7bd6b-312">Aby utworzyć plik cookie zawierający informacje o użytkowniku, należy tworzyć [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="7bd6b-313">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-313">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bd6b-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7bd6b-315">Tworzenie [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) z każdego wymaganego [oświadczenia](/dotnet/api/system.security.claims.claim)s, a następnie wywołać [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) do logowania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-315">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bd6b-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7bd6b-317">Wywołaj [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) do logowania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-317">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="7bd6b-318">`SignInAsync` Tworzy zaszyfrowanego pliku cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-318">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="7bd6b-319">Jeśli nie określisz `AuthenticationScheme`, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-319">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="7bd6b-320">Dzieje się w tle, szyfrowanie, używana jest platforma ASP.NET Core [ochrony danych](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) systemu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-320">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="7bd6b-321">Jeśli hostujesz aplikację na wielu maszynach, równoważenia obciążenia w aplikacjach lub przy użyciu farmy sieci web, a następnie należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) korzystać z tej samej pierścień klucz i identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-321">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="7bd6b-322">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="7bd6b-322">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bd6b-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7bd6b-324">Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="7bd6b-324">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bd6b-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7bd6b-326">Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="7bd6b-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="7bd6b-327">Jeśli nie używasz `CookieAuthenticationDefaults.AuthenticationScheme` (lub "Plików cookie" ") jako systemu (na przykład" ContosoCookie"), podaj Schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-327">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="7bd6b-328">W przeciwnym razie używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-328">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="7bd6b-329">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="7bd6b-329">React to back-end changes</span></span>

<span data-ttu-id="7bd6b-330">Po utworzeniu pliku cookie staje się pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-330">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="7bd6b-331">Nawet jeśli użytkownik zostanie wyłączone w systemach zaplecza, system plików cookie uwierzytelniania nie ma informacji o tym, a użytkownik pozostaje zalogowany, tak długo, jak ich plik cookie jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-331">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="7bd6b-332">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) zdarzeń w programie ASP.NET Core 2.x lub [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metodę w programie ASP.NET Core 1.x może służyć do przechwycenia, a następnie zastąpić weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-332">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="7bd6b-333">Takie podejście zmniejsza ryzyko odwołanych użytkownikom dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-333">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="7bd6b-334">Jedno z podejść do weryfikacji opiera się na rejestrowanie informacji o bazie danych użytkownika po zmianie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-334">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="7bd6b-335">Jeśli baza danych nie zostały zmodyfikowane, ponieważ został wystawiony przez użytkownika w pliku cookie, nie ma potrzeby ponownego uwierzytelnienia użytkownika, jeśli ich plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-335">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="7bd6b-336">Aby zrealizować ten scenariusz, w bazie danych, która jest zaimplementowana w `IUserRepository` w tym przykładzie przechowuje `LastChanged` wartość.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-336">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="7bd6b-337">Po zaktualizowaniu dowolnego użytkownika w bazie danych `LastChanged` wartość jest ustawiona na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-337">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="7bd6b-338">W celu unieważnienia pliku cookie, gdy na podstawie zmian w bazie danych `LastChanged` wartość, należy utworzyć plik cookie z `LastChanged` oświadczenia, zawierającego bieżącego `LastChanged` wartość z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-338">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bd6b-339">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-339">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7bd6b-340">Aby zaimplementować zastąpienie `ValidatePrincipal` zdarzeń, napisz metody z następującą sygnaturą w klasie, która pochodzi z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="7bd6b-340">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="7bd6b-341">Przykład wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-341">An example looks like the following:</span></span>

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

<span data-ttu-id="7bd6b-342">Rejestrowanie wystąpienia zdarzenia podczas rejestracji usługa plików cookie w `ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-342">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="7bd6b-343">Podaj rejestracji usługi o określonym zakresie z `CustomCookieAuthenticationEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-343">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bd6b-344">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-344">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7bd6b-345">Aby zaimplementować zastąpienie `ValidateAsync` zdarzeń, napisz metody z następującą sygnaturą:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-345">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="7bd6b-346">Tożsamości platformy ASP.NET Core to sprawdzenie są implementowane jako część jej [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="7bd6b-346">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="7bd6b-347">Przykład wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-347">An example looks like the following:</span></span>

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

<span data-ttu-id="7bd6b-348">Rejestrowanie zdarzeń podczas konfigurowania uwierzytelniania plików cookie w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="7bd6b-348">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="7bd6b-349">Rozważmy sytuację, w którym nazwa użytkownika jest aktualizowana &mdash; decyzji, który nie ma wpływu na zabezpieczenia w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-349">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="7bd6b-350">Jeśli chcesz zaktualizować bezpieczny głównej nazwy użytkownika, wywołaj `context.ReplacePrincipal` i ustaw `context.ShouldRenew` właściwość `true`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-350">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="7bd6b-351">W sposób opisany poniżej są wyzwalane na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-351">The approach described here is triggered on every request.</span></span> <span data-ttu-id="7bd6b-352">Może to spowodować spadek wydajności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-352">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="7bd6b-353">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="7bd6b-353">Persistent cookies</span></span>

<span data-ttu-id="7bd6b-354">Możesz chcieć plików cookie, aby zachować w wielu sesjach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-354">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="7bd6b-355">Ten stan trwały można włączyć tylko za zgodą użytkownika za pomocą checkbox "Pamiętaj mnie" logowania lub podobny mechanizm.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-355">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="7bd6b-356">Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który przeżyje za pośrednictwem przeglądarki zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-356">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="7bd6b-357">Ustawienia wygaśnięcia przewijania wcześniej skonfigurowane są uznawane.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-357">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="7bd6b-358">Jeśli plik cookie wygasa, gdy nastąpi zamknięcie okna przeglądarki, przeglądarki usuwa plik cookie po ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-358">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bd6b-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="7bd6b-360">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) klasa znajduje się w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-360">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bd6b-361">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-361">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="7bd6b-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) klasa znajduje się w `Microsoft.AspNetCore.Http.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="7bd6b-363">Datę ważności pliku cookie bezwzględne</span><span class="sxs-lookup"><span data-stu-id="7bd6b-363">Absolute cookie expiration</span></span>

<span data-ttu-id="7bd6b-364">Można ustawić czasu wygaśnięcia bezwzględne za pomocą `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-364">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="7bd6b-365">Należy także ustawić `IsPersistent`; w przeciwnym razie `ExpiresUtc` jest ignorowany i zostanie utworzony plik cookie sesji dla jednego.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-365">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="7bd6b-366">Gdy `ExpiresUtc` jest ustawiona na `SignInAsync`, zastępuje ona wartość `ExpireTimeSpan` opcji `CookieAuthenticationOptions`, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-366">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="7bd6b-367">Poniższy fragment kodu tworzy tożsamości i odpowiedniego pliku cookie, który jest ważny przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-367">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="7bd6b-368">To ignoruje wszelkie przewijania wcześniej skonfigurowane ustawienia wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="7bd6b-368">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bd6b-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bd6b-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bd6b-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

---

## <a name="additional-resources"></a><span data-ttu-id="7bd6b-371">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7bd6b-371">Additional resources</span></span>

* [<span data-ttu-id="7bd6b-372">Zmiany uwierzytelniania w wersji 2.0 / migracji ogłoszenia</span><span class="sxs-lookup"><span data-stu-id="7bd6b-372">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="7bd6b-373">Rola oparta na zasadach kontroli</span><span class="sxs-lookup"><span data-stu-id="7bd6b-373">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
