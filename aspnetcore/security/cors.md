---
title: Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak CORS jako standardowe rozwiązanie dla zezwolenie lub odrzucenie żądań cross-origin w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 6be8b4da1642a9eff021371c229a17071d6e9bfb
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978474"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="8b61e-103">Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b61e-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="8b61e-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8b61e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8b61e-105">W tym artykule pokazano, jak włączanie mechanizmu CORS w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b61e-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="8b61e-106">Poziom zabezpieczeń przeglądarki zapobiega wysyłania żądań do innej domeny niż ta, która obsłużyła stronę sieci web strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="8b61e-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="8b61e-107">To ograniczenie jest nazywany *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="8b61e-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="8b61e-108">Zasada tego samego źródła uniemożliwia złośliwych witryn odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="8b61e-109">Czasami możesz chcieć zezwala na innych witryn wprowadzać żądań cross-origin Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="8b61e-110">Aby uzyskać więcej informacji, zobacz [artykułu Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="8b61e-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="8b61e-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (między źródłami CORS):</span><span class="sxs-lookup"><span data-stu-id="8b61e-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="8b61e-112">Jest W3C w warstwie standardowa, dzięki któremu serwer może Poluzować zasady tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="8b61e-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="8b61e-113">Jest **nie** to funkcja zabezpieczeń CORS dotyczących zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8b61e-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="8b61e-114">Interfejs API nie jest bezpieczniejsze, umożliwiając mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="8b61e-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="8b61e-115">Aby uzyskać więcej informacji, zobacz [działa jak CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="8b61e-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="8b61e-116">Zezwala serwerowi jawnie zezwolić na niektóre żądania między źródłami, jednocześnie odrzucając inne.</span><span class="sxs-lookup"><span data-stu-id="8b61e-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="8b61e-117">Jest bezpieczniejsze i bardziej elastyczne niż wcześniej technik, takich jak [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="8b61e-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="8b61e-118">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8b61e-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="8b61e-119">Tego samego źródła</span><span class="sxs-lookup"><span data-stu-id="8b61e-119">Same origin</span></span>

<span data-ttu-id="8b61e-120">Dwa adresy URL mają tego samego źródła, jeśli mają one hostów, porty i schematy identyczne ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="8b61e-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="8b61e-121">Te dwa adresy URL są tego samego źródła:</span><span class="sxs-lookup"><span data-stu-id="8b61e-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="8b61e-122">Te adresy URL są różne źródła niż poprzednie dwa adresy URL:</span><span class="sxs-lookup"><span data-stu-id="8b61e-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="8b61e-123">`https://example.net` &ndash; Inną domenę</span><span class="sxs-lookup"><span data-stu-id="8b61e-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="8b61e-124">`https://www.example.com/foo.html` &ndash; Różne domeny podrzędnej</span><span class="sxs-lookup"><span data-stu-id="8b61e-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="8b61e-125">`http://example.com/foo.html` &ndash; Inny schemat</span><span class="sxs-lookup"><span data-stu-id="8b61e-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="8b61e-126">`https://example.com:9000/foo.html` &ndash; Inny port</span><span class="sxs-lookup"><span data-stu-id="8b61e-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="8b61e-127">Program Internet Explorer nie bierze pod uwagę portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="8b61e-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="8b61e-128">Mechanizm CORS za pomocą nazwanych zasad i oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="8b61e-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="8b61e-129">Oprogramowanie pośredniczące CORS obsługuje żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="8b61e-130">Poniższy kod obsługuje mechanizm CORS dla całej aplikacji z określonej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8b61e-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="8b61e-131">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="8b61e-131">The preceding code:</span></span>

* <span data-ttu-id="8b61e-132">Ustawia nazwę zasady "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="8b61e-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="8b61e-133">Nazwa zasad jest dowolnego.</span><span class="sxs-lookup"><span data-stu-id="8b61e-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="8b61e-134">Wywołania <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metodę rozszerzenia, która umożliwia rdzeni.</span><span class="sxs-lookup"><span data-stu-id="8b61e-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables cores.</span></span>
* <span data-ttu-id="8b61e-135">Wywołania <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> z [wyrażenia lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="8b61e-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="8b61e-136">Wyrażenie lambda ma <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> obiektu.</span><span class="sxs-lookup"><span data-stu-id="8b61e-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="8b61e-137">[Opcje konfiguracji](#cors-policy-options), takich jak `WithOrigins`, są opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="8b61e-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="8b61e-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> Wywołania metody dodaje mechanizmu CORS usługi do aplikacji kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="8b61e-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="8b61e-139">Aby uzyskać więcej informacji, zobacz [opcje zasad CORS](#cpo) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="8b61e-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="8b61e-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Metoda można połączyć w łańcuch metod, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="8b61e-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="8b61e-141">Następujący wyróżniony kod stosuje zasady CORS do wszystkich aplikacji punktów końcowych, które przez oprogramowanie pośredniczące CORS:</span><span class="sxs-lookup"><span data-stu-id="8b61e-141">The following highlighted code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors(); 

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

<span data-ttu-id="8b61e-142">Zobacz [Włączanie mechanizmu CORS w stron Razor, kontrolerów i metod akcji](#ecors) zastosować zasady CORS na poziomie akcji/kontrolera/strony.</span><span class="sxs-lookup"><span data-stu-id="8b61e-142">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="8b61e-143">Uwaga:</span><span class="sxs-lookup"><span data-stu-id="8b61e-143">Note:</span></span>

* <span data-ttu-id="8b61e-144">`UseCors` musi zostać wywołana przed `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="8b61e-144">`UseCors` must be called before `UseMvc`.</span></span>
* <span data-ttu-id="8b61e-145">Adres URL powinien **nie** zawierać ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="8b61e-145">The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="8b61e-146">Jeśli adres URL kończy się za pomocą `/`, zwraca wynik porównania `false` i zostanie zwrócony bez nagłówka.</span><span class="sxs-lookup"><span data-stu-id="8b61e-146">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="8b61e-147">Zobacz [CORS testu](#test) instrukcje dotyczące badania w poprzednim kodzie.</span><span class="sxs-lookup"><span data-stu-id="8b61e-147">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="8b61e-148">Włączanie mechanizmu CORS za pomocą atrybutów</span><span class="sxs-lookup"><span data-stu-id="8b61e-148">Enable CORS with attributes</span></span>

<span data-ttu-id="8b61e-149">[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atrybut stanowi alternatywę dla globalnie stosowania mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="8b61e-149">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="8b61e-150">`[EnableCors]` Atrybutu obsługuje mechanizm CORS dla wybranych punktów końcowych, a nie dla wszystkich punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="8b61e-150">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="8b61e-151">Użyj `[EnableCors]` określić domyślne zasady i `[EnableCors("{Policy String}")]` można określić zasady.</span><span class="sxs-lookup"><span data-stu-id="8b61e-151">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="8b61e-152">`[EnableCors]` Atrybut można stosować do:</span><span class="sxs-lookup"><span data-stu-id="8b61e-152">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="8b61e-153">Strona razor `PageModel`</span><span class="sxs-lookup"><span data-stu-id="8b61e-153">Razor Page `PageModel`</span></span>
* <span data-ttu-id="8b61e-154">Kontroler</span><span class="sxs-lookup"><span data-stu-id="8b61e-154">Controller</span></span>
* <span data-ttu-id="8b61e-155">Metoda akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="8b61e-155">Controller action method</span></span>

<span data-ttu-id="8b61e-156">Różne zasady można stosować do kontrolera/strony-/ Akcja modelu przy użyciu `[EnableCors]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8b61e-156">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="8b61e-157">Gdy `[EnableCors]` atrybut jest stosowany do metody kontrolerów/strony-/ Akcja modelu i mechanizmu CORS jest włączona w oprogramowaniu pośredniczącym, obie zasady są stosowane.</span><span class="sxs-lookup"><span data-stu-id="8b61e-157">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="8b61e-158">Odradza się łączenie zasad.</span><span class="sxs-lookup"><span data-stu-id="8b61e-158">We recommend against combining policies.</span></span> <span data-ttu-id="8b61e-159">Użyj `[EnableCors]` atrybutu lub oprogramowanie pośredniczące nie oba w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-159">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="8b61e-160">Poniższy kod mają zastosowanie inne zasady do każdej metody:</span><span class="sxs-lookup"><span data-stu-id="8b61e-160">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="8b61e-161">Poniższy kod tworzy domyślne zasady mechanizmu CORS i zasad o nazwie `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="8b61e-161">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="8b61e-162">Wyłączenia CORS</span><span class="sxs-lookup"><span data-stu-id="8b61e-162">Disable CORS</span></span>

<span data-ttu-id="8b61e-163">[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atrybut wyłącza mechanizm CORS dla/strony — model/akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8b61e-163">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="8b61e-164">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="8b61e-164">CORS policy options</span></span>

<span data-ttu-id="8b61e-165">W tej sekcji opisano różne opcje, które można ustawić zasady CORS:</span><span class="sxs-lookup"><span data-stu-id="8b61e-165">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="8b61e-166">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="8b61e-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="8b61e-167">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="8b61e-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="8b61e-168">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="8b61e-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="8b61e-169">Ustawianie nagłówków odpowiedzi narażone</span><span class="sxs-lookup"><span data-stu-id="8b61e-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="8b61e-170">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="8b61e-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="8b61e-171">Ustaw czas wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="8b61e-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

 <span data-ttu-id="8b61e-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> jest wywoływana w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8b61e-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8b61e-173">Niektóre opcje mogą być pomocne dla odczytu [działa jak CORS](#how-cors) najpierw sekcji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-173">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="8b61e-174">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="8b61e-174">Set the allowed origins</span></span>

<span data-ttu-id="8b61e-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Umożliwia żądań CORPS ze wszystkich źródeł przy użyciu dowolnego schematu (`http` lub `https`).</span><span class="sxs-lookup"><span data-stu-id="8b61e-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="8b61e-176">`AllowAnyOrigin` jest niebezpieczne ponieważ *dowolnej witrynie sieci Web* mogą wysyłać żądań cross-origin do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-176">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="8b61e-177">Określanie `AllowAnyOrigin` i `AllowCredentials` jest Konfiguracja niebezpieczne i może spowodować fałszerstwo żądania międzywitrynowego.</span><span class="sxs-lookup"><span data-stu-id="8b61e-177">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="8b61e-178">Usługa CORS zwraca nieprawidłową odpowiedź CORS, gdy aplikacja jest skonfigurowana za pomocą obu metod.</span><span class="sxs-lookup"><span data-stu-id="8b61e-178">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="8b61e-179">Określanie `AllowAnyOrigin` i `AllowCredentials` jest Konfiguracja niebezpieczne i może spowodować fałszerstwo żądania międzywitrynowego.</span><span class="sxs-lookup"><span data-stu-id="8b61e-179">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="8b61e-180">Dla bezpiecznych aplikacji należy określić dokładnej listy źródeł, jeśli należy autoryzować klienta w samej dostęp do zasobów serwera.</span><span class="sxs-lookup"><span data-stu-id="8b61e-180">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

  ::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**. 
-->

  <span data-ttu-id="8b61e-181">`AllowAnyOrigin` wpływa na stanu wstępnego żądania i `Access-Control-Allow-Origin` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="8b61e-181">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="8b61e-182">Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-182">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8b61e-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Zestawy <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> właściwości zasad jako funkcja, która umożliwia źródeł dopasować domeny z symbolami wieloznacznymi skonfigurowane podczas obliczania, jeśli źródło jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="8b61e-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="8b61e-184">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="8b61e-184">Set the allowed HTTP methods</span></span>

<span data-ttu-id="8b61e-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="8b61e-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="8b61e-186">Umożliwia dowolną metodę HTTP:</span><span class="sxs-lookup"><span data-stu-id="8b61e-186">Allows any HTTP method:</span></span>
* <span data-ttu-id="8b61e-187">Wpływa na stanu wstępnego żądania i `Access-Control-Allow-Methods` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="8b61e-187">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="8b61e-188">Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="8b61e-189">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="8b61e-189">Set the allowed request headers</span></span>

<span data-ttu-id="8b61e-190">Aby zezwolić na określone nagłówki do wysłania żądania CORS, o nazwie *tworzyć nagłówki żądania*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:</span><span class="sxs-lookup"><span data-stu-id="8b61e-190">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="8b61e-191">Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="8b61e-191">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="8b61e-192">To ustawienie dotyczy żądań wstępnych i `Access-Control-Request-Headers` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="8b61e-192">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="8b61e-193">Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8b61e-194">Oprogramowanie pośredniczące CORS dopasowania zasad do określonych nagłówków, określony przez `WithHeaders` jest możliwe tylko wtedy, gdy nagłówki są wysyłane `Access-Control-Request-Headers` dokładnie pasują do nagłówków w `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="8b61e-194">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="8b61e-195">Na przykład należy wziąć pod uwagę aplikacji skonfigurowanej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8b61e-195">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="8b61e-196">Oprogramowanie pośredniczące CORS odmawia żądania wstępnego za pomocą następujących nagłówek żądania, ponieważ `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) nie ma na liście w `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="8b61e-196">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="8b61e-197">Zwraca aplikacji *200 OK* odpowiedzi, ale nie wysyła ponownie nagłówków CORS.</span><span class="sxs-lookup"><span data-stu-id="8b61e-197">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="8b61e-198">W związku z tym przeglądarka nie podejmować żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-198">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8b61e-199">Oprogramowanie pośredniczące CORS zawsze zezwala na cztery nagłówków w `Access-Control-Request-Headers` mają być wysyłane niezależnie od wartości skonfigurowanych w CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="8b61e-199">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="8b61e-200">Ta lista nagłówków zawiera:</span><span class="sxs-lookup"><span data-stu-id="8b61e-200">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="8b61e-201">Na przykład należy wziąć pod uwagę aplikacji skonfigurowanej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8b61e-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="8b61e-202">Oprogramowanie pośredniczące CORS odpowiada pomyślnie żądania wstępnego za pomocą następujących nagłówek żądania ponieważ `Content-Language` zawsze jest umieszczona na białej liście:</span><span class="sxs-lookup"><span data-stu-id="8b61e-202">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="8b61e-203">Ustawianie nagłówków odpowiedzi narażone</span><span class="sxs-lookup"><span data-stu-id="8b61e-203">Set the exposed response headers</span></span>

<span data-ttu-id="8b61e-204">Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-204">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="8b61e-205">Aby uzyskać więcej informacji, zobacz [W3C Cross-Origin Resource Sharing (terminologii): Nagłówek odpowiedzi proste](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="8b61e-205">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="8b61e-206">Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="8b61e-206">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="8b61e-207">Specyfikacja CORS wywołuje te nagłówki *nagłówki odpowiedzi proste*.</span><span class="sxs-lookup"><span data-stu-id="8b61e-207">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="8b61e-208">Aby udostępnić innych nagłówków do aplikacji, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="8b61e-208">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="8b61e-209">Poświadczenia w żądań cross-origin</span><span class="sxs-lookup"><span data-stu-id="8b61e-209">Credentials in cross-origin requests</span></span>

<span data-ttu-id="8b61e-210">Poświadczenia wymagają specjalnej obsługi żądania CORS.</span><span class="sxs-lookup"><span data-stu-id="8b61e-210">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="8b61e-211">Domyślnie przeglądarka nie wysyła poświadczenia z żądaniem cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-211">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="8b61e-212">Poświadczenia obejmują pliki cookie i schematów uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b61e-212">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="8b61e-213">Do wysyłania poświadczeń z żądaniem cross-origin, klient musi być ustawiona `XMLHttpRequest.withCredentials` do `true`.</span><span class="sxs-lookup"><span data-stu-id="8b61e-213">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="8b61e-214">Za pomocą `XMLHttpRequest` bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="8b61e-214">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="8b61e-215">Przy użyciu jQuery:</span><span class="sxs-lookup"><span data-stu-id="8b61e-215">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="8b61e-216">Za pomocą [interfejsu API Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="8b61e-216">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="8b61e-217">Serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="8b61e-217">The server must allow the credentials.</span></span> <span data-ttu-id="8b61e-218">Zezwalaj na współużytkowanie poświadczeń, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="8b61e-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="8b61e-219">Odpowiedź HTTP zawiera `Access-Control-Allow-Credentials` nagłówka, który informuje przeglądarkę o tym, że serwer pozwala poświadczenia dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="8b61e-220">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowej `Access-Control-Allow-Credentials` nagłówka, przeglądarka nie ujawnia odpowiedzi do aplikacji, a liczba nieudanych żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="8b61e-221">Dzięki czemu poświadczenia cross-origin to zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="8b61e-221">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="8b61e-222">Witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8b61e-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="8b61e-223">Specyfikacja CORS również stany to ustawienie źródeł do `"*"` (wszystkie źródła) jest nieprawidłowa Jeśli `Access-Control-Allow-Credentials` nagłówka znajduje się.</span><span class="sxs-lookup"><span data-stu-id="8b61e-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="8b61e-224">Żądania wstępnego</span><span class="sxs-lookup"><span data-stu-id="8b61e-224">Preflight requests</span></span>

<span data-ttu-id="8b61e-225">Dla niektórych żądań CORPS przeglądarce wysyła żądanie dodatkowe przed wprowadzeniem rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="8b61e-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="8b61e-226">To żądanie jest nazywany *żądania wstępnego*.</span><span class="sxs-lookup"><span data-stu-id="8b61e-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="8b61e-227">Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="8b61e-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="8b61e-228">Metoda żądania jest GET, HEAD lub POST.</span><span class="sxs-lookup"><span data-stu-id="8b61e-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="8b61e-229">Aplikacja nie ustawia nagłówki żądania innego niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, lub `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="8b61e-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="8b61e-230">`Content-Type` Nagłówka, jeśli ustawiona, ma jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="8b61e-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="8b61e-231">Reguła w nagłówkach żądania skonfigurowana dla żądania klienta, który ma zastosowanie do nagłówków, które aplikacja ustawia przez wywołanie metody `setRequestHeader` na `XMLHttpRequest` obiektu.</span><span class="sxs-lookup"><span data-stu-id="8b61e-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="8b61e-232">Specyfikacja CORS wywołuje te nagłówki *tworzyć nagłówki żądania*.</span><span class="sxs-lookup"><span data-stu-id="8b61e-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="8b61e-233">Reguła nie ma zastosowania do przeglądarki można ustawić, takie jak nagłówki `User-Agent`, `Host`, lub `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="8b61e-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="8b61e-234">Oto przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="8b61e-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="8b61e-235">Żądanie krótkiej wykorzystuje metodę OPTIONS protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b61e-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="8b61e-236">Zawiera ona dwie specjalnych nagłówków:</span><span class="sxs-lookup"><span data-stu-id="8b61e-236">It includes two special headers:</span></span>

* <span data-ttu-id="8b61e-237">`Access-Control-Request-Method`: Metoda HTTP, która będzie służyć do rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="8b61e-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="8b61e-238">`Access-Control-Request-Headers`: Lista nagłówków żądań, które aplikacja ustawia rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="8b61e-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="8b61e-239">Jak wspomniano wcześniej, to nie zawiera nagłówki, które ustawia przeglądarki, takich jak `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="8b61e-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="8b61e-240">Może obejmować żądania wstępnego CORS `Access-Control-Request-Headers` nagłówka, który wskazuje na serwerze, nagłówki, które są wysyłane przy użyciu rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="8b61e-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="8b61e-241">Aby zezwolić na określone nagłówki, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="8b61e-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="8b61e-242">Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="8b61e-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="8b61e-243">Przeglądarki nie są w pełni zgodne, w jaki sposób ustawić `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="8b61e-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="8b61e-244">Jeśli ustawisz nagłówki do żadnego elementu innego niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), powinien obejmować co najmniej `Accept`, `Content-Type`, i `Origin`, oraz niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="8b61e-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="8b61e-245">Poniżej zamieszczono przykładową odpowiedź do żądania wstępnego (przy założeniu, że serwer zezwala na żądanie):</span><span class="sxs-lookup"><span data-stu-id="8b61e-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="8b61e-246">Odpowiedź zawiera `Access-Control-Allow-Methods` nagłówek, który zawiera listę dozwolonych metod i opcjonalnie `Access-Control-Allow-Headers` nagłówka, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="8b61e-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="8b61e-247">Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="8b61e-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="8b61e-248">W przypadku odmowy żądania wstępnego aplikacja zwróci *200 OK* odpowiedzi, ale nie wysyła ponownie nagłówków CORS.</span><span class="sxs-lookup"><span data-stu-id="8b61e-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="8b61e-249">W związku z tym przeglądarka nie podejmować żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="8b61e-250">Ustaw czas wygaśnięcia wstępnego</span><span class="sxs-lookup"><span data-stu-id="8b61e-250">Set the preflight expiration time</span></span>

<span data-ttu-id="8b61e-251">`Access-Control-Max-Age` Nagłówek Określa, jak długo mogą być buforowane odpowiedzi na żądania wstępnego.</span><span class="sxs-lookup"><span data-stu-id="8b61e-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="8b61e-252">Aby ustawić tego pliku nagłówkowego, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="8b61e-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="8b61e-253">Jak działa mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="8b61e-253">How CORS works</span></span>

<span data-ttu-id="8b61e-254">W tej sekcji opisano, co dzieje się w [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) żądania na poziomie wiadomości HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b61e-254">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="8b61e-255">CORS to **nie** to funkcja zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8b61e-255">CORS is **not** a security feature.</span></span> <span data-ttu-id="8b61e-256">CORS to W3C standard, dzięki któremu serwer może Poluzować zasady tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="8b61e-256">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="8b61e-257">Na przykład użyć złośliwego aktora [zapobiec Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) względem lokacji, a następnie wykonaj fałszerstwo żądania do swojej lokacji włączono mechanizm CORS do wykradania informacji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-257">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="8b61e-258">Interfejs API nie jest bezpieczniejsze, umożliwiając mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="8b61e-258">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="8b61e-259">Wszystko zależy klienta (przeglądarki) wymuszenie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="8b61e-259">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="8b61e-260">Serwer ten wykonuje żądanie i zwraca odpowiedź okazał się klient, który zwraca błąd i bloków odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="8b61e-260">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="8b61e-261">Na przykład dowolne z następujących narzędzi spowoduje wyświetlenie odpowiedź serwera:</span><span class="sxs-lookup"><span data-stu-id="8b61e-261">For example, any of the following tools will display the server response:</span></span>
     * [<span data-ttu-id="8b61e-262">Fiddler</span><span class="sxs-lookup"><span data-stu-id="8b61e-262">Fiddler</span></span>](https://www.telerik.com/fiddler)
     * [<span data-ttu-id="8b61e-263">Postman</span><span class="sxs-lookup"><span data-stu-id="8b61e-263">Postman</span></span>](https://www.getpostman.com/)
     * [<span data-ttu-id="8b61e-264">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="8b61e-264">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
     * <span data-ttu-id="8b61e-265">Przeglądarki sieci web przy użyciu adresu URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8b61e-265">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="8b61e-266">Jest to sposób wykonania cross-origin dla serwera umożliwić przeglądarki [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) lub [pobrania interfejsu API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) żądania, w przeciwnym razie mogłyby być zabroniony.</span><span class="sxs-lookup"><span data-stu-id="8b61e-266">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="8b61e-267">Przeglądarek (CORS) nie można wykonać żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-267">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="8b61e-268">Przed mechanizmu CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) został użyty na obejście tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="8b61e-268">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="8b61e-269">Jest używany inny JSONP XHR, używa ona `<script>` tag do odbierania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8b61e-269">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="8b61e-270">Skrypty mogą zostać załadowane cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-270">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="8b61e-271">[Specyfikacji CORS]() wprowadzono kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-271">The [CORS specification]() introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="8b61e-272">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="8b61e-272">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="8b61e-273">Niestandardowy kod JavaScript nie jest wymagane, aby włączyć mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="8b61e-273">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="8b61e-274">Oto przykład żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="8b61e-274">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="8b61e-275">`Origin` Nagłówek zapewnia domeny obiektu, z której wysłano żądanie:</span><span class="sxs-lookup"><span data-stu-id="8b61e-275">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="8b61e-276">Jeśli serwer zezwala na żądanie, ustawia `Access-Control-Allow-Origin` nagłówka w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8b61e-276">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="8b61e-277">Wartość tego nagłówka stanowi albo odpowiada `Origin` nagłówek z żądania lub wartość symbolu wieloznacznego `"*"`, co oznacza, że każde źródło jest dozwolony:</span><span class="sxs-lookup"><span data-stu-id="8b61e-277">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="8b61e-278">Jeżeli odpowiedź nie zawiera `Access-Control-Allow-Origin` nagłówka żądania między źródłami zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="8b61e-278">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="8b61e-279">W szczególności przeglądarki nie zezwalają na żądanie.</span><span class="sxs-lookup"><span data-stu-id="8b61e-279">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="8b61e-280">Nawet jeśli serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi do aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="8b61e-280">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="8b61e-281">Test CORS</span><span class="sxs-lookup"><span data-stu-id="8b61e-281">Test CORS</span></span>

<span data-ttu-id="8b61e-282">Aby przetestować CORS:</span><span class="sxs-lookup"><span data-stu-id="8b61e-282">To test CORS:</span></span>

1. <span data-ttu-id="8b61e-283">[Utwórz projekt interfejsu API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="8b61e-283">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="8b61e-284">Możesz też [pobrać przykład](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="8b61e-284">Alternatively, you can [download the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="8b61e-285">Włączanie mechanizmu CORS za pomocą jednej z metod, w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="8b61e-285">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="8b61e-286">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8b61e-286">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]
  
  > [!WARNING]
  > <span data-ttu-id="8b61e-287">`WithOrigins("https://localhost:<port>");` powinna służyć wyłącznie do testowania podobne do przykładowej aplikacji [Pobierz przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="8b61e-287">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="8b61e-288">Tworzenie projektu aplikacji sieci web (stron Razor lub MVC).</span><span class="sxs-lookup"><span data-stu-id="8b61e-288">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="8b61e-289">W przykładzie użyto stron Razor.</span><span class="sxs-lookup"><span data-stu-id="8b61e-289">The sample uses Razor Pages.</span></span> <span data-ttu-id="8b61e-290">W tym samym rozwiązaniu co projekt interfejsu API, można utworzyć aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="8b61e-290">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="8b61e-291">Dodaj następujący wyróżniony kod do *Index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="8b61e-291">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="8b61e-292">W poprzednim kodzie Zastąp `url: 'https://<web app>.azurewebsites.net/api/values/1',` z adresem URL wdrożonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b61e-292">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="8b61e-293">Wdróż projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="8b61e-293">Deploy the API project.</span></span> <span data-ttu-id="8b61e-294">Na przykład [wdrażanie na platformie Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="8b61e-294">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="8b61e-295">Uruchamianie aplikacji stron Razor lub MVC z pulpitu, a następnie kliknij przycisk na **testu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="8b61e-295">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="8b61e-296">Użyj narzędzi F12, aby przejrzeć komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="8b61e-296">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="8b61e-297">Usuń źródła localhost z `WithOrigins` i wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="8b61e-297">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="8b61e-298">Możesz też uruchomić aplikacji klienckiej, używając innego portu.</span><span class="sxs-lookup"><span data-stu-id="8b61e-298">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="8b61e-299">Na przykład uruchomić z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8b61e-299">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="8b61e-300">Testowanie za pomocą aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="8b61e-300">Test with the client app.</span></span> <span data-ttu-id="8b61e-301">Błędy CORS zwróci błąd, ale komunikat o błędzie nie jest dostępna w kodzie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8b61e-301">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="8b61e-302">Karta konsoli w F12 tools do wyświetlony błąd.</span><span class="sxs-lookup"><span data-stu-id="8b61e-302">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="8b61e-303">W zależności od przeglądarki wystąpi błąd (w konsoli narzędzia F12) podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="8b61e-303">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

  * <span data-ttu-id="8b61e-304">Korzystanie z przeglądarki Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="8b61e-304">Using Microsoft Edge:</span></span>

    <span data-ttu-id="8b61e-305">**SEC7120: [CORS] źródło `https://localhost:44375` nie znalazł `https://localhost:44375` w nagłówku odpowiedzi Access-Control-Allow-Origin zasobu cross-origin `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="8b61e-305">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

  * <span data-ttu-id="8b61e-306">Za pomocą przeglądarki Chrome:</span><span class="sxs-lookup"><span data-stu-id="8b61e-306">Using Chrome:</span></span>

    <span data-ttu-id="8b61e-307">**Dostęp do XMLHttpRequest na `https://webapi.azurewebsites.net/api/values/1` pochodzenia `https://localhost:44375` została zablokowana przez zasady CORS: Brak nagłówka "Access-Control-Allow-Origin" jest obecny dla żądanego zasobu.**</span><span class="sxs-lookup"><span data-stu-id="8b61e-307">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b61e-308">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8b61e-308">Additional resources</span></span>

* [<span data-ttu-id="8b61e-309">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="8b61e-309">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
