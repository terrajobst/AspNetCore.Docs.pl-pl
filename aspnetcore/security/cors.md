---
title: Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak CORS jako standardowe rozwiązanie dla zezwolenie lub odrzucenie żądań cross-origin w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458546"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="0d857-103">Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d857-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="0d857-104">Przez [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0d857-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0d857-105">Poziom zabezpieczeń przeglądarki zapobiega wysyłania żądań do innej domeny niż ta, która obsłużyła stronę sieci web strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="0d857-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="0d857-106">To ograniczenie jest nazywany *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="0d857-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="0d857-107">Zasada tego samego źródła uniemożliwia złośliwych witryn odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="0d857-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="0d857-108">Czasami możesz chcieć zezwala na innych witryn wprowadzać żądań cross-origin Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0d857-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="0d857-109">[Krzyżowe współużytkowanie zasobów](https://www.w3.org/TR/cors/) (między źródłami CORS) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="0d857-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="0d857-110">Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne.</span><span class="sxs-lookup"><span data-stu-id="0d857-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="0d857-111">CORS to bezpieczniejsze i bardziej elastyczne niż wcześniej technik, takich jak [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="0d857-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="0d857-112">W tym temacie pokazano, jak włączanie mechanizmu CORS w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0d857-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="0d857-113">Tego samego źródła</span><span class="sxs-lookup"><span data-stu-id="0d857-113">Same origin</span></span>

<span data-ttu-id="0d857-114">Dwa adresy URL mają tego samego źródła, jeśli mają one hostów, porty i schematy identyczne ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="0d857-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="0d857-115">Te dwa adresy URL są tego samego źródła:</span><span class="sxs-lookup"><span data-stu-id="0d857-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="0d857-116">Te adresy URL są różne źródła niż poprzednie dwa adresy URL:</span><span class="sxs-lookup"><span data-stu-id="0d857-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="0d857-117">`https://example.net` &ndash; Inną domenę</span><span class="sxs-lookup"><span data-stu-id="0d857-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="0d857-118">`https://www.example.com/foo.html` &ndash; Różne domeny podrzędnej</span><span class="sxs-lookup"><span data-stu-id="0d857-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="0d857-119">`http://example.com/foo.html` &ndash; Inny schemat</span><span class="sxs-lookup"><span data-stu-id="0d857-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="0d857-120">`https://example.com:9000/foo.html` &ndash; Inny port</span><span class="sxs-lookup"><span data-stu-id="0d857-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="0d857-121">Program Internet Explorer nie bierze pod uwagę portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="0d857-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="0d857-122">Rejestrowanie usługi CORS</span><span class="sxs-lookup"><span data-stu-id="0d857-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0d857-123">Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="0d857-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0d857-124">Odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="0d857-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0d857-125">Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="0d857-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="0d857-126">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> w `Startup.ConfigureServices` nad dodaniem usług CORS do aplikacji kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="0d857-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="0d857-127">Włączanie mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="0d857-127">Enable CORS</span></span>

<span data-ttu-id="0d857-128">Po zarejestrowaniu mechanizmu CORS usługi, użyj jednej z następujących metod Włączanie mechanizmu CORS w aplikacji ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0d857-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="0d857-129">[Oprogramowanie pośredniczące CORS](#enable-cors-with-cors-middleware) &ndash; zasady CORS do zastosowania globalnie do aplikacji za pomocą oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="0d857-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="0d857-130">[Mechanizm CORS w MVC](#enable-cors-in-mvc) &ndash; zasady CORS do zastosowania na akcji lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d857-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="0d857-131">Oprogramowanie pośredniczące CORS nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="0d857-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="0d857-132">Włączanie mechanizmu CORS z oprogramowaniem pośredniczącym CORS</span><span class="sxs-lookup"><span data-stu-id="0d857-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="0d857-133">Oprogramowanie pośredniczące CORS obsługuje żądań cross-origin w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0d857-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="0d857-134">Aby włączyć oprogramowanie pośredniczące CORS w potoku przetwarzania żądań, należy wywołać <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metody rozszerzenia w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0d857-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="0d857-135">Oprogramowanie pośredniczące CORS musi poprzedzać żadnych zdefiniowanych punktów końcowych w aplikacji, której chcesz obsługiwać żądań cross-origin (na przykład przed wywołaniem do `UseMvc` dla oprogramowania pośredniczącego stron MVC i Razor).</span><span class="sxs-lookup"><span data-stu-id="0d857-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="0d857-136">A *zasad cross-origin* można określić podczas dodawania przy użyciu oprogramowanie pośredniczące CORS <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> klasy.</span><span class="sxs-lookup"><span data-stu-id="0d857-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="0d857-137">Dostępne są dwie opcje do definiowania zasad CORS:</span><span class="sxs-lookup"><span data-stu-id="0d857-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="0d857-138">Wywołaj `UseCors` za pomocą wyrażenia lambda:</span><span class="sxs-lookup"><span data-stu-id="0d857-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="0d857-139">Wyrażenie lambda ma <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> obiektu.</span><span class="sxs-lookup"><span data-stu-id="0d857-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="0d857-140">[Opcje konfiguracji](#cors-policy-options), takich jak `WithOrigins`, są opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="0d857-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="0d857-141">W powyższym przykładzie zasady umożliwiają żądań cross-origin z `https://example.com` i innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="0d857-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="0d857-142">Należy określić adres URL bez znaku ukośnika na końcu (`/`).</span><span class="sxs-lookup"><span data-stu-id="0d857-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="0d857-143">Jeśli adres URL kończy się za pomocą `/`, zwraca wynik porównania `false` i zostanie zwrócony bez nagłówka.</span><span class="sxs-lookup"><span data-stu-id="0d857-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="0d857-144">`CorsPolicyBuilder` ma interfejs fluent API, dzięki czemu można połączyć w łańcuch wywołań metod:</span><span class="sxs-lookup"><span data-stu-id="0d857-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="0d857-145">Zdefiniuj co najmniej jedne zasady o nazwie CORS i wybierz zasady według nazwy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0d857-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="0d857-146">W poniższym przykładzie dodano zasady CORS zdefiniowanych przez użytkownika o nazwie *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="0d857-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="0d857-147">Aby wybrać zasady, należy przekazać nazwę aby `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="0d857-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="0d857-148">Włączanie mechanizmu CORS na platformie MVC</span><span class="sxs-lookup"><span data-stu-id="0d857-148">Enable CORS in MVC</span></span>

<span data-ttu-id="0d857-149">Można też użyć MVC zastosować określone zasady CORS na akcji lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d857-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="0d857-150">Używanie wzorca MVC, aby włączyć mechanizm CORS, używane są zarejestrowane usługi CORS.</span><span class="sxs-lookup"><span data-stu-id="0d857-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="0d857-151">Oprogramowanie pośredniczące CORS nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="0d857-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="0d857-152">Za akcję</span><span class="sxs-lookup"><span data-stu-id="0d857-152">Per action</span></span>

<span data-ttu-id="0d857-153">Aby określić zasad CORS dla danego działania, Dodaj [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atrybutu akcji.</span><span class="sxs-lookup"><span data-stu-id="0d857-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="0d857-154">Podaj nazwę zasad.</span><span class="sxs-lookup"><span data-stu-id="0d857-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="0d857-155">Każdy kontroler</span><span class="sxs-lookup"><span data-stu-id="0d857-155">Per controller</span></span>

<span data-ttu-id="0d857-156">Aby określić zasady CORS dla określonego kontrolera, należy dodać [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atrybutów do klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d857-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="0d857-157">Podaj nazwę zasad.</span><span class="sxs-lookup"><span data-stu-id="0d857-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="0d857-158">Kolejność pierwszeństwa jest:</span><span class="sxs-lookup"><span data-stu-id="0d857-158">The precedence order is:</span></span>

1. <span data-ttu-id="0d857-159">Akcja</span><span class="sxs-lookup"><span data-stu-id="0d857-159">action</span></span>
1. <span data-ttu-id="0d857-160">kontroler</span><span class="sxs-lookup"><span data-stu-id="0d857-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="0d857-161">Wyłączenia CORS</span><span class="sxs-lookup"><span data-stu-id="0d857-161">Disable CORS</span></span>

<span data-ttu-id="0d857-162">Aby wyłączyć CORS dla kontrolera lub akcji, należy użyć [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="0d857-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="0d857-163">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="0d857-163">CORS policy options</span></span>

<span data-ttu-id="0d857-164">W tej sekcji opisano różne opcje, które można ustawić zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="0d857-164">This section describes the various options that you can set in a CORS policy.</span></span> <span data-ttu-id="0d857-165"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> Metoda jest wywoływana w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0d857-165">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> method is called in `Startup.ConfigureServices`.</span></span>

* [<span data-ttu-id="0d857-166">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="0d857-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="0d857-167">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="0d857-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="0d857-168">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="0d857-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="0d857-169">Ustawianie nagłówków odpowiedzi narażone</span><span class="sxs-lookup"><span data-stu-id="0d857-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="0d857-170">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="0d857-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="0d857-171">Ustaw czas wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="0d857-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="0d857-172">Niektóre opcje mogą być pomocne dla odczytu [działa jak CORS](#how-cors-works) najpierw sekcji.</span><span class="sxs-lookup"><span data-stu-id="0d857-172">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="0d857-173">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="0d857-173">Set the allowed origins</span></span>

<span data-ttu-id="0d857-174">Oprogramowanie pośredniczące CORS w aplikacji ASP.NET Core MVC ma kilka sposobów, aby określić dozwolone źródła:</span><span class="sxs-lookup"><span data-stu-id="0d857-174">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="0d857-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Umożliwia określenie co najmniej jeden adres URL.</span><span class="sxs-lookup"><span data-stu-id="0d857-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Allows specifying one or more URLs.</span></span> <span data-ttu-id="0d857-176">Adres URL może zawierać schemat, nazwę hosta i portu bez żadnych informacji o ścieżce.</span><span class="sxs-lookup"><span data-stu-id="0d857-176">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="0d857-177">Na przykład `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="0d857-177">For example, `https://example.com`.</span></span> <span data-ttu-id="0d857-178">Należy określić adres URL bez znaku ukośnika na końcu (`/`).</span><span class="sxs-lookup"><span data-stu-id="0d857-178">The URL must be specified without a trailing slash (`/`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <span data-ttu-id="0d857-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Umożliwia żądań CORPS ze wszystkich źródeł przy użyciu dowolnego schematu (`http` lub `https`).</span><span class="sxs-lookup"><span data-stu-id="0d857-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  <span data-ttu-id="0d857-180">Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła.</span><span class="sxs-lookup"><span data-stu-id="0d857-180">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="0d857-181">Zezwolenie na żądania z dowolnego źródła oznacza, że *dowolnej witrynie sieci Web* mogą wysyłać żądań cross-origin do swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0d857-181">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="0d857-182">Określanie `AllowAnyOrigin` i `AllowCredentials` jest Konfiguracja niebezpieczne i może spowodować fałszerstwo żądania międzywitrynowego.</span><span class="sxs-lookup"><span data-stu-id="0d857-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="0d857-183">Usługa CORS zwraca nieprawidłową odpowiedź CORS, gdy aplikacja jest skonfigurowana za pomocą obu metod.</span><span class="sxs-lookup"><span data-stu-id="0d857-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="0d857-184">Określanie `AllowAnyOrigin` i `AllowCredentials` jest Konfiguracja niebezpieczne i może spowodować fałszerstwo żądania międzywitrynowego.</span><span class="sxs-lookup"><span data-stu-id="0d857-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="0d857-185">Należy rozważyć Określanie dokładna lista źródeł, jeśli należy autoryzować klienta w samej dostęp do zasobów serwera.</span><span class="sxs-lookup"><span data-stu-id="0d857-185">Consider specifying an exact list of origins if the client must authorize itself to access server resources.</span></span>

  ::: moniker-end

  <span data-ttu-id="0d857-186">To ustawienie dotyczy żądań wstępnych i `Access-Control-Allow-Origin` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="0d857-186">This setting affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="0d857-187">Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.</span><span class="sxs-lookup"><span data-stu-id="0d857-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="0d857-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Zestawy <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> właściwości zasad jako funkcja, która umożliwia źródeł dopasować domeny skonfigurowane ze znakami wieloznacznymi, gdy sprawdzanie, czy jest dozwolone pochodzenie.</span><span class="sxs-lookup"><span data-stu-id="0d857-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcarded domain when evaluating if the origin is allowed.</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="0d857-189">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="0d857-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="0d857-190">Aby zezwolić na wszystkie metody HTTP, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="0d857-190">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

<span data-ttu-id="0d857-191">To ustawienie dotyczy żądań wstępnych i `Access-Control-Allow-Methods` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="0d857-191">This setting affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="0d857-192">Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.</span><span class="sxs-lookup"><span data-stu-id="0d857-192">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="0d857-193">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="0d857-193">Set the allowed request headers</span></span>

<span data-ttu-id="0d857-194">Aby zezwolić na określone nagłówki do wysłania żądania CORS, o nazwie *tworzyć nagłówki żądania*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:</span><span class="sxs-lookup"><span data-stu-id="0d857-194">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="0d857-195">Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="0d857-195">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="0d857-196">To ustawienie dotyczy żądań wstępnych i `Access-Control-Request-Headers` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="0d857-196">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="0d857-197">Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.</span><span class="sxs-lookup"><span data-stu-id="0d857-197">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0d857-198">Oprogramowanie pośredniczące CORS dopasowania zasad do określonych nagłówków, określony przez `WithHeaders` jest możliwe tylko wtedy, gdy nagłówki są wysyłane `Access-Control-Request-Headers` dokładnie pasują do nagłówków w `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="0d857-198">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="0d857-199">Na przykład należy wziąć pod uwagę aplikacji skonfigurowanej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0d857-199">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="0d857-200">Oprogramowanie pośredniczące CORS odmawia żądania wstępnego za pomocą następujących nagłówek żądania, ponieważ `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) nie ma na liście w `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="0d857-200">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="0d857-201">Zwraca aplikacji *200 OK* odpowiedzi, ale nie wysyła ponownie nagłówków CORS.</span><span class="sxs-lookup"><span data-stu-id="0d857-201">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="0d857-202">W związku z tym przeglądarka nie podejmować żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0d857-202">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0d857-203">Oprogramowanie pośredniczące CORS zawsze zezwala na cztery nagłówków w `Access-Control-Request-Headers` mają być wysyłane niezależnie od wartości skonfigurowanych w CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="0d857-203">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="0d857-204">Ta lista nagłówków zawiera:</span><span class="sxs-lookup"><span data-stu-id="0d857-204">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="0d857-205">Na przykład należy wziąć pod uwagę aplikacji skonfigurowanej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0d857-205">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="0d857-206">Oprogramowanie pośredniczące CORS odpowiada pomyślnie żądania wstępnego za pomocą następujących nagłówek żądania ponieważ `Content-Language` zawsze jest umieszczona na białej liście:</span><span class="sxs-lookup"><span data-stu-id="0d857-206">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="0d857-207">Ustawianie nagłówków odpowiedzi narażone</span><span class="sxs-lookup"><span data-stu-id="0d857-207">Set the exposed response headers</span></span>

<span data-ttu-id="0d857-208">Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0d857-208">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="0d857-209">Aby uzyskać więcej informacji, zobacz [W3C Cross-Origin Resource Sharing (terminologii): prosty nagłówek odpowiedzi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="0d857-209">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="0d857-210">Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="0d857-210">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="0d857-211">Specyfikacja CORS wywołuje te nagłówki *nagłówki odpowiedzi proste*.</span><span class="sxs-lookup"><span data-stu-id="0d857-211">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="0d857-212">Aby udostępnić innych nagłówków do aplikacji, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="0d857-212">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="0d857-213">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="0d857-213">Credentials in cross-origin requests</span></span>

<span data-ttu-id="0d857-214">Poświadczenia wymagają specjalnej obsługi żądania CORS.</span><span class="sxs-lookup"><span data-stu-id="0d857-214">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="0d857-215">Domyślnie przeglądarka nie wysyła poświadczenia z żądaniem cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0d857-215">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="0d857-216">Poświadczenia obejmują pliki cookie i schematów uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="0d857-216">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="0d857-217">Do wysyłania poświadczeń z żądaniem cross-origin, klient musi być ustawiona `XMLHttpRequest.withCredentials` do `true`.</span><span class="sxs-lookup"><span data-stu-id="0d857-217">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="0d857-218">Za pomocą `XMLHttpRequest` bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="0d857-218">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="0d857-219">W technologii jQuery:</span><span class="sxs-lookup"><span data-stu-id="0d857-219">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="0d857-220">Ponadto serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="0d857-220">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="0d857-221">Zezwalaj na współużytkowanie poświadczeń, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="0d857-221">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="0d857-222">Odpowiedź HTTP zawiera `Access-Control-Allow-Credentials` nagłówka, który informuje przeglądarkę o tym, że serwer pozwala poświadczenia dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0d857-222">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="0d857-223">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowej `Access-Control-Allow-Credentials` nagłówka, przeglądarka nie ujawnia odpowiedzi do aplikacji, a liczba nieudanych żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0d857-223">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="0d857-224">Należy zachować ostrożność, dzięki czemu poświadczenia cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0d857-224">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="0d857-225">Witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0d857-225">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="0d857-226">Specyfikacja CORS również stany to ustawienie źródeł do `"*"` (wszystkie źródła) jest nieprawidłowa Jeśli `Access-Control-Allow-Credentials` nagłówka znajduje się.</span><span class="sxs-lookup"><span data-stu-id="0d857-226">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="0d857-227">Żądania wstępnego</span><span class="sxs-lookup"><span data-stu-id="0d857-227">Preflight requests</span></span>

<span data-ttu-id="0d857-228">Dla niektórych żądań CORPS przeglądarce wysyła żądanie dodatkowe przed wprowadzeniem rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d857-228">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="0d857-229">To żądanie jest nazywany *żądania wstępnego*.</span><span class="sxs-lookup"><span data-stu-id="0d857-229">This request is called a *preflight request*.</span></span> <span data-ttu-id="0d857-230">Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="0d857-230">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="0d857-231">Metoda żądania jest GET, HEAD lub POST.</span><span class="sxs-lookup"><span data-stu-id="0d857-231">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="0d857-232">Aplikacja nie ustawia nagłówki żądania innego niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, lub `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="0d857-232">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="0d857-233">`Content-Type` Nagłówka, jeśli ustawiona, ma jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="0d857-233">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="0d857-234">Reguła w nagłówkach żądania skonfigurowana dla żądania klienta, który ma zastosowanie do nagłówków, które aplikacja ustawia przez wywołanie metody `setRequestHeader` na `XMLHttpRequest` obiektu.</span><span class="sxs-lookup"><span data-stu-id="0d857-234">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="0d857-235">Specyfikacja CORS wywołuje te nagłówki *tworzyć nagłówki żądania*.</span><span class="sxs-lookup"><span data-stu-id="0d857-235">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="0d857-236">Reguła nie ma zastosowania do przeglądarki można ustawić, takie jak nagłówki `User-Agent`, `Host`, lub `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="0d857-236">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="0d857-237">Oto przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="0d857-237">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="0d857-238">Żądanie krótkiej wykorzystuje metodę OPTIONS protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="0d857-238">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="0d857-239">Zawiera ona dwie specjalnych nagłówków:</span><span class="sxs-lookup"><span data-stu-id="0d857-239">It includes two special headers:</span></span>

* <span data-ttu-id="0d857-240">`Access-Control-Request-Method`: Metoda HTTP, która będzie służyć do rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d857-240">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="0d857-241">`Access-Control-Request-Headers`: Lista nagłówków żądań, które aplikacja ustawia rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d857-241">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="0d857-242">Jak wspomniano wcześniej, to nie zawiera nagłówki, które ustawia przeglądarki, takich jak `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="0d857-242">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="0d857-243">Może obejmować żądania wstępnego CORS `Access-Control-Request-Headers` nagłówka, który wskazuje na serwerze, nagłówki, które są wysyłane przy użyciu rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d857-243">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="0d857-244">Aby zezwolić na określone nagłówki, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="0d857-244">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="0d857-245">Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="0d857-245">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="0d857-246">Przeglądarki nie są w pełni zgodne, w jaki sposób ustawić `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="0d857-246">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="0d857-247">Jeśli ustawisz nagłówki do żadnego elementu innego niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), powinien obejmować co najmniej `Accept`, `Content-Type`, i `Origin`, oraz niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="0d857-247">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="0d857-248">Poniżej zamieszczono przykładową odpowiedź do żądania wstępnego (przy założeniu, że serwer zezwala na żądanie):</span><span class="sxs-lookup"><span data-stu-id="0d857-248">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="0d857-249">Odpowiedź zawiera `Access-Control-Allow-Methods` nagłówek, który zawiera listę dozwolonych metod i opcjonalnie `Access-Control-Allow-Headers` nagłówka, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="0d857-249">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="0d857-250">Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d857-250">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="0d857-251">W przypadku odmowy żądania wstępnego aplikacja zwróci *200 OK* odpowiedzi, ale nie wysyła ponownie nagłówków CORS.</span><span class="sxs-lookup"><span data-stu-id="0d857-251">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="0d857-252">W związku z tym przeglądarka nie podejmować żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0d857-252">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="0d857-253">Ustaw czas wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="0d857-253">Set the preflight expiration time</span></span>

<span data-ttu-id="0d857-254">`Access-Control-Max-Age` Nagłówek Określa, jak długo mogą być buforowane odpowiedzi na żądania wstępnego.</span><span class="sxs-lookup"><span data-stu-id="0d857-254">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="0d857-255">Aby ustawić tego pliku nagłówkowego, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="0d857-255">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="0d857-256">Jak działa mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="0d857-256">How CORS works</span></span>

<span data-ttu-id="0d857-257">W tej sekcji opisano, co się stanie, w ramach żądania CORS na poziomie wiadomości HTTP.</span><span class="sxs-lookup"><span data-stu-id="0d857-257">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="0d857-258">Należy zrozumieć, jak CORS działa tak, aby zasady CORS może być prawidłowo skonfigurowane i debugowania, gdy wystąpią nieoczekiwane wyniki.</span><span class="sxs-lookup"><span data-stu-id="0d857-258">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="0d857-259">Specyfikacja CORS wprowadza kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0d857-259">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="0d857-260">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0d857-260">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="0d857-261">Niestandardowy kod JavaScript nie jest wymagane, aby włączyć mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="0d857-261">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="0d857-262">Oto przykład żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="0d857-262">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="0d857-263">`Origin` Nagłówek zapewnia domeny obiektu, z której wysłano żądanie:</span><span class="sxs-lookup"><span data-stu-id="0d857-263">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="0d857-264">Jeśli serwer zezwala na żądanie, ustawia `Access-Control-Allow-Origin` nagłówka w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0d857-264">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="0d857-265">Wartość tego nagłówka stanowi albo odpowiada `Origin` nagłówek z żądania lub wartość symbolu wieloznacznego `"*"`, co oznacza, że każde źródło jest dozwolony:</span><span class="sxs-lookup"><span data-stu-id="0d857-265">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="0d857-266">Jeżeli odpowiedź nie zawiera `Access-Control-Allow-Origin` nagłówka żądania między źródłami zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="0d857-266">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="0d857-267">W szczególności przeglądarki nie zezwalają na żądanie.</span><span class="sxs-lookup"><span data-stu-id="0d857-267">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="0d857-268">Nawet jeśli serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi do aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="0d857-268">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d857-269">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0d857-269">Additional resources</span></span>

* [<span data-ttu-id="0d857-270">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="0d857-270">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
