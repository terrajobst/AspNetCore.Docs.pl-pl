---
title: Operacje żądań i odpowiedzi w programie ASP.NET Core
author: jkotalik
description: Dowiedz się, jak treści żądania odczytu i zapisu treści odpowiedzi w programie ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jkotalik
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b6e3cd4b79e0c062b271c65cd5ecbdb4ef80c3a1
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208060"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="135e7-103">Operacje żądań i odpowiedzi w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="135e7-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="135e7-104">Przez [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="135e7-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="135e7-105">W tym artykule wyjaśniono, jak czytać z treści żądania i zapisywać w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="135e7-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="135e7-106">Może być konieczne pisanie kodu dla tych operacji, podczas pisania miej oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="135e7-106">You might need to write code for these operations when you're writing middleware.</span></span> <span data-ttu-id="135e7-107">W przeciwnym razie zwykle nie trzeba napisać ten kod, ponieważ operacje są obsługiwane przez MVC i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="135e7-107">Otherwise, you typically don't have to write this code because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="135e7-108">W programie ASP.NET Core 3.0 są dwa elementy abstrakcji do treści żądania i odpowiedzi: <xref:System.IO.Stream> i <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="135e7-108">In ASP.NET Core 3.0, there are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="135e7-109">Żądanie czytelności [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) jest <xref:System.IO.Stream>, i `HttpRequest.BodyPipe` jest <xref:System.IO.Pipelines.PipeReader>.</span><span class="sxs-lookup"><span data-stu-id="135e7-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyPipe` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="135e7-110">Do zapisu odpowiedzi [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) jest `HttpResponse.BodyPipe` jest <xref:System.IO.Pipelines.PipeWriter>.</span><span class="sxs-lookup"><span data-stu-id="135e7-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a `HttpResponse.BodyPipe` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="135e7-111">Firma Microsoft zaleca potoki za pośrednictwem strumieni.</span><span class="sxs-lookup"><span data-stu-id="135e7-111">We recommend pipelines over streams.</span></span> <span data-ttu-id="135e7-112">Strumienie może być łatwiejszy w obsłudze dla niektórych prostych operacji, ale potoki mają korzystać wydajność i są łatwiejsze w obsłudze w większości scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="135e7-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="135e7-113">3.0 ASP.NET Core zaczyna używać potoków zamiast strumieni wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="135e7-113">In 3.0, ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="135e7-114">Przykłady obejmują:</span><span class="sxs-lookup"><span data-stu-id="135e7-114">Examples include:</span></span>

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

<span data-ttu-id="135e7-115">Strumienie nie będą natychmiast.</span><span class="sxs-lookup"><span data-stu-id="135e7-115">Streams aren't going away.</span></span> <span data-ttu-id="135e7-116">Mogą być nadal używane w całej platformy .NET i wiele typów strumienia nie ma odpowiedników potoku, takie jak `FileStreams` i `ResponseCompression`.</span><span class="sxs-lookup"><span data-stu-id="135e7-116">They continue to be used throughout .NET, and many stream types don't have pipe equivalents, like `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="135e7-117">Przykłady Stream</span><span class="sxs-lookup"><span data-stu-id="135e7-117">Stream examples</span></span>

<span data-ttu-id="135e7-118">Załóżmy, że chcesz utworzyć oprogramowania pośredniczącego, które odczytuje całej treści żądania jako listę ciągów, dzielenie w nowych wierszach.</span><span class="sxs-lookup"><span data-stu-id="135e7-118">Suppose you want to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="135e7-119">Wykonania prostego strumień może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="135e7-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="135e7-120">Ten kod działa, ale występują pewne problemy:</span><span class="sxs-lookup"><span data-stu-id="135e7-120">This code works, but there are some issues:</span></span>

- <span data-ttu-id="135e7-121">Przed dołączeniem do `StringBuilder`, w przykładzie jest tworzony inny ciąg (`encodedString`), jest wyrzucać natychmiast.</span><span class="sxs-lookup"><span data-stu-id="135e7-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="135e7-122">Ten proces odbywa się dla wszystkich bajtów w strumieniu, więc dodatkową pamięć przydział rozmiaru całej treści żądania.</span><span class="sxs-lookup"><span data-stu-id="135e7-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
- <span data-ttu-id="135e7-123">Przykład odczytuje cały ciąg przed podziałem w nowych wierszach.</span><span class="sxs-lookup"><span data-stu-id="135e7-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="135e7-124">Byłoby bardziej wydajne, aby sprawdzić, czy są dostępne nowe wiersze w tablicy bajtów.</span><span class="sxs-lookup"><span data-stu-id="135e7-124">It would be more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="135e7-125">Oto przykład, w którym niektóre z tych problemów poprawki:</span><span class="sxs-lookup"><span data-stu-id="135e7-125">Here's an example that fixes some of these issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="135e7-126">W tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="135e7-126">This example:</span></span>

- <span data-ttu-id="135e7-127">Nie buforuje całej treści żądania w `StringBuilder` chyba, że nie ma żadnych znaków nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="135e7-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
- <span data-ttu-id="135e7-128">Nie wywołuje `Split` na ciąg.</span><span class="sxs-lookup"><span data-stu-id="135e7-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="135e7-129">Istnieją jednak są nadal kilka kwestii:</span><span class="sxs-lookup"><span data-stu-id="135e7-129">However, there are still are a few issues:</span></span>

- <span data-ttu-id="135e7-130">W przypadku rozrzedzone znakami znaczną część treści żądania są buforowane w ciągu.</span><span class="sxs-lookup"><span data-stu-id="135e7-130">If newline characters are sparse, much of the request body is buffered in the string .</span></span>
- <span data-ttu-id="135e7-131">Nadal tworzy ciągów (`remainingString`) i dodaje je do buforu ciągu, co skutkuje dodatkowy przydział.</span><span class="sxs-lookup"><span data-stu-id="135e7-131">It still creates strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="135e7-132">Te problemy to naprawić, ale kod jest staje się coraz bardziej skomplikowane niewielkie ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="135e7-132">These issues are fixable, but the code is becoming more and more complicated with little improvement.</span></span> <span data-ttu-id="135e7-133">Potoki zapewniają sposób, aby rozwiązać te problemy za pomocą minimalnej ilości kodu, złożoności.</span><span class="sxs-lookup"><span data-stu-id="135e7-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="135e7-134">Potoki</span><span class="sxs-lookup"><span data-stu-id="135e7-134">Pipelines</span></span>

<span data-ttu-id="135e7-135">W poniższym przykładzie pokazano, jak ten sam scenariusz może być obsługiwane za pomocą `PipeReader`:</span><span class="sxs-lookup"><span data-stu-id="135e7-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="135e7-136">W tym przykładzie rozwiązuje wiele problemów, które miały implementacje strumieni:</span><span class="sxs-lookup"><span data-stu-id="135e7-136">This example fixes many issues that the streams implementations had:</span></span>

- <span data-ttu-id="135e7-137">Nie jest konieczne dla buforu ciągu, ponieważ `PipeReader` obsługuje bajtów, które nie były używane.</span><span class="sxs-lookup"><span data-stu-id="135e7-137">There is no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
- <span data-ttu-id="135e7-138">Parametry zakodowanej bezpośrednio są dodawane do listy ciągów zwrócone.</span><span class="sxs-lookup"><span data-stu-id="135e7-138">Encoded strings are directly added to the list of returned strings.</span></span>
- <span data-ttu-id="135e7-139">Tworzenie ciągu jest bezpłatne alokacji oprócz pamięci używanej przez ciąg (z wyjątkiem `ToArray()` wywołania).</span><span class="sxs-lookup"><span data-stu-id="135e7-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="135e7-140">Karty</span><span class="sxs-lookup"><span data-stu-id="135e7-140">Adapters</span></span>

<span data-ttu-id="135e7-141">Skoro zarówno `Body` i `BodyPipe` właściwości są dostępne dla `HttpRequest` i `HttpResponse`, co się stanie po ustawieniu `Body` do różnych strumienia?</span><span class="sxs-lookup"><span data-stu-id="135e7-141">Now that both `Body` and `BodyPipe` properties are available for `HttpRequest` and `HttpResponse`, what happens when you set `Body` to a different stream?</span></span> <span data-ttu-id="135e7-142">3.0 nowy zbiór kart automatycznie Adaptowane każdego typu na drugi.</span><span class="sxs-lookup"><span data-stu-id="135e7-142">In 3.0, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="135e7-143">Na przykład jeśli ustawisz `HttpRequest.Body` na nowy strumień `HttpRequest.BodyPipe` automatycznie zostaje ustawiony nowy `PipeReader` to opakowuje `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="135e7-143">For example, if you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyPipe` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span> <span data-ttu-id="135e7-144">Takie samo zachowanie dotyczy ustawienie `BodyPipe` właściwości.</span><span class="sxs-lookup"><span data-stu-id="135e7-144">The same behavior applies to setting the `BodyPipe` property.</span></span> <span data-ttu-id="135e7-145">Jeśli `HttpResponse.BodyPipe` ustawiono nową `PipeWriter`, `HttpResponse.Body` automatycznie zostaje ustawiony nowy strumień, który otacza `HttpResponse.BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="135e7-145">If `HttpResponse.BodyPipe` is set to a new `PipeWriter`, the `HttpResponse.Body` is automatically set to a new stream that wraps `HttpResponse.BodyPipe`.</span></span>

## <a name="startasync"></a><span data-ttu-id="135e7-146">StartAsync</span><span class="sxs-lookup"><span data-stu-id="135e7-146">StartAsync</span></span>

<span data-ttu-id="135e7-147">`HttpResponse.StartAsync` jest nowa w 3.0.</span><span class="sxs-lookup"><span data-stu-id="135e7-147">`HttpResponse.StartAsync` is new in 3.0.</span></span> <span data-ttu-id="135e7-148">Służy do wskazania, że nagłówki są niemodyfikowalnych i uruchamiania `OnStarting` wywołań zwrotnych.</span><span class="sxs-lookup"><span data-stu-id="135e7-148">It is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="135e7-149">W 3.0 preview3, należy wywołać `StartAsync` przed użyciem `HttpRequest.BodyPipe`, i w przyszłych wersjach będą zalecenia.</span><span class="sxs-lookup"><span data-stu-id="135e7-149">In 3.0-preview3, you have to call `StartAsync` before using `HttpRequest.BodyPipe`, and in future releases, it will be a recommendation.</span></span> <span data-ttu-id="135e7-150">Korzystając z Kestrel jako serwera, wywoływanie StartAsync przed rozpoczęciem korzystania z `PipeReader` gwarantuje, że pamięć zwrócony przez `GetMemory` będą należeć do firmy Kestrel wewnętrzny <xref:System.IO.Pipelines.Pipe> zamiast zewnętrznych buforu.</span><span class="sxs-lookup"><span data-stu-id="135e7-150">When using Kestrel as a server, calling StartAsync before using the `PipeReader` guarantees that memory returned by `GetMemory` will belong to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="135e7-151">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="135e7-151">Additional resources</span></span>

- [<span data-ttu-id="135e7-152">Introducing System.IO.Pipelines</span><span class="sxs-lookup"><span data-stu-id="135e7-152">Introducing System.IO.Pipelines</span></span>](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
- <xref:fundamentals/middleware/write>