---
title: Operacje żądań i odpowiedzi w ASP.NET Core
author: jkotalik
description: Dowiedz się, jak odczytać treść żądania i napisać treść odpowiedzi w ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: 5e531c0ce0ed48097054fd81ddc3655a66cc7c5f
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081674"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>Operacje żądań i odpowiedzi w ASP.NET Core

Autor [Justin Kotalik](https://github.com/jkotalik)

W tym artykule wyjaśniono sposób odczytywania treści żądania i zapisywania jej w treści odpowiedzi. Kod dla tych operacji może być wymagany podczas pisania oprogramowania pośredniczącego. Poza pisaniem oprogramowania pośredniczącego kod niestandardowy nie jest zazwyczaj wymagany, ponieważ operacje są obsługiwane przez MVC i Razor Pages.

Istnieją dwa abstrakcje treści żądania i odpowiedzi: <xref:System.IO.Stream> i. <xref:System.IO.Pipelines.Pipe> W przypadku odczytu żądania żądanie [HttpRequest. Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) ma <xref:System.IO.Stream>wartość <xref:System.IO.Pipelines.PipeReader>, `HttpRequest.BodyReader` a jest. Do pisania odpowiedzi [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) jest <xref:System.IO.Stream>i `HttpResponse.BodyWriter` <xref:System.IO.Pipelines.PipeWriter>.

Potoki są zalecane za pośrednictwem strumieni. Strumienie mogą być łatwiejsze w przypadku niektórych prostych operacji, ale potoki mają zalety wydajności i są łatwiejsze w większości scenariuszy. ASP.NET Core zaczyna używać potoków zamiast strumieni wewnętrznie. Przykłady obejmują:

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

Strumienie nie są usuwane z struktury. Strumienie są nadal używane w środowisku .NET, a wiele typów strumieni nie ma odpowiedników potoku, `FileStreams` takich `ResponseCompression`jak i.

## <a name="stream-examples"></a>Przykłady przesyłania strumieniowego

Załóżmy, że celem jest utworzenie oprogramowania pośredniczącego, które odczytuje całą treść żądania jako listę ciągów, dzieląc je na nowe wiersze. Prosta implementacja strumienia może wyglądać podobnie do poniższego przykładu:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

Ten kod działa, ale wystąpiły pewne problemy:

* Przed dołączeniem do `StringBuilder`, przykład tworzy kolejny ciąg (`encodedString`), który jest wyrzucany natychmiast. Ten proces występuje dla wszystkich bajtów w strumieniu, więc wynikiem jest dodatkowa alokacja pamięci o rozmiarze całej treści żądania.
* Przykład odczytuje cały ciąg przed podziałem w nowych wierszach. Aby sprawdzić, czy nowe wiersze w tablicy bajtów są bardziej wydajne.

Oto przykład, który rozwiązuje niektóre z powyższych problemów:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

Ten poprzedni przykład:

* Nie buforuje całej treści żądania w a `StringBuilder` , chyba że nie występują żadne znaki nowego wiersza.
* Nie wywołuje `Split` ciągu.

Jednak nadal występują pewne problemy:

* Jeśli znaki nowego wiersza są rozrzedzone, większość treści żądania jest buforowana w ciągu.
* Kod nadal tworzy ciągi (`remainingString`) i dodaje je do buforu ciągów, co skutkuje dodatkową alokacją.

Te problemy są fixable, ale kod staje się coraz bardziej skomplikowany przy niewielkim ulepszaniu. Potoki umożliwiają rozwiązanie tych problemów przy minimalnej złożoności kodu.

## <a name="pipelines"></a>potoki

Poniższy przykład pokazuje, jak można obsłużyć ten sam scenariusz przy `PipeReader`użyciu:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

Ten przykład naprawia wiele problemów, które zostały wdrożone przez implementacje strumieni:

* Nie ma potrzeby używania bufora ciągów, ponieważ `PipeReader` obsługuje on bajty, które nie były używane.
* Zakodowane ciągi są bezpośrednio dodawane do listy zwracanych ciągów.
* Tworzenie ciągów jest wolne od alokacji poza pamięcią używaną przez ciąg (z wyjątkiem `ToArray()` wywołania).

## <a name="adapters"></a>Zainstalowanych

Obie `Body` właściwości `BodyReader/BodyWriter` i są dostępne dla `HttpRequest` i `HttpResponse`. Po ustawieniu `Body` innego strumienia nowy zestaw kart automatycznie dostosowuje każdy typ do drugiego. Jeśli ustawisz `HttpRequest.Body` nowy strumień, `HttpRequest.BodyReader` zostanie automatycznie ustawiony na nowe `PipeReader` Zawijanie `HttpRequest.Body`.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync`służy do wskazywania, że nagłówki są niemodyfikowalne i `OnStarting` aby można było uruchamiać wywołania zwrotne. Podczas używania Kestrel jako serwera, `StartAsync` wywołując przed `PipeReader` użyciem gwarancji, że pamięć zwrócona przez `GetMemory` należy do wewnętrznego <xref:System.IO.Pipelines.Pipe> , a nie do buforu zewnętrznego.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>
