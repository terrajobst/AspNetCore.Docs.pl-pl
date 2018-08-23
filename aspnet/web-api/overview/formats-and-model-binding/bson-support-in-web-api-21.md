---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Obsługa formatu BSON we wzorcu ASP.NET Web API 2.1 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 709fb0266c0725176358a1bd0d08b3e07fa6e2a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757063"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Obsługa formatu BSON we wzorcu ASP.NET Web API 2.1
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Składnik Web API 2.1 wprowadzono obsługę formatu BSON. W tym temacie pokazano, jak używać formatu BSON w kontrolerze interfejsu API sieci Web (po stronie serwera) i w aplikacji klienckiej .NET.

## <a name="what-is-bson"></a>Co to jest BSON?

[BSON](http://bsonspec.org/) to binarny format serializacji. "BSON" oznacza "Binarny JSON", ale są bardzo różny sposób serializacji formatu BSON i JSON. Jest BSON "Notacji JSON", ponieważ obiekty są reprezentowane jako pary nazwa wartość, podobny do formatu JSON. W przeciwieństwie do formatu JSON numeryczne typy danych są przechowywane w postaci bajtów, a nie ciągi

BSON został zaprojektowany tak, to lekki, łatwość skanowania oraz szybkość kodowania dekodowania.

- BSON jest porównywalny rozmiar do formatu JSON. W zależności od danych ładunek BSON może być mniejszy lub większy od ładunku JSON. Dla serializacji danych binarnych, takich jak plik obrazu BSON jest mniejszy niż JSON, dane, ponieważ dane binarne nie jest zakodowany w formacie base64.
- BSON dokumenty są łatwe do skanowania, ponieważ elementy mają prefiks pola długości, dlatego analizatorem przejść elementów bez ich dekodowania.
- Kodowania i dekodowania są wydajne, ponieważ typy danych liczbowych są przechowywane jako liczby, ciągi nie.

Natywne klientów, takich jak aplikacje klienckie .NET, mogą korzystać z formatu BSON zamiast oparte na tekście formatów, takich jak JSON lub XML. Dla klientów w przeglądarkach prawdopodobnie można działać zgodnie z formatu JSON, ponieważ usługa języka JavaScript bezpośrednio można przekonwertować ładunek JSON.

Na szczęście usługa korzysta z interfejsu API sieci Web [negocjacji zawartości](content-negotiation.md), więc może obsługiwać obu formatów i zezwala komputerom, wybierz interfejsu API.

## <a name="enabling-bson-on-the-server"></a>Włączanie BSON na serwerze

W konfiguracji interfejsu API sieci Web, Dodaj **BsonMediaTypeFormatter** do kolekcji elementów formatujących.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Teraz jeśli zażąda "application/bson" interfejsu API sieci Web zostanie użyty program formatujący formatu BSON.

Aby skojarzyć BSON z innych typów nośników, należy dodać je do kolekcji SupportedMediaTypes. Poniższy kod dodaje "application/vnd.contoso" do typów nośników obsługiwanych:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Przykład sesji HTTP

W tym przykładzie użyjemy następujące klasy modelu, a także proste kontrolera interfejsu API sieci Web:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Klient może wysłać poniższe żądanie HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Poniżej przedstawiono odpowiedzi:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

W tym miejscu został zastąpiony danymi binarnymi o &quot;.&quot; znaków. Zrzut ekranu z następujących z programu Fiddler pokazuje pierwotne wartości szesnastkowych.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Przy użyciu formatu BSON z HttpClient

Aplikacje klientów platformy .NET mogą używać formatu BSON element formatujący **HttpClient**. Aby uzyskać więcej informacji na temat **HttpClient**, zobacz [wywołania sieci Web interfejsu API z klienta programu .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Poniższy kod wysyła żądanie GET, które akceptuje formatu BSON i deserializuje ładunek BSON w odpowiedzi.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Aby zażądać BSON z serwera, ustawić nagłówek Accept do "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Do deserializacji treści odpowiedzi, należy użyć **BsonMediaTypeFormatter**. Ten program formatujący nie jest w domyślnej kolekcji elementów formatujących, więc trzeba je wprowadzić podczas odczytu treści odpowiedzi:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Następny przykład przedstawia sposób wysłania żądania POST, która zawiera formatu BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Większość tego kodu jest taka sama, jak w poprzednim przykładzie. Ale w **PostAsync** metody, określ **BsonMediaTypeFormatter** jako element formatujący:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializacja typów pierwotnych najwyższego poziomu

Każdy dokument BSON jest listą par klucz/wartość. Specyfikacja formatu BSON nie definiuje składni dla serializacji pojedynczej wartości pierwotnych, takich jak typu integer lub string.

Aby obejść to ograniczenie **BsonMediaTypeFormatter** traktuje typów pierwotnych w szczególnych przypadkach. Przed rozpoczęciem serializacji, konwertuje wartość w parę klucza i wartości z kluczem "Value". Na przykład załóżmy, że dany kontroler interfejsu API zwraca liczbę całkowitą:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Przed rozpoczęciem serializacji, element formatujący BSON konwertuje to następujące pary klucz/wartość:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Podczas deserializacji, element formatujący konwertuje je do oryginalnej wartości. Jednak jeśli internetowy interfejs API zwraca wartości w wierszach klientów przy użyciu innego analizatora BSON należy się do obsługi tej sprawy. Ogólnie rzecz biorąc należy rozważyć, zwracając ustrukturyzowanych danych, a nie wartości w wierszach.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykład formatu BSON interfejsu API sieci Web](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Programy formatujące multimedia](media-formatters.md)
