---
title: Buforowanie odpowiedzi w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać odpowiedzi buforowanie, aby niższe wymagania dotyczące przepustowości i zwiększyć wydajność aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: c53ae3f6ab8d26588533772dd4fdacb36ec12059
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077767"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="f9c2b-103">Buforowanie odpowiedzi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9c2b-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="f9c2b-104">Przez [Luo Jan](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f9c2b-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="f9c2b-105">Na stronach Razor buforowanie odpowiedzi jest dostępne w programie ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="f9c2b-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f9c2b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f9c2b-107">Buforowanie odpowiedzi zmniejsza liczbę żądań, które powoduje, że klient lub serwer proxy na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="f9c2b-108">Buforowanie odpowiedzi zmniejsza ilość pracy serwera sieci web wykonuje do generowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="f9c2b-109">Buforowanie odpowiedzi jest kontrolowana przez nagłówki, które określają sposób klienta, serwera proxy i oprogramowanie pośredniczące do odpowiedzi z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="f9c2b-110">Serwer sieci web może buforować odpowiedzi, po dodaniu [buforowanie odpowiedzi w oprogramowaniu pośredniczącym](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="f9c2b-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="f9c2b-111">Buforowanie odpowiedzi oparte na protokole HTTP</span><span class="sxs-lookup"><span data-stu-id="f9c2b-111">HTTP-based response caching</span></span>

<span data-ttu-id="f9c2b-112">[Specyfikacji protokołu HTTP 1.1 buforowanie](https://tools.ietf.org/html/rfc7234) opisano zachowanie pamięci podręcznych Internet.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="f9c2b-113">Jest podstawowy nagłówka HTTP używana do buforowania [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), który służy do określania pamięci podręcznej *dyrektywy*.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="f9c2b-114">Dyrektywy kontroli zachowania buforowania żądań umożliwiającej im od klientów na serwerach i ponieważ odpowiedź sposób ich z serwerów do klientów.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="f9c2b-115">Żądania i odpowiedzi Przenieś za pośrednictwem serwerów proxy, i serwery proxy również musi być zgodna z Specyfikacja protokołu HTTP 1.1 buforowania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="f9c2b-116">Typowe `Cache-Control` dyrektywy przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="f9c2b-117">Dyrektywy</span><span class="sxs-lookup"><span data-stu-id="f9c2b-117">Directive</span></span>                                                       | <span data-ttu-id="f9c2b-118">Akcja</span><span class="sxs-lookup"><span data-stu-id="f9c2b-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="f9c2b-119">public</span><span class="sxs-lookup"><span data-stu-id="f9c2b-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="f9c2b-120">Odpowiedzi mogą być przechowywane w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="f9c2b-121">private</span><span class="sxs-lookup"><span data-stu-id="f9c2b-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="f9c2b-122">Odpowiedź nie mogą być przechowywane przez współużytkowanej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="f9c2b-123">Prywatne pamięć podręczna może przechowywać i ponowne użycie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="f9c2b-124">max-age</span><span class="sxs-lookup"><span data-stu-id="f9c2b-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="f9c2b-125">Klient nie będzie akceptować odpowiedzi, którego okres ważności jest większa niż określoną liczbę sekund.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="f9c2b-126">Przykłady: `max-age=60` (60 sekund), `max-age=2592000` (miesiąc)</span><span class="sxs-lookup"><span data-stu-id="f9c2b-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="f9c2b-127">no-cache</span><span class="sxs-lookup"><span data-stu-id="f9c2b-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="f9c2b-128">**W odpowiedzi na żądania**: pamięci podręcznej nie może używać odpowiedzi przechowywane do spełnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="f9c2b-129">Uwaga: Serwer pochodzenia ponownie generuje odpowiedzi do klienta, a oprogramowanie pośredniczące aktualizuje odpowiedzi przechowywane w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="f9c2b-130">**W odpowiedzi**: odpowiedź nie mogą być używane dla kolejnych żądań bez sprawdzania poprawności na serwerze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="f9c2b-131">nie magazynu</span><span class="sxs-lookup"><span data-stu-id="f9c2b-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="f9c2b-132">**W odpowiedzi na żądania**: pamięć podręczna nie mogą przechowywać żądania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="f9c2b-133">**W odpowiedzi**: pamięć podręczna nie mogą przechowywać dowolną część odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="f9c2b-134">W poniższej tabeli przedstawiono innych nagłówków pamięci podręcznej, które uczestniczy w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="f9c2b-135">nagłówek</span><span class="sxs-lookup"><span data-stu-id="f9c2b-135">Header</span></span>                                                     | <span data-ttu-id="f9c2b-136">Funkcja</span><span class="sxs-lookup"><span data-stu-id="f9c2b-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="f9c2b-137">Okres ważności</span><span class="sxs-lookup"><span data-stu-id="f9c2b-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="f9c2b-138">Szacowana ilość czasu w sekundach od czasu odpowiedzi został wygenerowany lub pomyślnie zweryfikowane na serwerze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="f9c2b-139">Wygasa</span><span class="sxs-lookup"><span data-stu-id="f9c2b-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="f9c2b-140">Data/Godzina po upływie którego odpowiedź jest uznawane za przestarzałe.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="f9c2b-141">Wartość dyrektywy pragma</span><span class="sxs-lookup"><span data-stu-id="f9c2b-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="f9c2b-142">Istnieje dla zapewnienia zgodności z protokołu HTTP/1.0 buforuje ustawienia `no-cache` zachowanie.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="f9c2b-143">Jeśli `Cache-Control` występuje nagłówek `Pragma` nagłówka zostanie zignorowany.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="f9c2b-144">Różnią się</span><span class="sxs-lookup"><span data-stu-id="f9c2b-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="f9c2b-145">Określa, że buforowanej odpowiedzi może nie zostać wysłane dopóki wszystkie z `Vary` pola nagłówka są takie same w nowe żądanie i odpowiedź buforowana oryginalne żądanie.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="f9c2b-146">Oparte na protokole HTTP względem buforowania żądań dyrektywy Cache-Control</span><span class="sxs-lookup"><span data-stu-id="f9c2b-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="f9c2b-147">[HTTP 1.1 buforowanie specyfikacji nagłówek Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) wymaga pamięci podręcznej uwzględnić prawidłową `Cache-Control` nagłówka wysłane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="f9c2b-148">Klientowi mogą wysyłać żądania z `no-cache` wartość nagłówka i Wymuś serwera, aby wygenerować nową odpowiedź na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="f9c2b-149">Zawsze zachowaniu klienta `Cache-Control` nagłówków żądań ma sens, jeśli należy wziąć pod uwagę w celu buforowania HTTP.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="f9c2b-150">W obszarze specyfikację oficjalnego buforowanie ma zmniejszyć koszty opóźnienia i sieci spełnienia żądania w sieci klientów, serwery proxy i serwerów.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="f9c2b-151">Nie jest zawsze pozwala sterować obciążenia na serwerze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="f9c2b-152">Istnieje nie bieżącego deweloperowi kontroli nad to zachowanie buforowania, gdy przy użyciu [buforowanie odpowiedzi w oprogramowaniu pośredniczącym](xref:performance/caching/middleware) ponieważ oprogramowanie pośredniczące odpowiada oficjalne buforowanie specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="f9c2b-153">[Przyszłe ulepszenia oprogramowanie pośredniczące](https://github.com/aspnet/ResponseCaching/issues/96) pozwoli na konfigurowanie oprogramowania pośredniczącego, aby zignorować żądania `Cache-Control` nagłówka podczas podejmowania decyzji o do obsługi buforowanej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="f9c2b-154">To zaoferuje Ci możliwość lepszą kontrolę obciążenia na serwerze używania oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="f9c2b-155">Inne technologie buforowania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9c2b-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="f9c2b-156">Buforowanie w pamięci</span><span class="sxs-lookup"><span data-stu-id="f9c2b-156">In-memory caching</span></span>

<span data-ttu-id="f9c2b-157">Buforowanie w pamięci używa pamięci serwera do przechowywania danych z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="f9c2b-158">Tego rodzaju buforowanie jest odpowiedni dla pojedynczy serwer lub wiele serwerów przy użyciu *trwałe sesje*.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="f9c2b-159">Trwałe sesje oznacza, że żądania od klienta zawsze są kierowane do tego samego serwera przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="f9c2b-160">Aby uzyskać więcej informacji, zobacz [pamięci podręcznej w pamięci](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="f9c2b-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="f9c2b-161">Rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f9c2b-161">Distributed Cache</span></span>

<span data-ttu-id="f9c2b-162">Rozproszonej pamięci podręcznej umożliwia przechowywanie danych w pamięci, gdy aplikacja jest hostowana na farmie sieci chmury lub serwera.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="f9c2b-163">Pamięć podręczna jest współużytkowana przez serwery, które przetwarzają żądania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="f9c2b-164">Klienci mogą przesyłać żądania, który jest obsługiwany przez dowolnego serwera w grupie, jeśli są dostępne dane pamięci podręcznej klienta.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="f9c2b-165">Platformy ASP.NET Core oferuje programu SQL Server i pamięci podręczne Redis rozproszonych.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="f9c2b-166">Aby uzyskać więcej informacji, zobacz [pracować z rozproszonej pamięci podręcznej](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="f9c2b-166">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="f9c2b-167">Pamięć podręczna pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="f9c2b-167">Cache Tag Helper</span></span>

<span data-ttu-id="f9c2b-168">Może buforować zawartość z widoku MVC lub Razor strony z pamięci podręcznej pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="f9c2b-169">Pamięć podręczna pomocnika tagów używa do przechowywania danych buforowanie w pamięci.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="f9c2b-170">Aby uzyskać więcej informacji, zobacz [pomocnika tagów pamięci podręcznej w programie ASP.NET MVC Core](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f9c2b-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="f9c2b-171">Pomocnik Tag rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f9c2b-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="f9c2b-172">Zawartość z widoku MVC lub Razor strony w chmurze rozproszonej lub scenariusze farmy sieci web może buforować z Pomocnika Tag rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="f9c2b-173">Pomocnika Tag rozproszonej pamięci podręcznej używa programu SQL Server lub Redis do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="f9c2b-174">Aby uzyskać więcej informacji, zobacz [rozproszonej pamięci podręcznej pomocnika tagów](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f9c2b-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="f9c2b-175">Atrybut ResponseCache</span><span class="sxs-lookup"><span data-stu-id="f9c2b-175">ResponseCache attribute</span></span>

<span data-ttu-id="f9c2b-176">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) określa parametry, które są niezbędne do ustawiania odpowiednich nagłówków w ramach buforowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="f9c2b-177">Wyłącz buforowanie zawartości, który zawiera informacje dotyczące klientów, uwierzytelnionym.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="f9c2b-178">Buforowanie powinna być włączona tylko dla zawartości, która nie zmienia się na podstawie tożsamości użytkownika lub określa, czy użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="f9c2b-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) odpowiedzi przechowywane zależy od wartości danej listy kluczy zapytania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="f9c2b-180">Gdy do pojedynczej wartości `*` została podana, oprogramowanie pośredniczące zmienia się odpowiedzi dla wszystkich żądań parametrów ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="f9c2b-181">`VaryByQueryKeys` wymaga platformy ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="f9c2b-182">Oprogramowanie pośredniczące buforowanie odpowiedzi musi być włączona, aby ustawić `VaryByQueryKeys` właściwość; w przeciwnym razie jest zgłaszany wyjątek czasu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="f9c2b-183">Nie ma odpowiedniego nagłówek HTTP dla `VaryByQueryKeys` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="f9c2b-184">Właściwość jest funkcją HTTP obsługiwane przez oprogramowanie pośredniczące buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="f9c2b-185">Oprogramowanie pośredniczące do obsługi buforowaną odpowiedź ciąg zapytania i wartość ciągu zapytania musi odpowiadać poprzedniego żądania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="f9c2b-186">Rozważmy na przykład sekwencji żądań i wyniki będą wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="f9c2b-187">Żądanie</span><span class="sxs-lookup"><span data-stu-id="f9c2b-187">Request</span></span>                          | <span data-ttu-id="f9c2b-188">Wynik</span><span class="sxs-lookup"><span data-stu-id="f9c2b-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="f9c2b-189">Zwrócony z serwera</span><span class="sxs-lookup"><span data-stu-id="f9c2b-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="f9c2b-190">Zwrócone przez oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="f9c2b-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="f9c2b-191">Zwrócony z serwera</span><span class="sxs-lookup"><span data-stu-id="f9c2b-191">Returned from server</span></span>     |

<span data-ttu-id="f9c2b-192">Pierwsze żądanie jest zwrócony przez serwer i w pamięci podręcznej w oprogramowaniu pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="f9c2b-193">Drugie żądanie jest zwracana przez oprogramowanie pośredniczące, ponieważ ciąg zapytania jest zgodna z poprzedniego żądania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="f9c2b-194">W pamięci podręcznej oprogramowania pośredniczącego nie trzeci żądania, ponieważ wartość ciągu kwerendy nie pasuje do poprzedniego żądania.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="f9c2b-195">`ResponseCacheAttribute` Służy do konfigurowania i utworzyć (za pośrednictwem `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="f9c2b-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="f9c2b-196">`ResponseCacheFilter` Wykonuje pracę aktualizacji odpowiednich nagłówków HTTP i funkcje odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="f9c2b-197">Filtr:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-197">The filter:</span></span>

* <span data-ttu-id="f9c2b-198">Usuwa wszystkie istniejące nagłówki dla `Vary`, `Cache-Control`, i `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="f9c2b-199">Zapisuje się odpowiednie nagłówki na podstawie właściwości w `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="f9c2b-200">Aktualizuje odpowiedzi HTTP funkcji buforowania, jeśli `VaryByQueryKeys` jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="f9c2b-201">Różnią się</span><span class="sxs-lookup"><span data-stu-id="f9c2b-201">Vary</span></span>

<span data-ttu-id="f9c2b-202">Ten nagłówek są zapisywane tylko, gdy `VaryByHeader` właściwość jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="f9c2b-203">Ma ustawioną wartość `Vary` wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="f9c2b-204">Następujące przykładowe używa `VaryByHeader` właściwości:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="f9c2b-205">Można wyświetlić nagłówki odpowiedzi z narzędziami sieci w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="f9c2b-206">Na poniższej ilustracji przedstawiono krawędzi naciśnięcia klawisza F12, dane wyjściowe na **sieci** karcie kiedy `About2` odświeżeniu metody akcji:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Krawędzi F12 dane wyjściowe na karcie sieciowej, gdy wywoływana jest metoda akcji About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="f9c2b-208">NoStore i Location.None</span><span class="sxs-lookup"><span data-stu-id="f9c2b-208">NoStore and Location.None</span></span>

<span data-ttu-id="f9c2b-209">`NoStore` zastępuje większości innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="f9c2b-210">Jeśli ta właściwość jest skonfigurowana `true`, `Cache-Control` ma ustawioną wartość nagłówka `no-store`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="f9c2b-211">Jeśli `Location` ma ustawioną wartość `None`:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="f9c2b-212">`Cache-Control` ustawiono `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="f9c2b-213">`Pragma` ustawiono `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="f9c2b-214">Jeśli `NoStore` jest `false` i `Location` jest `None`, `Cache-Control` i `Pragma` są ustawione na `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="f9c2b-215">Zwykle ustawiana `NoStore` do `true` na stron błędów.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="f9c2b-216">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="f9c2b-217">Powoduje to następujące nagłówki:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="f9c2b-218">Miejsce i czas trwania</span><span class="sxs-lookup"><span data-stu-id="f9c2b-218">Location and Duration</span></span>

<span data-ttu-id="f9c2b-219">Aby włączyć buforowanie, `Duration` musi mieć ustawioną wartość dodatnią i `Location` musi być równa albo `Any` (ustawienie domyślne) lub `Client`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="f9c2b-220">W takim przypadku `Cache-Control` nagłówka ma ustawioną wartość lokalizacji, a następnie `max-age` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="f9c2b-221">`Location`w opcji `Any` i `Client` umożliwiło `Cache-Control` wartości nagłówka `public` i `private`odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="f9c2b-222">Jak wspomniano wcześniej, ustawienie `Location` do `None` ustawia zarówno `Cache-Control` i `Pragma` nagłówków `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="f9c2b-223">Poniżej przedstawiono przykładowy nagłówki powstaje przez ustawienie `Duration` i pozostawienie domyślnej `Location` wartość:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="f9c2b-224">Spowoduje to utworzenie następujący nagłówek:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="f9c2b-225">Profile pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f9c2b-225">Cache profiles</span></span>

<span data-ttu-id="f9c2b-226">Zamiast duplikowania `ResponseCache` ustawienia na wiele atrybutów akcji kontrolera, profile pamięci podręcznej można skonfigurować jako opcje podczas konfigurowania MVC w `ConfigureServices` metoda `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="f9c2b-227">Wartości znajdujące się w profilu pamięci podręcznej przywoływanego są używane jako domyślne przez `ResponseCache` atrybutu i zostały zastąpione przez dowolne właściwości określony w atrybucie.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="f9c2b-228">Konfigurowanie profilu pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f9c2b-229">Odwołanie do profilu pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="f9c2b-230">`ResponseCache` Atrybut może odnosić się zarówno do działania (metody) i kontrolery (klasy).</span><span class="sxs-lookup"><span data-stu-id="f9c2b-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="f9c2b-231">Atrybuty na poziomie metody zastępują ustawienia określone w poziomie klasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="f9c2b-232">W powyższym przykładzie atrybut poziomie klasy określa czas trwania wynoszący 30 sekund, podczas gdy atrybut poziomie metody odwołuje się do profilu pamięci podręcznej o czasie trwania ustawioną 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="f9c2b-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="f9c2b-233">Wynikowa nagłówka:</span><span class="sxs-lookup"><span data-stu-id="f9c2b-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="f9c2b-234">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f9c2b-234">Additional resources</span></span>

* [<span data-ttu-id="f9c2b-235">Przechowywanie odpowiedzi w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f9c2b-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="f9c2b-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="f9c2b-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="f9c2b-237">Buforowanie w pamięci</span><span class="sxs-lookup"><span data-stu-id="f9c2b-237">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="f9c2b-238">Praca z rozproszoną pamięcią podręczną</span><span class="sxs-lookup"><span data-stu-id="f9c2b-238">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="f9c2b-239">Wykrywanie zmian z tokenami zmiany</span><span class="sxs-lookup"><span data-stu-id="f9c2b-239">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="f9c2b-240">Oprogramowanie pośredniczące buforowania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="f9c2b-240">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="f9c2b-241">Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f9c2b-241">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="f9c2b-242">Pomocnik tagu rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f9c2b-242">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
