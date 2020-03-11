---
title: Operacje żądań i odpowiedzi w ASP.NET Core
author: jkotalik
description: Dowiedz się, jak odczytać treść żądania i napisać treść odpowiedzi w ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b473fa02e1d23f02bc5d2e15fa54ab7b1dbbb17c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667217"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="180af-103">Operacje żądań i odpowiedzi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="180af-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="180af-104">Autor [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="180af-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="180af-105">W tym artykule wyjaśniono sposób odczytywania treści żądania i zapisywania jej w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="180af-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="180af-106">Kod dla tych operacji może być wymagany podczas pisania oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="180af-106">Code for these operations might be required when writing middleware.</span></span> <span data-ttu-id="180af-107">Poza pisaniem oprogramowania pośredniczącego kod niestandardowy nie jest zazwyczaj wymagany, ponieważ operacje są obsługiwane przez MVC i Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="180af-107">Outside of writing middleware, custom code isn't generally required because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="180af-108">Istnieją dwa abstrakcje treści żądania i odpowiedzi: <xref:System.IO.Stream> i <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="180af-108">There are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="180af-109">W przypadku odczytu żądania żądanie [HttpRequest. Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) jest <xref:System.IO.Stream>, a `HttpRequest.BodyReader` jest <xref:System.IO.Pipelines.PipeReader>em.</span><span class="sxs-lookup"><span data-stu-id="180af-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyReader` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="180af-110">Na potrzeby pisania odpowiedzi [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) jest <xref:System.IO.Stream>, a `HttpResponse.BodyWriter` jest <xref:System.IO.Pipelines.PipeWriter>em.</span><span class="sxs-lookup"><span data-stu-id="180af-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a <xref:System.IO.Stream>, and `HttpResponse.BodyWriter` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="180af-111">[Potoki](/dotnet/standard/io/pipelines) są zalecane za pośrednictwem strumieni.</span><span class="sxs-lookup"><span data-stu-id="180af-111">[Pipelines](/dotnet/standard/io/pipelines) are recommended over streams.</span></span> <span data-ttu-id="180af-112">Strumienie mogą być łatwiejsze w przypadku niektórych prostych operacji, ale potoki mają zalety wydajności i są łatwiejsze w większości scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="180af-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="180af-113">ASP.NET Core zaczyna używać potoków zamiast strumieni wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="180af-113">ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="180af-114">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="180af-114">Examples include:</span></span>

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

<span data-ttu-id="180af-115">Strumienie nie są usuwane z struktury.</span><span class="sxs-lookup"><span data-stu-id="180af-115">Streams aren't being removed from the framework.</span></span> <span data-ttu-id="180af-116">Strumienie są nadal używane w środowisku .NET, a wiele typów strumieni nie ma odpowiedników potoku, takich jak `FileStreams` i `ResponseCompression`.</span><span class="sxs-lookup"><span data-stu-id="180af-116">Streams continue to be used throughout .NET, and many stream types don't have pipe equivalents, such as `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="180af-117">Przykłady przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="180af-117">Stream examples</span></span>

<span data-ttu-id="180af-118">Załóżmy, że celem jest utworzenie oprogramowania pośredniczącego, które odczytuje całą treść żądania jako listę ciągów, dzieląc je na nowe wiersze.</span><span class="sxs-lookup"><span data-stu-id="180af-118">Suppose the goal is to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="180af-119">Prosta implementacja strumienia może wyglądać podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="180af-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="180af-120">Ten kod działa, ale wystąpiły pewne problemy:</span><span class="sxs-lookup"><span data-stu-id="180af-120">This code works, but there are some issues:</span></span>

* <span data-ttu-id="180af-121">Przed dołączeniem do `StringBuilder`, w przykładzie tworzony jest kolejny ciąg (`encodedString`), który zostanie natychmiast wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="180af-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="180af-122">Ten proces występuje dla wszystkich bajtów w strumieniu, więc wynikiem jest dodatkowa alokacja pamięci o rozmiarze całej treści żądania.</span><span class="sxs-lookup"><span data-stu-id="180af-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
* <span data-ttu-id="180af-123">Przykład odczytuje cały ciąg przed podziałem w nowych wierszach.</span><span class="sxs-lookup"><span data-stu-id="180af-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="180af-124">Aby sprawdzić, czy nowe wiersze w tablicy bajtów są bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="180af-124">It's more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="180af-125">Oto przykład, który rozwiązuje niektóre z powyższych problemów:</span><span class="sxs-lookup"><span data-stu-id="180af-125">Here's an example that fixes some of the preceding issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="180af-126">Ten poprzedni przykład:</span><span class="sxs-lookup"><span data-stu-id="180af-126">This preceding example:</span></span>

* <span data-ttu-id="180af-127">Nie buforuje całej treści żądania w `StringBuilder`, chyba że nie ma żadnych znaków nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="180af-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
* <span data-ttu-id="180af-128">Nie wywołuje `Split` w ciągu.</span><span class="sxs-lookup"><span data-stu-id="180af-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="180af-129">Jednak nadal występują pewne problemy:</span><span class="sxs-lookup"><span data-stu-id="180af-129">However, there are still are a few issues:</span></span>

* <span data-ttu-id="180af-130">Jeśli znaki nowego wiersza są rozrzedzone, większość treści żądania jest buforowana w ciągu.</span><span class="sxs-lookup"><span data-stu-id="180af-130">If newline characters are sparse, much of the request body is buffered in the string.</span></span>
* <span data-ttu-id="180af-131">Kod nadal tworzy ciągi (`remainingString`) i dodaje je do buforu ciągów, co skutkuje dodatkową alokacją.</span><span class="sxs-lookup"><span data-stu-id="180af-131">The code continues to create strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="180af-132">Te problemy są fixable, ale kod staje się coraz bardziej skomplikowany przy niewielkim ulepszaniu.</span><span class="sxs-lookup"><span data-stu-id="180af-132">These issues are fixable, but the code is becoming progressively more complicated with little improvement.</span></span> <span data-ttu-id="180af-133">Potoki umożliwiają rozwiązanie tych problemów przy minimalnej złożoności kodu.</span><span class="sxs-lookup"><span data-stu-id="180af-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="180af-134">Potoki</span><span class="sxs-lookup"><span data-stu-id="180af-134">Pipelines</span></span>

<span data-ttu-id="180af-135">Poniższy przykład pokazuje, jak można obsłużyć ten sam scenariusz przy użyciu `PipeReader`:</span><span class="sxs-lookup"><span data-stu-id="180af-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="180af-136">Ten przykład naprawia wiele problemów, które zostały wdrożone przez implementacje strumieni:</span><span class="sxs-lookup"><span data-stu-id="180af-136">This example fixes many issues that the streams implementations had:</span></span>

* <span data-ttu-id="180af-137">Bufor ciągu nie jest potrzebny, ponieważ `PipeReader` obsługuje bajty, które nie zostały użyte.</span><span class="sxs-lookup"><span data-stu-id="180af-137">There's no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
* <span data-ttu-id="180af-138">Zakodowane ciągi są bezpośrednio dodawane do listy zwracanych ciągów.</span><span class="sxs-lookup"><span data-stu-id="180af-138">Encoded strings are directly added to the list of returned strings.</span></span>
* <span data-ttu-id="180af-139">Tworzenie ciągów jest wolne od alokacji poza pamięcią używaną przez ciąg (z wyjątkiem wywołania `ToArray()`).</span><span class="sxs-lookup"><span data-stu-id="180af-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="180af-140">Zainstalowanych</span><span class="sxs-lookup"><span data-stu-id="180af-140">Adapters</span></span>

<span data-ttu-id="180af-141">Zarówno `Body`, jak i właściwości `BodyReader/BodyWriter` są dostępne dla `HttpRequest` i `HttpResponse`.</span><span class="sxs-lookup"><span data-stu-id="180af-141">Both `Body` and `BodyReader/BodyWriter` properties are available for `HttpRequest` and `HttpResponse`.</span></span> <span data-ttu-id="180af-142">Po ustawieniu `Body` na inny strumień, nowy zestaw kart automatycznie dostosowuje każdy typ do drugiego.</span><span class="sxs-lookup"><span data-stu-id="180af-142">When you set `Body` to a different stream, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="180af-143">Jeśli ustawisz `HttpRequest.Body` na nowy strumień, `HttpRequest.BodyReader` zostanie automatycznie ustawiona na nowy `PipeReader` zawijający `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="180af-143">If you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyReader` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span>

## <a name="startasync"></a><span data-ttu-id="180af-144">StartAsync</span><span class="sxs-lookup"><span data-stu-id="180af-144">StartAsync</span></span>

<span data-ttu-id="180af-145">`HttpResponse.StartAsync` służy do wskazywania, że nagłówki są niemodyfikowalne i aby można było uruchamiać wywołania zwrotne `OnStarting`.</span><span class="sxs-lookup"><span data-stu-id="180af-145">`HttpResponse.StartAsync` is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="180af-146">Gdy jest używany Kestrel jako serwer, wywoływanie `StartAsync` przed użyciem `PipeReader` gwarantuje, że pamięć zwracana przez `GetMemory` należy do wewnętrznego <xref:System.IO.Pipelines.Pipe>, a nie do buforu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="180af-146">When using Kestrel as a server, calling `StartAsync` before using the `PipeReader` guarantees that memory returned by `GetMemory` belongs to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="180af-147">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="180af-147">Additional resources</span></span>

* [<span data-ttu-id="180af-148">Wprowadzenie do System. IO. potoków</span><span class="sxs-lookup"><span data-stu-id="180af-148">Introducing System.IO.Pipelines</span></span>](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>
