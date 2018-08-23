---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Część 2: Tworzenie modeli domeny | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: cb98f42df411a7ba12ff4566c30ddfbf253253d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753615"
---
<a name="part-2-creating-the-domain-models"></a>Część 2: Tworzenie modeli domeny
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Dodawanie modeli

Istnieją trzy sposoby podejścia platformy Entity Framework:

- Bazy danych: Uruchom z bazą danych i Entity Framework generuje kod.
- Pierwszy model: Uruchom z modelem visual i Entity Framework generuje bazy danych i kod.
- Najpierw kod: Uruchom z kodem i Entity Framework generuje bazy danych.

Używamy podejście najpierw kod, dzięki czemu możemy rozpocząć od zdefiniowania naszych obiektów domeny jako POCOs (stary zwykły obiektów CLR). W przypadku najpierw kod metody obiektów domeny nie ma potrzeby wprowadzania dodatkowego kodu do obsługi warstwy bazy danych, takich jak transakcje lub trwałość. (W szczególności nie musi dziedziczyć z [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) klasy.) Korzystanie z adnotacji danych, można nadal kontrolować, jak Entity Framework tworzy schemat bazy danych.

Ponieważ POCOs nie mają żadnych dodatkowych właściwości, które opisują [bazy danych stanu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), łatwo może być serializowany do formatu JSON lub XML. Jednakże, nie znaczy, że zawsze należy udostępnić swoje modele platformy Entity Framework bezpośrednio do klientów, jak opisano w dalszej części tego samouczka.

Utworzymy POCOs następujące:

- Produkt
- Kolejność
- OrderDetail

Aby utworzyć każdej klasy, kliknij prawym przyciskiem myszy folder modeli w Eksploratorze rozwiązań. Z menu kontekstowego wybierz **Dodaj** , a następnie wybierz **klasy.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Dodaj `Product` klasy następującą implementacją:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Zgodnie z Konwencją, używa platformy Entity Framework `Id` właściwość jako klucz podstawowy i jego mapuje kolumną tożsamości w tabeli bazy danych. Podczas tworzenia nowego `Product` wystąpienia nie ustawisz wartość `Id`, ponieważ baza danych generuje wartość.

**ScaffoldColumn** atrybut poinformuje platformę ASP.NET MVC, aby pominąć `Id` właściwości podczas generowania formie edytora. **Wymagane** atrybut jest używany do sprawdzania poprawności modelu. Określa, że `Name` właściwość musi być niepustym ciągiem.

Dodaj `Order` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Dodaj `OrderDetail` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relacje klucza obcego

Zamówienie zawiera wiele szczegółów zamówienia, a każdy szczegół kolejności odwołuje się do jednego produktu. Do reprezentowania te relacje `OrderDetail` klasy definiuje właściwości o nazwie `OrderId` i `ProductId`. Entity Framework wywnioskuje te właściwości reprezentują kluczy obcych i doda ograniczenia foreign key z bazą danych.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order` i `OrderDetail` klasy również zawierać właściwości "navigation", które zawierają odwołania do powiązanych obiektów. Biorąc pod uwagę zamówienie, można przejść do produktów w kolejności, postępując zgodnie z właściwości nawigacji.

Skompiluj teraz projekt. Entity Framework używa odbicia, aby odnaleźć właściwości te modele, dzięki czemu wymaga skompilowanego zestawu w celu utworzenia schematu bazy danych.

## <a name="configure-the-media-type-formatters"></a>Konfiguruj programy formatujące typy nośnika

A [program formatujący typ nośnika](../../formats-and-model-binding/media-formatters.md) jest obiektem, który serializuje dane, gdy interfejs API sieci Web zapisuje treści odpowiedzi HTTP. Wbudowane elementy formatujące obsługuje dane wyjściowe JSON i XML. Domyślnie oba te elementy formatujące serializować wszystkich obiektów według wartości.

Serializacja według wartości tworzy problem, jeśli wykresu obiektu zawiera odwołania cykliczne. Dokładnie tak, za pomocą `Order` i `OrderDetail` klasy, ponieważ każdy zawiera odwołanie do drugiego. Element formatujący wykonaj odwołań, zapisywanie każdy obiekt przez wartość i go w programie koła. W związku z tym należy zmienić zachowanie domyślne.

W Eksploratorze rozwiązań rozwiń aplikacji\_Uruchom folder i Otwórz plik o nazwie WebApiConfig.cs. Dodaj następujący kod do `WebApiConfig` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Ten kod ustawia element formatujący JSON, aby zachować odwołania do obiektu i całkowicie usunie element formatujący XML z potoku. (Można skonfigurować program formatujący kod XML, aby zachować odwołania do obiektu, ale jest nieco więcej pracy, a potrzebujemy tylko JSON dla tej aplikacji. Aby uzyskać więcej informacji, zobacz [obsługi odwołań cyklicznych obiektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-1.md)
> [dalej](using-web-api-with-entity-framework-part-3.md)
