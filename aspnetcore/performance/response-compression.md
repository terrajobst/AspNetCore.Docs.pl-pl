---
title: Kompresja odpowiedzi w programie ASP.NET Core
author: guardrex
description: Informacje o kompresji odpowiedzi i sposobie używania oprogramowanie pośredniczące kompresji odpowiedzi w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: performance/response-compression
ms.openlocfilehash: 2516fbb30e55990dc4ad0d92069853bc26874bc9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861891"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="3737c-103">Kompresja odpowiedzi w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3737c-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="3737c-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3737c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3737c-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3737c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3737c-106">Przepustowość sieci jest ograniczona zasobów.</span><span class="sxs-lookup"><span data-stu-id="3737c-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="3737c-107">Zazwyczaj zmniejszenie rozmiaru odpowiedzi często znacznie zwiększa szybkość reakcji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3737c-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="3737c-108">Jednym ze sposobów, aby zmniejszyć rozmiar ładunku jest kompresji odpowiedzi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3737c-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="3737c-109">Kiedy należy używać oprogramowanie pośredniczące kompresji odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3737c-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="3737c-110">Użyj technologii kompresji odpowiedzi na serwerze usług IIS, Apache i Nginx.</span><span class="sxs-lookup"><span data-stu-id="3737c-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="3737c-111">Wydajność oprogramowania pośredniczącego prawdopodobnie nie będzie zgodne z modułów serwera.</span><span class="sxs-lookup"><span data-stu-id="3737c-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="3737c-112">[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) serwera i [Kestrel](xref:fundamentals/servers/kestrel) server obecnie nie oferują obsługę wbudowanych kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="3737c-113">Oprogramowanie pośredniczące kompresji odpowiedzi należy użyć, jeśli:</span><span class="sxs-lookup"><span data-stu-id="3737c-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="3737c-114">Nie można użyć następujących technologii serwerowych kompresji:</span><span class="sxs-lookup"><span data-stu-id="3737c-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="3737c-115">Moduł dynamicznej kompresji usług IIS</span><span class="sxs-lookup"><span data-stu-id="3737c-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="3737c-116">Moduł mod_deflate Apache</span><span class="sxs-lookup"><span data-stu-id="3737c-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="3737c-117">Serwer Nginx kompresja i Dekompresja</span><span class="sxs-lookup"><span data-stu-id="3737c-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="3737c-118">Hosting bezpośrednio na:</span><span class="sxs-lookup"><span data-stu-id="3737c-118">Hosting directly on:</span></span>
  * <span data-ttu-id="3737c-119">[Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) server (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="3737c-119">[HTTP.sys](xref:fundamentals/servers/httpsys) server (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * <span data-ttu-id="3737c-120">[Kestrel](xref:fundamentals/servers/kestrel) serwera</span><span class="sxs-lookup"><span data-stu-id="3737c-120">[Kestrel](xref:fundamentals/servers/kestrel) server</span></span>

## <a name="response-compression"></a><span data-ttu-id="3737c-121">Kompresja odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3737c-121">Response compression</span></span>

<span data-ttu-id="3737c-122">Zazwyczaj nie natywnie skompresowane odpowiedzi mogą korzystać z kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="3737c-123">Odpowiedzi natywnie skompresowane zwykle obejmują: CSS, JavaScript, HTML, XML i JSON.</span><span class="sxs-lookup"><span data-stu-id="3737c-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="3737c-124">Nie należy kompresować natywnie skompresowany zasobów, takich jak pliki PNG.</span><span class="sxs-lookup"><span data-stu-id="3737c-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="3737c-125">Jeśli użytkownik podejmie próbę dalszego skompresować natywnie skompresowane odpowiedzi, to wszystkie małe dodatkowe skrócenie czasu rozmiar i przekazywania będzie prawdopodobnie overshadowed przez czas potrzebny do przetworzenia kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="3737c-126">Nie Kompresuj pliki mniejsze niż około 150 – 1000 bajtów (w zależności od zawartości pliku i wydajność kompresji).</span><span class="sxs-lookup"><span data-stu-id="3737c-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="3737c-127">Obciążenie kompresowanie małych plików może powodować większe niż nieskompresowanego pliku skompresowanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3737c-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="3737c-128">W przypadku klienta może przetwarzać skompresowanej treści, klient musi powiadomić serwera jego możliwości, wysyłając `Accept-Encoding` nagłówek z żądania.</span><span class="sxs-lookup"><span data-stu-id="3737c-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="3737c-129">Gdy serwer wysyła skompresowanej treści, musi on zawierać informacje zawarte w `Content-Encoding` nagłówek jak skompresowane odpowiedzi jest zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="3737c-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="3737c-130">Zawartość nazw kodowania, obsługiwane przez oprogramowanie pośredniczące są wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="3737c-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="3737c-131">`Accept-Encoding` wartości nagłówka</span><span class="sxs-lookup"><span data-stu-id="3737c-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="3737c-132">Obsługiwane oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="3737c-132">Middleware Supported</span></span> | <span data-ttu-id="3737c-133">Opis</span><span class="sxs-lookup"><span data-stu-id="3737c-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="3737c-134">Tak (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="3737c-134">Yes (default)</span></span>        | [<span data-ttu-id="3737c-135">Format skompresowanych danych Brotli</span><span class="sxs-lookup"><span data-stu-id="3737c-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="3737c-136">Nie</span><span class="sxs-lookup"><span data-stu-id="3737c-136">No</span></span>                   | [<span data-ttu-id="3737c-137">Format skompresowanych danych DEFLATE</span><span class="sxs-lookup"><span data-stu-id="3737c-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="3737c-138">Nie</span><span class="sxs-lookup"><span data-stu-id="3737c-138">No</span></span>                   | [<span data-ttu-id="3737c-139">Wymiana wydajne XML W3C</span><span class="sxs-lookup"><span data-stu-id="3737c-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="3737c-140">Tak</span><span class="sxs-lookup"><span data-stu-id="3737c-140">Yes</span></span>                  | [<span data-ttu-id="3737c-141">Format pliku gzip</span><span class="sxs-lookup"><span data-stu-id="3737c-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="3737c-142">Tak</span><span class="sxs-lookup"><span data-stu-id="3737c-142">Yes</span></span>                  | <span data-ttu-id="3737c-143">Identyfikator "Bez kodowania": nie musi być zakodowany odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="3737c-144">Nie</span><span class="sxs-lookup"><span data-stu-id="3737c-144">No</span></span>                   | [<span data-ttu-id="3737c-145">Format Transfer sieci archiwa Java</span><span class="sxs-lookup"><span data-stu-id="3737c-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="3737c-146">Tak</span><span class="sxs-lookup"><span data-stu-id="3737c-146">Yes</span></span>                  | <span data-ttu-id="3737c-147">Kodowanie nie jest jawnie żądanej zawartości dostępne</span><span class="sxs-lookup"><span data-stu-id="3737c-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="3737c-148">`Accept-Encoding` wartości nagłówka</span><span class="sxs-lookup"><span data-stu-id="3737c-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="3737c-149">Obsługiwane oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="3737c-149">Middleware Supported</span></span> | <span data-ttu-id="3737c-150">Opis</span><span class="sxs-lookup"><span data-stu-id="3737c-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="3737c-151">Nie</span><span class="sxs-lookup"><span data-stu-id="3737c-151">No</span></span>                   | [<span data-ttu-id="3737c-152">Format skompresowanych danych Brotli</span><span class="sxs-lookup"><span data-stu-id="3737c-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="3737c-153">Nie</span><span class="sxs-lookup"><span data-stu-id="3737c-153">No</span></span>                   | [<span data-ttu-id="3737c-154">Format skompresowanych danych DEFLATE</span><span class="sxs-lookup"><span data-stu-id="3737c-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="3737c-155">Nie</span><span class="sxs-lookup"><span data-stu-id="3737c-155">No</span></span>                   | [<span data-ttu-id="3737c-156">Wymiana wydajne XML W3C</span><span class="sxs-lookup"><span data-stu-id="3737c-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="3737c-157">Tak (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="3737c-157">Yes (default)</span></span>        | [<span data-ttu-id="3737c-158">Format pliku gzip</span><span class="sxs-lookup"><span data-stu-id="3737c-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="3737c-159">Tak</span><span class="sxs-lookup"><span data-stu-id="3737c-159">Yes</span></span>                  | <span data-ttu-id="3737c-160">Identyfikator "Bez kodowania": nie musi być zakodowany odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="3737c-161">Nie</span><span class="sxs-lookup"><span data-stu-id="3737c-161">No</span></span>                   | [<span data-ttu-id="3737c-162">Format Transfer sieci archiwa Java</span><span class="sxs-lookup"><span data-stu-id="3737c-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="3737c-163">Tak</span><span class="sxs-lookup"><span data-stu-id="3737c-163">Yes</span></span>                  | <span data-ttu-id="3737c-164">Kodowanie nie jest jawnie żądanej zawartości dostępne</span><span class="sxs-lookup"><span data-stu-id="3737c-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="3737c-165">Aby uzyskać więcej informacji, zobacz [IANA oficjalne kodowania listy zawartości](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="3737c-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="3737c-166">Oprogramowanie pośredniczące pozwala na dodawanie kompresji dodatkowych dostawców na potrzeby niestandardowych `Accept-Encoding` wartości nagłówka.</span><span class="sxs-lookup"><span data-stu-id="3737c-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="3737c-167">Aby uzyskać więcej informacji, zobacz [niestandardowi](#custom-providers) poniżej.</span><span class="sxs-lookup"><span data-stu-id="3737c-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="3737c-168">Oprogramowanie pośredniczące jest w stanie reagowanie na wartości jakości (qvalue, `q`) wagi, gdy wysłane przez klienta w celu określenia priorytetów schematów kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="3737c-169">Aby uzyskać więcej informacji, zobacz [RFC 7231: Zaakceptuj kodowania](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="3737c-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="3737c-170">Algorytmy kompresji podlegają zależność między szybkości kompresji i efektywność kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="3737c-171">*Skuteczność* w tym kontekście odnosi się do rozmiaru danych wyjściowych po kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="3737c-172">Najmniejszy rozmiar odbywa się przez większość *optymalne* kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="3737c-173">Zaangażowane w żądanie nagłówki wysyłanie, buforowanie i odbieranie skompresowanej treści są opisane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="3737c-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="3737c-174">nagłówek</span><span class="sxs-lookup"><span data-stu-id="3737c-174">Header</span></span>             | <span data-ttu-id="3737c-175">Rola</span><span class="sxs-lookup"><span data-stu-id="3737c-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="3737c-176">Wysłanych z klienta do serwera w celu wskazania kodowania schematy dopuszczalne klientowi zawartości.</span><span class="sxs-lookup"><span data-stu-id="3737c-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="3737c-177">Wysyłane z serwera do klienta w celu wskazania kodowania zawartości w ładunku.</span><span class="sxs-lookup"><span data-stu-id="3737c-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="3737c-178">W przypadku kompresji `Content-Length` nagłówka zostanie usunięty, ponieważ zmiany zawartości treści, gdy odpowiedź jest skompresowany.</span><span class="sxs-lookup"><span data-stu-id="3737c-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="3737c-179">W przypadku kompresji `Content-MD5` nagłówka zostanie usunięty, ponieważ zmieniono treść i skrót nie jest już prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="3737c-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="3737c-180">Określa typ MIME zawartości.</span><span class="sxs-lookup"><span data-stu-id="3737c-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="3737c-181">Należy określić w każdej odpowiedzi jego `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="3737c-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="3737c-182">Oprogramowanie pośredniczące sprawdza tę wartość, aby określić, jeśli odpowiedź powinien być skompresowany.</span><span class="sxs-lookup"><span data-stu-id="3737c-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="3737c-183">Oprogramowanie pośredniczące określa zestaw [domyślne typy MIME](#mime-types) można kodować, ale można zastąpić, lub dodać typy MIME.</span><span class="sxs-lookup"><span data-stu-id="3737c-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="3737c-184">Gdy wysyłane przez serwer o wartości `Accept-Encoding` do klientów i serwerów proxy, `Vary` nagłówek wskazuje do klienta lub serwera proxy, który powinien pamięci podręcznej (różne) odpowiedzi na podstawie wartości z `Accept-Encoding` nagłówku żądania.</span><span class="sxs-lookup"><span data-stu-id="3737c-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="3737c-185">Wynik zwracania zawartości przy użyciu `Vary: Accept-Encoding` nagłówek jest zarówno skompresowane i bez kompresji odpowiedzi są buforowane osobno.</span><span class="sxs-lookup"><span data-stu-id="3737c-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="3737c-186">Zapoznaj się z funkcjami oprogramowanie pośredniczące kompresji odpowiedzi z [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="3737c-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="3737c-187">Ilustruje przykład:</span><span class="sxs-lookup"><span data-stu-id="3737c-187">The sample illustrates:</span></span>

* <span data-ttu-id="3737c-188">Kompresja odpowiedzi aplikacji za pomocą Gzip i dostawców niestandardowych kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="3737c-189">Jak dodać typ MIME do domyślnej listy typów MIME dla kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="3737c-190">Package</span><span class="sxs-lookup"><span data-stu-id="3737c-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3737c-191">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), która obejmuje [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="3737c-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3737c-192">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage), która obejmuje [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="3737c-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3737c-193">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="3737c-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="3737c-194">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="3737c-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3737c-195">Poniższy kod przedstawia sposób włączania oprogramowanie pośredniczące kompresji odpowiedzi dla domyślnych typów MIME i dostawców kompresji ([Brotli](#brotli-compression-provider) i [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="3737c-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3737c-196">Poniższy kod przedstawia sposób włączania oprogramowanie pośredniczące kompresji odpowiedzi na domyślnej stronie typy MIME i [dostawcy kompresji Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="3737c-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

> [!NOTE]
> <span data-ttu-id="3737c-197">Użyj narzędzia, takiego jak [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), lub [Postman](https://www.getpostman.com/) można ustawić `Accept-Encoding` nagłówek żądania i badanie nagłówki odpowiedzi, rozmiar i treść.</span><span class="sxs-lookup"><span data-stu-id="3737c-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="3737c-198">Prześlij żądanie do przykładowej aplikacji bez `Accept-Encoding` nagłówka i sprawdź, czy odpowiedź jest bez kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="3737c-199">`Content-Encoding` i `Vary` nagłówki nie są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Wynik żądania bez nagłówka Accept-Encoding wyświetlana w oknie programu fiddler.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3737c-202">Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: br` nagłówka (Brotli kompresji) i sprawdź, czy odpowiedź jest skompresowany.</span><span class="sxs-lookup"><span data-stu-id="3737c-202">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="3737c-203">`Content-Encoding` i `Vary` nagłówki są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Wynik żądania przy użyciu nagłówka Accept-Encoding i wartość br okno programu fiddler.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3737c-207">Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: gzip` nagłówka i sprawdź, czy odpowiedź jest skompresowany.</span><span class="sxs-lookup"><span data-stu-id="3737c-207">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="3737c-208">`Content-Encoding` i `Vary` nagłówki są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-208">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Okno narzędzia fiddler wynik żądania przy użyciu nagłówka Accept-Encoding i wartość gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="3737c-212">dostawcy</span><span class="sxs-lookup"><span data-stu-id="3737c-212">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="3737c-213">Dostawca kompresji Brotli</span><span class="sxs-lookup"><span data-stu-id="3737c-213">Brotli Compression Provider</span></span>

<span data-ttu-id="3737c-214">Użyj `BrotliCompressionProvider` kompresji odpowiedzi z [format skompresowanych danych Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="3737c-214">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="3737c-215">Jeśli żaden dostawca kompresji są jawnie dodawane do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="3737c-215">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="3737c-216">Dostawca kompresji Brotli jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcy kompresji Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="3737c-216">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="3737c-217">Kompresja domyślnie kompresji Brotli Brotli format skompresowanych danych jest obsługiwany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3737c-217">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="3737c-218">Jeśli Brotli nie jest obsługiwane przez klienta, kompresji domyślnie Gzip, gdy klient obsługuje kompresję Gzip.</span><span class="sxs-lookup"><span data-stu-id="3737c-218">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="3737c-219">Dostawca kompresji Brotoli muszą być dodane po dodaniu jawnie żadnego dostawcy kompresji:</span><span class="sxs-lookup"><span data-stu-id="3737c-219">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

<span data-ttu-id="3737c-220">Ustaw poziom dzięki kompresji `BrotliCompressionProviderOptions`.</span><span class="sxs-lookup"><span data-stu-id="3737c-220">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="3737c-221">Dostawca kompresji Brotli wartość domyślna to najszybszy poziom kompresji ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), może nie dawać kompresji najbardziej efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="3737c-221">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="3737c-222">W razie potrzeby kompresji najbardziej efektywny sposób konfiguracji oprogramowania pośredniczącego optymalnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-222">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="3737c-223">Poziom kompresji</span><span class="sxs-lookup"><span data-stu-id="3737c-223">Compression Level</span></span> | <span data-ttu-id="3737c-224">Opis</span><span class="sxs-lookup"><span data-stu-id="3737c-224">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="3737c-225">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="3737c-225">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="3737c-226">Kompresja powinno zająć tak szybko, jak to możliwe, nawet jeśli dane wyjściowe nie są optymalnie skompresowane.</span><span class="sxs-lookup"><span data-stu-id="3737c-226">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="3737c-227">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="3737c-227">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="3737c-228">Kompresja nie powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="3737c-228">No compression should be performed.</span></span> |
| [<span data-ttu-id="3737c-229">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="3737c-229">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="3737c-230">Odpowiedzi powinny zostać skompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu na ukończenie.</span><span class="sxs-lookup"><span data-stu-id="3737c-230">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="3737c-231">Dostawca kompresji gzip</span><span class="sxs-lookup"><span data-stu-id="3737c-231">Gzip Compression Provider</span></span>

<span data-ttu-id="3737c-232">Użyj <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> kompresji odpowiedzi z [format pliku Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="3737c-232">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="3737c-233">Jeśli żaden dostawca kompresji są jawnie dodawane do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="3737c-233">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3737c-234">Dostawca kompresji Gzip jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcy kompresji Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="3737c-234">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="3737c-235">Kompresja domyślnie kompresji Brotli Brotli format skompresowanych danych jest obsługiwany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3737c-235">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="3737c-236">Jeśli Brotli nie jest obsługiwane przez klienta, kompresji domyślnie Gzip, gdy klient obsługuje kompresję Gzip.</span><span class="sxs-lookup"><span data-stu-id="3737c-236">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="3737c-237">Dostawca kompresji Gzip jest domyślnie dodawany do tablicy dostawców kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-237">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="3737c-238">Domyślnie kompresji Gzip, gdy klient obsługuje kompresję Gzip.</span><span class="sxs-lookup"><span data-stu-id="3737c-238">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="3737c-239">Dostawca kompresji Gzip muszą być dodane po dodaniu jawnie żadnego dostawcy kompresji:</span><span class="sxs-lookup"><span data-stu-id="3737c-239">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="3737c-240">Ustaw poziom dzięki kompresji <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="3737c-240">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="3737c-241">Dostawca kompresji Gzip, wartość domyślna to najszybszy poziom kompresji ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), może nie dawać kompresji najbardziej efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="3737c-241">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="3737c-242">W razie potrzeby kompresji najbardziej efektywny sposób konfiguracji oprogramowania pośredniczącego optymalnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-242">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="3737c-243">Poziom kompresji</span><span class="sxs-lookup"><span data-stu-id="3737c-243">Compression Level</span></span> | <span data-ttu-id="3737c-244">Opis</span><span class="sxs-lookup"><span data-stu-id="3737c-244">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="3737c-245">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="3737c-245">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="3737c-246">Kompresja powinno zająć tak szybko, jak to możliwe, nawet jeśli dane wyjściowe nie są optymalnie skompresowane.</span><span class="sxs-lookup"><span data-stu-id="3737c-246">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="3737c-247">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="3737c-247">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="3737c-248">Kompresja nie powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="3737c-248">No compression should be performed.</span></span> |
| [<span data-ttu-id="3737c-249">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="3737c-249">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="3737c-250">Odpowiedzi powinny zostać skompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu na ukończenie.</span><span class="sxs-lookup"><span data-stu-id="3737c-250">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="3737c-251">Dostawcy niestandardowi</span><span class="sxs-lookup"><span data-stu-id="3737c-251">Custom providers</span></span>

<span data-ttu-id="3737c-252">Tworzenie implementacji niestandardowych kompresji z <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="3737c-252">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="3737c-253"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Reprezentuje zawartość, kodowanie, że `ICompressionProvider` tworzy.</span><span class="sxs-lookup"><span data-stu-id="3737c-253">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="3737c-254">Oprogramowanie pośredniczące używa tych informacji do wybierz dostawcę oparte na listę określoną w `Accept-Encoding` nagłówku żądania.</span><span class="sxs-lookup"><span data-stu-id="3737c-254">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="3737c-255">Korzystając z przykładowej aplikacji, klient przesyła żądanie z `Accept-Encoding: mycustomcompression` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="3737c-255">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="3737c-256">Oprogramowanie pośredniczące używa implementacji niestandardowych kompresji i zwraca odpowiedź z `Content-Encoding: mycustomcompression` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="3737c-256">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="3737c-257">Klient musi mieć możliwość Dekompresuj niestandardowego kodowania w kolejności dla implementacji niestandardowych kompresji do pracy.</span><span class="sxs-lookup"><span data-stu-id="3737c-257">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="3737c-258">Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: mycustomcompression` nagłówka i sprawdź, czy nagłówki odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-258">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="3737c-259">`Vary` i `Content-Encoding` nagłówki są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-259">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="3737c-260">Treść odpowiedzi (niewyświetlany) nie jest skompresowany za próbki.</span><span class="sxs-lookup"><span data-stu-id="3737c-260">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="3737c-261">Nie ma implementacji kompresji `CustomCompressionProvider` klasy próbki.</span><span class="sxs-lookup"><span data-stu-id="3737c-261">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="3737c-262">Jednak przykład pokazuje, gdzie będzie implementować algorytm kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-262">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Wynik żądania przy użyciu nagłówka Accept-Encoding i wartość mycustomcompression okno programu fiddler.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="3737c-265">MIME, typy</span><span class="sxs-lookup"><span data-stu-id="3737c-265">MIME types</span></span>

<span data-ttu-id="3737c-266">Oprogramowanie pośredniczące Określa domyślny zestaw typów MIME dla kompresji:</span><span class="sxs-lookup"><span data-stu-id="3737c-266">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="3737c-267">Zastąp lub Dołącz typy MIME przy użyciu opcji oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-267">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="3737c-268">Należy pamiętać, że symbol wieloznaczny MIME typów, takich jak `text/*` nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="3737c-268">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="3737c-269">Przykładowa aplikacja dodaje typ MIME dla `image/svg+xml` kompresuje i służy transparent obrazu platformy ASP.NET Core (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="3737c-269">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="3737c-270">Kompresja z bezpiecznego protokołu</span><span class="sxs-lookup"><span data-stu-id="3737c-270">Compression with secure protocol</span></span>

<span data-ttu-id="3737c-271">Skompresowane odpowiedzi za pośrednictwem bezpiecznego połączenia, mogą być kontrolowane za pomocą `EnableForHttps` opcja, która jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="3737c-271">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="3737c-272">Przy użyciu kompresji, za pomocą stron dynamicznie generowanym może prowadzić do problemów zabezpieczeń takich jak [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków.</span><span class="sxs-lookup"><span data-stu-id="3737c-272">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="3737c-273">Dodawanie nagłówka zależne</span><span class="sxs-lookup"><span data-stu-id="3737c-273">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3737c-274">Podczas kompresowania odpowiedzi na podstawie `Accept-Encoding` nagłówka, występują potencjalnie wiele wersji skompresowane odpowiedzi i wersji bez kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-274">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="3737c-275">Aby można było wydać polecenie pamięci podręcznej klienta i serwera proxy, że wiele wersji istnieją i powinny być przechowywane, `Vary` nagłówek jest dodawana z `Accept-Encoding` wartości.</span><span class="sxs-lookup"><span data-stu-id="3737c-275">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="3737c-276">W programie ASP.NET Core 2.0 lub nowszej, dodaje oprogramowanie pośredniczące `Vary` nagłówka automatycznie, gdy odpowiedź jest skompresowany.</span><span class="sxs-lookup"><span data-stu-id="3737c-276">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3737c-277">Podczas kompresowania odpowiedzi na podstawie `Accept-Encoding` nagłówka, występują potencjalnie wiele wersji skompresowane odpowiedzi i wersji bez kompresji.</span><span class="sxs-lookup"><span data-stu-id="3737c-277">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="3737c-278">Aby można było wydać polecenie pamięci podręcznej klienta i serwera proxy, że wiele wersji istnieją i powinny być przechowywane, `Vary` nagłówek jest dodawana z `Accept-Encoding` wartości.</span><span class="sxs-lookup"><span data-stu-id="3737c-278">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="3737c-279">W programie ASP.NET Core 1.x, dodając `Vary` nagłówek odpowiedzi odbywa się ręcznie:</span><span class="sxs-lookup"><span data-stu-id="3737c-279">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="3737c-280">Oprogramowanie pośredniczące problem za zaporą zwrotny serwer proxy Nginx</span><span class="sxs-lookup"><span data-stu-id="3737c-280">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="3737c-281">Gdy żądanie jest przekazywane przez serwer Nginx, `Accept-Encoding` nagłówka zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="3737c-281">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="3737c-282">Usuwanie `Accept-Encoding` nagłówka zapobiega oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-282">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="3737c-283">Aby uzyskać więcej informacji, zobacz [NGINX: kompresja i Dekompresja](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="3737c-283">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="3737c-284">Ten problem jest śledzona przez [ustalić przekazywanego kompresji dla kontenera Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="3737c-284">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="3737c-285">Praca z kompresji dynamicznej usług IIS</span><span class="sxs-lookup"><span data-stu-id="3737c-285">Working with IIS dynamic compression</span></span>

<span data-ttu-id="3737c-286">Jeśli masz aktywne dynamicznej kompresji Moduł IIS konfigurowane na poziomie serwera, który chcesz wyłączyć dla aplikacji, Wyłącz moduł za pomocą dodatku *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="3737c-286">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="3737c-287">Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="3737c-287">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3737c-288">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="3737c-288">Troubleshooting</span></span>

<span data-ttu-id="3737c-289">Użyj narzędzia, takiego jak [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), lub [Postman](https://www.getpostman.com/), umożliwiają ustawianie `Accept-Encoding` nagłówek żądania i badanie nagłówki odpowiedzi, rozmiar i treść.</span><span class="sxs-lookup"><span data-stu-id="3737c-289">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="3737c-290">Domyślnie oprogramowanie pośredniczące kompresji odpowiedzi kompresuje odpowiedzi, które spełniają poniższe warunki:</span><span class="sxs-lookup"><span data-stu-id="3737c-290">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3737c-291">`Accept-Encoding` Występuje nagłówek, wartością `br`, `gzip`, `*`, lub niestandardowego kodowania, które odpowiada dostawcy niestandardowego kompresji, który został określony.</span><span class="sxs-lookup"><span data-stu-id="3737c-291">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="3737c-292">Wartość nie może być `identity` ani mieć wartości jakości (qvalue, `q`) ustawienie 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="3737c-292">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="3737c-293">Typ MIME (`Content-Type`) musi być ustawiona, a musi być zgodny typ MIME skonfigurowane na <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="3737c-293">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="3737c-294">Żądanie nie może zawierać `Content-Range` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="3737c-294">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="3737c-295">Żądanie musi używać protokołu niezabezpieczonego (http), chyba, że bezpiecznego protokołu (https) jest skonfigurowana w opcjach oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-295">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="3737c-296">*Należy pamiętać, niebezpieczeństwo [opisanych powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznego kompresję zawartości.*</span><span class="sxs-lookup"><span data-stu-id="3737c-296">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="3737c-297">`Accept-Encoding` Występuje nagłówek, wartością `gzip`, `*`, lub niestandardowego kodowania, które odpowiada dostawcy niestandardowego kompresji, który został określony.</span><span class="sxs-lookup"><span data-stu-id="3737c-297">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="3737c-298">Wartość nie może być `identity` ani mieć wartości jakości (qvalue, `q`) ustawienie 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="3737c-298">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="3737c-299">Typ MIME (`Content-Type`) musi być ustawiona, a musi być zgodny typ MIME skonfigurowane na <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="3737c-299">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="3737c-300">Żądanie nie może zawierać `Content-Range` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="3737c-300">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="3737c-301">Żądanie musi używać protokołu niezabezpieczonego (http), chyba, że bezpiecznego protokołu (https) jest skonfigurowana w opcjach oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3737c-301">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="3737c-302">*Należy pamiętać, niebezpieczeństwo [opisanych powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznego kompresję zawartości.*</span><span class="sxs-lookup"><span data-stu-id="3737c-302">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3737c-303">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3737c-303">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="3737c-304">Mozilla Developer Network: Zaakceptuj kodowania</span><span class="sxs-lookup"><span data-stu-id="3737c-304">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="3737c-305">Sekcji 7231 RFC 3.1.2.1: Zawierać zawartości</span><span class="sxs-lookup"><span data-stu-id="3737c-305">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="3737c-306">RFC 7230 sekcja 4.2.3: Gzip kodowania</span><span class="sxs-lookup"><span data-stu-id="3737c-306">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="3737c-307">Wersja specyfikacji formatu pliku GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="3737c-307">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
