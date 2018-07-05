---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Obsługa relacji jednostek | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 7759b828068d99f9975d56671e427ccf6e94aef6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400956"
---
<a name="handling-entity-relations"></a>Obsługa relacji jednostek
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji opisano niektóre szczegóły, jak EF ładuje powiązanych jednostek i sposób obsługi właściwości nawigacji cykliczne w klasach modeli. (Ta sekcja zawiera wiedzę i nie jest wymagane do ukończenia tego samouczka. Jeśli wolisz, przejdź do [część 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Eager, ładowania i ładowania z opóźnieniem

Przy użyciu programu EF z relacyjnej bazy danych, jest ważne, aby zrozumieć, jak EF ładuje powiązanych danych.

Jest to również przydatne wyświetlić zapytania SQL, które generuje EF. Śledzenie SQL, Dodaj następujący wiersz kodu, aby `BookServiceContext` Konstruktor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Jeśli wyślesz żądanie GET do /api/books zwraca JSON podobne do następującego:

[!code-console[Main](part-4/samples/sample2.cmd)]

Widać, że właściwość Autor ma wartość null, mimo że książka zawiera prawidłową wartość IDAutora. Wynika to z programów EF nie trwa ładowanie powiązanych jednostek autora. Zapytanie SQL w dzienniku śledzenia potwierdza to:

[!code-console[Main](part-4/samples/sample3.sql)]

Instrukcja SELECT przyjmuje z tabeli książki, a nie odwołuje się do tabeli autora.

Odwołanie, Oto metoda `BooksController` klasę, która zwraca listę książki.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Zobaczmy, jak firma Microsoft może zwracać autora jako część danych JSON. Istnieją trzy sposoby ładowanie powiązanych danych platformy Entity Framework: wczesne ładowanie, powolne ładowanie i jawne ładowanie. Ma wad i zalet z każdą z technik, więc jest ważne zrozumieć, jak działają.

### <a name="eager-loading"></a>Wczesne ładowanie

Za pomocą *wczesne ładowanie*, EF ładuje powiązanych jednostek jako część zapytania początkowej bazy danych. Aby wykonać wczesne ładowanie, użyj **System.Data.Entity.Include** — metoda rozszerzenia.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Informuje EF, aby dołączyć dane autora w zapytaniu. Jeśli chcesz wprowadzić tę zmianę i uruchomić aplikację, dane JSON wygląda teraz następująco:

[!code-console[Main](part-4/samples/sample6.cmd)]

W dzienniku śledzenia pokazuje, że EF wykonywane sprzężenia w tabelach książki i autora.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Ładowanie z opóźnieniem

Przy użyciu ładowania z opóźnieniem EF automatycznie ładuje powiązanej jednostki, kiedy jest wyłuskiwany właściwości nawigacji dla danej jednostki. Aby włączyć powolne ładowanie, Tworzenie wirtualnego właściwości nawigacji. Na przykład w klasie książki:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Teraz należy wziąć pod uwagę następujący kod:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Po włączeniu ładowania z opóźnieniem, uzyskiwanie dostępu do `Author` właściwość `books[0]` powoduje, że EF dla autora kwerendy bazy danych.

Powolne ładowanie wymaga wielu rund bazy danych, ponieważ EF wysyła zapytanie każdorazowo pobiera powiązanej jednostki. Ogólnie rzecz biorąc ma powolne ładowanie wyłączone dla obiektów, które można serializować. Serializator musi odczytywać wszystkie właściwości w modelu, który wyzwala ładowanie powiązanych jednostek. Na przykład poniżej przedstawiono zapytania SQL podczas EF serializuje listy książek przy użyciu ładowania z opóźnieniem włączone. Aby zobaczyć, że EF sprawia, że trzy oddzielne zapytania dla trzech autorów.

[!code-console[Main](part-4/samples/sample10.sql)]

Czas, kiedy warto korzystać z opóźnieniem ładowania nadal istnieją. Wczesne ładowanie może spowodować EF do generowania bardzo złożone sprzężenia. Lub możesz potrzebować powiązanych jednostek dla małego podzbioru danych i powolne ładowanie będzie bardziej wydajne.

Jednym ze sposobów, aby uniknąć problemów z serializacji jest do wykonywania serializacji obiektów transferu danych (dto) zamiast obiektów jednostek. Zaprezentuję, to podejście w dalszej części tego artykułu.

### <a name="explicit-loading"></a>Jawne ładowanie

Jawne ładowanie jest podobny do ładowania z opóźnieniem, chyba że jawnie uzyskać powiązanych danych w kodzie nie będzie automatycznie gdy uzyskujesz dostęp do właściwości nawigacji. Jawne ładowanie daje większą kontrolę nad tym, kiedy ładowanie powiązanych danych, ale wymaga dodatkowego kodu. Aby uzyskać więcej informacji na temat jawne ładowanie zobacz [ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Właściwości nawigacji i odwołania cykliczne

Po zdefiniowaniu książki i tworzenie modeli I na jest zdefiniowana właściwość nawigacji `Book` klasy relacji autor książki, ale nie mogę definiują właściwości nawigacji w drugą stronę.

Co się stanie po dodaniu odpowiednią właściwość nawigacji do `Author` klasy?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Niestety spowoduje to utworzenie problem podczas serializacji modeli. Jeśli załadujesz powiązanych danych, tworzy wykres obiektu cykliczne.

![](part-4/_static/image1.png)

Próba serializacji grafu przez program formatujący JSON lub XML zgłosi wyjątek. Dwa elementy formatujące generują komunikaty o wyjątkach różne. Oto przykład dla elementu formatującego JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Poniżej przedstawiono program formatujący kod XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Jedno rozwiązanie ma używać dto, zawierające w następnej sekcji. Alternatywnie można skonfigurować formatami JSON i XML elementy formatujące do obsługi cykle wykresu. Aby uzyskać więcej informacji, zobacz [obsługi odwołań cyklicznych obiektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Na potrzeby tego samouczka nie ma potrzeby `Author.Book` właściwość nawigacji, dzięki czemu możesz pozostawić.

> [!div class="step-by-step"]
> [Poprzednie](part-3.md)
> [dalej](part-5.md)
