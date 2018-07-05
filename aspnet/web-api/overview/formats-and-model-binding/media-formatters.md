---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Programy formatujące multimedia we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 824fea5a8837ff8b09af832b7d2a094c7d82907b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368919"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Programy formatujące multimedia we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym samouczku pokazano, jak obsługiwać dodatkowe formaty w Web API platformy ASP.NET.

## <a name="internet-media-types"></a>Typy nośników Internet

Typ nośnika, nazywany również typ MIME, identyfikuje format elementu danych. W protokole HTTP typów nośników opisujący format treści wiadomości. Typ nośnika składa się z dwóch ciągów, typem i podtypem. Na przykład:

- text/html
- image/png
- Application/json

Gdy wiadomość HTTP zawiera treść jednostki, nagłówek Content-Type określa format treści wiadomości. Informuje odbiorcę jak przeanalizować zawartość treści wiadomości.

Na przykład jeśli odpowiedź HTTP zawiera obraz PNG, odpowiedź może mieć następujące nagłówki.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Gdy klient wysyła komunikat żądania, może obejmować nagłówek Accept. Nagłówek Accept informuje, że serwer multimedialnych typ(-y) klient chce z serwera. Na przykład:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Ten nagłówek informuje serwer, że klient chce HTML, XHTML lub XML.

Typ nośnika określa, jak interfejs API sieci Web serializuje i deserializuje treść komunikatu HTTP. Interfejs API sieci Web ma wbudowaną obsługę XML, JSON, BSON i dane zakodowane i może obsługiwać dodatkowe pliki multimedialne, pisząc *nośnika elementu formatującego*.

Aby utworzyć element formatujący nośnik, pochodzi z jednej z tych klas:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Odczyt asynchroniczny używa klasy i metody zapisu.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Ta klasa jest pochodną **element MediaTypeFormatter** , ale używa sychronous odczytu/zapisu metod.

Wyprowadzanie z **BufferedMediaTypeFormatter** jest prostsze, ponieważ brak kodu asynchronicznego, ale oznacza to również, zablokować wątek wywołujący podczas operacji We/Wy.

## <a name="example-creating-a-csv-media-formatter"></a>Przykład: Tworzenie nośnika elementu formatującego CSV

Poniższy przykład pokazuje element formatujący typu nośnika, który może wykonać serializację obiektu produktu w formacie wartości rozdzielanych przecinkami (CSV). W tym przykładzie użyto typu produktu, zdefiniowane w tym samouczku [Tworzenie internetowego interfejsu API tej operacji CRUD obsługuje](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Oto definicja obiektu produktu:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Aby zaimplementować elementu formatującego CSV, Definiowanie klasy, która pochodzi od klasy **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

W Konstruktorze należy dodać do typów nośników, które obsługuje program formatujący. W tym przykładzie element formatujący obsługuje typ jednym nośniku, &quot;pliku tekstowego lub csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Zastąp **CanWriteType** metodę w celu wskazania typów element formatujący może serializować:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

W tym przykładzie element formatujący może serializować pojedynczego `Product` obiektów oraz kolekcjami `Product` obiektów.

Podobnie, Zastąp **CanReadType** metodę w celu wskazania typów element formatujący może wykonywać deserializację. W tym przykładzie element formatujący nie obsługuje deserializacji, więc metoda po prostu zwraca **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Na koniec Zastąp **WriteToStream** metody. Ta metoda wykonuje serializację typu zapisując go w strumieniu. Jeśli formatujący deserializacji także Przesłoń **ReadFromStream** metody.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Dodawanie nośnika elementu formatującego do potoku składnika Web API

Aby dodać typ nośnika elementu formatującego do potok składnika Web API, użyj **elementy formatujące** właściwość **HttpConfiguration** obiektu.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Kodowanie znaków

Opcjonalnie element formatujący nośnik może obsługiwać wiele kodowania znaków, takich jak UTF-8 lub ISO 8859-1.

W konstruktorze, należy dodać co najmniej jedno [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) typy do **SupportedEncodings** kolekcji. Umieść domyślne kodowanie pierwszy.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

W **WriteToStream** i **ReadFromStream** wywołania metody, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) wybrać kodowanie znaków preferowaną. Ta metoda jest zgodna z nagłówków żądań z listą obsługiwanych kodowań. Użyj zwracanego **kodowanie** podczas odczytu lub zapisu ze strumienia:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
