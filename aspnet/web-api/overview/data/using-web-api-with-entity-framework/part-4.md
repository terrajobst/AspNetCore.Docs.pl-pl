---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: "Obsługa relacjami jednostek | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 58a9dfb621630f23b37247b96ed3a19a661857f1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="handling-entity-relations"></a>Obsługa relacjami jednostek
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji opisano niektóre szczegółowe informacje, jak EF ładuje powiązanych jednostek i sposób obsługi właściwości nawigacji cykliczne w klasach modeli. (Ta sekcja zawiera tła wiedzy i nie jest wymagane do ukończenia tego samouczka. Jeśli wolisz, przejdź do [część 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Eager ładowania i opóźnionego ładowania

Jeśli używasz EF z relacyjnej bazy danych, jest zrozumieć, jak EF ładuje dane dotyczące.

Jest również zobaczyć zapytań SQL, które generuje EF. Śledzenie SQL, Dodaj następujący wiersz kodu w celu `BookServiceContext` konstruktora:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Po wysłaniu żądania GET do /api/books zwraca JSON podobne do poniższych:

[!code-console[Main](part-4/samples/sample2.cmd)]

Widać, właściwość Autor jest pusty, nawet jeśli książce zawiera prawidłową wartość IDAutora. Wynika to z EF nie ładuje powiązanych jednostek autora. Dziennik śledzenia zapytania SQL potwierdza to:

[!code-console[Main](part-4/samples/sample3.sql)]

Instrukcja SELECT przyjmuje z tabeli książki i nie odwołuje się do tabeli autora.

Odwołanie, w tym miejscu jest metoda `BooksController` klasy, która zwraca listy książek.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Zobaczmy, jak zostanie zwrócona autora w ramach dane JSON. Istnieją trzy sposoby załadować powiązanych danych w programie Entity Framework: wczesny ładowania opóźnionego ładowania i jawnego ładowania. Istnieją kompromis każdego technika, dlatego ważne jest, aby zrozumieć, jak działają.

### <a name="eager-loading"></a>Ładowanie wczesny

Z *wczesny ładowania*, EF ładuje powiązanych jednostek jako część zapytania początkowej bazy danych. Aby przeprowadzić ładowanie wczesny, użyj **System.Data.Entity.Include** — metoda rozszerzenia.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Ta wartość informuje EF, aby dołączyć dane autora w zapytaniu. Jeśli chcesz wprowadzić tę zmianę i uruchomić aplikację, teraz danych JSON wygląda następująco:

[!code-console[Main](part-4/samples/sample6.cmd)]

Dziennik śledzenia pokazuje EF wykonanie sprzężenia w tabelach książki i autora.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Powolne ładowanie

Z opóźnieniem ładowania EF automatycznie ładuje obiektu pokrewnego, gdy jest wyłuskiwany właściwości nawigacji dla danej jednostki. Aby włączyć ładowanie opóźnieniem, Tworzenie wirtualnego właściwości nawigacji. Na przykład w klasie książki:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Teraz Rozważmy następujący kod:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Podczas ładowania opóźnieniem jest włączone, dostęp do `Author` właściwość `books[0]` powoduje, że EF dla autora kwerendy bazy danych.

Powolne ładowanie wymaga wiele rund bazy danych, ponieważ EF wysyła zapytanie zawsze pobiera powiązanej jednostki. Ogólnie rzecz biorąc ma powolne ładowanie wyłączone dla obiektów, które można serializować. Serializator musi odczytywać wszystkie właściwości w modelu, który wyzwala ładowanie powiązanych jednostek. Na przykład poniżej przedstawiono zapytania SQL podczas EF serializuje listy książek z opóźnieniem ładowania włączone. Widać, że EF udostępnia trzy oddzielne zapytania dla trzech autorów.

[!code-console[Main](part-4/samples/sample10.sql)]

Nadal istnieją razy podczas możesz chcieć użyć opóźnionego ładowania. Ładowanie wczesny może spowodować EF do generowania bardzo złożonych sprzężenia. Powiązanych jednostek mogą być wymagane do małego podzbioru danych i bardziej efektywne byłoby opóźnionego ładowania.

Jest jednym ze sposobów uniknąć problemów serializacji do serializacji obiektów transfer danych (DTOs) zamiast obiektów jednostek. Ta metoda będzie wyświetlić w dalszej części tego artykułu.

### <a name="explicit-loading"></a>Ładowanie jawne

Jawne ładowania jest podobny do opóźnionego ładowania, z wyjątkiem jawnie pobrać powiązanych danych w kodzie; go nie jest realizowane automatycznie podczas dostępu do właściwości nawigacji. Ładowanie jawne zapewnia deweloperom większą kontrolę nad tym, kiedy można załadować dane dotyczące, ale wymaga dodatkowego kodu. Aby uzyskać więcej informacji na temat jawnego ładowania, zobacz [ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Właściwości nawigacji oraz odwołania cykliczne

Po zdefiniowaniu I modele książki i autora, na jest zdefiniowana właściwość nawigacji `Book` klasy relacji Autor księgi, ale nie zdefiniowano I właściwości nawigacji w innym kierunku.

Co się stanie po dodaniu odpowiednią właściwość nawigacji do `Author` klasy?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Niestety tworzy to problem podczas serializacji modeli. Po załadowaniu powiązanych danych tworzy wykres obiektu cykliczne.

![](part-4/_static/image1.png)

Próba serializować wykresu przez program formatujący JSON i XML zgłosi wyjątek. Dwa elementy formatujące generują komunikaty o różnych wyjątku. Oto przykład dla elementu formatującego JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Oto element formatujący XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Rozwiązanie polega na Użyj DTOs, opisujące w następnej sekcji. Alternatywnie można skonfigurować JSON i XML programów formatujących do obsługi cykle wykresu. Aby uzyskać więcej informacji, zobacz [obsługi odwołań cyklicznych obiektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

W tym samouczku, nie potrzebujesz `Author.Book` właściwość nawigacji, więc możesz go Opuść.

>[!div class="step-by-step"]
[Poprzednie](part-3.md)
[dalej](part-5.md)
