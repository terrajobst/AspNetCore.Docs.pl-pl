---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Tworzenie interfejsu API REST atrybutu routingu w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 1f1e90544c9dd8439a522f2196d81d020ea2f4f2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30223265"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Tworzenie interfejsu API REST z atrybutem routingu w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Składnik Web API 2 obsługuje nowy typ routingu, nazywany *trasami atrybutów*. Ogólne omówienie trasami atrybutów, zobacz [atrybutu routingu w sieci Web API 2](attribute-routing-in-web-api-2.md). W tym samouczku użyjesz trasami atrybutów do tworzenia interfejsu API REST dla kolekcji książek. Interfejs API obsługuje następujące akcje:

| Akcja | Przykład identyfikatora URI |
| --- | --- |
| Pobranie listy wszystkich książek. | / api/książek |
| Pobierz książkę według identyfikatora. | /API/Books/1 |
| Pobieranie szczegółów książkę. | /API/Books/1/details |
| Pobierz listę książek według rodzaju. | /API/Books/fantasy |
| Pobierz listę książek według daty publikacji. | /api/books/date/2013/02/16 /API/Books/Date/2013-02-16 (alternatywny) |
| Pobierz listę książek przez konkretnego autora. | /API/authors/1/Books |

Wszystkie metody są tylko do odczytu (żądania HTTP GET).

Dla warstwy danych użyjemy Entity Framework. Rejestruje książki będzie zawierać następujące pola:

- ID
- Tytuł
- Genre
- Data publikacji
- Ceny
- Opis
- Wartość IDAutora (klucz obcy tabeli Autorzy)

Jednak większość żądań interfejsu API zwraca podzbiór danych (tytuł, autora i genre). Aby uzyskać pełny rekord klienta żądania `/api/books/{id}/details`.

## <a name="prerequisites"></a>Wymagania wstępne

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional lub Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

Rozpocznij od działanie programu Visual Studio. Z **pliku** menu, wybierz opcję **nowy** , a następnie wybierz **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu. W obszarze ". Dodaj foldery i podstawowe odwołania dla" Wybierz **interfejsu API sieci Web** wyboru. Kliknij przycisk **Utwórz projekt**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Spowoduje to utworzenie szkielet projektu, który jest skonfigurowany do obsługi funkcji interfejsu API sieci Web.

### <a name="domain-models"></a>Modele domeny

Następnie Dodaj klasy dla modeli domeny. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli. Wybierz **Dodaj**, a następnie wybierz pozycję **klasy**. Nazwa klasy `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Zastąp kod w Author.cs następujące czynności:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Teraz Dodaj kolejną klasę o nazwie `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Dodawanie kontrolera interfejsu API sieci Web

W tym kroku zostanie dodany kontroler Web API, który używa programu Entity Framework jako warstwa danych.

Naciśnij kombinację klawiszy CTRL+SHIFT+B. Projekt zostanie skompilowany. Entity Framework używa odbicia do odnajdywania właściwości modeli, dzięki czemu wymaga skompilowanym zestawie utworzyć schemat bazy danych.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery. Wybierz **Dodaj**, a następnie wybierz pozycję **kontrolera**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję "składnika Web API 2 z akcjami odczytu/zapisu, kontroler, przy użyciu programu Entity Framework."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

W **Dodaj kontroler** okna dialogowego, aby uzyskać **nazwy kontrolera**, wprowadź &quot;BooksController&quot;. Wybierz &quot;używać asynchronicznych akcji kontrolera&quot; wyboru. Aby uzyskać **klasa modelu**, wybierz pozycję &quot;książki&quot;. (Jeśli nie widzisz `Book` klasy wyświetlane na liście rozwijanej, upewnij się, że utworzono projekt.) Następnie kliknij przycisk "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Kliknij przycisk **Dodaj** w **nowy kontekst danych** okna dialogowego.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Kliknij przycisk **Dodaj** w **Dodaj kontroler** okna dialogowego. Rusztowania Dodaje klasę o nazwie `BooksController` definiuje kontrolera interfejsu API. Dodaje również klasę o nazwie `BooksAPIContext` w folderze modeli, który definiuje kontekstu danych Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Inicjatora bazy danych

Wybierz z menu narzędzia **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.

W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

To polecenie tworzy Migrations folder i dodaje nowy plik kodu o nazwie Configuration.cs. Otwórz ten plik i Dodaj następujący kod do `Configuration.Seed` metody.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

W oknie Konsola Menedżera pakietów wpisz następujące polecenia.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Te polecenia Utwórz lokalną bazę danych i wywołania metody inicjatora do wypełniania bazy danych.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Dodawanie klasy DTO

Uruchom aplikację teraz, Wyślij żądanie GET do /api/books/1 odpowiedzi wygląda podobnie do następującego. (Po dodaniu wcięcie dla czytelności.)

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Zamiast tego ma zwracają podzbiór pola to żądanie. Ponadto ma być zwracany imię i nazwisko autora, zamiast identyfikatora autora. W tym celu możemy zmodyfikować metod kontrolera do zwrócenia *obiektu transferu danych* (DTO) zamiast EF modelu. DTO jest obiekt, który jest przeznaczony tylko do przesyłania danych.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** | **nowy Folder**. Nazwa folderu &quot;DTOs&quot;. Dodaj klasę o nazwie `BookDto` do folderu DTOs z następującej definicji:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Dodaj kolejną klasę o nazwie `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Następnie zaktualizuj `BooksController` służącą do zwracania `BookDto` wystąpień. Użyjemy [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metody do projektu `Book` wystąpień do `BookDto` wystąpień. Oto zaktualizowanego kodu do klasy kontrolera.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Po usunięciu `PutBook`, `PostBook`, i `DeleteBook` metody, ponieważ nie są one potrzebne w tym samouczku.


Teraz uruchom aplikację, żądania /api/books/1 treść odpowiedzi powinna wyglądać następująco:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Dodawanie tras atrybutów

Następnie firma Microsoft będzie przekonwertować kontrolera, aby korzystać z routingu atrybutu. Najpierw dodaj **RoutePrefix** atrybutu do kontrolera. Ten atrybut definiuje początkowej segmentów identyfikatora URI dla wszystkich metod na tym kontrolerze.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Następnie dodaj **[trasy]** atrybuty do akcji kontrolera, w następujący sposób:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Szablon trasy dla każdej metody kontrolera jest prefiksem plus ciąg określony w **trasy** atrybutu. Dla `GetBook` metoda, szablon trasy zawiera ciągu sparametryzowanego &quot;{identyfikator: int}&quot;, zgodnej, jeśli segment identyfikator URI zawiera wartość całkowitą.

| Metoda | Szablon trasy | Przykład identyfikatora URI |
| --- | --- | --- |
| `GetBooks` | "interfejsu api/books" | `http://localhost/api/books` |
| `GetBook` | "książek/api / {identyfikator: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Uzyskiwanie szczegółowych informacji książki

Aby uzyskać szczegóły książki, klient wyśle żądanie GET `/api/books/{id}/details`, gdzie *{id}* jest Identyfikatorem książki.

Dodaj następującą metodę do `BooksController` klasy.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Jeśli żądanie zostało wysłane `/api/books/1/details`, odpowiedź wygląda następująco:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Pobierz książek według rodzaju

Aby uzyskać listę książek określonego rodzaju, klient wyśle żądanie GET `/api/books/genre`, gdzie *genre* jest nazwą rodzaju. (Na przykład `/api/books/fantasy`.)

Dodaj następującą metodę do `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

W tym miejscu możemy definiowania trasy, który zawiera parametr {genre} w szablon identyfikatora URI. Zwróć uwagę, że interfejs API sieci Web jest w stanie rozróżnienia tych dwóch identyfikatorów i kierowania ich do różnych metod:

`/api/books/1`

`/api/books/fantasy`

Jest to spowodowane tym `GetBook` metoda zawiera ograniczenie, że segment "id" musi być liczbą całkowitą:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Jeśli żądania /api/books/fantasy odpowiedzi wygląda następująco:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Pobierz książek przez autora

Aby uzyskać listę książek dla konkretnego autora, klient wyśle żądanie GET `/api/authors/id/books`, gdzie *identyfikator* jest Identyfikatorem autora.

Dodaj następującą metodę do `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

W tym przykładzie jest ciekawe ponieważ &quot;książki&quot; jest traktowane zasobu podrzędnego &quot;autorów&quot;. Ten wzorzec jest dość często w interfejsy API RESTful.

Prefiks trasy w zastępuje tyldy (~) w szablonie trasy **RoutePrefix** atrybutu.

## <a name="get-books-by-publication-date"></a>Pobierz książek przez Data publikacji

Aby uzyskać listę książek według daty publikacji, klient wyśle żądanie GET `/api/books/date/yyyy-mm-dd`, gdzie *rrrr mm-dd* jest datą.

Oto jeden ze sposobów:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}` Parametr jest ograniczony do dopasowania **DateTime** wartość. To działa, ale jest rzeczywiście mniej ograniczająca niż chcielibyśmy. Na przykład tych identyfikatorów również zgodna trasy:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Nie ma problem z zezwoleniem na tych identyfikatorów. Może jednak ograniczyć trasy z formatem określonym przez dodanie ograniczenia wyrażenia regularnego do szablonu trasy:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Obecnie tylko daty w postaci &quot;rrrr mm-dd&quot; będą zgodne. Zwróć uwagę, że nie używamy wyrażenia regularnego do zweryfikowania, że dotarliśmy rzeczywistą datę. Który jest obsługiwane w przypadku interfejsu API sieci Web próbuje przekonwertować segmentem identyfikatora URI do **DateTime** wystąpienia. Nieprawidłowa data, takich jak "2012-47-99" zakończy się niepowodzeniem ma zostać przekonwertowane, a klient otrzyma błąd 404.

Można również obsługiwać separatora ukośnika (`/api/books/date/yyyy/mm/dd`) przez dodanie kolejnego **[trasy]** atrybut o różnych wyrażenia regularnego.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Brak niewielkie, ale ważne szczegóły tutaj. Drugi szablon trasy zawiera symbol wieloznaczny (\*) na początku parametru {pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Ta wartość informuje aparatu routingu tego {pubdate} powinna być taka sama pozostałej części identyfikatora URI. Domyślnie parametr szablonu odpowiada jednej segmentem identyfikatora URI. W takim przypadku chcemy {pubdate} span kilkoma segmentami identyfikatora URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Kod kontrolera

W tym miejscu jest kompletny kod dla klasy BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Podsumowanie

Atrybut routingu daje więcej kontroli i większą elastyczność podczas projektowania identyfikatorów URI dla interfejsu API.
