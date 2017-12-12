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
<a name="media-formatters-in-aspnet-web-api-2"></a>Programy formatujące multimedia w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Ten samouczek przedstawia sposób obsługuje dodatkowe formaty w interfejsie API sieci Web ASP.NET.

## <a name="internet-media-types"></a>Typy nośnika internetowych

Typ nośnika, nazywany również typ MIME, określa format elementu danych. W protokole HTTP typów nośników opisywania formatu treści wiadomości. Typ nośnika składa się z dwóch ciągów, typu i podtypu. Na przykład:

- tekst i html
- Obraz/png
- Application/json

Jeśli wiadomość HTTP zawiera treść jednostki, nagłówka Content-Type określa format treści wiadomości. Ta wartość informuje odbiornika jak przeanalizować zawartość treści wiadomości.

Na przykład jeśli odpowiedź HTTP zawiera obrazu PNG, odpowiedzi może mieć następujące nagłówki.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Gdy klient wysyła komunikat żądania, może obejmować nagłówek Accept. Nagłówek Accept informuje, że potrzebuje serwera multimedialnych typ(-y) klienta z serwera. Na przykład:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Ten nagłówek informuje serwer, że klient chce HTML, XHTML lub XML.

Typ nośnika określa sposób interfejsu API sieci Web serializuje i deserializuje treść komunikatu HTTP. Interfejs API sieci Web ma wbudowaną obsługę XML, JSON formatu BSON i urlencoded formularza danych i może obsługiwać dodatkowe pliki multimedialne pisząc *nośnika elementu formatującego*.

Aby utworzyć element formatujący nośnik, pochodzi z jednego z tych klas:

- [Klasa MediaTypeFormatter](https://msdn.microsoft.com/en-us/library/system.net.http.formatting.mediatypeformatter.aspx). Odczyt asynchroniczny używa klasy i metody zapisu.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/en-us/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Ta klasa pochodzi od **MediaTypeFormatter** , ale sychronous metody odczytu/zapisu.

Wyprowadzanie z **BufferedMediaTypeFormatter** jest łatwiejsze, ponieważ nie jest wykonywany kod asynchroniczne, ale oznacza to również wątek wywołujący może zablokować podczas operacji We/Wy.

## <a name="example-creating-a-csv-media-formatter"></a>Przykład: Tworzenie elementu formatującego nośnika CSV

W poniższym przykładzie pokazano element formatujący typu nośnika, który może wykonać serializację obiektu produktu w formacie wartości rozdzielanych przecinkami (CSV). W tym przykładzie użyto produktu zdefiniowanych w samouczku [Tworzenie składnika Web API ten obsługuje operacje CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). W tym miejscu znajduje się definicja metody obiektu produktu:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Aby zaimplementować elementu formatującego CSV, Definiowanie klasy, która jest pochodną **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

W Konstruktorze Dodaj do typów nośników, które obsługuje program formatujący. W tym przykładzie element formatujący obsługuje typ jednym nośniku, &quot;tekstu/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Zastąpienie **CanWriteType** metodę, aby wskazać typów element formatujący może serializować:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

W tym przykładzie element formatujący może serializować pojedynczego `Product` obiektów oraz kolekcji `Product` obiektów.

Podobnie, Zastąp **CanReadType** metodę, aby wskazać typów element formatujący może deserializować. W tym przykładzie element formatujący nie obsługuje deserializacji, dlatego metoda po prostu zwraca **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Ponadto Zastąp **WriteToStream** metody. Ta metoda wykonuje serializację typu przez zapisywanie w strumieniu. Jeśli formatujący deserializacji także zastępować **ReadFromStream** metody.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Dodawanie elementu formatującego nośnika do potoku składnika Web API

Aby dodać typ nośnika elementu formatującego do potoku interfejsu API sieci Web, użyj **elementy formatujące** właściwość **HttpConfiguration** obiektu.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Kodowanie znaków

Opcjonalnie element formatujący nośnik może obsługiwać wiele kodowanie znaków, takich jak UTF-8 lub ISO 8859-1.

W konstruktorze, należy dodać co najmniej jeden [System.Text.Encoding](https://msdn.microsoft.com/en-us/library/system.text.encoding.aspx) typów, aby **SupportedEncodings** kolekcji. Umieść domyślne kodowanie pierwszej.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

W **WriteToStream** i **ReadFromStream** wywołania metody, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/en-us/library/hh969054.aspx) Wybierz kodowanie znaków preferowany. Tej metody jest zgodny z listą obsługiwanych kodowań nagłówki żądania. Użyj zwróconego **kodowanie** podczas odczytu lub zapisu strumienia:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
