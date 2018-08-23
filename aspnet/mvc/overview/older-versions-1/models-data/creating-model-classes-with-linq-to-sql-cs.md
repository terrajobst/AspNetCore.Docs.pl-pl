---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Tworzenie klas modelu za pomocą LINQ to SQL (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Celem tego samouczka jest aby wyjaśnić, jedną z metod Tworzenie klas modelu dla aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak tworzyć c modelu...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: c76e92ebfab1db162151ea5753888a60a4d54e59
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755374"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Tworzenie klas modelu za pomocą LINQ to SQL (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Celem tego samouczka jest aby wyjaśnić, jedną z metod Tworzenie klas modelu dla aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak tworzenie klas modelu i wykonywać dostępu do bazy danych, wykorzystując Microsoft LINQ to SQL.


Celem tego samouczka jest aby wyjaśnić, jedną z metod Tworzenie klas modelu dla aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak tworzenie klas modelu i wykonywać dostępu do bazy danych, wykorzystując Microsoft LINQ to SQL

W tym samouczku wbudowujemy podstawowa aplikacji bazy danych filmów. Rozpoczniemy pracę przez tworzenie aplikacji bazy danych filmów w najszybszy i najłatwiejszy sposób możliwe. Firma Microsoft wykonaj wszystkie nasze dostępu do danych bezpośrednio z naszych akcji kontrolera.

Następnie dowiesz się, jak użyć wzorca repozytorium. Przy użyciu wzorca repozytorium wymaga nieco więcej pracy. Jednak zaletą przyjęcie tego wzorca jest pozwala tworzyć aplikacje, które są pasujący do zmiany i można łatwo przetestować.

## <a name="what-is-a-model-class"></a>Co to jest klasa modelu?

Modelu MVC zawiera wszystkie logiki aplikacji, który nie jest zawarty w widoku składnika MVC lub kontroler MVC. W szczególności modelu MVC zawiera wszystkich aplikacji biznesowych i logiką dostępu do danych.

Aby zaimplementować logikę dostępu do danych, można użyć szereg różnych technologii. Można na przykład, utworzyć Twoje klas dostępu do danych przy użyciu klas Microsoft Entity Framework, NHibernate, Subsonic lub ADO.NET.

W tym samouczku używam programu LINQ to SQL do wykonywania zapytań i aktualizacji bazy danych. LINQ do SQL zapewnia bardzo prosty sposób interakcji z bazą danych programu Microsoft SQL Server. Jednak ważne jest zrozumienie, że platforma ASP.NET MVC nie jest związany z LINQ to SQL w dowolny sposób. ASP.NET MVC jest zgodny z dowolnej technologii dostępu do danych.

## <a name="create-a-movie-database"></a>Tworzenie bazy danych filmów

W tym samouczku — aby zilustrować Tworzenie klas modelu — możemy utworzyć prostą aplikację bazy danych filmów. Pierwszym krokiem jest utworzenie nowej bazy danych. Kliknij prawym przyciskiem myszy aplikację\_folderu danych w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**. Wybierz **bazy danych SQL Server** szablonu, nadaj mu nazwę MoviesDB.mdf, a następnie kliknij przycisk **Dodaj** przycisku (patrz rysunek 1).


[![Dodawanie nowej bazy danych serwera SQL](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Rysunek 01**: dodanie nowej bazy danych serwera SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Po utworzeniu nowej bazy danych, można otworzyć bazy danych, klikając dwukrotnie plik MoviesDB.mdf w aplikacji\_folderu danych. Dwukrotne kliknięcie pliku MoviesDB.mdf zostanie otwarte okno Eksplorator serwera (zobacz rysunek 2).

Okno Eksploratora serwera jest wywoływana okno Eksplorator bazy danych, korzystając z programu Visual Web Developer.


[![W oknie Eksploratora serwera](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Rysunek 02**: Korzystanie z okna Eksploratora serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Musimy dodać jednej tabeli do naszych bazy danych, która reprezentuje nasze filmy. Kliknij prawym przyciskiem myszy folder Tabele i wybierz opcję menu **Dodaj nową tabelę**. Wybranie tej opcji menu zostanie otwarty projektant tabel (zobacz rysunek 3).


[![W oknie Eksploratora serwera](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Rysunek 03**: projektanta tabel ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Należy dodać następujące kolumny do tabeli bazy danych:

| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Id | int | False |
| Tytuł | Nvarchar(200) | False |
| Dyrektor ds. | nvarchar(50) | False |

Należy wykonać dwie czynności specjalne do kolumny identyfikatora. Najpierw należy oznaczyć kolumna identyfikatora jako kolumna klucza podstawowego, wybierając kolumnę w Projektancie tabel i klikając ikonę klucza. LINQ do SQL, należy określić klucz podstawowy, podczas wykonywania wstawia lub aktualizuje w bazie danych.

Następnie należy oznaczyć kolumna identyfikatora jako kolumnę tożsamości, przypisując wartość tak, aby **tożsamości jest** właściwości (zobacz rysunek 3). Kolumna tożsamości to kolumna, która jest przypisywany nowy numer po każdym dodaniu nowego wiersza danych do tabeli.

## <a name="create-linq-to-sql-classes"></a>Tworzenie klasy LINQ do SQL

Nasz model MVC będzie zawierać LINQ do klas SQL, które reprezentują tblMovie tabeli bazy danych. Najprostszym sposobem utworzenia te klasy programu LINQ to SQL ma kliknij prawym przyciskiem myszy folderu modeli, wybierz **Dodaj, nowy element**, wybierz LINQ do klas SQL szablonu, nazwę klasy Movie.dbml, a następnie kliknij przycisk **Dodaj**przycisku (zobacz rysunek 4).


[![Tworzenie zapytań LINQ do klas SQL](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Rysunek 04**: Tworzenie klasy programu LINQ to SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Bezpośrednio po utworzeniu filmu klasy programu LINQ to SQL, zostanie wyświetlony Object Relational Designer. Można przeciągnąć tabele bazy danych z okna Eksploratora serwera na Object Relational Designer, aby tworzyć LINQ do klas SQL, które reprezentują tabele określonej bazy danych. Musimy dodać tabelę bazy danych tblMovie na Object Relational Designer (zobacz rysunek 5).


[![Za pomocą Object Relational Designer](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Rysunek 05**: przy użyciu Object Relational Designer ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Domyślnie Object Relational Designer tworzy klasę o nazwie bardzo identycznej z nazwą tabeli bazy danych, która została przeciągnięta na projektanta. Jednak firma Microsoft nie chcesz wywołać klasy nasze `tblMovie`. W związku z tym kliknij nazwę klasy w Projektancie i Zmień nazwę klasy do filmu.

Na koniec należy pamiętać o kliknij **Zapisz** przycisk (obraz dyskietek), aby zapisać LINQ do klas SQL. W przeciwnym razie klasy programu LINQ to SQL nie będą generowane przez Object Relational Designer.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Za pomocą LINQ to SQL za pomocą akcji kontrolera

Teraz gdy mamy nasz klasy programu LINQ to SQL, możemy użyć tych klas do pobierania danych z bazy danych. W tej sekcji dowiesz się, jak używać programu LINQ do klas SQL bezpośrednio w ramach akcji kontrolera. Na liście filmów z tabeli bazy danych tblMovies będzie wyświetlana w widoku MVC.

Najpierw należy zmodyfikować klasy HomeController. Ta klasa znajdują się w folderze kontrolery aplikacji. Modyfikuj klasę, tak wygląda jak klasa w ofercie 1.

**1 — Lista `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()` Akcja w ofercie 1 używa składnika LINQ to SQL DataContext klasy ( `MovieDataContext`) do reprezentowania `MoviesDB` bazy danych. `MoveDataContext` Klasy został wygenerowany przez program Visual Studio Object Relational Designer.

Kwerenda LINQ nie jest przeprowadzana DataContext, aby pobrać wszystkie filmy z `tblMovies` tabeli bazy danych. Na liście filmów jest przypisany do zmiennej lokalnej o nazwie `movies`. Na koniec listy filmów jest przekazywane do widoku danych widoku.

Aby pokazać filmy, obok potrzebujemy do modyfikowania widoku indeksu. Można znaleźć w widoku indeksu `Views\Home\` folderu. Aktualizacja widoku indeks, tak aby wygląda jak widok w ofercie 2.

**2 — Lista `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Należy zauważyć, że zmodyfikowany widok indeksu zawiera `<%@ import namespace %>` dyrektywę w górnej części widoku. Ta dyrektywa importuje `MvcApplication1.Models namespace`. Potrzebujemy tej przestrzeni nazw, aby pracować z `model` klasy — w szczególności `Movie` klasy — w widoku.

Widok w ofercie 2 zawiera `foreach` pętlę iterującą przez wszystkie elementy reprezentowane przez `ViewData.Model` właściwości. Wartość `Title` właściwość jest wyświetlana dla każdego `movie`.

Należy zauważyć, że wartość `ViewData.Model` właściwość jest rzutowany na `IEnumerable`. Jest to konieczne, aby przeglądać zawartość `ViewData.Model`. W tym miejscu innym rozwiązaniem jest utworzenie, silnie typizowanego `view`. Podczas tworzenia silnie typizowanego `view`, można rzutować `ViewData.Model` właściwości do określonego typu w klasie CodeBehind widoku.

Po uruchomieniu aplikacji po zmodyfikowaniu `HomeController` klasy i indeksu w widoku, a następnie zostanie wyświetlona pusta strona. Otrzymasz pustą stronę, ponieważ nie ma żadnych rekordów filmu w `tblMovies` tabeli bazy danych.

Aby można było dodać rekordy `tblMovies` bazy danych tabeli, kliknij prawym przyciskiem myszy `tblMovies` bazy danych tabeli w oknie Eksploratora serwera (Okno Eksplorator bazy danych w Visual Web Developer) i wybierz opcję menu Pokaż dane tabeli. Możesz wstawić `movie` rekordów przy użyciu siatki, która pojawia się (patrz rysunek 6).


[![Wstawianie filmów](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Rysunek 06**: Wstawianie filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Po dodaniu kilka rekordów bazy danych, aby `tblMovies` tabeli i uruchomić aplikację, więc zostanie wyświetlona strona na rysunku 7. Wszystkie rekordy bazy danych filmów są wyświetlane na liście punktowanej.


[![Wyświetlanie filmów z widoku indeksu](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Rysunek 07**: wyświetlanie filmów z widoku indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Przy użyciu wzorca repozytorium

W poprzedniej sekcji użyliśmy LINQ do klas SQL bezpośrednio w ramach akcji kontrolera. Użyliśmy `MovieDataContext` klasy bezpośrednio z `Index()` akcji kontrolera. Nie ma problem z takiego postępowania w przypadku prostej aplikacji. Jednak pracy bezpośrednio za pomocą LINQ to SQL w klasie controller stwarza problemy, gdy potrzebne do utworzenia bardziej złożonych aplikacji.

Za pomocą LINQ to SQL w obrębie klasy kontrolera utrudnia Przełącz technologii dostępu do danych w przyszłości. Na przykład można zdecydować przełączyć się z przy użyciu Microsoft Entity Framework jako technologii dostępu do danych za pomocą Microsoft LINQ to SQL. W takim przypadku należy zmodyfikować każdy kontroler, który uzyskuje dostęp do bazy danych w aplikacji.

Za pomocą LINQ to SQL w obrębie klasy kontrolera również utrudnia do tworzenia testów jednostkowych dla aplikacji. Zwykle nie mają wchodzić w interakcje z bazą danych podczas przeprowadzania testów jednostkowych. Chcesz przetestować logika aplikacji i nie jest serwer bazy danych za pomocą testów jednostkowych.

Aby można było tworzyć aplikację MVC bardziej pasujący do przyszłych zmian i który można łatwo przetestować, należy rozważyć użycie wzorca repozytorium. Korzystając z wzorca repozytorium, utworzysz klasy osobnym repozytorium, która zawiera całą logikę dostępu do bazy danych.

Kiedy tworzysz klasę repozytorium, utworzysz interfejs, który reprezentuje wszystkie metody używane przez klasę repozytorium. W ramach kontrolerach piszesz kod interfejsu, a nie repozytorium. W ten sposób można zaimplementować repozytorium przy użyciu technologii dostępu do danych w przyszłości.

Nosi nazwę interfejsu w ofercie 3 `IMovieRepository` i reprezentuje jedną metodę o nazwie `ListAll()`.

**3 — lista `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

Implementuje klasę repozytorium w ofercie 4 `IMovieRepository` interfejsu. Zwróć uwagę, że zawiera on metodę o nazwie `ListAll()` odnosi się do metody, wymagane przez `IMovieRepository` interfejsu.

**4 — lista `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Na koniec `MoviesController` klasy w ofercie 5 zastosowanie wzorca repozytorium. Nie jest już używa programu LINQ do klas SQL bezpośrednio.

**5 — lista `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Należy zauważyć, że `MoviesController` klasa w ofercie 5 ma dwa konstruktory. Pierwszy Konstruktor konstruktora bez parametrów jest wywoływana, gdy aplikacja jest uruchomiona. Ten konstruktor tworzy wystąpienie `MovieRepository` klasy i przekazuje go do konstruktora drugiego.

Drugi Konstruktor ma jeden parametr: `IMovieRepository` parametru. Ten konstruktor po prostu przypisuje wartość parametru na poziomie klasy pole o nazwie `_repository`.

`MoviesController` Klasy jest wykorzystując wzorzec projektowania oprogramowania o nazwie wzorzec iniekcji zależności. W szczególności wykorzystywana jest coś, co wywołuje konstruktor iniekcji zależności. Możesz dowiedzieć się więcej o tego wzorca, zapoznając się z następującym artykułem Martina fowlera:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Należy zauważyć, że cały kod w `MoviesController` klasy (z wyjątkiem pierwszy Konstruktor) wchodzi w interakcję z `IMovieRepository` interfejsu, a nie rzeczywiste `MovieRepository` klasy. Kod współdziała z abstrakcyjny interfejs, a nie konkretną implementację interfejsu.

Jeśli chcesz zmodyfikować technologii dostępu do danych, które są używane przez aplikację, a następnie po prostu można zaimplementować `IMovieRepository` interfejsu z klasą, która używa technologii dostępu do alternatywnej bazy danych. Na przykład można utworzyć `EntityFrameworkMovieRepository` klasy lub `SubSonicMovieRepository` klasy. Ponieważ klasa kontrolera jest programowane względem interfejsu, możesz przekazać nową metodę implementacji `IMovieRepository` do kontrolera klasy i klasa będzie nadal działać.

Ponadto jeśli chcesz przetestować `MoviesController` klasy, a następnie możesz przekazać filmu fałszywych klasy repozytorium do `HomeController`. Możesz zaimplementować `IMovieRepository` klasy z klasą, która nie jest faktycznie dostępu do bazy danych, ale zawiera wszystkie wymagane metod `IMovieRepository` interfejsu. W ten sposób możesz test jednostkowy `MoviesController` klasy bez faktycznego uzyskiwania dostępu do rzeczywistego bazy danych.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było pokazują, jak można tworzyć klasy modelu MVC, wykorzystując Microsoft LINQ to SQL. Zbadaliśmy pomocy dwóch strategii do wyświetlania danych z baz danych w aplikacji ASP.NET MVC. Najpierw możemy utworzyć LINQ do klas SQL i używane klasy bezpośrednio w ramach akcji kontrolera. Za pomocą LINQ do klas SQL w ramach kontrolera pozwala szybko i łatwo wyświetlać dane z bazy danych w aplikacji MVC.

Następnie rozważyliśmy nieco trudniejsze, ale zdecydowanie więcej składa ścieżki do wyświetlania danych z baz danych. Firma Microsoft skorzystała wzorca repozytorium i umieścić wszystkie nasze logiką dostępu do bazy danych w klasie osobnym repozytorium. W naszym kontrolera napisaliśmy wszystkich naszych kodować interfejs zamiast klasy konkretnej. Zaletą wzorca repozytorium jest, że dzięki temu możemy łatwo zmienić w przyszłych technologii dostępu do bazy danych i pozwala na łatwe testowanie działania naszych zajęć kontrolera.

> [!div class="step-by-step"]
> [Poprzednie](creating-model-classes-with-the-entity-framework-cs.md)
> [dalej](displaying-a-table-of-database-data-cs.md)
