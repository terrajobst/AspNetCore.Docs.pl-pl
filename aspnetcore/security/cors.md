---
title: Włącz żądania między źródłami (CORS) w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób mechanizm CORS jest standardem umożliwiającym lub odrzucanie żądań między źródłami w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 57098be73164c71d1b0d1fe2f3aee7ec41a32346
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727315"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="1dfbe-103">Włącz żądania między źródłami (CORS) w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1dfbe-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="1dfbe-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1dfbe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1dfbe-105">W tym artykule pokazano, jak włączyć funkcję CORS w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="1dfbe-106">Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która była obsługiwana przez stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="1dfbe-107">To ograniczenie jest nazywane *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="1dfbe-108">Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="1dfbe-109">Czasami możesz chcieć zezwolić innym lokacjom na wykonywanie żądań między źródłami do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="1dfbe-110">Aby uzyskać więcej informacji, zobacz [artykuł CORS firmy Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="1dfbe-111">[Współużytkowanie zasobów między źródłami](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="1dfbe-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="1dfbe-112">Jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="1dfbe-113">**Nie** jest funkcją zabezpieczeń, funkcja CORS osłabi zabezpieczenia.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="1dfbe-114">Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="1dfbe-115">Aby uzyskać więcej informacji, zobacz [jak działa mechanizm CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="1dfbe-116">Zezwala serwerowi jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="1dfbe-117">Jest bezpieczniejsze i bardziej elastyczne niż wcześniejsze techniki, takie jak [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="1dfbe-118">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1dfbe-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="1dfbe-119">To samo źródło</span><span class="sxs-lookup"><span data-stu-id="1dfbe-119">Same origin</span></span>

<span data-ttu-id="1dfbe-120">Dwa adresy URL mają te same źródła, jeśli mają identyczne schematy, hosty i porty ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="1dfbe-121">Te dwa adresy URL mają te same źródła:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="1dfbe-122">Te adresy URL mają różne źródła niż poprzednie dwa adresy URL:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="1dfbe-123">`https://example.net` &ndash; innej domeny</span><span class="sxs-lookup"><span data-stu-id="1dfbe-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="1dfbe-124">`https://www.example.com/foo.html` &ndash; inną poddomeną</span><span class="sxs-lookup"><span data-stu-id="1dfbe-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="1dfbe-125">`http://example.com/foo.html` &ndash; innego schematu</span><span class="sxs-lookup"><span data-stu-id="1dfbe-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="1dfbe-126">`https://example.com:9000/foo.html` &ndash; inny port</span><span class="sxs-lookup"><span data-stu-id="1dfbe-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="1dfbe-127">Program Internet Explorer nie traktuje portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="1dfbe-128">CORS z nazwanymi zasadami i programem pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="1dfbe-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="1dfbe-129">Oprogramowanie pośredniczące CORS obsługuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="1dfbe-130">Poniższy kod włącza mechanizm CORS dla całej aplikacji z określonym źródłem:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="1dfbe-131">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-131">The preceding code:</span></span>

* <span data-ttu-id="1dfbe-132">Ustawia nazwę zasad na "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="1dfbe-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="1dfbe-133">Nazwa zasad jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="1dfbe-134">Wywołuje metodę rozszerzenia <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, która umożliwia mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="1dfbe-135">Wywołuje <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> z [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="1dfbe-136">Wyrażenie lambda przyjmuje obiekt <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="1dfbe-137">[Opcje konfiguracji](#cors-policy-options), takie jak `WithOrigins`, zostały opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="1dfbe-138">Wywołanie metody <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> dodaje usługi CORS do kontenera usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="1dfbe-139">Aby uzyskać więcej informacji, zobacz [Opcje zasad CORS](#cpo) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="1dfbe-140">Metoda <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> może łączyć metody łańcucha, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="1dfbe-141">Uwaga: adres URL **nie** może zawierać końcowego ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="1dfbe-142">Jeśli adres URL kończy się `/`, porównanie zwraca `false` i żaden nagłówek nie zostanie zwrócony.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="1dfbe-143">Zastosuj zasady CORS do wszystkich punktów końcowych</span><span class="sxs-lookup"><span data-stu-id="1dfbe-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="1dfbe-144">Poniższy kod dotyczy zasad CORS dla wszystkich punktów końcowych aplikacji za pośrednictwem oprogramowania do obsługi mechanizmu CORS:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
> <span data-ttu-id="1dfbe-145">W przypadku routingu punktu końcowego oprogramowanie do obsługi mechanizmu CORS musi być skonfigurowane do wykonywania między wywołaniami `UseRouting` i `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="1dfbe-146">Niepoprawna konfiguracja spowoduje, że oprogramowanie pośredniczące przestanie działać poprawnie.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="1dfbe-147">Poniższy kod dotyczy zasad CORS dla wszystkich punktów końcowych aplikacji za pośrednictwem oprogramowania do obsługi mechanizmu CORS:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-147">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="1dfbe-148">Uwaga: przed `UseMvc`należy wywołać `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-148">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="1dfbe-149">Zobacz [Włączanie mechanizmu CORS w Razor Pages, kontrolerach i metodach akcji](#ecors) , aby zastosować zasady CORS na poziomie strony/kontrolera/akcji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-149">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="1dfbe-150">Aby uzyskać instrukcje dotyczące testowania poprzedniego kodu, zobacz temat [CORS testów](#test) .</span><span class="sxs-lookup"><span data-stu-id="1dfbe-150">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="1dfbe-151">Włączanie mechanizmu CORS przy użyciu routingu punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="1dfbe-151">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="1dfbe-152">Za pomocą routingu punktu końcowego można włączyć funkcję CORS dla poszczególnych punktów końcowych przy użyciu zestawu `RequireCors` metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-152">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="1dfbe-153">Podobnie można również włączyć funkcję CORS dla wszystkich kontrolerów:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-153">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="1dfbe-154">Włączanie mechanizmu CORS z atrybutami</span><span class="sxs-lookup"><span data-stu-id="1dfbe-154">Enable CORS with attributes</span></span>

<span data-ttu-id="1dfbe-155">Atrybut [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) stanowi alternatywę do stosowania mechanizmu CORS globalnie.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-155">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="1dfbe-156">Atrybut `[EnableCors]` włącza funkcję CORS dla wybranych punktów końcowych, a nie wszystkich punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-156">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="1dfbe-157">Użyj `[EnableCors]`, aby określić zasady domyślne i `[EnableCors("{Policy String}")]`, aby określić zasady.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-157">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="1dfbe-158">Atrybut `[EnableCors]` można zastosować do:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-158">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="1dfbe-159">`PageModel` strony Razor</span><span class="sxs-lookup"><span data-stu-id="1dfbe-159">Razor Page `PageModel`</span></span>
* <span data-ttu-id="1dfbe-160">Kontroler</span><span class="sxs-lookup"><span data-stu-id="1dfbe-160">Controller</span></span>
* <span data-ttu-id="1dfbe-161">Metoda akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="1dfbe-161">Controller action method</span></span>

<span data-ttu-id="1dfbe-162">Możesz zastosować różne zasady do kontrolera/strony-model/akcja z atrybutem `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-162">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="1dfbe-163">Gdy atrybut `[EnableCors]` jest stosowany do metody controllers/model/akcja, a funkcja CORS jest włączona w oprogramowaniu pośredniczącym, obie zasady są stosowane.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-163">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="1dfbe-164">Zalecamy łączenie zasad.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-164">We recommend against combining policies.</span></span> <span data-ttu-id="1dfbe-165">Użyj atrybutu `[EnableCors]` lub oprogramowania pośredniczącego, a nie obu w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-165">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="1dfbe-166">Poniższy kod stosuje różne zasady do każdej metody:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-166">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="1dfbe-167">Poniższy kod tworzy domyślne zasady CORS i zasady o nazwie `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-167">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="1dfbe-168">Wyłącz funkcję CORS</span><span class="sxs-lookup"><span data-stu-id="1dfbe-168">Disable CORS</span></span>

<span data-ttu-id="1dfbe-169">Atrybut [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) wyłącza funkcję CORS dla kontrolera/strony-modelu/akcji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-169">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="1dfbe-170">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="1dfbe-170">CORS policy options</span></span>

<span data-ttu-id="1dfbe-171">W tej sekcji opisano różne opcje, które można ustawić w zasadach CORS:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-171">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="1dfbe-172">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="1dfbe-172">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="1dfbe-173">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="1dfbe-173">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="1dfbe-174">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="1dfbe-174">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="1dfbe-175">Ustawianie nagłówków uwidocznionych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="1dfbe-175">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="1dfbe-176">Poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="1dfbe-176">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="1dfbe-177">Ustaw czas wygaśnięcia inspekcji wstępnej</span><span class="sxs-lookup"><span data-stu-id="1dfbe-177">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="1dfbe-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> jest wywoływana w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1dfbe-179">W przypadku niektórych opcji warto przeczytać najpierw sekcję [jak działa mechanizm CORS](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="1dfbe-179">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="1dfbe-180">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="1dfbe-180">Set the allowed origins</span></span>

<span data-ttu-id="1dfbe-181">&ndash; <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> zezwala na żądania CORS ze wszystkich źródeł z dowolnym schematem (`http` lub `https`).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="1dfbe-182">`AllowAnyOrigin` jest niezabezpieczona, ponieważ *Każda witryna sieci Web* może wprowadzać żądania między źródłami do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-182">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="1dfbe-183">Określanie `AllowAnyOrigin` i `AllowCredentials` jest niebezpieczną konfiguracją i może skutkować fałszerstwem żądania między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="1dfbe-184">Usługa CORS zwraca nieprawidłową odpowiedź CORS, gdy aplikacja jest skonfigurowana przy użyciu obu metod.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-184">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="1dfbe-185">Określanie `AllowAnyOrigin` i `AllowCredentials` jest niebezpieczną konfiguracją i może skutkować fałszerstwem żądania między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-185">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="1dfbe-186">W przypadku bezpiecznej aplikacji należy określić dokładną listę źródeł, jeśli klient musi autoryzować sam do uzyskiwania dostępu do zasobów serwera.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-186">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="1dfbe-187">`AllowAnyOrigin` ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-187">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="1dfbe-188">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="1dfbe-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1dfbe-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; ustawia właściwość <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> zasad jako funkcję, która umożliwia pochodzenie do dopasowania do skonfigurowanej domeny z symbolami wieloznacznymi podczas oceniania, czy pochodzenie jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="1dfbe-190">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="1dfbe-190">Set the allowed HTTP methods</span></span>

<span data-ttu-id="1dfbe-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="1dfbe-192">Zezwala na dowolną metodę HTTP:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-192">Allows any HTTP method:</span></span>
* <span data-ttu-id="1dfbe-193">Ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-193">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="1dfbe-194">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="1dfbe-194">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="1dfbe-195">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="1dfbe-195">Set the allowed request headers</span></span>

<span data-ttu-id="1dfbe-196">Aby zezwolić na wysyłanie określonych nagłówków w żądaniu CORS, nazywanymi *nagłówkami żądań autora*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-196">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="1dfbe-197">Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-197">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="1dfbe-198">To ustawienie ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-198">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="1dfbe-199">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="1dfbe-199">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1dfbe-200">Zasady oprogramowania CORS są zgodne z określonymi nagłówkami określonymi przez `WithHeaders` jest możliwe tylko wtedy, gdy nagłówki wysyłane w `Access-Control-Request-Headers` dokładnie pasują do nagłówków określonych w `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-200">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="1dfbe-201">Na przykład rozważ zastosowanie skonfigurowanej aplikacji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="1dfbe-202">Oprogramowanie pośredniczące CORS odrzuca żądanie wstępne z następującym nagłówkiem żądania, ponieważ `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) nie znajduje się na liście `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-202">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="1dfbe-203">Aplikacja zwraca odpowiedź *200 OK* , ale nie wysyła nagłówków CORS z powrotem.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-203">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="1dfbe-204">W związku z tym przeglądarka nie próbuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-204">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1dfbe-205">Oprogramowanie pośredniczące CORS zawsze umożliwia wysyłanie czterech nagłówków w `Access-Control-Request-Headers` niezależnie od wartości skonfigurowanych w CorsPolicy. Heads.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-205">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="1dfbe-206">Ta lista nagłówków obejmuje:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-206">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="1dfbe-207">Na przykład rozważ zastosowanie skonfigurowanej aplikacji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-207">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="1dfbe-208">Oprogramowanie pośredniczące CORS pomyślnie reaguje na żądanie inspekcji wstępnej z następującym nagłówkiem żądania, ponieważ `Content-Language` jest zawsze listy dozwolonych:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-208">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="1dfbe-209">Ustawianie nagłówków uwidocznionych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="1dfbe-209">Set the exposed response headers</span></span>

<span data-ttu-id="1dfbe-210">Domyślnie przeglądarka nie ujawnia wszystkich nagłówków odpowiedzi dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-210">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="1dfbe-211">Aby uzyskać więcej informacji, zobacz temat [udostępnianie zasobów między źródłami W3C (terminologia): prosty nagłówek odpowiedzi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-211">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="1dfbe-212">Domyślnie dostępne są nagłówki odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-212">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="1dfbe-213">Specyfikacja CORS wywołuje te nagłówki *proste odpowiedzi*.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-213">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="1dfbe-214">Aby udostępnić inne nagłówki dla aplikacji, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-214">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="1dfbe-215">Poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="1dfbe-215">Credentials in cross-origin requests</span></span>

<span data-ttu-id="1dfbe-216">Poświadczenia wymagają specjalnej obsługi w żądaniu CORS.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-216">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="1dfbe-217">Domyślnie przeglądarka nie wysyła poświadczeń z żądaniem między źródłami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-217">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="1dfbe-218">Poświadczenia obejmują pliki cookie i schematy uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-218">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="1dfbe-219">Aby wysłać poświadczenia z żądaniem między źródłami, klient musi ustawić `XMLHttpRequest.withCredentials`, aby `true`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-219">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="1dfbe-220">Używanie `XMLHttpRequest` bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-220">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="1dfbe-221">Za pomocą jQuery:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-221">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="1dfbe-222">Za pomocą [interfejsu API pobierania](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="1dfbe-222">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="1dfbe-223">Serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-223">The server must allow the credentials.</span></span> <span data-ttu-id="1dfbe-224">Aby zezwolić na poświadczenia między źródłami, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-224">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="1dfbe-225">Odpowiedź HTTP zawiera nagłówek `Access-Control-Allow-Credentials`, który informuje przeglądarkę, że serwer zezwala na poświadczenia dla żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-225">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="1dfbe-226">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka `Access-Control-Allow-Credentials`, przeglądarka nie ujawnia odpowiedzi aplikacji, a żądanie między źródłami nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-226">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="1dfbe-227">Zezwalanie na poświadczenia między źródłami stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-227">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="1dfbe-228">Witryna sieci Web w innej domenie może wysyłać poświadczenia zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-228">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="1dfbe-229">Specyfikacja CORS określa również, że źródła ustawień dla `"*"` (wszystkie źródła) są nieprawidłowe, jeśli nagłówek `Access-Control-Allow-Credentials` jest obecny.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-229">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="1dfbe-230">Żądania wstępnego lotu</span><span class="sxs-lookup"><span data-stu-id="1dfbe-230">Preflight requests</span></span>

<span data-ttu-id="1dfbe-231">W przypadku niektórych żądań CORS przeglądarka wysyła dodatkowe żądanie przed wykonaniem rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-231">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="1dfbe-232">To żądanie jest nazywane *żądaniem wstępnym*.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-232">This request is called a *preflight request*.</span></span> <span data-ttu-id="1dfbe-233">Jeśli spełnione są następujące warunki, przeglądarka może pominąć żądanie wstępne:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-233">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="1dfbe-234">Metoda żądania ma wartość GET, główna lub OPUBLIKOWANa.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-234">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="1dfbe-235">Aplikacja nie ustawia nagłówków żądań innych niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`lub `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-235">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="1dfbe-236">Nagłówek `Content-Type`, jeśli jest ustawiony, ma jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-236">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="1dfbe-237">Reguła dotycząca nagłówków żądań ustawiona dla żądania klienta dotyczy nagłówków, które są ustawiane przez aplikację przez wywołanie `setRequestHeader` na obiekcie `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-237">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="1dfbe-238">Specyfikacja CORS wywołuje *nagłówki żądania autora*tych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-238">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="1dfbe-239">Reguła nie ma zastosowania do nagłówków, które można ustawić w przeglądarce, takich jak `User-Agent`, `Host`lub `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-239">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="1dfbe-240">Poniżej znajduje się przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-240">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="1dfbe-241">Żądanie przed inspekcją używa metody HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-241">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="1dfbe-242">Zawiera dwa specjalne nagłówki:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-242">It includes two special headers:</span></span>

* <span data-ttu-id="1dfbe-243">`Access-Control-Request-Method`: metoda HTTP, która będzie używana dla rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-243">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="1dfbe-244">`Access-Control-Request-Headers`: lista nagłówków żądań, które aplikacja ustawia na rzeczywiste żądanie.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-244">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="1dfbe-245">Jak wspomniano wcześniej, nie obejmuje to nagłówków, które są ustawiane przez przeglądarkę, takich jak `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-245">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="1dfbe-246">Żądanie inspekcji wstępnej CORS może zawierać nagłówek `Access-Control-Request-Headers` wskazujący serwerowi nagłówki wysyłane z rzeczywistym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-246">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="1dfbe-247">Aby zezwolić na określone nagłówki, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-247">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="1dfbe-248">Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-248">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="1dfbe-249">Przeglądarki nie są w pełni spójne w sposób, w jaki ustawili `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-249">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="1dfbe-250">Jeśli ustawisz nagłówki na inne niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), należy uwzględnić co najmniej `Accept`, `Content-Type`i `Origin`oraz wszystkie niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-250">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="1dfbe-251">Poniżej znajduje się Przykładowa odpowiedź na żądanie inspekcji wstępnej (przy założeniu, że serwer zezwala na żądanie):</span><span class="sxs-lookup"><span data-stu-id="1dfbe-251">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="1dfbe-252">Odpowiedź zawiera nagłówek `Access-Control-Allow-Methods`, który zawiera listę dozwolonych metod i opcjonalnie nagłówek `Access-Control-Allow-Headers`, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-252">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="1dfbe-253">W przypadku pomyślnego przeprowadzenia żądania wstępnego przeglądarka wyśle rzeczywiste żądanie.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-253">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="1dfbe-254">Jeśli żądanie wstępne nie zostanie odrzucone, aplikacja zwróci odpowiedź *200 OK* , ale nie wyśle nagłówków CORS z powrotem.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-254">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="1dfbe-255">W związku z tym przeglądarka nie próbuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-255">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="1dfbe-256">Ustaw czas wygaśnięcia inspekcji wstępnej</span><span class="sxs-lookup"><span data-stu-id="1dfbe-256">Set the preflight expiration time</span></span>

<span data-ttu-id="1dfbe-257">Nagłówek `Access-Control-Max-Age` określa, jak długo odpowiedź na żądanie inspekcji wstępnej może być buforowana.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-257">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="1dfbe-258">Aby ustawić ten nagłówek, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-258">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="1dfbe-259">Jak działa mechanizm CORS</span><span class="sxs-lookup"><span data-stu-id="1dfbe-259">How CORS works</span></span>

<span data-ttu-id="1dfbe-260">W tej sekcji opisano, co się dzieje w żądaniu [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) na poziomie komunikatów http.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-260">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="1dfbe-261">Mechanizm CORS **nie** jest funkcją zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-261">CORS is **not** a security feature.</span></span> <span data-ttu-id="1dfbe-262">CORS jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-262">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="1dfbe-263">Na przykład złośliwy aktor może użyć [zapobiegania skryptom między lokacjami (XSS)](xref:security/cross-site-scripting) względem witryny i wykonać żądanie między lokacjami w celu wykraść informacji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-263">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="1dfbe-264">Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-264">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="1dfbe-265">Aby wymusić mechanizm CORS, należy do klienta (przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-265">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="1dfbe-266">Serwer wykonuje żądanie i zwraca odpowiedź, jest to klient, który zwraca błąd i blokuje odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-266">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="1dfbe-267">Na przykład w dowolnym z poniższych narzędzi zostanie wyświetlona odpowiedź serwera:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-267">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="1dfbe-268">Fiddler</span><span class="sxs-lookup"><span data-stu-id="1dfbe-268">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="1dfbe-269">Postman</span><span class="sxs-lookup"><span data-stu-id="1dfbe-269">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="1dfbe-270">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="1dfbe-270">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="1dfbe-271">Przeglądarka sieci Web, wprowadzając adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-271">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="1dfbe-272">Jest to sposób, aby serwer zezwalał przeglądarce na wykonywanie żądania [interfejsu API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) lub pobierania między źródłami, które w przeciwnym razie byłoby zabronione.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-272">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="1dfbe-273">Przeglądarki (bez CORS) nie mogą wykonywać żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-273">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="1dfbe-274">Przed zastosowaniem mechanizmu CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) został użyty do obejścia tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-274">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="1dfbe-275">JSONP nie używa XHR, aby otrzymać odpowiedź, używa znacznika `<script>`.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-275">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="1dfbe-276">Skrypty mogą być ładowane między różnymi źródłami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-276">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="1dfbe-277">[Specyfikacja CORS](https://www.w3.org/TR/cors/) wprowadziła kilka nowych nagłówków HTTP, które umożliwiają żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-277">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="1dfbe-278">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia te nagłówki automatycznie dla żądań cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-278">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="1dfbe-279">Niestandardowy kod JavaScript nie jest wymagany do włączenia mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-279">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="1dfbe-280">Poniżej przedstawiono przykład żądania między źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-280">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="1dfbe-281">Nagłówek `Origin` zapewnia domenę lokacji, która żąda żądania.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-281">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="1dfbe-282">Nagłówek `Origin` jest wymagany i musi się różnić od hosta.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-282">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="1dfbe-283">Jeśli serwer zezwala na żądanie, ustawia nagłówek `Access-Control-Allow-Origin` w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-283">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="1dfbe-284">Wartość tego nagłówka jest zgodna z nagłówkiem `Origin` z żądania lub jest wartością symbolu wieloznacznego `"*"`, co oznacza, że wszystkie źródła są dozwolone:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-284">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="1dfbe-285">Jeśli odpowiedź nie zawiera nagłówka `Access-Control-Allow-Origin`, żądanie między źródłami nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-285">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="1dfbe-286">W programie przeglądarka nie zezwala na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-286">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="1dfbe-287">Nawet jeśli serwer zwróci pomyślną odpowiedź, przeglądarka nie udostępni odpowiedzi dla aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-287">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="1dfbe-288">Testowanie CORS</span><span class="sxs-lookup"><span data-stu-id="1dfbe-288">Test CORS</span></span>

<span data-ttu-id="1dfbe-289">Aby przetestować CORS:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-289">To test CORS:</span></span>

1. <span data-ttu-id="1dfbe-290">[Utwórz projekt interfejsu API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-290">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="1dfbe-291">Możesz również [pobrać przykład](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-291">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="1dfbe-292">Włącz funkcję CORS przy użyciu jednego z metod w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-292">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="1dfbe-293">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-293">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="1dfbe-294">`WithOrigins("https://localhost:<port>");` należy używać tylko do testowania przykładowej aplikacji podobnej do [przykładowego kodu pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-294">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="1dfbe-295">Utwórz projekt aplikacji sieci Web (Razor Pages lub MVC).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-295">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="1dfbe-296">Przykład używa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-296">The sample uses Razor Pages.</span></span> <span data-ttu-id="1dfbe-297">Aplikację sieci Web można utworzyć w tym samym rozwiązaniu co projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-297">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="1dfbe-298">Dodaj następujący wyróżniony kod do pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="1dfbe-298">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="1dfbe-299">W poprzednim kodzie Zastąp `url: 'https://<web app>.azurewebsites.net/api/values/1',` adresem URL wdrożonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-299">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="1dfbe-300">Wdróż projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-300">Deploy the API project.</span></span> <span data-ttu-id="1dfbe-301">Na przykład [Wdróż na platformie Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-301">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="1dfbe-302">Uruchom aplikację Razor Pages lub MVC na pulpicie, a następnie kliknij przycisk **Testuj** .</span><span class="sxs-lookup"><span data-stu-id="1dfbe-302">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="1dfbe-303">Użyj narzędzi F12, aby przejrzeć komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-303">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="1dfbe-304">Usuń pochodzenie hosta lokalnego z `WithOrigins` i Wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-304">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="1dfbe-305">Alternatywnie Uruchom aplikację kliencką z innym portem.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-305">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="1dfbe-306">Na przykład uruchom polecenie z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-306">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="1dfbe-307">Przetestuj za pomocą aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-307">Test with the client app.</span></span> <span data-ttu-id="1dfbe-308">Błędy funkcji CORS zwracają błąd, ale komunikat o błędzie nie jest dostępny dla języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-308">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="1dfbe-309">Aby wyświetlić błąd, Użyj karty konsola w narzędziach F12.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-309">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="1dfbe-310">W zależności od przeglądarki pojawia się błąd (w konsoli narzędzia F12) podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-310">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="1dfbe-311">Korzystanie z przeglądarki Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-311">Using Microsoft Edge:</span></span>

     <span data-ttu-id="1dfbe-312">**SEC7120: [CORS] Źródło `https://localhost:44375` nie znalazło `https://localhost:44375` w nagłówku odpowiedzi "Access-Control-Allow-Origin" dla zasobu Cross-Origin w `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="1dfbe-312">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="1dfbe-313">Korzystanie z programu Chrome:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-313">Using Chrome:</span></span>

     <span data-ttu-id="1dfbe-314">**Dostęp do elementu XMLHttpRequest na `https://webapi.azurewebsites.net/api/values/1` ze źródła `https://localhost:44375` został zablokowany przez zasady CORS: brak nagłówka "Access-Control-Allow-Origin" w żądanym zasobie.**</span><span class="sxs-lookup"><span data-stu-id="1dfbe-314">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="1dfbe-315">Punkty końcowe z obsługą mechanizmu CORS można testować za pomocą narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler) lub [Poster](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="1dfbe-315">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="1dfbe-316">W przypadku korzystania z narzędzia, Źródło żądania określone przez nagłówek `Origin` musi różnić się od hosta przyjmującego żądanie.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-316">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="1dfbe-317">Jeśli żądanie nie jest *źródłem krzyżowe* na podstawie wartości nagłówka `Origin`:</span><span class="sxs-lookup"><span data-stu-id="1dfbe-317">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="1dfbe-318">Nie ma potrzeby przetwarzania żądania przez oprogramowanie pośredniczące CORS.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-318">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="1dfbe-319">Nagłówki CORS nie są zwracane w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-319">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="1dfbe-320">Mechanizm CORS w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="1dfbe-320">CORS in IIS</span></span>

<span data-ttu-id="1dfbe-321">W przypadku wdrażania w programie IIS należy uruchomić funkcję CORS przed uwierzytelnianiem systemu Windows, jeśli serwer nie jest skonfigurowany do zezwalania na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-321">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="1dfbe-322">Aby zapewnić obsługę tego scenariusza, należy zainstalować i skonfigurować [moduł CORS usług IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1dfbe-322">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1dfbe-323">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1dfbe-323">Additional resources</span></span>

* [<span data-ttu-id="1dfbe-324">Współużytkowanie zasobów między źródłami (CORS)</span><span class="sxs-lookup"><span data-stu-id="1dfbe-324">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="1dfbe-325">Wprowadzenie do modułu CORS usług IIS</span><span class="sxs-lookup"><span data-stu-id="1dfbe-325">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)
