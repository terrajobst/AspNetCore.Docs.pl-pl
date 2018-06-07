---
title: Użyj plików cookie uwierzytelniania bez ASP.NET Core Identity
author: rick-anderson
description: Wyjaśnienie przy użyciu uwierzytelniania plików cookie bez ASP.NET Core Identity
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: 82f826bbc2bb19339851d5e25c539ea2c03acfb3
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819113"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="8f22c-103">Użyj plików cookie uwierzytelniania bez ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="8f22c-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="8f22c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8f22c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8f22c-105">Jak przedstawiono w starszych tematy dotyczące uwierzytelniania [ASP.NET Core Identity](xref:security/authentication/identity) jest dostawcą uwierzytelniania pełną, kompletne za tworzenie i obsługę logowania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="8f22c-106">Można jednak logiki niestandardowe uwierzytelnianie za pomocą pliku cookie uwierzytelniania w czasie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="8f22c-107">Jako autonomiczny dostawca uwierzytelniania bez ASP.NET Core Identity, można użyć uwierzytelniania opartego na pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="8f22c-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8f22c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8f22c-109">Dla celów demonstracyjnych w przykładowej aplikacji konto użytkownika dla użytkownika hipotetyczny, Maria Rodriguez jest zapisane na stałe w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f22c-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="8f22c-110">Użyj nazwy użytkownika wiadomości E-mail "maria.rodriguez@contoso.com" i wszelkich hasło do logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8f22c-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="8f22c-111">Użytkownik jest uwierzytelniany w `AuthenticateUser` metody w *Pages/Account/Login.cshtml.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="8f22c-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="8f22c-112">W przykładzie rzeczywistych użytkownik będzie uwierzytelniony w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8f22c-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="8f22c-113">Aby uzyskać informacje dotyczące migrowania uwierzytelniania opartego na pliku cookie z platformy ASP.NET Core 1.x 2.0, zobacz [migracji uwierzytelnianie i tożsamość do tematu ASP.NET Core 2.0 (uwierzytelnianie na podstawie plików Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="8f22c-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="8f22c-114">Aby korzystać z ASP.NET Core Identity, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity) tematu.</span><span class="sxs-lookup"><span data-stu-id="8f22c-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="8f22c-115">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="8f22c-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8f22c-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="8f22c-117">Jeśli aplikacja nie używać [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), Utwórz odwołanie do pakietu w pliku projektu dla [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu (wersja 2.1.0 lub później).</span><span class="sxs-lookup"><span data-stu-id="8f22c-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="8f22c-118">W `ConfigureServices` metody, Utwórz usługę oprogramowania pośredniczącego uwierzytelniania za pomocą `AddAuthentication` i `AddCookie` metod:</span><span class="sxs-lookup"><span data-stu-id="8f22c-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="8f22c-119">`AuthenticationScheme` przekazany do `AddAuthentication` ustawia domyślny schemat uwierzytelniania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f22c-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="8f22c-120">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania plików cookie i chcesz [autoryzacji z określonym schematem](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="8f22c-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="8f22c-121">Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" schematu.</span><span class="sxs-lookup"><span data-stu-id="8f22c-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="8f22c-122">Możesz podać dowolną wartość ciągu, która odróżnia schemat.</span><span class="sxs-lookup"><span data-stu-id="8f22c-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="8f22c-123">W `Configure` metody, użyj `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="8f22c-123">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="8f22c-124">Wywołanie `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="8f22c-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="8f22c-125">**Opcje AddCookie**</span><span class="sxs-lookup"><span data-stu-id="8f22c-125">**AddCookie Options**</span></span>

<span data-ttu-id="8f22c-126">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-126">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="8f22c-127">Opcja</span><span class="sxs-lookup"><span data-stu-id="8f22c-127">Option</span></span> | <span data-ttu-id="8f22c-128">Opis</span><span class="sxs-lookup"><span data-stu-id="8f22c-128">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="8f22c-129">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="8f22c-129">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-130">Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania), gdy wyzwalana przez `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-130">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="8f22c-131">Wartość domyślna to `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-131">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="8f22c-132">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="8f22c-132">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-133">Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość wszelkie oświadczenia utworzone przez usługę uwierzytelniania pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-133">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="8f22c-134">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="8f22c-134">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-135">Nazwa domeny, w której plik cookie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="8f22c-135">The domain name where the cookie is served.</span></span> <span data-ttu-id="8f22c-136">Domyślnie jest to nazwa hosta z żądania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-136">By default, this is the host name of the request.</span></span> <span data-ttu-id="8f22c-137">Przeglądarka wysyła tylko plik cookie w żądaniach pasujące nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="8f22c-137">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="8f22c-138">Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-138">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="8f22c-139">Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia do `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-139">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="8f22c-140">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="8f22c-140">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-141">Pobiera lub ustawia informacje o czasie pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-141">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="8f22c-142">Obecnie to opcja ops nie i staną się nieaktualne w ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="8f22c-142">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="8f22c-143">Użyj `ExpireTimeSpan` opcję, aby określić datę ważności pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-143">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="8f22c-144">Aby uzyskać więcej informacji, zobacz [wyjaśnić zachowanie CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="8f22c-144">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="8f22c-145">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="8f22c-145">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-146">Flaga wskazująca, czy plik cookie powinien być dostępny tylko na serwerach.</span><span class="sxs-lookup"><span data-stu-id="8f22c-146">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="8f22c-147">Zmiana tej wartości do `false` zezwala na dostęp do pliku cookie skryptów po stronie klienta i może otworzyć aplikację, aby kradzieży plików cookie powinien mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="8f22c-147">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="8f22c-148">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-148">The default value is `true`.</span></span> |
| [<span data-ttu-id="8f22c-149">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="8f22c-149">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-150">Ustawia nazwę pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-150">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="8f22c-151">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="8f22c-151">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-152">Używane do izolowania aplikacji działających na takiej samej nazwie hosta.</span><span class="sxs-lookup"><span data-stu-id="8f22c-152">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="8f22c-153">Jeśli masz aplikację, w którym uruchomiono `/app1` i chcesz ograniczyć plików cookie do tej aplikacji, ustaw `CookiePath` właściwości `/app1`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-153">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="8f22c-154">W ten sposób plik cookie jest dostępny tylko dla żądań wysyłanych na `/app1` i wszystkich aplikacji znajdujących się pod nim.</span><span class="sxs-lookup"><span data-stu-id="8f22c-154">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="8f22c-155">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="8f22c-155">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-156">Wskazuje, czy przeglądarka powinna zezwalać plik cookie jest dołączony do tej samej lokacji tylko żądania (`SameSiteMode.Strict`) lub przy użyciu tej samej lokacji żądań i bezpieczne metody HTTP żądania między witrynami (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="8f22c-156">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="8f22c-157">Jeśli wartość `SameSiteMode.None`, nie jest ustawiona wartość nagłówka pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-157">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="8f22c-158">Należy pamiętać, że [oprogramowaniu pośredniczącym pliku Cookie zasad](#cookie-policy-middleware) może zastąpić wartość, którą należy podać.</span><span class="sxs-lookup"><span data-stu-id="8f22c-158">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="8f22c-159">Aby zapewnić obsługę uwierzytelniania OAuth, wartością domyślną jest `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-159">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="8f22c-160">Aby uzyskać więcej informacji, zobacz [uwierzytelniania OAuth przerwane z powodu zasad pliku cookie SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="8f22c-160">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="8f22c-161">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="8f22c-161">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-162">Flaga wskazująca, czy utworzony plik cookie powinien być ograniczony do HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="8f22c-162">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="8f22c-163">Wartość domyślna to `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-163">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="8f22c-164">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="8f22c-164">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-165">Ustawia `DataProtectionProvider` używany do tworzenia domyślnie `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-165">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="8f22c-166">Jeśli `TicketDataFormat` właściwość jest ustawiona, `DataProtectionProvider` nie jest używana opcja.</span><span class="sxs-lookup"><span data-stu-id="8f22c-166">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="8f22c-167">Jeśli nie zostanie podana, jest używany domyślny dostawca ochrony danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f22c-167">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="8f22c-168">Zdarzenia</span><span class="sxs-lookup"><span data-stu-id="8f22c-168">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-169">Program obsługi wywołuje metody dla dostawcy, który mieć kontrolę aplikacji w niektórych punktach przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-169">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="8f22c-170">Jeśli `Events` nie są dostarczane podano domyślnego wystąpienia, które nie działają, gdy metody są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="8f22c-170">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="8f22c-171">EventsType</span><span class="sxs-lookup"><span data-stu-id="8f22c-171">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-172">Używane jako typ usługi, aby uzyskać `Events` wystąpienia zamiast właściwości.</span><span class="sxs-lookup"><span data-stu-id="8f22c-172">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="8f22c-173">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="8f22c-173">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-174">`TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-174">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="8f22c-175">`ExpireTimeSpan` zostanie dodany do bieżącego czasu można utworzyć czas wygaśnięcia biletu.</span><span class="sxs-lookup"><span data-stu-id="8f22c-175">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="8f22c-176">`ExpiredTimeSpan` Wartość zawsze przechodzi w stan AuthTicket zaszyfrowane, zweryfikowane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="8f22c-176">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="8f22c-177">Może ona także przejść do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) nagłówka, ale tylko wtedy, gdy `IsPersistent` jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="8f22c-177">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="8f22c-178">Aby ustawić `IsPersistent` do `true`, skonfiguruj [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) przekazany do `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-178">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="8f22c-179">Wartość domyślna `ExpireTimeSpan` 14 dni.</span><span class="sxs-lookup"><span data-stu-id="8f22c-179">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="8f22c-180">LoginPath</span><span class="sxs-lookup"><span data-stu-id="8f22c-180">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-181">Określa ścieżkę do dostarczania 302 Found (adres URL przekierowania), gdy wyzwalana przez `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-181">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="8f22c-182">Bieżący adres URL, który wygenerował kod 401, jest dodawany do `LoginPath` jako parametr ciągu zapytania o nazwie `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-182">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="8f22c-183">Raz żądanie `LoginPath` nadaje nową tożsamość logowania, `ReturnUrlParameter` wartość jest używana do przekierowania przeglądarki z powrotem do adresu URL, który spowodował oryginalnego kodu stanu nieautoryzowanego.</span><span class="sxs-lookup"><span data-stu-id="8f22c-183">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="8f22c-184">Wartość domyślna to `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-184">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="8f22c-185">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="8f22c-185">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-186">Jeśli `LogoutPath` podano do programu obsługi, a następnie przekierowuje żądanie do tej ścieżki na podstawie wartości z `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-186">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="8f22c-187">Wartość domyślna to `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-187">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="8f22c-188">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="8f22c-188">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-189">Określa nazwę parametru ciągu zapytania, która jest dołączana przez procedurę obsługi dla odpowiedzi 302 Found (adres URL przekierowania).</span><span class="sxs-lookup"><span data-stu-id="8f22c-189">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="8f22c-190">`ReturnUrlParameter` jest używany, gdy żądanie dociera na `LoginPath` lub `LogoutPath` aby wrócić do oryginalnego adresu URL w przeglądarce, po wykonaniu akcji logowanie lub wylogowanie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-190">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="8f22c-191">Wartość domyślna to `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-191">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="8f22c-192">SessionStore</span><span class="sxs-lookup"><span data-stu-id="8f22c-192">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-193">Opcjonalny kontener używany do przechowywania tożsamości dla żądań.</span><span class="sxs-lookup"><span data-stu-id="8f22c-193">An optional container used to store identity across requests.</span></span> <span data-ttu-id="8f22c-194">W przypadku identyfikatora sesji są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="8f22c-194">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="8f22c-195">`SessionStore` można wyeliminować potencjalne problemy z tożsamościami duże.</span><span class="sxs-lookup"><span data-stu-id="8f22c-195">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="8f22c-196">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="8f22c-196">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-197">Flaga wskazująca, czy nowy plik cookie z czasem wygaśnięcia zaktualizowane powinien zostać wystawiony dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-197">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="8f22c-198">Może to się zdarzyć na każde żądanie, w którym bieżącego okresu wygaśnięcia pliku cookie jest więcej niż 50% wygasł.</span><span class="sxs-lookup"><span data-stu-id="8f22c-198">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="8f22c-199">Nowa data wygaśnięcia została przeniesiona do przodu do była bieżąca data i `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-199">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="8f22c-200">[Czas wygaśnięcia bezwzględne cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić przy użyciu `AuthenticationProperties` klasy podczas wywoływania metody `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-200">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="8f22c-201">Czas wygaśnięcia bezwzględne zwiększyć bezpieczeństwo aplikacji poprzez ograniczenie ilość czasu, który jest prawidłowy plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-201">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="8f22c-202">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-202">The default value is `true`.</span></span> |
| [<span data-ttu-id="8f22c-203">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="8f22c-203">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-204">`TicketDataFormat` Służy do ustawiania i usuwania ochrony tożsamości i innych właściwości, które są przechowywane w wartości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-204">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="8f22c-205">Jeśli nie zostanie podana, `TicketDataFormat` jest tworzony przy użyciu [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="8f22c-205">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="8f22c-206">Sprawdzanie poprawności</span><span class="sxs-lookup"><span data-stu-id="8f22c-206">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="8f22c-207">Metoda, która sprawdza, czy ustawienia są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="8f22c-207">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="8f22c-208">Ustaw `CookieAuthenticationOptions` w konfiguracji usługi dla uwierzytelniania w `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="8f22c-208">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8f22c-209">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-209">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="8f22c-210">Plik cookie używa 1.x platformy ASP.NET Core [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) serializujący głównej nazwy użytkownika do zaszyfrowanego pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="8f22c-211">Kolejne żądania plik cookie jest weryfikowana i ponownie utworzyć podmiot zabezpieczeń i ma przypisaną do `HttpContext.User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="8f22c-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="8f22c-212">Zainstaluj [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pakietu NuGet w projekcie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="8f22c-213">Ten pakiet zawiera oprogramowaniu pośredniczącym pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="8f22c-214">Użyj `UseCookieAuthentication` metody w `Configure` metody w Twojej *Startup.cs* pliku przed `UseMvc` lub `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="8f22c-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="8f22c-215">**Opcje CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="8f22c-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="8f22c-216">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) klasa jest używana do konfigurowania opcji dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="8f22c-217">Opcja</span><span class="sxs-lookup"><span data-stu-id="8f22c-217">Option</span></span> | <span data-ttu-id="8f22c-218">Opis</span><span class="sxs-lookup"><span data-stu-id="8f22c-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="8f22c-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="8f22c-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-220">Ustawia schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-220">Sets the authentication scheme.</span></span> <span data-ttu-id="8f22c-221">`AuthenticationScheme` jest przydatne, gdy istnieje wiele wystąpień uwierzytelniania i chcesz [autoryzacji z określonym schematem](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="8f22c-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="8f22c-222">Ustawienie `AuthenticationScheme` do `CookieAuthenticationDefaults.AuthenticationScheme` zawiera wartość "Plików cookie" schematu.</span><span class="sxs-lookup"><span data-stu-id="8f22c-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="8f22c-223">Możesz podać dowolną wartość ciągu, która odróżnia schemat.</span><span class="sxs-lookup"><span data-stu-id="8f22c-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="8f22c-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="8f22c-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-225">Ustawia wartość oznacza, że uwierzytelniania plików cookie należy uruchomić na każde żądanie i próbę zweryfikowania i rekonstrukcji serializacji podmiot zabezpieczeń, utworzonych.</span><span class="sxs-lookup"><span data-stu-id="8f22c-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="8f22c-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="8f22c-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-227">Jeśli PRAWDA, oprogramowanie pośredniczące uwierzytelniania obsługuje automatyczne wyzwania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="8f22c-228">Jeśli ma wartość FAŁSZ, oprogramowanie pośredniczące uwierzytelniania zmienia tylko odpowiedzi, gdy jest to wyraźnie wskazane przez `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="8f22c-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="8f22c-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-230">Wystawca używany dla [wystawcy](/dotnet/api/system.security.claims.claim.issuer) właściwość wszelkie oświadczenia utworzone przez oprogramowanie pośredniczące uwierzytelniania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="8f22c-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="8f22c-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-232">Nazwa domeny, w której plik cookie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="8f22c-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="8f22c-233">Domyślnie jest to nazwa hosta z żądania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="8f22c-234">Przeglądarka służy tylko pliku cookie do dopasowania nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="8f22c-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="8f22c-235">Możesz dostosować, aby pliki cookie dostępne dla każdego hosta w domenie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="8f22c-236">Na przykład ustawienie domeny pliku cookie `.contoso.com` udostępnia do `contoso.com`, `www.contoso.com`, i `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="8f22c-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="8f22c-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-238">Flaga wskazująca, czy plik cookie powinien być dostępny tylko na serwerach.</span><span class="sxs-lookup"><span data-stu-id="8f22c-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="8f22c-239">Zmiana tej wartości do `false` zezwala na dostęp do pliku cookie skryptów po stronie klienta i może otworzyć aplikację, aby kradzieży plików cookie powinien mieć aplikację [skryptów między witrynami (XSS)](xref:security/cross-site-scripting) luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="8f22c-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="8f22c-240">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="8f22c-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="8f22c-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-242">Używane do izolowania aplikacji działających na takiej samej nazwie hosta.</span><span class="sxs-lookup"><span data-stu-id="8f22c-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="8f22c-243">Jeśli masz aplikację, w którym uruchomiono `/app1` i chcesz ograniczyć plików cookie do tej aplikacji, ustaw `CookiePath` właściwości `/app1`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="8f22c-244">W ten sposób plik cookie jest dostępny tylko dla żądań wysyłanych na `/app1` i wszystkich aplikacji znajdujących się pod nim.</span><span class="sxs-lookup"><span data-stu-id="8f22c-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="8f22c-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="8f22c-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-246">Flaga wskazująca, czy utworzony plik cookie powinien być ograniczony do HTTPS (`CookieSecurePolicy.Always`), protokołu HTTP lub HTTPS (`CookieSecurePolicy.None`), lub ten sam protokół jako żądania (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="8f22c-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="8f22c-247">Wartość domyślna to `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="8f22c-248">Opis</span><span class="sxs-lookup"><span data-stu-id="8f22c-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-249">Dodatkowe informacje o typie uwierzytelniania, który ma zostać udostępnione do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f22c-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="8f22c-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="8f22c-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-251">`TimeSpan` Po którym wygaśnięcia biletu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="8f22c-252">Jest ona dodawana do bieżącego czasu, aby utworzyć czas wygaśnięcia biletu.</span><span class="sxs-lookup"><span data-stu-id="8f22c-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="8f22c-253">Aby użyć `ExpireTimeSpan`, należy ustawić `IsPersistent` do `true` w `AuthenticationProperties` przekazany do `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="8f22c-254">Wartość domyślna to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="8f22c-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="8f22c-255">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="8f22c-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="8f22c-256">Flaga wskazująca, czy data wygaśnięcia pliku cookie resetuje kiedy więcej niż połowy `ExpireTimeSpan` upływie interwału.</span><span class="sxs-lookup"><span data-stu-id="8f22c-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="8f22c-257">Nowy termin exipiration została przeniesiona do przodu do była bieżąca data i `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="8f22c-258">[Czas wygaśnięcia bezwzględne cookie](xref:security/authentication/cookie#absolute-cookie-expiration) można ustawić przy użyciu `AuthenticationProperties` klasy podczas wywoływania metody `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="8f22c-259">Czas wygaśnięcia bezwzględne zwiększyć bezpieczeństwo aplikacji poprzez ograniczenie ilość czasu, który jest prawidłowy plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="8f22c-260">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-260">The default value is `true`.</span></span> |

<span data-ttu-id="8f22c-261">Ustaw `CookieAuthenticationOptions` dla oprogramowania pośredniczącego uwierzytelniania pliku Cookie w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="8f22c-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="8f22c-262">Oprogramowanie pośredniczące zasad pliku cookie</span><span class="sxs-lookup"><span data-stu-id="8f22c-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="8f22c-263">[Oprogramowanie pośredniczące zasad pliku cookie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) włącza funkcje zasad pliku cookie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f22c-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="8f22c-264">Dodawanie oprogramowania pośredniczącego do potoku przetwarzania aplikacji jest kolejność liter; wpływa tylko na składniki zarejestrowane po nim w potoku.</span><span class="sxs-lookup"><span data-stu-id="8f22c-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="8f22c-265">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) dostarczony do pliku Cookie oprogramowanie pośredniczące zasady umożliwiają kontrolowanie globalnych właściwości przetwarzania pliku cookie i haku do obsługi przetwarzania pliku cookie, gdy pliki cookie są dołączane lub usuwane.</span><span class="sxs-lookup"><span data-stu-id="8f22c-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="8f22c-266">Właściwość</span><span class="sxs-lookup"><span data-stu-id="8f22c-266">Property</span></span> | <span data-ttu-id="8f22c-267">Opis</span><span class="sxs-lookup"><span data-stu-id="8f22c-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="8f22c-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="8f22c-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="8f22c-269">Dotyczy, czy pliki cookie musi być HttpOnly, który jest flagę wskazującą, jeśli plik cookie powinien być dostępny tylko na serwerach.</span><span class="sxs-lookup"><span data-stu-id="8f22c-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="8f22c-270">Wartość domyślna to `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="8f22c-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="8f22c-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="8f22c-272">Wpływa na plik cookie tej samej lokacji atrybutu (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="8f22c-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="8f22c-273">Wartość domyślna to `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="8f22c-274">Ta opcja jest dostępna dla platformy ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="8f22c-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="8f22c-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="8f22c-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="8f22c-276">Wywoływane, gdy jest dołączany plik cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="8f22c-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="8f22c-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="8f22c-278">Wywoływane, gdy plik cookie zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="8f22c-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="8f22c-279">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="8f22c-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="8f22c-280">Wpływa na pliki cookie musi być zabezpieczona.</span><span class="sxs-lookup"><span data-stu-id="8f22c-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="8f22c-281">Wartość domyślna to `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="8f22c-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span><span class="sxs-lookup"><span data-stu-id="8f22c-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="8f22c-283">Wartość domyślna `MinimumSameSitePolicy` wartość jest `SameSiteMode.Lax` umożliwiające uwierzytelniania OAuth2.</span><span class="sxs-lookup"><span data-stu-id="8f22c-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="8f22c-284">Ściśle wymuszać zasady tej samej lokacji `SameSiteMode.Strict`ustaw `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="8f22c-285">Mimo że to ustawienie dzieli OAuth2 i inne schematy uwierzytelniania między źródłami, eksponuje poziom zabezpieczeń plików cookie dla innych typów aplikacji, które nie należy polegać na przetwarzanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8f22c-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="8f22c-286">Ustawienie oprogramowaniu pośredniczącym pliku Cookie zasad `MinimumSameSitePolicy` mogą mieć wpływ na ustawienia z `Cookie.SameSite` w `CookieAuthenticationOptions` ustawienia zgodnie z poniższym macierzy.</span><span class="sxs-lookup"><span data-stu-id="8f22c-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="8f22c-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="8f22c-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="8f22c-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="8f22c-288">Cookie.SameSite</span></span> | <span data-ttu-id="8f22c-289">Wynikowe ustawienie Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="8f22c-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="8f22c-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="8f22c-290">SameSiteMode.None</span></span>     | <span data-ttu-id="8f22c-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="8f22c-291">SameSiteMode.None</span></span><br><span data-ttu-id="8f22c-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="8f22c-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="8f22c-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="8f22c-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="8f22c-294">SameSiteMode.None</span></span><br><span data-ttu-id="8f22c-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="8f22c-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="8f22c-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="8f22c-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="8f22c-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="8f22c-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="8f22c-298">SameSiteMode.None</span></span><br><span data-ttu-id="8f22c-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="8f22c-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="8f22c-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="8f22c-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="8f22c-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="8f22c-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="8f22c-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="8f22c-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="8f22c-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="8f22c-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="8f22c-305">SameSiteMode.None</span></span><br><span data-ttu-id="8f22c-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="8f22c-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="8f22c-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="8f22c-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="8f22c-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="8f22c-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="8f22c-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="8f22c-311">Tworzenie pliku cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="8f22c-311">Create an authentication cookie</span></span>

<span data-ttu-id="8f22c-312">Aby utworzyć plik cookie zawierający informacje o użytkownikach, należy tworzyć [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="8f22c-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="8f22c-313">Informacje o użytkowniku są serializowane i przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-313">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8f22c-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="8f22c-315">Utwórz [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) z każdego wymaganego [oświadczeń](/dotnet/api/system.security.claims.claim)s i wywołanie [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) do logowania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="8f22c-315">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8f22c-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="8f22c-317">Wywołanie [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) do logowania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="8f22c-317">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="8f22c-318">`SignInAsync` Tworzy zaszyfrowanego pliku cookie i dodaje go do bieżącej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8f22c-318">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="8f22c-319">Jeśli nie określisz `AuthenticationScheme`, używany jest domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="8f22c-319">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="8f22c-320">W obszarze obejmuje szyfrowania używany jest platformy ASP.NET Core [ochrony danych](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) systemu.</span><span class="sxs-lookup"><span data-stu-id="8f22c-320">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="8f22c-321">Jeśli przechowujesz aplikacji na wiele komputerów, równoważenia obciążenia w aplikacjach lub przy użyciu farmy sieci web, a następnie należy [skonfigurować ochronę danych](xref:security/data-protection/configuration/overview) korzystać z tej samej pierścień klucz i identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f22c-321">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="8f22c-322">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="8f22c-322">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8f22c-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="8f22c-324">Wyloguj z bieżącego użytkownika i usuń ich pliku cookie, wywołaj [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="8f22c-324">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8f22c-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="8f22c-326">Wyloguj z bieżącego użytkownika i usuń ich pliku cookie, wywołaj [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="8f22c-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="8f22c-327">Jeśli nie używasz `CookieAuthenticationDefaults.AuthenticationScheme` (lub "Plików cookie") jako systemu (na przykład "ContosoCookie"), podaj Schemat używany podczas konfigurowania dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8f22c-327">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="8f22c-328">W przeciwnym razie jest używany domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="8f22c-328">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="8f22c-329">Reagowanie na zmiany zaplecza</span><span class="sxs-lookup"><span data-stu-id="8f22c-329">React to back-end changes</span></span>

<span data-ttu-id="8f22c-330">Po utworzeniu pliku cookie, staje się pojedynczym źródłem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="8f22c-330">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="8f22c-331">Nawet wtedy, gdy użytkownik jest wyłączone w przez systemy zaplecza, system plików cookie uwierzytelniania nie ma informacji o tym, a użytkownik pozostaje zalogowany, tak długo, jak ich plik cookie jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="8f22c-331">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="8f22c-332">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) zdarzenie platformy ASP.NET Core 2.x lub [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metoda platformy ASP.NET Core 1.x, można przechwytywać i zastąpić weryfikacji tożsamości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-332">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="8f22c-333">Takie podejście zmniejsza ryzyko odwołane użytkowników uzyskujących dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f22c-333">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="8f22c-334">Jeden ze sposobów sprawdzania poprawności pliku cookie jest oparta na rejestrowanie informacji o bazie danych użytkownika po zmianie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-334">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="8f22c-335">Jeśli bazy danych nie został zmodyfikowany od czasu plików cookie użytkownika został wystawiony, jest niepotrzebna ponownego uwierzytelnienia użytkownika, jeśli ich plik cookie jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="8f22c-335">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="8f22c-336">Aby zaimplementować bazy danych, która jest zaimplementowana w tym scenariuszu `IUserRepository` w tym przykładzie przechowuje `LastChanged` wartość.</span><span class="sxs-lookup"><span data-stu-id="8f22c-336">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="8f22c-337">Po zaktualizowaniu każdy użytkownik w bazie danych, `LastChanged` wartość jest ustawiona na bieżącą godzinę.</span><span class="sxs-lookup"><span data-stu-id="8f22c-337">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="8f22c-338">W celu unieważnienia pliku cookie, gdy na podstawie zmian w bazie danych `LastChanged` wartość, Utwórz plik cookie z `LastChanged` oświadczeń, zawierający bieżącą `LastChanged` wartości z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="8f22c-338">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8f22c-339">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-339">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8f22c-340">Aby zaimplementować zastąpienie `ValidatePrincipal` zdarzeń, zapisu, a metoda z następującą sygnaturą w klasie, która pochodzi z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="8f22c-340">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="8f22c-341">Przykład wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="8f22c-341">An example looks like the following:</span></span>

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

<span data-ttu-id="8f22c-342">Zarejestruj wystąpienie zdarzenia podczas rejestracji usługi plików cookie w `ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="8f22c-342">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="8f22c-343">Podaj rejestracji usługi w zakresie sieci `CustomCookieAuthenticationEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="8f22c-343">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8f22c-344">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-344">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8f22c-345">Aby zaimplementować zastąpienie `ValidateAsync` zdarzeń, zapisu, a metoda z następującą sygnaturą:</span><span class="sxs-lookup"><span data-stu-id="8f22c-345">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="8f22c-346">ASP.NET Core Identity implementuje ten test jako część jej [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="8f22c-346">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="8f22c-347">Przykład wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="8f22c-347">An example looks like the following:</span></span>

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

<span data-ttu-id="8f22c-348">Zarejestruj zdarzenie podczas konfigurowania uwierzytelniania plików cookie w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="8f22c-348">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="8f22c-349">Rozważmy sytuację, w którym nazwa użytkownika jest aktualizowana &mdash; decyzja, który nie ma wpływu na zabezpieczenia w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="8f22c-349">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="8f22c-350">Jeśli chcesz zaktualizować bezpieczny głównej nazwy użytkownika, należy wywołać `context.ReplacePrincipal` i ustaw `context.ShouldRenew` właściwości `true`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-350">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="8f22c-351">W sposób opisany poniżej jest wyzwalane na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="8f22c-351">The approach described here is triggered on every request.</span></span> <span data-ttu-id="8f22c-352">Może to spowodować zmniejszenie wydajności dużych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f22c-352">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="8f22c-353">Trwałe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="8f22c-353">Persistent cookies</span></span>

<span data-ttu-id="8f22c-354">Możesz pliku cookie do utrwalenia w wielu sesjach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8f22c-354">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="8f22c-355">Ta trwałości powinna być włączona tylko za zgodą użytkownika jawne z checkbox "Zapamiętaj moje" na nazwy logowania lub podobny mechanizm.</span><span class="sxs-lookup"><span data-stu-id="8f22c-355">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="8f22c-356">Poniższy fragment kodu tworzy tożsamość i odpowiedniego pliku cookie, który przeżyje za pośrednictwem zamknięcia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8f22c-356">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="8f22c-357">Ustawienia wygaśnięcia przesuwanego wcześniej skonfigurowane są uznawane.</span><span class="sxs-lookup"><span data-stu-id="8f22c-357">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="8f22c-358">Jeśli plik cookie wygasa, gdy nastąpi zamknięcie okna przeglądarki, przeglądarki usuwa plik cookie po ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="8f22c-358">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8f22c-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="8f22c-360">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) klasa znajduje się w `Microsoft.AspNetCore.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="8f22c-360">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8f22c-361">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-361">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="8f22c-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) klasa znajduje się w `Microsoft.AspNetCore.Http.Authentication` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="8f22c-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="8f22c-363">Datę ważności pliku cookie bezwzględne</span><span class="sxs-lookup"><span data-stu-id="8f22c-363">Absolute cookie expiration</span></span>

<span data-ttu-id="8f22c-364">Można ustawić czas wygaśnięcia bezwzględne z `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="8f22c-364">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="8f22c-365">Należy także ustawić `IsPersistent`; w przeciwnym razie `ExpiresUtc` jest ignorowany i jest tworzony plik cookie sesji jednego.</span><span class="sxs-lookup"><span data-stu-id="8f22c-365">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="8f22c-366">Podczas `ExpiresUtc` jest ustawiona na `SignInAsync`, zastępuje on wartość `ExpireTimeSpan` opcji `CookieAuthenticationOptions`, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="8f22c-366">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="8f22c-367">Poniższy fragment kodu tworzy tożsamość i odpowiedniego pliku cookie, który jest ważny przez 20 minut.</span><span class="sxs-lookup"><span data-stu-id="8f22c-367">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="8f22c-368">To ignoruje wszystkie przesuwanego wcześniej skonfigurowane ustawienia wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="8f22c-368">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8f22c-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8f22c-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8f22c-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="additional-resources"></a><span data-ttu-id="8f22c-371">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8f22c-371">Additional resources</span></span>

* [<span data-ttu-id="8f22c-372">Zmiany uwierzytelniania 2.0 / migracji anonsu</span><span class="sxs-lookup"><span data-stu-id="8f22c-372">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="8f22c-373">Ograniczanie tożsamości według schematu</span><span class="sxs-lookup"><span data-stu-id="8f22c-373">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* [<span data-ttu-id="8f22c-374">Autoryzacja oparta na oświadczeniach</span><span class="sxs-lookup"><span data-stu-id="8f22c-374">Claims-Based Authorization</span></span>](xref:security/authorization/claims)
* [<span data-ttu-id="8f22c-375">Rola oparta na zasadach kontroli</span><span class="sxs-lookup"><span data-stu-id="8f22c-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
