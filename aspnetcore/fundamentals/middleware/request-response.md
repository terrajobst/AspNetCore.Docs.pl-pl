---
title: Operacje żądań i odpowiedzi w ASP.NET Core
author: jkotalik
description: Dowiedz się, jak odczytać treść żądania i napisać treść odpowiedzi w ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: e992401da2d194b178afbe51a293d103def0f940
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238155"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="8c38e-103">Operacje żądań i odpowiedzi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c38e-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="8c38e-104">Autor [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="8c38e-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="8c38e-105">W tym artykule wyjaśniono sposób odczytywania treści żądania i zapisywania jej w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8c38e-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="8c38e-106">Kod dla tych operacji może być wymagany podczas pisania oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="8c38e-106">Code for these operations might be required when writing middleware.</span></span> <span data-ttu-id="8c38e-107">Poza pisaniem oprogramowania pośredniczącego kod niestandardowy nie jest zazwyczaj wymagany, ponieważ operacje są obsługiwane przez MVC i Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8c38e-107">Outside of writing middleware, custom code isn't generally required because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="8c38e-108">Istnieją dwa abstrakcje treści żądania i odpowiedzi: <xref:System.IO.Stream> i. <xref:System.IO.Pipelines.Pipe></span><span class="sxs-lookup"><span data-stu-id="8c38e-108">There are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="8c38e-109">W przypadku odczytu żądania żądanie [HttpRequest. Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) ma <xref:System.IO.Stream>wartość <xref:System.IO.Pipelines.PipeReader>, `HttpRequest.BodyReader` a jest.</span><span class="sxs-lookup"><span data-stu-id="8c38e-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyReader` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="8c38e-110">Na potrzeby pisania odpowiedzi [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) to `HttpResponse.BodyWriter` <xref:System.IO.Pipelines.PipeWriter>a.</span><span class="sxs-lookup"><span data-stu-id="8c38e-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a `HttpResponse.BodyWriter` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="8c38e-111">Potoki są zalecane za pośrednictwem strumieni.</span><span class="sxs-lookup"><span data-stu-id="8c38e-111">Pipelines are recommended over streams.</span></span> <span data-ttu-id="8c38e-112">Strumienie mogą być łatwiejsze w przypadku niektórych prostych operacji, ale potoki mają zalety wydajności i są łatwiejsze w większości scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="8c38e-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="8c38e-113">ASP.NET Core zaczyna używać potoków zamiast strumieni wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="8c38e-113">ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="8c38e-114">Przykłady obejmują:</span><span class="sxs-lookup"><span data-stu-id="8c38e-114">Examples include:</span></span>

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

<span data-ttu-id="8c38e-115">Strumienie nie są usuwane z struktury.</span><span class="sxs-lookup"><span data-stu-id="8c38e-115">Streams aren't being removed from the framework.</span></span> <span data-ttu-id="8c38e-116">Strumienie są nadal używane w środowisku .NET, a wiele typów strumieni nie ma odpowiedników potoku, `FileStreams` takich `ResponseCompression`jak i.</span><span class="sxs-lookup"><span data-stu-id="8c38e-116">Streams continue to be used throughout .NET, and many stream types don't have pipe equivalents, such as `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="8c38e-117">Przykłady przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="8c38e-117">Stream examples</span></span>

<span data-ttu-id="8c38e-118">Załóżmy, że celem jest utworzenie oprogramowania pośredniczącego, które odczytuje całą treść żądania jako listę ciągów, dzieląc je na nowe wiersze.</span><span class="sxs-lookup"><span data-stu-id="8c38e-118">Suppose the goal is to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="8c38e-119">Prosta implementacja strumienia może wyglądać podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="8c38e-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="8c38e-120">Ten kod działa, ale wystąpiły pewne problemy:</span><span class="sxs-lookup"><span data-stu-id="8c38e-120">This code works, but there are some issues:</span></span>

* <span data-ttu-id="8c38e-121">Przed dołączeniem do `StringBuilder`, przykład tworzy kolejny ciąg (`encodedString`), który jest wyrzucany natychmiast.</span><span class="sxs-lookup"><span data-stu-id="8c38e-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="8c38e-122">Ten proces występuje dla wszystkich bajtów w strumieniu, więc wynikiem jest dodatkowa alokacja pamięci o rozmiarze całej treści żądania.</span><span class="sxs-lookup"><span data-stu-id="8c38e-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
* <span data-ttu-id="8c38e-123">Przykład odczytuje cały ciąg przed podziałem w nowych wierszach.</span><span class="sxs-lookup"><span data-stu-id="8c38e-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="8c38e-124">Aby sprawdzić, czy nowe wiersze w tablicy bajtów są bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="8c38e-124">It's more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="8c38e-125">Oto przykład, który rozwiązuje niektóre z powyższych problemów:</span><span class="sxs-lookup"><span data-stu-id="8c38e-125">Here's an example that fixes some of the preceding issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="8c38e-126">Ten poprzedni przykład:</span><span class="sxs-lookup"><span data-stu-id="8c38e-126">This preceding example:</span></span>

* <span data-ttu-id="8c38e-127">Nie buforuje całej treści żądania w a `StringBuilder` , chyba że nie występują żadne znaki nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="8c38e-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
* <span data-ttu-id="8c38e-128">Nie wywołuje `Split` ciągu.</span><span class="sxs-lookup"><span data-stu-id="8c38e-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="8c38e-129">Jednak nadal występują pewne problemy:</span><span class="sxs-lookup"><span data-stu-id="8c38e-129">However, there are still are a few issues:</span></span>

* <span data-ttu-id="8c38e-130">Jeśli znaki nowego wiersza są rozrzedzone, większość treści żądania jest buforowana w ciągu.</span><span class="sxs-lookup"><span data-stu-id="8c38e-130">If newline characters are sparse, much of the request body is buffered in the string.</span></span>
* <span data-ttu-id="8c38e-131">Kod nadal tworzy ciągi (`remainingString`) i dodaje je do buforu ciągów, co skutkuje dodatkową alokacją.</span><span class="sxs-lookup"><span data-stu-id="8c38e-131">The code continues to create strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="8c38e-132">Te problemy są fixable, ale kod staje się coraz bardziej skomplikowany przy niewielkim ulepszaniu.</span><span class="sxs-lookup"><span data-stu-id="8c38e-132">These issues are fixable, but the code is becoming progressively more complicated with little improvement.</span></span> <span data-ttu-id="8c38e-133">Potoki umożliwiają rozwiązanie tych problemów przy minimalnej złożoności kodu.</span><span class="sxs-lookup"><span data-stu-id="8c38e-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="8c38e-134">Potoki</span><span class="sxs-lookup"><span data-stu-id="8c38e-134">Pipelines</span></span>

<span data-ttu-id="8c38e-135">Poniższy przykład pokazuje, jak można obsłużyć ten sam scenariusz przy `PipeReader`użyciu:</span><span class="sxs-lookup"><span data-stu-id="8c38e-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="8c38e-136">Ten przykład naprawia wiele problemów, które zostały wdrożone przez implementacje strumieni:</span><span class="sxs-lookup"><span data-stu-id="8c38e-136">This example fixes many issues that the streams implementations had:</span></span>

* <span data-ttu-id="8c38e-137">Nie ma potrzeby używania bufora ciągów, ponieważ `PipeReader` obsługuje on bajty, które nie były używane.</span><span class="sxs-lookup"><span data-stu-id="8c38e-137">There's no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
* <span data-ttu-id="8c38e-138">Zakodowane ciągi są bezpośrednio dodawane do listy zwracanych ciągów.</span><span class="sxs-lookup"><span data-stu-id="8c38e-138">Encoded strings are directly added to the list of returned strings.</span></span>
* <span data-ttu-id="8c38e-139">Tworzenie ciągów jest wolne od alokacji poza pamięcią używaną przez ciąg (z wyjątkiem `ToArray()` wywołania).</span><span class="sxs-lookup"><span data-stu-id="8c38e-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="8c38e-140">Zainstalowanych</span><span class="sxs-lookup"><span data-stu-id="8c38e-140">Adapters</span></span>

<span data-ttu-id="8c38e-141">Obie `Body` właściwości `BodyReader/BodyWriter` i są dostępne dla `HttpRequest` i `HttpResponse`.</span><span class="sxs-lookup"><span data-stu-id="8c38e-141">Both `Body` and `BodyReader/BodyWriter` properties are available for `HttpRequest` and `HttpResponse`.</span></span> <span data-ttu-id="8c38e-142">Po ustawieniu `Body` innego strumienia nowy zestaw kart automatycznie dostosowuje każdy typ do drugiego.</span><span class="sxs-lookup"><span data-stu-id="8c38e-142">When you set `Body` to a different stream, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="8c38e-143">Jeśli ustawisz `HttpRequest.Body` nowy strumień, `HttpRequest.BodyReader` zostanie automatycznie ustawiony na nowe `PipeReader` Zawijanie `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="8c38e-143">If you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyReader` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span>

## <a name="startasync"></a><span data-ttu-id="8c38e-144">StartAsync</span><span class="sxs-lookup"><span data-stu-id="8c38e-144">StartAsync</span></span>

<span data-ttu-id="8c38e-145">`HttpResponse.StartAsync`służy do wskazywania, że nagłówki są niemodyfikowalne i `OnStarting` aby można było uruchamiać wywołania zwrotne.</span><span class="sxs-lookup"><span data-stu-id="8c38e-145">`HttpResponse.StartAsync` is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="8c38e-146">Podczas używania Kestrel jako serwera, `StartAsync` wywołując przed `PipeReader` użyciem gwarancji, że pamięć zwrócona przez `GetMemory` należy do wewnętrznego <xref:System.IO.Pipelines.Pipe> , a nie do buforu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="8c38e-146">When using Kestrel as a server, calling `StartAsync` before using the `PipeReader` guarantees that memory returned by `GetMemory` belongs to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c38e-147">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8c38e-147">Additional resources</span></span>

* [<span data-ttu-id="8c38e-148">Introducing System.IO.Pipelines</span><span class="sxs-lookup"><span data-stu-id="8c38e-148">Introducing System.IO.Pipelines</span></span>](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>
