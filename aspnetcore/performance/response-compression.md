---
title: Kompresja odpowiedzi w ASP.NET Core
author: guardrex
description: Dowiedz się więcej o kompresji odpowiedzi i sposobach używania oprogramowania pośredniczącego kompresji odpowiedzi w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/response-compression
ms.openlocfilehash: b8a84418a3258e9ac43b4eadd8564c0708590bce
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726961"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="05ac9-103">Kompresja odpowiedzi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05ac9-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="05ac9-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="05ac9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="05ac9-105">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05ac9-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="05ac9-106">Przepustowość sieci jest ograniczonym zasobem.</span><span class="sxs-lookup"><span data-stu-id="05ac9-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="05ac9-107">Zmniejszenie rozmiaru odpowiedzi zwykle zwiększa czas odpowiedzi aplikacji, często znacząco.</span><span class="sxs-lookup"><span data-stu-id="05ac9-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="05ac9-108">Jednym ze sposobów zmniejszenia rozmiaru ładunku jest kompresowanie odpowiedzi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="05ac9-109">Kiedy używać oprogramowania pośredniczącego kompresji odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="05ac9-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="05ac9-110">Użyj technologii kompresji odpowiedzi opartej na serwerze w usługach IIS, Apache lub nginx.</span><span class="sxs-lookup"><span data-stu-id="05ac9-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="05ac9-111">Wydajność oprogramowania pośredniczącego prawdopodobnie nie jest zgodna z tymi, które są używane w modułach serwera.</span><span class="sxs-lookup"><span data-stu-id="05ac9-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="05ac9-112">Serwer [serwerowy http. sys](xref:fundamentals/servers/httpsys) i serwer [Kestrel](xref:fundamentals/servers/kestrel) nie oferują obecnie wbudowanej obsługi kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="05ac9-113">Użyj oprogramowania pośredniczącego kompresji odpowiedzi, gdy jesteś:</span><span class="sxs-lookup"><span data-stu-id="05ac9-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="05ac9-114">Nie można użyć następujących technologii kompresji opartych na serwerze:</span><span class="sxs-lookup"><span data-stu-id="05ac9-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="05ac9-115">Moduł kompresji dynamicznej usług IIS</span><span class="sxs-lookup"><span data-stu-id="05ac9-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="05ac9-116">Moduł mod_deflate Apache</span><span class="sxs-lookup"><span data-stu-id="05ac9-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="05ac9-117">Kompresja i dekompresja Nginx</span><span class="sxs-lookup"><span data-stu-id="05ac9-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="05ac9-118">Hosting bezpośrednio na:</span><span class="sxs-lookup"><span data-stu-id="05ac9-118">Hosting directly on:</span></span>
  * <span data-ttu-id="05ac9-119">[Serwer http. sys](xref:fundamentals/servers/httpsys) (dawniej nazywany webListener)</span><span class="sxs-lookup"><span data-stu-id="05ac9-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="05ac9-120">Serwer Kestrel</span><span class="sxs-lookup"><span data-stu-id="05ac9-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="05ac9-121">Kompresja odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="05ac9-121">Response compression</span></span>

<span data-ttu-id="05ac9-122">Zazwyczaj Każda odpowiedź nieskompresowana natywnie może korzystać z kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="05ac9-123">Odpowiedzi nienatywnie skompresowane zazwyczaj obejmują: CSS, JavaScript, HTML, XML i JSON.</span><span class="sxs-lookup"><span data-stu-id="05ac9-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="05ac9-124">Nie należy kompresować natywnie skompresowanych zasobów, takich jak pliki PNG.</span><span class="sxs-lookup"><span data-stu-id="05ac9-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="05ac9-125">W przypadku próby przeprowadzenia dalszej kompresji natywnie skompresowanej odpowiedzi wszystkie niewielkie dodatkowe zmniejszenie rozmiaru i czasu transmisji będą prawdopodobnie przesłonięte przez czas potrzebny do przetworzenia kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="05ac9-126">Nie Kompresuj plików mniejszych niż około 150-1000 bajtów (w zależności od zawartości pliku i wydajności kompresji).</span><span class="sxs-lookup"><span data-stu-id="05ac9-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="05ac9-127">Narzuty kompresowania małych plików może generować skompresowany plik większy niż plik nieskompresowany.</span><span class="sxs-lookup"><span data-stu-id="05ac9-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="05ac9-128">Gdy klient może przetwarzać skompresowaną zawartość, klient musi poinformować serwer o swoich możliwościach, wysyłając do `Accept-Encoding` nagłówek z żądaniem.</span><span class="sxs-lookup"><span data-stu-id="05ac9-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="05ac9-129">Gdy serwer wysyła skompresowaną zawartość, musi zawierać informacje w nagłówku `Content-Encoding` na sposób kodowania skompresowanej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="05ac9-130">Oznaczenia kodowania zawartości obsługiwane przez oprogramowanie pośredniczące przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="05ac9-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="05ac9-131">`Accept-Encoding` wartości nagłówka</span><span class="sxs-lookup"><span data-stu-id="05ac9-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="05ac9-132">Obsługiwane oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="05ac9-132">Middleware Supported</span></span> | <span data-ttu-id="05ac9-133">Opis</span><span class="sxs-lookup"><span data-stu-id="05ac9-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="05ac9-134">Tak (domyślnie)</span><span class="sxs-lookup"><span data-stu-id="05ac9-134">Yes (default)</span></span>        | [<span data-ttu-id="05ac9-135">Format skompresowanych danych Brotli</span><span class="sxs-lookup"><span data-stu-id="05ac9-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="05ac9-136">Nie</span><span class="sxs-lookup"><span data-stu-id="05ac9-136">No</span></span>                   | [<span data-ttu-id="05ac9-137">WKLĘŚNIĘCIE — skompresowany format danych</span><span class="sxs-lookup"><span data-stu-id="05ac9-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="05ac9-138">Nie</span><span class="sxs-lookup"><span data-stu-id="05ac9-138">No</span></span>                   | [<span data-ttu-id="05ac9-139">Wydajna wymiana XML</span><span class="sxs-lookup"><span data-stu-id="05ac9-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="05ac9-140">Tak</span><span class="sxs-lookup"><span data-stu-id="05ac9-140">Yes</span></span>                  | [<span data-ttu-id="05ac9-141">Format pliku gzip</span><span class="sxs-lookup"><span data-stu-id="05ac9-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="05ac9-142">Tak</span><span class="sxs-lookup"><span data-stu-id="05ac9-142">Yes</span></span>                  | <span data-ttu-id="05ac9-143">Identyfikator "bez kodowania": odpowiedź nie może być zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="05ac9-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="05ac9-144">Nie</span><span class="sxs-lookup"><span data-stu-id="05ac9-144">No</span></span>                   | [<span data-ttu-id="05ac9-145">Format transferu sieciowego dla archiwów języka Java</span><span class="sxs-lookup"><span data-stu-id="05ac9-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="05ac9-146">Tak</span><span class="sxs-lookup"><span data-stu-id="05ac9-146">Yes</span></span>                  | <span data-ttu-id="05ac9-147">Wszystkie dostępne kodowanie zawartości nie jest jawnie wymagane</span><span class="sxs-lookup"><span data-stu-id="05ac9-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="05ac9-148">`Accept-Encoding` wartości nagłówka</span><span class="sxs-lookup"><span data-stu-id="05ac9-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="05ac9-149">Obsługiwane oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="05ac9-149">Middleware Supported</span></span> | <span data-ttu-id="05ac9-150">Opis</span><span class="sxs-lookup"><span data-stu-id="05ac9-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="05ac9-151">Nie</span><span class="sxs-lookup"><span data-stu-id="05ac9-151">No</span></span>                   | [<span data-ttu-id="05ac9-152">Format skompresowanych danych Brotli</span><span class="sxs-lookup"><span data-stu-id="05ac9-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="05ac9-153">Nie</span><span class="sxs-lookup"><span data-stu-id="05ac9-153">No</span></span>                   | [<span data-ttu-id="05ac9-154">WKLĘŚNIĘCIE — skompresowany format danych</span><span class="sxs-lookup"><span data-stu-id="05ac9-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="05ac9-155">Nie</span><span class="sxs-lookup"><span data-stu-id="05ac9-155">No</span></span>                   | [<span data-ttu-id="05ac9-156">Wydajna wymiana XML</span><span class="sxs-lookup"><span data-stu-id="05ac9-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="05ac9-157">Tak (domyślnie)</span><span class="sxs-lookup"><span data-stu-id="05ac9-157">Yes (default)</span></span>        | [<span data-ttu-id="05ac9-158">Format pliku gzip</span><span class="sxs-lookup"><span data-stu-id="05ac9-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="05ac9-159">Tak</span><span class="sxs-lookup"><span data-stu-id="05ac9-159">Yes</span></span>                  | <span data-ttu-id="05ac9-160">Identyfikator "bez kodowania": odpowiedź nie może być zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="05ac9-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="05ac9-161">Nie</span><span class="sxs-lookup"><span data-stu-id="05ac9-161">No</span></span>                   | [<span data-ttu-id="05ac9-162">Format transferu sieciowego dla archiwów języka Java</span><span class="sxs-lookup"><span data-stu-id="05ac9-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="05ac9-163">Tak</span><span class="sxs-lookup"><span data-stu-id="05ac9-163">Yes</span></span>                  | <span data-ttu-id="05ac9-164">Wszystkie dostępne kodowanie zawartości nie jest jawnie wymagane</span><span class="sxs-lookup"><span data-stu-id="05ac9-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="05ac9-165">Aby uzyskać więcej informacji, zapoznaj się z [listą oficjalnych kodowania zawartości organizacji Iana](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="05ac9-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="05ac9-166">Oprogramowanie pośredniczące umożliwia dodanie dodatkowych dostawców kompresji dla wartości nagłówków niestandardowych `Accept-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="05ac9-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="05ac9-167">Aby uzyskać więcej informacji, zobacz [niestandardowe dostawcy](#custom-providers) poniżej.</span><span class="sxs-lookup"><span data-stu-id="05ac9-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="05ac9-168">Oprogramowanie pośredniczące może resłużyć do rozważenia wartości jakości (qvalue, `q`), gdy są wysyłane przez klienta w celu określenia priorytetów schematów kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="05ac9-169">Aby uzyskać więcej informacji, zobacz [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="05ac9-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="05ac9-170">Algorytmy kompresji są uzależnione od kompromisu między szybkością kompresji i skuteczności kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="05ac9-171">*Efektywność* w tym kontekście odnosi się do rozmiaru danych wyjściowych po kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="05ac9-172">Najmniejszy rozmiar jest osiągany przez najbardziej *optymalną* kompresję.</span><span class="sxs-lookup"><span data-stu-id="05ac9-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="05ac9-173">W poniższej tabeli opisano nagłówki dotyczące żądania, wysyłania, buforowania i otrzymywania zawartości skompresowanej.</span><span class="sxs-lookup"><span data-stu-id="05ac9-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="05ac9-174">nagłówek</span><span class="sxs-lookup"><span data-stu-id="05ac9-174">Header</span></span>             | <span data-ttu-id="05ac9-175">Rola</span><span class="sxs-lookup"><span data-stu-id="05ac9-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="05ac9-176">Wysyłany z klienta do serwera w celu wskazania schematów kodowania zawartości akceptowalnych dla klienta.</span><span class="sxs-lookup"><span data-stu-id="05ac9-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="05ac9-177">Wysyłany z serwera do klienta, aby wskazać kodowanie zawartości ładunku.</span><span class="sxs-lookup"><span data-stu-id="05ac9-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="05ac9-178">W przypadku kompresowania następuje usunięcie nagłówka `Content-Length`, ponieważ zawartość treści zmienia się podczas kompresowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="05ac9-179">W przypadku kompresowania następuje usunięcie nagłówka `Content-MD5`, ponieważ zawartość treści została zmieniona i wartość skrótu nie jest już prawidłowa.</span><span class="sxs-lookup"><span data-stu-id="05ac9-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="05ac9-180">Określa typ MIME zawartości.</span><span class="sxs-lookup"><span data-stu-id="05ac9-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="05ac9-181">Każda odpowiedź powinna określać `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="05ac9-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="05ac9-182">Oprogramowanie pośredniczące sprawdza tę wartość, aby określić, czy odpowiedź powinna być skompresowana.</span><span class="sxs-lookup"><span data-stu-id="05ac9-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="05ac9-183">Oprogramowanie pośredniczące określa zestaw [domyślnych typów MIME](#mime-types) , które mogą być kodowane, ale można zastąpić lub dodać typy MIME.</span><span class="sxs-lookup"><span data-stu-id="05ac9-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="05ac9-184">W przypadku wysłania przez serwer z wartością `Accept-Encoding` do klientów i serwerów proxy, nagłówek `Vary` wskazuje na klienta lub serwer proxy, który powinien buforować (Zróżnicuj) odpowiedzi na podstawie wartości nagłówka `Accept-Encoding` żądania.</span><span class="sxs-lookup"><span data-stu-id="05ac9-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="05ac9-185">Wynik zwrócenia zawartości z nagłówkiem `Vary: Accept-Encoding` polega na tym, że skompresowane i nieskompresowane odpowiedzi są buforowane osobno.</span><span class="sxs-lookup"><span data-stu-id="05ac9-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="05ac9-186">Poznaj funkcje oprogramowania pośredniczącego kompresji odpowiedzi z [przykładową aplikacją](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="05ac9-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="05ac9-187">Przykład ilustruje:</span><span class="sxs-lookup"><span data-stu-id="05ac9-187">The sample illustrates:</span></span>

* <span data-ttu-id="05ac9-188">Kompresja odpowiedzi aplikacji przy użyciu strumienia gzip i niestandardowych dostawców kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="05ac9-189">Jak dodać typ MIME do domyślnej listy typów MIME do kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="05ac9-190">Package</span><span class="sxs-lookup"><span data-stu-id="05ac9-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="05ac9-191">Oprogramowanie pośredniczące kompresji odpowiedzi jest dostarczane przez pakiet [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , który jest niejawnie uwzględniony w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05ac9-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="05ac9-192">Aby uwzględnić oprogramowanie pośredniczące w projekcie, Dodaj odwołanie do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), który obejmuje pakiet [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .</span><span class="sxs-lookup"><span data-stu-id="05ac9-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="05ac9-193">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="05ac9-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="05ac9-194">Poniższy kod pokazuje, jak włączyć oprogramowanie pośredniczące kompresji odpowiedzi dla domyślnych typów MIME i dostawców kompresji ([Brotli](#brotli-compression-provider) i [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="05ac9-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="05ac9-195">Poniższy kod pokazuje, jak włączyć oprogramowanie pośredniczące kompresji odpowiedzi dla domyślnych typów MIME i [dostawcy kompresji gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="05ac9-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

<span data-ttu-id="05ac9-196">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="05ac9-196">Notes:</span></span>

* <span data-ttu-id="05ac9-197">`app.UseResponseCompression` musi zostać wywołana przed jakimkolwiek oprogramowanie pośredniczące, które kompresuje odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-197">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="05ac9-198">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index#middleware-order>.</span><span class="sxs-lookup"><span data-stu-id="05ac9-198">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="05ac9-199">Użyj narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)lub [Poster](https://www.getpostman.com/) , aby ustawić nagłówek żądania `Accept-Encoding` i zbadać nagłówki, rozmiar i treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="05ac9-200">Prześlij żądanie do przykładowej aplikacji bez nagłówka `Accept-Encoding` i zwróć uwagę na to, że odpowiedź jest nieskompresowana.</span><span class="sxs-lookup"><span data-stu-id="05ac9-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="05ac9-201">Nagłówki `Content-Encoding` i `Vary` nie są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Okno programu Fiddler przedstawiające wynik żądania bez nagłówka Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="05ac9-204">Prześlij żądanie do przykładowej aplikacji przy użyciu nagłówka `Accept-Encoding: br` (kompresji Brotli) i sprawdź, czy odpowiedź jest skompresowana.</span><span class="sxs-lookup"><span data-stu-id="05ac9-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="05ac9-205">Nagłówki `Content-Encoding` i `Vary` są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Okno programu Fiddler przedstawiające wynik żądania z nagłówkiem Accept-Encoding i wartością br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="05ac9-209">Prześlij żądanie do przykładowej aplikacji przy użyciu nagłówka `Accept-Encoding: gzip` i zwróć uwagę na to, że odpowiedź jest skompresowana.</span><span class="sxs-lookup"><span data-stu-id="05ac9-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="05ac9-210">Nagłówki `Content-Encoding` i `Vary` są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Okno programu Fiddler przedstawiające wynik żądania z nagłówkiem Accept-Encoding i wartością gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="05ac9-214">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="05ac9-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="05ac9-215">Dostawca kompresji Brotli</span><span class="sxs-lookup"><span data-stu-id="05ac9-215">Brotli Compression Provider</span></span>

<span data-ttu-id="05ac9-216">Użyj <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider>, aby skompresować odpowiedzi przy użyciu [formatu skompresowanych danych Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="05ac9-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="05ac9-217">Jeśli żaden dostawca kompresji nie zostanie jawnie dodany do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="05ac9-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="05ac9-218">Dostawca kompresji Brotli jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcą kompresji gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="05ac9-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="05ac9-219">Wartość domyślna kompresji to kompresja Brotli, gdy klient obsługuje format skompresowanych danych Brotli.</span><span class="sxs-lookup"><span data-stu-id="05ac9-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="05ac9-220">Jeśli klient Brotli nie jest obsługiwany przez klienta, kompresja jest domyślnie ustawiona na gzip, gdy klient obsługuje kompresję gzip.</span><span class="sxs-lookup"><span data-stu-id="05ac9-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="05ac9-221">Dostawca kompresji Brotoli należy dodać, gdy wszyscy dostawcy kompresji są jawnie dodani:</span><span class="sxs-lookup"><span data-stu-id="05ac9-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="05ac9-222">Ustaw poziom kompresji za pomocą <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="05ac9-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="05ac9-223">Dostawca kompresji Brotli domyślnie jest najszybszym poziomem kompresji ([CompressionLevel. najszybszy](xref:System.IO.Compression.CompressionLevel)), który może nie generować najbardziej wydajnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="05ac9-224">Jeśli wymagana jest najbardziej wydajna kompresja, Skonfiguruj oprogramowanie pośredniczące na potrzeby optymalnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="05ac9-225">Poziom kompresji</span><span class="sxs-lookup"><span data-stu-id="05ac9-225">Compression Level</span></span> | <span data-ttu-id="05ac9-226">Opis</span><span class="sxs-lookup"><span data-stu-id="05ac9-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="05ac9-227">CompressionLevel. najszybszy</span><span class="sxs-lookup"><span data-stu-id="05ac9-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="05ac9-228">Kompresja powinna zostać ukończona tak szybko, jak to możliwe, nawet jeśli wynikowe dane wyjściowe nie są optymalnie skompresowane.</span><span class="sxs-lookup"><span data-stu-id="05ac9-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="05ac9-229">CompressionLevel. nocompression</span><span class="sxs-lookup"><span data-stu-id="05ac9-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="05ac9-230">Nie należy wykonywać kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="05ac9-231">CompressionLevel. Optymalna</span><span class="sxs-lookup"><span data-stu-id="05ac9-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="05ac9-232">Odpowiedzi powinny być kompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu.</span><span class="sxs-lookup"><span data-stu-id="05ac9-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="05ac9-233">Dostawca kompresji gzip</span><span class="sxs-lookup"><span data-stu-id="05ac9-233">Gzip Compression Provider</span></span>

<span data-ttu-id="05ac9-234">Użyj <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider>, aby skompresować odpowiedzi w [formacie pliku gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="05ac9-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="05ac9-235">Jeśli żaden dostawca kompresji nie zostanie jawnie dodany do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="05ac9-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="05ac9-236">Dostawca kompresji gzip jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcą kompresji Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="05ac9-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="05ac9-237">Wartość domyślna kompresji to kompresja Brotli, gdy klient obsługuje format skompresowanych danych Brotli.</span><span class="sxs-lookup"><span data-stu-id="05ac9-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="05ac9-238">Jeśli klient Brotli nie jest obsługiwany przez klienta, kompresja jest domyślnie ustawiona na gzip, gdy klient obsługuje kompresję gzip.</span><span class="sxs-lookup"><span data-stu-id="05ac9-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="05ac9-239">Dostawca kompresji gzip jest domyślnie dodawany do tablicy dostawców kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="05ac9-240">Wartość domyślna kompresji to gzip, gdy klient obsługuje kompresję gzip.</span><span class="sxs-lookup"><span data-stu-id="05ac9-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="05ac9-241">Dostawca kompresji gzip należy dodać, gdy wszyscy dostawcy kompresji są jawnie dodani:</span><span class="sxs-lookup"><span data-stu-id="05ac9-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="05ac9-242">Ustaw poziom kompresji za pomocą <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="05ac9-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="05ac9-243">Dostawca kompresji gzip domyślnie jest najszybszym poziomem kompresji ([CompressionLevel. najszybszy](xref:System.IO.Compression.CompressionLevel)), który może nie generować najbardziej wydajnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="05ac9-244">Jeśli wymagana jest najbardziej wydajna kompresja, Skonfiguruj oprogramowanie pośredniczące na potrzeby optymalnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="05ac9-245">Poziom kompresji</span><span class="sxs-lookup"><span data-stu-id="05ac9-245">Compression Level</span></span> | <span data-ttu-id="05ac9-246">Opis</span><span class="sxs-lookup"><span data-stu-id="05ac9-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="05ac9-247">CompressionLevel. najszybszy</span><span class="sxs-lookup"><span data-stu-id="05ac9-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="05ac9-248">Kompresja powinna zostać ukończona tak szybko, jak to możliwe, nawet jeśli wynikowe dane wyjściowe nie są optymalnie skompresowane.</span><span class="sxs-lookup"><span data-stu-id="05ac9-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="05ac9-249">CompressionLevel. nocompression</span><span class="sxs-lookup"><span data-stu-id="05ac9-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="05ac9-250">Nie należy wykonywać kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="05ac9-251">CompressionLevel. Optymalna</span><span class="sxs-lookup"><span data-stu-id="05ac9-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="05ac9-252">Odpowiedzi powinny być kompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu.</span><span class="sxs-lookup"><span data-stu-id="05ac9-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="05ac9-253">Dostawcy niestandardowi</span><span class="sxs-lookup"><span data-stu-id="05ac9-253">Custom providers</span></span>

<span data-ttu-id="05ac9-254">Tworzenie niestandardowych implementacji kompresji przy użyciu <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="05ac9-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="05ac9-255"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> reprezentuje kodowanie zawartości, które tworzy `ICompressionProvider`.</span><span class="sxs-lookup"><span data-stu-id="05ac9-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="05ac9-256">Oprogramowanie pośredniczące używa tych informacji do wybrania dostawcy w oparciu o listę określoną w nagłówku `Accept-Encoding` żądania.</span><span class="sxs-lookup"><span data-stu-id="05ac9-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="05ac9-257">Za pomocą przykładowej aplikacji klient przesyła żądanie z nagłówkiem `Accept-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="05ac9-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="05ac9-258">Oprogramowanie pośredniczące używa niestandardowej implementacji kompresji i zwraca odpowiedź z nagłówkiem `Content-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="05ac9-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="05ac9-259">Aby Implementacja kompresji niestandardowej działała, klient musi być w stanie zdekompresować niestandardowe kodowanie.</span><span class="sxs-lookup"><span data-stu-id="05ac9-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="05ac9-260">Prześlij żądanie do przykładowej aplikacji z nagłówkiem `Accept-Encoding: mycustomcompression` i obserwuj nagłówki odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="05ac9-261">Nagłówki `Vary` i `Content-Encoding` są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="05ac9-262">Treść odpowiedzi (niepokazywana) nie jest skompresowana przez przykład.</span><span class="sxs-lookup"><span data-stu-id="05ac9-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="05ac9-263">Brak implementacji kompresji w klasie `CustomCompressionProvider` próbki.</span><span class="sxs-lookup"><span data-stu-id="05ac9-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="05ac9-264">Jednak przykład pokazuje, gdzie należy zaimplementować taki algorytm kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Okno programu Fiddler przedstawiające wynik żądania z nagłówkiem Accept-Encoding i wartością mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="05ac9-267">MIME, typy</span><span class="sxs-lookup"><span data-stu-id="05ac9-267">MIME types</span></span>

<span data-ttu-id="05ac9-268">Oprogramowanie pośredniczące określa domyślny zestaw typów MIME dla kompresji:</span><span class="sxs-lookup"><span data-stu-id="05ac9-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="05ac9-269">Zastąp lub Dołącz typy MIME z opcjami oprogramowania pośredniczącego kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="05ac9-270">Należy zauważyć, że symbole wieloznaczne MIME, takie jak `text/*` nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="05ac9-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="05ac9-271">Przykładowa aplikacja dodaje typ MIME dla `image/svg+xml` i kompresuje i służy do ASP.NET Core obrazu transparentu (*transparent. SVG*).</span><span class="sxs-lookup"><span data-stu-id="05ac9-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="05ac9-272">Kompresja z bezpiecznym protokołem</span><span class="sxs-lookup"><span data-stu-id="05ac9-272">Compression with secure protocol</span></span>

<span data-ttu-id="05ac9-273">Skompresowane odpowiedzi za pośrednictwem bezpiecznych połączeń mogą być kontrolowane przy użyciu opcji `EnableForHttps`, która jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="05ac9-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="05ac9-274">Używanie kompresji z dynamicznie generowanymi stronami może prowadzić do problemów z zabezpieczeniami, takich jak ataki w ramach [przestępczości](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszeń](https://wikipedia.org/wiki/BREACH_(security_exploit)) .</span><span class="sxs-lookup"><span data-stu-id="05ac9-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="05ac9-275">Dodawanie nagłówka Vary</span><span class="sxs-lookup"><span data-stu-id="05ac9-275">Adding the Vary header</span></span>

<span data-ttu-id="05ac9-276">Podczas kompresowania odpowiedzi na podstawie nagłówka `Accept-Encoding` istnieje potencjalnie wiele skompresowanych wersji odpowiedzi i nieskompresowanej wersji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="05ac9-277">Aby wymusić, że w pamięci podręcznej klienta i serwera proxy istnieją różne wersje i powinny być przechowywane, `Vary` nagłówk zostanie dodany z wartością `Accept-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="05ac9-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="05ac9-278">W ASP.NET Core 2,0 lub nowszej, oprogramowanie pośredniczące automatycznie dodaje nagłówek `Vary`, gdy odpowiedź zostanie skompresowana.</span><span class="sxs-lookup"><span data-stu-id="05ac9-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="05ac9-279">Problem dotyczący oprogramowania pośredniczącego w przypadku Nginx zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="05ac9-279">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="05ac9-280">Gdy żądanie jest przekazywane przez Nginx, nagłówek `Accept-Encoding` jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="05ac9-280">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="05ac9-281">Usunięcie nagłówka `Accept-Encoding` zapobiega kompresji odpowiedzi przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="05ac9-281">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="05ac9-282">Aby uzyskać więcej informacji, zobacz [Nginx: kompresja i dekompresja](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="05ac9-282">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="05ac9-283">Ten problem jest śledzony przez [ilustrację kompresji przekazującej dla Nginx (ASPNET/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="05ac9-283">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="05ac9-284">Praca z kompresją dynamiczną usług IIS</span><span class="sxs-lookup"><span data-stu-id="05ac9-284">Working with IIS dynamic compression</span></span>

<span data-ttu-id="05ac9-285">Jeśli masz aktywny moduł dynamicznej kompresji usług IIS skonfigurowany na poziomie serwera, który chcesz wyłączyć dla aplikacji, wyłącz moduł z dodatkiem do pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="05ac9-285">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="05ac9-286">Aby uzyskać więcej informacji, zobacz [wyłączanie modułów IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="05ac9-286">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="05ac9-287">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="05ac9-287">Troubleshooting</span></span>

<span data-ttu-id="05ac9-288">Użyj narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)lub [Poster](https://www.getpostman.com/), które pozwala ustawić nagłówek żądania `Accept-Encoding` i zbadać nagłówki, rozmiar i treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-288">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="05ac9-289">Domyślnie oprogramowanie pośredniczące kompresji odpowiedzi kompresuje odpowiedzi, które spełniają następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="05ac9-289">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="05ac9-290">Nagłówek `Accept-Encoding` jest obecny z wartością `br`, `gzip`, `*`lub kodowaniem niestandardowym, które pasuje do utworzonego niestandardowego dostawcy kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-290">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="05ac9-291">Wartość nie może być `identity` lub mieć ustawienie wartości jakości (qvalue, `q`) równe 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="05ac9-291">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="05ac9-292">Typ MIME (`Content-Type`) musi być ustawiony i musi być zgodny z typem MIME skonfigurowanym na <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="05ac9-292">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="05ac9-293">Żądanie nie może zawierać nagłówka `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="05ac9-293">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="05ac9-294">Żądanie musi korzystać z protokołu niezabezpieczonego (http), o ile nie skonfigurowano protokołu Secure Protocol (https) w opcjach oprogramowania pośredniczącego kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-294">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="05ac9-295">*Należy pamiętać o niebezpieczeństwie [opisanym powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznej kompresji zawartości.*</span><span class="sxs-lookup"><span data-stu-id="05ac9-295">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="05ac9-296">Nagłówek `Accept-Encoding` jest obecny z wartością `gzip`, `*`lub kodowaniem niestandardowym, które jest zgodne z ustanowionym przez Ciebie niestandardowym dostawcą kompresji.</span><span class="sxs-lookup"><span data-stu-id="05ac9-296">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="05ac9-297">Wartość nie może być `identity` lub mieć ustawienie wartości jakości (qvalue, `q`) równe 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="05ac9-297">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="05ac9-298">Typ MIME (`Content-Type`) musi być ustawiony i musi być zgodny z typem MIME skonfigurowanym na <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="05ac9-298">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="05ac9-299">Żądanie nie może zawierać nagłówka `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="05ac9-299">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="05ac9-300">Żądanie musi korzystać z protokołu niezabezpieczonego (http), o ile nie skonfigurowano protokołu Secure Protocol (https) w opcjach oprogramowania pośredniczącego kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="05ac9-300">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="05ac9-301">*Należy pamiętać o niebezpieczeństwie [opisanym powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznej kompresji zawartości.*</span><span class="sxs-lookup"><span data-stu-id="05ac9-301">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="05ac9-302">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="05ac9-302">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="05ac9-303">Sieć deweloperów Mozilla: akceptowanie kodowania</span><span class="sxs-lookup"><span data-stu-id="05ac9-303">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="05ac9-304">Sekcja RFC 7231 3.1.2.1: kodowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="05ac9-304">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="05ac9-305">RFC 7230, sekcja 4.2.3: kodowanie gzip</span><span class="sxs-lookup"><span data-stu-id="05ac9-305">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="05ac9-306">Specyfikacja formatu pliku GZIP w wersji 4,3</span><span class="sxs-lookup"><span data-stu-id="05ac9-306">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
