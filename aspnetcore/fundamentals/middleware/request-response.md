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
# <a name="request-and-response-operations-in-aspnet-core"></a>Operacje żądań i odpowiedzi w ASP.NET Core

Autor [Justin Kotalik](https://github.com/jkotalik)

W tym artykule wyjaśniono sposób odczytywania treści żądania i zapisywania jej w treści odpowiedzi. Kod dla tych operacji może być wymagany podczas pisania oprogramowania pośredniczącego. Poza pisaniem oprogramowania pośredniczącego kod niestandardowy nie jest zazwyczaj wymagany, ponieważ operacje są obsługiwane przez MVC i Razor Pages.

Istnieją dwa abstrakcje treści żądania i odpowiedzi: <xref:System.IO.Stream> i <xref:System.IO.Pipelines.Pipe>. W przypadku odczytu żądania żądanie [HttpRequest. Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) jest <xref:System.IO.Stream>, a `HttpRequest.BodyReader` jest <xref:System.IO.Pipelines.PipeReader>em. Na potrzeby pisania odpowiedzi [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) jest <xref:System.IO.Stream>, a `HttpResponse.BodyWriter` jest <xref:System.IO.Pipelines.PipeWriter>em.

[Potoki](/dotnet/standard/io/pipelines) są zalecane za pośrednictwem strumieni. Strumienie mogą być łatwiejsze w przypadku niektórych prostych operacji, ale potoki mają zalety wydajności i są łatwiejsze w większości scenariuszy. ASP.NET Core zaczyna używać potoków zamiast strumieni wewnętrznie. Przykłady:

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

Strumienie nie są usuwane z struktury. Strumienie są nadal używane w środowisku .NET, a wiele typów strumieni nie ma odpowiedników potoku, takich jak `FileStreams` i `ResponseCompression`.

## <a name="stream-examples"></a>Przykłady przesyłania strumieniowego

Załóżmy, że celem jest utworzenie oprogramowania pośredniczącego, które odczytuje całą treść żądania jako listę ciągów, dzieląc je na nowe wiersze. Prosta implementacja strumienia może wyglądać podobnie do poniższego przykładu:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

Ten kod działa, ale wystąpiły pewne problemy:

* Przed dołączeniem do `StringBuilder`, w przykładzie tworzony jest kolejny ciąg (`encodedString`), który zostanie natychmiast wygenerowany. Ten proces występuje dla wszystkich bajtów w strumieniu, więc wynikiem jest dodatkowa alokacja pamięci o rozmiarze całej treści żądania.
* Przykład odczytuje cały ciąg przed podziałem w nowych wierszach. Aby sprawdzić, czy nowe wiersze w tablicy bajtów są bardziej wydajne.

Oto przykład, który rozwiązuje niektóre z powyższych problemów:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

Ten poprzedni przykład:

* Nie buforuje całej treści żądania w `StringBuilder`, chyba że nie ma żadnych znaków nowego wiersza.
* Nie wywołuje `Split` w ciągu.

Jednak nadal występują pewne problemy:

* Jeśli znaki nowego wiersza są rozrzedzone, większość treści żądania jest buforowana w ciągu.
* Kod nadal tworzy ciągi (`remainingString`) i dodaje je do buforu ciągów, co skutkuje dodatkową alokacją.

Te problemy są fixable, ale kod staje się coraz bardziej skomplikowany przy niewielkim ulepszaniu. Potoki umożliwiają rozwiązanie tych problemów przy minimalnej złożoności kodu.

## <a name="pipelines"></a>Potoki

Poniższy przykład pokazuje, jak można obsłużyć ten sam scenariusz przy użyciu `PipeReader`:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

Ten przykład naprawia wiele problemów, które zostały wdrożone przez implementacje strumieni:

* Bufor ciągu nie jest potrzebny, ponieważ `PipeReader` obsługuje bajty, które nie zostały użyte.
* Zakodowane ciągi są bezpośrednio dodawane do listy zwracanych ciągów.
* Tworzenie ciągów jest wolne od alokacji poza pamięcią używaną przez ciąg (z wyjątkiem wywołania `ToArray()`).

## <a name="adapters"></a>Zainstalowanych

Zarówno `Body`, jak i właściwości `BodyReader/BodyWriter` są dostępne dla `HttpRequest` i `HttpResponse`. Po ustawieniu `Body` na inny strumień, nowy zestaw kart automatycznie dostosowuje każdy typ do drugiego. Jeśli ustawisz `HttpRequest.Body` na nowy strumień, `HttpRequest.BodyReader` zostanie automatycznie ustawiona na nowy `PipeReader` zawijający `HttpRequest.Body`.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync` służy do wskazywania, że nagłówki są niemodyfikowalne i aby można było uruchamiać wywołania zwrotne `OnStarting`. Gdy jest używany Kestrel jako serwer, wywoływanie `StartAsync` przed użyciem `PipeReader` gwarantuje, że pamięć zwracana przez `GetMemory` należy do wewnętrznego <xref:System.IO.Pipelines.Pipe>, a nie do buforu zewnętrznego.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do System. IO. potoków](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>
