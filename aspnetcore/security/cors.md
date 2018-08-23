---
title: Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak CORS jako standardowe rozwiązanie dla zezwolenie lub odrzucenie żądań cross-origin w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2018
ms.locfileid: "41756339"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="29748-103">Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29748-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="29748-104">Przez [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="29748-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="29748-105">Poziom zabezpieczeń przeglądarki uniemożliwia strony sieci web wprowadzanie wysyłanie żądań AJAX do innej domeny.</span><span class="sxs-lookup"><span data-stu-id="29748-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="29748-106">To ograniczenie jest nazywany *zasadami tego samego źródła*i zapobiega złośliwych witryn odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="29748-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="29748-107">Jednak czasami możesz chcieć umożliwić innych witryn, wprowadzać żądań cross-origin w interfejsie API sieci web.</span><span class="sxs-lookup"><span data-stu-id="29748-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="29748-108">[Krzyżowe współużytkowanie zasobów](http://www.w3.org/TR/cors/) (między źródłami CORS) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="29748-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="29748-109">Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne.</span><span class="sxs-lookup"><span data-stu-id="29748-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="29748-110">CORS to bezpieczniejsze i bardziej elastyczne niż wcześniej techniki takie jak [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="29748-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="29748-111">W tym temacie pokazano, jak włączanie mechanizmu CORS w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="29748-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="29748-112">Co to jest "tego samego origin"?</span><span class="sxs-lookup"><span data-stu-id="29748-112">What is "same origin"?</span></span>

<span data-ttu-id="29748-113">Dwa adresy URL mają tego samego źródła, jeśli mają identyczne schematy, hosty i portów.</span><span class="sxs-lookup"><span data-stu-id="29748-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="29748-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="29748-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="29748-115">Te dwa adresy URL są tego samego źródła:</span><span class="sxs-lookup"><span data-stu-id="29748-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="29748-116">Te adresy URL są źródła innego niż poprzednie dwa:</span><span class="sxs-lookup"><span data-stu-id="29748-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="29748-117">`http://example.net` -Innej domeny</span><span class="sxs-lookup"><span data-stu-id="29748-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="29748-118">`http://www.example.com/foo.html` — Różne domeny podrzędnej</span><span class="sxs-lookup"><span data-stu-id="29748-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="29748-119">`https://example.com/foo.html` -Innego schematu</span><span class="sxs-lookup"><span data-stu-id="29748-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="29748-120">`http://example.com:9000/foo.html` -Innego portu</span><span class="sxs-lookup"><span data-stu-id="29748-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="29748-121">Program Internet Explorer nie bierze pod uwagę portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="29748-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="29748-122">Włączanie mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="29748-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="29748-123">Aby skonfigurować mechanizmu CORS w aplikacji Dodaj `Microsoft.AspNetCore.Cors` pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="29748-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="29748-124">Wywołaj [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="29748-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="29748-125">Włączanie funkcji CORS, za pomocą oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="29748-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="29748-126">Aby włączyć mechanizm CORS, Dodaj oprogramowanie pośredniczące CORS do potoku żądania przy użyciu `UseCors` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="29748-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="29748-127">Oprogramowanie pośredniczące CORS musi poprzedzać żadnych zdefiniowanych punktów końcowych w aplikacji, której chcesz obsługiwać żądań cross-origin (na przykład, zanim każde wywołanie `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="29748-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="29748-128">Podczas dodawania przy użyciu oprogramowanie pośredniczące CORS można określić zasady cross-origin [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) klasy.</span><span class="sxs-lookup"><span data-stu-id="29748-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="29748-129">Istnieją dwa sposoby, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="29748-129">There are two ways to do this.</span></span> <span data-ttu-id="29748-130">Pierwszy jest wywołanie `UseCors` za pomocą wyrażenia lambda:</span><span class="sxs-lookup"><span data-stu-id="29748-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="29748-131">**Uwaga:** musi być określony adres URL bez znaku ukośnika na końcu (`/`).</span><span class="sxs-lookup"><span data-stu-id="29748-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="29748-132">Jeśli adres URL kończy się za pomocą `/`, zwróci wynik porównania `false` i zostanie zwrócony bez nagłówka.</span><span class="sxs-lookup"><span data-stu-id="29748-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="29748-133">Wyrażenie lambda ma `CorsPolicyBuilder` obiektu.</span><span class="sxs-lookup"><span data-stu-id="29748-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="29748-134">Znajdziesz listę [opcje konfiguracji](#cors-policy-options) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="29748-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="29748-135">W tym przykładzie zasady umożliwiają żądań cross-origin z `http://example.com` i innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="29748-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="29748-136">CorsPolicyBuilder ma interfejs fluent API, dzięki czemu można połączyć w łańcuch wywołań metod:</span><span class="sxs-lookup"><span data-stu-id="29748-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="29748-137">Druga metoda jest zdefiniować co najmniej jedne zasady o nazwie CORS, a następnie wybierz pozycję zasady według nazwy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="29748-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="29748-138">Ten przykład dodaje zasady CORS, o nazwie "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="29748-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="29748-139">Aby wybrać zasady, należy przekazać nazwę aby `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="29748-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="29748-140">Włączanie funkcji CORS na platformie MVC</span><span class="sxs-lookup"><span data-stu-id="29748-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="29748-141">Można też użyć MVC do zastosowania określonego CORS każdej akcji, każdy kontroler lub globalnie dla wszystkich kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="29748-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="29748-142">Gdy za pomocą MVC w celu włączenia CORS tych samych usług mechanizmu CORS są używane, ale nie jest oprogramowanie pośredniczące CORS.</span><span class="sxs-lookup"><span data-stu-id="29748-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="29748-143">Za akcję</span><span class="sxs-lookup"><span data-stu-id="29748-143">Per action</span></span>

<span data-ttu-id="29748-144">Aby określić zasad CORS dla danego działania Dodaj `[EnableCors]` atrybutu akcji.</span><span class="sxs-lookup"><span data-stu-id="29748-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="29748-145">Podaj nazwę zasad.</span><span class="sxs-lookup"><span data-stu-id="29748-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="29748-146">Każdy kontroler</span><span class="sxs-lookup"><span data-stu-id="29748-146">Per controller</span></span>

<span data-ttu-id="29748-147">Aby określić zasady CORS dla określonego kontrolera Dodaj `[EnableCors]` atrybutów do klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="29748-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="29748-148">Podaj nazwę zasad.</span><span class="sxs-lookup"><span data-stu-id="29748-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="29748-149">Globalnie</span><span class="sxs-lookup"><span data-stu-id="29748-149">Globally</span></span>

<span data-ttu-id="29748-150">Można włączyć mechanizm CORS globalnie dla wszystkich kontrolerów przez dodanie `CorsAuthorizationFilterFactory` filtr do kolekcji filtrów globalnych:</span><span class="sxs-lookup"><span data-stu-id="29748-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="29748-151">Kolejność pierwszeństwa jest: działania, kontrolera, globalne.</span><span class="sxs-lookup"><span data-stu-id="29748-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="29748-152">Zasady na poziomie akcji mają pierwszeństwo przed zasad na poziomie kontrolera, a zasady na poziomie kontrolera mają pierwszeństwo przed zasad globalnych.</span><span class="sxs-lookup"><span data-stu-id="29748-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="29748-153">Wyłączenia CORS</span><span class="sxs-lookup"><span data-stu-id="29748-153">Disable CORS</span></span>

<span data-ttu-id="29748-154">Aby wyłączyć CORS dla kontrolera lub akcji, należy użyć `[DisableCors]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="29748-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="29748-155">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="29748-155">CORS policy options</span></span>

<span data-ttu-id="29748-156">W tej sekcji opisano różne opcje, które można ustawić zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="29748-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="29748-157">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="29748-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="29748-158">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="29748-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="29748-159">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="29748-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="29748-160">Ustawianie nagłówków odpowiedzi narażone</span><span class="sxs-lookup"><span data-stu-id="29748-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="29748-161">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="29748-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="29748-162">Ustaw czas wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="29748-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="29748-163">Niektóre opcje mogą być pomocne dla odczytu [działa jak CORS](#how-cors-works) pierwszy.</span><span class="sxs-lookup"><span data-stu-id="29748-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="29748-164">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="29748-164">Set the allowed origins</span></span>

<span data-ttu-id="29748-165">Aby zezwolić na co najmniej jeden z określonych źródeł:</span><span class="sxs-lookup"><span data-stu-id="29748-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="29748-166">Aby zezwolić na wszystkie pochodzenia:</span><span class="sxs-lookup"><span data-stu-id="29748-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="29748-167">Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła.</span><span class="sxs-lookup"><span data-stu-id="29748-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="29748-168">Oznacza to, że dosłownie w dowolnej witrynie sieci Web mogą przesłać wywołania AJAX do interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="29748-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="29748-169">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="29748-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="29748-170">Aby zezwolić na wszystkie metody HTTP:</span><span class="sxs-lookup"><span data-stu-id="29748-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="29748-171">Dotyczy to żądań krótkiej i nagłówka Access-kontroli-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="29748-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="29748-172">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="29748-172">Set the allowed request headers</span></span>

<span data-ttu-id="29748-173">Żądania wstępnego CORS może obejmować nagłówka Access-Control-Request-Headers, lista nagłówków HTTP, ustawiane przez aplikację (tak zwane "Autor nagłówki żądania").</span><span class="sxs-lookup"><span data-stu-id="29748-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="29748-174">Do listy dozwolonych nagłówków określonych:</span><span class="sxs-lookup"><span data-stu-id="29748-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="29748-175">Aby umożliwić Twórz wszystkie nagłówki żądania:</span><span class="sxs-lookup"><span data-stu-id="29748-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="29748-176">Przeglądarki nie są całkowicie zgodne, w jaki ustawiają Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="29748-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="29748-177">Jeśli ustawisz nagłówki do żadnego elementu innego niż "\*", użytkownik powinien zawierać co najmniej "Zaakceptuj", "content-type" i "origin", a także niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="29748-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="29748-178">Ustawianie nagłówków odpowiedzi narażone</span><span class="sxs-lookup"><span data-stu-id="29748-178">Set the exposed response headers</span></span>

<span data-ttu-id="29748-179">Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="29748-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="29748-180">(Zobacz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="29748-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="29748-181">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="29748-181">Cache-Control</span></span>

* <span data-ttu-id="29748-182">Język zawartości</span><span class="sxs-lookup"><span data-stu-id="29748-182">Content-Language</span></span>

* <span data-ttu-id="29748-183">Typ zawartości</span><span class="sxs-lookup"><span data-stu-id="29748-183">Content-Type</span></span>

* <span data-ttu-id="29748-184">Wygasa</span><span class="sxs-lookup"><span data-stu-id="29748-184">Expires</span></span>

* <span data-ttu-id="29748-185">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="29748-185">Last-Modified</span></span>

* <span data-ttu-id="29748-186">Dyrektywy pragma</span><span class="sxs-lookup"><span data-stu-id="29748-186">Pragma</span></span>

<span data-ttu-id="29748-187">Specyfikacja CORS wywołuje te *nagłówki odpowiedzi proste*.</span><span class="sxs-lookup"><span data-stu-id="29748-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="29748-188">Aby udostępnić innych nagłówków do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="29748-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="29748-189">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="29748-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="29748-190">Poświadczenia wymagają specjalnej obsługi żądania CORS.</span><span class="sxs-lookup"><span data-stu-id="29748-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="29748-191">Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem cross-origin.</span><span class="sxs-lookup"><span data-stu-id="29748-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="29748-192">Poświadczenia obejmują pliki cookie, a także schematów uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="29748-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="29748-193">Do wysyłania poświadczeń przy użyciu żądania między źródłami, klient musi ustawić XMLHttpRequest.withCredentials na wartość true.</span><span class="sxs-lookup"><span data-stu-id="29748-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="29748-194">Bezpośrednio za pomocą obiektu XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="29748-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="29748-195">W technologii jQuery:</span><span class="sxs-lookup"><span data-stu-id="29748-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="29748-196">Ponadto serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="29748-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="29748-197">Aby zezwolić na współużytkowanie poświadczeń:</span><span class="sxs-lookup"><span data-stu-id="29748-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="29748-198">Teraz odpowiedzi HTTP będzie zawierać nagłówka Access-kontroli-Allow-Credentials, który informuje przeglądarkę o tym, że serwer pozwala poświadczenia dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="29748-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="29748-199">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka Access-kontroli-Allow-Credentials, przeglądarka nie będzie ujawniać odpowiedź do aplikacji, a żądanie AJAX nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="29748-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="29748-200">Należy zachować ostrożność, dzięki czemu poświadczenia cross-origin.</span><span class="sxs-lookup"><span data-stu-id="29748-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="29748-201">Witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="29748-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="29748-202">Specyfikacja CORS również stany to ustawienie źródeł do `"*"` (wszystkie źródła) jest nieprawidłowa Jeśli `Access-Control-Allow-Credentials` nagłówka znajduje się.</span><span class="sxs-lookup"><span data-stu-id="29748-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="29748-203">Ustaw czas wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="29748-203">Set the preflight expiration time</span></span>

<span data-ttu-id="29748-204">Nagłówek dostępu — kontrola-Max-Age Określa, jak długo mogą być buforowane odpowiedzi na żądania wstępnego.</span><span class="sxs-lookup"><span data-stu-id="29748-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="29748-205">Aby ustawić tego nagłówka:</span><span class="sxs-lookup"><span data-stu-id="29748-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="29748-206">Jak działa mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="29748-206">How CORS works</span></span>

<span data-ttu-id="29748-207">W tej sekcji opisano, co się stanie, w ramach żądania CORS na poziomie wiadomości HTTP.</span><span class="sxs-lookup"><span data-stu-id="29748-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="29748-208">Należy zrozumieć, jak CORS działa tak, aby zasady CORS może być prawidłowo skonfigurowane i debugowania, gdy wystąpią nieoczekiwane wyniki.</span><span class="sxs-lookup"><span data-stu-id="29748-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="29748-209">Specyfikacja CORS wprowadza kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="29748-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="29748-210">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="29748-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="29748-211">Niestandardowy kod JavaScript nie jest wymagane, aby włączyć mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="29748-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="29748-212">Oto przykład żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="29748-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="29748-213">`Origin` Nagłówek zapewnia domeny obiektu, z której wysłano żądanie:</span><span class="sxs-lookup"><span data-stu-id="29748-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="29748-214">Jeśli serwer zezwala na żądanie, ustawia nagłówka Access-Control-Allow-Origin w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="29748-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="29748-215">Wartość tego nagłówka stanowi odpowiada nagłówek źródła z żądania albo jest wartością symbol wieloznaczny "\*", co oznacza, że każde źródło jest dozwolony:</span><span class="sxs-lookup"><span data-stu-id="29748-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="29748-216">Odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="29748-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="29748-217">W szczególności przeglądarki nie zezwalają na żądanie.</span><span class="sxs-lookup"><span data-stu-id="29748-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="29748-218">Nawet jeśli serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi do aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="29748-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="29748-219">Żądania wstępnego</span><span class="sxs-lookup"><span data-stu-id="29748-219">Preflight Requests</span></span>

<span data-ttu-id="29748-220">W przypadku niektórych żądań CORPS przeglądarki przesyła wysłanie dodatkowego żądania o nazwie "żądania wstępnego", przed wysłaniem rzeczywistego żądania dla zasobu.</span><span class="sxs-lookup"><span data-stu-id="29748-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="29748-221">Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="29748-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="29748-222">Metoda żądania jest GET, HEAD lub WPIS, a</span><span class="sxs-lookup"><span data-stu-id="29748-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="29748-223">Aplikacja nie zmienia żadnych nagłówków żądania innego niż zawartości Accept, Accept-Language-Language, Content-Type lub ostatni-Event-ID, a</span><span class="sxs-lookup"><span data-stu-id="29748-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="29748-224">Nagłówek Content-Type (jeśli ustawić) jest jednym z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="29748-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="29748-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="29748-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="29748-226">multipart/formularza data</span><span class="sxs-lookup"><span data-stu-id="29748-226">multipart/form-data</span></span>

  * <span data-ttu-id="29748-227">zwykły tekst</span><span class="sxs-lookup"><span data-stu-id="29748-227">text/plain</span></span>

<span data-ttu-id="29748-228">Reguła o nagłówki żądania dotyczy nagłówki, które aplikacja ustawia przez wywołanie metody setRequestHeader obiektu XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="29748-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="29748-229">(Specyfikacji CORS wywołuje te "Autor nagłówki żądania"). Reguła nie ma zastosowania do nagłówków, które można ustawić przeglądarki, takich jak Agent użytkownika, Host lub Content-Length.</span><span class="sxs-lookup"><span data-stu-id="29748-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="29748-230">Oto przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="29748-230">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="29748-231">Żądanie krótkiej wykorzystuje metodę OPTIONS protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="29748-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="29748-232">Zawiera ona dwie specjalnych nagłówków:</span><span class="sxs-lookup"><span data-stu-id="29748-232">It includes two special headers:</span></span>

* <span data-ttu-id="29748-233">Access-Control-Request-Method: Metoda HTTP, która będzie służyć do rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="29748-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="29748-234">Access-Control-Request-Headers: Lista nagłówków żądań, które aplikacja jest ustawiona na rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="29748-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="29748-235">(Ponownie, to nie obejmuje nagłówki, które ustawia w przeglądarce).</span><span class="sxs-lookup"><span data-stu-id="29748-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="29748-236">Poniżej przedstawiono przykładową odpowiedź, przy założeniu, że serwer zezwala na żądanie:</span><span class="sxs-lookup"><span data-stu-id="29748-236">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="29748-237">Odpowiedź zawiera nagłówek dostępu — kontrola-Allow-Methods, zawierającego dozwolone metody i, opcjonalnie nagłówka Access-Control-Zezwalaj-Headers, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="29748-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="29748-238">Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania, zgodnie z wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="29748-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
