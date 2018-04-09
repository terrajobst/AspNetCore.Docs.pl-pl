---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Część 2: Tworzenie modeli domeny | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-2-creating-the-domain-models"></a>Część 2: Tworzenie modeli domeny
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Dodawanie modeli

Istnieją trzy sposoby podejście Entity Framework:

- Pierwszy bazy danych: rozpoczyna się z bazą danych oraz Entity Framework generuje kod.
- Pierwszy modelu: zaczynać visual modelu i generuje Entity Framework, bazy danych i kodu.
- Pierwszy kod: Uruchom z kodem i Entity Framework generuje bazy danych.

Użyto podejście pierwszy kod, więc Rozpoczniemy definiując naszych obiektów domeny jako POCOs (stary zwykły obiektów CLR). Podejście pierwszy kod obiektów domeny nie wymagają żadnych dodatkowych kodu do obsługi warstwy bazy danych, takich jak trwałości lub transakcji. (W szczególności nie muszą dziedziczyć [typu EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) klasy.) Można nadal używać adnotacji danych do kontrolowania sposobu Entity Framework utworzy schemat bazy danych.

Ponieważ POCOs nie zawierają żadnych dodatkowych właściwości opisujących [bazy danych stanu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), łatwo może być Zserializowany do formatu JSON i XML. Jednak, który oznacza to, że zawsze powinny ujawniać modeli Entity Framework bezpośrednio do klientów, jak zajmiemy się później w samouczku.

Utworzymy POCOs następujące:

- Produkt
- Kolejność
- OrderDetail

Do tworzenia każdej klasy, kliknij prawym przyciskiem myszy folder modeli w Eksploratorze rozwiązań. Wybierz z menu kontekstowego **Dodaj** , a następnie wybierz **klasy.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Dodaj `Product` klas z implementacji następujących:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Według Konwencji, korzysta z programu Entity Framework `Id` właściwość jako klucz podstawowy i mapuje go do kolumny tożsamości w tabeli bazy danych. Podczas tworzenia nowego `Product` wystąpienia, nie będzie ustaw wartość `Id`, ponieważ wartość generuje bazy danych.

**ScaffoldColumn** atrybutu poinformuje platformę ASP.NET MVC, aby pominąć `Id` właściwości podczas generowania formularza edytora. **Wymagane** atrybut służy do sprawdzania poprawności modelu. Określa, że `Name` właściwość musi być niepustym ciągiem.

Dodaj `Order` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Dodaj `OrderDetail` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relacje klucza obcego

Kolejność zawiera wiele szczegółów zamówienia, a każdy szczegółami zamówienia odwołuje się do pojedynczego produktu. Do reprezentowania tych relacji `OrderDetail` klasy definiuje właściwości o nazwie `OrderId` i `ProductId`. Entity Framework wywnioskuje te właściwości reprezentują kluczy obcych i doda ograniczenia klucza obcego do bazy danych.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order` i `OrderDetail` klasy również zawierać właściwości "nawigacji", które zawierają odwołania do powiązanych obiektów. Podana kolejności, można przejść do produktów w kolejności, wykonując właściwości nawigacji.

Teraz skompilować projekt. Entity Framework używa odbicia do odnajdywania właściwości modeli, dzięki czemu wymaga skompilowanym zestawie utworzyć schemat bazy danych.

## <a name="configure-the-media-type-formatters"></a>Konfiguruj programy formatujące typy nośnika

A [program formatujący typ nośnika](../../formats-and-model-binding/media-formatters.md) to obiekt, który serializuje dane, gdy interfejs API sieci Web zapisuje treści odpowiedzi HTTP. Wbudowane elementy formatujące obsługuje dane wyjściowe JSON i XML. Domyślnie wszystkie obiekty oba te elementy formatujące serializować przez wartość.

Serializacja przez wartość tworzy problem, jeśli obiekt zawiera cykliczne odwołanie. To dokładnie w przypadku `Order` i `OrderDetail` klas, ponieważ każdy zawiera odwołanie do drugiego. Program formatujący wykonaj odwołań, zapisywanie każdego obiektu według wartości i go w programie okręgi. W związku z tym należy zmienić to zachowanie domyślne.

W Eksploratorze rozwiązań rozwiń aplikacji\_Uruchom folderu i Otwórz plik o nazwie WebApiConfig.cs. Dodaj następujący kod do `WebApiConfig` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Ten kod ustawia element formatujący JSON, aby zachować odwołania do obiektów i całkowicie usuwa element formatujący XML z potoku. (Można skonfigurować element formatujący XML, aby zachować odwołania do obiektów, ale jest nieco więcej pracy i musimy tylko JSON dla tej aplikacji. Aby uzyskać więcej informacji, zobacz [obsługi odwołań cyklicznych obiektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-1.md)
> [dalej](using-web-api-with-entity-framework-part-3.md)
