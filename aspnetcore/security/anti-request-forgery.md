---
title: Zapobiegaj Cross-Site żądania (XSRF/CSRF) fałszerstwie w ASP.NET Core
author: steve-smith
description: Wykryj jak nie dopuścić do ataków na aplikacje sieci web, w którym złośliwą witrynę sieci Web może mieć wpływ interakcji między przeglądarką klienta i aplikacji.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 3bca96f4a2e247eeeb93140df93221371d88d4d3
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341863"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="aaa3f-103">Zapobiegaj Cross-Site żądania (XSRF/CSRF) fałszerstwie w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aaa3f-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="aaa3f-104">Przez [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aaa3f-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aaa3f-105">Fałszowanie żądań między witrynami (znanej także jako XSRF lub CSRF, Wymowa *Małgiew zobacz*) jest atak wykorzystujący aplikacji hostowanych w sieci web, zgodnie z którymi aplikacja sieci web złośliwego mogą mieć wpływ interakcji między przeglądarką klienta i aplikacji sieci web, które ufają, który Przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="aaa3f-106">Tego rodzaju ataki są możliwe, ponieważ przeglądarki sieci web wysyłać niektóre typy tokenów uwierzytelniania automatycznie z każdego żądania do witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="aaa3f-107">Ten formularz wykorzystać jest także znana jako *ataku jednym kliknięciem* lub *sesji jazda* ponieważ ataku korzysta z użytkownik wcześniej uwierzytelniona sesji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="aaa3f-108">Przykład atak CSRF:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="aaa3f-109">Użytkownik zaloguje się do `www.good-banking-site.com` uwierzytelnianie za pomocą formularzy.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="aaa3f-110">Serwer uwierzytelnia użytkownika i generuje odpowiedzi, który zawiera plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="aaa3f-111">Witryna jest narażony na ataki, ponieważ subskrypcja ufa każde żądanie otrzymanych z prawidłowy plik cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="aaa3f-112">Użytkownik odwiedzi złośliwą witrynę, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="aaa3f-113">Złośliwa witryna `www.bad-crook-site.com`, zawiera formularza HTML podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="aaa3f-114">Zwróć uwagę, że formularza `action` wpisów do lokacji narażony, a nie do niebezpiecznej witryny.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="aaa3f-115">Jest to część CSRF "cross-site".</span><span class="sxs-lookup"><span data-stu-id="aaa3f-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="aaa3f-116">Gdy użytkownik wybierze przycisk przesyłania.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-116">The user selects the submit button.</span></span> <span data-ttu-id="aaa3f-117">Przeglądarka wysyła żądanie i automatycznie uwzględnia pliku cookie uwierzytelniania dla żądanej domeny `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="aaa3f-118">Żądanie jest uruchamiany na `www.good-banking-site.com` serwera z kontekstem uwierzytelniania użytkownika i mogą wykonywać żadnych czynności, że uwierzytelniony użytkownik może wykonywać.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="aaa3f-119">Oprócz scenariusz, w którym użytkownik wybierze przycisk, aby przesłać formularza złośliwa witryna można:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="aaa3f-120">Uruchom skrypt, który automatycznie wyśle formularz.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="aaa3f-121">Wyślij przesyłania formularza jako żądaniem AJAX.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="aaa3f-122">Ukryj do formularza używającego CSS.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-122">Hide the form using CSS.</span></span>

<span data-ttu-id="aaa3f-123">Te scenariusze alternatywnych nie wymagają żadnych akcji lub danych wejściowych od użytkownika niż początkowo odwiedzający niebezpiecznej witryny.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="aaa3f-124">Przy użyciu protokołu HTTPS nie uniemożliwia atak CSRF.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="aaa3f-125">Złośliwa witryna można wysyłać `https://www.good-banking-site.com/` równie proste, jak może wysłać żądanie niezabezpieczonego żądania.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="aaa3f-126">Niektóre ataki docelowych punktów końcowych, które odpowiadają na żądania GET w takim przypadku tag obrazu mogą posłużyć do wykonania akcji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="aaa3f-127">Ta forma ataku jest typowe w lokacjach forum, zezwolenie na obrazy, które blokowania języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="aaa3f-128">Aplikacje, które spowodują zmianę stanu dla żądań GET wysyłanych, gdy modyfikacji zmiennych lub zasobów, są podatne na złośliwe ataki.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="aaa3f-129">**Żądania GET, które spowodują zmianę stanu jest niebezpieczne. Najlepszym rozwiązaniem jest nigdy nie ulegną zmianie stanu na żądanie GET.**</span><span class="sxs-lookup"><span data-stu-id="aaa3f-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="aaa3f-130">Ataki CSRF są możliwe w dla aplikacji sieci web, które używają plików cookie do uwierzytelniania, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="aaa3f-131">Przeglądarki przechowywania plików cookie wystawiony przez aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="aaa3f-132">Przechowywane pliki cookie obejmują pliki cookie sesji dla uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="aaa3f-133">Przeglądarki Wyślij wszystkie pliki cookie skojarzone z domeną w aplikacji sieci web każde żądanie niezależnie od tego, jak żądania do aplikacji został wygenerowany w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="aaa3f-134">Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="aaa3f-135">Na przykład uwierzytelnianie podstawowe i szyfrowane również są zagrożone.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="aaa3f-136">Po zalogowaniu się za pomocą uwierzytelnianie podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia do sesji&dagger; kończy się.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="aaa3f-137">&dagger;W tym kontekście *sesji* odwołuje się do sesji po stronie klienta, w którym użytkownik jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="aaa3f-138">Jest niezwiązanych ze sobą do sesji po stronie serwera lub [platformy ASP.NET Core sesji w oprogramowaniu pośredniczącym](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="aaa3f-139">Użytkownicy mogą chronią przed luk w zabezpieczeniach CSRF wykonując środki ostrożności:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="aaa3f-140">Zaloguj się wylogowuje aplikacje sieci web po zakończeniu korzystania z nich.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="aaa3f-141">Wyczyść pliki cookie przeglądarki okresowo.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="aaa3f-142">Jednak luk w zabezpieczeniach CSRF są zasadniczo problem z aplikacji sieci web, a nie użytkownika końcowego.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="aaa3f-143">Podstawowe informacje dotyczące uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="aaa3f-143">Authentication fundamentals</span></span>

<span data-ttu-id="aaa3f-144">Plik cookie uwierzytelniania jest popularnych formy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="aaa3f-145">Systemy uwierzytelniania opartego na tokenie rośnie w popularne, szczególnie w przypadku aplikacje jednostronicowe (źródła).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="aaa3f-146">Uwierzytelnianie na podstawie plików cookie</span><span class="sxs-lookup"><span data-stu-id="aaa3f-146">Cookie-based authentication</span></span>

<span data-ttu-id="aaa3f-147">Podczas uwierzytelniania przy użyciu nazwy użytkownika i hasła użytkownika, są one wystawiony token, zawierający bilet uwierzytelniania, który może służyć do uwierzytelniania i autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="aaa3f-148">Token jest przechowywany jako sprawia, że plik cookie dołączona każde żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="aaa3f-149">Generowanie i sprawdzanie poprawności ten plik cookie jest wykonywane przez oprogramowanie pośredniczące uwierzytelniania plików Cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="aaa3f-150">[Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) serializuje głównej nazwy użytkownika do zaszyfrowanego pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="aaa3f-151">Kolejne żądania oprogramowania pośredniczącego sprawdza poprawność pliku cookie, odtwarza podmiot zabezpieczeń i przypisuje do podmiotu zabezpieczeń [użytkownika](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) właściwość [element HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="aaa3f-152">Uwierzytelnianie na podstawie tokenu</span><span class="sxs-lookup"><span data-stu-id="aaa3f-152">Token-based authentication</span></span>

<span data-ttu-id="aaa3f-153">Podczas uwierzytelniania użytkownika, są one wystawiony token (nie antiforgery token).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="aaa3f-154">Token zawiera informacje o użytkowniku w formie [oświadczeń](/dotnet/framework/security/claims-based-identity-model) lub tokenu odwołania, który wskazuje aplikacji stanu użytkownika, obsługiwane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="aaa3f-155">Gdy użytkownik próbuje uzyskać dostęp do zasobów wymagających uwierzytelniania, token jest wysyłany do aplikacji z nagłówkiem dodatkowe autoryzacji w formie tokenów elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="aaa3f-156">Dzięki temu aplikacji bezstanowych.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-156">This makes the app stateless.</span></span> <span data-ttu-id="aaa3f-157">W kolejnych żądań token jest przekazywany w żądaniu weryfikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="aaa3f-158">Ten token nie jest *zaszyfrowanych*; ma *zakodowane*.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="aaa3f-159">Na serwerze token jest dekodowany uzyskiwać dostęp do swoich informacji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="aaa3f-160">Aby wysłać token dla kolejnych żądań, należy przechowywać token w magazynie lokalnym w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="aaa3f-161">Nie można zajmującym się luka w zabezpieczeniach CSRF, jeśli token jest przechowywany w magazynie lokalnym w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="aaa3f-162">CSRF jest istotny, gdy token jest przechowywana w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="aaa3f-163">Wiele aplikacji hostowanych w jednej domenie</span><span class="sxs-lookup"><span data-stu-id="aaa3f-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="aaa3f-164">Udostępnionych środowiskach macierzystych są podatne na przejęcie kontroli sesji logowania CSRF i inne ataki.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="aaa3f-165">Mimo że `example1.contoso.net` i `example2.contoso.net` są różnych hostach, ma zależności nawiązywanie niejawnych relacji zaufania między hostami w obszarze `*.contoso.net` domeny.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="aaa3f-166">Ta relacja zaufania niejawne umożliwia potencjalnie niezaufane hosty wpływa na siebie nawzajem pliki cookie (zasad tego samego źródła, które będą zarządzały sposobem żądania AJAX nie zawsze dotyczą plików cookie protokołu HTTP).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="aaa3f-167">Ataki wykorzystujące zaufanych plików cookie między aplikacji hostowanych w tej samej domenie można zapobiec przez nie udostępnianie domen.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="aaa3f-168">Każda aplikacja znajduje się na jego własnej domeny, nie ma żadnych relacji zaufania niejawnych plików cookie do wykorzystania.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="aaa3f-169">Konfiguracja antiforgery platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aaa3f-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="aaa3f-170">Implementuje platformy ASP.NET Core za pomocą antiforgery [ochrony danych platformy ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="aaa3f-171">Stos ochrony danych musi być skonfigurowana do pracy w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="aaa3f-172">Zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="aaa3f-173">W programie ASP.NET Core 2.0 lub nowszej [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokenów do elementów formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="aaa3f-174">Następujący kod w pliku Razor automatycznie generuje tokeny antiforgery:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="aaa3f-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generuje tokeny antiforgery domyślnie, jeśli nie będzie formularza metody GET.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="aaa3f-176">Automatycznego generowania tokenów antiforgery elementów formularza HTML się stanie po `<form>` tag zawiera `method="post"` atrybut i jednej z następujących są spełnione:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="aaa3f-177">Atrybut akcji jest pusty (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="aaa3f-178">Nie jest podana w atrybucie akcji (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="aaa3f-179">Można wyłączyć automatyczne generowanie tokenów antiforgery elementów formularza HTML:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="aaa3f-180">Jawnie Wyłącz antiforgery tokeny z `asp-antiforgery` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="aaa3f-181">Form element jest wybrana opcja poza pomocników tagów przy użyciu Pomocnika tagów [! symbol Wypisz](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="aaa3f-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="aaa3f-182">Usuń `FormTagHelper` z widoku.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="aaa3f-183">`FormTagHelper` Można usunąć, dodając następujące dyrektywy do widoku Razor z widoku:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="aaa3f-184">[Stron razor](xref:mvc/razor-pages/index) są automatycznie chronione przed XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-184">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="aaa3f-185">Aby uzyskać więcej informacji, zobacz [XSRF/CSRF i stron Razor](xref:mvc/razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-185">For more information, see [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf).</span></span>

<span data-ttu-id="aaa3f-186">Najbardziej typowym podejściem do obrony przed atakami CSRF jest użycie *wzorzec tokenu Synchronizatora* (STP).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="aaa3f-187">STP jest używany, gdy użytkownik zażąda strony z danymi formularza:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="aaa3f-188">Serwer wysyła token skojarzone z tożsamością bieżącego użytkownika do klienta.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="aaa3f-189">Klient wysyła ponownie token do serwera w celu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="aaa3f-190">Jeśli serwer odbiera token, który nie odpowiada tożsamość uwierzytelnionego użytkownika, żądanie zostanie odrzucone.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="aaa3f-191">Token jest unikatowy i nieprzewidywalny.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="aaa3f-192">Token mogą służyć do zapewnienia prawidłowego sekwencjonowania szereg żądań (na przykład, zapewniając sekwencji żądanie: strona 1 &ndash; strony 2 &ndash; strony 3).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="aaa3f-193">Wszystkie formularze w szablonach platformy ASP.NET Core MVC i stron Razor generowania antiforgery tokenów.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="aaa3f-194">Para następujące przykłady widoku generowania tokenów antiforgery:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="aaa3f-195">Jawnie dodać antiforgery token do `<form>` elementu bez użycia pomocników tagów z Pomocnika kodu HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="aaa3f-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="aaa3f-196">W każdym z powyższych przypadków platformy ASP.NET Core dodaje ukryte pole formularza podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="aaa3f-197">Platformy ASP.NET Core obejmuje trzy [filtry](xref:mvc/controllers/filters) do pracy z tokenami antiforgery:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="aaa3f-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="aaa3f-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="aaa3f-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="aaa3f-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="aaa3f-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="aaa3f-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="aaa3f-201">Opcje antiforgery</span><span class="sxs-lookup"><span data-stu-id="aaa3f-201">Antiforgery options</span></span>

<span data-ttu-id="aaa3f-202">Dostosowywanie [antiforgery opcje](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

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

| <span data-ttu-id="aaa3f-203">Opcja</span><span class="sxs-lookup"><span data-stu-id="aaa3f-203">Option</span></span> | <span data-ttu-id="aaa3f-204">Opis</span><span class="sxs-lookup"><span data-stu-id="aaa3f-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="aaa3f-205">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="aaa3f-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="aaa3f-206">Określa ustawienia używane do tworzenia antiforgery plików cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="aaa3f-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="aaa3f-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="aaa3f-208">Domena pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-208">The domain of the cookie.</span></span> <span data-ttu-id="aaa3f-209">Domyślnie `null`.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-209">Defaults to `null`.</span></span> <span data-ttu-id="aaa3f-210">Ta właściwość jest przestarzała i zostanie usunięta w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="aaa3f-211">Zalecaną alternatywą jest Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="aaa3f-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="aaa3f-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="aaa3f-213">Nazwa pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-213">The name of the cookie.</span></span> <span data-ttu-id="aaa3f-214">Jeśli nie jest ustawiona, system generuje unikatowa nazwa zaczyna się od [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="aaa3f-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="aaa3f-215">Ta właściwość jest przestarzała i zostanie usunięta w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="aaa3f-216">Zalecaną alternatywą jest Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="aaa3f-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="aaa3f-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="aaa3f-218">Ścieżka ustawiona w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-218">The path set on the cookie.</span></span> <span data-ttu-id="aaa3f-219">Ta właściwość jest przestarzała i zostanie usunięta w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="aaa3f-220">Zalecaną alternatywą jest Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="aaa3f-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="aaa3f-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="aaa3f-222">Nazwa ukryte pole formularza używany przez antiforgery system do renderowania antiforgery tokenów w widokach.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="aaa3f-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="aaa3f-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="aaa3f-224">Nazwa nagłówka używany przez antiforgery system.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="aaa3f-225">Jeśli `null`, system uważa tylko dane formularza.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="aaa3f-226">parametru requireSsl</span><span class="sxs-lookup"><span data-stu-id="aaa3f-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="aaa3f-227">Określa, czy protokół SSL jest wymagany przez antiforgery system.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="aaa3f-228">Jeśli `true`, Niepowodzenie żądania bez użycia protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="aaa3f-229">Domyślnie `false`.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-229">Defaults to `false`.</span></span> <span data-ttu-id="aaa3f-230">Ta właściwość jest przestarzała i zostanie usunięta w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="aaa3f-231">Zalecaną alternatywą jest Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="aaa3f-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="aaa3f-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="aaa3f-233">Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="aaa3f-234">Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="aaa3f-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="aaa3f-235">Domyślnie `false`.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-235">Defaults to `false`.</span></span> |

<span data-ttu-id="aaa3f-236">Aby uzyskać więcej informacji, zobacz [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="aaa3f-237">Konfigurowanie funkcji antiforgery z IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="aaa3f-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="aaa3f-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) udostępnia interfejs API umożliwia konfigurowanie funkcji antiforgery.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="aaa3f-239">`IAntiforgery` może być wymagane w `Configure` metody `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="aaa3f-240">W poniższym przykładzie użyto oprogramowanie pośredniczące ze strony głównej aplikacji do wygenerowania tokenu antiforgery i wysłać go w odpowiedzi jako plik cookie (przy użyciu domyślnego kątowego konwencji nazewnictwa opisane w dalszej części tego tematu):</span><span class="sxs-lookup"><span data-stu-id="aaa3f-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

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

### <a name="require-antiforgery-validation"></a><span data-ttu-id="aaa3f-241">Wymagaj antiforgery weryfikacji</span><span class="sxs-lookup"><span data-stu-id="aaa3f-241">Require antiforgery validation</span></span>

<span data-ttu-id="aaa3f-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) jest filtr akcji, który można zastosować do poszczególnych akcji kontrolera, lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="aaa3f-243">Żądania skierowane do akcji, które mają ten filtr są zablokowane, chyba że żądanie zawiera prawidłowy token antiforgery.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="aaa3f-244">`ValidateAntiForgeryToken` Atrybut wymaga tokenu dla żądań kierowanych do metody akcji decorates, łącznie z żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="aaa3f-245">Jeśli `ValidateAntiForgeryToken` atrybut jest stosowany przez kontrolery aplikacji, może zostać zastąpiona przez `IgnoreAntiforgeryToken` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="aaa3f-246">Platformy ASP.NET Core nie obsługuje automatyczne dodawanie tokenów antiforgery żądania GET.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="aaa3f-247">Automatycznie sprawdzania poprawności tokenów antiforgery niebezpieczny metod HTTP</span><span class="sxs-lookup"><span data-stu-id="aaa3f-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="aaa3f-248">Aplikacje platformy ASP.NET Core nie generować antiforgery tokeny dla bezpieczne metody HTTP (GET, HEAD, opcje i śledzenia).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="aaa3f-249">Zamiast stosowania szeroko `ValidateAntiForgeryToken` atrybutu, a następnie przesłanianie go z `IgnoreAntiforgeryToken` atrybutów, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atrybut może być używany.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="aaa3f-250">Ten atrybut działa identycznie do `ValidateAntiForgeryToken` atrybutów, z wyjątkiem tego, czy nie wymaga tokeny żądań zostało nawiązane przy użyciu następujących metod HTTP:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="aaa3f-251">POBIERZ</span><span class="sxs-lookup"><span data-stu-id="aaa3f-251">GET</span></span>
* <span data-ttu-id="aaa3f-252">HEAD</span><span class="sxs-lookup"><span data-stu-id="aaa3f-252">HEAD</span></span>
* <span data-ttu-id="aaa3f-253">OPCJE</span><span class="sxs-lookup"><span data-stu-id="aaa3f-253">OPTIONS</span></span>
* <span data-ttu-id="aaa3f-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="aaa3f-254">TRACE</span></span>

<span data-ttu-id="aaa3f-255">Firma Microsoft zaleca użycie `AutoValidateAntiforgeryToken` szeroko dla scenariuszy-API.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="aaa3f-256">Dzięki temu akcje po są chronione przez domyślny.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="aaa3f-257">Alternatywą jest Ignoruj antiforgery tokeny domyślnie, chyba że `ValidateAntiForgeryToken` jest stosowany do poszczególnych metod akcji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="aaa3f-258">Go najprawdopodobniej w tym scenariuszu dla metody akcji POST pozostać niechronionej przez pomyłkę, pozostawiając narażony na ataki CSRF aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="aaa3f-259">Wszystkie wpisy należy wysłać antiforgery tokenu.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="aaa3f-260">Interfejsy API nie mają mechanizm automatycznego wysyłania-cookie część tokenu.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="aaa3f-261">Implementacja prawdopodobnie zależy od implementacji kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="aaa3f-262">Poniżej przedstawiono kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-262">Some examples are shown below:</span></span>

<span data-ttu-id="aaa3f-263">Przykład poziomie klasy:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="aaa3f-264">Przykład globalne:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="aaa3f-265">Zastąpienie globalnych lub atrybutów antiforgery kontrolera</span><span class="sxs-lookup"><span data-stu-id="aaa3f-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="aaa3f-266">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) użyć filtru, aby wyeliminować potrzebę antiforgery token dla danej akcji (lub kontrolera).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="aaa3f-267">Po zastosowaniu filtru zastępuje `ValidateAntiForgeryToken` i `AutoValidateAntiforgeryToken` filtry określone na wyższym poziomie (globalnie lub w kontrolerze).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="aaa3f-268">Tokenów odświeżania po uwierzytelnieniu</span><span class="sxs-lookup"><span data-stu-id="aaa3f-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="aaa3f-269">Tokeny należy odświeżyć, po uwierzytelnieniu użytkownika przez przekierowanie użytkownika do widoku lub strony stron Razor.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="aaa3f-270">JavaScript, AJAX i źródła</span><span class="sxs-lookup"><span data-stu-id="aaa3f-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="aaa3f-271">W tradycyjne aplikacje oparte na języku HTML antiforgery tokenów są przekazywane do serwera przy użyciu pól ukrytym.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="aaa3f-272">Nowoczesne aplikacje oparte na języku JavaScript i źródła wielu wniosków programowo.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="aaa3f-273">Te żądania AJAX mogą używać innych technik (np. nagłówki żądania lub pliki cookie) do wysyłania tokenu.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="aaa3f-274">Jeśli pliki cookie są używane do przechowywania tokeny uwierzytelniania i do uwierzytelniania żądań interfejsu API na serwerze, CSRF jest potencjalnym problemie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="aaa3f-275">Jeśli Magazyn lokalny jest używany do przechowywania token, CSRF luki w zabezpieczeniach mogą skorygowane, ponieważ wartości z magazynu lokalnego nie są automatycznie wysyłane do serwera z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="aaa3f-276">W związku z tym korzystanie z magazynu lokalnego do przechowywania antiforgery token na kliencie i wysyłania tokenu, ponieważ nagłówek żądania jest zalecane podejście.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="aaa3f-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="aaa3f-277">JavaScript</span></span>

<span data-ttu-id="aaa3f-278">Przy użyciu języka JavaScript z widokami, token mogą być tworzone przy użyciu usługi z widoku.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="aaa3f-279">Wstaw [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) usługi do widoku i wywołanie [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="aaa3f-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="aaa3f-280">Takie podejście eliminuje potrzebę bezpośrednio przez ustawienia plików cookie z serwera lub odczytywania ich z klienta.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="aaa3f-281">Powyższy przykład używa JavaScript można odczytać wartości pola ukrytego nagłówka AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="aaa3f-282">JavaScript można uzyskać dostępu do tokenów w plikach cookie i użyj zawartość pliku cookie, aby utworzyć nagłówek z wartością tokenu.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="aaa3f-283">Zakładając, że skrypt żądania do wysyłania tokenu w nagłówku o nazwie `X-CSRF-TOKEN`, skonfiguruj usługę antiforgery do wyszukania `X-CSRF-TOKEN` nagłówka:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="aaa3f-284">W poniższym przykładzie użyto JavaScript żądania AJAX z odpowiedni nagłówek:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

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

### <a name="angularjs"></a><span data-ttu-id="aaa3f-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="aaa3f-285">AngularJS</span></span>

<span data-ttu-id="aaa3f-286">AngularJS używa konwencji adres CSRF.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="aaa3f-287">Jeśli serwer wysyła plik cookie o nazwie `XSRF-TOKEN`, AngularJS `$http` usługi dodaje wartość pliku cookie do nagłówka podczas wysyłania żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="aaa3f-288">Ten proces odbywa się automatycznie.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-288">This process is automatic.</span></span> <span data-ttu-id="aaa3f-289">Nagłówek nie musi być ustawiony w sposób jawny.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="aaa3f-290">Nazwa nagłówka jest `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="aaa3f-291">Serwer powinna wykryć ten nagłówek i zweryfikować jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="aaa3f-292">Dla interfejsu API platformy ASP.NET Core współpracują z tę Konwencję:</span><span class="sxs-lookup"><span data-stu-id="aaa3f-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="aaa3f-293">Skonfiguruj aplikację, aby dostarczyć token w pliku cookie o nazwie `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="aaa3f-294">Skonfiguruj usługę antiforgery do wyszukania nagłówek o nazwie `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="aaa3f-295">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aaa3f-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="aaa3f-296">Rozszerzanie antiforgery</span><span class="sxs-lookup"><span data-stu-id="aaa3f-296">Extend antiforgery</span></span>

<span data-ttu-id="aaa3f-297">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typu umożliwia deweloperom rozszerzyć zachowanie systemu anti-CSRF przez dwustronną komunikację w każdym token dodatkowe dane.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="aaa3f-298">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) metoda jest wywoływana za każdym razem zostanie wygenerowany token pola i wartość zwracana jest osadzony w wygenerowany token.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="aaa3f-299">Realizator może zwracać sygnaturę czasową, identyfikatora jednorazowego lub wszelkie inne wartości, a następnie wywołać [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) do sprawdzania poprawności danych podczas weryfikowania tokenu.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="aaa3f-300">Nazwa użytkownika klienta już jest osadzony w generowanych tokenach, więc nie trzeba wpisywać te informacje.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="aaa3f-301">Jeśli token zawiera dane dodatkowe, ale nie `IAntiForgeryAdditionalDataProvider` jest skonfigurowany, dane dodatkowe nie jest zweryfikowany.</span><span class="sxs-lookup"><span data-stu-id="aaa3f-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aaa3f-302">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="aaa3f-302">Additional resources</span></span>

* <span data-ttu-id="aaa3f-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="aaa3f-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
