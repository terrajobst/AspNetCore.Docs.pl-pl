---
title: Buforowanie oprogramowania pośredniczącego w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować i używać oprogramowania pośredniczącego buforowania odpowiedzi w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: performance/caching/middleware
ms.openlocfilehash: 6371f42b100f70c6042064a6372c7b9e41fd5c73
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914995"
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="b18b1-103">Buforowanie oprogramowania pośredniczącego w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b18b1-103">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="b18b1-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Jan Luo](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="b18b1-104">By [Luke Latham](https://github.com/guardrex) and [John Luo](https://github.com/JunTaoLuo)</span></span>

<span data-ttu-id="b18b1-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b18b1-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b18b1-106">W tym artykule opisano sposób konfigurowania oprogramowania pośredniczącego buforowania odpowiedzi w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b18b1-106">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="b18b1-107">Oprogramowanie pośredniczące określa, kiedy odpowiedzi są buforowane, przechowuje odpowiedzi i obsługuje odpowiedzi z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b18b1-107">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="b18b1-108">Aby zapoznać się z wprowadzeniem do buforowania HTTP i atrybutem [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) , zobacz [buforowanie odpowiedzi](xref:performance/caching/response).</span><span class="sxs-lookup"><span data-stu-id="b18b1-108">For an introduction to HTTP caching and the [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

## <a name="configuration"></a><span data-ttu-id="b18b1-109">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="b18b1-109">Configuration</span></span>

<span data-ttu-id="b18b1-110">Użyj pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. AspNetCore. ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) .</span><span class="sxs-lookup"><span data-stu-id="b18b1-110">Use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

<span data-ttu-id="b18b1-111">W `Startup.ConfigureServices`programie Dodaj oprogramowanie pośredniczące buforowania odpowiedzi do kolekcji usług:</span><span class="sxs-lookup"><span data-stu-id="b18b1-111">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

::: moniker-end

<span data-ttu-id="b18b1-112">Skonfiguruj aplikację do korzystania z oprogramowania pośredniczącego z <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> metodą rozszerzenia, która dodaje oprogramowanie pośredniczące do potoku przetwarzania żądań w: `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="b18b1-112">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

::: moniker-end

<span data-ttu-id="b18b1-113">Przykładowa aplikacja dodaje nagłówki, aby kontrolować buforowanie w kolejnych żądaniach:</span><span class="sxs-lookup"><span data-stu-id="b18b1-113">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="b18b1-114">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Buforuje odpowiedzi w pamięci podręcznej przez maksymalnie 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="b18b1-114">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="b18b1-115">[Różni](https://tools.ietf.org/html/rfc7231#section-7.1.4) się Konfiguruje oprogramowanie pośredniczące do obsługiwania buforowanej odpowiedzi tylko [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) wtedy, gdy nagłówek kolejnych żądań jest zgodny z oryginalnym żądaniem. &ndash;</span><span class="sxs-lookup"><span data-stu-id="b18b1-115">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Configures the middleware to serve a cached response only if the [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

::: moniker-end

<span data-ttu-id="b18b1-116">Buforowanie odpowiedzi w pamięci podręcznej tylko buforuje odpowiedzi serwera, które powodują, że jest to kod stanu 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="b18b1-116">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="b18b1-117">Wszystkie inne odpowiedzi, w tym [strony błędów](xref:fundamentals/error-handling), są ignorowane przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="b18b1-117">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="b18b1-118">Odpowiedzi zawierające zawartość uwierzytelnionych klientów muszą być oznaczone jako nieobsługujące pamięci podręcznej, aby uniemożliwić przechowywanie i obsługę tych odpowiedzi przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="b18b1-118">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="b18b1-119">Zobacz [warunki buforowania](#conditions-for-caching) , aby uzyskać szczegółowe informacje na temat sposobu, w jaki oprogramowanie pośredniczące określa, czy odpowiedź jest buforowana.</span><span class="sxs-lookup"><span data-stu-id="b18b1-119">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="b18b1-120">Opcje</span><span class="sxs-lookup"><span data-stu-id="b18b1-120">Options</span></span>

<span data-ttu-id="b18b1-121">Opcje buforowania odpowiedzi przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b18b1-121">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="b18b1-122">Opcja</span><span class="sxs-lookup"><span data-stu-id="b18b1-122">Option</span></span> | <span data-ttu-id="b18b1-123">Opis</span><span class="sxs-lookup"><span data-stu-id="b18b1-123">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="b18b1-124">Największy rozmiar pamięci podręcznej dla treści odpowiedzi (w bajtach).</span><span class="sxs-lookup"><span data-stu-id="b18b1-124">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="b18b1-125">Wartość domyślna to `64 * 1024 * 1024` (64 MB).</span><span class="sxs-lookup"><span data-stu-id="b18b1-125">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="b18b1-126">Limit rozmiaru dla oprogramowania pośredniczącego pamięci podręcznej odpowiedzi w bajtach.</span><span class="sxs-lookup"><span data-stu-id="b18b1-126">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="b18b1-127">Wartość domyślna to `100 * 1024 * 1024` (100 MB).</span><span class="sxs-lookup"><span data-stu-id="b18b1-127">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="b18b1-128">Określa, czy odpowiedzi są buforowane w przypadku ścieżek z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="b18b1-128">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="b18b1-129">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="b18b1-129">The default value is `false`.</span></span> |

<span data-ttu-id="b18b1-130">Poniższy przykład konfiguruje oprogramowanie pośredniczące w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b18b1-130">The following example configures the middleware to:</span></span>

* <span data-ttu-id="b18b1-131">Buforuj odpowiedzi o rozmiarze treści mniejszym lub równym 1 024 bajtów.</span><span class="sxs-lookup"><span data-stu-id="b18b1-131">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="b18b1-132">Przechowaj odpowiedzi w ścieżce z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="b18b1-132">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="b18b1-133">Na przykład `/page1` i `/Page1` są przechowywane osobno.</span><span class="sxs-lookup"><span data-stu-id="b18b1-133">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="b18b1-134">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="b18b1-134">VaryByQueryKeys</span></span>

<span data-ttu-id="b18b1-135">W przypadku używania kontrolerów MVC/Web API lub Razor Pages modele stron atrybut [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) określa parametry niezbędne do ustawiania odpowiednich nagłówków dla buforowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b18b1-135">When using MVC / web API controllers or Razor Pages page models, the [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="b18b1-136">Jedyny parametr `[ResponseCache]` atrybutu, który ściśle wymaga oprogramowania pośredniczącego, to <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, który nie odpowiada rzeczywistemu nagłówkowi http.</span><span class="sxs-lookup"><span data-stu-id="b18b1-136">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="b18b1-137">Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response#responsecache-attribute>.</span><span class="sxs-lookup"><span data-stu-id="b18b1-137">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="b18b1-138">Gdy nie korzystasz `[ResponseCache]` z atrybutu, buforowanie odpowiedzi może być zróżnicowane z. `VaryByQueryKeys`</span><span class="sxs-lookup"><span data-stu-id="b18b1-138">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="b18b1-139">Użyj bezpośrednio z [funkcji HttpContext. Features:](xref:Microsoft.AspNetCore.Http.HttpContext.Features) <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature></span><span class="sxs-lookup"><span data-stu-id="b18b1-139">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="b18b1-140">Użycie pojedynczej wartości równej `*` w `VaryByQueryKeys` w programie zmienia pamięć podręczną przez wszystkie parametry zapytania żądania.</span><span class="sxs-lookup"><span data-stu-id="b18b1-140">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="b18b1-141">Nagłówki HTTP używane przez oprogramowanie pośredniczące buforowania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="b18b1-141">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="b18b1-142">Poniższa tabela zawiera informacje dotyczące nagłówków HTTP, które mają wpływ na buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b18b1-142">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="b18b1-143">nagłówek</span><span class="sxs-lookup"><span data-stu-id="b18b1-143">Header</span></span> | <span data-ttu-id="b18b1-144">Szczegóły</span><span class="sxs-lookup"><span data-stu-id="b18b1-144">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="b18b1-145">Odpowiedź nie jest buforowana, jeśli nagłówek istnieje.</span><span class="sxs-lookup"><span data-stu-id="b18b1-145">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="b18b1-146">Oprogramowanie pośredniczące traktuje tylko odpowiedzi w pamięci `public` podręcznej oznaczone za pomocą dyrektywy Cache.</span><span class="sxs-lookup"><span data-stu-id="b18b1-146">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="b18b1-147">Sterowanie buforowaniem przy użyciu następujących parametrów:</span><span class="sxs-lookup"><span data-stu-id="b18b1-147">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="b18b1-148">maks. wiek</span><span class="sxs-lookup"><span data-stu-id="b18b1-148">max-age</span></span></li><li><span data-ttu-id="b18b1-149">max-stale&#8224;</span><span class="sxs-lookup"><span data-stu-id="b18b1-149">max-stale&#8224;</span></span></li><li><span data-ttu-id="b18b1-150">min — nowy</span><span class="sxs-lookup"><span data-stu-id="b18b1-150">min-fresh</span></span></li><li><span data-ttu-id="b18b1-151">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="b18b1-151">must-revalidate</span></span></li><li><span data-ttu-id="b18b1-152">no-cache</span><span class="sxs-lookup"><span data-stu-id="b18b1-152">no-cache</span></span></li><li><span data-ttu-id="b18b1-153">bez sklepu</span><span class="sxs-lookup"><span data-stu-id="b18b1-153">no-store</span></span></li><li><span data-ttu-id="b18b1-154">tylko-w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b18b1-154">only-if-cached</span></span></li><li><span data-ttu-id="b18b1-155">private</span><span class="sxs-lookup"><span data-stu-id="b18b1-155">private</span></span></li><li><span data-ttu-id="b18b1-156">public</span><span class="sxs-lookup"><span data-stu-id="b18b1-156">public</span></span></li><li><span data-ttu-id="b18b1-157">s-maxage</span><span class="sxs-lookup"><span data-stu-id="b18b1-157">s-maxage</span></span></li><li><span data-ttu-id="b18b1-158">proxy-revalidate&#8225;</span><span class="sxs-lookup"><span data-stu-id="b18b1-158">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="b18b1-159">&#8224;Jeśli żaden limit nie zostanie określony `max-stale`do, oprogramowanie pośredniczące nie podejmuje żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="b18b1-159">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="b18b1-160">&#8225;`proxy-revalidate`ma ten sam skutek co `must-revalidate`.</span><span class="sxs-lookup"><span data-stu-id="b18b1-160">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="b18b1-161">Aby uzyskać więcej informacji, [zobacz RFC 7231: Żądaj dyrektyw](https://tools.ietf.org/html/rfc7234#section-5.2.1)kontroli pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b18b1-161">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="b18b1-162">Nagłówek w żądaniu daje ten sam skutek co `Cache-Control: no-cache`. `Pragma: no-cache`</span><span class="sxs-lookup"><span data-stu-id="b18b1-162">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="b18b1-163">Ten nagłówek jest zastępowany przez odpowiednie dyrektywy w `Cache-Control` nagłówku, jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="b18b1-163">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="b18b1-164">Uwzględnianie zgodności z poprzednimi wersjami przy użyciu protokołu HTTP/1.0.</span><span class="sxs-lookup"><span data-stu-id="b18b1-164">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="b18b1-165">Odpowiedź nie jest buforowana, jeśli nagłówek istnieje.</span><span class="sxs-lookup"><span data-stu-id="b18b1-165">The response isn't cached if the header exists.</span></span> <span data-ttu-id="b18b1-166">Wszystkie oprogramowanie pośredniczące w potoku przetwarzania żądań, które ustawia jeden lub więcej plików cookie, uniemożliwia buforowanie odpowiedzi z buforowania odpowiedzi (na przykład [dostawcy TempData opartego na plikach cookie](xref:fundamentals/app-state#tempdata)).</span><span class="sxs-lookup"><span data-stu-id="b18b1-166">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="b18b1-167">`Vary` Nagłówek jest używany do różnicowania buforowanej odpowiedzi przez inny nagłówek.</span><span class="sxs-lookup"><span data-stu-id="b18b1-167">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="b18b1-168">Na przykład w pamięci podręcznej odpowiedzi są kodowane `Vary: Accept-Encoding` przez dołączenie nagłówka, w którym są buforowane `Accept-Encoding: gzip` odpowiedzi `Accept-Encoding: text/plain` na żądania z nagłówkami i oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="b18b1-168">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="b18b1-169">Odpowiedź z wartością `*` nagłówka nigdy nie jest przechowywana.</span><span class="sxs-lookup"><span data-stu-id="b18b1-169">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="b18b1-170">Odpowiedź uznana za nieodświeżoną przez ten nagłówek nie jest zapisywana ani pobierana, chyba że zostanie zastąpiona przez inne `Cache-Control` nagłówki.</span><span class="sxs-lookup"><span data-stu-id="b18b1-170">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="b18b1-171">Pełna odpowiedź jest obsługiwana z pamięci podręcznej, jeśli `*` wartość nie `ETag` jest, a odpowiedź nie jest zgodna z żadną z podanych wartości.</span><span class="sxs-lookup"><span data-stu-id="b18b1-171">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="b18b1-172">W przeciwnym razie jest obsługiwana odpowiedź 304 (nie modyfikowane).</span><span class="sxs-lookup"><span data-stu-id="b18b1-172">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="b18b1-173">`If-None-Match` Jeśli nagłówek nie istnieje, jest obsługiwana Pełna odpowiedź z pamięci podręcznej, jeśli data odpowiedzi w pamięci podręcznej jest nowsza niż podana wartość.</span><span class="sxs-lookup"><span data-stu-id="b18b1-173">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="b18b1-174">W przeciwnym razie jest obsługiwana odpowiedź *304 — nie zmodyfikowano* .</span><span class="sxs-lookup"><span data-stu-id="b18b1-174">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="b18b1-175">W przypadku obsługi z pamięci podręcznej `Date` nagłówek jest ustawiany przez oprogramowanie pośredniczące, jeśli nie został podany w oryginalnej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b18b1-175">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="b18b1-176">W przypadku obsługi z pamięci podręcznej `Content-Length` nagłówek jest ustawiany przez oprogramowanie pośredniczące, jeśli nie został podany w oryginalnej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b18b1-176">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="b18b1-177">`Age` Nagłówek wysłany w oryginalnej odpowiedzi jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="b18b1-177">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="b18b1-178">Oprogramowanie pośredniczące oblicza nową wartość w przypadku obsługi odpowiedzi w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b18b1-178">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="b18b1-179">Buforowanie — dyrektywy dotyczące kontroli pamięci podręcznej żądań</span><span class="sxs-lookup"><span data-stu-id="b18b1-179">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="b18b1-180">Oprogramowanie pośredniczące uwzględnia reguły [buforowania HTTP 1,1](https://tools.ietf.org/html/rfc7234#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="b18b1-180">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="b18b1-181">Reguły wymagają pamięci podręcznej, aby honorować prawidłowy `Cache-Control` nagłówek Wysłany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="b18b1-181">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="b18b1-182">W obszarze specyfikacji klient może wykonywać żądania z `no-cache` wartością nagłówka i wymusić, aby serwer generował nową odpowiedź dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b18b1-182">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="b18b1-183">Obecnie nie ma kontroli nad tym zachowaniem buforowania podczas korzystania z oprogramowania pośredniczącego, ponieważ oprogramowanie pośredniczące jest zgodne z oficjalną specyfikacją buforowania.</span><span class="sxs-lookup"><span data-stu-id="b18b1-183">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="b18b1-184">Aby uzyskać większą kontrolę nad zachowaniem buforowania, zapoznaj się z innymi funkcjami buforowania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b18b1-184">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="b18b1-185">Zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="b18b1-185">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="b18b1-186">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="b18b1-186">Troubleshooting</span></span>

<span data-ttu-id="b18b1-187">Jeśli zachowanie buforowania nie jest zgodne z oczekiwaniami, upewnij się, że odpowiedzi są buforowane i mogą być obsługiwane z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b18b1-187">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="b18b1-188">Sprawdź przychodzące nagłówki żądania i wychodzące nagłówki odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b18b1-188">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="b18b1-189">Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby pomóc w debugowaniu.</span><span class="sxs-lookup"><span data-stu-id="b18b1-189">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="b18b1-190">Podczas testowania i rozwiązywania problemów z pamięcią podręczną przeglądarka może ustawić nagłówki żądań, które mają wpływ na buforowanie na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="b18b1-190">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="b18b1-191">Na przykład przeglądarka może ustawić `Cache-Control` nagłówek na `no-cache` lub `max-age=0` podczas odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="b18b1-191">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="b18b1-192">Poniższe narzędzia mogą jawnie ustawić nagłówki żądań i są preferowane w celu przeprowadzenia testu w pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="b18b1-192">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="b18b1-193">Fiddler</span><span class="sxs-lookup"><span data-stu-id="b18b1-193">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="b18b1-194">Postman</span><span class="sxs-lookup"><span data-stu-id="b18b1-194">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="b18b1-195">Warunki buforowania</span><span class="sxs-lookup"><span data-stu-id="b18b1-195">Conditions for caching</span></span>

* <span data-ttu-id="b18b1-196">Żądanie musi spowodować odpowiedź serwera z kodem stanu 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="b18b1-196">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="b18b1-197">Metoda żądania musi mieć wartość GET lub $.</span><span class="sxs-lookup"><span data-stu-id="b18b1-197">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="b18b1-198">W `Startup.Configure`programie oprogramowanie pośredniczące buforowania odpowiedzi należy umieścić przed oprogramowanie pośredniczące wymagające buforowania.</span><span class="sxs-lookup"><span data-stu-id="b18b1-198">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="b18b1-199">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="b18b1-199">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="b18b1-200">`Authorization` Nagłówek nie może być obecny.</span><span class="sxs-lookup"><span data-stu-id="b18b1-200">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="b18b1-201">`Cache-Control`parametry nagłówka muszą być prawidłowe, a odpowiedź musi być oznaczona jako `public` nieoznaczona `private`.</span><span class="sxs-lookup"><span data-stu-id="b18b1-201">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="b18b1-202">Nagłówek nie może być obecny, `Cache-Control` Jeśli nagłówek nie istnieje, `Pragma` jako że `Cache-Control` nagłówek zastępuje nagłówek, gdy jest obecny. `Pragma: no-cache`</span><span class="sxs-lookup"><span data-stu-id="b18b1-202">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="b18b1-203">`Set-Cookie` Nagłówek nie może być obecny.</span><span class="sxs-lookup"><span data-stu-id="b18b1-203">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="b18b1-204">`Vary`parametry nagłówka muszą być prawidłowe i nie mogą `*`być równe.</span><span class="sxs-lookup"><span data-stu-id="b18b1-204">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="b18b1-205">Wartość `Content-Length` nagłówka (jeśli jest ustawiona) musi być zgodna z rozmiarem treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b18b1-205">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="b18b1-206"><xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> Nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="b18b1-206">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="b18b1-207">Odpowiedź nie może być nieodświeżona zgodnie z `Expires` definicją nagłówka `max-age` i `s-maxage` dyrektywy pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b18b1-207">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="b18b1-208">Buforowanie odpowiedzi musi się powieść.</span><span class="sxs-lookup"><span data-stu-id="b18b1-208">Response buffering must be successful.</span></span> <span data-ttu-id="b18b1-209">Rozmiar odpowiedzi musi być mniejszy niż skonfigurowany lub domyślny <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span><span class="sxs-lookup"><span data-stu-id="b18b1-209">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="b18b1-210">Rozmiar treści odpowiedzi musi być mniejszy niż skonfigurowany lub domyślny <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span><span class="sxs-lookup"><span data-stu-id="b18b1-210">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="b18b1-211">Odpowiedź musi być buforowana zgodnie ze specyfikacją [RFC 7234](https://tools.ietf.org/html/rfc7234) .</span><span class="sxs-lookup"><span data-stu-id="b18b1-211">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="b18b1-212">Na przykład `no-store` dyrektywa nie może istnieć w polach żądania ani nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b18b1-212">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="b18b1-213">Zobacz *sekcję 3: Przechowywanie odpowiedzi w pamięci podręcznej* [RFC 7234](https://tools.ietf.org/html/rfc7234) w celu uzyskania szczegółów.</span><span class="sxs-lookup"><span data-stu-id="b18b1-213">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="b18b1-214">System antysfałszowany służący do generowania zabezpieczonych tokenów, aby zapobiec atakom `Cache-Control` przez wiele witryn (CSRF), w `no-cache` związku z czym `Pragma` odpowiedzi nie są buforowane.</span><span class="sxs-lookup"><span data-stu-id="b18b1-214">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="b18b1-215">Aby uzyskać informacje na temat sposobu wyłączania tokenów antysfałszowanych dla elementów <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>formularza HTML, zobacz.</span><span class="sxs-lookup"><span data-stu-id="b18b1-215">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b18b1-216">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b18b1-216">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
