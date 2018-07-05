---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Zawartość negocjacji we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, jak ASP.NET Web API implementuje negocjacje zawartości HTTP.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: c4e7a0c2601ca60f081876e83757997a2e920298
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368932"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="2b4b0-103">Negocjowanie zawartości we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="2b4b0-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="2b4b0-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2b4b0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2b4b0-105">W tym artykule opisano, jak ASP.NET Web API implementuje negocjacji zawartości.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="2b4b0-106">Specyfikacja protokołu HTTP (RFC 2616) definiuje negocjacje zawartości jako "proces wybierania najlepszych reprezentację danej odpowiedzi, gdy istnieje wiele reprezentacji."</span><span class="sxs-lookup"><span data-stu-id="2b4b0-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="2b4b0-107">Te nagłówki żądania są podstawowym mechanizmem negocjowanie zawartości w protokole HTTP:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="2b4b0-108">**Akceptuj:** typów nośników, które są obsługiwane w odpowiedzi, np. "application/json", "application/xml" lub typu niestandardowe, takie jak &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="2b4b0-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="2b4b0-109">**Accept-Charset:** zestawów znaków, które są dopuszczalne, takich jak UTF-8 lub ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="2b4b0-110">**Zaakceptuj-Encoding:** które kodowań zawartości są dopuszczalne, takich jak gzip.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="2b4b0-111">**Zaakceptuj — wartość pola język:** preferowanego języka naturalnego, takich jak "en-us".</span><span class="sxs-lookup"><span data-stu-id="2b4b0-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="2b4b0-112">Serwer można również przeglądać inne części żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="2b4b0-113">Na przykład jeśli żądanie zawiera nagłówek X-Requested-With, wskazujący żądaniem AJAX serwer może domyślnie do formatu JSON w przypadku bez nagłówka Accept.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="2b4b0-114">W tym artykule omówimy używaniu nagłówków Accept i Accept-Charset interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="2b4b0-115">(W tej chwili nie jest brak wbudowanej obsługi Accept-Encoding lub Accept-Language).</span><span class="sxs-lookup"><span data-stu-id="2b4b0-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="2b4b0-116">Serializacja</span><span class="sxs-lookup"><span data-stu-id="2b4b0-116">Serialization</span></span>

<span data-ttu-id="2b4b0-117">Jeśli kontroler internetowego interfejsu API zwraca zasobu jako typ CLR, potok serializuje wartość zwracaną i zapisuje go w treści odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="2b4b0-118">Na przykład należy wziąć pod uwagę następujące akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="2b4b0-119">Klient może wysłać to żądanie HTTP:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="2b4b0-120">Serwer może wysłać w odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="2b4b0-121">W tym przykładzie klient zażądał JSON, Javascript lub "nic" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="2b4b0-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="2b4b0-122">Serwer odpowiada przy użyciu reprezentacji JSON `Product` obiektu.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="2b4b0-123">Należy zauważyć, że nagłówek Content-Type w odpowiedzi jest ustawiona na &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="2b4b0-124">Kontroler może również zwracać **obiektu HttpResponseMessage** obiektu.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="2b4b0-125">Aby określić obiekt CLR treść odpowiedzi, należy wywołać **CreateResponse** — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="2b4b0-126">Ta opcja zapewnia większą kontrolę nad szczegóły odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="2b4b0-127">Można ustawić kod stanu, Dodaj nagłówki HTTP i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="2b4b0-128">Obiekt, który serializuje zasobu jest nazywany *nośnika elementu formatującego*.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="2b4b0-129">Programy formatujące multimedia pochodzić od **element MediaTypeFormatter** klasy.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="2b4b0-130">Internetowy interfejs API udostępnia programy formatujące multimedia dla formatów XML i JSON. Ponadto można tworzyć niestandardowe elementy formatujące do obsługi innych typów nośników.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="2b4b0-131">Informacje na temat pisania niestandardowego elementu formatującego, zobacz [programy formatujące multimedia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="2b4b0-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="2b4b0-132">Jak zawartości działa negocjowania</span><span class="sxs-lookup"><span data-stu-id="2b4b0-132">How Content Negotiation Works</span></span>

<span data-ttu-id="2b4b0-133">Po pierwsze, potok pobiera **IContentNegotiator** usługi z **HttpConfiguration** obiektu.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="2b4b0-134">Zapewnia również na liście programy formatujące multimedia z **HttpConfiguration.Formatters** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="2b4b0-135">Następnie potok wywołuje **IContentNegotiatior.Negotiate**, przekazując:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="2b4b0-136">Typ obiektu do zserializowania</span><span class="sxs-lookup"><span data-stu-id="2b4b0-136">The type of object to serialize</span></span>
- <span data-ttu-id="2b4b0-137">Kolekcja programy formatujące multimedia</span><span class="sxs-lookup"><span data-stu-id="2b4b0-137">The collection of media formatters</span></span>
- <span data-ttu-id="2b4b0-138">Żądanie HTTP</span><span class="sxs-lookup"><span data-stu-id="2b4b0-138">The HTTP request</span></span>

<span data-ttu-id="2b4b0-139">**Negotiate** metoda zwraca dwóch rodzajów informacji:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="2b4b0-140">Które element formujący do używania</span><span class="sxs-lookup"><span data-stu-id="2b4b0-140">Which formatter to use</span></span>
- <span data-ttu-id="2b4b0-141">Typ nośnika dla odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="2b4b0-141">The media type for the response</span></span>

<span data-ttu-id="2b4b0-142">Jeśli element formatujący nie zostanie znaleziony, **Negotiate** metoda zwraca **null**i klienta błędu recevies HTTP 406 (niedozwolone).</span><span class="sxs-lookup"><span data-stu-id="2b4b0-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="2b4b0-143">Poniższy kod pokazuje, jak kontroler bezpośrednio wywołać negocjacje zawartości:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="2b4b0-144">Ten kod jest równoważna do strony co potoku następuje automatyczne.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="2b4b0-145">Moduł negocjowania zawartości domyślny</span><span class="sxs-lookup"><span data-stu-id="2b4b0-145">Default Content Negotiator</span></span>

<span data-ttu-id="2b4b0-146">**DefaultContentNegotiator** klasa udostępnia domyślną implementację elementu **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="2b4b0-147">Aby wybrać element formatujący używa kilku kryteriów.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="2b4b0-148">Po pierwsze element formatujący musi mieć możliwość wykonywać serializację typu.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="2b4b0-149">To jest weryfikowana przez wywołanie metody **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="2b4b0-150">Następnie moduł negocjowania zawartości sprawdza każdy element formatujący i ocenia, jak dobrze pasuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="2b4b0-151">Aby ocenić dopasowanie, moduł negocjowania zawartości sprawdza na element formatujący dwie rzeczy:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="2b4b0-152">**SupportedMediaTypes** kolekcji, która zawiera listę obsługiwanych typów nośnika.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="2b4b0-153">Moduł negocjowania zawartości próbuje dopasować tej listy przed żądaniem nagłówek Accept.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="2b4b0-154">Należy pamiętać, że nagłówek Accept mogą zawierać zakresy.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="2b4b0-155">Na przykład "text/plain" to pasuje do tekstu /\* lub \* / \*.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="2b4b0-156">**MediaTypeMappings** kolekcji, która zawiera listę **MediaTypeMapping** obiektów.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="2b4b0-157">**MediaTypeMapping** klasy zapewnia ogólny sposób zgodne z żądaniami HTTP przy użyciu typów nośników.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="2b4b0-158">Na przykład go zamapuj niestandardowego nagłówka HTTP, do określonego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="2b4b0-159">Jeśli dostępnych jest wiele jest zgodny, zgodna z wins współczynnik najwyższej jakości.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="2b4b0-160">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2b4b0-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="2b4b0-161">W tym przykładzie application/json ma czynnikiem dorozumianych jakości w wersji 1.0, więc jest preferowana nad application/xml.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="2b4b0-162">Jeśli nie znaleziono żadnych dopasowań, moduł negocjowania zawartości próbuje dopasować na typ nośnika treści żądania, jeśli istnieje.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="2b4b0-163">Na przykład jeśli żądanie zawiera dane JSON, moduł negocjowania zawartości szuka elementu formatującego JSON.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="2b4b0-164">Jeśli nadal istnieją żadnych dopasowań, moduł negocjowania zawartości po prostu wybiera pierwszy element formatujący, który może wykonać serializację typu.</span><span class="sxs-lookup"><span data-stu-id="2b4b0-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="2b4b0-165">Wybranie kodowania znaków</span><span class="sxs-lookup"><span data-stu-id="2b4b0-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="2b4b0-166">Po wybraniu elementu formatującego moduł negocjowania zawartości wybiera optymalne kodowanie znaków, analizując **SupportedEncodings** właściwość elementu formatującego i dopasowywanie względem nagłówek Accept-Charset w żądaniu (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="2b4b0-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
