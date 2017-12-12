---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: "Programy formatujące multimedia w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7d85b995cd577d0ff90fe96bce508c7fbdc6ebbb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="3d1c8-102">Programy formatujące multimedia w składniku ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="3d1c8-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="3d1c8-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3d1c8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3d1c8-104">Ten samouczek przedstawia sposób obsługuje dodatkowe formaty w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-104">This tutorial shows how support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="3d1c8-105">Typy nośnika internetowych</span><span class="sxs-lookup"><span data-stu-id="3d1c8-105">Internet Media Types</span></span>

<span data-ttu-id="3d1c8-106">Typ nośnika, nazywany również typ MIME, określa format elementu danych.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="3d1c8-107">W protokole HTTP typów nośników opisywania formatu treści wiadomości.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="3d1c8-108">Typ nośnika składa się z dwóch ciągów, typu i podtypu.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="3d1c8-109">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3d1c8-109">For example:</span></span>

- <span data-ttu-id="3d1c8-110">tekst i html</span><span class="sxs-lookup"><span data-stu-id="3d1c8-110">text/html</span></span>
- <span data-ttu-id="3d1c8-111">Obraz/png</span><span class="sxs-lookup"><span data-stu-id="3d1c8-111">image/png</span></span>
- <span data-ttu-id="3d1c8-112">Application/json</span><span class="sxs-lookup"><span data-stu-id="3d1c8-112">application/json</span></span>

<span data-ttu-id="3d1c8-113">Jeśli wiadomość HTTP zawiera treść jednostki, nagłówka Content-Type określa format treści wiadomości.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="3d1c8-114">Ta wartość informuje odbiornika jak przeanalizować zawartość treści wiadomości.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="3d1c8-115">Na przykład jeśli odpowiedź HTTP zawiera obrazu PNG, odpowiedzi może mieć następujące nagłówki.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="3d1c8-116">Gdy klient wysyła komunikat żądania, może obejmować nagłówek Accept.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="3d1c8-117">Nagłówek Accept informuje, że potrzebuje serwera multimedialnych typ(-y) klienta z serwera.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="3d1c8-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3d1c8-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="3d1c8-119">Ten nagłówek informuje serwer, że klient chce HTML, XHTML lub XML.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="3d1c8-120">Typ nośnika określa sposób interfejsu API sieci Web serializuje i deserializuje treść komunikatu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="3d1c8-121">Interfejs API sieci Web ma wbudowaną obsługę XML, JSON formatu BSON i urlencoded formularza danych i może obsługiwać dodatkowe pliki multimedialne pisząc *nośnika elementu formatującego*.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="3d1c8-122">Aby utworzyć element formatujący nośnik, pochodzi z jednego z tych klas:</span><span class="sxs-lookup"><span data-stu-id="3d1c8-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="3d1c8-123">[Klasa MediaTypeFormatter](https://msdn.microsoft.com/en-us/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d1c8-123">[MediaTypeFormatter](https://msdn.microsoft.com/en-us/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="3d1c8-124">Odczyt asynchroniczny używa klasy i metody zapisu.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="3d1c8-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/en-us/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d1c8-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/en-us/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="3d1c8-126">Ta klasa pochodzi od **MediaTypeFormatter** , ale sychronous metody odczytu/zapisu.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="3d1c8-127">Wyprowadzanie z **BufferedMediaTypeFormatter** jest łatwiejsze, ponieważ nie jest wykonywany kod asynchroniczne, ale oznacza to również wątek wywołujący może zablokować podczas operacji We/Wy.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="3d1c8-128">Przykład: Tworzenie elementu formatującego nośnika CSV</span><span class="sxs-lookup"><span data-stu-id="3d1c8-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="3d1c8-129">W poniższym przykładzie pokazano element formatujący typu nośnika, który może wykonać serializację obiektu produktu w formacie wartości rozdzielanych przecinkami (CSV).</span><span class="sxs-lookup"><span data-stu-id="3d1c8-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="3d1c8-130">W tym przykładzie użyto produktu zdefiniowanych w samouczku [Tworzenie składnika Web API ten obsługuje operacje CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="3d1c8-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="3d1c8-131">W tym miejscu znajduje się definicja metody obiektu produktu:</span><span class="sxs-lookup"><span data-stu-id="3d1c8-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="3d1c8-132">Aby zaimplementować elementu formatującego CSV, Definiowanie klasy, która jest pochodną **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="3d1c8-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="3d1c8-133">W Konstruktorze Dodaj do typów nośników, które obsługuje program formatujący.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="3d1c8-134">W tym przykładzie element formatujący obsługuje typ jednym nośniku, &quot;tekstu/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="3d1c8-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="3d1c8-135">Zastąpienie **CanWriteType** metodę, aby wskazać typów element formatujący może serializować:</span><span class="sxs-lookup"><span data-stu-id="3d1c8-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="3d1c8-136">W tym przykładzie element formatujący może serializować pojedynczego `Product` obiektów oraz kolekcji `Product` obiektów.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="3d1c8-137">Podobnie, Zastąp **CanReadType** metodę, aby wskazać typów element formatujący może deserializować.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="3d1c8-138">W tym przykładzie element formatujący nie obsługuje deserializacji, dlatego metoda po prostu zwraca **false**.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="3d1c8-139">Ponadto Zastąp **WriteToStream** metody.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="3d1c8-140">Ta metoda wykonuje serializację typu przez zapisywanie w strumieniu.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="3d1c8-141">Jeśli formatujący deserializacji także zastępować **ReadFromStream** metody.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="3d1c8-142">Dodawanie elementu formatującego nośnika do potoku składnika Web API</span><span class="sxs-lookup"><span data-stu-id="3d1c8-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="3d1c8-143">Aby dodać typ nośnika elementu formatującego do potoku interfejsu API sieci Web, użyj **elementy formatujące** właściwość **HttpConfiguration** obiektu.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="3d1c8-144">Kodowanie znaków</span><span class="sxs-lookup"><span data-stu-id="3d1c8-144">Character Encodings</span></span>

<span data-ttu-id="3d1c8-145">Opcjonalnie element formatujący nośnik może obsługiwać wiele kodowanie znaków, takich jak UTF-8 lub ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="3d1c8-146">W konstruktorze, należy dodać co najmniej jeden [System.Text.Encoding](https://msdn.microsoft.com/en-us/library/system.text.encoding.aspx) typów, aby **SupportedEncodings** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/en-us/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="3d1c8-147">Umieść domyślne kodowanie pierwszej.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="3d1c8-148">W **WriteToStream** i **ReadFromStream** wywołania metody, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/en-us/library/hh969054.aspx) Wybierz kodowanie znaków preferowany.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/en-us/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="3d1c8-149">Tej metody jest zgodny z listą obsługiwanych kodowań nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="3d1c8-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="3d1c8-150">Użyj zwróconego **kodowanie** podczas odczytu lub zapisu strumienia:</span><span class="sxs-lookup"><span data-stu-id="3d1c8-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
