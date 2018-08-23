---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu za pomocą kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2b5747f49a31a6f3559609bbae765025e43c650b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754847"
---
<a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji utworzysz nową `MoviesController` klasy, a następnie napisać kod, który pobiera dane filmów i wyświetla go w przeglądarce, za pomocą szablonu widoku.

**Skompiluj aplikację** przed przejściem do następnego kroku.

Kliknij prawym przyciskiem myszy *kontrolerów* folderze i utworzyć nową `MoviesController` kontrolera. Poniższe opcje nie będą wyświetlane do momentu, gdy kompilujesz aplikację. Wybierz następujące opcje:

- Nazwa kontrolera: **MoviesController**. (Jest to wartość domyślna. )
- Szablon: **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework**.
- Klasa modelu: **Movie (MvcMovie.Models)**.
- Klasa kontekstu danych: **MovieDBContext (MvcMovie.Models)**.
- Widoki: **Razor (CSHTML)**. (Ustawienie domyślne).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Kliknij przycisk **Dodaj**. Visual Studio Express tworzy następujące pliki i foldery:

- *MoviesController.cs* pliku w projekcie *kontrolerów* folderu.
- A *filmy* folderu w projekcie *widoków* folderu.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, i *Index.cshtml* w nowym *Views\Movies* folderu.

ASP.NET MVC 4 jest automatycznie tworzony CRUD (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoków (automatyczne tworzenie widoków i metod akcji CRUD jest znany jako funkcja szkieletów). Masz teraz aplikację internetową w pełni funkcjonalne, która umożliwia tworzenie, listy, edytować i usuwać wpisy filmu.

Uruchom aplikację, a następnie przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL w pasku adresu przeglądarki. Ponieważ aplikacja powołuje się na domyślny routing (zdefiniowane w *Global.asax* pliku), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowany do domyślnego `Index` metody akcji `Movies` kontrolera. Innymi słowy żądanie przeglądarki `http://localhost:xxxxx/Movies` skutecznie jest taka sama jak żądanie przeglądarki `http://localhost:xxxxx/Movies/Index`. Wynik jest pusta lista filmy, ponieważ nie dodano żadnego jeszcze.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz **Utwórz nowy** łącza. Wprowadź informacje na temat filmów, a następnie kliknij przycisk **Utwórz** przycisku.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Klikając **Utwórz** przycisku powoduje, że formularz do opublikowania na serwerze, gdzie informacje filmu są zapisywane w bazie danych. Użytkownik jest następnie przekierowywane do */Movies* adresu URL, w którym można zobaczyć nowo utworzoną filmu na liście.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Utwórz kilka więcej wpisów filmu. Spróbuj **Edytuj**, **szczegóły**, i **Usuń** łącza, które są wszystkie funkcjonalności.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz *Controllers\MoviesController.cs* plików i zbadaj wygenerowany `Index` metody. Część kontroler film z `Index` metoda znajdują się poniżej.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Poniższy wiersz z `MoviesController` klasy tworzy kontekst bazy danych filmów, zgodnie z wcześniejszym opisem. Kontekst bazy danych filmów umożliwia zapytania, edytowanie i usuwanie filmów.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Żądanie `Movies` kontroler zwraca wszystkie wpisy w `Movies` tabeli bazy danych filmów, a następnie przekazuje wyniki do `Index` widoku.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silnie Typizowane modeli i @model — słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler można przekazać dane i obiekty za pomocą szablonu widoku `ViewBag` obiektu. `ViewBag` To obiekt dynamiczny, która zapewnia wygodny sposób z późnym wiązaniem do przekazywania informacji do widoku.

ASP.NET MVC udostępnia również możliwość przekazywania silnie typizowane dane i obiekty w szablonie widoku. Silnie typizowane to podejście umożliwia lepsze kompilacji Sprawdzanie kodu oraz ulepszoną funkcję IntelliSense w edytorze programu Visual Studio. Mechanizm tworzenia szkieletów w programie Visual Studio używane z tym podejściem `MoviesController` klasy i Wyświetl szablony utworzenia metod i widoki.

W *Controllers\MoviesController.cs* pliku zbadać wygenerowany `Details` metody. Część kontroler film z `Details` metoda znajdują się poniżej.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Jeśli `Movie` zostanie znaleziony, wystąpienie `Movie` modelu jest przekazywana do widoku szczegółów. Sprawdź zawartość *Views\Movies\Details.cshtml* pliku.

Jeśli dołączysz `@model` instrukcji w górnej części pliku szablonu widoku, można określić typu obiektu, który oczekuje, że widok. Podczas tworzenia kontrolera filmu programu Visual Studio automatycznie uwzględnione następujące `@model` instrukcji na górze *Details.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

To `@model` dyrektywy umożliwia dostęp do filmów, która kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Details.cshtml* szablonu, kod przekazuje każdego pola film, aby `DisplayNameFor` i [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocników HTML za pomocą silnie typizowanej `Model` obiektu. Metody tworzenia i edycji i Wyświetl szablony też przekazać obiekt modelu filmu.

Sprawdź *Index.cshtml* Wyświetl szablon i `Index` method in Class metoda *MoviesController.cs* pliku. Zwróć uwagę, jak kod tworzy [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) obiektu, kiedy wywoływanych przez nią `View` metody pomocnika w `Index` metody akcji. Kod następnie przekazuje to `Movies` listy z kontrolera do widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Podczas tworzenia kontrolera filmu, Visual Studio Express automatycznie uwzględnione następujące `@model` instrukcji na górze *Index.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

To `@model` dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Index.cshtml* szablonu, kod w pętli filmy wykonując `foreach` instrukcji na silnie typizowaną `Model` obiektu:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Ponieważ `Model` obiektu zdecydowanie jest wpisane (jako `IEnumerable<Movie>` obiektu), każdy `item` obiektu w pętli jest wpisana jako `Movie`. Wśród innych korzyści oznacza to, możesz uzyskać w czasie kompilacji sprawdzania kodu i pełną obsługę technologii IntelliSense w edytorze kodu:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Praca z bazą danych LocalDB programu SQL Server

Entity Framework Code First wykrył, że parametry połączenia bazy danych, który został dostarczony wskazywany `Movies` bazy danych, która nie istnieje jeszcze, więc Code First baza danych utworzona automatycznie. Możesz sprawdzić, czy został on utworzony przez wyszukiwanie *aplikacji\_danych* folderu. Jeśli nie widzisz *Movies.mdf* plików, kliknij **Pokaż wszystkie pliki** znajdujący się w **Eksploratora rozwiązań** narzędzi, kliknij przycisk **Odśwież** przycisk, a następnie rozwiń *aplikacji\_danych* folderu.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Kliknij dwukrotnie *Movies.mdf* otworzyć **EKSPLORATOR bazy danych**, następnie rozwiń **tabel** folder, aby wyświetlić tabelę filmów.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Jeśli Eksplorator bazy danych nie jest wyświetlane, z **narzędzia** menu, wybierz opcję **Połącz z bazą danych**, następnie Anuluj **wybierz źródło danych** okna dialogowego. Spowoduje to wymuszenie Otwórz Eksplorator bazy danych.


> [!NOTE]
> Jeśli używasz VWD lub Visual Studio 2010 i otrzymać błąd podobny do dowolnego z następujących następujące czynności:
> 
> - Baza danych "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF "nie można otworzyć, ponieważ jest wersja 706. Ten serwer obsługuje wersję 655 i starszych. Starszą nie jest obsługiwana.
> - &quot;Invalidoperation powodujący zakończenie działania wyjątek został obsłużony przez kod użytkownika&quot; dostarczony parametr SqlConnection nie określa wykazu początkowego.
> 
> Musisz zainstalować [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) i [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Sprawdź `MovieDBContext` parametry połączenia określone na poprzedniej stronie.


Kliknij prawym przyciskiem myszy `Movies` tabeli, a następnie wybierz pozycję **Pokaż dane tabeli** do wyświetlenia danych został utworzony.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Kliknij prawym przyciskiem myszy `Movies` tabeli, a następnie wybierz pozycję **Otwórz definicję tabeli** do znajdują się w tabeli struktury tego Entity Framework Code First utworzone automatycznie.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Zwróć uwagę jak schemat `Movies` mapy do tabel `Movie` klasa została utworzona wcześniej. Entity Framework Code First automatycznie tworzony w tym schemacie na podstawie Twojej `Movie` klasy.

Gdy skończysz, zamknij połączenie przez kliknięcie prawym przyciskiem myszy *MovieDBContext* i wybierając polecenie **zamknij połączenie**. (Jeśli nie zamkniesz połączenie, możesz otrzymać błąd przy następnym uruchomieniu projektu).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Masz teraz bazę danych i prostego strony do wyświetlania zawartości z niego. W następnym samouczku utworzymy Sprawdź pozostałą część utworzony szkielet kodu i dodamy `SearchIndex` metody i `SearchIndex` widok, który umożliwia wyszukiwanie filmów w tej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-model.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)
