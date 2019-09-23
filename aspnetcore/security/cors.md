---
title: Włącz żądania między źródłami (CORS) w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób mechanizm CORS jest standardem umożliwiającym lub odrzucanie żądań między źródłami w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a02b3497684979c1a9e792437f9f1a4c467600f0
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187257"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="8fb16-103">Włącz żądania między źródłami (CORS) w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8fb16-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="8fb16-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8fb16-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8fb16-105">W tym artykule pokazano, jak włączyć funkcję CORS w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8fb16-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="8fb16-106">Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która była obsługiwana przez stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8fb16-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="8fb16-107">To ograniczenie jest nazywane *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="8fb16-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="8fb16-108">Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="8fb16-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="8fb16-109">Czasami możesz chcieć zezwolić innym lokacjom na wykonywanie żądań między źródłami do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8fb16-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="8fb16-110">Aby uzyskać więcej informacji, zobacz [artykuł CORS firmy Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="8fb16-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="8fb16-111">[Udostępnianie zasobów między źródłami](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="8fb16-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="8fb16-112">Jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="8fb16-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="8fb16-113">**Nie** jest funkcją zabezpieczeń, funkcja CORS osłabi zabezpieczenia.</span><span class="sxs-lookup"><span data-stu-id="8fb16-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="8fb16-114">Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="8fb16-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="8fb16-115">Aby uzyskać więcej informacji, zobacz [jak działa mechanizm CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="8fb16-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="8fb16-116">Zezwala serwerowi jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych.</span><span class="sxs-lookup"><span data-stu-id="8fb16-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="8fb16-117">Jest bezpieczniejsze i bardziej elastyczne niż wcześniejsze techniki, takie jak [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="8fb16-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="8fb16-118">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8fb16-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="8fb16-119">To samo źródło</span><span class="sxs-lookup"><span data-stu-id="8fb16-119">Same origin</span></span>

<span data-ttu-id="8fb16-120">Dwa adresy URL mają te same źródła, jeśli mają identyczne schematy, hosty i porty ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="8fb16-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="8fb16-121">Te dwa adresy URL mają te same źródła:</span><span class="sxs-lookup"><span data-stu-id="8fb16-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="8fb16-122">Te adresy URL mają różne źródła niż poprzednie dwa adresy URL:</span><span class="sxs-lookup"><span data-stu-id="8fb16-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="8fb16-123">`https://example.net`&ndash; Inna domena</span><span class="sxs-lookup"><span data-stu-id="8fb16-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="8fb16-124">`https://www.example.com/foo.html`&ndash; Inna poddomena</span><span class="sxs-lookup"><span data-stu-id="8fb16-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="8fb16-125">`http://example.com/foo.html`&ndash; Inny schemat</span><span class="sxs-lookup"><span data-stu-id="8fb16-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="8fb16-126">`https://example.com:9000/foo.html`&ndash; Inny port</span><span class="sxs-lookup"><span data-stu-id="8fb16-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="8fb16-127">Program Internet Explorer nie traktuje portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="8fb16-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="8fb16-128">CORS z nazwanymi zasadami i programem pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="8fb16-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="8fb16-129">Oprogramowanie pośredniczące CORS obsługuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="8fb16-130">Poniższy kod włącza mechanizm CORS dla całej aplikacji z określonym źródłem:</span><span class="sxs-lookup"><span data-stu-id="8fb16-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="8fb16-131">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="8fb16-131">The preceding code:</span></span>

* <span data-ttu-id="8fb16-132">Ustawia nazwę zasad na "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="8fb16-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="8fb16-133">Nazwa zasad jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="8fb16-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="8fb16-134">Wywołuje metodę <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> rozszerzenia, która umożliwia mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="8fb16-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="8fb16-135">Wywołania <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> z [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="8fb16-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="8fb16-136">Wyrażenie lambda przyjmuje <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> obiekt.</span><span class="sxs-lookup"><span data-stu-id="8fb16-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="8fb16-137">[Opcje konfiguracji](#cors-policy-options), takie jak `WithOrigins`, zostały opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="8fb16-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="8fb16-138">Wywołanie <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> metody dodaje usługi CORS do kontenera usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8fb16-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="8fb16-139">Aby uzyskać więcej informacji, zobacz [Opcje zasad CORS](#cpo) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="8fb16-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="8fb16-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Metoda może łączyć metody łańcucha, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="8fb16-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="8fb16-141">Uwaga: Adres URL **nie** może zawierać końcowego ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="8fb16-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="8fb16-142">Jeśli adres URL kończy się `/`na, porównywanie zwraca `false` i nie jest zwracany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="8fb16-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8fb16-143">Poniższy kod dotyczy zasad CORS dla wszystkich punktów końcowych aplikacji za pośrednictwem oprogramowania do obsługi mechanizmu CORS:</span><span class="sxs-lookup"><span data-stu-id="8fb16-143">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> <span data-ttu-id="8fb16-144">W przypadku routingu punktu końcowego oprogramowanie do obsługi mechanizmu CORS musi być skonfigurowane do wykonywania między `UseRouting` wywołaniami `UseEndpoints`i.</span><span class="sxs-lookup"><span data-stu-id="8fb16-144">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="8fb16-145">Niepoprawna konfiguracja spowoduje, że oprogramowanie pośredniczące przestanie działać poprawnie.</span><span class="sxs-lookup"><span data-stu-id="8fb16-145">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="8fb16-146">Poniższy kod dotyczy zasad CORS dla wszystkich punktów końcowych aplikacji za pośrednictwem oprogramowania do obsługi mechanizmu CORS:</span><span class="sxs-lookup"><span data-stu-id="8fb16-146">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="8fb16-147">Uwaga: `UseCors` należy wywołać przed `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="8fb16-147">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="8fb16-148">Zobacz [Włączanie mechanizmu CORS w Razor Pages, kontrolerach i metodach akcji](#ecors) , aby zastosować zasady CORS na poziomie strony/kontrolera/akcji.</span><span class="sxs-lookup"><span data-stu-id="8fb16-148">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="8fb16-149">Aby uzyskać instrukcje dotyczące testowania poprzedniego kodu, zobacz temat [CORS testów](#test) .</span><span class="sxs-lookup"><span data-stu-id="8fb16-149">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="8fb16-150">Włączanie mechanizmu CORS przy użyciu routingu punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="8fb16-150">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="8fb16-151">Za pomocą routingu punktu końcowego można włączyć funkcję CORS dla poszczególnych punktów końcowych przy użyciu `RequireCors` zestawu metod rozszerzających.</span><span class="sxs-lookup"><span data-stu-id="8fb16-151">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="8fb16-152">Podobnie można również włączyć funkcję CORS dla wszystkich kontrolerów:</span><span class="sxs-lookup"><span data-stu-id="8fb16-152">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="8fb16-153">Włączanie mechanizmu CORS z atrybutami</span><span class="sxs-lookup"><span data-stu-id="8fb16-153">Enable CORS with attributes</span></span>

<span data-ttu-id="8fb16-154">Atrybut [EnableCors&rbrack;stanowi alternatywę dla zastosowania mechanizmu CORS globalnie. &lbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="8fb16-154">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="8fb16-155">Ten `[EnableCors]` atrybut włącza funkcję CORS dla wybranych punktów końcowych, a nie wszystkich punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="8fb16-155">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="8fb16-156">Użyj `[EnableCors]` , aby określić zasady domyślne i `[EnableCors("{Policy String}")]` określić zasady.</span><span class="sxs-lookup"><span data-stu-id="8fb16-156">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="8fb16-157">Ten `[EnableCors]` atrybut może być stosowany do:</span><span class="sxs-lookup"><span data-stu-id="8fb16-157">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="8fb16-158">Strona Razor`PageModel`</span><span class="sxs-lookup"><span data-stu-id="8fb16-158">Razor Page `PageModel`</span></span>
* <span data-ttu-id="8fb16-159">Kontroler</span><span class="sxs-lookup"><span data-stu-id="8fb16-159">Controller</span></span>
* <span data-ttu-id="8fb16-160">Metoda akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="8fb16-160">Controller action method</span></span>

<span data-ttu-id="8fb16-161">Możesz zastosować różne zasady do kontrolera/strony-model/akcja z `[EnableCors]` atrybutem.</span><span class="sxs-lookup"><span data-stu-id="8fb16-161">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="8fb16-162">`[EnableCors]` Gdy atrybut jest stosowany do metody controllers/model/akcja, a funkcja CORS jest włączona w oprogramowaniu pośredniczącym, stosowane są obie zasady.</span><span class="sxs-lookup"><span data-stu-id="8fb16-162">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="8fb16-163">Zalecamy łączenie zasad.</span><span class="sxs-lookup"><span data-stu-id="8fb16-163">We recommend against combining policies.</span></span> <span data-ttu-id="8fb16-164">`[EnableCors]` Użyj atrybutu lub oprogramowania pośredniczącego, a nie obu w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8fb16-164">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="8fb16-165">Poniższy kod stosuje różne zasady do każdej metody:</span><span class="sxs-lookup"><span data-stu-id="8fb16-165">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="8fb16-166">Poniższy kod tworzy domyślne zasady CORS i zasady o nazwie `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="8fb16-166">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="8fb16-167">Wyłącz funkcję CORS</span><span class="sxs-lookup"><span data-stu-id="8fb16-167">Disable CORS</span></span>

<span data-ttu-id="8fb16-168">Atrybut [DisableCors&rbrack;wyłącza funkcję CORS dla kontrolera/strony-modelu/akcji. &lbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="8fb16-168">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="8fb16-169">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="8fb16-169">CORS policy options</span></span>

<span data-ttu-id="8fb16-170">W tej sekcji opisano różne opcje, które można ustawić w zasadach CORS:</span><span class="sxs-lookup"><span data-stu-id="8fb16-170">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="8fb16-171">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="8fb16-171">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="8fb16-172">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="8fb16-172">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="8fb16-173">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="8fb16-173">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="8fb16-174">Ustawianie nagłówków uwidocznionych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="8fb16-174">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="8fb16-175">Poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="8fb16-175">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="8fb16-176">Ustaw czas wygaśnięcia inspekcji wstępnej</span><span class="sxs-lookup"><span data-stu-id="8fb16-176">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="8fb16-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>jest wywoływana w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8fb16-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8fb16-178">W przypadku niektórych opcji warto przeczytać najpierw sekcję [jak działa mechanizm CORS](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="8fb16-178">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="8fb16-179">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="8fb16-179">Set the allowed origins</span></span>

<span data-ttu-id="8fb16-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Zezwala na żądania CORS ze wszystkich źródeł z dowolnym schematem`http` ( `https`lub). &ndash;</span><span class="sxs-lookup"><span data-stu-id="8fb16-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="8fb16-181">`AllowAnyOrigin`jest niezabezpieczony, ponieważ *Każda witryna sieci Web* może wprowadzać żądania między źródłami do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8fb16-181">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="8fb16-182">Określenie `AllowAnyOrigin` i`AllowCredentials` jest niebezpieczną konfiguracją i może skutkować fałszerstwem żądania między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="8fb16-183">Usługa CORS zwraca nieprawidłową odpowiedź CORS, gdy aplikacja jest skonfigurowana przy użyciu obu metod.</span><span class="sxs-lookup"><span data-stu-id="8fb16-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="8fb16-184">Określenie `AllowAnyOrigin` i`AllowCredentials` jest niebezpieczną konfiguracją i może skutkować fałszerstwem żądania między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="8fb16-185">W przypadku bezpiecznej aplikacji należy określić dokładną listę źródeł, jeśli klient musi autoryzować sam do uzyskiwania dostępu do zasobów serwera.</span><span class="sxs-lookup"><span data-stu-id="8fb16-185">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="8fb16-186">`AllowAnyOrigin`ma wpływ na żądania inspekcji wstępnej i `Access-Control-Allow-Origin` nagłówek.</span><span class="sxs-lookup"><span data-stu-id="8fb16-186">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="8fb16-187">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="8fb16-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8fb16-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Ustawia Właściwośćzasadjakofunkcję,którapozwalanapochodzeniezgodnezeskonfigurowanądomenąwieloznacznąpodczasoceniania,czy<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> pochodzenie jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="8fb16-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="8fb16-189">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="8fb16-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="8fb16-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="8fb16-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="8fb16-191">Zezwala na dowolną metodę HTTP:</span><span class="sxs-lookup"><span data-stu-id="8fb16-191">Allows any HTTP method:</span></span>
* <span data-ttu-id="8fb16-192">Ma wpływ na żądania inspekcji wstępnej i `Access-Control-Allow-Methods` nagłówek.</span><span class="sxs-lookup"><span data-stu-id="8fb16-192">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="8fb16-193">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="8fb16-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="8fb16-194">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="8fb16-194">Set the allowed request headers</span></span>

<span data-ttu-id="8fb16-195">Aby zezwolić na wysyłanie określonych nagłówków w żądaniu CORS, nazywanymi *nagłówkami żądań autora*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:</span><span class="sxs-lookup"><span data-stu-id="8fb16-195">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="8fb16-196">Aby zezwolić na wszystkie nagłówki żądań autora, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>Wywołaj:</span><span class="sxs-lookup"><span data-stu-id="8fb16-196">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="8fb16-197">To ustawienie ma wpływ na żądania inspekcji wstępnej i `Access-Control-Request-Headers` nagłówek.</span><span class="sxs-lookup"><span data-stu-id="8fb16-197">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="8fb16-198">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="8fb16-198">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8fb16-199">Zasady oprogramowania CORS są zgodne z określonymi nagłówkami określonymi przez `WithHeaders` jest możliwe tylko wtedy, gdy nagłówki `Access-Control-Request-Headers` wysyłane dokładnie pasują do nagłówków określonych `WithHeaders`w.</span><span class="sxs-lookup"><span data-stu-id="8fb16-199">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="8fb16-200">Na przykład rozważ zastosowanie skonfigurowanej aplikacji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8fb16-200">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="8fb16-201">Oprogramowanie pośredniczące CORS odrzuca żądanie wstępne z następującym nagłówkiem żądania, ponieważ `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) nie ma na `WithHeaders`liście:</span><span class="sxs-lookup"><span data-stu-id="8fb16-201">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="8fb16-202">Aplikacja zwraca odpowiedź *200 OK* , ale nie wysyła nagłówków CORS z powrotem.</span><span class="sxs-lookup"><span data-stu-id="8fb16-202">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="8fb16-203">W związku z tym przeglądarka nie próbuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-203">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8fb16-204">Oprogramowanie pośredniczące CORS zawsze umożliwia wysyłanie czterech nagłówków `Access-Control-Request-Headers` w celu, niezależnie od wartości skonfigurowanych w CorsPolicy. Heads.</span><span class="sxs-lookup"><span data-stu-id="8fb16-204">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="8fb16-205">Ta lista nagłówków obejmuje:</span><span class="sxs-lookup"><span data-stu-id="8fb16-205">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="8fb16-206">Na przykład rozważ zastosowanie skonfigurowanej aplikacji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8fb16-206">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="8fb16-207">Oprogramowanie pośredniczące CORS pomyślnie reaguje na żądanie inspekcji wstępnej z następującym nagłówkiem żądania `Content-Language` , ponieważ zawsze jest listy dozwolonych:</span><span class="sxs-lookup"><span data-stu-id="8fb16-207">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="8fb16-208">Ustawianie nagłówków uwidocznionych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="8fb16-208">Set the exposed response headers</span></span>

<span data-ttu-id="8fb16-209">Domyślnie przeglądarka nie ujawnia wszystkich nagłówków odpowiedzi dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8fb16-209">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="8fb16-210">Aby uzyskać więcej informacji, [zobacz Udostępnianie zasobów między źródłami W3C (terminologia): Prosty nagłówek](https://www.w3.org/TR/cors/#simple-response-header)odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8fb16-210">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="8fb16-211">Domyślnie dostępne są nagłówki odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="8fb16-211">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="8fb16-212">Specyfikacja CORS wywołuje te nagłówki *proste odpowiedzi*.</span><span class="sxs-lookup"><span data-stu-id="8fb16-212">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="8fb16-213">Aby udostępnić inne nagłówki aplikacji, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="8fb16-213">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="8fb16-214">Poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="8fb16-214">Credentials in cross-origin requests</span></span>

<span data-ttu-id="8fb16-215">Poświadczenia wymagają specjalnej obsługi w żądaniu CORS.</span><span class="sxs-lookup"><span data-stu-id="8fb16-215">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="8fb16-216">Domyślnie przeglądarka nie wysyła poświadczeń z żądaniem między źródłami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-216">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="8fb16-217">Poświadczenia obejmują pliki cookie i schematy uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="8fb16-217">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="8fb16-218">Aby wysłać poświadczenia z żądaniem między źródłami, klient musi mieć ustawioną `XMLHttpRequest.withCredentials` wartość `true`.</span><span class="sxs-lookup"><span data-stu-id="8fb16-218">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="8fb16-219">Bezpośrednie `XMLHttpRequest` używanie:</span><span class="sxs-lookup"><span data-stu-id="8fb16-219">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="8fb16-220">Za pomocą jQuery:</span><span class="sxs-lookup"><span data-stu-id="8fb16-220">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="8fb16-221">Za pomocą [interfejsu API pobierania](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="8fb16-221">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="8fb16-222">Serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="8fb16-222">The server must allow the credentials.</span></span> <span data-ttu-id="8fb16-223">Aby zezwolić na poświadczenia między źródłami, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>Wywołaj:</span><span class="sxs-lookup"><span data-stu-id="8fb16-223">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="8fb16-224">Odpowiedź HTTP zawiera `Access-Control-Allow-Credentials` nagłówek, który informuje przeglądarkę, że serwer zezwala na poświadczenia dla żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-224">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="8fb16-225">Jeśli przeglądarka wyśle poświadczenia, ale odpowiedź nie zawiera prawidłowego `Access-Control-Allow-Credentials` nagłówka, przeglądarka nie ujawnia odpowiedzi aplikacji, a żądanie między źródłami nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="8fb16-225">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="8fb16-226">Zezwalanie na poświadczenia między źródłami stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="8fb16-226">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="8fb16-227">Witryna sieci Web w innej domenie może wysyłać poświadczenia zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8fb16-227">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="8fb16-228">Specyfikacja mechanizmu CORS określa również, że ustawienia pochodzenia `"*"` (wszystkie źródła) są nieprawidłowe, `Access-Control-Allow-Credentials` Jeśli nagłówek jest obecny.</span><span class="sxs-lookup"><span data-stu-id="8fb16-228">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="8fb16-229">Żądania wstępnego lotu</span><span class="sxs-lookup"><span data-stu-id="8fb16-229">Preflight requests</span></span>

<span data-ttu-id="8fb16-230">W przypadku niektórych żądań CORS przeglądarka wysyła dodatkowe żądanie przed wykonaniem rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="8fb16-230">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="8fb16-231">To żądanie jest nazywane *żądaniem wstępnym*.</span><span class="sxs-lookup"><span data-stu-id="8fb16-231">This request is called a *preflight request*.</span></span> <span data-ttu-id="8fb16-232">Jeśli spełnione są następujące warunki, przeglądarka może pominąć żądanie wstępne:</span><span class="sxs-lookup"><span data-stu-id="8fb16-232">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="8fb16-233">Metoda żądania ma wartość GET, główna lub OPUBLIKOWANa.</span><span class="sxs-lookup"><span data-stu-id="8fb16-233">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="8fb16-234">Aplikacja nie ustawia nagłówków żądań innych niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, lub `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="8fb16-234">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="8fb16-235">`Content-Type` Nagłówek, jeśli jest ustawiony, ma jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="8fb16-235">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="8fb16-236">Reguła dotycząca nagłówków żądań ustawiona dla żądania klienta dotyczy nagłówków, które są ustawiane przez aplikację przez wywołanie `setRequestHeader` `XMLHttpRequest` na obiekcie.</span><span class="sxs-lookup"><span data-stu-id="8fb16-236">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="8fb16-237">Specyfikacja CORS wywołuje *nagłówki żądania autora*tych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="8fb16-237">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="8fb16-238">Reguła nie ma zastosowania do nagłówków, które można ustawić w przeglądarce, `User-Agent`na `Host`przykład, `Content-Length`lub.</span><span class="sxs-lookup"><span data-stu-id="8fb16-238">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="8fb16-239">Poniżej znajduje się przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="8fb16-239">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="8fb16-240">Żądanie przed inspekcją używa metody HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="8fb16-240">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="8fb16-241">Zawiera dwa specjalne nagłówki:</span><span class="sxs-lookup"><span data-stu-id="8fb16-241">It includes two special headers:</span></span>

* <span data-ttu-id="8fb16-242">`Access-Control-Request-Method`: Metoda HTTP, która będzie używana dla rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="8fb16-242">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="8fb16-243">`Access-Control-Request-Headers`: Lista nagłówków żądań, które aplikacja ustawia na rzeczywiste żądanie.</span><span class="sxs-lookup"><span data-stu-id="8fb16-243">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="8fb16-244">Jak wspomniano wcześniej, nie obejmuje to nagłówków, które są ustawiane przez `User-Agent`przeglądarkę, takich jak.</span><span class="sxs-lookup"><span data-stu-id="8fb16-244">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="8fb16-245">Żądanie inspekcji wstępnej CORS może zawierać `Access-Control-Request-Headers` nagłówek, który wskazuje serwerowi nagłówki wysyłane z rzeczywistym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="8fb16-245">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="8fb16-246">Aby zezwolić na określone nagłówki, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>Wywołaj:</span><span class="sxs-lookup"><span data-stu-id="8fb16-246">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="8fb16-247">Aby zezwolić na wszystkie nagłówki żądań autora, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>Wywołaj:</span><span class="sxs-lookup"><span data-stu-id="8fb16-247">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="8fb16-248">Przeglądarki nie są w pełni spójne w sposób `Access-Control-Request-Headers`ich ustawiania.</span><span class="sxs-lookup"><span data-stu-id="8fb16-248">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="8fb16-249">Jeśli ustawisz nagłówki jako elementy inne niż `"*"` (lub użycie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), należy uwzględnić co najmniej `Accept`, `Content-Type`i i `Origin`wszystkie niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="8fb16-249">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="8fb16-250">Poniżej znajduje się Przykładowa odpowiedź na żądanie inspekcji wstępnej (przy założeniu, że serwer zezwala na żądanie):</span><span class="sxs-lookup"><span data-stu-id="8fb16-250">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="8fb16-251">Odpowiedź zawiera `Access-Control-Allow-Methods` nagłówek, który zawiera listę dozwolonych metod i `Access-Control-Allow-Headers` opcjonalnie nagłówek, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="8fb16-251">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="8fb16-252">W przypadku pomyślnego przeprowadzenia żądania wstępnego przeglądarka wyśle rzeczywiste żądanie.</span><span class="sxs-lookup"><span data-stu-id="8fb16-252">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="8fb16-253">Jeśli żądanie wstępne nie zostanie odrzucone, aplikacja zwróci odpowiedź *200 OK* , ale nie wyśle nagłówków CORS z powrotem.</span><span class="sxs-lookup"><span data-stu-id="8fb16-253">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="8fb16-254">W związku z tym przeglądarka nie próbuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-254">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="8fb16-255">Ustaw czas wygaśnięcia inspekcji wstępnej</span><span class="sxs-lookup"><span data-stu-id="8fb16-255">Set the preflight expiration time</span></span>

<span data-ttu-id="8fb16-256">Nagłówek `Access-Control-Max-Age` określa, jak długo odpowiedź na żądanie inspekcji wstępnej może być buforowana.</span><span class="sxs-lookup"><span data-stu-id="8fb16-256">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="8fb16-257">Aby ustawić ten nagłówek, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="8fb16-257">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="8fb16-258">Jak działa mechanizm CORS</span><span class="sxs-lookup"><span data-stu-id="8fb16-258">How CORS works</span></span>

<span data-ttu-id="8fb16-259">W tej sekcji opisano, co się dzieje w żądaniu [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) na poziomie komunikatów http.</span><span class="sxs-lookup"><span data-stu-id="8fb16-259">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="8fb16-260">Mechanizm CORS **nie** jest funkcją zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8fb16-260">CORS is **not** a security feature.</span></span> <span data-ttu-id="8fb16-261">CORS jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="8fb16-261">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="8fb16-262">Na przykład złośliwy aktor może użyć [zapobiegania skryptom między lokacjami (XSS)](xref:security/cross-site-scripting) względem witryny i wykonać żądanie między lokacjami w celu wykraść informacji.</span><span class="sxs-lookup"><span data-stu-id="8fb16-262">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="8fb16-263">Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="8fb16-263">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="8fb16-264">Aby wymusić mechanizm CORS, należy do klienta (przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="8fb16-264">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="8fb16-265">Serwer wykonuje żądanie i zwraca odpowiedź, jest to klient, który zwraca błąd i blokuje odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="8fb16-265">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="8fb16-266">Na przykład w dowolnym z poniższych narzędzi zostanie wyświetlona odpowiedź serwera:</span><span class="sxs-lookup"><span data-stu-id="8fb16-266">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="8fb16-267">Fiddler</span><span class="sxs-lookup"><span data-stu-id="8fb16-267">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="8fb16-268">Postman</span><span class="sxs-lookup"><span data-stu-id="8fb16-268">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="8fb16-269">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="8fb16-269">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="8fb16-270">Przeglądarka sieci Web, wprowadzając adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8fb16-270">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="8fb16-271">Jest to sposób, aby serwer zezwalał przeglądarce na wykonywanie żądania [interfejsu API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) lub pobierania między źródłami, które w przeciwnym razie byłoby zabronione.</span><span class="sxs-lookup"><span data-stu-id="8fb16-271">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="8fb16-272">Przeglądarki (bez CORS) nie mogą wykonywać żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-272">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="8fb16-273">Przed zastosowaniem mechanizmu CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) został użyty do obejścia tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="8fb16-273">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="8fb16-274">JSONP nie używa XHR, używa `<script>` znacznika do odbierania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8fb16-274">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="8fb16-275">Skrypty mogą być ładowane między różnymi źródłami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-275">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="8fb16-276">[Specyfikacja CORS](https://www.w3.org/TR/cors/) wprowadziła kilka nowych nagłówków HTTP, które umożliwiają żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="8fb16-276">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="8fb16-277">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia te nagłówki automatycznie dla żądań cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="8fb16-277">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="8fb16-278">Niestandardowy kod JavaScript nie jest wymagany do włączenia mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="8fb16-278">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="8fb16-279">Poniżej przedstawiono przykład żądania między źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="8fb16-279">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="8fb16-280">`Origin` Nagłówek zawiera domenę lokacji, która żąda żądania:</span><span class="sxs-lookup"><span data-stu-id="8fb16-280">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="8fb16-281">Jeśli serwer zezwala na żądanie, ustawia `Access-Control-Allow-Origin` nagłówek w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8fb16-281">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="8fb16-282">Wartość tego nagłówka jest zgodna `Origin` z nagłówkiem z żądania lub jest wartością `"*"`symbolu wieloznacznego, co oznacza, że wszystkie źródła są dozwolone:</span><span class="sxs-lookup"><span data-stu-id="8fb16-282">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="8fb16-283">Jeśli odpowiedź nie zawiera `Access-Control-Allow-Origin` nagłówka, żądanie między źródłami nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="8fb16-283">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="8fb16-284">W programie przeglądarka nie zezwala na żądanie.</span><span class="sxs-lookup"><span data-stu-id="8fb16-284">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="8fb16-285">Nawet jeśli serwer zwróci pomyślną odpowiedź, przeglądarka nie udostępni odpowiedzi dla aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="8fb16-285">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="8fb16-286">Testowanie CORS</span><span class="sxs-lookup"><span data-stu-id="8fb16-286">Test CORS</span></span>

<span data-ttu-id="8fb16-287">Aby przetestować CORS:</span><span class="sxs-lookup"><span data-stu-id="8fb16-287">To test CORS:</span></span>

1. <span data-ttu-id="8fb16-288">[Utwórz projekt interfejsu API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="8fb16-288">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="8fb16-289">Możesz również [pobrać przykład](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="8fb16-289">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="8fb16-290">Włącz funkcję CORS przy użyciu jednego z metod w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="8fb16-290">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="8fb16-291">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8fb16-291">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="8fb16-292">`WithOrigins("https://localhost:<port>");`powinien być używany tylko do testowania przykładowej aplikacji podobnej do [przykładowego kodu pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="8fb16-292">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="8fb16-293">Utwórz projekt aplikacji sieci Web (Razor Pages lub MVC).</span><span class="sxs-lookup"><span data-stu-id="8fb16-293">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="8fb16-294">Przykład używa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8fb16-294">The sample uses Razor Pages.</span></span> <span data-ttu-id="8fb16-295">Aplikację sieci Web można utworzyć w tym samym rozwiązaniu co projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="8fb16-295">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="8fb16-296">Dodaj następujący wyróżniony kod do pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="8fb16-296">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="8fb16-297">W poprzednim kodzie Zastąp `url: 'https://<web app>.azurewebsites.net/api/values/1',` ciąg adresem URL wdrożonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8fb16-297">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="8fb16-298">Wdróż projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="8fb16-298">Deploy the API project.</span></span> <span data-ttu-id="8fb16-299">Na przykład [Wdróż na platformie Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="8fb16-299">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="8fb16-300">Uruchom aplikację Razor Pages lub MVC na pulpicie, a następnie kliknij przycisk **Testuj** .</span><span class="sxs-lookup"><span data-stu-id="8fb16-300">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="8fb16-301">Użyj narzędzi F12, aby przejrzeć komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="8fb16-301">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="8fb16-302">Usuń pochodzenie hosta lokalnego `WithOrigins` z i Wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="8fb16-302">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="8fb16-303">Alternatywnie Uruchom aplikację kliencką z innym portem.</span><span class="sxs-lookup"><span data-stu-id="8fb16-303">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="8fb16-304">Na przykład uruchom polecenie z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8fb16-304">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="8fb16-305">Przetestuj za pomocą aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="8fb16-305">Test with the client app.</span></span> <span data-ttu-id="8fb16-306">Błędy funkcji CORS zwracają błąd, ale komunikat o błędzie nie jest dostępny dla języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8fb16-306">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="8fb16-307">Aby wyświetlić błąd, Użyj karty konsola w narzędziach F12.</span><span class="sxs-lookup"><span data-stu-id="8fb16-307">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="8fb16-308">W zależności od przeglądarki pojawia się błąd (w konsoli narzędzia F12) podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="8fb16-308">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="8fb16-309">Korzystanie z przeglądarki Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="8fb16-309">Using Microsoft Edge:</span></span>

     <span data-ttu-id="8fb16-310">**SEC7120: [CORS] nie znaleziono `https://localhost:44375` `https://localhost:44375` źródła w nagłówku odpowiedzi "Access-Control-Allow-Origin" dla zasobu Cross-Origin w`https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="8fb16-310">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="8fb16-311">Korzystanie z programu Chrome:</span><span class="sxs-lookup"><span data-stu-id="8fb16-311">Using Chrome:</span></span>

     <span data-ttu-id="8fb16-312">**Dostęp do elementu XMLHttpRequest `https://webapi.azurewebsites.net/api/values/1` w lokalizacji `https://localhost:44375` z punktu początkowego został zablokowany przez zasady CORS: Brak nagłówka "Access-Control-Allow-Origin" w żądanym zasobie.**</span><span class="sxs-lookup"><span data-stu-id="8fb16-312">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8fb16-313">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8fb16-313">Additional resources</span></span>

* [<span data-ttu-id="8fb16-314">Współużytkowanie zasobów między źródłami (CORS)</span><span class="sxs-lookup"><span data-stu-id="8fb16-314">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
