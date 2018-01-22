---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: "Obsługa formatu BSON w składniku ASP.NET Web API 2.1 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="1fbc7-102">Obsługa formatu BSON w składniku ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="1fbc7-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="1fbc7-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1fbc7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1fbc7-104">Składnik Web API 2.1 wprowadzono obsługę formatu BSON.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="1fbc7-105">W tym temacie pokazano, jak używać formatu BSON w kontrolerze interfejsu API sieci Web (po stronie serwera) i w aplikacji klienta .NET.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="1fbc7-106">Co to jest BSON?</span><span class="sxs-lookup"><span data-stu-id="1fbc7-106">What is BSON?</span></span>

<span data-ttu-id="1fbc7-107">[BSON](http://bsonspec.org/) to format serializacji binarnej.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="1fbc7-108">"BSON" oznacza "Binary JSON", ale są bardzo inaczej serializowane formatu BSON i JSON.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="1fbc7-109">Jest BSON "JSON przypominającej", ponieważ obiekty są reprezentowane jako pary nazwa wartość, podobny do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="1fbc7-110">W przeciwieństwie do formatu JSON numeryczne typy danych są przechowywane w postaci bajtów, nie ciągów</span><span class="sxs-lookup"><span data-stu-id="1fbc7-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="1fbc7-111">BSON został opracowany jako lekkie, łatwo przeglądać i szybkie do kodowania dekodowania.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="1fbc7-112">BSON jest porównywalna o rozmiarze do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="1fbc7-113">W zależności od danych ładunku BSON może być mniejsza lub większa od ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="1fbc7-114">Dla serializacji danych binarnych, takich jak plik obrazu BSON jest mniejszy niż JSON, ponieważ dane binarne nie jest kodowany w formacie base64.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="1fbc7-115">Dokumenty BSON są łatwe do skanowania, ponieważ elementy są poprzedzane prefiksem długość pola, więc analizatorem przejść elementów bez ich dekodowania.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="1fbc7-116">Kodowania i dekodowania są wydajne, ponieważ numeryczne typy danych są przechowywane w postaci liczb i ciągów nie.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="1fbc7-117">Natywny klientów, takich jak aplikacje klienta .NET, mogą korzystać z formatu BSON zamiast formatów tekstowym, takich jak JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="1fbc7-118">Dla klientów w przeglądarkach prawdopodobnie można przestrzegaj JSON, ponieważ usługa języka JavaScript bezpośrednio można przekonwertować ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="1fbc7-119">Na szczęście używa interfejsu API sieci Web [negocjowanie zawartości](content-negotiation.md), więc interfejsu API można obsługuje zarówno formaty i umożliwić klientowi wybierz.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="1fbc7-120">Włączanie BSON na serwerze</span><span class="sxs-lookup"><span data-stu-id="1fbc7-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="1fbc7-121">W konfiguracji interfejsu API sieci Web, Dodaj **BsonMediaTypeFormatter** do kolekcji elementów formatujących.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="1fbc7-122">Teraz Jeśli klient żąda "application/bson", interfejsu API sieci Web zostanie użyty element formatujący formatu BSON.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="1fbc7-123">Aby skojarzyć BSON z innych typów nośników, należy je dodać do kolekcji SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="1fbc7-124">Poniższy kod dodaje "application/vnd.contoso" do typów nośników obsługiwanych:</span><span class="sxs-lookup"><span data-stu-id="1fbc7-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="1fbc7-125">Przykład sesji HTTP</span><span class="sxs-lookup"><span data-stu-id="1fbc7-125">Example HTTP Session</span></span>

<span data-ttu-id="1fbc7-126">W tym przykładzie użyjemy następującej klasy modelu oraz proste kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="1fbc7-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="1fbc7-127">Klient może wysyłać następujące żądania HTTP:</span><span class="sxs-lookup"><span data-stu-id="1fbc7-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="1fbc7-128">Poniżej przedstawiono odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="1fbc7-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="1fbc7-129">W tym miejscu zostały zastąpione danych binarnych z &quot;.&quot; znaków.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="1fbc7-130">Zrzut ekranu następujące z pokazuje Fiddler nieprzetworzonej wartości szesnastkowych.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="1fbc7-131">Przy użyciu formatu BSON z HttpClient</span><span class="sxs-lookup"><span data-stu-id="1fbc7-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="1fbc7-132">Aplikacje klientów .NET mogą używać element formatujący BSON **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="1fbc7-133">Aby uzyskać więcej informacji na temat **HttpClient**, zobacz [wywoływania sieci Web interfejsu API z klienta programu .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="1fbc7-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="1fbc7-134">Poniższy kod wysyła żądanie GET, który akceptuje BSON, a następnie deserializuje ładunku BSON w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="1fbc7-135">Aby zażądać BSON z serwera, ustawić nagłówek Accept do "application/bson":</span><span class="sxs-lookup"><span data-stu-id="1fbc7-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="1fbc7-136">Aby deserializacji treści odpowiedzi, należy użyć **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="1fbc7-137">Ten program formatujący nie jest w domyślnej kolekcji elementów formatujących, dlatego należy określić podczas odczytu treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="1fbc7-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="1fbc7-138">Kolejnym przykładzie pokazano, jak można wysłać żądania POST, który zawiera BSON.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="1fbc7-139">Większość tego kodu jest taka sama, co w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="1fbc7-140">Jednak **PostAsync** metody, określ **BsonMediaTypeFormatter** jako element formatujący:</span><span class="sxs-lookup"><span data-stu-id="1fbc7-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="1fbc7-141">Serializacja najwyższego poziomu typy pierwotne</span><span class="sxs-lookup"><span data-stu-id="1fbc7-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="1fbc7-142">Każdy dokument BSON znajduje się lista par klucz/wartość. Specyfikacja BSON nie definiuje składni dla serializacji jednego nieprzetworzonej wartości, takich jak integer lub string.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="1fbc7-143">Aby obejść to ograniczenie **BsonMediaTypeFormatter** traktuje typów pierwotnych w szczególnych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="1fbc7-144">Przed rozpoczęciem serializacji, konwertuje wartość na parę klucz wartość z kluczem "Value".</span><span class="sxs-lookup"><span data-stu-id="1fbc7-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="1fbc7-145">Na przykład załóżmy, że dany kontroler interfejsu API zwraca liczbę całkowitą:</span><span class="sxs-lookup"><span data-stu-id="1fbc7-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="1fbc7-146">Przed rozpoczęciem serializacji, element formatujący BSON konwertuje to z następującą parą klucza i wartości:</span><span class="sxs-lookup"><span data-stu-id="1fbc7-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="1fbc7-147">Podczas deserializacji, element formatujący konwertuje je do oryginalnej wartości.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="1fbc7-148">Jednak klientów przy użyciu różnych analizatora BSON należy obsługiwać ten przypadek, jeśli interfejs API sieci web zwraca wartości w wierszach.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="1fbc7-149">Ogólnie rzecz biorąc należy rozważyć zwracanie wartości w wierszach, a nie dane strukturalne.</span><span class="sxs-lookup"><span data-stu-id="1fbc7-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1fbc7-150">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1fbc7-150">Additional Resources</span></span>

[<span data-ttu-id="1fbc7-151">Przykładowe BSON interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="1fbc7-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="1fbc7-152">Programy formatujące multimedia</span><span class="sxs-lookup"><span data-stu-id="1fbc7-152">Media Formatters</span></span>](media-formatters.md)
