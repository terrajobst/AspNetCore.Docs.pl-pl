---
title: Buforowanie odpowiedzi w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać odpowiedzi z pamięci podręcznej w celu niższymi wymaganiami co do przepustowości i zwiększenie wydajności aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 01/07/2018
uid: performance/caching/response
ms.openlocfilehash: 5fbcaddff6e53d01a19ba8a7455c719feb614326
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098951"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="47f7c-103">Buforowanie odpowiedzi w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47f7c-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="47f7c-104">Przez [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="47f7c-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="47f7c-105">Buforowanie odpowiedzi w stron Razor jest dostępna w programie ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="47f7c-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="47f7c-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47f7c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="47f7c-107">Buforowanie odpowiedzi zmniejsza liczbę żądań, które sprawia, że klient lub serwer proxy na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="47f7c-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="47f7c-108">Buforowanie odpowiedzi zmniejsza ilość pracy serwera sieci web wykonuje do generowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="47f7c-109">Buforowanie odpowiedzi jest kontrolowana przez nagłówki, które określają, jak chcesz klienta, serwera proxy i oprogramowaniu pośredniczącym, aby buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="47f7c-110">[Atrybut ResponseCache](#responsecache-attribute) uczestniczy podczas ustawiania nagłówków, których klienci mogą przestrzegać podczas buforowania odpowiedzi buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-110">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="47f7c-111">[Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) może służyć do odpowiedzi z pamięci podręcznej na serwerze.</span><span class="sxs-lookup"><span data-stu-id="47f7c-111">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="47f7c-112">Można użyć oprogramowania pośredniczącego `ResponseCache` atrybutu właściwości w celu wywierania wpływu na zachowanie buforowania po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="47f7c-112">The middleware can use `ResponseCache` attribute properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="47f7c-113">Buforowanie odpowiedzi oparte na protokole HTTP</span><span class="sxs-lookup"><span data-stu-id="47f7c-113">HTTP-based response caching</span></span>

<span data-ttu-id="47f7c-114">[Specyfikacji protokołu HTTP 1.1 buforowania](https://tools.ietf.org/html/rfc7234) opisuje zachowanie pamięci podręcznych Internet.</span><span class="sxs-lookup"><span data-stu-id="47f7c-114">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="47f7c-115">Jest podstawowym nagłówek HTTP używana do buforowania [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), który jest używany do określenia pamięci podręcznej *dyrektywy*.</span><span class="sxs-lookup"><span data-stu-id="47f7c-115">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="47f7c-116">Dyrektywy Sterowanie zachowaniem buforowania żądań przedostają się od klientów do serwerów i jako odpowiedź przedostają się z serwerów z powrotem do klientów.</span><span class="sxs-lookup"><span data-stu-id="47f7c-116">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="47f7c-117">Żądania i odpowiedzi, przenoszenie za pośrednictwem serwerów proxy i serwery proxy również musi być zgodna ze specyfikacją protokołu HTTP 1.1 buforowania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-117">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="47f7c-118">Typowe `Cache-Control` dyrektywy są wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="47f7c-118">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="47f7c-119">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="47f7c-119">Directive</span></span>                                                       | <span data-ttu-id="47f7c-120">Akcja</span><span class="sxs-lookup"><span data-stu-id="47f7c-120">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="47f7c-121">public</span><span class="sxs-lookup"><span data-stu-id="47f7c-121">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="47f7c-122">Pamięć podręczna może przechowywać odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-122">A cache may store the response.</span></span> |
| [<span data-ttu-id="47f7c-123">private</span><span class="sxs-lookup"><span data-stu-id="47f7c-123">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="47f7c-124">Odpowiedź nie mogą być przechowywane w udostępnionej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="47f7c-124">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="47f7c-125">Prywatnej pamięci podręcznej może przechowywać i ponowne użycie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-125">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="47f7c-126">max-age</span><span class="sxs-lookup"><span data-stu-id="47f7c-126">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="47f7c-127">Klient nie będzie akceptować odpowiedzi, którego okres ważności jest większa niż określoną liczbę sekund.</span><span class="sxs-lookup"><span data-stu-id="47f7c-127">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="47f7c-128">Przykłady: `max-age=60` (60 sekund), `max-age=2592000` (1 miesiąc)</span><span class="sxs-lookup"><span data-stu-id="47f7c-128">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="47f7c-129">no-cache</span><span class="sxs-lookup"><span data-stu-id="47f7c-129">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="47f7c-130">**W odpowiedzi na żądania**: Pamięć podręczna nie może używać odpowiedzi przechowywane do spełnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-130">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="47f7c-131">Uwaga: Serwer pochodzenia ponownie generuje odpowiedzi klienta, a oprogramowanie pośredniczące aktualizuje odpowiedzi przechowywane w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="47f7c-131">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="47f7c-132">**W odpowiedzi**: Odpowiedź nie mogą być używane dla kolejnych żądań bez sprawdzania poprawności na serwerze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="47f7c-132">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="47f7c-133">nie-store</span><span class="sxs-lookup"><span data-stu-id="47f7c-133">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="47f7c-134">**W odpowiedzi na żądania**: Pamięć podręczna nie musi przechowywać żądania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-134">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="47f7c-135">**W odpowiedzi**: Pamięć podręczna nie musi przechowywać dowolną część odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-135">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="47f7c-136">W poniższej tabeli przedstawiono innych nagłówków pamięci podręcznej, które mają znaczenie w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="47f7c-136">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="47f7c-137">nagłówek</span><span class="sxs-lookup"><span data-stu-id="47f7c-137">Header</span></span>                                                     | <span data-ttu-id="47f7c-138">Funkcja</span><span class="sxs-lookup"><span data-stu-id="47f7c-138">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="47f7c-139">Wiek</span><span class="sxs-lookup"><span data-stu-id="47f7c-139">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="47f7c-140">Szacowana ilość czasu w sekundach, ponieważ odpowiedzi został wygenerowany pomyślnie zweryfikowane na serwerze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="47f7c-140">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="47f7c-141">Wygasa</span><span class="sxs-lookup"><span data-stu-id="47f7c-141">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="47f7c-142">Data/Godzina, po czym odpowiedź jest uznawana za starych.</span><span class="sxs-lookup"><span data-stu-id="47f7c-142">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="47f7c-143">Dyrektywy pragma</span><span class="sxs-lookup"><span data-stu-id="47f7c-143">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="47f7c-144">Istnieje wstecznej zgodności przy użyciu protokołu HTTP/1.0 buforuje ustawienia `no-cache` zachowanie.</span><span class="sxs-lookup"><span data-stu-id="47f7c-144">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="47f7c-145">Jeśli `Cache-Control` nagłówek jest obecny, `Pragma` nagłówka zostanie zignorowany.</span><span class="sxs-lookup"><span data-stu-id="47f7c-145">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="47f7c-146">różnią się</span><span class="sxs-lookup"><span data-stu-id="47f7c-146">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="47f7c-147">Określa, czy odpowiedzi z pamięci podręcznej musi nie zostać wysłane dopóki wszystkie z `Vary` pola nagłówka są takie same zarówno buforowaną odpowiedź oryginalne żądanie, jak i nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="47f7c-147">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="47f7c-148">Oparte na protokole HTTP względem buforowania żądań dyrektyw sterujących pamięcią podręczną</span><span class="sxs-lookup"><span data-stu-id="47f7c-148">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="47f7c-149">[Specyfikacji protokołu HTTP 1.1 buforowania dla nagłówka Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) wymaga pamięci podręcznej respektować prawidłową `Cache-Control` nagłówka wysłane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="47f7c-149">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="47f7c-150">Klientowi mogą wysyłać żądania z `no-cache` wartość nagłówka i Wymuś serwera, aby wygenerować nową odpowiedź dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-150">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="47f7c-151">Zawsze zapewniane klienta `Cache-Control` nagłówków żądań ma sens należy wziąć pod uwagę w celu buforowania HTTP.</span><span class="sxs-lookup"><span data-stu-id="47f7c-151">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="47f7c-152">W ramach specyfikacji oficjalne buforowania mają na celu obniżenie kosztów opóźnienia oraz sieci spełnienia żądania za pośrednictwem sieci klientów, serwery proxy i serwerów.</span><span class="sxs-lookup"><span data-stu-id="47f7c-152">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="47f7c-153">Nie jest koniecznie sposób kontroluje obciążenie serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="47f7c-153">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="47f7c-154">Istnieje nie deweloperowi kontroli nad tym zachowaniem buforowania przy użyciu [oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) ponieważ oprogramowanie pośredniczące działa zgodnie z oficjalną buforowania specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="47f7c-154">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="47f7c-155">[Planowane ulepszenia oprogramowanie pośredniczące](https://github.com/aspnet/AspNetCore/issues/2612) jest możliwość konfiguracji oprogramowania pośredniczącego, aby zignorować żądania `Cache-Control` nagłówka podczas podejmowania decyzji o do obsługi buforowanych odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-155">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="47f7c-156">Ulepszenia planowane udostępniają możliwość lepszej obciążenie serwera kontroli.</span><span class="sxs-lookup"><span data-stu-id="47f7c-156">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="47f7c-157">Innych technologii buforowania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47f7c-157">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="47f7c-158">Buforowanie w pamięci</span><span class="sxs-lookup"><span data-stu-id="47f7c-158">In-memory caching</span></span>

<span data-ttu-id="47f7c-159">Wewnątrzpamięciowa pamięć podręczna używa pamięci serwera do przechowywania danych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="47f7c-159">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="47f7c-160">Ten rodzaj buforowania nadaje się do pojedynczego serwera lub kilku serwerach wykorzystujących *trwałych sesji*.</span><span class="sxs-lookup"><span data-stu-id="47f7c-160">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="47f7c-161">Trwałych sesji oznacza, że żądania klienta są zawsze kierowane do tego samego serwera w celu przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="47f7c-161">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="47f7c-162">Aby uzyskać więcej informacji, zobacz [pamięci podręcznej w pamięci](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="47f7c-162">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="47f7c-163">Rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="47f7c-163">Distributed Cache</span></span>

<span data-ttu-id="47f7c-164">Umożliwia przechowywanie danych w pamięci, gdy aplikacja jest hostowana w chmurze i na serwerze farmie rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="47f7c-164">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="47f7c-165">Pamięć podręczna jest współużytkowany przez serwery, które przetwarzają żądania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-165">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="47f7c-166">Klienci mogą przesyłać żądania, które jest obsługiwane przez dowolnego serwera w grupie, jeśli dane w pamięci podręcznej klienta jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="47f7c-166">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="47f7c-167">ASP.NET Core oferuje program SQL Server i pamięci podręczne Redis rozproszonych.</span><span class="sxs-lookup"><span data-stu-id="47f7c-167">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="47f7c-168">Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="47f7c-168">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="47f7c-169">Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="47f7c-169">Cache Tag Helper</span></span>

<span data-ttu-id="47f7c-170">Za pomocą Pomocnik tagu pamięci podręcznej, można buforować zawartość widoku składnika MVC lub strona Razor.</span><span class="sxs-lookup"><span data-stu-id="47f7c-170">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="47f7c-171">Pomocnik tagu pamięci podręcznej używa buforowanie w pamięci do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="47f7c-171">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="47f7c-172">Aby uzyskać więcej informacji, zobacz [Pomocnik tagu pamięci podręcznej na platformie ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="47f7c-172">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="47f7c-173">Pomocnik tagu rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="47f7c-173">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="47f7c-174">Za pomocą rozproszonej Pomocnik tagu pamięci podręcznej, można buforować zawartość widoku składnika MVC lub strona Razor w rozproszonych w chmurze lub w scenariuszach z farmami internetowymi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-174">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="47f7c-175">Pomocnik tagu rozproszonej pamięci podręcznej używa programu SQL Server lub usługi Redis do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="47f7c-175">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="47f7c-176">Aby uzyskać więcej informacji, zobacz [rozproszonych Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="47f7c-176">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="47f7c-177">Atrybut ResponseCache</span><span class="sxs-lookup"><span data-stu-id="47f7c-177">ResponseCache attribute</span></span>

<span data-ttu-id="47f7c-178">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) określa parametry, które są niezbędne do ustawienie odpowiednich nagłówków w buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-178">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="47f7c-179">Wyłącz buforowanie zawartości, która zawiera informacje dotyczące uwierzytelnionych klientów.</span><span class="sxs-lookup"><span data-stu-id="47f7c-179">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="47f7c-180">Pamięć podręczna powinna być włączona tylko dla zawartości, która nie zmienia się na podstawie tożsamości użytkownika lub tego, czy użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="47f7c-180">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="47f7c-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) odpowiedzi przechowywane jest zależna od wartości podanej listy kluczy zapytania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="47f7c-182">Gdy do pojedynczej wartości `*` zostanie podana, oprogramowanie pośredniczące różni się w odpowiedzi dla wszystkich żądań parametrów ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-182">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="47f7c-183">`VaryByQueryKeys` wymaga platformy ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="47f7c-183">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="47f7c-184">[Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) musi być włączona, aby ustawić `VaryByQueryKeys` właściwości; w przeciwnym razie jest zgłaszany wyjątek czasu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-184">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="47f7c-185">Nie ma odpowiedniego nagłówka HTTP dla `VaryByQueryKeys` właściwości.</span><span class="sxs-lookup"><span data-stu-id="47f7c-185">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="47f7c-186">Właściwość jest funkcja protokołu HTTP obsługiwane przez oprogramowanie pośredniczące buforowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-186">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="47f7c-187">Oprogramowaniu pośredniczącym, aby obsługiwać odpowiedzi z pamięci podręcznej ciąg zapytania i wartość ciągu zapytania musi odpowiadać poprzedniego żądania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-187">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="47f7c-188">Na przykład należy wziąć pod uwagę sekwencji żądań i wyniki wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="47f7c-188">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="47f7c-189">Żądanie</span><span class="sxs-lookup"><span data-stu-id="47f7c-189">Request</span></span>                          | <span data-ttu-id="47f7c-190">Wynik</span><span class="sxs-lookup"><span data-stu-id="47f7c-190">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="47f7c-191">Zwrócone z serwera</span><span class="sxs-lookup"><span data-stu-id="47f7c-191">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="47f7c-192">Zwrócone przez oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="47f7c-192">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="47f7c-193">Zwrócone z serwera</span><span class="sxs-lookup"><span data-stu-id="47f7c-193">Returned from server</span></span>     |

<span data-ttu-id="47f7c-194">Pierwsze żądanie został zwrócony przez serwer, a następnie pamięci podręcznej w oprogramowaniu pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="47f7c-194">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="47f7c-195">Drugie żądanie jest zwracany przez oprogramowanie pośredniczące, ponieważ poprzednie żądanie pasuje do ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-195">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="47f7c-196">Trzecie żądanie nie jest w pamięci podręcznej oprogramowania pośredniczącego, ponieważ wartość ciągu zapytania nie pasuje do poprzedniego żądania.</span><span class="sxs-lookup"><span data-stu-id="47f7c-196">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="47f7c-197">`ResponseCacheAttribute` Służy do konfigurowania i utworzyć (za pośrednictwem `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="47f7c-197">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="47f7c-198">`ResponseCacheFilter` Wykonuje pracę aktualizowania tych funkcji, odpowiedzi i odpowiednie nagłówki HTTP.</span><span class="sxs-lookup"><span data-stu-id="47f7c-198">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="47f7c-199">Filtr:</span><span class="sxs-lookup"><span data-stu-id="47f7c-199">The filter:</span></span>

* <span data-ttu-id="47f7c-200">Usuwa wszystkie istniejące nagłówki `Vary`, `Cache-Control`, i `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-200">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="47f7c-201">Zapisuje się odpowiednie nagłówki, na podstawie właściwości ustawione w `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-201">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="47f7c-202">Aktualizuje odpowiedzi buforowania funkcji protokołu HTTP, jeśli `VaryByQueryKeys` jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="47f7c-202">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="47f7c-203">różnią się</span><span class="sxs-lookup"><span data-stu-id="47f7c-203">Vary</span></span>

<span data-ttu-id="47f7c-204">Tego pliku nagłówkowego są zapisywane tylko wtedy, kiedy `VaryByHeader` właściwość jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="47f7c-204">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="47f7c-205">Jest równa `Vary` wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="47f7c-205">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="47f7c-206">Następujące przykładowe używa `VaryByHeader` właściwości:</span><span class="sxs-lookup"><span data-stu-id="47f7c-206">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="47f7c-207">Można wyświetlić nagłówki odpowiedzi za pomocą narzędzi sieci w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="47f7c-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="47f7c-208">Na poniższej ilustracji przedstawiono F12 krawędzi, dane wyjściowe na **sieci** kartę, kiedy `About2` metody akcji jest odświeżany:</span><span class="sxs-lookup"><span data-stu-id="47f7c-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Krawędzi F12 w danych wyjściowych karty sieciowej, gdy wywoływana jest metoda akcji About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="47f7c-210">NoStore i Location.None</span><span class="sxs-lookup"><span data-stu-id="47f7c-210">NoStore and Location.None</span></span>

<span data-ttu-id="47f7c-211">`NoStore` zastępuje większości innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="47f7c-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="47f7c-212">Jeśli ta właściwość jest równa `true`, `Cache-Control` nagłówka ustawiono `no-store`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="47f7c-213">Jeśli `Location` ustawiono `None`:</span><span class="sxs-lookup"><span data-stu-id="47f7c-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="47f7c-214">`Cache-Control` ustawiono `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="47f7c-215">`Pragma` ustawiono `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="47f7c-216">Jeśli `NoStore` jest `false` i `Location` jest `None`, `Cache-Control` i `Pragma` są ustawione na `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="47f7c-217">Zwykle ustawiana `NoStore` do `true` na stron błędów.</span><span class="sxs-lookup"><span data-stu-id="47f7c-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="47f7c-218">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="47f7c-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="47f7c-219">Skutkuje to następujące nagłówki:</span><span class="sxs-lookup"><span data-stu-id="47f7c-219">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="47f7c-220">Lokalizacja i czas trwania</span><span class="sxs-lookup"><span data-stu-id="47f7c-220">Location and Duration</span></span>

<span data-ttu-id="47f7c-221">Aby włączyć buforowanie, `Duration` musi być równa wartości dodatniej i `Location` musi być albo `Any` (ustawienie domyślne) lub `Client`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-221">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="47f7c-222">W tym przypadku `Cache-Control` nagłówka ustawiono wartość lokalizacji, a następnie `max-age` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47f7c-222">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="47f7c-223">`Location`w opcji `Any` i `Client` przekłada się na `Cache-Control` wartości nagłówka `public` i `private`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="47f7c-223">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="47f7c-224">Jak wspomniano wcześniej, ustawienie `Location` do `None` ustawia zarówno `Cache-Control` i `Pragma` nagłówki, aby `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-224">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="47f7c-225">Poniżej przedstawiono przykładowy nagłówki określonemu przez ustawienie `Duration` i pozostawiając domyślną `Location` wartość:</span><span class="sxs-lookup"><span data-stu-id="47f7c-225">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="47f7c-226">To generuje następujący nagłówek:</span><span class="sxs-lookup"><span data-stu-id="47f7c-226">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="47f7c-227">Profile pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="47f7c-227">Cache profiles</span></span>

<span data-ttu-id="47f7c-228">Zamiast duplikowania `ResponseCache` ustawienia na wiele atrybutów akcji kontrolera, profile pamięci podręcznej można skonfigurować jako opcje podczas definiowania MVC w `ConfigureServices` method in Class metoda `Startup`.</span><span class="sxs-lookup"><span data-stu-id="47f7c-228">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="47f7c-229">Wartości znajdujące się w profilu pamięci podręcznej odwołania są używane jako domyślne za `ResponseCache` atrybutu i są zastępowane przez dowolne właściwości określone w atrybucie.</span><span class="sxs-lookup"><span data-stu-id="47f7c-229">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="47f7c-230">Konfigurowanie profilu pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="47f7c-230">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="47f7c-231">Odwoływanie się do profilu pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="47f7c-231">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="47f7c-232">`ResponseCache` Atrybut można stosować zarówno do działania (metod) i kontrolerów (grupy).</span><span class="sxs-lookup"><span data-stu-id="47f7c-232">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="47f7c-233">Atrybuty na poziomie zastępują ustawienia określone atrybuty na poziomie klasy.</span><span class="sxs-lookup"><span data-stu-id="47f7c-233">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="47f7c-234">W powyższym przykładzie atrybut poziomu klasa określa czas trwania wynoszący 30 sekund, gdy atrybut poziom metody odwołuje się do profilu pamięci podręcznej o czasie trwania wartość 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="47f7c-234">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="47f7c-235">Nagłówek wynikowy:</span><span class="sxs-lookup"><span data-stu-id="47f7c-235">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="47f7c-236">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="47f7c-236">Additional resources</span></span>

* [<span data-ttu-id="47f7c-237">Zapisywanie odpowiedzi w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="47f7c-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="47f7c-238">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="47f7c-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
