---
title: "Włączanie żądań Cross-Origin (CORS)"
author: rick-anderson
description: "Ten dokument wprowadza CORS jako standard zezwalających lub odrzucanie żądań cross-origin w aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: e6b49b9dde94cc7d035ea91b992a13df8cb8caf2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="enabling-cross-origin-requests-cors"></a><span data-ttu-id="be7e2-103">Włączanie żądań Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="be7e2-103">Enabling Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="be7e2-104">Przez [Wasson Jan](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), i [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="be7e2-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="be7e2-105">Poziom zabezpieczeń przeglądarki uniemożliwia wprowadzanie żądania AJAX do innej domeny przez stronę sieci web.</span><span class="sxs-lookup"><span data-stu-id="be7e2-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="be7e2-106">To ograniczenie jest nazywany *zasad samego pochodzenia*i zapobiega złośliwa witryna odczytywanie danych poufnych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="be7e2-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="be7e2-107">Jednak czasami może mają mieć możliwość innych witryn, wprowadzić żądań cross-origin w interfejsie API sieci web.</span><span class="sxs-lookup"><span data-stu-id="be7e2-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="be7e2-108">[Krzyżowe udostępniania zasobów pochodzenia](http://www.w3.org/TR/cors/) (CORS) jest standardem W3C, dzięki której serwer złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="be7e2-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="be7e2-109">Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin podczas odrzucenia innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="be7e2-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="be7e2-110">CORS jest bezpieczniejsze i bardziej elastyczne niż wcześniejszych metod takich jak [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="be7e2-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="be7e2-111">W tym temacie pokazano, jak włączyć mechanizm CORS w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="be7e2-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="be7e2-112">Co to jest "tego samego źródła"?</span><span class="sxs-lookup"><span data-stu-id="be7e2-112">What is "same origin"?</span></span>

<span data-ttu-id="be7e2-113">Dwa adresy URL mają tego samego źródła, gdy mają identyczne schematy, hostów i portów.</span><span class="sxs-lookup"><span data-stu-id="be7e2-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="be7e2-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="be7e2-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="be7e2-115">Te dwa adresy URL są tego samego źródła:</span><span class="sxs-lookup"><span data-stu-id="be7e2-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="be7e2-116">Tych adresów URL mają różne źródła niż poprzedniej dwóch:</span><span class="sxs-lookup"><span data-stu-id="be7e2-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="be7e2-117">`http://example.net`-Innej domeny</span><span class="sxs-lookup"><span data-stu-id="be7e2-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="be7e2-118">`http://www.example.com/foo.html`-Różnych poddomeny</span><span class="sxs-lookup"><span data-stu-id="be7e2-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="be7e2-119">`https://example.com/foo.html`-Inny schemat</span><span class="sxs-lookup"><span data-stu-id="be7e2-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="be7e2-120">`http://example.com:9000/foo.html`-Różnych portów:</span><span class="sxs-lookup"><span data-stu-id="be7e2-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="be7e2-121">Program Internet Explorer nie należy wziąć pod uwagę portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="be7e2-121">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="be7e2-122">Konfigurowanie mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="be7e2-122">Setting up CORS</span></span>

<span data-ttu-id="be7e2-123">Aby skonfigurować dodać CORS dla aplikacji `Microsoft.AspNetCore.Cors` pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="be7e2-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="be7e2-124">Dodaj usługi CORS w pliku Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="be7e2-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="be7e2-125">Włączanie mechanizmu CORS z oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="be7e2-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="be7e2-126">Aby włączyć mechanizm CORS dla całej aplikacji Dodaj oprogramowanie pośredniczące CORS do Twojego żądania potoku przy użyciu `UseCors` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="be7e2-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="be7e2-127">Należy pamiętać, że oprogramowanie pośredniczące CORS musi poprzedzać żadnych zdefiniowanych punktów końcowych w aplikacji do obsługi żądań cross-origin (np.</span><span class="sxs-lookup"><span data-stu-id="be7e2-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="be7e2-128">przed wywołaniem dowolnej `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="be7e2-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="be7e2-129">Podczas dodawania przy użyciu oprogramowanie pośredniczące CORS, można określić zasady cross-origin `CorsPolicyBuilder` klasy.</span><span class="sxs-lookup"><span data-stu-id="be7e2-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="be7e2-130">Istnieją dwa sposoby, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="be7e2-130">There are two ways to do this.</span></span> <span data-ttu-id="be7e2-131">Pierwsza to wywołanie UseCors z wyrażenia lambda:</span><span class="sxs-lookup"><span data-stu-id="be7e2-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="be7e2-132">**Uwaga:** musi być określony adres URL bez ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="be7e2-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="be7e2-133">Jeśli adres URL kończy z `/`, zwróci porównanie `false` i zostanie zwrócony bez nagłówka.</span><span class="sxs-lookup"><span data-stu-id="be7e2-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="be7e2-134">Wyrażenie lambda ma `CorsPolicyBuilder` obiektu.</span><span class="sxs-lookup"><span data-stu-id="be7e2-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="be7e2-135">Listę można znaleźć [opcje konfiguracji](#cors-policy-options) dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="be7e2-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="be7e2-136">W tym przykładzie zasady umożliwia żądań cross-origin z `http://example.com` i innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="be7e2-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="be7e2-137">Należy pamiętać, że CorsPolicyBuilder ma fluent API, więc tworzenia łańcucha wywołań metody:</span><span class="sxs-lookup"><span data-stu-id="be7e2-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="be7e2-138">Drugi podejściem jest zdefiniować co najmniej jedne zasady CORS nazwanego, a następnie wybierz zasady według nazwy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="be7e2-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="be7e2-139">W tym przykładzie dodaje zasady CORS o nazwie "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="be7e2-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="be7e2-140">Aby wybrać zasady, przekaż nazwę `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="be7e2-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="be7e2-141">Włączanie mechanizmu CORS na platformie MVC</span><span class="sxs-lookup"><span data-stu-id="be7e2-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="be7e2-142">MVC służy również do zastosowania określonego CORS każdej akcji, każdy kontroler lub globalnie do wszystkich kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="be7e2-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="be7e2-143">W przypadku używania MVC do włączenia CORS są używane te same usługi CORS, ale nie jest oprogramowanie pośredniczące CORS.</span><span class="sxs-lookup"><span data-stu-id="be7e2-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware is not.</span></span>

### <a name="per-action"></a><span data-ttu-id="be7e2-144">Każdej akcji</span><span class="sxs-lookup"><span data-stu-id="be7e2-144">Per action</span></span>

<span data-ttu-id="be7e2-145">Aby określić Dodaj zasad CORS dla danego działania `[EnableCors]` atrybutu w celu wykonania akcji.</span><span class="sxs-lookup"><span data-stu-id="be7e2-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="be7e2-146">Określ nazwę zasady.</span><span class="sxs-lookup"><span data-stu-id="be7e2-146">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="be7e2-147">Każdy kontroler</span><span class="sxs-lookup"><span data-stu-id="be7e2-147">Per controller</span></span>

<span data-ttu-id="be7e2-148">Aby określić zasady CORS dla określonego kontrolera Dodaj `[EnableCors]` atrybutu do klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="be7e2-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="be7e2-149">Określ nazwę zasady.</span><span class="sxs-lookup"><span data-stu-id="be7e2-149">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="be7e2-150">Globalny</span><span class="sxs-lookup"><span data-stu-id="be7e2-150">Globally</span></span>

<span data-ttu-id="be7e2-151">Można włączyć mechanizm CORS globalnie do wszystkich kontrolerów przez dodanie `CorsAuthorizationFilterFactory` filtr do kolekcji filtrów globalnych:</span><span class="sxs-lookup"><span data-stu-id="be7e2-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="be7e2-152">Kolejność jest: działania, kontrolera, globalnych.</span><span class="sxs-lookup"><span data-stu-id="be7e2-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="be7e2-153">Zasad na poziomie akcji mają pierwszeństwo przed zasad na poziomie kontrolera, a zasad na poziomie kontrolera mają pierwszeństwo przed zasadami globalnego.</span><span class="sxs-lookup"><span data-stu-id="be7e2-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="be7e2-154">Wyłączenia CORS</span><span class="sxs-lookup"><span data-stu-id="be7e2-154">Disable CORS</span></span>

<span data-ttu-id="be7e2-155">Aby wyłączyć CORS dla kontrolera lub akcji, należy użyć `[DisableCors]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="be7e2-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="be7e2-156">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="be7e2-156">CORS policy options</span></span>

<span data-ttu-id="be7e2-157">W tej sekcji opisano różne opcje, które można ustawić zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="be7e2-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="be7e2-158">Ustaw dozwolonych źródeł</span><span class="sxs-lookup"><span data-stu-id="be7e2-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="be7e2-159">Ustaw dopuszczalnych metod HTTP</span><span class="sxs-lookup"><span data-stu-id="be7e2-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="be7e2-160">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="be7e2-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="be7e2-161">Ustawianie nagłówków odpowiedzi narażonych</span><span class="sxs-lookup"><span data-stu-id="be7e2-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="be7e2-162">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="be7e2-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="be7e2-163">Ustawianie czasu wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="be7e2-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="be7e2-164">Dla niektórych opcji może być przydatne do odczytu [działa jak CORS](#how-cors-works) pierwszy.</span><span class="sxs-lookup"><span data-stu-id="be7e2-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="be7e2-165">Ustaw dozwolonych źródeł</span><span class="sxs-lookup"><span data-stu-id="be7e2-165">Set the allowed origins</span></span>

<span data-ttu-id="be7e2-166">Aby zezwolić na co najmniej jeden z określonych źródeł:</span><span class="sxs-lookup"><span data-stu-id="be7e2-166">To allow one or more specific origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="be7e2-167">Aby zezwolić na wszystkie pochodzenia:</span><span class="sxs-lookup"><span data-stu-id="be7e2-167">To allow all origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="be7e2-168">Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła.</span><span class="sxs-lookup"><span data-stu-id="be7e2-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="be7e2-169">Oznacza to, że dosłownie w dowolnej witrynie sieci Web można wykonywać wywołania AJAX do interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="be7e2-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="be7e2-170">Ustaw dopuszczalnych metod HTTP</span><span class="sxs-lookup"><span data-stu-id="be7e2-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="be7e2-171">Aby zezwolić na wszystkie metody HTTP:</span><span class="sxs-lookup"><span data-stu-id="be7e2-171">To allow all HTTP methods:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="be7e2-172">Ma to wpływ na żądania wstępnego transmitowane i nagłówek dostępu-formant-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="be7e2-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="be7e2-173">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="be7e2-173">Set the allowed request headers</span></span>

<span data-ttu-id="be7e2-174">Żądania wstępnego CORS może obejmować nagłówka Access-Control-Request-Headers, lista nagłówków HTTP, ustawiane przez aplikację (tak zwane "Autor nagłówki żądania").</span><span class="sxs-lookup"><span data-stu-id="be7e2-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="be7e2-175">Do listy dozwolonych adresów IP określonych nagłówków:</span><span class="sxs-lookup"><span data-stu-id="be7e2-175">To whitelist specific headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="be7e2-176">Aby umożliwić tworzenie wszystkie nagłówki żądania:</span><span class="sxs-lookup"><span data-stu-id="be7e2-176">To allow all author request headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="be7e2-177">Przeglądarki nie są całkowicie zgodne, w konfiguracji do programu Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="be7e2-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="be7e2-178">Po ustawieniu nagłówki na niczego innych niż "\*", użytkownik powinien zawierać co najmniej "Zaakceptuj", "content-type", "origin", a także niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="be7e2-178">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="be7e2-179">Ustawianie nagłówków odpowiedzi narażonych</span><span class="sxs-lookup"><span data-stu-id="be7e2-179">Set the exposed response headers</span></span>

<span data-ttu-id="be7e2-180">Domyślnie przeglądarka nie ujawnia wszystkich nagłówków odpowiedzi do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be7e2-180">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="be7e2-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="be7e2-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="be7e2-182">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="be7e2-182">Cache-Control</span></span>

* <span data-ttu-id="be7e2-183">Język zawartości</span><span class="sxs-lookup"><span data-stu-id="be7e2-183">Content-Language</span></span>

* <span data-ttu-id="be7e2-184">Typ zawartości</span><span class="sxs-lookup"><span data-stu-id="be7e2-184">Content-Type</span></span>

* <span data-ttu-id="be7e2-185">Wygasa</span><span class="sxs-lookup"><span data-stu-id="be7e2-185">Expires</span></span>

* <span data-ttu-id="be7e2-186">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="be7e2-186">Last-Modified</span></span>

* <span data-ttu-id="be7e2-187">Wartość dyrektywy pragma</span><span class="sxs-lookup"><span data-stu-id="be7e2-187">Pragma</span></span>

<span data-ttu-id="be7e2-188">Specyfikacja CORS wywołuje te *nagłówki odpowiedzi proste*.</span><span class="sxs-lookup"><span data-stu-id="be7e2-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="be7e2-189">Aby udostępnić nagłówki innych aplikacji:</span><span class="sxs-lookup"><span data-stu-id="be7e2-189">To make other headers available to the application:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="be7e2-190">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="be7e2-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="be7e2-191">Poświadczenia wymagają specjalnej obsługi żądania CORS.</span><span class="sxs-lookup"><span data-stu-id="be7e2-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="be7e2-192">Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem cross-origin.</span><span class="sxs-lookup"><span data-stu-id="be7e2-192">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="be7e2-193">Poświadczenia obejmują pliki cookie, a także schematy uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="be7e2-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="be7e2-194">Aby wysłać poświadczeń z żądaniem cross-origin, klient musi Ustaw XMLHttpRequest.withCredentials wartość true.</span><span class="sxs-lookup"><span data-stu-id="be7e2-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="be7e2-195">Bezpośrednio za pomocą obiektu XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="be7e2-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="be7e2-196">W jQuery:</span><span class="sxs-lookup"><span data-stu-id="be7e2-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="be7e2-197">Ponadto poświadczenia muszą zezwalać na serwerze.</span><span class="sxs-lookup"><span data-stu-id="be7e2-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="be7e2-198">Aby umożliwić cross-origin poświadczeń:</span><span class="sxs-lookup"><span data-stu-id="be7e2-198">To allow cross-origin credentials:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="be7e2-199">Teraz odpowiedzi HTTP będzie zawierać nagłówka dostępu-formant-Allow-Credentials, który informuje przeglądarkę, że serwer umożliwia poświadczenia dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="be7e2-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="be7e2-200">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka dostępu-formant-Allow-Credentials, przeglądarka nie powoduje to udostępnienie odpowiedzi do aplikacji i żądanie AJAX nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="be7e2-200">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="be7e2-201">Należy zachować ostrożność bardzo stosowanie cross-origin poświadczeń, ponieważ oznacza to, że witryna sieci Web w innej domenie można wysłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="be7e2-201">Be very careful about allowing cross-origin credentials, because it means a website at another domain can send a logged-in user’s credentials to your app on the user’s behalf, without the user being aware.</span></span> <span data-ttu-id="be7e2-202">CORS spec również stanów tego ustawienia źródeł do "\*" (wszystkie pochodzenia) jest nieprawidłowy, jeśli jest obecny nagłówek dostępu-formant-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="be7e2-202">The CORS spec also states that setting origins to "\*" (all origins) is invalid if the Access-Control-Allow-Credentials header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="be7e2-203">Ustawianie czasu wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="be7e2-203">Set the preflight expiration time</span></span>

<span data-ttu-id="be7e2-204">Nagłówka Access-formant-Max-Age Określa, jak długo mogą być buforowane odpowiedzi na żądania wstępnego.</span><span class="sxs-lookup"><span data-stu-id="be7e2-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="be7e2-205">Aby ustawić ten nagłówek:</span><span class="sxs-lookup"><span data-stu-id="be7e2-205">To set this header:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="be7e2-206">Jak działa CORS</span><span class="sxs-lookup"><span data-stu-id="be7e2-206">How CORS works</span></span>

<span data-ttu-id="be7e2-207">W tej sekcji opisano, co się stanie w żądanie CORS na poziomie wiadomości HTTP.</span><span class="sxs-lookup"><span data-stu-id="be7e2-207">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="be7e2-208">Należy zrozumieć sposób działania CORS, tak aby poprawnie skonfigurować zasady CORS i rozwiązywanie problemów, jeśli elementy nie działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="be7e2-208">It’s important to understand how CORS works, so that you can configure your CORS policy correctly, and troubleshoot if things don’t work as you expect.</span></span>

<span data-ttu-id="be7e2-209">Specyfikacja CORS wprowadzono kilka nowych nagłówków HTTP, które Włączanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="be7e2-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="be7e2-210">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia automatycznie tych nagłówków żądań cross-origin; nie trzeba wykonywać żadnych czynności specjalne w kodzie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="be7e2-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don’t need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="be7e2-211">Oto przykład żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="be7e2-211">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="be7e2-212">Nagłówek "Origin" daje domeny witryny, do której wysłano żądanie:</span><span class="sxs-lookup"><span data-stu-id="be7e2-212">The "Origin" header gives the domain of the site that is making the request:</span></span>

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

<span data-ttu-id="be7e2-213">Jeśli serwer zezwala na żądanie, ustawia nagłówka Access-Control-Allow-Origin w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="be7e2-213">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="be7e2-214">Wartość tego nagłówka dopasowuje początek nagłówek z żądania albo ma wartość symbol wieloznaczny "\*", co oznacza, że każde źródło jest dozwolone:</span><span class="sxs-lookup"><span data-stu-id="be7e2-214">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="be7e2-215">Odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="be7e2-215">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="be7e2-216">W szczególności przeglądarki nie zezwala na żądanie.</span><span class="sxs-lookup"><span data-stu-id="be7e2-216">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="be7e2-217">Nawet wtedy, gdy serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="be7e2-217">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="be7e2-218">Żądania wstępnego</span><span class="sxs-lookup"><span data-stu-id="be7e2-218">Preflight Requests</span></span>

<span data-ttu-id="be7e2-219">Dla niektórych żądań CORS przeglądarce wysyła żądanie dodatkowych, o nazwie "żądania wstępnego", przed wysłaniem rzeczywistego żądania dla zasobu.</span><span class="sxs-lookup"><span data-stu-id="be7e2-219">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="be7e2-220">Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="be7e2-220">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="be7e2-221">Metoda żądania jest GET, HEAD lub POST, a</span><span class="sxs-lookup"><span data-stu-id="be7e2-221">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="be7e2-222">Aplikacja nie określa żadnych nagłówków żądania innego niż Akceptuj, Accept-Language, Content-Language, Content-Type lub Last-zdarzenia-ID, a</span><span class="sxs-lookup"><span data-stu-id="be7e2-222">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="be7e2-223">Nagłówek Content-Type (Jeśli ustawiona) jest jednym z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="be7e2-223">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="be7e2-224">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="be7e2-224">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="be7e2-225">dane multipart/formularza</span><span class="sxs-lookup"><span data-stu-id="be7e2-225">multipart/form-data</span></span>

  * <span data-ttu-id="be7e2-226">zwykły tekst</span><span class="sxs-lookup"><span data-stu-id="be7e2-226">text/plain</span></span>

<span data-ttu-id="be7e2-227">Reguła o nagłówków żądań ma zastosowanie do nagłówki, które ustawia aplikacji przez wywołanie metody setRequestHeader obiektu XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="be7e2-227">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="be7e2-228">(Specyfikacja CORS wywołuje te "Autor nagłówki żądania"). Reguła nie ma zastosowania do nagłówki, które można ustawić przeglądarki, takich jak Agent użytkownika, hosta lub Content-Length.</span><span class="sxs-lookup"><span data-stu-id="be7e2-228">(The CORS specification calls these "author request headers".) The rule does not apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="be7e2-229">Oto przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="be7e2-229">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="be7e2-230">Żądania wstępnego transmitowane używa metody OPTIONS protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="be7e2-230">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="be7e2-231">Zawiera dwa nagłówki specjalne:</span><span class="sxs-lookup"><span data-stu-id="be7e2-231">It includes two special headers:</span></span>

* <span data-ttu-id="be7e2-232">Access-Control-Request-Method: Metoda HTTP, która będzie służyć do rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="be7e2-232">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="be7e2-233">Access-Control-Request-Headers: Lista nagłówków żądań, których aplikacja jest ustawiona na rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="be7e2-233">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="be7e2-234">(Ponownie, ta nie obejmuje nagłówki, które ustawia przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="be7e2-234">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="be7e2-235">Oto przykład odpowiedzi, przy założeniu, że serwer zezwala na żądanie:</span><span class="sxs-lookup"><span data-stu-id="be7e2-235">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="be7e2-236">Odpowiedź zawiera nagłówek dostępu-formant-Allow-Methods, który wyświetla listę dozwolonych metod i opcjonalnie nagłówka Access-Control-Zezwalaj-Headers, który zawiera listę dozwolone nagłówki.</span><span class="sxs-lookup"><span data-stu-id="be7e2-236">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="be7e2-237">Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania, zgodnie z wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="be7e2-237">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>