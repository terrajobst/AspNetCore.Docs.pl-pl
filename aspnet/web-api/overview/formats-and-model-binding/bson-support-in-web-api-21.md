---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Obsługa formatu BSON w składniku ASP.NET Web API 2.1 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
ms.locfileid: "27984586"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Obsługa formatu BSON w składniku ASP.NET Web API 2.1
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Składnik Web API 2.1 wprowadzono obsługę formatu BSON. W tym temacie pokazano, jak używać formatu BSON w kontrolerze interfejsu API sieci Web (po stronie serwera) i w aplikacji klienta .NET.

## <a name="what-is-bson"></a>Co to jest BSON?

[BSON](http://bsonspec.org/) to format serializacji binarnej. "BSON" oznacza "Binary JSON", ale są bardzo inaczej serializowane formatu BSON i JSON. Jest BSON "JSON przypominającej", ponieważ obiekty są reprezentowane jako pary nazwa wartość, podobny do formatu JSON. W przeciwieństwie do formatu JSON numeryczne typy danych są przechowywane w postaci bajtów, nie ciągów

BSON został opracowany jako lekkie, łatwo przeglądać i szybkie do kodowania dekodowania.

- BSON jest porównywalna o rozmiarze do formatu JSON. W zależności od danych ładunku BSON może być mniejsza lub większa od ładunek JSON. Dla serializacji danych binarnych, takich jak plik obrazu BSON jest mniejszy niż JSON, ponieważ dane binarne nie jest kodowany w formacie base64.
- Dokumenty BSON są łatwe do skanowania, ponieważ elementy są poprzedzane prefiksem długość pola, więc analizatorem przejść elementów bez ich dekodowania.
- Kodowania i dekodowania są wydajne, ponieważ numeryczne typy danych są przechowywane w postaci liczb i ciągów nie.

Natywny klientów, takich jak aplikacje klienta .NET, mogą korzystać z formatu BSON zamiast formatów tekstowym, takich jak JSON i XML. Dla klientów w przeglądarkach prawdopodobnie można przestrzegaj JSON, ponieważ usługa języka JavaScript bezpośrednio można przekonwertować ładunek JSON.

Na szczęście używa interfejsu API sieci Web [negocjowanie zawartości](content-negotiation.md), więc interfejsu API można obsługuje zarówno formaty i umożliwić klientowi wybierz.

## <a name="enabling-bson-on-the-server"></a>Włączanie BSON na serwerze

W konfiguracji interfejsu API sieci Web, Dodaj **BsonMediaTypeFormatter** do kolekcji elementów formatujących.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Teraz Jeśli klient żąda "application/bson", interfejsu API sieci Web zostanie użyty element formatujący formatu BSON.

Aby skojarzyć BSON z innych typów nośników, należy je dodać do kolekcji SupportedMediaTypes. Poniższy kod dodaje "application/vnd.contoso" do typów nośników obsługiwanych:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Przykład sesji HTTP

W tym przykładzie użyjemy następującej klasy modelu oraz proste kontrolera interfejsu API sieci Web:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Klient może wysyłać następujące żądania HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Poniżej przedstawiono odpowiedzi:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

W tym miejscu zostały zastąpione danych binarnych z &quot;.&quot; znaków. Zrzut ekranu następujące z pokazuje Fiddler nieprzetworzonej wartości szesnastkowych.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Przy użyciu formatu BSON z HttpClient

Aplikacje klientów .NET mogą używać element formatujący BSON **HttpClient**. Aby uzyskać więcej informacji na temat **HttpClient**, zobacz [wywoływania sieci Web interfejsu API z klienta programu .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Poniższy kod wysyła żądanie GET, który akceptuje BSON, a następnie deserializuje ładunku BSON w odpowiedzi.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Aby zażądać BSON z serwera, ustawić nagłówek Accept do "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Aby deserializacji treści odpowiedzi, należy użyć **BsonMediaTypeFormatter**. Ten program formatujący nie jest w domyślnej kolekcji elementów formatujących, dlatego należy określić podczas odczytu treści odpowiedzi:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Kolejnym przykładzie pokazano, jak można wysłać żądania POST, który zawiera BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Większość tego kodu jest taka sama, co w poprzednim przykładzie. Jednak **PostAsync** metody, określ **BsonMediaTypeFormatter** jako element formatujący:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializacja najwyższego poziomu typy pierwotne

Każdy dokument BSON znajduje się lista par klucz/wartość. Specyfikacja BSON nie definiuje składni dla serializacji jednego nieprzetworzonej wartości, takich jak integer lub string.

Aby obejść to ograniczenie **BsonMediaTypeFormatter** traktuje typów pierwotnych w szczególnych przypadkach. Przed rozpoczęciem serializacji, konwertuje wartość na parę klucz wartość z kluczem "Value". Na przykład załóżmy, że dany kontroler interfejsu API zwraca liczbę całkowitą:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Przed rozpoczęciem serializacji, element formatujący BSON konwertuje to z następującą parą klucza i wartości:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Podczas deserializacji, element formatujący konwertuje je do oryginalnej wartości. Jednak klientów przy użyciu różnych analizatora BSON należy obsługiwać ten przypadek, jeśli interfejs API sieci web zwraca wartości w wierszach. Ogólnie rzecz biorąc należy rozważyć zwracanie wartości w wierszach, a nie dane strukturalne.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykładowe BSON interfejsu API sieci Web](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Programy formatujące multimedia](media-formatters.md)
