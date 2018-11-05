---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Tworzenie interfejsu API REST przy użyciu atrybutu routingu we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 18a44c280e6df1603837938d24d7d639d8c87cc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021420"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Tworzenie interfejsu API REST z atrybutem routingu we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Składnik Web API 2 obsługuje nowy typ routingu, o nazwie *trasowanie atrybutów*. Aby uzyskać ogólne omówienie trasowanie atrybutów, zobacz [atrybut routingu w sieci Web API 2](attribute-routing-in-web-api-2.md). W tym samouczku użyjesz trasowanie atrybutów do tworzenia interfejsu API REST dla kolekcji książek. Interfejs API obsługuje następujące akcje:

| Akcja | Przykład identyfikatora URI |
| --- | --- |
| Zostanie wyświetlona lista wszystkich książek. | / api/książki |
| Pobierz książkę według identyfikatora. | /API/Books/1 |
| Pobieranie szczegółów książki. | /API/Books/1/details |
| Zostanie wyświetlona lista książek według gatunku. | /API/Books/fantasy |
| Zostanie wyświetlona lista książek według daty publikacji. | /api/books/date/2013/02/16 /API/Books/Date/2013-02-16 (alternatywna postać) |
| Zostanie wyświetlona lista książek przez określonego autora. | /API/authors/1/Books |

Wszystkie metody są tylko do odczytu (żądania HTTP GET).

Dla warstwy danych użyto programu Entity Framework. Rekordy książek będzie zawierać następujące pola:

- ID
- Tytuł
- Gatunku
- Data publikacji
- Cena
- Opis
- Wartość IDAutora (klucz obcy tabeli Autorzy)

W większości żądań, natomiast interfejs API zwróci podzbiór danych (tytuł, autor i gatunku). Do żądania get całego rekordu klienta `/api/books/{id}/details`.

## <a name="prerequisites"></a>Wymagania wstępne

[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

Rozpocznij od uruchamianie programu Visual Studio. Z **pliku** menu, wybierz opcję **New** , a następnie wybierz **projektu**.

Rozwiń **zainstalowane** > **Visual C#** kategorii. W obszarze **Visual C#**, wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web platformy ASP.NET (.NET Framework)**. Nadaj projektowi nazwę &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

W **Nowa aplikacja internetowa ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu. W obszarze "Dodaj foldery i podstawowe odwołania dla" Wybierz **interfejsu API sieci Web** pola wyboru. Kliknij przycisk **OK**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Spowoduje to utworzenie szkielet projektu, który jest skonfigurowany do obsługi funkcji interfejsu API sieci Web.

### <a name="domain-models"></a>Modeli domeny

Następnie Dodaj klasy dla modeli domeny. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele. Wybierz **Dodaj**, a następnie wybierz **klasy**. Nazwa klasy `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Zastąp kod w Author.cs następujących czynności:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Teraz Dodaj klasę o nazwie `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Dodaj Kontroler interfejsu API sieci Web

W tym kroku dodasz kontroler internetowego interfejsu API, który używa programu Entity Framework jako warstwa danych.

Naciśnij kombinację klawiszy CTRL+SHIFT+B. Projekt zostanie skompilowany. Entity Framework używa odbicia, aby odnaleźć właściwości te modele, dzięki czemu wymaga skompilowanego zestawu w celu utworzenia schematu bazy danych.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj**, a następnie wybierz **kontrolera**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler internetowego interfejsu API 2 z akcjami używający narzędzia Entity Framework**.

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

W **Dodaj kontroler** okna dialogowego, aby uzyskać **nazwy kontrolera**, wprowadź &quot;BooksController&quot;. Wybierz &quot;używać asynchronicznych akcji kontrolera&quot; pola wyboru. Aby uzyskać **klasa modelu**, wybierz opcję &quot;książki&quot;. (Jeśli nie widzisz `Book` klasy na liście rozwijanej, upewnij się, że utworzony projekt.) Następnie kliknij przycisk "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Kliknij przycisk **Dodaj** w **nowy kontekst danych** okna dialogowego.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Kliknij przycisk **Dodaj** w **Dodaj kontroler** okna dialogowego. Szkieletu Dodaje klasę o nazwie `BooksController` definiujący Kontroler interfejsu API. Dodaje również klasę o nazwie `BooksAPIContext` w folderze modeli, która definiuje kontekst danych Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Inicjowanie bazy danych

Wybierz z menu narzędzia **Menedżera pakietów NuGet**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.

W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

To polecenie tworzy folder migracje i dodaje nowy plik kodu o nazwie Configuration.cs. Otwórz ten plik i Dodaj następujący kod do `Configuration.Seed` metody.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

W oknie Konsola Menedżera pakietów wpisz następujące polecenia.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Te polecenia Utwórz lokalnej bazy danych i wywołaj metodę inicjatora do wypełniania bazy danych.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Dodaj obiekt DTO klasy

Uruchom aplikację teraz, Wyślij żądanie Pobierz do /api/books/1 odpowiedź wygląda podobnie do następującego. (Po dodaniu wcięć dla czytelności.)

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Zamiast tego chcę tego żądania, aby zwrócić podzbiór pól. Ponadto powinna być zwraca imię i nazwisko autora, a nie identyfikatora autora. Aby to osiągnąć, zmodyfikujemy metody kontrolera, aby zwrócić *obiekt transferu danych* (DTO) zamiast modelu platformy EF. Obiekt DTO jest obiektem, który jest przeznaczony tylko do przesyłania danych.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** | **nowy Folder**. Nazwa folderu &quot;dto&quot;. Dodaj klasę o nazwie `BookDto` do folderu dto, przy użyciu następujących definicji:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Dodaj klasę o nazwie `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Następnie zaktualizuj `BooksController` klasy w celu zwracania `BookDto` wystąpień. Użyjemy [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metody do projektu `Book` wystąpień do `BookDto` wystąpień. Oto zaktualizowany kod do klasy kontrolera.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Po usunięciu `PutBook`, `PostBook`, i `DeleteBook` metod, ponieważ nie są one potrzebne w tym samouczku.


Teraz uruchom aplikację, żądanie /api/books/1 treść odpowiedzi powinna wyglądać następująco:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Dodawanie atrybutów trasy

Firma Microsoft będzie następnie przekonwertować kontroler trasowanie atrybutów. Najpierw dodaj **RoutePrefix** atrybutu do kontrolera. Ten atrybut definiuje początkowe segmentów identyfikator URI dla wszystkich metod na tym kontrolerze.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Następnie dodaj **[trasy]** atrybuty do akcji kontrolera, w następujący sposób:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Szablon trasy dla każdej metody kontrolera jest prefiksem, a także ciąg określony w **trasy** atrybutu. Dla `GetBook` , szablon trasy zawiera ciągu sparametryzowanego &quot;{identyfikator: int}&quot;, która pasuje, jeśli segmentem identyfikatora URI zawiera wartość całkowitą.

| Metoda | Szablon trasy | Przykład identyfikatora URI |
| --- | --- | --- |
| `GetBooks` | "interfejs api/books" | `http://localhost/api/books` |
| `GetBook` | "interfejs api/książki / {identyfikator: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Pobieranie szczegółów książki

Aby uzyskać szczegółowe informacje z książki, klient wyśle żądanie GET w celu `/api/books/{id}/details`, gdzie *{id}* to identyfikator książki.

Dodaj następującą metodę do `BooksController` klasy.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Jeśli żądania `/api/books/1/details`, odpowiedź wygląda następująco:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Pobieranie książki według gatunku

Aby uzyskać listę książek określonego rodzaju, klient wyśle żądanie GET w celu `/api/books/genre`, gdzie *gatunku* to nazwa tego gatunku. (Na przykład `/api/books/fantasy`.)

Dodaj następującą metodę do `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

W tym miejscu możemy definiowania tras, która zawiera parametr {gatunku} w szablon identyfikatora URI. Zauważ, że interfejs API sieci Web rozróżnienie tych dwóch identyfikatorów i kierowania ich do różnych metod:

`/api/books/1`

`/api/books/fantasy`

To dlatego, że `GetBook` metoda zawiera ograniczenie, że segment "id" musi być wartością całkowitą z zakresu:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Jeśli żądania /api/books/fantasy odpowiedź wygląda następująco:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Pobieranie książki według autora

Aby uzyskać listę książek dla danego autora, klient wyśle żądanie GET w celu `/api/authors/id/books`, gdzie *identyfikator* to identyfikator autora.

Dodaj następującą metodę do `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

W tym przykładzie jest interesująca ponieważ &quot;książki&quot; jest traktowane zasobu podrzędnego &quot;autorzy&quot;. Ten wzorzec jest bardzo częsty w interfejsów API RESTful.

Tyldy (~) w szablonie trasy zastępuje prefiks trasy w **RoutePrefix** atrybutu.

## <a name="get-books-by-publication-date"></a>Pobieranie książki według: Data publikacji

Aby uzyskać listę książki według daty publikacji, klient wyśle żądanie GET w celu `/api/books/date/yyyy-mm-dd`, gdzie *rrrr mm-dd* to dzień.

Oto jeden ze sposobów, aby to zrobić:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}` Ograniczony jest parametr, aby dopasować **daty/godziny** wartość. Ta funkcja działa, ale jest faktycznie mniej ograniczająca niż prosimy o poświęcenie. Na przykład następujące identyfikatory URI, również będą zgodne trasy:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Nie ma problem z zezwoleniem na te identyfikatory URI. Może jednak ograniczyć trasy do określonego formatu, Dodawanie ograniczenia wyrażenia regularnego do szablonu trasy:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Teraz tylko daty w postaci &quot;rrrr mm-dd&quot; będą zgodne. Należy zauważyć, że nie używamy wyrażenia regularnego można zweryfikować, że mamy rzeczywiste daty. Który odbywa się w przypadku interfejsu API sieci Web podejmuje próbę przekonwertowania segmentem identyfikatora URI do **daty/godziny** wystąpienia. Nieprawidłowa data, takie jak "2012-47-99" zakończy się niepowodzeniem ma zostać przekonwertowany, a klient otrzyma błąd 404.

Może również obsługiwać separator ukośnika (`/api/books/date/yyyy/mm/dd`) przez dodanie innej **[trasy]** atrybut o inne wyrażenie regularne.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Brak subtelne, ale ważne szczegóły poniżej. Drugi szablon trasy zawiera symbol wieloznaczny (\*) na początku parametru {pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Informuje aparat routingu, że {pubdate} powinien być zgodny z pozostałą część identyfikatora URI. Domyślnie parametrem szablonu dopasowuje pojedynczy segment identyfikatora URI. W tym przypadku chcemy {pubdate} obejmować wiele segmentów identyfikatora URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Kod kontrolera

Oto kompletny kod dla klasy BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Podsumowanie

Trasowanie atrybutów zapewnia więcej kontroli i większą elastyczność podczas projektowania identyfikatorów URI dla interfejsu API.
