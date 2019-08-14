---
title: Kompresja odpowiedzi w ASP.NET Core
author: guardrex
description: Dowiedz się więcej o kompresji odpowiedzi i sposobach używania oprogramowania pośredniczącego kompresji odpowiedzi w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/09/2019
uid: performance/response-compression
ms.openlocfilehash: e320e87179f9f1b9773a55c380684a3f3f712632
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993471"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="f3dd6-103">Kompresja odpowiedzi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3dd6-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="f3dd6-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f3dd6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f3dd6-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f3dd6-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f3dd6-106">Przepustowość sieci jest ograniczonym zasobem.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="f3dd6-107">Zmniejszenie rozmiaru odpowiedzi zwykle zwiększa czas odpowiedzi aplikacji, często znacząco.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="f3dd6-108">Jednym ze sposobów zmniejszenia rozmiaru ładunku jest kompresowanie odpowiedzi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="f3dd6-109">Kiedy używać oprogramowania pośredniczącego kompresji odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="f3dd6-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="f3dd6-110">Użyj technologii kompresji odpowiedzi opartej na serwerze w usługach IIS, Apache lub nginx.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="f3dd6-111">Wydajność oprogramowania pośredniczącego prawdopodobnie nie jest zgodna z tymi, które są używane w modułach serwera.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="f3dd6-112">Serwer [serwerowy http. sys](xref:fundamentals/servers/httpsys) i serwer [Kestrel](xref:fundamentals/servers/kestrel) nie oferują obecnie wbudowanej obsługi kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="f3dd6-113">Użyj oprogramowania pośredniczącego kompresji odpowiedzi, gdy jesteś:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="f3dd6-114">Nie można użyć następujących technologii kompresji opartych na serwerze:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="f3dd6-115">Moduł kompresji dynamicznej usług IIS</span><span class="sxs-lookup"><span data-stu-id="f3dd6-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="f3dd6-116">Moduł Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="f3dd6-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="f3dd6-117">Kompresja i dekompresja Nginx</span><span class="sxs-lookup"><span data-stu-id="f3dd6-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="f3dd6-118">Hosting bezpośrednio na:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-118">Hosting directly on:</span></span>
  * <span data-ttu-id="f3dd6-119">[Serwer http. sys](xref:fundamentals/servers/httpsys) (dawniej nazywany webListener)</span><span class="sxs-lookup"><span data-stu-id="f3dd6-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="f3dd6-120">Serwer Kestrel</span><span class="sxs-lookup"><span data-stu-id="f3dd6-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="f3dd6-121">Kompresja odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="f3dd6-121">Response compression</span></span>

<span data-ttu-id="f3dd6-122">Zazwyczaj Każda odpowiedź nieskompresowana natywnie może korzystać z kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="f3dd6-123">Odpowiedzi nienatywnie skompresowane zazwyczaj obejmują: CSS, JavaScript, HTML, XML i JSON.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="f3dd6-124">Nie należy kompresować natywnie skompresowanych zasobów, takich jak pliki PNG.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="f3dd6-125">W przypadku próby przeprowadzenia dalszej kompresji natywnie skompresowanej odpowiedzi wszystkie niewielkie dodatkowe zmniejszenie rozmiaru i czasu transmisji będą prawdopodobnie przesłonięte przez czas potrzebny do przetworzenia kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="f3dd6-126">Nie Kompresuj plików mniejszych niż około 150-1000 bajtów (w zależności od zawartości pliku i wydajności kompresji).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="f3dd6-127">Narzuty kompresowania małych plików może generować skompresowany plik większy niż plik nieskompresowany.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="f3dd6-128">Gdy klient może przetwarzać skompresowaną zawartość, klient musi poinformować serwer o swoich możliwościach, wysyłając `Accept-Encoding` nagłówek z żądaniem.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="f3dd6-129">Gdy serwer wysyła skompresowaną zawartość, musi zawierać informacje w `Content-Encoding` nagłówku dotyczące sposobu kodowania skompresowanej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="f3dd6-130">Oznaczenia kodowania zawartości obsługiwane przez oprogramowanie pośredniczące przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="f3dd6-131">`Accept-Encoding`wartości nagłówka</span><span class="sxs-lookup"><span data-stu-id="f3dd6-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="f3dd6-132">Obsługiwane oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="f3dd6-132">Middleware Supported</span></span> | <span data-ttu-id="f3dd6-133">Opis</span><span class="sxs-lookup"><span data-stu-id="f3dd6-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="f3dd6-134">Tak (domyślnie)</span><span class="sxs-lookup"><span data-stu-id="f3dd6-134">Yes (default)</span></span>        | [<span data-ttu-id="f3dd6-135">Format skompresowanych danych Brotli</span><span class="sxs-lookup"><span data-stu-id="f3dd6-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="f3dd6-136">Nie</span><span class="sxs-lookup"><span data-stu-id="f3dd6-136">No</span></span>                   | [<span data-ttu-id="f3dd6-137">WKLĘŚNIĘCIE — skompresowany format danych</span><span class="sxs-lookup"><span data-stu-id="f3dd6-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="f3dd6-138">Nie</span><span class="sxs-lookup"><span data-stu-id="f3dd6-138">No</span></span>                   | [<span data-ttu-id="f3dd6-139">Wydajna wymiana XML</span><span class="sxs-lookup"><span data-stu-id="f3dd6-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="f3dd6-140">Tak</span><span class="sxs-lookup"><span data-stu-id="f3dd6-140">Yes</span></span>                  | [<span data-ttu-id="f3dd6-141">Format pliku gzip</span><span class="sxs-lookup"><span data-stu-id="f3dd6-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="f3dd6-142">Tak</span><span class="sxs-lookup"><span data-stu-id="f3dd6-142">Yes</span></span>                  | <span data-ttu-id="f3dd6-143">Identyfikator "bez kodowania": Odpowiedź nie może być zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="f3dd6-144">Nie</span><span class="sxs-lookup"><span data-stu-id="f3dd6-144">No</span></span>                   | [<span data-ttu-id="f3dd6-145">Format transferu sieciowego dla archiwów języka Java</span><span class="sxs-lookup"><span data-stu-id="f3dd6-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="f3dd6-146">Tak</span><span class="sxs-lookup"><span data-stu-id="f3dd6-146">Yes</span></span>                  | <span data-ttu-id="f3dd6-147">Wszystkie dostępne kodowanie zawartości nie jest jawnie wymagane</span><span class="sxs-lookup"><span data-stu-id="f3dd6-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="f3dd6-148">`Accept-Encoding`wartości nagłówka</span><span class="sxs-lookup"><span data-stu-id="f3dd6-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="f3dd6-149">Obsługiwane oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="f3dd6-149">Middleware Supported</span></span> | <span data-ttu-id="f3dd6-150">Opis</span><span class="sxs-lookup"><span data-stu-id="f3dd6-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="f3dd6-151">Nie</span><span class="sxs-lookup"><span data-stu-id="f3dd6-151">No</span></span>                   | [<span data-ttu-id="f3dd6-152">Format skompresowanych danych Brotli</span><span class="sxs-lookup"><span data-stu-id="f3dd6-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="f3dd6-153">Nie</span><span class="sxs-lookup"><span data-stu-id="f3dd6-153">No</span></span>                   | [<span data-ttu-id="f3dd6-154">WKLĘŚNIĘCIE — skompresowany format danych</span><span class="sxs-lookup"><span data-stu-id="f3dd6-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="f3dd6-155">Nie</span><span class="sxs-lookup"><span data-stu-id="f3dd6-155">No</span></span>                   | [<span data-ttu-id="f3dd6-156">Wydajna wymiana XML</span><span class="sxs-lookup"><span data-stu-id="f3dd6-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="f3dd6-157">Tak (domyślnie)</span><span class="sxs-lookup"><span data-stu-id="f3dd6-157">Yes (default)</span></span>        | [<span data-ttu-id="f3dd6-158">Format pliku gzip</span><span class="sxs-lookup"><span data-stu-id="f3dd6-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="f3dd6-159">Tak</span><span class="sxs-lookup"><span data-stu-id="f3dd6-159">Yes</span></span>                  | <span data-ttu-id="f3dd6-160">Identyfikator "bez kodowania": Odpowiedź nie może być zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="f3dd6-161">Nie</span><span class="sxs-lookup"><span data-stu-id="f3dd6-161">No</span></span>                   | [<span data-ttu-id="f3dd6-162">Format transferu sieciowego dla archiwów języka Java</span><span class="sxs-lookup"><span data-stu-id="f3dd6-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="f3dd6-163">Tak</span><span class="sxs-lookup"><span data-stu-id="f3dd6-163">Yes</span></span>                  | <span data-ttu-id="f3dd6-164">Wszystkie dostępne kodowanie zawartości nie jest jawnie wymagane</span><span class="sxs-lookup"><span data-stu-id="f3dd6-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="f3dd6-165">Aby uzyskać więcej informacji, zapoznaj się z [listą oficjalnych kodowania zawartości organizacji Iana](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="f3dd6-166">Oprogramowanie pośredniczące umożliwia dodanie dodatkowych dostawców kompresji dla niestandardowych `Accept-Encoding` wartości nagłówków.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="f3dd6-167">Aby uzyskać więcej informacji, zobacz [niestandardowe dostawcy](#custom-providers) poniżej.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="f3dd6-168">Oprogramowanie pośredniczące może resłużyć do rozważenia wartości jakości (qvalue `q`), gdy są wysyłane przez klienta w celu określenia priorytetów schematów kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="f3dd6-169">Aby uzyskać więcej informacji, [zobacz RFC 7231: Zaakceptuj — kodowanie](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="f3dd6-170">Algorytmy kompresji są uzależnione od kompromisu między szybkością kompresji i skuteczności kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="f3dd6-171">*Efektywność* w tym kontekście odnosi się do rozmiaru danych wyjściowych po kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="f3dd6-172">Najmniejszy rozmiar jest osiągany przez najbardziej *optymalną* kompresję.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="f3dd6-173">W poniższej tabeli opisano nagłówki dotyczące żądania, wysyłania, buforowania i otrzymywania zawartości skompresowanej.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="f3dd6-174">nagłówek</span><span class="sxs-lookup"><span data-stu-id="f3dd6-174">Header</span></span>             | <span data-ttu-id="f3dd6-175">Role</span><span class="sxs-lookup"><span data-stu-id="f3dd6-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="f3dd6-176">Wysyłany z klienta do serwera w celu wskazania schematów kodowania zawartości akceptowalnych dla klienta.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="f3dd6-177">Wysyłany z serwera do klienta, aby wskazać kodowanie zawartości ładunku.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="f3dd6-178">W `Content-Length` przypadku kompresowania następuje usunięcie nagłówka, ponieważ zawartość treści zmienia się podczas kompresowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="f3dd6-179">W `Content-MD5` przypadku kompresowania następuje usunięcie nagłówka, ponieważ zawartość treści została zmieniona i wartość skrótu nie jest już prawidłowa.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="f3dd6-180">Określa typ MIME zawartości.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="f3dd6-181">Należy określić `Content-Type`dla każdej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="f3dd6-182">Oprogramowanie pośredniczące sprawdza tę wartość, aby określić, czy odpowiedź powinna być skompresowana.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="f3dd6-183">Oprogramowanie pośredniczące określa zestaw [domyślnych typów MIME](#mime-types) , które mogą być kodowane, ale można zastąpić lub dodać typy MIME.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="f3dd6-184">W `Accept-Encoding` `Accept-Encoding` przypadku wysłania przez serwer z wartością do klientów i serwerów proxy nagłówek wskazuje na klienta lub serwer proxy, który powinien buforować (Zróżnicuj) odpowiedzi na podstawie wartości nagłówka żądania. `Vary`</span><span class="sxs-lookup"><span data-stu-id="f3dd6-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="f3dd6-185">Wynik zwrócenia zawartości z `Vary: Accept-Encoding` nagłówkiem polega na tym, że skompresowane i nieskompresowane odpowiedzi są buforowane osobno.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="f3dd6-186">Poznaj funkcje oprogramowania pośredniczącego kompresji odpowiedzi z [przykładową aplikacją](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="f3dd6-187">Przykład ilustruje:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-187">The sample illustrates:</span></span>

* <span data-ttu-id="f3dd6-188">Kompresja odpowiedzi aplikacji przy użyciu strumienia gzip i niestandardowych dostawców kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="f3dd6-189">Jak dodać typ MIME do domyślnej listy typów MIME do kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="f3dd6-190">Package</span><span class="sxs-lookup"><span data-stu-id="f3dd6-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f3dd6-191">Oprogramowanie pośredniczące kompresji odpowiedzi jest dostarczane przez pakiet [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , który jest niejawnie uwzględniony w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f3dd6-192">Aby uwzględnić oprogramowanie pośredniczące w projekcie, Dodaj odwołanie do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), który obejmuje pakiet [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .</span><span class="sxs-lookup"><span data-stu-id="f3dd6-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="f3dd6-193">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="f3dd6-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f3dd6-194">Poniższy kod pokazuje, jak włączyć oprogramowanie pośredniczące kompresji odpowiedzi dla domyślnych typów MIME i dostawców kompresji ([Brotli](#brotli-compression-provider) i [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="f3dd6-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f3dd6-195">Poniższy kod pokazuje, jak włączyć oprogramowanie pośredniczące kompresji odpowiedzi dla domyślnych typów MIME i [dostawcy kompresji gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="f3dd6-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="f3dd6-196">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-196">Notes:</span></span>

* <span data-ttu-id="f3dd6-197">`app.UseResponseCompression`musi być wywoływana przed `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-197">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="f3dd6-198">Użyj narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)lub [Poster](https://www.getpostman.com/) , aby ustawić `Accept-Encoding` nagłówek żądania i zbadać nagłówki, rozmiar i treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-198">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="f3dd6-199">Prześlij żądanie do przykładowej aplikacji bez `Accept-Encoding` nagłówka i zwróć uwagę na to, że odpowiedź jest nieskompresowana.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-199">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="f3dd6-200">Nagłówki `Content-Encoding` i`Vary` nie są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-200">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Okno programu Fiddler przedstawiające wynik żądania bez nagłówka Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f3dd6-203">Prześlij żądanie do aplikacji przykładowej przy użyciu `Accept-Encoding: br` nagłówka (kompresji Brotli) i sprawdź, czy odpowiedź jest skompresowana.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-203">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="f3dd6-204">W odpowiedzi `Vary` znajdują się nagłówki i.`Content-Encoding`</span><span class="sxs-lookup"><span data-stu-id="f3dd6-204">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Okno programu Fiddler przedstawiające wynik żądania z nagłówkiem Accept-Encoding i wartością br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f3dd6-208">Prześlij żądanie do przykładowej aplikacji z `Accept-Encoding: gzip` nagłówkiem i obserwuj, że odpowiedź jest skompresowana.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-208">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="f3dd6-209">W odpowiedzi `Vary` znajdują się nagłówki i.`Content-Encoding`</span><span class="sxs-lookup"><span data-stu-id="f3dd6-209">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Okno programu Fiddler przedstawiające wynik żądania z nagłówkiem Accept-Encoding i wartością gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="f3dd6-213">Udostępnia</span><span class="sxs-lookup"><span data-stu-id="f3dd6-213">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="f3dd6-214">Dostawca kompresji Brotli</span><span class="sxs-lookup"><span data-stu-id="f3dd6-214">Brotli Compression Provider</span></span>

<span data-ttu-id="f3dd6-215">Użyj, <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> Aby skompresować odpowiedzi w [formacie skompresowanych danych Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-215">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="f3dd6-216">Jeśli żaden dostawca kompresji nie zostanie jawnie dodany do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-216">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="f3dd6-217">Dostawca kompresji Brotli jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcą kompresji gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-217">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="f3dd6-218">Wartość domyślna kompresji to kompresja Brotli, gdy klient obsługuje format skompresowanych danych Brotli.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-218">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="f3dd6-219">Jeśli klient Brotli nie jest obsługiwany przez klienta, kompresja jest domyślnie ustawiona na gzip, gdy klient obsługuje kompresję gzip.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-219">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="f3dd6-220">Dostawca kompresji Brotoli należy dodać, gdy wszyscy dostawcy kompresji są jawnie dodani:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-220">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f3dd6-221">Ustaw poziom kompresji za pomocą <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-221">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="f3dd6-222">Dostawca kompresji Brotli domyślnie jest najszybszym poziomem kompresji ([CompressionLevel. najszybszy](xref:System.IO.Compression.CompressionLevel)), który może nie generować najbardziej wydajnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-222">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="f3dd6-223">Jeśli wymagana jest najbardziej wydajna kompresja, Skonfiguruj oprogramowanie pośredniczące na potrzeby optymalnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-223">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="f3dd6-224">Poziom kompresji</span><span class="sxs-lookup"><span data-stu-id="f3dd6-224">Compression Level</span></span> | <span data-ttu-id="f3dd6-225">Opis</span><span class="sxs-lookup"><span data-stu-id="f3dd6-225">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="f3dd6-226">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="f3dd6-226">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="f3dd6-227">Kompresja powinna zostać ukończona tak szybko, jak to możliwe, nawet jeśli wynikowe dane wyjściowe nie są optymalnie skompresowane.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-227">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="f3dd6-228">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="f3dd6-228">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="f3dd6-229">Nie należy wykonywać kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-229">No compression should be performed.</span></span> |
| [<span data-ttu-id="f3dd6-230">CompressionLevel. Optymalna</span><span class="sxs-lookup"><span data-stu-id="f3dd6-230">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="f3dd6-231">Odpowiedzi powinny być kompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-231">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="f3dd6-232">Dostawca kompresji gzip</span><span class="sxs-lookup"><span data-stu-id="f3dd6-232">Gzip Compression Provider</span></span>

<span data-ttu-id="f3dd6-233">Użyj, <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> Aby skompresować odpowiedzi w [formacie pliku gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-233">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="f3dd6-234">Jeśli żaden dostawca kompresji nie zostanie jawnie dodany do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-234">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f3dd6-235">Dostawca kompresji gzip jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcą kompresji Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-235">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="f3dd6-236">Wartość domyślna kompresji to kompresja Brotli, gdy klient obsługuje format skompresowanych danych Brotli.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-236">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="f3dd6-237">Jeśli klient Brotli nie jest obsługiwany przez klienta, kompresja jest domyślnie ustawiona na gzip, gdy klient obsługuje kompresję gzip.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-237">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="f3dd6-238">Dostawca kompresji gzip jest domyślnie dodawany do tablicy dostawców kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-238">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="f3dd6-239">Wartość domyślna kompresji to gzip, gdy klient obsługuje kompresję gzip.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-239">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="f3dd6-240">Dostawca kompresji gzip należy dodać, gdy wszyscy dostawcy kompresji są jawnie dodani:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-240">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="f3dd6-241">Ustaw poziom kompresji za pomocą <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-241">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="f3dd6-242">Dostawca kompresji gzip domyślnie jest najszybszym poziomem kompresji ([CompressionLevel. najszybszy](xref:System.IO.Compression.CompressionLevel)), który może nie generować najbardziej wydajnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-242">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="f3dd6-243">Jeśli wymagana jest najbardziej wydajna kompresja, Skonfiguruj oprogramowanie pośredniczące na potrzeby optymalnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-243">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="f3dd6-244">Poziom kompresji</span><span class="sxs-lookup"><span data-stu-id="f3dd6-244">Compression Level</span></span> | <span data-ttu-id="f3dd6-245">Opis</span><span class="sxs-lookup"><span data-stu-id="f3dd6-245">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="f3dd6-246">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="f3dd6-246">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="f3dd6-247">Kompresja powinna zostać ukończona tak szybko, jak to możliwe, nawet jeśli wynikowe dane wyjściowe nie są optymalnie skompresowane.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-247">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="f3dd6-248">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="f3dd6-248">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="f3dd6-249">Nie należy wykonywać kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-249">No compression should be performed.</span></span> |
| [<span data-ttu-id="f3dd6-250">CompressionLevel. Optymalna</span><span class="sxs-lookup"><span data-stu-id="f3dd6-250">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="f3dd6-251">Odpowiedzi powinny być kompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-251">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="f3dd6-252">Dostawcy niestandardowi</span><span class="sxs-lookup"><span data-stu-id="f3dd6-252">Custom providers</span></span>

<span data-ttu-id="f3dd6-253">Tworzenie niestandardowych implementacji kompresji przy <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>użyciu programu.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-253">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="f3dd6-254">Reprezentuje kodowanie zawartości `ICompressionProvider` , które tworzy. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*></span><span class="sxs-lookup"><span data-stu-id="f3dd6-254">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="f3dd6-255">Oprogramowanie pośredniczące używa tych informacji do wybrania dostawcy w oparciu o listę określoną w `Accept-Encoding` nagłówku żądania.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-255">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="f3dd6-256">Za pomocą przykładowej aplikacji klient przesyła żądanie z `Accept-Encoding: mycustomcompression` nagłówkiem.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-256">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="f3dd6-257">Oprogramowanie pośredniczące używa niestandardowej implementacji kompresji i zwraca odpowiedź z `Content-Encoding: mycustomcompression` nagłówkiem.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-257">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="f3dd6-258">Aby Implementacja kompresji niestandardowej działała, klient musi być w stanie zdekompresować niestandardowe kodowanie.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-258">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f3dd6-259">Prześlij żądanie do przykładowej aplikacji z `Accept-Encoding: mycustomcompression` nagłówkiem i obserwuj nagłówki odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-259">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="f3dd6-260">W odpowiedzi `Content-Encoding` znajdują się nagłówki i.`Vary`</span><span class="sxs-lookup"><span data-stu-id="f3dd6-260">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="f3dd6-261">Treść odpowiedzi (niepokazywana) nie jest skompresowana przez przykład.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-261">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="f3dd6-262">Brak implementacji kompresji w `CustomCompressionProvider` klasie przykładowej.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-262">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="f3dd6-263">Jednak przykład pokazuje, gdzie należy zaimplementować taki algorytm kompresji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-263">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Okno programu Fiddler przedstawiające wynik żądania z nagłówkiem Accept-Encoding i wartością mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="f3dd6-266">MIME, typy</span><span class="sxs-lookup"><span data-stu-id="f3dd6-266">MIME types</span></span>

<span data-ttu-id="f3dd6-267">Oprogramowanie pośredniczące określa domyślny zestaw typów MIME dla kompresji:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-267">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="f3dd6-268">Zastąp lub Dołącz typy MIME z opcjami oprogramowania pośredniczącego kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-268">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="f3dd6-269">Należy zauważyć, że symbole wieloznaczne MIME `text/*` , takie jak nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-269">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="f3dd6-270">Przykładowa aplikacja dodaje typ MIME dla `image/svg+xml` i kompresuje i obsługuje obraz transparentu ASP.NET Core (*transparent. SVG*).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-270">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="f3dd6-271">Kompresja z bezpiecznym protokołem</span><span class="sxs-lookup"><span data-stu-id="f3dd6-271">Compression with secure protocol</span></span>

<span data-ttu-id="f3dd6-272">Skompresowane odpowiedzi za pośrednictwem bezpiecznych połączeń można kontrolować przy `EnableForHttps` użyciu opcji, która jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-272">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="f3dd6-273">Używanie kompresji z dynamicznie generowanymi stronami może prowadzić do problemów z zabezpieczeniami, takich jak ataki w ramach [przestępczości](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszeń](https://wikipedia.org/wiki/BREACH_(security_exploit)) .</span><span class="sxs-lookup"><span data-stu-id="f3dd6-273">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="f3dd6-274">Dodawanie nagłówka Vary</span><span class="sxs-lookup"><span data-stu-id="f3dd6-274">Adding the Vary header</span></span>

<span data-ttu-id="f3dd6-275">Podczas kompresowania odpowiedzi na podstawie `Accept-Encoding` nagłówka istnieje potencjalnie wiele skompresowanych wersji odpowiedzi i nieskompresowanej wersji.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-275">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="f3dd6-276">Aby wymusić, że w pamięci podręcznej klienta i serwera proxy istnieją różne wersje i `Vary` powinny być przechowywane, nagłówek `Accept-Encoding` zostanie dodany z wartością.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-276">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="f3dd6-277">W ASP.NET Core 2,0 lub nowszej, oprogramowanie pośredniczące automatycznie dodaje `Vary` nagłówek podczas kompresowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-277">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="f3dd6-278">Problem dotyczący oprogramowania pośredniczącego w przypadku Nginx zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="f3dd6-278">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="f3dd6-279">Gdy żądanie jest przekazywane przez Nginx, `Accept-Encoding` nagłówek zostaje usunięty.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-279">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="f3dd6-280">`Accept-Encoding` Usunięcie nagłówka zapobiega kompresji odpowiedzi przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-280">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="f3dd6-281">Aby uzyskać więcej informacji, [Zobacz Nginx: Kompresja i dekompresja](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-281">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="f3dd6-282">Ten problem jest śledzony przez [ilustrację kompresji przekazującej dla Nginx (ASPNET/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-282">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="f3dd6-283">Praca z kompresją dynamiczną usług IIS</span><span class="sxs-lookup"><span data-stu-id="f3dd6-283">Working with IIS dynamic compression</span></span>

<span data-ttu-id="f3dd6-284">Jeśli masz aktywny moduł dynamicznej kompresji usług IIS skonfigurowany na poziomie serwera, który chcesz wyłączyć dla aplikacji, wyłącz moduł z dodatkiem do pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="f3dd6-284">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="f3dd6-285">Aby uzyskać więcej informacji, zobacz [wyłączanie modułów IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-285">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f3dd6-286">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="f3dd6-286">Troubleshooting</span></span>

<span data-ttu-id="f3dd6-287">Użyj narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)lub [Poster](https://www.getpostman.com/), które `Accept-Encoding` pozwala ustawić nagłówek żądania i zbadać nagłówki, rozmiar i treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-287">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="f3dd6-288">Domyślnie oprogramowanie pośredniczące kompresji odpowiedzi kompresuje odpowiedzi, które spełniają następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="f3dd6-288">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f3dd6-289">Nagłówek jest obecny z `br`wartością, `gzip`, `*`lub kodowaniem niestandardowym, które jest zgodne z ustanowionym przez Ciebie niestandardowym dostawcą kompresji. `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="f3dd6-289">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="f3dd6-290">Wartość nie może być `identity` lub mieć ustawienie wartości jakości (qvalue, `q`) równe 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-290">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="f3dd6-291">Typ MIME (`Content-Type`) musi być ustawiony i musi być zgodny z typem MIME skonfigurowanym <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>na.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-291">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="f3dd6-292">Żądanie nie może zawierać `Content-Range` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-292">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="f3dd6-293">Żądanie musi korzystać z protokołu niezabezpieczonego (http), o ile nie skonfigurowano protokołu Secure Protocol (https) w opcjach oprogramowania pośredniczącego kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-293">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="f3dd6-294">*Należy pamiętać o niebezpieczeństwie [opisanym powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznej kompresji zawartości.*</span><span class="sxs-lookup"><span data-stu-id="f3dd6-294">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="f3dd6-295">Nagłówek jest obecny z `gzip`wartością `*`lub kodowaniem niestandardowym, które pasuje do utworzonego niestandardowego dostawcy kompresji. `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="f3dd6-295">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="f3dd6-296">Wartość nie może być `identity` lub mieć ustawienie wartości jakości (qvalue, `q`) równe 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="f3dd6-296">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="f3dd6-297">Typ MIME (`Content-Type`) musi być ustawiony i musi być zgodny z typem MIME skonfigurowanym <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>na.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-297">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="f3dd6-298">Żądanie nie może zawierać `Content-Range` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-298">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="f3dd6-299">Żądanie musi korzystać z protokołu niezabezpieczonego (http), o ile nie skonfigurowano protokołu Secure Protocol (https) w opcjach oprogramowania pośredniczącego kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3dd6-299">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="f3dd6-300">*Należy pamiętać o niebezpieczeństwie [opisanym powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznej kompresji zawartości.*</span><span class="sxs-lookup"><span data-stu-id="f3dd6-300">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f3dd6-301">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f3dd6-301">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="f3dd6-302">Mozilla Developer Network: Akceptuj — kodowanie</span><span class="sxs-lookup"><span data-stu-id="f3dd6-302">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="f3dd6-303">3.1.2.1 sekcja RFC 7231: Kodowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="f3dd6-303">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="f3dd6-304">Sekcja 4.2.3 RFC 7230: Kodowanie gzip</span><span class="sxs-lookup"><span data-stu-id="f3dd6-304">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="f3dd6-305">Specyfikacja formatu pliku GZIP w wersji 4,3</span><span class="sxs-lookup"><span data-stu-id="f3dd6-305">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
