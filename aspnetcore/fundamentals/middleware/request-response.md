---
title: Operacje żądań i odpowiedzi w programie ASP.NET Core
author: jkotalik
description: Dowiedz się, jak treści żądania odczytu i zapisu treści odpowiedzi w programie ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jkotalik
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: f29c0aff95887d639cf3d1a8c8fe9f9228159f7a
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400991"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>Operacje żądań i odpowiedzi w programie ASP.NET Core

Przez [Justin Kotalik](https://github.com/jkotalik)

W tym artykule wyjaśniono, jak czytać z treści żądania i zapisywać w treści odpowiedzi. Może być konieczne pisanie kodu dla tych operacji, podczas pisania miej oprogramowania pośredniczącego. W przeciwnym razie zwykle nie trzeba napisać ten kod, ponieważ operacje są obsługiwane przez MVC i stron Razor.

W programie ASP.NET Core 3.0 są dwa elementy abstrakcji do treści żądania i odpowiedzi: <xref:System.IO.Stream> i <xref:System.IO.Pipelines.Pipe>. Żądanie czytelności [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) jest <xref:System.IO.Stream>, i `HttpRequest.BodyPipe` jest <xref:System.IO.Pipelines.PipeReader>. Do zapisu odpowiedzi [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) jest `HttpResponse.BodyPipe` jest <xref:System.IO.Pipelines.PipeWriter>.

Firma Microsoft zaleca potoki za pośrednictwem strumieni. Strumienie może być łatwiejszy w obsłudze dla niektórych prostych operacji, ale potoki mają korzystać wydajność i są łatwiejsze w obsłudze w większości scenariuszy. 3.0 ASP.NET Core zaczyna używać potoków zamiast strumieni wewnętrznie. Przykłady obejmują:

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

Strumienie nie będą natychmiast. Mogą być nadal używane w całej platformy .NET i wiele typów strumienia nie ma odpowiedników potoku, takie jak `FileStreams` i `ResponseCompression`.

## <a name="stream-examples"></a>Przykłady Stream

Załóżmy, że chcesz utworzyć oprogramowania pośredniczącego, które odczytuje całej treści żądania jako listę ciągów, dzielenie w nowych wierszach. Wykonania prostego strumień może wyglądać następująco:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

Ten kod działa, ale występują pewne problemy:

- Przed dołączeniem do `StringBuilder`, w przykładzie jest tworzony inny ciąg (`encodedString`), jest wyrzucać natychmiast. Ten proces odbywa się dla wszystkich bajtów w strumieniu, więc dodatkową pamięć przydział rozmiaru całej treści żądania.
- Przykład odczytuje cały ciąg przed podziałem w nowych wierszach. Byłoby bardziej wydajne, aby sprawdzić, czy są dostępne nowe wiersze w tablicy bajtów.

Oto przykład, w którym niektóre z tych problemów poprawki:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

W tym przykładzie:

- Nie buforuje całej treści żądania w `StringBuilder` chyba, że nie ma żadnych znaków nowego wiersza.
- Nie wywołuje `Split` na ciąg.

Istnieją jednak są nadal kilka kwestii:

- W przypadku rozrzedzone znakami znaczną część treści żądania są buforowane w ciągu.
- Nadal tworzy ciągów (`remainingString`) i dodaje je do buforu ciągu, co skutkuje dodatkowy przydział.

Te problemy to naprawić, ale kod jest staje się coraz bardziej skomplikowane niewielkie ulepszenia. Potoki zapewniają sposób, aby rozwiązać te problemy za pomocą minimalnej ilości kodu, złożoności.

## <a name="pipelines"></a>Potoki

W poniższym przykładzie pokazano, jak ten sam scenariusz może być obsługiwane za pomocą `PipeReader`:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

W tym przykładzie rozwiązuje wiele problemów, które miały implementacje strumieni:

- Nie jest konieczne dla buforu ciągu, ponieważ `PipeReader` obsługuje bajtów, które nie były używane.
- Parametry zakodowanej bezpośrednio są dodawane do listy ciągów zwrócone.
- Tworzenie ciągu jest bezpłatne alokacji oprócz pamięci używanej przez ciąg (z wyjątkiem `ToArray()` wywołania).

## <a name="adapters"></a>Karty

Skoro zarówno `Body` i `BodyPipe` właściwości są dostępne dla `HttpRequest` i `HttpResponse`, co się stanie po ustawieniu `Body` do różnych strumienia? 3.0 nowy zbiór kart automatycznie Adaptowane każdego typu na drugi. Na przykład jeśli ustawisz `HttpRequest.Body` na nowy strumień `HttpRequest.BodyPipe` automatycznie zostaje ustawiony nowy `PipeReader` to opakowuje `HttpRequest.Body`. Takie samo zachowanie dotyczy ustawienie `BodyPipe` właściwości. Jeśli `HttpResponse.BodyPipe` ustawiono nową `PipeWriter`, `HttpResponse.Body` automatycznie zostaje ustawiony nowy strumień, który otacza `HttpResponse.BodyPipe`.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync` jest nowa w 3.0. Służy do wskazania, że nagłówki są niemodyfikowalnych i uruchamiania `OnStarting` wywołań zwrotnych. W 3.0 preview3, należy wywołać `StartAsync` przed użyciem `HttpRequest.BodyPipe`, i w przyszłych wersjach będą zalecenia. Korzystając z Kestrel jako serwera, wywoływanie StartAsync przed rozpoczęciem korzystania z `PipeReader` gwarantuje, że pamięć zwrócony przez `GetMemory` będą należeć do firmy Kestrel wewnętrzny <xref:System.IO.Pipelines.Pipe> zamiast zewnętrznych buforu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>