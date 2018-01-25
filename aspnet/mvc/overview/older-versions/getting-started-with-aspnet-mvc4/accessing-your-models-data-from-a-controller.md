---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: "Uzyskiwanie dostępu do modelu danych z kontrolera | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f323fe37da739d957a609dc7ca4e71a3c3ab475e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do modelu danych z kontrolera
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji utworzysz nową `MoviesController` klasy i pisania kodu, który pobiera dane filmu i wyświetla je w przeglądarce, za pomocą szablonu widoku.

**Tworzenie aplikacji** przed przejściem do następnego kroku.

Kliknij prawym przyciskiem myszy *kontrolerów* folderze i utworzyć nową `MoviesController` kontrolera. Poniższe opcje będą widoczne dopiero skompilować aplikację. Wybierz następujące opcje:

- Nazwa kontrolera: **MoviesController**. (Jest to wartość domyślna. )
- Szablon: **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework**.
- Klasa modelu: **Movie (MvcMovie.Models)**.
- Klasa kontekstu danych: **MovieDBContext (MvcMovie.Models)**.
- Widoki: **Razor (CSHTML)**. (Ustawienie domyślne).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Kliknij przycisk **Dodaj**. Visual Studio Express tworzy następujące pliki i foldery:

- *MoviesController.cs* plik do projektu *kontrolerów* folderu.
- A *filmy* folderu do projektu *widoków* folderu.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, i *Index.cshtml* w nowym *Views\Movies* folderu.

ASP.NET MVC 4 tworzone automatycznie CRUD (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoków (automatyczne tworzenie widoków i metod akcji CRUD nosi nazwę szkieletów). Masz teraz aplikacji sieci web w pełni funkcjonalne, która umożliwia tworzenie, listy, edytować i usuwać wpisy filmu.

Uruchom aplikację i przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL na pasku adresu przeglądarki. Ponieważ aplikacja jest polegania na domyślny routing (zdefiniowany w *Global.asax* pliku), żądanie `http://localhost:xxxxx/Movies` jest kierowany do domyślnej `Index` metody akcji `Movies` kontrolera. Innymi słowy żądanie `http://localhost:xxxxx/Movies` jest taka sama jak żądanie `http://localhost:xxxxx/Movies/Index`. Wynik jest wyświetlana pusta lista filmów, ponieważ nie dodano żadnych serwerów jeszcze.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz **Utwórz nowy** łącza. Wprowadź niektóre szczegóły na temat film, a następnie kliknij przycisk **Utwórz** przycisku.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Kliknięcie przycisku **Utwórz** kliknięcie przycisku powoduje formularza można opublikować na serwerze, gdzie informacje filmu są zapisywane w bazie danych. Użytkownik jest przekierowywane do */Movies* adres URL, w którym można obejrzeć film nowo utworzony na liście.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Utwórz kilka więcej wpisów filmu. Spróbuj **Edytuj**, **szczegóły**, i **usunąć** łącza, które są wszystkie funkcjonalności.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz *Controllers\MoviesController.cs* pliku i sprawdź, czy wygenerowany `Index` metody. Część kontroler film z `Index` metody są wyświetlane poniżej.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Poniższy wiersz z `MoviesController` klasy tworzy filmu kontekst bazy danych, jak opisano wcześniej. Film kontekst bazy danych służy do zapytań, edytowanie i usuwanie filmów.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Żądanie `Movies` kontrolera zwraca wszystkie wpisy w `Movies` tabeli filmu bazy danych, a następnie przekazuje wyniki w `Index` widoku.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silnie Typizowane modeli i @model — słowo kluczowe

Wcześniej w tym samouczku widać, jak kontroler może przekazać dane i obiekty do szablonu widoku przy użyciu `ViewBag` obiektu. `ViewBag` Jest to obiekt dynamiczny, które oferują wygodny sposób późnym wiązaniem do przekazywania informacji do widoku.

ASP.NET MVC dostarcza również możliwość przekazywania silnie typizowanej danych lub obiektów do szablonu widoku. To silnie typizowane metody umożliwia lepsze kompilacji Sprawdzanie kodu i bardziej zaawansowane funkcje IntelliSense w edytorze programu Visual Studio. Takie podejście z używany mechanizm szkieletów w programie Visual Studio `MoviesController` szablonów klasy i widoku podczas jego tworzenia widoków i metod.

W *Controllers\MoviesController.cs* zbadać wygenerowany plik `Details` metody. Część kontroler film z `Details` metody są wyświetlane poniżej.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Jeśli `Movie` zostanie znaleziony, wystąpienie `Movie` modelu są przekazywane do widoku szczegółów. Sprawdź zawartość *Views\Movies\Details.cshtml* pliku.

W tym `@model` instrukcji w górnej części pliku szablonu widoku, można określić typu obiektu, który oczekuje widoku. Podczas tworzenia kontrolera film, Visual Studio automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Details.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

To `@model` dyrektywy umożliwia dostęp do filmów, który kontroler przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Details.cshtml* szablonu, kod przekazuje każde pole film, aby `DisplayNameFor` i [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocników HTML z silnie typizowaną `Model` obiektu. Tworzenie i edytowanie metody i Wyświetl szablony również przekazać film obiekt modelu.

Sprawdź *Index.cshtml* Wyświetl szablon i `Index` metody w *MoviesController.cs* pliku. Zwróć uwagę, jak kod tworzy [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) obiektu, gdy wywołuje `View` metody pomocnika w `Index` metody akcji. Kod następnie przekazuje to `Movies` listy z kontrolera do widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Podczas tworzenia kontrolera film, Visual Studio Express automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Index.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

To `@model` dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Index.cshtml* szablonu, kod w pętli filmów za pomocą ćwiczeń `foreach` instrukcji na silnie typizowaną `Model` obiektu:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Ponieważ `Model` obiektu jest silnie typizowane (jako `IEnumerable<Movie>` obiektu), każdy `item` jest typu obiektu w pętli `Movie`. Wśród innych korzyści oznacza to, Pobierz sprawdzanie kompilacji kodu, a następnie pełne obsługę funkcji IntelliSense w edytorze kodu:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Praca z bazy danych LocalDB programu SQL Server

Entity Framework Code First wykrył, że podany ciąg połączenia bazy danych wskazywanego `Movies` bazy danych, która nie istnieje jeszcze, więc Code First baza danych utworzona automatycznie. Możesz sprawdzić, czy jest on utworzony przez wyszukiwanie *aplikacji\_danych* folderu. Jeśli nie widzisz *Movies.mdf* plików, kliknij **Pokaż wszystkie pliki** przycisk **Eksploratora rozwiązań** narzędzi, kliknij przycisk **Odśwież** przycisk, a następnie rozwiń *aplikacji\_danych* folderu.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Kliknij dwukrotnie *Movies.mdf* otworzyć **EKSPLORATORA bazy danych**, następnie rozwiń węzeł **tabel** folder, aby wyświetlić tabelę filmów.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Jeśli nie ma pomocy Eksploratora bazy danych, z **narzędzia** menu, wybierz **Połącz z bazą danych**, następnie Anuluj **wybierz źródło danych** okna dialogowego. Spowoduje to wymuszenie Otwórz Eksploratora bazy danych.


> [!NOTE]
> Jeśli używasz VWD lub Visual Studio 2010 i wystąpi błąd podobny do następującego następujących:
> 
> - Bazy danych "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF "nie można otworzyć, ponieważ jest wersja 706. Ten serwer obsługuje wersję 655 i wcześniejszych. Na starszą nie jest obsługiwane.
> - &quot;InvalidOperation wyjątek został obsłużony przez kod użytkownika&quot; dostarczony parametr SqlConnection nie określa wykazu początkowego.
> 
> Musisz zainstalować [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) i [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Sprawdź `MovieDBContext` parametrów połączenia określonych na poprzedniej stronie.


Kliknij prawym przyciskiem myszy `Movies` tabeli i wybierz **Pokaż dane tabeli** do wyświetlania danych został utworzony.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Kliknij prawym przyciskiem myszy `Movies` tabeli i wybierz **Otwórz definicję tabeli** do tabeli struktury tego Entity Framework Code First utworzone automatycznie.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Powiadomienie jak schemat `Movies` mapy do tabel `Movie` klasy utworzony wcześniej. Entity Framework Code First automatycznie utworzyć ten schemat na podstawie Twojej `Movie` klasy.

Po zakończeniu zamknij połączenie przez kliknięcie prawym przyciskiem myszy *MovieDBContext* i wybierając **zamknąć połączenia**. (Jeśli nie zamkniesz połączenie, może być błąd pojawia się przy następnym uruchomieniu projektu).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Istnieje już baza danych i proste listy na stronie zawartości z niego. W następnym samouczku będzie firma Microsoft bada reszty szkieletu kodu i dodać `SearchIndex` — metoda i `SearchIndex` widoku, który umożliwia wyszukiwanie filmów w tej bazie danych.

>[!div class="step-by-step"]
[Poprzednie](adding-a-model.md)
[dalej](examining-the-edit-methods-and-edit-view.md)
