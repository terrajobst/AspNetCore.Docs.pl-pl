---
title: Włącz żądania między źródłami (CORS) w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób mechanizm CORS jest standardem umożliwiającym lub odrzucanie żądań między źródłami w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 09cc296ebf605907371619124cac00883beb6abb
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511577"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="3160d-103">Włącz żądania między źródłami (CORS) w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3160d-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3160d-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3160d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3160d-105">W tym artykule pokazano, jak włączyć funkcję CORS w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3160d-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="3160d-106">Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która była obsługiwana przez stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3160d-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="3160d-107">To ograniczenie jest nazywane *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="3160d-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="3160d-108">Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="3160d-109">Czasami możesz chcieć zezwolić innym lokacjom na wykonywanie żądań między źródłami do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="3160d-110">Aby uzyskać więcej informacji, zobacz [artykuł CORS firmy Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="3160d-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="3160d-111">[Współużytkowanie zasobów między źródłami](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="3160d-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="3160d-112">Jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="3160d-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="3160d-113">**Nie** jest funkcją zabezpieczeń, funkcja CORS osłabi zabezpieczenia.</span><span class="sxs-lookup"><span data-stu-id="3160d-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="3160d-114">Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="3160d-115">Aby uzyskać więcej informacji, zobacz [jak działa mechanizm CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="3160d-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="3160d-116">Zezwala serwerowi jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych.</span><span class="sxs-lookup"><span data-stu-id="3160d-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="3160d-117">Jest bezpieczniejsze i bardziej elastyczne niż wcześniejsze techniki, takie jak [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="3160d-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="3160d-118">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3160d-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="3160d-119">To samo źródło</span><span class="sxs-lookup"><span data-stu-id="3160d-119">Same origin</span></span>

<span data-ttu-id="3160d-120">Dwa adresy URL mają te same źródła, jeśli mają identyczne schematy, hosty i porty ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="3160d-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="3160d-121">Te dwa adresy URL mają te same źródła:</span><span class="sxs-lookup"><span data-stu-id="3160d-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="3160d-122">Te adresy URL mają różne źródła niż poprzednie dwa adresy URL:</span><span class="sxs-lookup"><span data-stu-id="3160d-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="3160d-123">`https://example.net` &ndash; innej domeny</span><span class="sxs-lookup"><span data-stu-id="3160d-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="3160d-124">`https://www.example.com/foo.html` &ndash; inną poddomeną</span><span class="sxs-lookup"><span data-stu-id="3160d-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="3160d-125">`http://example.com/foo.html` &ndash; innego schematu</span><span class="sxs-lookup"><span data-stu-id="3160d-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="3160d-126">`https://example.com:9000/foo.html` &ndash; inny port</span><span class="sxs-lookup"><span data-stu-id="3160d-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="3160d-127">Program Internet Explorer nie traktuje portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="3160d-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="3160d-128">CORS z nazwanymi zasadami i programem pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="3160d-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="3160d-129">Oprogramowanie pośredniczące CORS obsługuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="3160d-130">Poniższy kod włącza mechanizm CORS dla całej aplikacji z określonym źródłem:</span><span class="sxs-lookup"><span data-stu-id="3160d-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="3160d-131">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="3160d-131">The preceding code:</span></span>

* <span data-ttu-id="3160d-132">Ustawia nazwę zasad na "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="3160d-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="3160d-133">Nazwa zasad jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="3160d-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="3160d-134">Wywołuje metodę rozszerzenia <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, która umożliwia mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="3160d-135">Wywołuje <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> z [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="3160d-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="3160d-136">Wyrażenie lambda przyjmuje obiekt <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="3160d-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="3160d-137">[Opcje konfiguracji](#cors-policy-options), takie jak `WithOrigins`, zostały opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="3160d-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="3160d-138">Wywołanie metody <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> dodaje usługi CORS do kontenera usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3160d-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="3160d-139">Aby uzyskać więcej informacji, zobacz [Opcje zasad CORS](#cpo) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="3160d-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="3160d-140">Metoda <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> może łączyć metody łańcucha, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="3160d-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="3160d-141">Uwaga: adres URL **nie** może zawierać końcowego ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="3160d-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="3160d-142">Jeśli adres URL kończy się `/`, porównanie zwraca `false` i żaden nagłówek nie zostanie zwrócony.</span><span class="sxs-lookup"><span data-stu-id="3160d-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="3160d-143">Zastosuj zasady CORS do wszystkich punktów końcowych</span><span class="sxs-lookup"><span data-stu-id="3160d-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="3160d-144">Poniższy kod dotyczy zasad CORS dla wszystkich punktów końcowych aplikacji za pośrednictwem oprogramowania do obsługi mechanizmu CORS:</span><span class="sxs-lookup"><span data-stu-id="3160d-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code omitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code omitted.
}
```

> [!WARNING]
> <span data-ttu-id="3160d-145">W przypadku routingu punktu końcowego oprogramowanie do obsługi mechanizmu CORS musi być skonfigurowane do wykonywania między wywołaniami `UseRouting` i `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="3160d-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="3160d-146">Niepoprawna konfiguracja spowoduje, że oprogramowanie pośredniczące przestanie działać poprawnie.</span><span class="sxs-lookup"><span data-stu-id="3160d-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

<span data-ttu-id="3160d-147">Zobacz [Włączanie mechanizmu CORS w Razor Pages, kontrolerach i metodach akcji](#ecors) , aby zastosować zasady CORS na poziomie strony/kontrolera/akcji.</span><span class="sxs-lookup"><span data-stu-id="3160d-147">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="3160d-148">Aby uzyskać instrukcje dotyczące testowania poprzedniego kodu, zobacz temat [CORS testów](#test) .</span><span class="sxs-lookup"><span data-stu-id="3160d-148">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="3160d-149">Włączanie mechanizmu CORS przy użyciu routingu punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="3160d-149">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="3160d-150">Za pomocą routingu punktu końcowego można włączyć funkcję CORS dla poszczególnych punktów końcowych przy użyciu zestawu `RequireCors` metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="3160d-150">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="3160d-151">Podobnie można również włączyć funkcję CORS dla wszystkich kontrolerów:</span><span class="sxs-lookup"><span data-stu-id="3160d-151">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="3160d-152">Włączanie mechanizmu CORS z atrybutami</span><span class="sxs-lookup"><span data-stu-id="3160d-152">Enable CORS with attributes</span></span>

<span data-ttu-id="3160d-153">Atrybut [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) stanowi alternatywę do stosowania mechanizmu CORS globalnie.</span><span class="sxs-lookup"><span data-stu-id="3160d-153">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="3160d-154">Atrybut `[EnableCors]` włącza funkcję CORS dla wybranych punktów końcowych, a nie wszystkich punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="3160d-154">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="3160d-155">Użyj `[EnableCors]`, aby określić zasady domyślne i `[EnableCors("{Policy String}")]`, aby określić zasady.</span><span class="sxs-lookup"><span data-stu-id="3160d-155">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="3160d-156">Atrybut `[EnableCors]` można zastosować do:</span><span class="sxs-lookup"><span data-stu-id="3160d-156">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="3160d-157">`PageModel` strony Razor</span><span class="sxs-lookup"><span data-stu-id="3160d-157">Razor Page `PageModel`</span></span>
* <span data-ttu-id="3160d-158">Kontroler</span><span class="sxs-lookup"><span data-stu-id="3160d-158">Controller</span></span>
* <span data-ttu-id="3160d-159">Metoda akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="3160d-159">Controller action method</span></span>

<span data-ttu-id="3160d-160">Możesz zastosować różne zasady do kontrolera/strony-model/akcja z atrybutem `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="3160d-160">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="3160d-161">Gdy atrybut `[EnableCors]` jest stosowany do metody controllers/model/akcja, a funkcja CORS jest włączona w oprogramowaniu pośredniczącym, obie zasady są stosowane.</span><span class="sxs-lookup"><span data-stu-id="3160d-161">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="3160d-162">Zalecamy łączenie zasad.</span><span class="sxs-lookup"><span data-stu-id="3160d-162">We recommend against combining policies.</span></span> <span data-ttu-id="3160d-163">Użyj atrybutu `[EnableCors]` lub oprogramowania pośredniczącego, a nie obu w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-163">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="3160d-164">Poniższy kod stosuje różne zasady do każdej metody:</span><span class="sxs-lookup"><span data-stu-id="3160d-164">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="3160d-165">Poniższy kod tworzy domyślne zasady CORS i zasady o nazwie `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="3160d-165">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="3160d-166">Wyłącz funkcję CORS</span><span class="sxs-lookup"><span data-stu-id="3160d-166">Disable CORS</span></span>

<span data-ttu-id="3160d-167">Atrybut [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) wyłącza funkcję CORS dla kontrolera/strony-modelu/akcji.</span><span class="sxs-lookup"><span data-stu-id="3160d-167">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="3160d-168">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="3160d-168">CORS policy options</span></span>

<span data-ttu-id="3160d-169">W tej sekcji opisano różne opcje, które można ustawić w zasadach CORS:</span><span class="sxs-lookup"><span data-stu-id="3160d-169">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="3160d-170">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="3160d-170">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="3160d-171">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="3160d-171">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="3160d-172">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="3160d-172">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="3160d-173">Ustawianie nagłówków uwidocznionych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3160d-173">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="3160d-174">Poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="3160d-174">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="3160d-175">Ustaw czas wygaśnięcia inspekcji wstępnej</span><span class="sxs-lookup"><span data-stu-id="3160d-175">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="3160d-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> jest wywoływana w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3160d-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3160d-177">W przypadku niektórych opcji warto przeczytać najpierw sekcję [jak działa mechanizm CORS](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="3160d-177">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="3160d-178">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="3160d-178">Set the allowed origins</span></span>

<span data-ttu-id="3160d-179">&ndash; <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> zezwala na żądania CORS ze wszystkich źródeł z dowolnym schematem (`http` lub `https`).</span><span class="sxs-lookup"><span data-stu-id="3160d-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="3160d-180">`AllowAnyOrigin` jest niezabezpieczona, ponieważ *Każda witryna sieci Web* może wprowadzać żądania między źródłami do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-180">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="3160d-181">Określanie `AllowAnyOrigin` i `AllowCredentials` jest niebezpieczną konfiguracją i może skutkować fałszerstwem żądania między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="3160d-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="3160d-182">Usługa CORS zwraca nieprawidłową odpowiedź CORS, gdy aplikacja jest skonfigurowana przy użyciu obu metod.</span><span class="sxs-lookup"><span data-stu-id="3160d-182">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="3160d-183">`AllowAnyOrigin` ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="3160d-183">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="3160d-184">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="3160d-184">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="3160d-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; ustawia właściwość <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> zasad jako funkcję, która umożliwia pochodzenie do dopasowania do skonfigurowanej domeny z symbolami wieloznacznymi podczas oceniania, czy pochodzenie jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="3160d-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="3160d-186">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="3160d-186">Set the allowed HTTP methods</span></span>

<span data-ttu-id="3160d-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="3160d-188">Zezwala na dowolną metodę HTTP:</span><span class="sxs-lookup"><span data-stu-id="3160d-188">Allows any HTTP method:</span></span>
* <span data-ttu-id="3160d-189">Ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="3160d-189">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="3160d-190">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="3160d-190">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="3160d-191">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="3160d-191">Set the allowed request headers</span></span>

<span data-ttu-id="3160d-192">Aby zezwolić na wysyłanie określonych nagłówków w żądaniu CORS, nazywanymi *nagłówkami żądań autora*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:</span><span class="sxs-lookup"><span data-stu-id="3160d-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="3160d-193">Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="3160d-194">To ustawienie ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="3160d-194">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="3160d-195">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="3160d-195">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>


<span data-ttu-id="3160d-196">Zasady oprogramowania CORS są zgodne z określonymi nagłówkami określonymi przez `WithHeaders` jest możliwe tylko wtedy, gdy nagłówki wysyłane w `Access-Control-Request-Headers` dokładnie pasują do nagłówków określonych w `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="3160d-196">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="3160d-197">Na przykład rozważ zastosowanie skonfigurowanej aplikacji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3160d-197">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="3160d-198">Oprogramowanie pośredniczące CORS odrzuca żądanie wstępne z następującym nagłówkiem żądania, ponieważ `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) nie znajduje się na liście `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="3160d-198">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="3160d-199">Aplikacja zwraca odpowiedź *200 OK* , ale nie wysyła nagłówków CORS z powrotem.</span><span class="sxs-lookup"><span data-stu-id="3160d-199">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="3160d-200">W związku z tym przeglądarka nie próbuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-200">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="3160d-201">Ustawianie nagłówków uwidocznionych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3160d-201">Set the exposed response headers</span></span>

<span data-ttu-id="3160d-202">Domyślnie przeglądarka nie ujawnia wszystkich nagłówków odpowiedzi dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-202">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="3160d-203">Aby uzyskać więcej informacji, zobacz temat [udostępnianie zasobów między źródłami W3C (terminologia): prosty nagłówek odpowiedzi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="3160d-203">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="3160d-204">Domyślnie dostępne są nagłówki odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="3160d-204">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="3160d-205">Specyfikacja CORS wywołuje te nagłówki *proste odpowiedzi*.</span><span class="sxs-lookup"><span data-stu-id="3160d-205">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="3160d-206">Aby udostępnić inne nagłówki dla aplikacji, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-206">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="3160d-207">Poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="3160d-207">Credentials in cross-origin requests</span></span>

<span data-ttu-id="3160d-208">Poświadczenia wymagają specjalnej obsługi w żądaniu CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-208">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="3160d-209">Domyślnie przeglądarka nie wysyła poświadczeń z żądaniem między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-209">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="3160d-210">Poświadczenia obejmują pliki cookie i schematy uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3160d-210">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="3160d-211">Aby wysłać poświadczenia z żądaniem między źródłami, klient musi ustawić `XMLHttpRequest.withCredentials`, aby `true`.</span><span class="sxs-lookup"><span data-stu-id="3160d-211">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="3160d-212">Używanie `XMLHttpRequest` bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="3160d-212">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="3160d-213">Za pomocą jQuery:</span><span class="sxs-lookup"><span data-stu-id="3160d-213">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="3160d-214">Za pomocą [interfejsu API pobierania](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="3160d-214">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="3160d-215">Serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="3160d-215">The server must allow the credentials.</span></span> <span data-ttu-id="3160d-216">Aby zezwolić na poświadczenia między źródłami, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-216">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="3160d-217">Odpowiedź HTTP zawiera nagłówek `Access-Control-Allow-Credentials`, który informuje przeglądarkę, że serwer zezwala na poświadczenia dla żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-217">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="3160d-218">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka `Access-Control-Allow-Credentials`, przeglądarka nie ujawnia odpowiedzi aplikacji, a żądanie między źródłami nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="3160d-218">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="3160d-219">Zezwalanie na poświadczenia między źródłami stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="3160d-219">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="3160d-220">Witryna sieci Web w innej domenie może wysyłać poświadczenia zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3160d-220">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="3160d-221">Specyfikacja CORS określa również, że źródła ustawień dla `"*"` (wszystkie źródła) są nieprawidłowe, jeśli nagłówek `Access-Control-Allow-Credentials` jest obecny.</span><span class="sxs-lookup"><span data-stu-id="3160d-221">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="3160d-222">Żądania wstępnego lotu</span><span class="sxs-lookup"><span data-stu-id="3160d-222">Preflight requests</span></span>

<span data-ttu-id="3160d-223">W przypadku niektórych żądań CORS przeglądarka wysyła dodatkowe żądanie przed wykonaniem rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="3160d-223">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="3160d-224">To żądanie jest nazywane *żądaniem wstępnym*.</span><span class="sxs-lookup"><span data-stu-id="3160d-224">This request is called a *preflight request*.</span></span> <span data-ttu-id="3160d-225">Jeśli spełnione są następujące warunki, przeglądarka może pominąć żądanie wstępne:</span><span class="sxs-lookup"><span data-stu-id="3160d-225">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="3160d-226">Metoda żądania ma wartość GET, główna lub OPUBLIKOWANa.</span><span class="sxs-lookup"><span data-stu-id="3160d-226">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="3160d-227">Aplikacja nie ustawia nagłówków żądań innych niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`lub `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="3160d-227">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="3160d-228">Nagłówek `Content-Type`, jeśli jest ustawiony, ma jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="3160d-228">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="3160d-229">Reguła dotycząca nagłówków żądań ustawiona dla żądania klienta dotyczy nagłówków, które są ustawiane przez aplikację przez wywołanie `setRequestHeader` na obiekcie `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="3160d-229">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="3160d-230">Specyfikacja CORS wywołuje *nagłówki żądania autora*tych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="3160d-230">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="3160d-231">Reguła nie ma zastosowania do nagłówków, które można ustawić w przeglądarce, takich jak `User-Agent`, `Host`lub `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="3160d-231">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="3160d-232">Poniżej znajduje się przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="3160d-232">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="3160d-233">Żądanie przed inspekcją używa metody HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="3160d-233">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="3160d-234">Zawiera dwa specjalne nagłówki:</span><span class="sxs-lookup"><span data-stu-id="3160d-234">It includes two special headers:</span></span>

* <span data-ttu-id="3160d-235">`Access-Control-Request-Method`: metoda HTTP, która będzie używana dla rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="3160d-235">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="3160d-236">`Access-Control-Request-Headers`: lista nagłówków żądań, które aplikacja ustawia na rzeczywiste żądanie.</span><span class="sxs-lookup"><span data-stu-id="3160d-236">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="3160d-237">Jak wspomniano wcześniej, nie obejmuje to nagłówków, które są ustawiane przez przeglądarkę, takich jak `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="3160d-237">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="3160d-238">Żądanie inspekcji wstępnej CORS może zawierać nagłówek `Access-Control-Request-Headers` wskazujący serwerowi nagłówki wysyłane z rzeczywistym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="3160d-238">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="3160d-239">Aby zezwolić na określone nagłówki, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-239">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="3160d-240">Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-240">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="3160d-241">Przeglądarki nie są w pełni spójne w sposób, w jaki ustawili `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="3160d-241">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="3160d-242">Jeśli ustawisz nagłówki na inne niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), należy uwzględnić co najmniej `Accept`, `Content-Type`i `Origin`oraz wszystkie niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="3160d-242">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="3160d-243">Poniżej znajduje się Przykładowa odpowiedź na żądanie inspekcji wstępnej (przy założeniu, że serwer zezwala na żądanie):</span><span class="sxs-lookup"><span data-stu-id="3160d-243">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="3160d-244">Odpowiedź zawiera nagłówek `Access-Control-Allow-Methods`, który zawiera listę dozwolonych metod i opcjonalnie nagłówek `Access-Control-Allow-Headers`, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="3160d-244">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="3160d-245">W przypadku pomyślnego przeprowadzenia żądania wstępnego przeglądarka wyśle rzeczywiste żądanie.</span><span class="sxs-lookup"><span data-stu-id="3160d-245">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="3160d-246">Jeśli żądanie wstępne nie zostanie odrzucone, aplikacja zwróci odpowiedź *200 OK* , ale nie wyśle nagłówków CORS z powrotem.</span><span class="sxs-lookup"><span data-stu-id="3160d-246">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="3160d-247">W związku z tym przeglądarka nie próbuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-247">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="3160d-248">Ustaw czas wygaśnięcia inspekcji wstępnej</span><span class="sxs-lookup"><span data-stu-id="3160d-248">Set the preflight expiration time</span></span>

<span data-ttu-id="3160d-249">Nagłówek `Access-Control-Max-Age` określa, jak długo odpowiedź na żądanie inspekcji wstępnej może być buforowana.</span><span class="sxs-lookup"><span data-stu-id="3160d-249">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="3160d-250">Aby ustawić ten nagłówek, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-250">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="3160d-251">Jak działa mechanizm CORS</span><span class="sxs-lookup"><span data-stu-id="3160d-251">How CORS works</span></span>

<span data-ttu-id="3160d-252">W tej sekcji opisano, co się dzieje w żądaniu [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) na poziomie komunikatów http.</span><span class="sxs-lookup"><span data-stu-id="3160d-252">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="3160d-253">Mechanizm CORS **nie** jest funkcją zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3160d-253">CORS is **not** a security feature.</span></span> <span data-ttu-id="3160d-254">CORS jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="3160d-254">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="3160d-255">Na przykład złośliwy aktor może użyć [zapobiegania skryptom między lokacjami (XSS)](xref:security/cross-site-scripting) względem witryny i wykonać żądanie między lokacjami w celu wykraść informacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-255">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="3160d-256">Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-256">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="3160d-257">Aby wymusić mechanizm CORS, należy do klienta (przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="3160d-257">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="3160d-258">Serwer wykonuje żądanie i zwraca odpowiedź, jest to klient, który zwraca błąd i blokuje odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="3160d-258">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="3160d-259">Na przykład w dowolnym z poniższych narzędzi zostanie wyświetlona odpowiedź serwera:</span><span class="sxs-lookup"><span data-stu-id="3160d-259">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="3160d-260">Fiddler</span><span class="sxs-lookup"><span data-stu-id="3160d-260">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="3160d-261">Postman</span><span class="sxs-lookup"><span data-stu-id="3160d-261">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="3160d-262">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="3160d-262">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="3160d-263">Przeglądarka sieci Web, wprowadzając adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="3160d-263">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="3160d-264">Jest to sposób, aby serwer zezwalał przeglądarce na wykonywanie żądania [interfejsu API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) lub pobierania między źródłami, które w przeciwnym razie byłoby zabronione.</span><span class="sxs-lookup"><span data-stu-id="3160d-264">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="3160d-265">Przeglądarki (bez CORS) nie mogą wykonywać żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-265">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="3160d-266">Przed zastosowaniem mechanizmu CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) został użyty do obejścia tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="3160d-266">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="3160d-267">JSONP nie używa XHR, aby otrzymać odpowiedź, używa znacznika `<script>`.</span><span class="sxs-lookup"><span data-stu-id="3160d-267">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="3160d-268">Skrypty mogą być ładowane między różnymi źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-268">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="3160d-269">[Specyfikacja CORS](https://www.w3.org/TR/cors/) wprowadziła kilka nowych nagłówków HTTP, które umożliwiają żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-269">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="3160d-270">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia te nagłówki automatycznie dla żądań cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="3160d-270">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="3160d-271">Niestandardowy kod JavaScript nie jest wymagany do włączenia mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-271">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="3160d-272">Poniżej przedstawiono przykład żądania między źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="3160d-272">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="3160d-273">Nagłówek `Origin` zapewnia domenę lokacji, która żąda żądania.</span><span class="sxs-lookup"><span data-stu-id="3160d-273">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="3160d-274">Nagłówek `Origin` jest wymagany i musi się różnić od hosta.</span><span class="sxs-lookup"><span data-stu-id="3160d-274">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="3160d-275">Jeśli serwer zezwala na żądanie, ustawia nagłówek `Access-Control-Allow-Origin` w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3160d-275">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="3160d-276">Wartość tego nagłówka jest zgodna z nagłówkiem `Origin` z żądania lub jest wartością symbolu wieloznacznego `"*"`, co oznacza, że wszystkie źródła są dozwolone:</span><span class="sxs-lookup"><span data-stu-id="3160d-276">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="3160d-277">Jeśli odpowiedź nie zawiera nagłówka `Access-Control-Allow-Origin`, żądanie między źródłami nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="3160d-277">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="3160d-278">W programie przeglądarka nie zezwala na żądanie.</span><span class="sxs-lookup"><span data-stu-id="3160d-278">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="3160d-279">Nawet jeśli serwer zwróci pomyślną odpowiedź, przeglądarka nie udostępni odpowiedzi dla aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="3160d-279">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="3160d-280">Testowanie CORS</span><span class="sxs-lookup"><span data-stu-id="3160d-280">Test CORS</span></span>

<span data-ttu-id="3160d-281">Aby przetestować CORS:</span><span class="sxs-lookup"><span data-stu-id="3160d-281">To test CORS:</span></span>

1. <span data-ttu-id="3160d-282">[Utwórz projekt interfejsu API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="3160d-282">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="3160d-283">Możesz również [pobrać przykład](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="3160d-283">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="3160d-284">Włącz funkcję CORS przy użyciu jednego z metod w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="3160d-284">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="3160d-285">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3160d-285">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="3160d-286">`WithOrigins("https://localhost:<port>");` należy używać tylko do testowania przykładowej aplikacji podobnej do [przykładowego kodu pobierania](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="3160d-286">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="3160d-287">Utwórz projekt aplikacji sieci Web (Razor Pages lub MVC).</span><span class="sxs-lookup"><span data-stu-id="3160d-287">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="3160d-288">Przykład używa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3160d-288">The sample uses Razor Pages.</span></span> <span data-ttu-id="3160d-289">Aplikację sieci Web można utworzyć w tym samym rozwiązaniu co projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="3160d-289">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="3160d-290">Dodaj następujący wyróżniony kod do pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3160d-290">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="3160d-291">W poprzednim kodzie Zastąp `url: 'https://<web app>.azurewebsites.net/api/values/1',` adresem URL wdrożonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-291">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="3160d-292">Wdróż projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="3160d-292">Deploy the API project.</span></span> <span data-ttu-id="3160d-293">Na przykład [Wdróż na platformie Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="3160d-293">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="3160d-294">Uruchom aplikację Razor Pages lub MVC na pulpicie, a następnie kliknij przycisk **Testuj** .</span><span class="sxs-lookup"><span data-stu-id="3160d-294">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="3160d-295">Użyj narzędzi F12, aby przejrzeć komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="3160d-295">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="3160d-296">Usuń pochodzenie hosta lokalnego z `WithOrigins` i Wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="3160d-296">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="3160d-297">Alternatywnie Uruchom aplikację kliencką z innym portem.</span><span class="sxs-lookup"><span data-stu-id="3160d-297">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="3160d-298">Na przykład uruchom polecenie z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3160d-298">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="3160d-299">Przetestuj za pomocą aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="3160d-299">Test with the client app.</span></span> <span data-ttu-id="3160d-300">Błędy funkcji CORS zwracają błąd, ale komunikat o błędzie nie jest dostępny dla języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3160d-300">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="3160d-301">Aby wyświetlić błąd, Użyj karty konsola w narzędziach F12.</span><span class="sxs-lookup"><span data-stu-id="3160d-301">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="3160d-302">W zależności od przeglądarki pojawia się błąd (w konsoli narzędzia F12) podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="3160d-302">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="3160d-303">Korzystanie z przeglądarki Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="3160d-303">Using Microsoft Edge:</span></span>

     <span data-ttu-id="3160d-304">**SEC7120: [CORS] Źródło `https://localhost:44375` nie znalazło `https://localhost:44375` w nagłówku odpowiedzi "Access-Control-Allow-Origin" dla zasobu Cross-Origin w `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="3160d-304">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="3160d-305">Korzystanie z programu Chrome:</span><span class="sxs-lookup"><span data-stu-id="3160d-305">Using Chrome:</span></span>

     <span data-ttu-id="3160d-306">**Dostęp do elementu XMLHttpRequest na `https://webapi.azurewebsites.net/api/values/1` ze źródła `https://localhost:44375` został zablokowany przez zasady CORS: brak nagłówka "Access-Control-Allow-Origin" w żądanym zasobie.**</span><span class="sxs-lookup"><span data-stu-id="3160d-306">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="3160d-307">Punkty końcowe z obsługą mechanizmu CORS można testować za pomocą narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler) lub [Poster](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="3160d-307">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="3160d-308">W przypadku korzystania z narzędzia, Źródło żądania określone przez nagłówek `Origin` musi różnić się od hosta przyjmującego żądanie.</span><span class="sxs-lookup"><span data-stu-id="3160d-308">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="3160d-309">Jeśli żądanie nie jest *źródłem krzyżowe* na podstawie wartości nagłówka `Origin`:</span><span class="sxs-lookup"><span data-stu-id="3160d-309">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="3160d-310">Nie ma potrzeby przetwarzania żądania przez oprogramowanie pośredniczące CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-310">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="3160d-311">Nagłówki CORS nie są zwracane w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3160d-311">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="3160d-312">Mechanizm CORS w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="3160d-312">CORS in IIS</span></span>

<span data-ttu-id="3160d-313">W przypadku wdrażania w programie IIS należy uruchomić funkcję CORS przed uwierzytelnianiem systemu Windows, jeśli serwer nie jest skonfigurowany do zezwalania na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="3160d-313">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="3160d-314">Aby zapewnić obsługę tego scenariusza, należy zainstalować i skonfigurować [moduł CORS usług IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-314">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3160d-315">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3160d-315">Additional resources</span></span>

* [<span data-ttu-id="3160d-316">Współużytkowanie zasobów między źródłami (CORS)</span><span class="sxs-lookup"><span data-stu-id="3160d-316">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="3160d-317">Wprowadzenie do modułu CORS usług IIS</span><span class="sxs-lookup"><span data-stu-id="3160d-317">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3160d-318">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3160d-318">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3160d-319">W tym artykule pokazano, jak włączyć funkcję CORS w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3160d-319">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="3160d-320">Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która była obsługiwana przez stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3160d-320">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="3160d-321">To ograniczenie jest nazywane *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="3160d-321">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="3160d-322">Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-322">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="3160d-323">Czasami możesz chcieć zezwolić innym lokacjom na wykonywanie żądań między źródłami do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-323">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="3160d-324">Aby uzyskać więcej informacji, zobacz [artykuł CORS firmy Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="3160d-324">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="3160d-325">[Współużytkowanie zasobów między źródłami](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="3160d-325">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="3160d-326">Jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="3160d-326">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="3160d-327">**Nie** jest funkcją zabezpieczeń, funkcja CORS osłabi zabezpieczenia.</span><span class="sxs-lookup"><span data-stu-id="3160d-327">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="3160d-328">Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-328">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="3160d-329">Aby uzyskać więcej informacji, zobacz [jak działa mechanizm CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="3160d-329">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="3160d-330">Zezwala serwerowi jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych.</span><span class="sxs-lookup"><span data-stu-id="3160d-330">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="3160d-331">Jest bezpieczniejsze i bardziej elastyczne niż wcześniejsze techniki, takie jak [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="3160d-331">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="3160d-332">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3160d-332">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="3160d-333">To samo źródło</span><span class="sxs-lookup"><span data-stu-id="3160d-333">Same origin</span></span>

<span data-ttu-id="3160d-334">Dwa adresy URL mają te same źródła, jeśli mają identyczne schematy, hosty i porty ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="3160d-334">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="3160d-335">Te dwa adresy URL mają te same źródła:</span><span class="sxs-lookup"><span data-stu-id="3160d-335">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="3160d-336">Te adresy URL mają różne źródła niż poprzednie dwa adresy URL:</span><span class="sxs-lookup"><span data-stu-id="3160d-336">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="3160d-337">`https://example.net` &ndash; innej domeny</span><span class="sxs-lookup"><span data-stu-id="3160d-337">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="3160d-338">`https://www.example.com/foo.html` &ndash; inną poddomeną</span><span class="sxs-lookup"><span data-stu-id="3160d-338">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="3160d-339">`http://example.com/foo.html` &ndash; innego schematu</span><span class="sxs-lookup"><span data-stu-id="3160d-339">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="3160d-340">`https://example.com:9000/foo.html` &ndash; inny port</span><span class="sxs-lookup"><span data-stu-id="3160d-340">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="3160d-341">Program Internet Explorer nie traktuje portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="3160d-341">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="3160d-342">CORS z nazwanymi zasadami i programem pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="3160d-342">CORS with named policy and middleware</span></span>

<span data-ttu-id="3160d-343">Oprogramowanie pośredniczące CORS obsługuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-343">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="3160d-344">Poniższy kod włącza mechanizm CORS dla całej aplikacji z określonym źródłem:</span><span class="sxs-lookup"><span data-stu-id="3160d-344">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="3160d-345">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="3160d-345">The preceding code:</span></span>

* <span data-ttu-id="3160d-346">Ustawia nazwę zasad na "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="3160d-346">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="3160d-347">Nazwa zasad jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="3160d-347">The policy name is arbitrary.</span></span>
* <span data-ttu-id="3160d-348">Wywołuje metodę rozszerzenia <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, która umożliwia mechanizm CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-348">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="3160d-349">Wywołuje <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> z [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="3160d-349">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="3160d-350">Wyrażenie lambda przyjmuje obiekt <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="3160d-350">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="3160d-351">[Opcje konfiguracji](#cors-policy-options), takie jak `WithOrigins`, zostały opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="3160d-351">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="3160d-352">Wywołanie metody <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> dodaje usługi CORS do kontenera usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3160d-352">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="3160d-353">Aby uzyskać więcej informacji, zobacz [Opcje zasad CORS](#cpo) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="3160d-353">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="3160d-354">Metoda <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> może łączyć metody łańcucha, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="3160d-354">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="3160d-355">Uwaga: adres URL **nie** może zawierać końcowego ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="3160d-355">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="3160d-356">Jeśli adres URL kończy się `/`, porównanie zwraca `false` i żaden nagłówek nie zostanie zwrócony.</span><span class="sxs-lookup"><span data-stu-id="3160d-356">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="3160d-357">Poniższy kod dotyczy zasad CORS dla wszystkich punktów końcowych aplikacji za pośrednictwem oprogramowania do obsługi mechanizmu CORS:</span><span class="sxs-lookup"><span data-stu-id="3160d-357">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="3160d-358">Uwaga: przed `UseMvc`należy wywołać `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="3160d-358">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="3160d-359">Zobacz [Włączanie mechanizmu CORS w Razor Pages, kontrolerach i metodach akcji](#ecors) , aby zastosować zasady CORS na poziomie strony/kontrolera/akcji.</span><span class="sxs-lookup"><span data-stu-id="3160d-359">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="3160d-360">Aby uzyskać instrukcje dotyczące testowania poprzedniego kodu, zobacz temat [CORS testów](#test) .</span><span class="sxs-lookup"><span data-stu-id="3160d-360">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="3160d-361">Włączanie mechanizmu CORS z atrybutami</span><span class="sxs-lookup"><span data-stu-id="3160d-361">Enable CORS with attributes</span></span>

<span data-ttu-id="3160d-362">Atrybut [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) stanowi alternatywę do stosowania mechanizmu CORS globalnie.</span><span class="sxs-lookup"><span data-stu-id="3160d-362">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="3160d-363">Atrybut `[EnableCors]` włącza funkcję CORS dla wybranych punktów końcowych, a nie wszystkich punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="3160d-363">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="3160d-364">Użyj `[EnableCors]`, aby określić zasady domyślne i `[EnableCors("{Policy String}")]`, aby określić zasady.</span><span class="sxs-lookup"><span data-stu-id="3160d-364">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="3160d-365">Atrybut `[EnableCors]` można zastosować do:</span><span class="sxs-lookup"><span data-stu-id="3160d-365">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="3160d-366">`PageModel` strony Razor</span><span class="sxs-lookup"><span data-stu-id="3160d-366">Razor Page `PageModel`</span></span>
* <span data-ttu-id="3160d-367">Kontroler</span><span class="sxs-lookup"><span data-stu-id="3160d-367">Controller</span></span>
* <span data-ttu-id="3160d-368">Metoda akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="3160d-368">Controller action method</span></span>

<span data-ttu-id="3160d-369">Możesz zastosować różne zasady do kontrolera/strony-model/akcja z atrybutem `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="3160d-369">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="3160d-370">Gdy atrybut `[EnableCors]` jest stosowany do metody controllers/model/akcja, a funkcja CORS jest włączona w oprogramowaniu pośredniczącym, obie zasady są stosowane.</span><span class="sxs-lookup"><span data-stu-id="3160d-370">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="3160d-371">Zalecamy łączenie zasad.</span><span class="sxs-lookup"><span data-stu-id="3160d-371">We recommend against combining policies.</span></span> <span data-ttu-id="3160d-372">Użyj atrybutu `[EnableCors]` lub oprogramowania pośredniczącego, a nie obu w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-372">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="3160d-373">Poniższy kod stosuje różne zasady do każdej metody:</span><span class="sxs-lookup"><span data-stu-id="3160d-373">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="3160d-374">Poniższy kod tworzy domyślne zasady CORS i zasady o nazwie `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="3160d-374">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="3160d-375">Wyłącz funkcję CORS</span><span class="sxs-lookup"><span data-stu-id="3160d-375">Disable CORS</span></span>

<span data-ttu-id="3160d-376">Atrybut [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) wyłącza funkcję CORS dla kontrolera/strony-modelu/akcji.</span><span class="sxs-lookup"><span data-stu-id="3160d-376">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="3160d-377">Opcje zasad CORS</span><span class="sxs-lookup"><span data-stu-id="3160d-377">CORS policy options</span></span>

<span data-ttu-id="3160d-378">W tej sekcji opisano różne opcje, które można ustawić w zasadach CORS:</span><span class="sxs-lookup"><span data-stu-id="3160d-378">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="3160d-379">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="3160d-379">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="3160d-380">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="3160d-380">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="3160d-381">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="3160d-381">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="3160d-382">Ustawianie nagłówków uwidocznionych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3160d-382">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="3160d-383">Poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="3160d-383">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="3160d-384">Ustaw czas wygaśnięcia inspekcji wstępnej</span><span class="sxs-lookup"><span data-stu-id="3160d-384">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="3160d-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> jest wywoływana w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3160d-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3160d-386">W przypadku niektórych opcji warto przeczytać najpierw sekcję [jak działa mechanizm CORS](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="3160d-386">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="3160d-387">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="3160d-387">Set the allowed origins</span></span>

<span data-ttu-id="3160d-388">&ndash; <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> zezwala na żądania CORS ze wszystkich źródeł z dowolnym schematem (`http` lub `https`).</span><span class="sxs-lookup"><span data-stu-id="3160d-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="3160d-389">`AllowAnyOrigin` jest niezabezpieczona, ponieważ *Każda witryna sieci Web* może wprowadzać żądania między źródłami do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-389">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="3160d-390">Określanie `AllowAnyOrigin` i `AllowCredentials` jest niebezpieczną konfiguracją i może skutkować fałszerstwem żądania między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="3160d-390">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="3160d-391">W przypadku bezpiecznej aplikacji należy określić dokładną listę źródeł, jeśli klient musi autoryzować sam do uzyskiwania dostępu do zasobów serwera.</span><span class="sxs-lookup"><span data-stu-id="3160d-391">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="3160d-392">`AllowAnyOrigin` ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="3160d-392">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="3160d-393">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="3160d-393">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="3160d-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; ustawia właściwość <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> zasad jako funkcję, która umożliwia pochodzenie do dopasowania do skonfigurowanej domeny z symbolami wieloznacznymi podczas oceniania, czy pochodzenie jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="3160d-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="3160d-395">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="3160d-395">Set the allowed HTTP methods</span></span>

<span data-ttu-id="3160d-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="3160d-397">Zezwala na dowolną metodę HTTP:</span><span class="sxs-lookup"><span data-stu-id="3160d-397">Allows any HTTP method:</span></span>
* <span data-ttu-id="3160d-398">Ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="3160d-398">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="3160d-399">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="3160d-399">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="3160d-400">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="3160d-400">Set the allowed request headers</span></span>

<span data-ttu-id="3160d-401">Aby zezwolić na wysyłanie określonych nagłówków w żądaniu CORS, nazywanymi *nagłówkami żądań autora*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:</span><span class="sxs-lookup"><span data-stu-id="3160d-401">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="3160d-402">Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-402">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="3160d-403">To ustawienie ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="3160d-403">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="3160d-404">Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="3160d-404">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="3160d-405">Oprogramowanie pośredniczące CORS zawsze umożliwia wysyłanie czterech nagłówków w `Access-Control-Request-Headers` niezależnie od wartości skonfigurowanych w CorsPolicy. Heads.</span><span class="sxs-lookup"><span data-stu-id="3160d-405">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="3160d-406">Ta lista nagłówków obejmuje:</span><span class="sxs-lookup"><span data-stu-id="3160d-406">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="3160d-407">Na przykład rozważ zastosowanie skonfigurowanej aplikacji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3160d-407">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="3160d-408">Oprogramowanie pośredniczące CORS pomyślnie reaguje na żądanie inspekcji wstępnej z następującym nagłówkiem żądania, ponieważ `Content-Language` jest zawsze listy dozwolonych:</span><span class="sxs-lookup"><span data-stu-id="3160d-408">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="3160d-409">Ustawianie nagłówków uwidocznionych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3160d-409">Set the exposed response headers</span></span>

<span data-ttu-id="3160d-410">Domyślnie przeglądarka nie ujawnia wszystkich nagłówków odpowiedzi dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-410">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="3160d-411">Aby uzyskać więcej informacji, zobacz temat [udostępnianie zasobów między źródłami W3C (terminologia): prosty nagłówek odpowiedzi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="3160d-411">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="3160d-412">Domyślnie dostępne są nagłówki odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="3160d-412">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="3160d-413">Specyfikacja CORS wywołuje te nagłówki *proste odpowiedzi*.</span><span class="sxs-lookup"><span data-stu-id="3160d-413">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="3160d-414">Aby udostępnić inne nagłówki dla aplikacji, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-414">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="3160d-415">Poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="3160d-415">Credentials in cross-origin requests</span></span>

<span data-ttu-id="3160d-416">Poświadczenia wymagają specjalnej obsługi w żądaniu CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-416">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="3160d-417">Domyślnie przeglądarka nie wysyła poświadczeń z żądaniem między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-417">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="3160d-418">Poświadczenia obejmują pliki cookie i schematy uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3160d-418">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="3160d-419">Aby wysłać poświadczenia z żądaniem między źródłami, klient musi ustawić `XMLHttpRequest.withCredentials`, aby `true`.</span><span class="sxs-lookup"><span data-stu-id="3160d-419">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="3160d-420">Używanie `XMLHttpRequest` bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="3160d-420">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="3160d-421">Za pomocą jQuery:</span><span class="sxs-lookup"><span data-stu-id="3160d-421">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="3160d-422">Za pomocą [interfejsu API pobierania](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="3160d-422">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="3160d-423">Serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="3160d-423">The server must allow the credentials.</span></span> <span data-ttu-id="3160d-424">Aby zezwolić na poświadczenia między źródłami, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-424">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="3160d-425">Odpowiedź HTTP zawiera nagłówek `Access-Control-Allow-Credentials`, który informuje przeglądarkę, że serwer zezwala na poświadczenia dla żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-425">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="3160d-426">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka `Access-Control-Allow-Credentials`, przeglądarka nie ujawnia odpowiedzi aplikacji, a żądanie między źródłami nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="3160d-426">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="3160d-427">Zezwalanie na poświadczenia między źródłami stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="3160d-427">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="3160d-428">Witryna sieci Web w innej domenie może wysyłać poświadczenia zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3160d-428">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="3160d-429">Specyfikacja CORS określa również, że źródła ustawień dla `"*"` (wszystkie źródła) są nieprawidłowe, jeśli nagłówek `Access-Control-Allow-Credentials` jest obecny.</span><span class="sxs-lookup"><span data-stu-id="3160d-429">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="3160d-430">Żądania wstępnego lotu</span><span class="sxs-lookup"><span data-stu-id="3160d-430">Preflight requests</span></span>

<span data-ttu-id="3160d-431">W przypadku niektórych żądań CORS przeglądarka wysyła dodatkowe żądanie przed wykonaniem rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="3160d-431">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="3160d-432">To żądanie jest nazywane *żądaniem wstępnym*.</span><span class="sxs-lookup"><span data-stu-id="3160d-432">This request is called a *preflight request*.</span></span> <span data-ttu-id="3160d-433">Jeśli spełnione są następujące warunki, przeglądarka może pominąć żądanie wstępne:</span><span class="sxs-lookup"><span data-stu-id="3160d-433">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="3160d-434">Metoda żądania ma wartość GET, główna lub OPUBLIKOWANa.</span><span class="sxs-lookup"><span data-stu-id="3160d-434">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="3160d-435">Aplikacja nie ustawia nagłówków żądań innych niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`lub `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="3160d-435">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="3160d-436">Nagłówek `Content-Type`, jeśli jest ustawiony, ma jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="3160d-436">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="3160d-437">Reguła dotycząca nagłówków żądań ustawiona dla żądania klienta dotyczy nagłówków, które są ustawiane przez aplikację przez wywołanie `setRequestHeader` na obiekcie `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="3160d-437">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="3160d-438">Specyfikacja CORS wywołuje *nagłówki żądania autora*tych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="3160d-438">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="3160d-439">Reguła nie ma zastosowania do nagłówków, które można ustawić w przeglądarce, takich jak `User-Agent`, `Host`lub `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="3160d-439">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="3160d-440">Poniżej znajduje się przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="3160d-440">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="3160d-441">Żądanie przed inspekcją używa metody HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="3160d-441">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="3160d-442">Zawiera dwa specjalne nagłówki:</span><span class="sxs-lookup"><span data-stu-id="3160d-442">It includes two special headers:</span></span>

* <span data-ttu-id="3160d-443">`Access-Control-Request-Method`: metoda HTTP, która będzie używana dla rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="3160d-443">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="3160d-444">`Access-Control-Request-Headers`: lista nagłówków żądań, które aplikacja ustawia na rzeczywiste żądanie.</span><span class="sxs-lookup"><span data-stu-id="3160d-444">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="3160d-445">Jak wspomniano wcześniej, nie obejmuje to nagłówków, które są ustawiane przez przeglądarkę, takich jak `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="3160d-445">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="3160d-446">Żądanie inspekcji wstępnej CORS może zawierać nagłówek `Access-Control-Request-Headers` wskazujący serwerowi nagłówki wysyłane z rzeczywistym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="3160d-446">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="3160d-447">Aby zezwolić na określone nagłówki, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-447">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="3160d-448">Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-448">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="3160d-449">Przeglądarki nie są w pełni spójne w sposób, w jaki ustawili `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="3160d-449">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="3160d-450">Jeśli ustawisz nagłówki na inne niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), należy uwzględnić co najmniej `Accept`, `Content-Type`i `Origin`oraz wszystkie niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="3160d-450">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="3160d-451">Poniżej znajduje się Przykładowa odpowiedź na żądanie inspekcji wstępnej (przy założeniu, że serwer zezwala na żądanie):</span><span class="sxs-lookup"><span data-stu-id="3160d-451">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="3160d-452">Odpowiedź zawiera nagłówek `Access-Control-Allow-Methods`, który zawiera listę dozwolonych metod i opcjonalnie nagłówek `Access-Control-Allow-Headers`, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="3160d-452">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="3160d-453">W przypadku pomyślnego przeprowadzenia żądania wstępnego przeglądarka wyśle rzeczywiste żądanie.</span><span class="sxs-lookup"><span data-stu-id="3160d-453">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="3160d-454">Jeśli żądanie wstępne nie zostanie odrzucone, aplikacja zwróci odpowiedź *200 OK* , ale nie wyśle nagłówków CORS z powrotem.</span><span class="sxs-lookup"><span data-stu-id="3160d-454">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="3160d-455">W związku z tym przeglądarka nie próbuje żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-455">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="3160d-456">Ustaw czas wygaśnięcia inspekcji wstępnej</span><span class="sxs-lookup"><span data-stu-id="3160d-456">Set the preflight expiration time</span></span>

<span data-ttu-id="3160d-457">Nagłówek `Access-Control-Max-Age` określa, jak długo odpowiedź na żądanie inspekcji wstępnej może być buforowana.</span><span class="sxs-lookup"><span data-stu-id="3160d-457">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="3160d-458">Aby ustawić ten nagłówek, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="3160d-458">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="3160d-459">Jak działa mechanizm CORS</span><span class="sxs-lookup"><span data-stu-id="3160d-459">How CORS works</span></span>

<span data-ttu-id="3160d-460">W tej sekcji opisano, co się dzieje w żądaniu [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) na poziomie komunikatów http.</span><span class="sxs-lookup"><span data-stu-id="3160d-460">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="3160d-461">Mechanizm CORS **nie** jest funkcją zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3160d-461">CORS is **not** a security feature.</span></span> <span data-ttu-id="3160d-462">CORS jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="3160d-462">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="3160d-463">Na przykład złośliwy aktor może użyć [zapobiegania skryptom między lokacjami (XSS)](xref:security/cross-site-scripting) względem witryny i wykonać żądanie między lokacjami w celu wykraść informacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-463">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="3160d-464">Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-464">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="3160d-465">Aby wymusić mechanizm CORS, należy do klienta (przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="3160d-465">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="3160d-466">Serwer wykonuje żądanie i zwraca odpowiedź, jest to klient, który zwraca błąd i blokuje odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="3160d-466">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="3160d-467">Na przykład w dowolnym z poniższych narzędzi zostanie wyświetlona odpowiedź serwera:</span><span class="sxs-lookup"><span data-stu-id="3160d-467">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="3160d-468">Fiddler</span><span class="sxs-lookup"><span data-stu-id="3160d-468">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="3160d-469">Postman</span><span class="sxs-lookup"><span data-stu-id="3160d-469">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="3160d-470">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="3160d-470">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="3160d-471">Przeglądarka sieci Web, wprowadzając adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="3160d-471">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="3160d-472">Jest to sposób, aby serwer zezwalał przeglądarce na wykonywanie żądania [interfejsu API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) lub pobierania między źródłami, które w przeciwnym razie byłoby zabronione.</span><span class="sxs-lookup"><span data-stu-id="3160d-472">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="3160d-473">Przeglądarki (bez CORS) nie mogą wykonywać żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-473">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="3160d-474">Przed zastosowaniem mechanizmu CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) został użyty do obejścia tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="3160d-474">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="3160d-475">JSONP nie używa XHR, aby otrzymać odpowiedź, używa znacznika `<script>`.</span><span class="sxs-lookup"><span data-stu-id="3160d-475">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="3160d-476">Skrypty mogą być ładowane między różnymi źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-476">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="3160d-477">[Specyfikacja CORS](https://www.w3.org/TR/cors/) wprowadziła kilka nowych nagłówków HTTP, które umożliwiają żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="3160d-477">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="3160d-478">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia te nagłówki automatycznie dla żądań cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="3160d-478">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="3160d-479">Niestandardowy kod JavaScript nie jest wymagany do włączenia mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-479">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="3160d-480">Poniżej przedstawiono przykład żądania między źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="3160d-480">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="3160d-481">Nagłówek `Origin` zapewnia domenę lokacji, która żąda żądania.</span><span class="sxs-lookup"><span data-stu-id="3160d-481">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="3160d-482">Nagłówek `Origin` jest wymagany i musi się różnić od hosta.</span><span class="sxs-lookup"><span data-stu-id="3160d-482">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="3160d-483">Jeśli serwer zezwala na żądanie, ustawia nagłówek `Access-Control-Allow-Origin` w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3160d-483">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="3160d-484">Wartość tego nagłówka jest zgodna z nagłówkiem `Origin` z żądania lub jest wartością symbolu wieloznacznego `"*"`, co oznacza, że wszystkie źródła są dozwolone:</span><span class="sxs-lookup"><span data-stu-id="3160d-484">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="3160d-485">Jeśli odpowiedź nie zawiera nagłówka `Access-Control-Allow-Origin`, żądanie między źródłami nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="3160d-485">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="3160d-486">W programie przeglądarka nie zezwala na żądanie.</span><span class="sxs-lookup"><span data-stu-id="3160d-486">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="3160d-487">Nawet jeśli serwer zwróci pomyślną odpowiedź, przeglądarka nie udostępni odpowiedzi dla aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="3160d-487">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="3160d-488">Testowanie CORS</span><span class="sxs-lookup"><span data-stu-id="3160d-488">Test CORS</span></span>

<span data-ttu-id="3160d-489">Aby przetestować CORS:</span><span class="sxs-lookup"><span data-stu-id="3160d-489">To test CORS:</span></span>

1. <span data-ttu-id="3160d-490">[Utwórz projekt interfejsu API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="3160d-490">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="3160d-491">Możesz również [pobrać przykład](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="3160d-491">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="3160d-492">Włącz funkcję CORS przy użyciu jednego z metod w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="3160d-492">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="3160d-493">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3160d-493">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="3160d-494">`WithOrigins("https://localhost:<port>");` należy używać tylko do testowania przykładowej aplikacji podobnej do [przykładowego kodu pobierania](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="3160d-494">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="3160d-495">Utwórz projekt aplikacji sieci Web (Razor Pages lub MVC).</span><span class="sxs-lookup"><span data-stu-id="3160d-495">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="3160d-496">Przykład używa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3160d-496">The sample uses Razor Pages.</span></span> <span data-ttu-id="3160d-497">Aplikację sieci Web można utworzyć w tym samym rozwiązaniu co projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="3160d-497">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="3160d-498">Dodaj następujący wyróżniony kod do pliku *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3160d-498">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="3160d-499">W poprzednim kodzie Zastąp `url: 'https://<web app>.azurewebsites.net/api/values/1',` adresem URL wdrożonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-499">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="3160d-500">Wdróż projekt interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="3160d-500">Deploy the API project.</span></span> <span data-ttu-id="3160d-501">Na przykład [Wdróż na platformie Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="3160d-501">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="3160d-502">Uruchom aplikację Razor Pages lub MVC na pulpicie, a następnie kliknij przycisk **Testuj** .</span><span class="sxs-lookup"><span data-stu-id="3160d-502">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="3160d-503">Użyj narzędzi F12, aby przejrzeć komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="3160d-503">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="3160d-504">Usuń pochodzenie hosta lokalnego z `WithOrigins` i Wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="3160d-504">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="3160d-505">Alternatywnie Uruchom aplikację kliencką z innym portem.</span><span class="sxs-lookup"><span data-stu-id="3160d-505">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="3160d-506">Na przykład uruchom polecenie z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3160d-506">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="3160d-507">Przetestuj za pomocą aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="3160d-507">Test with the client app.</span></span> <span data-ttu-id="3160d-508">Błędy funkcji CORS zwracają błąd, ale komunikat o błędzie nie jest dostępny dla języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3160d-508">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="3160d-509">Aby wyświetlić błąd, Użyj karty konsola w narzędziach F12.</span><span class="sxs-lookup"><span data-stu-id="3160d-509">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="3160d-510">W zależności od przeglądarki pojawia się błąd (w konsoli narzędzia F12) podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="3160d-510">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="3160d-511">Korzystanie z przeglądarki Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="3160d-511">Using Microsoft Edge:</span></span>

     <span data-ttu-id="3160d-512">**SEC7120: [CORS] Źródło `https://localhost:44375` nie znalazło `https://localhost:44375` w nagłówku odpowiedzi "Access-Control-Allow-Origin" dla zasobu Cross-Origin w `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="3160d-512">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="3160d-513">Korzystanie z programu Chrome:</span><span class="sxs-lookup"><span data-stu-id="3160d-513">Using Chrome:</span></span>

     <span data-ttu-id="3160d-514">**Dostęp do elementu XMLHttpRequest na `https://webapi.azurewebsites.net/api/values/1` ze źródła `https://localhost:44375` został zablokowany przez zasady CORS: brak nagłówka "Access-Control-Allow-Origin" w żądanym zasobie.**</span><span class="sxs-lookup"><span data-stu-id="3160d-514">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="3160d-515">Punkty końcowe z obsługą mechanizmu CORS można testować za pomocą narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler) lub [Poster](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="3160d-515">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="3160d-516">W przypadku korzystania z narzędzia, Źródło żądania określone przez nagłówek `Origin` musi różnić się od hosta przyjmującego żądanie.</span><span class="sxs-lookup"><span data-stu-id="3160d-516">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="3160d-517">Jeśli żądanie nie jest *źródłem krzyżowe* na podstawie wartości nagłówka `Origin`:</span><span class="sxs-lookup"><span data-stu-id="3160d-517">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="3160d-518">Nie ma potrzeby przetwarzania żądania przez oprogramowanie pośredniczące CORS.</span><span class="sxs-lookup"><span data-stu-id="3160d-518">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="3160d-519">Nagłówki CORS nie są zwracane w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3160d-519">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="3160d-520">Mechanizm CORS w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="3160d-520">CORS in IIS</span></span>

<span data-ttu-id="3160d-521">W przypadku wdrażania w programie IIS należy uruchomić funkcję CORS przed uwierzytelnianiem systemu Windows, jeśli serwer nie jest skonfigurowany do zezwalania na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="3160d-521">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="3160d-522">Aby zapewnić obsługę tego scenariusza, należy zainstalować i skonfigurować [moduł CORS usług IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3160d-522">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3160d-523">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3160d-523">Additional resources</span></span>

* [<span data-ttu-id="3160d-524">Współużytkowanie zasobów między źródłami (CORS)</span><span class="sxs-lookup"><span data-stu-id="3160d-524">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="3160d-525">Wprowadzenie do modułu CORS usług IIS</span><span class="sxs-lookup"><span data-stu-id="3160d-525">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end