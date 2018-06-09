---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Zawartość negocjacji w składniku ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, jak składnika ASP.NET Web API zaimplementowano negocjacje zawartości HTTP.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566411"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="3d6c5-103">Negocjowanie zawartości w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="3d6c5-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="3d6c5-104">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3d6c5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3d6c5-105">W tym artykule opisano, jak składnika ASP.NET Web API zaimplementowano negocjacji zawartości.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="3d6c5-106">Specyfikacja protokołu HTTP (RFC 2616) definiuje negocjacje zawartości jako "proces polegający na wyborze najlepsze reprezentację danej odpowiedzi, gdy wiele reprezentacje są dostępne."</span><span class="sxs-lookup"><span data-stu-id="3d6c5-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="3d6c5-107">Podstawowy mechanizm negocjacje zawartości HTTP czy te nagłówki żądań:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="3d6c5-108">**Akceptuj:** typów nośników, które są dozwolone dla odpowiedzi, takie jak "application/json", "application/xml" lub typu niestandardowe, takie jak &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="3d6c5-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="3d6c5-109">**Accept-Charset:** zestawów znaków, które są dozwolone, takich jak UTF-8 lub ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="3d6c5-110">**Zaakceptuj kodowania:** które kodowań zawartości są dozwolone, takich jak gzip.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="3d6c5-111">**Zaakceptuj język:** preferowanego języka naturalnego, takich jak "en-us".</span><span class="sxs-lookup"><span data-stu-id="3d6c5-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="3d6c5-112">Serwer można również sprawdzić innych części żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="3d6c5-113">Na przykład jeśli żądanie zawiera nagłówek X-Requested-With, wskazującą żądaniem AJAX serwer może być domyślnie JSON Jeśli nagłówek Accept nie występuje.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="3d6c5-114">W tym artykule wyjaśniono, jak interfejsu API sieci Web używa nagłówków Accept i Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="3d6c5-115">(W tej chwili nie jest brak wbudowaną obsługę Accept-Encoding lub Accept-Language).</span><span class="sxs-lookup"><span data-stu-id="3d6c5-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="3d6c5-116">Serializacja</span><span class="sxs-lookup"><span data-stu-id="3d6c5-116">Serialization</span></span>

<span data-ttu-id="3d6c5-117">Jeśli Kontroler interfejsu API sieci Web zwraca zasobu jako typ CLR, potoku serializuje zwracana wartość i zapisuje go w treści odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="3d6c5-118">Na przykład wziąć pod uwagę następujące akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="3d6c5-119">Klient może wysyłać żądania HTTP:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="3d6c5-120">W odpowiedzi serwer może wysłać:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="3d6c5-121">W tym przykładzie klient żądał JSON, Javascript lub "wszystko" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="3d6c5-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="3d6c5-122">Odpowiedź zawierała reprezentacja JSON serwera `Product` obiektu.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="3d6c5-123">Należy zauważyć, że ma ustawioną wartość nagłówka Content-Type w odpowiedzi &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="3d6c5-124">Kontroler może również zwrócić **HttpResponseMessage** obiektu.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="3d6c5-125">Aby określić obiektu CLR dla treści odpowiedzi, należy wywołać **CreateResponse** — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="3d6c5-126">Opcja ta zapewnia większą kontrolę nad treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="3d6c5-127">Można ustawić kod stanu, Dodaj nagłówków HTTP i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="3d6c5-128">Obiekt, który serializuje zasobu jest nazywany *nośnika elementu formatującego*.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="3d6c5-129">Programy formatujące multimedia pochodzi od **MediaTypeFormatter** klasy.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="3d6c5-130">Interfejs API sieci Web udostępnia programy formatujące multimedia XML i JSON, a można tworzyć niestandardowe elementy formatujące do obsługi innych typów nośników.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="3d6c5-131">Informacje na temat pisania niestandardowego elementu formatującego, zobacz [programy formatujące multimedia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="3d6c5-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="3d6c5-132">Jak zawartości działa negocjacji</span><span class="sxs-lookup"><span data-stu-id="3d6c5-132">How Content Negotiation Works</span></span>

<span data-ttu-id="3d6c5-133">Najpierw pobiera potoku **IContentNegotiator** usługi **HttpConfiguration** obiektu.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="3d6c5-134">Zapewnia również na liście programy formatujące multimedia z **HttpConfiguration.Formatters** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="3d6c5-135">Następnie wywołuje potok **IContentNegotiatior.Negotiate**, przekazując:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="3d6c5-136">Typ obiektu do serializacji</span><span class="sxs-lookup"><span data-stu-id="3d6c5-136">The type of object to serialize</span></span>
- <span data-ttu-id="3d6c5-137">Kolekcja programy formatujące multimedia</span><span class="sxs-lookup"><span data-stu-id="3d6c5-137">The collection of media formatters</span></span>
- <span data-ttu-id="3d6c5-138">Żądania HTTP</span><span class="sxs-lookup"><span data-stu-id="3d6c5-138">The HTTP request</span></span>

<span data-ttu-id="3d6c5-139">**Negotiate** metoda zwraca informacje:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="3d6c5-140">Które element formatujący do użycia</span><span class="sxs-lookup"><span data-stu-id="3d6c5-140">Which formatter to use</span></span>
- <span data-ttu-id="3d6c5-141">Typ nośnika dla odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3d6c5-141">The media type for the response</span></span>

<span data-ttu-id="3d6c5-142">Jeśli element formatujący nie zostanie znaleziony, **Negotiate** metoda zwraca **null**, a błąd recevies HTTP klienta 406 (niedopuszczalne).</span><span class="sxs-lookup"><span data-stu-id="3d6c5-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="3d6c5-143">Poniższy kod przedstawia sposób kontrolera można bezpośrednio wywołać negocjacje zawartości:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="3d6c5-144">Ten kod jest równoważna do potoku jest automatycznie.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="3d6c5-145">Moduł negocjowania zawartości domyślny</span><span class="sxs-lookup"><span data-stu-id="3d6c5-145">Default Content Negotiator</span></span>

<span data-ttu-id="3d6c5-146">**DefaultContentNegotiator** klasa udostępnia domyślną implementację elementu **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="3d6c5-147">Aby wybrać element formatujący używa kilku kryteriów.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="3d6c5-148">Po pierwsze element formatujący musi mieć możliwość wykonywać serializację typu.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="3d6c5-149">To jest zweryfikowany przez wywołanie metody **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="3d6c5-150">Następnie moduł negocjowania zawartości sprawdza elementu formatującego i ocenia skuteczność odpowiada żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="3d6c5-151">Aby ocenić dopasowania, moduł negocjowania zawartości sprawdza na program formatujący dwie czynności:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="3d6c5-152">**SupportedMediaTypes** kolekcji, która zawiera listę obsługiwanych typów nośnika.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="3d6c5-153">Moduł negocjowania zawartości próbuje odpowiada liście dla żądania nagłówek Accept.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="3d6c5-154">Należy pamiętać, że nagłówek Accept mogą obejmować zakresów.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="3d6c5-155">Na przykład "text/plain" ma dopasowania dla tekstu /\* lub \* / \*.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="3d6c5-156">**MediaTypeMappings** kolekcji, który zawiera listę **MediaTypeMapping** obiektów.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="3d6c5-157">**MediaTypeMapping** klasa umożliwia ogólne pasuje żądania HTTP do typów nośników.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="3d6c5-158">Na przykład można go mapować niestandardowy nagłówek HTTP do określonego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="3d6c5-159">Jeśli dostępnych jest wiele zgodne, niezgodne z wins współczynnik najwyższej jakości.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="3d6c5-160">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3d6c5-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="3d6c5-161">W tym przykładzie application/json ma współczynnik jakości domniemanych 1.0, więc jest preferowana przez kod xml i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="3d6c5-162">W przypadku nieodnalezienia żadnych dopasowań moduł negocjowania zawartości próbuje dopasować typ nośnika treści żądania, jeśli istnieje.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="3d6c5-163">Na przykład jeśli żądanie zawiera dane JSON, moduł negocjowania zawartości szuka elementu formatującego JSON.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="3d6c5-164">Jeśli nadal nie mają odpowiedników, moduł negocjowania zawartości po prostu wybiera pierwszy element formatujący, który może wykonać serializację typu.</span><span class="sxs-lookup"><span data-stu-id="3d6c5-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="3d6c5-165">Wybranie kodowania znaków</span><span class="sxs-lookup"><span data-stu-id="3d6c5-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="3d6c5-166">Po wybraniu elementu formatującego, moduł negocjowania zawartości wybierze optymalne kodowanie znaków na podstawie **SupportedEncodings** właściwość element formatujący, a odpowiadające mu względem nagłówka Accept-Charset w żądaniu (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="3d6c5-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
