---
title: Uniemożliwiaj ataki między witrynami (XSRF/CSRF) w ASP.NET Core
author: steve-smith
description: Dowiedz się, jak zapobiegać atakom na aplikacje sieci Web, w których złośliwa witryna sieci Web może mieć wpływ na interakcję między przeglądarką klienta a aplikacją.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2019
uid: security/anti-request-forgery
ms.openlocfilehash: c3d32a93bcbf9f3897c10e68ba4e43d269a3ea80
ms.sourcegitcommit: b3e1e31e5d8bdd94096cf27444594d4a7b065525
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74803373"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="8efc1-103">Uniemożliwiaj ataki między witrynami (XSRF/CSRF) w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8efc1-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="8efc1-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8efc1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8efc1-105">Sfałszowanie żądań między lokacjami (znane również jako XSRF lub CSRF) jest atakiem na aplikacje hostowane w sieci Web, dzięki czemu złośliwa aplikacja internetowa może mieć wpływ na interakcję między przeglądarką klienta a aplikacją sieci Web, która ufa tej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8efc1-105">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="8efc1-106">Te ataki są możliwe, ponieważ przeglądarki sieci Web wysyłają automatyczne typy tokenów uwierzytelniania przy użyciu każdego żądania do witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8efc1-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="8efc1-107">Ta forma wykorzystania jest również znana jako *atak dwukrotnego kliknięcia* lub *sesji* , ponieważ atakujący wykorzystuje wcześniej uwierzytelnioną sesję użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8efc1-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="8efc1-108">Przykład ataku CSRF:</span><span class="sxs-lookup"><span data-stu-id="8efc1-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="8efc1-109">Użytkownik loguje się `www.good-banking-site.com` przy użyciu uwierzytelniania formularzy.</span><span class="sxs-lookup"><span data-stu-id="8efc1-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="8efc1-110">Serwer uwierzytelnia użytkownika i wystawia odpowiedź obejmującą plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8efc1-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="8efc1-111">Lokacja jest narażona na ataki, ponieważ ufa każde żądanie odbierane z prawidłowym plikiem cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8efc1-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="8efc1-112">Użytkownik odwiedzi złośliwą witrynę `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="8efc1-113">Złośliwa witryna `www.bad-crook-site.com`, zawiera formularz HTML podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="8efc1-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="8efc1-114">Zwróć uwagę, że formularz `action` wpisów do zagrożonej witryny, a nie do złośliwej witryny.</span><span class="sxs-lookup"><span data-stu-id="8efc1-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="8efc1-115">To jest część "wiele witryn" z CSRF.</span><span class="sxs-lookup"><span data-stu-id="8efc1-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="8efc1-116">Użytkownik wybiera przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="8efc1-116">The user selects the submit button.</span></span> <span data-ttu-id="8efc1-117">Przeglądarka wysyła żądanie i automatycznie uwzględnia plik cookie uwierzytelniania dla żądanych domen, `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="8efc1-118">Żądanie jest uruchamiane na serwerze `www.good-banking-site.com` z kontekstem uwierzytelniania użytkownika i może wykonać dowolną akcję, którą może wykonać uwierzytelniony użytkownik.</span><span class="sxs-lookup"><span data-stu-id="8efc1-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="8efc1-119">Oprócz scenariusza, w którym użytkownik wybiera przycisk, aby przesłać formularz, złośliwa witryna może:</span><span class="sxs-lookup"><span data-stu-id="8efc1-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="8efc1-120">Uruchom skrypt, który automatycznie przesyła formularz.</span><span class="sxs-lookup"><span data-stu-id="8efc1-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="8efc1-121">Wyślij formularz przesyłania w postaci żądania AJAX.</span><span class="sxs-lookup"><span data-stu-id="8efc1-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="8efc1-122">Ukryj formularz przy użyciu CSS.</span><span class="sxs-lookup"><span data-stu-id="8efc1-122">Hide the form using CSS.</span></span>

<span data-ttu-id="8efc1-123">Te alternatywne scenariusze nie wymagają żadnych działań ani danych wejściowych od użytkownika innego niż początkowo odwiedzają złośliwe witryny.</span><span class="sxs-lookup"><span data-stu-id="8efc1-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="8efc1-124">Korzystanie z protokołu HTTPS nie uniemożliwia ataku CSRF.</span><span class="sxs-lookup"><span data-stu-id="8efc1-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="8efc1-125">Złośliwa lokacja może wysłać żądanie `https://www.good-banking-site.com/` równie łatwo, jak może wysłać niezabezpieczone żądanie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="8efc1-126">Niektóre ataki są ukierunkowanymi punktami końcowymi, które odpowiadają na żądania GET, w takim przypadku można użyć znacznika obrazu do wykonania akcji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="8efc1-127">Ta forma ataku jest wspólna w witrynach forum, które zezwalają na obrazy, ale blokują kod JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8efc1-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="8efc1-128">Aplikacje, które zmieniają stan w żądaniach GET, gdzie zmienne lub zasoby są zmieniane, są narażone na złośliwe ataki.</span><span class="sxs-lookup"><span data-stu-id="8efc1-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="8efc1-129">**Żądania GET, które zmieniają stan są niezabezpieczone. Najlepszym rozwiązaniem jest nigdy nie zmieniaj stanu żądania GET.**</span><span class="sxs-lookup"><span data-stu-id="8efc1-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="8efc1-130">Możliwe jest ataki CSRF na aplikacje sieci Web, które używają plików cookie do uwierzytelniania, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="8efc1-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="8efc1-131">Przeglądarki przechowują pliki cookie wystawione przez aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="8efc1-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="8efc1-132">Przechowywane pliki cookie obejmują pliki cookie sesji dla użytkowników uwierzytelnionych.</span><span class="sxs-lookup"><span data-stu-id="8efc1-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="8efc1-133">Przeglądarki wysyłają do aplikacji sieci Web wszystkie pliki cookie skojarzone z domeną, niezależnie od tego, w jaki sposób żądanie do aplikacji zostało wygenerowane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8efc1-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="8efc1-134">Jednak ataki CSRF nie ograniczają się do korzystania z plików cookie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="8efc1-135">Na przykład uwierzytelnianie podstawowe i szyfrowane jest również zagrożone.</span><span class="sxs-lookup"><span data-stu-id="8efc1-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="8efc1-136">Gdy użytkownik zaloguje się przy użyciu uwierzytelniania podstawowego lub szyfrowanego, przeglądarka automatycznie wyśle poświadczenia do momentu zakończenia sesji&dagger;.</span><span class="sxs-lookup"><span data-stu-id="8efc1-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="8efc1-137">&dagger;w tym kontekście *sesja* odwołuje się do sesji po stronie klienta, podczas której użytkownik jest uwierzytelniany.</span><span class="sxs-lookup"><span data-stu-id="8efc1-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="8efc1-138">Nie jest on związany z sesjami po stronie serwera lub [ASP.NET Core pośredniczących sesji](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="8efc1-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="8efc1-139">Użytkownicy mogą chronić przed lukami w CSRF, podejmując środki ostrożności:</span><span class="sxs-lookup"><span data-stu-id="8efc1-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="8efc1-140">Wyloguj się z aplikacji sieci Web po zakończeniu korzystania z nich.</span><span class="sxs-lookup"><span data-stu-id="8efc1-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="8efc1-141">Okresowe czyszczenie plików cookie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8efc1-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="8efc1-142">Jednak luki w zabezpieczeniach CSRF są zasadniczo problemem z aplikacją sieci Web, a nie użytkownikiem końcowym.</span><span class="sxs-lookup"><span data-stu-id="8efc1-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="8efc1-143">Podstawowe informacje uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="8efc1-143">Authentication fundamentals</span></span>

<span data-ttu-id="8efc1-144">Uwierzytelnianie na podstawie plików cookie to popularna forma uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8efc1-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="8efc1-145">Systemy uwierzytelniania oparte na tokenach zwiększają popularność, szczególnie w przypadku aplikacji jednostronicowych (aplikacji jednostronicowych).</span><span class="sxs-lookup"><span data-stu-id="8efc1-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="8efc1-146">Uwierzytelnianie na podstawie plików cookie</span><span class="sxs-lookup"><span data-stu-id="8efc1-146">Cookie-based authentication</span></span>

<span data-ttu-id="8efc1-147">Gdy użytkownik jest uwierzytelniany przy użyciu nazwy użytkownika i hasła, zostanie wystawiony token zawierający bilet uwierzytelniania, który może być używany do uwierzytelniania i autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="8efc1-148">Token jest przechowywany jako plik cookie, który jest dołączany do każdego żądania przez klienta.</span><span class="sxs-lookup"><span data-stu-id="8efc1-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="8efc1-149">Generowanie i sprawdzanie poprawności tego pliku cookie jest wykonywane przez oprogramowanie pośredniczące uwierzytelniania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="8efc1-150">[Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) deserializacji podmiotu zabezpieczeń użytkownika do zaszyfrowanego pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="8efc1-151">W kolejnych żądaniach oprogramowanie pośredniczące weryfikuje plik cookie, ponownie tworzy podmiot zabezpieczeń i przypisuje podmiotowi zabezpieczeń do właściwości [użytkownika](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="8efc1-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="8efc1-152">Uwierzytelnianie oparte na tokenach</span><span class="sxs-lookup"><span data-stu-id="8efc1-152">Token-based authentication</span></span>

<span data-ttu-id="8efc1-153">Po uwierzytelnieniu użytkownika są wystawiane tokeny (nie token antysfałszowany).</span><span class="sxs-lookup"><span data-stu-id="8efc1-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="8efc1-154">Token zawiera informacje o użytkowniku w formie [oświadczeń](/dotnet/framework/security/claims-based-identity-model) lub Token odwołania, który wskazuje, że stan użytkownika w aplikacji jest obsługiwany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="8efc1-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="8efc1-155">Gdy użytkownik próbuje uzyskać dostęp do zasobu wymagającego uwierzytelnienia, token jest wysyłany do aplikacji z dodatkowym nagłówkiem autoryzacji w postaci tokenu okaziciela.</span><span class="sxs-lookup"><span data-stu-id="8efc1-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="8efc1-156">Powoduje to bezstanową aplikację.</span><span class="sxs-lookup"><span data-stu-id="8efc1-156">This makes the app stateless.</span></span> <span data-ttu-id="8efc1-157">W każdym kolejnym żądaniu token jest przesyłany do żądania weryfikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="8efc1-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="8efc1-158">Ten token nie jest *szyfrowany*; jest ona *zaszyfrowana*.</span><span class="sxs-lookup"><span data-stu-id="8efc1-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="8efc1-159">Na serwerze token jest zdekodowany w celu uzyskania dostępu do informacji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="8efc1-160">Aby wysłać token w kolejnych żądaniach, Zapisz token w lokalnym magazynie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8efc1-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="8efc1-161">Nie należy obawiać się o luki w zabezpieczeniach CSRF, jeśli token jest przechowywany w lokalnym magazynie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8efc1-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="8efc1-162">CSRF jest problemem, gdy token jest przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-162">CSRF is a concern when the token is stored in a cookie.</span></span> <span data-ttu-id="8efc1-163">Aby uzyskać więcej informacji, zobacz przykładowy kod rozchód spa w usłudze GitHub [dodaje dwa pliki cookie](https://github.com/aspnet/AspNetCore.Docs/issues/13369).</span><span class="sxs-lookup"><span data-stu-id="8efc1-163">For more information, see the GitHub issue [SPA code sample adds two cookies](https://github.com/aspnet/AspNetCore.Docs/issues/13369).</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="8efc1-164">Wiele aplikacji hostowanych w jednej domenie</span><span class="sxs-lookup"><span data-stu-id="8efc1-164">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="8efc1-165">Udostępnione środowiska hostingu są podatne na przejmowanie sesji, CSRF logowania i inne ataki.</span><span class="sxs-lookup"><span data-stu-id="8efc1-165">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="8efc1-166">Mimo że `example1.contoso.net` i `example2.contoso.net` są różnymi hostami, istnieje niejawna relacja zaufania między hostami w ramach domeny `*.contoso.net`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-166">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="8efc1-167">Niejawne relacje zaufania umożliwiają potencjalnie niezaufanym hostom wpływanie na wszystkie inne pliki cookie (zasady tego samego źródła, które zarządza żądaniami AJAX, nie zawsze mają zastosowanie do plików cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="8efc1-167">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="8efc1-168">Ataki wykorzystujące zaufane pliki cookie między aplikacjami hostowanymi w tej samej domenie można zablokować, nie udostępniając domen.</span><span class="sxs-lookup"><span data-stu-id="8efc1-168">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="8efc1-169">Gdy każda aplikacja jest hostowana we własnej domenie, nie istnieje niejawna relacja zaufania plików cookie do wykorzystania.</span><span class="sxs-lookup"><span data-stu-id="8efc1-169">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="8efc1-170">Konfiguracja antysfałszowana ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8efc1-170">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="8efc1-171">ASP.NET Core implementuje ochronę przed fałszowaniem za pomocą [ASP.NET Core ochrony danych](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="8efc1-171">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="8efc1-172">Stos ochrony danych musi być skonfigurowany do pracy w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="8efc1-172">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="8efc1-173">Aby uzyskać więcej informacji, zobacz [Konfigurowanie ochrony danych](xref:security/data-protection/configuration/overview) .</span><span class="sxs-lookup"><span data-stu-id="8efc1-173">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8efc1-174">Oprogramowanie pośredniczące przed fałszowaniem jest dodawane do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) , gdy jeden z następujących interfejsów API jest wywoływany w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8efc1-174">Antiforgery middleware is added to the [Dependency injection](xref:fundamentals/dependency-injection) container when one of the following APIs is called in `Startup.ConfigureServices`:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>
* <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*>
* <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>
* <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8efc1-175">Oprogramowanie pośredniczące przed fałszowaniem jest dodawane do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) , gdy <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> jest wywoływana w `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="8efc1-175">Antiforgery middleware is added to the [Dependency injection](xref:fundamentals/dependency-injection) container when <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> is called in `Startup.ConfigureServices`</span></span>

::: moniker-end

<span data-ttu-id="8efc1-176">W ASP.NET Core 2,0 lub nowszej [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) wprowadza do elementów formularza HTML tokeny zabezpieczające przed fałszerstwem.</span><span class="sxs-lookup"><span data-stu-id="8efc1-176">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="8efc1-177">Następujące znaczniki w pliku Razor automatycznie generują tokeny przed fałszerstwem:</span><span class="sxs-lookup"><span data-stu-id="8efc1-177">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="8efc1-178">Podobnie [IHtmlHelper. BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) domyślnie generuje tokeny zabezpieczające przed fałszerstwem, jeśli nie można pobrać metody formularza.</span><span class="sxs-lookup"><span data-stu-id="8efc1-178">Similarly, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="8efc1-179">Automatyczne generowanie tokenów antysfałszowanych dla elementów formularza HTML występuje, gdy tag `<form>` zawiera atrybut `method="post"` i jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="8efc1-179">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

* <span data-ttu-id="8efc1-180">Atrybut Action jest pusty (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="8efc1-180">The action attribute is empty (`action=""`).</span></span>
* <span data-ttu-id="8efc1-181">Nie podano atrybutu Action (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="8efc1-181">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="8efc1-182">Automatyczne generowanie tokenów antysfałszowanych dla elementów formularza HTML może być wyłączone:</span><span class="sxs-lookup"><span data-stu-id="8efc1-182">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="8efc1-183">Jawnie wyłącz tokeny antysfałszowane z atrybutem `asp-antiforgery`:</span><span class="sxs-lookup"><span data-stu-id="8efc1-183">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="8efc1-184">Element form jest wyłączany z pomocników tagów przy użyciu [symbolu Autowypełniania tagów!](xref:mvc/views/tag-helpers/intro#opt-out)</span><span class="sxs-lookup"><span data-stu-id="8efc1-184">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="8efc1-185">Usuń `FormTagHelper` z widoku.</span><span class="sxs-lookup"><span data-stu-id="8efc1-185">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="8efc1-186">`FormTagHelper` można usunąć z widoku przez dodanie następującej dyrektywy do widoku Razor:</span><span class="sxs-lookup"><span data-stu-id="8efc1-186">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="8efc1-187">[Razor Pages](xref:razor-pages/index) są automatycznie chronione przed XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="8efc1-187">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="8efc1-188">Aby uzyskać więcej informacji, zobacz [XSRF/CSRF i Razor Pages](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="8efc1-188">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="8efc1-189">Najbardziej typowym podejściem do obrony przed atakami CSRF jest użycie *wzorca tokenu synchronizatora* (STP).</span><span class="sxs-lookup"><span data-stu-id="8efc1-189">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="8efc1-190">Wartość STP jest używana, gdy użytkownik żąda strony z danymi formularza:</span><span class="sxs-lookup"><span data-stu-id="8efc1-190">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="8efc1-191">Serwer wysyła do klienta token skojarzony z tożsamością bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8efc1-191">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="8efc1-192">Klient wysyła do serwera token z powrotem w celu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-192">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="8efc1-193">Jeśli serwer odbiera token, który nie jest zgodny z tożsamością uwierzytelnionego użytkownika, żądanie zostanie odrzucone.</span><span class="sxs-lookup"><span data-stu-id="8efc1-193">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="8efc1-194">Token jest unikatowy i nieprzewidywalny.</span><span class="sxs-lookup"><span data-stu-id="8efc1-194">The token is unique and unpredictable.</span></span> <span data-ttu-id="8efc1-195">Token może również służyć do zapewnienia prawidłowej sekwencji serii żądań (na przykład w celu zapewnienia sekwencji żądań: Strona 1 &ndash; stronie 2 &ndash; stronie 3).</span><span class="sxs-lookup"><span data-stu-id="8efc1-195">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="8efc1-196">Wszystkie formularze w szablonach ASP.NET Core MVC i Razor Pages generują tokeny zabezpieczające przed fałszerstwem.</span><span class="sxs-lookup"><span data-stu-id="8efc1-196">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="8efc1-197">Poniższa para przykładów widoku generuje tokeny zabezpieczające przed fałszerstwem:</span><span class="sxs-lookup"><span data-stu-id="8efc1-197">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="8efc1-198">Jawnie Dodaj token antysfałszowany do elementu `<form>` bez korzystania z pomocników tagów za pomocą usługi HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="8efc1-198">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="8efc1-199">W każdym z powyższych przypadków ASP.NET Core dodaje ukryte pole formularza podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="8efc1-199">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="8efc1-200">ASP.NET Core zawiera trzy [filtry](xref:mvc/controllers/filters) do pracy z tokenami antyfałszowanymi:</span><span class="sxs-lookup"><span data-stu-id="8efc1-200">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="8efc1-201">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="8efc1-201">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="8efc1-202">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="8efc1-202">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="8efc1-203">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="8efc1-203">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="8efc1-204">Opcje samopodrabiania</span><span class="sxs-lookup"><span data-stu-id="8efc1-204">Antiforgery options</span></span>

<span data-ttu-id="8efc1-205">Dostosuj [Opcje antysfałszowane](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8efc1-205">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

<span data-ttu-id="8efc1-206">&dagger;ustawić właściwości `Cookie` przed fałszerstwem przy użyciu właściwości klasy [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) .</span><span class="sxs-lookup"><span data-stu-id="8efc1-206">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="8efc1-207">Opcja</span><span class="sxs-lookup"><span data-stu-id="8efc1-207">Option</span></span> | <span data-ttu-id="8efc1-208">Opis</span><span class="sxs-lookup"><span data-stu-id="8efc1-208">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="8efc1-209">Plików</span><span class="sxs-lookup"><span data-stu-id="8efc1-209">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="8efc1-210">Określa ustawienia używane do tworzenia plików cookie z fałszerstwem.</span><span class="sxs-lookup"><span data-stu-id="8efc1-210">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="8efc1-211">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="8efc1-211">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="8efc1-212">Nazwa ukrytego pola formularza używanego przez system antysfałszowany do renderowania tokenów antysfałszowanych w widokach.</span><span class="sxs-lookup"><span data-stu-id="8efc1-212">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="8efc1-213">Nagłówek nagłówka</span><span class="sxs-lookup"><span data-stu-id="8efc1-213">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="8efc1-214">Nazwa nagłówka używanego przez system antysfałszowany.</span><span class="sxs-lookup"><span data-stu-id="8efc1-214">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="8efc1-215">Jeśli `null`, system uwzględnia tylko dane formularza.</span><span class="sxs-lookup"><span data-stu-id="8efc1-215">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="8efc1-216">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="8efc1-216">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="8efc1-217">Określa, czy należy pominąć generowanie nagłówka `X-Frame-Options`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-217">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="8efc1-218">Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="8efc1-218">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="8efc1-219">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-219">Defaults to `false`.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="8efc1-220">Opcja</span><span class="sxs-lookup"><span data-stu-id="8efc1-220">Option</span></span> | <span data-ttu-id="8efc1-221">Opis</span><span class="sxs-lookup"><span data-stu-id="8efc1-221">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="8efc1-222">Plików</span><span class="sxs-lookup"><span data-stu-id="8efc1-222">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="8efc1-223">Określa ustawienia używane do tworzenia plików cookie z fałszerstwem.</span><span class="sxs-lookup"><span data-stu-id="8efc1-223">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="8efc1-224">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="8efc1-224">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="8efc1-225">Domena pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-225">The domain of the cookie.</span></span> <span data-ttu-id="8efc1-226">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-226">Defaults to `null`.</span></span> <span data-ttu-id="8efc1-227">Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-227">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="8efc1-228">Zalecaną alternatywą jest plik cookie. domena.</span><span class="sxs-lookup"><span data-stu-id="8efc1-228">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="8efc1-229">CookieName</span><span class="sxs-lookup"><span data-stu-id="8efc1-229">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="8efc1-230">Nazwa pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-230">The name of the cookie.</span></span> <span data-ttu-id="8efc1-231">Jeśli nie zostanie ustawiona, system generuje unikatową nazwę rozpoczynającą się od [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore. przed fałszerstwem ").</span><span class="sxs-lookup"><span data-stu-id="8efc1-231">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="8efc1-232">Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-232">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="8efc1-233">Zalecaną alternatywą jest Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="8efc1-233">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="8efc1-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="8efc1-234">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="8efc1-235">Ścieżka ustawiona w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-235">The path set on the cookie.</span></span> <span data-ttu-id="8efc1-236">Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-236">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="8efc1-237">Zalecaną alternatywą jest plik cookie. Path.</span><span class="sxs-lookup"><span data-stu-id="8efc1-237">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="8efc1-238">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="8efc1-238">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="8efc1-239">Nazwa ukrytego pola formularza używanego przez system antysfałszowany do renderowania tokenów antysfałszowanych w widokach.</span><span class="sxs-lookup"><span data-stu-id="8efc1-239">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="8efc1-240">Nagłówek nagłówka</span><span class="sxs-lookup"><span data-stu-id="8efc1-240">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="8efc1-241">Nazwa nagłówka używanego przez system antysfałszowany.</span><span class="sxs-lookup"><span data-stu-id="8efc1-241">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="8efc1-242">Jeśli `null`, system uwzględnia tylko dane formularza.</span><span class="sxs-lookup"><span data-stu-id="8efc1-242">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="8efc1-243">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="8efc1-243">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="8efc1-244">Określa, czy protokół HTTPS jest wymagany przez system antysfałszowany.</span><span class="sxs-lookup"><span data-stu-id="8efc1-244">Specifies whether HTTPS is required by the antiforgery system.</span></span> <span data-ttu-id="8efc1-245">Jeśli `true`, żądania inne niż HTTPS kończą się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="8efc1-245">If `true`, non-HTTPS requests fail.</span></span> <span data-ttu-id="8efc1-246">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-246">Defaults to `false`.</span></span> <span data-ttu-id="8efc1-247">Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-247">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="8efc1-248">Zalecaną alternatywą jest ustawienie pliku cookie. SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="8efc1-248">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="8efc1-249">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="8efc1-249">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="8efc1-250">Określa, czy należy pominąć generowanie nagłówka `X-Frame-Options`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-250">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="8efc1-251">Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="8efc1-251">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="8efc1-252">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-252">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="8efc1-253">Aby uzyskać więcej informacji, zobacz [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="8efc1-253">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="8efc1-254">Konfigurowanie funkcji antysfałszowanych za pomocą IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="8efc1-254">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="8efc1-255">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) udostępnia interfejs API do konfigurowania funkcji antysfałszowanych.</span><span class="sxs-lookup"><span data-stu-id="8efc1-255">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="8efc1-256">`IAntiforgery` można żądać w metodzie `Configure` klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-256">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="8efc1-257">W poniższym przykładzie użyto oprogramowania pośredniczącego ze strony głównej aplikacji do wygenerowania tokenu antysfałszowanego i wysłania go w odpowiedzi jako plik cookie (przy użyciu domyślnej konwencji nazewnictwa kątowego opisanej w dalszej części tego tematu):</span><span class="sxs-lookup"><span data-stu-id="8efc1-257">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="8efc1-258">Wymagaj weryfikacji przed fałszerstwem</span><span class="sxs-lookup"><span data-stu-id="8efc1-258">Require antiforgery validation</span></span>

<span data-ttu-id="8efc1-259">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) to filtr akcji, który można zastosować do poszczególnych akcji, kontrolera lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-259">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="8efc1-260">Żądania wykonane do akcji, do których zastosowano ten filtr są blokowane, chyba że żądanie zawiera prawidłowy token antysfałszowany.</span><span class="sxs-lookup"><span data-stu-id="8efc1-260">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="8efc1-261">Atrybut `ValidateAntiForgeryToken` wymaga tokenu dla żądań do metod akcji, które zdobi, łącznie z żądaniami HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="8efc1-261">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="8efc1-262">Jeśli atrybut `ValidateAntiForgeryToken` jest stosowany w kontrolerach aplikacji, można go zastąpić atrybutem `IgnoreAntiforgeryToken`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-262">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="8efc1-263">ASP.NET Core nie obsługuje dodawania tokenów antysfałszowanych w celu automatycznego uzyskiwania żądań.</span><span class="sxs-lookup"><span data-stu-id="8efc1-263">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="8efc1-264">Automatycznie Weryfikuj tokeny antysfałszowane wyłącznie dla niebezpiecznych metod HTTP</span><span class="sxs-lookup"><span data-stu-id="8efc1-264">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="8efc1-265">Aplikacje ASP.NET Core nie generują tokenów antysfałszowanych dla bezpiecznych metod HTTP (GET, głowy, OPTIONS i TRACE).</span><span class="sxs-lookup"><span data-stu-id="8efc1-265">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="8efc1-266">Zamiast szeroko stosowanego atrybutu `ValidateAntiForgeryToken`, a następnie przesłania go przy użyciu atrybutów `IgnoreAntiforgeryToken`, można użyć atrybutu [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) .</span><span class="sxs-lookup"><span data-stu-id="8efc1-266">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="8efc1-267">Ten atrybut działa identycznie z atrybutem `ValidateAntiForgeryToken`, z tą różnicą, że nie wymagają tokenów dla żądań wysyłanych przy użyciu następujących metod HTTP:</span><span class="sxs-lookup"><span data-stu-id="8efc1-267">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="8efc1-268">GET</span><span class="sxs-lookup"><span data-stu-id="8efc1-268">GET</span></span>
* <span data-ttu-id="8efc1-269">HEAD</span><span class="sxs-lookup"><span data-stu-id="8efc1-269">HEAD</span></span>
* <span data-ttu-id="8efc1-270">OPCJE</span><span class="sxs-lookup"><span data-stu-id="8efc1-270">OPTIONS</span></span>
* <span data-ttu-id="8efc1-271">TRACE</span><span class="sxs-lookup"><span data-stu-id="8efc1-271">TRACE</span></span>

<span data-ttu-id="8efc1-272">Zalecamy korzystanie z `AutoValidateAntiforgeryToken` w szerokim zakresie dla scenariuszy innych niż interfejsy API.</span><span class="sxs-lookup"><span data-stu-id="8efc1-272">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="8efc1-273">Gwarantuje to, że akcje wykonywane domyślnie są chronione.</span><span class="sxs-lookup"><span data-stu-id="8efc1-273">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="8efc1-274">Alternatywą jest ignorowanie tokenów antysfałszowanych domyślnie, chyba że `ValidateAntiForgeryToken` jest stosowana do poszczególnych metod akcji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-274">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="8efc1-275">Bardziej prawdopodobnie w tym scenariuszu, aby metoda po akcji nie była chroniona przez pomyłkę, pozostawiając aplikację narażoną na ataki CSRF.</span><span class="sxs-lookup"><span data-stu-id="8efc1-275">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="8efc1-276">Wszystkie wpisy powinny wysyłać token antysfałszowany.</span><span class="sxs-lookup"><span data-stu-id="8efc1-276">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="8efc1-277">Interfejsy API nie mają mechanizmu automatycznego do wysyłania niezwiązanego z plikiem cookie części tokenu.</span><span class="sxs-lookup"><span data-stu-id="8efc1-277">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="8efc1-278">Implementacja prawdopodobnie zależy od implementacji kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="8efc1-278">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="8efc1-279">Poniżej przedstawiono kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="8efc1-279">Some examples are shown below:</span></span>

<span data-ttu-id="8efc1-280">Przykład na poziomie klasy:</span><span class="sxs-lookup"><span data-stu-id="8efc1-280">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="8efc1-281">Przykład globalny:</span><span class="sxs-lookup"><span data-stu-id="8efc1-281">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="8efc1-282">Zastąp atrybuty globalnym lub kontrolerem antyfałszowanym</span><span class="sxs-lookup"><span data-stu-id="8efc1-282">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="8efc1-283">Filtr [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) służy do eliminacji potrzeb tokenu antysfałszowanego dla danej akcji (lub kontrolera).</span><span class="sxs-lookup"><span data-stu-id="8efc1-283">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="8efc1-284">W przypadku zastosowania ten filtr zastępuje filtry `ValidateAntiForgeryToken` i `AutoValidateAntiforgeryToken` określone na wyższym poziomie (globalnie lub na kontrolerze).</span><span class="sxs-lookup"><span data-stu-id="8efc1-284">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="8efc1-285">Odśwież tokeny po uwierzytelnieniu</span><span class="sxs-lookup"><span data-stu-id="8efc1-285">Refresh tokens after authentication</span></span>

<span data-ttu-id="8efc1-286">Tokeny należy odświeżyć po uwierzytelnieniu użytkownika przez przekierowanie użytkownika do widoku lub Razor Pages strony.</span><span class="sxs-lookup"><span data-stu-id="8efc1-286">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="8efc1-287">JavaScript, AJAX i aplikacji jednostronicowych</span><span class="sxs-lookup"><span data-stu-id="8efc1-287">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="8efc1-288">W tradycyjnych aplikacjach opartych na języku HTML tokeny zabezpieczające są przesyłane do serwera przy użyciu ukrytych pól formularzy.</span><span class="sxs-lookup"><span data-stu-id="8efc1-288">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="8efc1-289">W nowoczesnych aplikacjach opartych na języku JavaScript i aplikacji jednostronicowych wiele żądań jest wykonywanych programowo.</span><span class="sxs-lookup"><span data-stu-id="8efc1-289">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="8efc1-290">Te żądania AJAX mogą używać innych technik (takich jak nagłówki żądań lub pliki cookie) do wysyłania tokenu.</span><span class="sxs-lookup"><span data-stu-id="8efc1-290">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="8efc1-291">Jeśli pliki cookie są używane do przechowywania tokenów uwierzytelniania i uwierzytelniania żądań interfejsu API na serwerze, CSRF jest potencjalnym problemem.</span><span class="sxs-lookup"><span data-stu-id="8efc1-291">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="8efc1-292">Jeśli magazyn lokalny jest używany do przechowywania tokenu, luka w zabezpieczeniach CSRF może zostać wyeliminowana, ponieważ wartości z magazynu lokalnego nie są automatycznie wysyłane do serwera przy użyciu każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="8efc1-292">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="8efc1-293">W tym celu należy użyć magazynu lokalnego do przechowywania na kliencie tokenu antysfałszowanego i wysyłania tokenu jako nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="8efc1-293">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="8efc1-294">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8efc1-294">JavaScript</span></span>

<span data-ttu-id="8efc1-295">Przy użyciu języka JavaScript z widokami token można utworzyć przy użyciu usługi z poziomu widoku.</span><span class="sxs-lookup"><span data-stu-id="8efc1-295">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="8efc1-296">Wsuń usługę [Microsoft. AspNetCore. antyfałszerstwe. IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) do widoku i Wywołaj [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="8efc1-296">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="8efc1-297">Takie podejście eliminuje konieczność bezpośredniej obsługi ustawień plików cookie z serwera lub odczytywanie ich z klienta programu.</span><span class="sxs-lookup"><span data-stu-id="8efc1-297">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="8efc1-298">Poprzedni przykład używa języka JavaScript do odczytywania wartości pola ukrytego dla nagłówka z WPISem AJAX.</span><span class="sxs-lookup"><span data-stu-id="8efc1-298">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="8efc1-299">Język JavaScript może również uzyskiwać dostęp do tokenów w plikach cookie i używać zawartości pliku cookie do tworzenia nagłówka z wartością tokenu.</span><span class="sxs-lookup"><span data-stu-id="8efc1-299">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="8efc1-300">Zakładając, że żądanie skryptu wysłano token w nagłówku o nazwie `X-CSRF-TOKEN`, skonfiguruj usługę antysfałszowaną, aby wyszukać nagłówek `X-CSRF-TOKEN`:</span><span class="sxs-lookup"><span data-stu-id="8efc1-300">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="8efc1-301">Poniższy przykład używa języka JavaScript, aby wykonać żądanie AJAX z odpowiednim nagłówkiem:</span><span class="sxs-lookup"><span data-stu-id="8efc1-301">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="8efc1-302">AngularJS</span><span class="sxs-lookup"><span data-stu-id="8efc1-302">AngularJS</span></span>

<span data-ttu-id="8efc1-303">AngularJS używa konwencji do adresowania CSRF.</span><span class="sxs-lookup"><span data-stu-id="8efc1-303">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="8efc1-304">Jeśli serwer wysyła plik cookie o nazwie `XSRF-TOKEN`, usługa AngularJS `$http` dodaje wartość cookie do nagłówka podczas wysyłania żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="8efc1-304">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="8efc1-305">Ten proces jest automatyczny.</span><span class="sxs-lookup"><span data-stu-id="8efc1-305">This process is automatic.</span></span> <span data-ttu-id="8efc1-306">Nagłówek nie musi być jawnie ustawiony na kliencie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-306">The header doesn't need to be set in the client explicitly.</span></span> <span data-ttu-id="8efc1-307">Nazwa nagłówka jest `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-307">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="8efc1-308">Serwer powinien wykryć ten nagłówek i zweryfikować jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="8efc1-308">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="8efc1-309">Aby program ASP.NET Core API działał z tą konwencją podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8efc1-309">For ASP.NET Core API to work with this convention in your application startup:</span></span>

* <span data-ttu-id="8efc1-310">Skonfiguruj aplikację, aby zapewnić token w pliku cookie o nazwie `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-310">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="8efc1-311">Skonfiguruj usługę antysfałszowaną, aby wyszukać nagłówek o nazwie `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="8efc1-311">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

<span data-ttu-id="8efc1-312">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8efc1-312">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="8efc1-313">Zwiększ fałszerstwo</span><span class="sxs-lookup"><span data-stu-id="8efc1-313">Extend antiforgery</span></span>

<span data-ttu-id="8efc1-314">Typ [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) umożliwia deweloperom zwiększenie zachowania systemu antyCSRFowego przez dwukrotne wyzwolenie dodatkowych danych w każdym tokenie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-314">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="8efc1-315">Metoda [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) jest wywoływana za każdym razem, gdy generowany jest token pola, a zwracana wartość jest osadzona w wygenerowanym tokenie.</span><span class="sxs-lookup"><span data-stu-id="8efc1-315">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="8efc1-316">Realizator może zwrócić sygnaturę czasową, identyfikator jednorazowy lub inną wartość, a następnie wywołać [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) , aby zweryfikować te dane po sprawdzeniu poprawności tokenu.</span><span class="sxs-lookup"><span data-stu-id="8efc1-316">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="8efc1-317">Nazwa użytkownika klienta jest już osadzona w wygenerowanych tokenach, więc nie trzeba uwzględniać tych informacji.</span><span class="sxs-lookup"><span data-stu-id="8efc1-317">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="8efc1-318">Jeśli token zawiera dane uzupełniające, ale nie skonfigurowano `IAntiForgeryAdditionalDataProvider`, dane uzupełniające nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="8efc1-318">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8efc1-319">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8efc1-319">Additional resources</span></span>

* <span data-ttu-id="8efc1-320">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) w [programie Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="8efc1-320">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
