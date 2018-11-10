---
title: Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak CORS jako standardowe rozwiązanie dla zezwolenie lub odrzucenie żądań cross-origin w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: security/cors
ms.openlocfilehash: 8e5056b448d47d75272e9394b03ce8a58b05a0f4
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191324"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="159b1-103">Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="159b1-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="159b1-104">Przez [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="159b1-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="159b1-105">Poziom zabezpieczeń przeglądarki zapobiega wysyłania żądań do innej domeny niż ta, która obsłużyła stronę sieci web strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="159b1-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="159b1-106">To ograniczenie jest nazywany *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="159b1-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="159b1-107">Zasada tego samego źródła uniemożliwia złośliwych witryn odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="159b1-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="159b1-108">Czasami możesz chcieć zezwala na innych witryn wprowadzać żądań cross-origin Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="159b1-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="159b1-109">[Krzyżowe współużytkowanie zasobów](https://www.w3.org/TR/cors/) (między źródłami CORS) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="159b1-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="159b1-110">Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne.</span><span class="sxs-lookup"><span data-stu-id="159b1-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="159b1-111">CORS to bezpieczniejsze i bardziej elastyczne niż wcześniej technik, takich jak [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="159b1-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="159b1-112">W tym temacie pokazano, jak włączanie mechanizmu CORS w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="159b1-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="159b1-113">Tego samego źródła</span><span class="sxs-lookup"><span data-stu-id="159b1-113">Same origin</span></span>

<span data-ttu-id="159b1-114">Dwa adresy URL mają tego samego źródła, jeśli mają one hostów, porty i schematy identyczne ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="159b1-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="159b1-115">Te dwa adresy URL są tego samego źródła:</span><span class="sxs-lookup"><span data-stu-id="159b1-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="159b1-116">Te adresy URL są różne źródła niż poprzednie dwa adresy URL:</span><span class="sxs-lookup"><span data-stu-id="159b1-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="159b1-117">`https://example.net` &ndash; Inną domenę</span><span class="sxs-lookup"><span data-stu-id="159b1-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="159b1-118">`https://www.example.com/foo.html` &ndash; Różne domeny podrzędnej</span><span class="sxs-lookup"><span data-stu-id="159b1-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="159b1-119">`http://example.com/foo.html` &ndash; Inny schemat</span><span class="sxs-lookup"><span data-stu-id="159b1-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="159b1-120">`https://example.com:9000/foo.html` &ndash; Inny port</span><span class="sxs-lookup"><span data-stu-id="159b1-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="159b1-121">Program Internet Explorer nie bierze pod uwagę portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="159b1-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="159b1-122">Rejestrowanie usługi CORS</span><span class="sxs-lookup"><span data-stu-id="159b1-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="159b1-123">Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="159b1-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="159b1-124">Odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="159b1-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="159b1-125">Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="159b1-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="159b1-126">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> w `Startup.ConfigureServices` nad dodaniem usług CORS do aplikacji kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="159b1-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="159b1-127">Włączanie mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="159b1-127">Enable CORS</span></span>

<span data-ttu-id="159b1-128">Po zarejestrowaniu mechanizmu CORS usługi, użyj jednej z następujących metod Włączanie mechanizmu CORS w aplikacji ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="159b1-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="159b1-129">[Oprogramowanie pośredniczące CORS](#enable-cors-with-cors-middleware) &ndash; zasady CORS do zastosowania globalnie do aplikacji za pomocą oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="159b1-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="159b1-130">[Mechanizm CORS w MVC](#enable-cors-in-mvc) &ndash; zasady CORS do zastosowania na akcji lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="159b1-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="159b1-131">Oprogramowanie pośredniczące CORS nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="159b1-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="159b1-132">Włączanie mechanizmu CORS z oprogramowaniem pośredniczącym CORS</span><span class="sxs-lookup"><span data-stu-id="159b1-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="159b1-133">Oprogramowanie pośredniczące CORS obsługuje żądań cross-origin w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="159b1-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="159b1-134">Aby włączyć oprogramowanie pośredniczące CORS w potoku przetwarzania żądań, należy wywołać <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metody rozszerzenia w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="159b1-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="159b1-135">Oprogramowanie pośredniczące CORS musi poprzedzać żadnych zdefiniowanych punktów końcowych w aplikacji, której chcesz obsługiwać żądań cross-origin (na przykład przed wywołaniem do `UseMvc` dla oprogramowania pośredniczącego stron MVC i Razor).</span><span class="sxs-lookup"><span data-stu-id="159b1-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="159b1-136">A *zasad cross-origin* można określić podczas dodawania przy użyciu oprogramowanie pośredniczące CORS <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> klasy.</span><span class="sxs-lookup"><span data-stu-id="159b1-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="159b1-137">Dostępne są dwie opcje do definiowania zasad CORS:</span><span class="sxs-lookup"><span data-stu-id="159b1-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="159b1-138">Wywołaj `UseCors` za pomocą wyrażenia lambda:</span><span class="sxs-lookup"><span data-stu-id="159b1-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="159b1-139">Wyrażenie lambda ma <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> obiektu.</span><span class="sxs-lookup"><span data-stu-id="159b1-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="159b1-140">[Opcje konfiguracji](#cors-policy-options), takich jak `WithOrigins`, są opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="159b1-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="159b1-141">W powyższym przykładzie zasady umożliwiają żądań cross-origin z `https://example.com` i innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="159b1-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="159b1-142">Należy określić adres URL bez znaku ukośnika na końcu (`/`).</span><span class="sxs-lookup"><span data-stu-id="159b1-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="159b1-143">Jeśli adres URL kończy się za pomocą `/`, zwraca wynik porównania `false` i zostanie zwrócony bez nagłówka.</span><span class="sxs-lookup"><span data-stu-id="159b1-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="159b1-144">`CorsPolicyBuilder` ma interfejs fluent API, dzięki czemu można połączyć w łańcuch wywołań metod:</span><span class="sxs-lookup"><span data-stu-id="159b1-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="159b1-145">Zdefiniuj co najmniej jedne zasady o nazwie CORS i wybierz zasady według nazwy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="159b1-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="159b1-146">W poniższym przykładzie dodano zasady CORS zdefiniowanych przez użytkownika o nazwie *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="159b1-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="159b1-147">Aby wybrać zasady, należy przekazać nazwę aby `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="159b1-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="159b1-148">Włączanie mechanizmu CORS na platformie MVC</span><span class="sxs-lookup"><span data-stu-id="159b1-148">Enable CORS in MVC</span></span>

<span data-ttu-id="159b1-149">Można też użyć MVC zastosować określone zasady CORS na akcji lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="159b1-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="159b1-150">Używanie wzorca MVC, aby włączyć mechanizm CORS, używane są zarejestrowane usługi CORS.</span><span class="sxs-lookup"><span data-stu-id="159b1-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="159b1-151">Oprogramowanie pośredniczące CORS nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="159b1-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="159b1-152">Za akcję</span><span class="sxs-lookup"><span data-stu-id="159b1-152">Per action</span></span>

<span data-ttu-id="159b1-153">Aby określić zasad CORS dla danego działania, Dodaj [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atrybutu akcji.</span><span class="sxs-lookup"><span data-stu-id="159b1-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="159b1-154">Podaj nazwę zasad.</span><span class="sxs-lookup"><span data-stu-id="159b1-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="159b1-155">Każdy kontroler</span><span class="sxs-lookup"><span data-stu-id="159b1-155">Per controller</span></span>

<span data-ttu-id="159b1-156">Aby określić zasady CORS dla określonego kontrolera, należy dodać [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atrybutów do klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="159b1-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="159b1-157">Podaj nazwę zasad.</span><span class="sxs-lookup"><span data-stu-id="159b1-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="159b1-158">Kolejność pierwszeństwa jest:</span><span class="sxs-lookup"><span data-stu-id="159b1-158">The precedence order is:</span></span>

1. <span data-ttu-id="159b1-159">Akcja</span><span class="sxs-lookup"><span data-stu-id="159b1-159">action</span></span>
1. <span data-ttu-id="159b1-160">kontroler</span><span class="sxs-lookup"><span data-stu-id="159b1-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="159b1-161">Wyłączenia CORS</span><span class="sxs-lookup"><span data-stu-id="159b1-161">Disable CORS</span></span>

<span data-ttu-id="159b1-162">Aby wyłączyć CORS dla kontrolera lub akcji, należy użyć [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="159b1-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="159b1-163">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="159b1-163">CORS policy options</span></span>

<span data-ttu-id="159b1-164">W tej sekcji opisano różne opcje, które można ustawić zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="159b1-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="159b1-165">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="159b1-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="159b1-166">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="159b1-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="159b1-167">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="159b1-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="159b1-168">Ustawianie nagłówków odpowiedzi narażone</span><span class="sxs-lookup"><span data-stu-id="159b1-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="159b1-169">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="159b1-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="159b1-170">Ustaw czas wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="159b1-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="159b1-171">Niektóre opcje mogą być pomocne dla odczytu [działa jak CORS](#how-cors-works) najpierw sekcji.</span><span class="sxs-lookup"><span data-stu-id="159b1-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="159b1-172">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="159b1-172">Set the allowed origins</span></span>

<span data-ttu-id="159b1-173">Oprogramowanie pośredniczące CORS w aplikacji ASP.NET Core MVC ma kilka sposobów, aby określić dozwolone źródła:</span><span class="sxs-lookup"><span data-stu-id="159b1-173">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="159b1-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Umożliwia określenie co najmniej jeden adres URL.</span><span class="sxs-lookup"><span data-stu-id="159b1-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Allows specifying one or more URLs.</span></span> <span data-ttu-id="159b1-175">Adres URL może zawierać schemat, nazwę hosta i portu bez żadnych informacji o ścieżce.</span><span class="sxs-lookup"><span data-stu-id="159b1-175">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="159b1-176">Na przykład `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="159b1-176">For example, `https://example.com`.</span></span> <span data-ttu-id="159b1-177">Należy określić adres URL bez znaku ukośnika na końcu (`/`).</span><span class="sxs-lookup"><span data-stu-id="159b1-177">The URL must be specified without a trailing slash (`/`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

* <span data-ttu-id="159b1-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Umożliwia żądań CORPS ze wszystkich źródeł przy użyciu dowolnego schematu (`http` lub `https`).</span><span class="sxs-lookup"><span data-stu-id="159b1-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="159b1-179">Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła.</span><span class="sxs-lookup"><span data-stu-id="159b1-179">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="159b1-180">Zezwolenie na żądania z dowolnego źródła oznacza, że *dowolnej witrynie sieci Web* mogą wysyłać żądań cross-origin do swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="159b1-180">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="159b1-181">Określanie `AllowAnyOrigin` i `AllowCredentials` jest Konfiguracja niebezpieczne i może spowodować fałszerstwo żądania międzywitrynowego.</span><span class="sxs-lookup"><span data-stu-id="159b1-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="159b1-182">Usługi CORS zwraca nieprawidłową odpowiedź CORS, gdy aplikacja jest skonfigurowana za pomocą dwóch.</span><span class="sxs-lookup"><span data-stu-id="159b1-182">The CORS service returns an invalid CORS response when an app is configured with the two.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="159b1-183">Określanie `AllowAnyOrigin` i `AllowCredentials` jest Konfiguracja niebezpieczne i może spowodować fałszerstwo żądania międzywitrynowego.</span><span class="sxs-lookup"><span data-stu-id="159b1-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="159b1-184">Należy rozważyć określenie dokładnej listy źródeł, jeśli Twój klient musi zezwolić na dostęp do zasobów serwera.</span><span class="sxs-lookup"><span data-stu-id="159b1-184">Consider specifying an exact list of origins if your client needs to authorize to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="159b1-185">To ustawienie dotyczy [stanu wstępnego żądania i nagłówka Access-Control-Allow-Origin](#preflight-requests) (opisanych w dalszej części tego tematu).</span><span class="sxs-lookup"><span data-stu-id="159b1-185">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

* <span data-ttu-id="159b1-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> — Umożliwia żądań CORPS z danej domeny podrzędne domeny.</span><span class="sxs-lookup"><span data-stu-id="159b1-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> - Allows CORS requests from any sub-domain of a given domain.</span></span> <span data-ttu-id="159b1-187">System nie może być symbol wieloznaczny.</span><span class="sxs-lookup"><span data-stu-id="159b1-187">The scheme cannot be a wildcard.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=98-104&highlight=4)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="159b1-188">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="159b1-188">Set the allowed HTTP methods</span></span>

<span data-ttu-id="159b1-189">Aby zezwolić na wszystkie metody HTTP, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="159b1-189">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="159b1-190">To ustawienie dotyczy [stanu wstępnego żądania i nagłówka Access-kontroli-Allow-Methods](#preflight-requests) (opisanych w dalszej części tego tematu).</span><span class="sxs-lookup"><span data-stu-id="159b1-190">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="159b1-191">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="159b1-191">Set the allowed request headers</span></span>

<span data-ttu-id="159b1-192">Aby zezwolić na określone nagłówki do wysłania żądania CORS, o nazwie *tworzyć nagłówki żądania*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:</span><span class="sxs-lookup"><span data-stu-id="159b1-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="159b1-193">Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="159b1-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="159b1-194">To ustawienie dotyczy [stanu wstępnego żądania i nagłówka Access-Control-Request-Headers](#preflight-requests) (opisanych w dalszej części tego tematu).</span><span class="sxs-lookup"><span data-stu-id="159b1-194">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="159b1-195">Oprogramowanie pośredniczące CORS dopasowania zasad do określonych nagłówków, określony przez `WithHeaders` jest możliwe tylko wtedy, gdy nagłówki są wysyłane `Access-Control-Request-Headers` dokładnie pasują do nagłówków w `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="159b1-195">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="159b1-196">Na przykład należy wziąć pod uwagę aplikacji skonfigurowanej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="159b1-196">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="159b1-197">Oprogramowanie pośredniczące CORS odmawia żądania wstępnego za pomocą następujących nagłówek żądania, ponieważ `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) nie ma na liście w `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="159b1-197">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="159b1-198">Zwraca aplikacji *200 OK* odpowiedzi, ale nie wysyła ponownie nagłówków CORS.</span><span class="sxs-lookup"><span data-stu-id="159b1-198">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="159b1-199">W związku z tym przeglądarka nie podejmować żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="159b1-199">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="159b1-200">Oprogramowanie pośredniczące CORS zawsze zezwala na cztery nagłówków w `Access-Control-Request-Headers` mają być wysyłane niezależnie od wartości skonfigurowanych w CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="159b1-200">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="159b1-201">Ta lista nagłówków zawiera:</span><span class="sxs-lookup"><span data-stu-id="159b1-201">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="159b1-202">Na przykład należy wziąć pod uwagę aplikacji skonfigurowanej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="159b1-202">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="159b1-203">Oprogramowanie pośredniczące CORS odpowiada pomyślnie żądania wstępnego za pomocą następujących nagłówek żądania ponieważ `Content-Language` zawsze jest umieszczona na białej liście:</span><span class="sxs-lookup"><span data-stu-id="159b1-203">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="159b1-204">Ustawianie nagłówków odpowiedzi narażone</span><span class="sxs-lookup"><span data-stu-id="159b1-204">Set the exposed response headers</span></span>

<span data-ttu-id="159b1-205">Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="159b1-205">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="159b1-206">Aby uzyskać więcej informacji, zobacz [W3C Cross-Origin Resource Sharing (terminologii): prosty nagłówek odpowiedzi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="159b1-206">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="159b1-207">Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="159b1-207">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="159b1-208">Specyfikacja CORS wywołuje te nagłówki *nagłówki odpowiedzi proste*.</span><span class="sxs-lookup"><span data-stu-id="159b1-208">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="159b1-209">Aby udostępnić innych nagłówków do aplikacji, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="159b1-209">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="159b1-210">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="159b1-210">Credentials in cross-origin requests</span></span>

<span data-ttu-id="159b1-211">Poświadczenia wymagają specjalnej obsługi żądania CORS.</span><span class="sxs-lookup"><span data-stu-id="159b1-211">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="159b1-212">Domyślnie przeglądarka nie wysyła poświadczenia z żądaniem cross-origin.</span><span class="sxs-lookup"><span data-stu-id="159b1-212">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="159b1-213">Poświadczenia obejmują pliki cookie i schematów uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="159b1-213">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="159b1-214">Do wysyłania poświadczeń z żądaniem cross-origin, klient musi być ustawiona `XMLHttpRequest.withCredentials` do `true`.</span><span class="sxs-lookup"><span data-stu-id="159b1-214">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="159b1-215">Za pomocą `XMLHttpRequest` bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="159b1-215">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="159b1-216">W technologii jQuery:</span><span class="sxs-lookup"><span data-stu-id="159b1-216">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="159b1-217">Ponadto serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="159b1-217">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="159b1-218">Zezwalaj na współużytkowanie poświadczeń, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="159b1-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="159b1-219">Odpowiedź HTTP zawiera `Access-Control-Allow-Credentials` nagłówka, który informuje przeglądarkę o tym, że serwer pozwala poświadczenia dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="159b1-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="159b1-220">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowej `Access-Control-Allow-Credentials` nagłówka, przeglądarka nie ujawnia odpowiedzi do aplikacji, a liczba nieudanych żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="159b1-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="159b1-221">Należy zachować ostrożność, dzięki czemu poświadczenia cross-origin.</span><span class="sxs-lookup"><span data-stu-id="159b1-221">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="159b1-222">Witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="159b1-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="159b1-223">Specyfikacja CORS również stany to ustawienie źródeł do `"*"` (wszystkie źródła) jest nieprawidłowa Jeśli `Access-Control-Allow-Credentials` nagłówka znajduje się.</span><span class="sxs-lookup"><span data-stu-id="159b1-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="159b1-224">Żądania wstępnego</span><span class="sxs-lookup"><span data-stu-id="159b1-224">Preflight requests</span></span>

<span data-ttu-id="159b1-225">Dla niektórych żądań CORPS przeglądarce wysyła żądanie dodatkowe przed wprowadzeniem rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="159b1-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="159b1-226">To żądanie jest nazywany *żądania wstępnego*.</span><span class="sxs-lookup"><span data-stu-id="159b1-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="159b1-227">Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="159b1-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="159b1-228">Metoda żądania jest GET, HEAD lub POST.</span><span class="sxs-lookup"><span data-stu-id="159b1-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="159b1-229">Aplikacja nie ustawia nagłówki żądania innego niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, lub `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="159b1-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="159b1-230">`Content-Type` Nagłówka, jeśli ustawiona, ma jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="159b1-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="159b1-231">Reguła w nagłówkach żądania skonfigurowana dla żądania klienta, który ma zastosowanie do nagłówków, które aplikacja ustawia przez wywołanie metody `setRequestHeader` na `XMLHttpRequest` obiektu.</span><span class="sxs-lookup"><span data-stu-id="159b1-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="159b1-232">Specyfikacja CORS wywołuje te nagłówki *tworzyć nagłówki żądania*.</span><span class="sxs-lookup"><span data-stu-id="159b1-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="159b1-233">Reguła nie ma zastosowania do przeglądarki można ustawić, takie jak nagłówki `User-Agent`, `Host`, lub `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="159b1-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="159b1-234">Oto przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="159b1-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="159b1-235">Żądanie krótkiej wykorzystuje metodę OPTIONS protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="159b1-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="159b1-236">Zawiera ona dwie specjalnych nagłówków:</span><span class="sxs-lookup"><span data-stu-id="159b1-236">It includes two special headers:</span></span>

* <span data-ttu-id="159b1-237">`Access-Control-Request-Method`: Metoda HTTP, która będzie służyć do rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="159b1-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="159b1-238">`Access-Control-Request-Headers`: Lista nagłówków żądań, które aplikacja ustawia rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="159b1-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="159b1-239">Jak wspomniano wcześniej, to nie zawiera nagłówki, które ustawia przeglądarki, takich jak `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="159b1-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="159b1-240">Może obejmować żądania wstępnego CORS `Access-Control-Request-Headers` nagłówka, który wskazuje na serwerze, nagłówki, które są wysyłane przy użyciu rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="159b1-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="159b1-241">Aby zezwolić na określone nagłówki, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="159b1-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="159b1-242">Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="159b1-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="159b1-243">Przeglądarki nie są w pełni zgodne, w jaki sposób ustawić `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="159b1-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="159b1-244">Jeśli ustawisz nagłówki do żadnego elementu innego niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), powinien obejmować co najmniej `Accept`, `Content-Type`, i `Origin`, oraz niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="159b1-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="159b1-245">Poniżej zamieszczono przykładową odpowiedź do żądania wstępnego (przy założeniu, że serwer zezwala na żądanie):</span><span class="sxs-lookup"><span data-stu-id="159b1-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="159b1-246">Odpowiedź zawiera `Access-Control-Allow-Methods` nagłówek, który zawiera listę dozwolonych metod i opcjonalnie `Access-Control-Allow-Headers` nagłówka, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="159b1-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="159b1-247">Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="159b1-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="159b1-248">W przypadku odmowy żądania wstępnego aplikacja zwróci *200 OK* odpowiedzi, ale nie wysyła ponownie nagłówków CORS.</span><span class="sxs-lookup"><span data-stu-id="159b1-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="159b1-249">W związku z tym przeglądarka nie podejmować żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="159b1-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="159b1-250">Ustaw czas wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="159b1-250">Set the preflight expiration time</span></span>

<span data-ttu-id="159b1-251">`Access-Control-Max-Age` Nagłówek Określa, jak długo mogą być buforowane odpowiedzi na żądania wstępnego.</span><span class="sxs-lookup"><span data-stu-id="159b1-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="159b1-252">Aby ustawić tego pliku nagłówkowego, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="159b1-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="159b1-253">Jak działa mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="159b1-253">How CORS works</span></span>

<span data-ttu-id="159b1-254">W tej sekcji opisano, co się stanie, w ramach żądania CORS na poziomie wiadomości HTTP.</span><span class="sxs-lookup"><span data-stu-id="159b1-254">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="159b1-255">Należy zrozumieć, jak CORS działa tak, aby zasady CORS może być prawidłowo skonfigurowane i debugowania, gdy wystąpią nieoczekiwane wyniki.</span><span class="sxs-lookup"><span data-stu-id="159b1-255">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="159b1-256">Specyfikacja CORS wprowadza kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="159b1-256">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="159b1-257">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="159b1-257">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="159b1-258">Niestandardowy kod JavaScript nie jest wymagane, aby włączyć mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="159b1-258">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="159b1-259">Oto przykład żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="159b1-259">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="159b1-260">`Origin` Nagłówek zapewnia domeny obiektu, z której wysłano żądanie:</span><span class="sxs-lookup"><span data-stu-id="159b1-260">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="159b1-261">Jeśli serwer zezwala na żądanie, ustawia `Access-Control-Allow-Origin` nagłówka w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="159b1-261">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="159b1-262">Wartość tego nagłówka stanowi albo odpowiada `Origin` nagłówek z żądania lub wartość symbolu wieloznacznego `"*"`, co oznacza, że każde źródło jest dozwolony:</span><span class="sxs-lookup"><span data-stu-id="159b1-262">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="159b1-263">Jeżeli odpowiedź nie zawiera `Access-Control-Allow-Origin` nagłówka żądania między źródłami zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="159b1-263">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="159b1-264">W szczególności przeglądarki nie zezwalają na żądanie.</span><span class="sxs-lookup"><span data-stu-id="159b1-264">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="159b1-265">Nawet jeśli serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi do aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="159b1-265">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="159b1-266">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="159b1-266">Additional resources</span></span>

* [<span data-ttu-id="159b1-267">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="159b1-267">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
