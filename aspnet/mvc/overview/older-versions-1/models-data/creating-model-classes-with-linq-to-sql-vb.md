---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Tworzenie klasy modelu za pomocą LINQ do SQL (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Celem tego samouczka jest wyjaśnienie jedną metodę tworzenia klasy modelu dla aplikacji platformy ASP.NET MVC. W tym samouczku Dowiedz się jak tworzyć c modelu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 5438838123c40d82afbda191a48878d6dca80736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874814"
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>Tworzenie klasy modelu za pomocą LINQ do SQL (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> Celem tego samouczka jest wyjaśnienie jedną metodę tworzenia klasy modelu dla aplikacji platformy ASP.NET MVC. W tym samouczku Dowiedz się jak utworzyć klasy modelu i wykonywać dostęp do bazy danych dzięki wykorzystaniu Microsoft LINQ do SQL.


Celem tego samouczka jest wyjaśnienie jedną metodę tworzenia klasy modelu dla aplikacji platformy ASP.NET MVC. W tym samouczku Dowiedz się jak utworzyć klasy modelu i wykonywać dostęp do bazy danych dzięki wykorzystaniu Microsoft LINQ do SQL.

W tym samouczku budujemy podstawowej filmu aplikacji bazy danych. Firma Microsoft Rozpocznij od utworzenia filmu aplikacji bazy danych w sposób najszybszym i najprostszym możliwe. Wszystkie nasze dostęp do danych wykonać bezpośrednio z naszych akcji kontrolera.

Następnie zostanie przedstawiony sposób użycia wzorca repozytorium. Przy użyciu wzorca repozytorium wymaga nieco więcej wysiłku. Jednak zaletą tego wzorca przyjmowanie jest umożliwia tworzenie aplikacji, które są pasujący do zmiany i mogą być testowane w prosty sposób.

## <a name="what-is-a-model-class"></a>Co to jest klasa modelu?

MVC model zawiera wszystkie logiki aplikacji, który nie jest zawarty w widoku MVC lub kontrolera MVC. W szczególności modelu MVC zawiera wszystkie aplikacja biznesowa i logika dostępu do danych.

Szereg różnych technologii służy do implementowania logika dostępu do danych. Na przykład można tworzyć przy użyciu klasy Microsoft Entity Framework, NHibernate, Subsonic lub ADO.NET klas dostępu do danych.

W tym samouczku używam LINQ do SQL do wykonywania zapytań i zaktualizować bazę danych. LINQ do SQL zapewnia bardzo łatwa metoda interakcji z bazą danych programu Microsoft SQL Server. Jest jednak wziąć pod uwagę, że struktura ASP.NET MVC nie jest związana z LINQ do SQL w dowolny sposób. ASP.NET MVC jest zgodny z technologii dostępu do żadnych danych.

## <a name="create-a-movie-database"></a>Utwórz bazę danych film

W tym samouczku — aby zilustrować, jak można utworzyć klasy modelu — możemy utworzyć prostą aplikację filmu bazy danych. Pierwszym krokiem jest utworzenie nowej bazy danych. Kliknij prawym przyciskiem myszy aplikację\_folderu danych w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**. Wybierz szablon bazy danych programu SQL Server, nazwę MoviesDB.mdf, a następnie kliknij przycisk **Dodaj** przycisk (zobacz rysunek 1).


[![Dodawanie nowej bazy danych serwera SQL](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Rysunek 01**: Dodawanie nowej bazy danych serwera SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


Po utworzeniu nowej bazy danych, można otworzyć bazy danych, klikając dwukrotnie plik MoviesDB.mdf w aplikacji\_folderem danych. Dwukrotne kliknięcie pliku MoviesDB.mdf powoduje otwarcie okna Eksploratora serwera (patrz rysunek 2).


|   | Okno Eksploratora serwera jest wywoływana okno Eksploratora bazy danych, korzystając z programu Visual Web Developer. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![W oknie Eksploratora serwera](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Rysunek 02**: Korzystanie z okna Eksploratora serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


Musimy dodać jednej tabeli do naszej bazy danych, która reprezentuje naszych filmów. Kliknij prawym przyciskiem myszy folder Tabele i wybierz opcję menu **Dodaj nową tabelę**. Wybranie tej opcji menu otwiera projektanta tabeli (patrz rysunek 3).


[![W oknie Eksploratora serwera](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Rysunek 03**: projektanta tabel ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


Należy dodać następujące kolumny do tabeli w naszej bazie danych:

| **Nazwa kolumny** | **Typ danych** | **Dopuszcza wartości null** |
| --- | --- | --- |
| Id | int | False |
| Tytuł | Nvarchar(200) | False |
| Dyrektor | nvarchar(50) | False |

Należy wykonać dwie czynności specjalne w kolumnie identyfikator. Najpierw należy oznaczyć w kolumnie identyfikator jako kolumna klucza podstawowego, wybierając kolumnę w projektanta tabel i kliknąć ikonę klucza. LINQ do SQL należy określić klucz podstawowy, podczas wykonywania wstawia lub aktualizacji w bazie danych.

Następnie należy oznaczyć identyfikator kolumny jako kolumny tożsamości przez przypisanie wartości, tak aby **tożsamości jest** właściwości (patrz rysunek 3). Kolumna tożsamości jest kolumnę, w której jest przypisany nowy numer automatycznie po dodaniu nowego wiersza danych do tabeli.

Po wprowadzeniu tych zmian zapisać tblMovie nazwy tabeli. Można zapisać tabeli, klikając przycisk Zapisz.

## <a name="create-linq-to-sql-classes"></a>Tworzenie klasy LINQ do SQL

Nasz model MVC będzie zawierać LINQ w klasach SQL, które reprezentują tblMovie tabeli bazy danych. Najprostszym sposobem tworzenia te klasy LINQ do SQL jest kliknij prawym przyciskiem myszy folder modeli, wybierz **Dodaj, nowy element**, wybierz LINQ w klasach SQL szablonie, nazwę klasy Movie.dbml, a następnie kliknij przycisk **Dodaj**przycisk (patrz rysunek 4).


[![Tworzenie LINQ w klasach SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Rysunek 04**: Tworzenie klasy LINQ do SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


Bezpośrednio po utworzeniu filmu klasy LINQ do SQL zostanie wyświetlona Projektanta obiektów relacyjnych. Przeciągnij z tabel bazy danych, z okna Eksploratora serwera do Projektanta obiektów relacyjnych można utworzyć klasy LINQ do SQL reprezentujących tabele określonej bazy danych. Musimy dodać tblMovie tabeli bazy danych do Projektanta obiektów relacyjnych (patrz rysunek 4).


[![Za pomocą Projektanta obiektów relacyjnych](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Rysunek 05**: za pomocą Projektanta obiektów relacyjnych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


Domyślnie Projektant obiektów relacyjnych tworzy klasę o tej samej nazwie, jak przeciągniętego projektanta tabeli bazy danych. Jednak nie chcemy wywołać naszych tblMovie klasy. W związku z tym kliknij nazwę klasy w Projektancie i Zmień nazwę klasy filmu.

Na koniec należy pamiętać o kliknij **zapisać** przycisk (obraz dyskietek), aby zapisać LINQ w klasach SQL. W przeciwnym razie LINQ w klasach SQL nie zostać wygenerowany przez Projektanta obiektów relacyjnych.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Za pomocą LINQ do SQL w akcji kontrolera

Teraz, gdy mamy naszej klasy LINQ do SQL, możemy użyć tych klas do pobierania danych z bazy danych. W tej sekcji Dowiedz się jak używać LINQ w klasach SQL bezpośrednio w ramach akcji kontrolera. Firma Microsoft będzie wyświetlać listy filmów z tabeli bazy danych tblMovies w widoku MVC.

Najpierw należy zmodyfikować klasy HomeController. Ta klasa znajduje się w folderze kontrolery aplikacji. Modyfikowanie klasy tak wygląda jak klasa wyświetlania 1.

**1 — Lista `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

Akcja indeks() wyświetlania 1 używa składnika LINQ to SQL DataContext klasy (MovieDataContext), do reprezentowania MoviesDB bazy danych. Klasa MoveDataContext został wygenerowany przez program Visual Studio Projektant obiektów relacyjnych.

Zapytania LINQ odbywa się przed DataContext pobrać wszystkie filmy tblMovies tabeli bazy danych. Lista filmów jest przypisany do zmiennej lokalnej o nazwie filmów. Na koniec listy filmów jest przekazywana do widoku danych widoku.

Aby pokazać filmy, następnie należy zmodyfikować widok indeksu. Widok indeksu można znaleźć w folderze Views\Home\. Widok indeksu należy zaktualizować, tak aby wygląda jak Widok wyświetlania 2.

**2 — Lista `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Należy zauważyć, że zmodyfikowany widok indeksu zawiera &lt;% @ importu przestrzeń nazw %&gt; dyrektywy u góry widoku. Ta dyrektywa importuje MvcApplication1 przestrzeni nazw. Potrzebujemy tej przestrzeni nazw, aby pracować z klasami modelu — w szczególności klasy filmu — w widoku.

Widok wyświetlania 2 zawiera For każdej pętli, który iteruje po wszystkich elementów reprezentowany przez właściwość ViewData.Model. Wartość właściwości tytułu jest wyświetlany dla każdego filmu.

Należy zauważyć, że wartość właściwości ViewData.Model jest rzutowane na interfejs IEnumerable. Jest to konieczne, aby przeglądać zawartość ViewData.Model. Inną opcją w tym miejscu jest tworzenie silnie typizowanego widoku. Podczas tworzenia widoku jednoznacznie można rzutować właściwości ViewData.Model do określonego typu w klasie związanej z kodem widoku.

Po uruchomieniu aplikacji po zmodyfikowaniu klasy HomeController i widoku indeksu, a następnie zostanie wyświetlona pusta strona. Zostanie wyświetlony pustej strony, ponieważ nie ma żadnych rekordów filmu tabeli tblMovies bazy danych.

Aby dodać rekordy do tabeli tblMovies bazy danych, kliknij prawym przyciskiem myszy tblMovies tabeli bazy danych w oknie Eksploratora serwera (okno Eksploratora bazy danych w Visual Web Developer) i wybierz opcję menu **Pokaż dane tabeli**. Można wstawić rekordów filmu przy użyciu siatki, którą (patrz rysunek 5).


[![Wstawianie filmy](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Rysunek 06**: Wstawianie filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


Po dodać niektóre rekordy z bazy danych do tabeli tblMovies i uruchomić aplikację, zobaczysz stronę na rysunku 7. Wszystkie rekordy filmu bazy danych są wyświetlane na liście punktowanej.


[![Wyświetlanie filmów z widokiem indeksu](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Rysunek 07**: wyświetlanie filmów z widoku Index ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Przy użyciu wzorca repozytorium

W poprzedniej sekcji użyliśmy LINQ w klasach SQL bezpośrednio w ramach akcji kontrolera. My używamy klasy MovieDataContext bezpośrednio z indeks() akcji kontrolera. Nie ma nic błąd w ten sposób w przypadku prostą aplikację. Jednak praca bezpośrednio z LINQ do SQL w klasie kontrolera stwarza problemy, gdy jest potrzebne do tworzenia bardziej złożonych aplikacji.

Za pomocą LINQ do SQL w obrębie klasy kontrolera utrudnia przełącznika technologii dostępu do danych w przyszłości. Na przykład można zdecydować przełączyć się z Microsoft LINQ do SQL, które jako technologii dostępu do danych za pomocą programu Microsoft Entity Framework. W takim przypadku należy zmodyfikować każdy kontroler, który uzyskuje dostęp do bazy danych w aplikacji.

Za pomocą LINQ do SQL w obrębie klasy kontrolera również utrudnia do tworzenia testów jednostkowych dla aplikacji. Zazwyczaj mają wchodzić w interakcje z bazy danych podczas przeprowadzania testów jednostkowych. Chcesz użyć testy jednostkowe do testowania logiki aplikacji i nie jest serwerem bazy danych.

Aby można było skompilować aplikację MVC więcej pasujący do przyszłych zmian i które można łatwiej przetestować, należy rozważyć zastosowanie wzorca repozytorium. Użycie wzorca repozytorium, Utwórz klasę oddzielne repozytorium, zawierający wszystkie logika dostępu do bazy danych.

Podczas tworzenia klasy repozytorium, możesz utworzyć interfejs, który reprezentuje wszystkie metody używane przez klasę repozytorium. W ramach kontrolerów pisania kodu interfejsu zamiast repozytorium. W ten sposób można zaimplementować repozytorium przy użyciu technologii dostępu do danych w przyszłości.

Interfejs w 3 wyświetlania o nazwie IMovieRepository i reprezentuje jedną metodę o nazwie ListAll().

**3 — lista `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

Klasa repozytorium w 4 lista implementuje interfejs IMovieRepository. Zwróć uwagę, że zawiera on metodę o nazwie ListAll() odpowiadającej metody wymagane przez interfejs IMovieRepository.

**Wyświetlanie listy 4. `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Ponadto klasa MoviesController listę 5 korzysta ze wzorca repozytorium. Nie jest już używa LINQ w klasach SQL bezpośrednio.

**Wyświetlanie listy 5 — `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Zwróć uwagę, że klasa MoviesController listę 5 ma dwa konstruktory. Pierwszy konstruktora konstruktora bez parametrów jest wywoływane, gdy aplikacja jest uruchomiona. Ten konstruktor tworzy wystąpienie klasy MovieRepository i przekazuje je do drugiego konstruktora.

Drugi Konstruktor ma jeden parametr: parametr IMovieRepository. Ten konstruktor po prostu przypisuje wartość parametru na poziomie klasy pole o nazwie \_repozytorium.

Klasa MoviesController jest korzystanie z oprogramowania wzorca projektowego nazywany wzorcem iniekcji zależności. W szczególności używa coś wywołuje konstruktor iniekcji zależności. Możesz przeczytać więcej na temat tego wzorca odczytując w następującym artykule przez pole Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Zwróć uwagę, że cały kod w klasie MoviesController (z wyjątkiem pierwszej Konstruktor) współdziała z interfejsu IMovieRepository zamiast rzeczywistej klasy MovieRepository. Kod wchodzi w interakcję z interfejsem abstrakcyjny, a nie konkretną implementację interfejsu.

Jeśli chcesz zmodyfikować technologii dostępu do danych, które są używane przez aplikację może po prostu implementować interfejs IMovieRepository z klasy, która korzysta z technologii dostępu do alternatywnej bazy danych. Na przykład można utworzyć klasy EntityFrameworkMovieRepository lub SubSonicMovieRepository. Ponieważ klasa kontrolera jest zaprogramowane w taki sposób, interfejsu, nowe implementacja IMovieRepository można przekazać do klasy kontrolera i klasy będzie nadal działać.

Ponadto jeśli chcesz przetestować klasy MoviesController, następnie można przekazać fałszywych filmu klasę repozytorium do MoviesController. Można zaimplementować klasy IMovieRepository z klasy, która nie faktycznie dostępu do bazy danych, ale zawiera wszystkie wymagane metody interfejsu IMovieRepository. W ten sposób można testu jednostkowego klasy MoviesController bez faktycznie dostęp do rzeczywistej bazy danych.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było pokazują, jak można utworzyć klasy modelu MVC dzięki wykorzystaniu Microsoft LINQ do SQL. Firma Microsoft zbadać dwóch strategii do wyświetlania danych w bazie danych w aplikacji platformy ASP.NET MVC. Najpierw możemy utworzyć LINQ w klasach SQL i używane klasy bezpośrednio w ramach akcji kontrolera. Za pomocą LINQ w klasach SQL w kontrolerze umożliwia szybkie i łatwe wyświetlać dane z bazy danych w aplikacji MVC.

Następnie możemy przedstawione nieco trudniejsze, ale ostatecznie więcej virtuous ścieżki do wyświetlania danych w bazie danych. Firma Microsoft trwało zaletą wzorca repozytorium i umieszczenie wszystkich naszych logika dostępu do bazy danych w klasie oddzielne repozytorium. W naszym kontrolera napisaliśmy wszystkie naszego kodu interfejsu zamiast klasy konkretnej. Zaletą wzorca repozytorium jest czy pozwala na łatwe w przyszłości zmienić technologii dostępu do bazy danych i pozwala na łatwiejsze testowanie naszej klasy kontrolera.

> [!div class="step-by-step"]
> [Poprzednie](creating-model-classes-with-the-entity-framework-vb.md)
> [dalej](displaying-a-table-of-database-data-vb.md)
