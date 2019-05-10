---
title: Kompresja odpowiedzi w programie ASP.NET Core
author: guardrex
description: Informacje o kompresji odpowiedzi i sposobie używania oprogramowanie pośredniczące kompresji odpowiedzi w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: performance/response-compression
ms.openlocfilehash: e312d43fb62106f6ecb98367c29daa377bb227c9
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899638"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="4642c-103">Kompresja odpowiedzi w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4642c-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="4642c-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4642c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4642c-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4642c-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4642c-106">Przepustowość sieci jest ograniczona zasobów.</span><span class="sxs-lookup"><span data-stu-id="4642c-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="4642c-107">Zazwyczaj zmniejszenie rozmiaru odpowiedzi często znacznie zwiększa szybkość reakcji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4642c-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="4642c-108">Jednym ze sposobów, aby zmniejszyć rozmiar ładunku jest kompresji odpowiedzi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4642c-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="4642c-109">Kiedy należy używać oprogramowanie pośredniczące kompresji odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="4642c-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="4642c-110">Użyj technologii kompresji odpowiedzi na serwerze usług IIS, Apache i Nginx.</span><span class="sxs-lookup"><span data-stu-id="4642c-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="4642c-111">Wydajność oprogramowania pośredniczącego prawdopodobnie nie będzie zgodne z modułów serwera.</span><span class="sxs-lookup"><span data-stu-id="4642c-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="4642c-112">[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) serwera i [Kestrel](xref:fundamentals/servers/kestrel) server obecnie nie oferują obsługę wbudowanych kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="4642c-113">Oprogramowanie pośredniczące kompresji odpowiedzi należy użyć, jeśli:</span><span class="sxs-lookup"><span data-stu-id="4642c-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="4642c-114">Nie można użyć następujących technologii serwerowych kompresji:</span><span class="sxs-lookup"><span data-stu-id="4642c-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="4642c-115">Moduł dynamicznej kompresji usług IIS</span><span class="sxs-lookup"><span data-stu-id="4642c-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="4642c-116">Moduł mod_deflate Apache</span><span class="sxs-lookup"><span data-stu-id="4642c-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="4642c-117">Serwer Nginx kompresja i Dekompresja</span><span class="sxs-lookup"><span data-stu-id="4642c-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="4642c-118">Hosting bezpośrednio na:</span><span class="sxs-lookup"><span data-stu-id="4642c-118">Hosting directly on:</span></span>
  * <span data-ttu-id="4642c-119">[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) (dawniej nazywanych WebListener)</span><span class="sxs-lookup"><span data-stu-id="4642c-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="4642c-120">Kestrel serwera</span><span class="sxs-lookup"><span data-stu-id="4642c-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="4642c-121">Kompresja odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="4642c-121">Response compression</span></span>

<span data-ttu-id="4642c-122">Zazwyczaj nie natywnie skompresowane odpowiedzi mogą korzystać z kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="4642c-123">Odpowiedzi natywnie skompresowane zwykle obejmują: CSS, JavaScript, HTML, XML i JSON.</span><span class="sxs-lookup"><span data-stu-id="4642c-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="4642c-124">Nie należy kompresować natywnie skompresowany zasobów, takich jak pliki PNG.</span><span class="sxs-lookup"><span data-stu-id="4642c-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="4642c-125">Jeśli użytkownik podejmie próbę dalszego skompresować natywnie skompresowane odpowiedzi, to wszystkie małe dodatkowe skrócenie czasu rozmiar i przekazywania będzie prawdopodobnie overshadowed przez czas potrzebny do przetworzenia kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="4642c-126">Nie Kompresuj pliki mniejsze niż około 150 – 1000 bajtów (w zależności od zawartości pliku i wydajność kompresji).</span><span class="sxs-lookup"><span data-stu-id="4642c-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="4642c-127">Obciążenie kompresowanie małych plików może powodować większe niż nieskompresowanego pliku skompresowanego pliku.</span><span class="sxs-lookup"><span data-stu-id="4642c-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="4642c-128">W przypadku klienta może przetwarzać skompresowanej treści, klient musi powiadomić serwera jego możliwości, wysyłając `Accept-Encoding` nagłówek z żądania.</span><span class="sxs-lookup"><span data-stu-id="4642c-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="4642c-129">Gdy serwer wysyła skompresowanej treści, musi on zawierać informacje zawarte w `Content-Encoding` nagłówek jak skompresowane odpowiedzi jest zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="4642c-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="4642c-130">Zawartość nazw kodowania, obsługiwane przez oprogramowanie pośredniczące są wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="4642c-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="4642c-131">`Accept-Encoding` wartości nagłówka</span><span class="sxs-lookup"><span data-stu-id="4642c-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="4642c-132">Obsługiwane oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="4642c-132">Middleware Supported</span></span> | <span data-ttu-id="4642c-133">Opis</span><span class="sxs-lookup"><span data-stu-id="4642c-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="4642c-134">Tak (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="4642c-134">Yes (default)</span></span>        | [<span data-ttu-id="4642c-135">Format skompresowanych danych Brotli</span><span class="sxs-lookup"><span data-stu-id="4642c-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="4642c-136">Nie</span><span class="sxs-lookup"><span data-stu-id="4642c-136">No</span></span>                   | [<span data-ttu-id="4642c-137">Format skompresowanych danych DEFLATE</span><span class="sxs-lookup"><span data-stu-id="4642c-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="4642c-138">Nie</span><span class="sxs-lookup"><span data-stu-id="4642c-138">No</span></span>                   | [<span data-ttu-id="4642c-139">Wymiana wydajne XML W3C</span><span class="sxs-lookup"><span data-stu-id="4642c-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="4642c-140">Yes</span><span class="sxs-lookup"><span data-stu-id="4642c-140">Yes</span></span>                  | [<span data-ttu-id="4642c-141">Format pliku gzip</span><span class="sxs-lookup"><span data-stu-id="4642c-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="4642c-142">Yes</span><span class="sxs-lookup"><span data-stu-id="4642c-142">Yes</span></span>                  | <span data-ttu-id="4642c-143">Identyfikator "Bez kodowania": Odpowiedź nie musi być zakodowany.</span><span class="sxs-lookup"><span data-stu-id="4642c-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="4642c-144">Nie</span><span class="sxs-lookup"><span data-stu-id="4642c-144">No</span></span>                   | [<span data-ttu-id="4642c-145">Format Transfer sieci archiwa Java</span><span class="sxs-lookup"><span data-stu-id="4642c-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="4642c-146">Tak</span><span class="sxs-lookup"><span data-stu-id="4642c-146">Yes</span></span>                  | <span data-ttu-id="4642c-147">Kodowanie nie jest jawnie żądanej zawartości dostępne</span><span class="sxs-lookup"><span data-stu-id="4642c-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="4642c-148">`Accept-Encoding` wartości nagłówka</span><span class="sxs-lookup"><span data-stu-id="4642c-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="4642c-149">Obsługiwane oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="4642c-149">Middleware Supported</span></span> | <span data-ttu-id="4642c-150">Opis</span><span class="sxs-lookup"><span data-stu-id="4642c-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="4642c-151">Nie</span><span class="sxs-lookup"><span data-stu-id="4642c-151">No</span></span>                   | [<span data-ttu-id="4642c-152">Format skompresowanych danych Brotli</span><span class="sxs-lookup"><span data-stu-id="4642c-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="4642c-153">Nie</span><span class="sxs-lookup"><span data-stu-id="4642c-153">No</span></span>                   | [<span data-ttu-id="4642c-154">Format skompresowanych danych DEFLATE</span><span class="sxs-lookup"><span data-stu-id="4642c-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="4642c-155">Nie</span><span class="sxs-lookup"><span data-stu-id="4642c-155">No</span></span>                   | [<span data-ttu-id="4642c-156">Wymiana wydajne XML W3C</span><span class="sxs-lookup"><span data-stu-id="4642c-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="4642c-157">Tak (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="4642c-157">Yes (default)</span></span>        | [<span data-ttu-id="4642c-158">Format pliku gzip</span><span class="sxs-lookup"><span data-stu-id="4642c-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="4642c-159">Tak</span><span class="sxs-lookup"><span data-stu-id="4642c-159">Yes</span></span>                  | <span data-ttu-id="4642c-160">Identyfikator "Bez kodowania": Odpowiedź nie musi być zakodowany.</span><span class="sxs-lookup"><span data-stu-id="4642c-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="4642c-161">Nie</span><span class="sxs-lookup"><span data-stu-id="4642c-161">No</span></span>                   | [<span data-ttu-id="4642c-162">Format Transfer sieci archiwa Java</span><span class="sxs-lookup"><span data-stu-id="4642c-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="4642c-163">Tak</span><span class="sxs-lookup"><span data-stu-id="4642c-163">Yes</span></span>                  | <span data-ttu-id="4642c-164">Kodowanie nie jest jawnie żądanej zawartości dostępne</span><span class="sxs-lookup"><span data-stu-id="4642c-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="4642c-165">Aby uzyskać więcej informacji, zobacz [IANA oficjalne kodowania listy zawartości](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="4642c-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="4642c-166">Oprogramowanie pośredniczące pozwala na dodawanie kompresji dodatkowych dostawców na potrzeby niestandardowych `Accept-Encoding` wartości nagłówka.</span><span class="sxs-lookup"><span data-stu-id="4642c-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="4642c-167">Aby uzyskać więcej informacji, zobacz [niestandardowi](#custom-providers) poniżej.</span><span class="sxs-lookup"><span data-stu-id="4642c-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="4642c-168">Oprogramowanie pośredniczące jest w stanie reagowanie na wartości jakości (qvalue, `q`) wagi, gdy wysłane przez klienta w celu określenia priorytetów schematów kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="4642c-169">Aby uzyskać więcej informacji, zobacz [RFC 7231: Zaakceptuj kodowanie](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="4642c-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="4642c-170">Algorytmy kompresji podlegają zależność między szybkości kompresji i efektywność kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="4642c-171">*Skuteczność* w tym kontekście odnosi się do rozmiaru danych wyjściowych po kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="4642c-172">Najmniejszy rozmiar odbywa się przez większość *optymalne* kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="4642c-173">Zaangażowane w żądanie nagłówki wysyłanie, buforowanie i odbieranie skompresowanej treści są opisane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="4642c-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="4642c-174">nagłówek</span><span class="sxs-lookup"><span data-stu-id="4642c-174">Header</span></span>             | <span data-ttu-id="4642c-175">Rola</span><span class="sxs-lookup"><span data-stu-id="4642c-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="4642c-176">Wysłanych z klienta do serwera w celu wskazania kodowania schematy dopuszczalne klientowi zawartości.</span><span class="sxs-lookup"><span data-stu-id="4642c-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="4642c-177">Wysyłane z serwera do klienta w celu wskazania kodowania zawartości w ładunku.</span><span class="sxs-lookup"><span data-stu-id="4642c-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="4642c-178">W przypadku kompresji `Content-Length` nagłówka zostanie usunięty, ponieważ zmiany zawartości treści, gdy odpowiedź jest skompresowany.</span><span class="sxs-lookup"><span data-stu-id="4642c-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="4642c-179">W przypadku kompresji `Content-MD5` nagłówka zostanie usunięty, ponieważ zmieniono treść i skrót nie jest już prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="4642c-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="4642c-180">Określa typ MIME zawartości.</span><span class="sxs-lookup"><span data-stu-id="4642c-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="4642c-181">Należy określić w każdej odpowiedzi jego `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="4642c-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="4642c-182">Oprogramowanie pośredniczące sprawdza tę wartość, aby określić, jeśli odpowiedź powinien być skompresowany.</span><span class="sxs-lookup"><span data-stu-id="4642c-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="4642c-183">Oprogramowanie pośredniczące określa zestaw [domyślne typy MIME](#mime-types) można kodować, ale można zastąpić, lub dodać typy MIME.</span><span class="sxs-lookup"><span data-stu-id="4642c-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="4642c-184">Gdy wysyłane przez serwer o wartości `Accept-Encoding` do klientów i serwerów proxy, `Vary` nagłówek wskazuje do klienta lub serwera proxy, który powinien pamięci podręcznej (różne) odpowiedzi na podstawie wartości z `Accept-Encoding` nagłówku żądania.</span><span class="sxs-lookup"><span data-stu-id="4642c-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="4642c-185">Wynik zwracania zawartości przy użyciu `Vary: Accept-Encoding` nagłówek jest zarówno skompresowane i bez kompresji odpowiedzi są buforowane osobno.</span><span class="sxs-lookup"><span data-stu-id="4642c-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="4642c-186">Zapoznaj się z funkcjami oprogramowanie pośredniczące kompresji odpowiedzi z [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="4642c-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="4642c-187">Ilustruje przykład:</span><span class="sxs-lookup"><span data-stu-id="4642c-187">The sample illustrates:</span></span>

* <span data-ttu-id="4642c-188">Kompresja odpowiedzi aplikacji za pomocą Gzip i dostawców niestandardowych kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="4642c-189">Jak dodać typ MIME do domyślnej listy typów MIME dla kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="4642c-190">Package</span><span class="sxs-lookup"><span data-stu-id="4642c-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4642c-191">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), która obejmuje [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="4642c-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4642c-192">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage), która obejmuje [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="4642c-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4642c-193">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="4642c-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="4642c-194">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="4642c-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4642c-195">Poniższy kod przedstawia sposób włączania oprogramowanie pośredniczące kompresji odpowiedzi dla domyślnych typów MIME i dostawców kompresji ([Brotli](#brotli-compression-provider) i [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="4642c-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4642c-196">Poniższy kod przedstawia sposób włączania oprogramowanie pośredniczące kompresji odpowiedzi na domyślnej stronie typy MIME i [dostawcy kompresji Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="4642c-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="4642c-197">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="4642c-197">Notes:</span></span>

* <span data-ttu-id="4642c-198">`app.UseResponseCompression` musi zostać wywołana przed `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="4642c-198">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="4642c-199">Użyj narzędzia takiego jak [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), lub [Postman](https://www.getpostman.com/) można ustawić `Accept-Encoding` nagłówek żądania i badanie nagłówki odpowiedzi, rozmiar i treść.</span><span class="sxs-lookup"><span data-stu-id="4642c-199">Use a tool such as [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="4642c-200">Prześlij żądanie do przykładowej aplikacji bez `Accept-Encoding` nagłówka i sprawdź, czy odpowiedź jest bez kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="4642c-201">`Content-Encoding` i `Vary` nagłówki nie są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Wynik żądania bez nagłówka Accept-Encoding wyświetlana w oknie programu fiddler.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4642c-204">Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: br` nagłówka (Brotli kompresji) i sprawdź, czy odpowiedź jest skompresowany.</span><span class="sxs-lookup"><span data-stu-id="4642c-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="4642c-205">`Content-Encoding` i `Vary` nagłówki są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Wynik żądania przy użyciu nagłówka Accept-Encoding i wartość br okno programu fiddler.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4642c-209">Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: gzip` nagłówka i sprawdź, czy odpowiedź jest skompresowany.</span><span class="sxs-lookup"><span data-stu-id="4642c-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="4642c-210">`Content-Encoding` i `Vary` nagłówki są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Okno narzędzia fiddler wynik żądania przy użyciu nagłówka Accept-Encoding i wartość gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="4642c-214">dostawcy</span><span class="sxs-lookup"><span data-stu-id="4642c-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="4642c-215">Dostawca kompresji Brotli</span><span class="sxs-lookup"><span data-stu-id="4642c-215">Brotli Compression Provider</span></span>

<span data-ttu-id="4642c-216">Użyj <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> kompresji odpowiedzi z [format skompresowanych danych Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="4642c-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="4642c-217">Jeśli żaden dostawca kompresji są jawnie dodawane do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="4642c-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="4642c-218">Dostawca kompresji Brotli jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcy kompresji Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="4642c-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="4642c-219">Kompresja domyślnie kompresji Brotli Brotli format skompresowanych danych jest obsługiwany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="4642c-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="4642c-220">Jeśli Brotli nie jest obsługiwane przez klienta, kompresji domyślnie Gzip, gdy klient obsługuje kompresję Gzip.</span><span class="sxs-lookup"><span data-stu-id="4642c-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="4642c-221">Dostawca kompresji Brotoli muszą być dodane po dodaniu jawnie żadnego dostawcy kompresji:</span><span class="sxs-lookup"><span data-stu-id="4642c-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="4642c-222">Ustaw poziom dzięki kompresji <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="4642c-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="4642c-223">Dostawca kompresji Brotli wartość domyślna to najszybszy poziom kompresji ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), może nie dawać kompresji najbardziej efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="4642c-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="4642c-224">W razie potrzeby kompresji najbardziej efektywny sposób konfiguracji oprogramowania pośredniczącego optymalnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="4642c-225">Poziom kompresji</span><span class="sxs-lookup"><span data-stu-id="4642c-225">Compression Level</span></span> | <span data-ttu-id="4642c-226">Opis</span><span class="sxs-lookup"><span data-stu-id="4642c-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="4642c-227">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="4642c-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="4642c-228">Kompresja powinno zająć tak szybko, jak to możliwe, nawet jeśli dane wyjściowe nie są optymalnie skompresowane.</span><span class="sxs-lookup"><span data-stu-id="4642c-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="4642c-229">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="4642c-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="4642c-230">Kompresja nie powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="4642c-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="4642c-231">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="4642c-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="4642c-232">Odpowiedzi powinny zostać skompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu na ukończenie.</span><span class="sxs-lookup"><span data-stu-id="4642c-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="4642c-233">Dostawca kompresji gzip</span><span class="sxs-lookup"><span data-stu-id="4642c-233">Gzip Compression Provider</span></span>

<span data-ttu-id="4642c-234">Użyj <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> kompresji odpowiedzi z [format pliku Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="4642c-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="4642c-235">Jeśli żaden dostawca kompresji są jawnie dodawane do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="4642c-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="4642c-236">Dostawca kompresji Gzip jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcy kompresji Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="4642c-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="4642c-237">Kompresja domyślnie kompresji Brotli Brotli format skompresowanych danych jest obsługiwany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="4642c-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="4642c-238">Jeśli Brotli nie jest obsługiwane przez klienta, kompresji domyślnie Gzip, gdy klient obsługuje kompresję Gzip.</span><span class="sxs-lookup"><span data-stu-id="4642c-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="4642c-239">Dostawca kompresji Gzip jest domyślnie dodawany do tablicy dostawców kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="4642c-240">Domyślnie kompresji Gzip, gdy klient obsługuje kompresję Gzip.</span><span class="sxs-lookup"><span data-stu-id="4642c-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="4642c-241">Dostawca kompresji Gzip muszą być dodane po dodaniu jawnie żadnego dostawcy kompresji:</span><span class="sxs-lookup"><span data-stu-id="4642c-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="4642c-242">Ustaw poziom dzięki kompresji <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="4642c-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="4642c-243">Dostawca kompresji Gzip, wartość domyślna to najszybszy poziom kompresji ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), może nie dawać kompresji najbardziej efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="4642c-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="4642c-244">W razie potrzeby kompresji najbardziej efektywny sposób konfiguracji oprogramowania pośredniczącego optymalnej kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="4642c-245">Poziom kompresji</span><span class="sxs-lookup"><span data-stu-id="4642c-245">Compression Level</span></span> | <span data-ttu-id="4642c-246">Opis</span><span class="sxs-lookup"><span data-stu-id="4642c-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="4642c-247">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="4642c-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="4642c-248">Kompresja powinno zająć tak szybko, jak to możliwe, nawet jeśli dane wyjściowe nie są optymalnie skompresowane.</span><span class="sxs-lookup"><span data-stu-id="4642c-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="4642c-249">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="4642c-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="4642c-250">Kompresja nie powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="4642c-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="4642c-251">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="4642c-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="4642c-252">Odpowiedzi powinny zostać skompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu na ukończenie.</span><span class="sxs-lookup"><span data-stu-id="4642c-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="4642c-253">Dostawcy niestandardowi</span><span class="sxs-lookup"><span data-stu-id="4642c-253">Custom providers</span></span>

<span data-ttu-id="4642c-254">Tworzenie implementacji niestandardowych kompresji z <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="4642c-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="4642c-255"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Reprezentuje zawartość, kodowanie, że `ICompressionProvider` tworzy.</span><span class="sxs-lookup"><span data-stu-id="4642c-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="4642c-256">Oprogramowanie pośredniczące używa tych informacji do wybierz dostawcę oparte na listę określoną w `Accept-Encoding` nagłówku żądania.</span><span class="sxs-lookup"><span data-stu-id="4642c-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="4642c-257">Korzystając z przykładowej aplikacji, klient przesyła żądanie z `Accept-Encoding: mycustomcompression` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="4642c-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="4642c-258">Oprogramowanie pośredniczące używa implementacji niestandardowych kompresji i zwraca odpowiedź z `Content-Encoding: mycustomcompression` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="4642c-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="4642c-259">Klient musi mieć możliwość Dekompresuj niestandardowego kodowania w kolejności dla implementacji niestandardowych kompresji do pracy.</span><span class="sxs-lookup"><span data-stu-id="4642c-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4642c-260">Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: mycustomcompression` nagłówka i sprawdź, czy nagłówki odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="4642c-261">`Vary` i `Content-Encoding` nagłówki są obecne w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="4642c-262">Treść odpowiedzi (niewyświetlany) nie jest skompresowany za próbki.</span><span class="sxs-lookup"><span data-stu-id="4642c-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="4642c-263">Nie ma implementacji kompresji `CustomCompressionProvider` klasy próbki.</span><span class="sxs-lookup"><span data-stu-id="4642c-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="4642c-264">Jednak przykład pokazuje, gdzie będzie implementować algorytm kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Wynik żądania przy użyciu nagłówka Accept-Encoding i wartość mycustomcompression okno programu fiddler.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="4642c-267">MIME, typy</span><span class="sxs-lookup"><span data-stu-id="4642c-267">MIME types</span></span>

<span data-ttu-id="4642c-268">Oprogramowanie pośredniczące Określa domyślny zestaw typów MIME dla kompresji:</span><span class="sxs-lookup"><span data-stu-id="4642c-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="4642c-269">Zastąp lub Dołącz typy MIME przy użyciu opcji oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="4642c-270">Należy pamiętać, że symbol wieloznaczny MIME typów, takich jak `text/*` nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4642c-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="4642c-271">Przykładowa aplikacja dodaje typ MIME dla `image/svg+xml` kompresuje i służy transparent obrazu platformy ASP.NET Core (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="4642c-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="4642c-272">Kompresja z bezpiecznego protokołu</span><span class="sxs-lookup"><span data-stu-id="4642c-272">Compression with secure protocol</span></span>

<span data-ttu-id="4642c-273">Skompresowane odpowiedzi za pośrednictwem bezpiecznego połączenia, mogą być kontrolowane za pomocą `EnableForHttps` opcja, która jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="4642c-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="4642c-274">Przy użyciu kompresji, za pomocą stron dynamicznie generowanym może prowadzić do problemów zabezpieczeń takich jak [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków.</span><span class="sxs-lookup"><span data-stu-id="4642c-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="4642c-275">Dodawanie nagłówka zależne</span><span class="sxs-lookup"><span data-stu-id="4642c-275">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4642c-276">Podczas kompresowania odpowiedzi na podstawie `Accept-Encoding` nagłówka, występują potencjalnie wiele wersji skompresowane odpowiedzi i wersji bez kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="4642c-277">Aby można było wydać polecenie pamięci podręcznej klienta i serwera proxy, że wiele wersji istnieją i powinny być przechowywane, `Vary` nagłówek jest dodawana z `Accept-Encoding` wartości.</span><span class="sxs-lookup"><span data-stu-id="4642c-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="4642c-278">W programie ASP.NET Core 2.0 lub nowszej, dodaje oprogramowanie pośredniczące `Vary` nagłówka automatycznie, gdy odpowiedź jest skompresowany.</span><span class="sxs-lookup"><span data-stu-id="4642c-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4642c-279">Podczas kompresowania odpowiedzi na podstawie `Accept-Encoding` nagłówka, występują potencjalnie wiele wersji skompresowane odpowiedzi i wersji bez kompresji.</span><span class="sxs-lookup"><span data-stu-id="4642c-279">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="4642c-280">Aby można było wydać polecenie pamięci podręcznej klienta i serwera proxy, że wiele wersji istnieją i powinny być przechowywane, `Vary` nagłówek jest dodawana z `Accept-Encoding` wartości.</span><span class="sxs-lookup"><span data-stu-id="4642c-280">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="4642c-281">W programie ASP.NET Core 1.x, dodając `Vary` nagłówek odpowiedzi odbywa się ręcznie:</span><span class="sxs-lookup"><span data-stu-id="4642c-281">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="4642c-282">Oprogramowanie pośredniczące problem za zaporą zwrotny serwer proxy Nginx</span><span class="sxs-lookup"><span data-stu-id="4642c-282">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="4642c-283">Gdy żądanie jest przekazywane przez serwer Nginx, `Accept-Encoding` nagłówka zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="4642c-283">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="4642c-284">Usuwanie `Accept-Encoding` nagłówka zapobiega oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-284">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="4642c-285">Aby uzyskać więcej informacji, zobacz [NGINX: Kompresja i Dekompresja](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="4642c-285">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="4642c-286">Ten problem jest śledzona przez [ustalić przekazywanego kompresji dla kontenera Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="4642c-286">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="4642c-287">Praca z kompresji dynamicznej usług IIS</span><span class="sxs-lookup"><span data-stu-id="4642c-287">Working with IIS dynamic compression</span></span>

<span data-ttu-id="4642c-288">Jeśli masz aktywne dynamicznej kompresji Moduł IIS konfigurowane na poziomie serwera, który chcesz wyłączyć dla aplikacji, Wyłącz moduł za pomocą dodatku *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="4642c-288">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="4642c-289">Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="4642c-289">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4642c-290">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="4642c-290">Troubleshooting</span></span>

<span data-ttu-id="4642c-291">Użyj narzędzia, takiego jak [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), lub [Postman](https://www.getpostman.com/), umożliwiają ustawianie `Accept-Encoding` nagłówek żądania i badanie nagłówki odpowiedzi, rozmiar i treść.</span><span class="sxs-lookup"><span data-stu-id="4642c-291">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="4642c-292">Domyślnie oprogramowanie pośredniczące kompresji odpowiedzi kompresuje odpowiedzi, które spełniają poniższe warunki:</span><span class="sxs-lookup"><span data-stu-id="4642c-292">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="4642c-293">`Accept-Encoding` Występuje nagłówek, wartością `br`, `gzip`, `*`, lub niestandardowego kodowania, które odpowiada dostawcy niestandardowego kompresji, który został określony.</span><span class="sxs-lookup"><span data-stu-id="4642c-293">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="4642c-294">Wartość nie może być `identity` ani mieć wartości jakości (qvalue, `q`) ustawienie 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="4642c-294">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="4642c-295">Typ MIME (`Content-Type`) musi być ustawiona, a musi być zgodny typ MIME skonfigurowane na <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="4642c-295">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="4642c-296">Żądanie nie może zawierać `Content-Range` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="4642c-296">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="4642c-297">Żądanie musi używać protokołu niezabezpieczonego (http), chyba, że bezpiecznego protokołu (https) jest skonfigurowana w opcjach oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-297">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="4642c-298">*Należy pamiętać, niebezpieczeństwo [opisanych powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznego kompresję zawartości.*</span><span class="sxs-lookup"><span data-stu-id="4642c-298">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="4642c-299">`Accept-Encoding` Występuje nagłówek, wartością `gzip`, `*`, lub niestandardowego kodowania, które odpowiada dostawcy niestandardowego kompresji, który został określony.</span><span class="sxs-lookup"><span data-stu-id="4642c-299">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="4642c-300">Wartość nie może być `identity` ani mieć wartości jakości (qvalue, `q`) ustawienie 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="4642c-300">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="4642c-301">Typ MIME (`Content-Type`) musi być ustawiona, a musi być zgodny typ MIME skonfigurowane na <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="4642c-301">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="4642c-302">Żądanie nie może zawierać `Content-Range` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="4642c-302">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="4642c-303">Żądanie musi używać protokołu niezabezpieczonego (http), chyba, że bezpiecznego protokołu (https) jest skonfigurowana w opcjach oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4642c-303">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="4642c-304">*Należy pamiętać, niebezpieczeństwo [opisanych powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznego kompresję zawartości.*</span><span class="sxs-lookup"><span data-stu-id="4642c-304">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4642c-305">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4642c-305">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="4642c-306">Mozilla Developer Network: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="4642c-306">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="4642c-307">RFC 7231 Section 3.1.2.1: Zawierać zawartości</span><span class="sxs-lookup"><span data-stu-id="4642c-307">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="4642c-308">RFC 7230 sekcja 4.2.3: Kodowanie w formacie gzip</span><span class="sxs-lookup"><span data-stu-id="4642c-308">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="4642c-309">Wersja specyfikacji formatu pliku GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="4642c-309">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
