---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu za pomocą kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 8d359de17bc35d0e14047ccd0589f080e0f949fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396121"
---
<a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

W tej sekcji utworzysz nową `MoviesController` klasy, a następnie napisać kod, który pobiera dane filmów i wyświetla go w przeglądarce, za pomocą szablonu widoku.

**Skompiluj aplikację** przed przejściem do następnego kroku. Nie można skompilować aplikację, otrzymasz błąd podczas dodawania kontrolera.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie kliknij przycisk **Dodaj**, następnie **kontrolera**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

W **Dodawanie szkieletu** okno dialogowe, kliknij przycisk **kontroler MVC 5 z widokami używający narzędzia Entity Framework**, a następnie kliknij przycisk **Dodaj**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Wybierz **Movie (MvcMovie.Models)** dla klasy modelu.
- Wybierz **MovieDBContext (MvcMovie.Models)** dla klasy kontekstu danych.
- Wprowadź nazwę kontrolera **MoviesController**.

  Na poniższej ilustracji przedstawiono okno dialogowe zakończone.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Kliknij przycisk **Dodaj**. (Jeśli wystąpi błąd, prawdopodobnie nie tworzysz aplikacji przed rozpoczęciem dodawania kontrolera.) Program Visual Studio tworzy następujące pliki i foldery:

- *MoviesController.cs* w pliku *kontrolerów* folderu.
- A *Views\Movies* folderu.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, i *Index.cshtml* w nowym *Views\Movies* folderu.

Visual Studio automatycznie utworzony [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoków (automatyczne tworzenie widoków i metod akcji CRUD jest znany jako funkcja szkieletów). Masz teraz aplikację internetową w pełni funkcjonalne, która umożliwia tworzenie, listy, edytować i usuwać wpisy filmu.

Uruchom aplikację, a następnie kliknij pozycję **filmu MVC** łącza (lub przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL w pasku adresu przeglądarki). Ponieważ aplikacja powołuje się na domyślny routing (zdefiniowane w *aplikacji\_Start\RouteConfig.cs* pliku), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowany do domyślnego `Index` metody akcji `Movies` kontrolera. Innymi słowy żądanie przeglądarki `http://localhost:xxxxx/Movies` skutecznie jest taka sama jak żądanie przeglądarki `http://localhost:xxxxx/Movies/Index`. Wynik jest pusta lista filmy, ponieważ nie dodano żadnego jeszcze.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz **Utwórz nowy** łącza. Wprowadź informacje na temat filmów, a następnie kliknij przycisk **Utwórz** przycisku.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Nie można wprowadzić w polu Cena kropki i przecinki. Do obsługi dotyczącą weryfikacji jQuery dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (&quot;,&quot;) dla punktu dziesiętnego i formaty daty inne niż angielski, należy wprowadzić *globalize.js* i konkretne  *cultures/globalize.cultures.js* pliku (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) i języka JavaScript, aby użyć `Globalize.parseFloat`. Czy mogę pokazano, jak to zrobić w następnym samouczku. Teraz po prostu wprowadź liczbami całkowitymi, takich jak 10.


Klikając **Utwórz** przycisku powoduje, że formularz do opublikowania na serwerze, gdzie informacje filmu są zapisywane w bazie danych. Użytkownik jest następnie przekierowywane do */Movies* adresu URL, w którym można zobaczyć nowo utworzoną filmu na liście.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Utwórz kilka więcej wpisów filmu. Spróbuj **Edytuj**, **szczegóły**, i **Usuń** łącza, które są wszystkie funkcjonalności.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz *Controllers\MoviesController.cs* plików i zbadaj wygenerowany `Index` metody. Część kontroler film z `Index` metoda znajdują się poniżej.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Żądanie `Movies` kontroler zwraca wszystkie wpisy w `Movies` tabeli, a następnie przekazuje wyniki do `Index` widoku. Poniższy wiersz z `MoviesController` klasy tworzy kontekst bazy danych filmów, zgodnie z wcześniejszym opisem. Kontekst bazy danych filmów umożliwia zapytania, edytowanie i usuwanie filmów.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silnie Typizowane modeli i @model — słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler można przekazać dane i obiekty za pomocą szablonu widoku `ViewBag` obiektu. `ViewBag` To obiekt dynamiczny, która zapewnia wygodny sposób z późnym wiązaniem do przekazywania informacji do widoku.

MVC udostępnia również możliwość przekazywania *silnie* typizowanych obiektów do szablonu widoku. To silnie typizowany podejście umożliwia lepsze kompilacji sprawdzania kodu i bardziej rozbudowane [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) w edytorze programu Visual Studio. Mechanizm tworzenia szkieletów w programie Visual Studio używane takie podejście (oznacza to, przekazując *silnie* typizowany model) z `MoviesController` klasy i Wyświetl szablony utworzenia metod i widoki.

W *Controllers\MoviesController.cs* pliku zbadać wygenerowany `Details` metody. `Details` Metoda znajdują się poniżej.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id` Parametr jest zwykle przekazywany jako dane trasy, na przykład `http://localhost:1234/movies/details/1` ustawi kontrolera do kontrolera filmu, działanie `details` i `id` 1. Można również przekazuje się w identyfikatorze o ciągu zapytania w następujący sposób:

`http://localhost:1234/movies/details?id=1`

Jeśli `Movie` zostanie znaleziony, wystąpienie `Movie` modelu jest przekazywany do `Details` widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Sprawdź zawartość *Views\Movies\Details.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Jeśli dołączysz `@model` instrukcji w górnej części pliku szablonu widoku, można określić typu obiektu, który oczekuje, że widok. Podczas tworzenia kontrolera filmu programu Visual Studio automatycznie uwzględnione następujące `@model` instrukcji na górze *Details.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

To `@model` dyrektywy umożliwia dostęp do filmów, która kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Details.cshtml* szablonu, kod przekazuje każdego pola film, aby `DisplayNameFor` i [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocników HTML za pomocą silnie typizowanej `Model` obiektu. `Create` i `Edit` metody i Wyświetl szablony też przekazać obiekt modelu filmu.

Sprawdź *Index.cshtml* Wyświetl szablon i `Index` method in Class metoda *MoviesController.cs* pliku. Zwróć uwagę, jak kod tworzy [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) obiektu, kiedy wywoływanych przez nią `View` metody pomocnika w `Index` metody akcji. Kod następnie przekazuje to `Movies` listy z `Index` metody akcji do widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Podczas tworzenia kontrolera filmu programu Visual Studio automatycznie uwzględnione następujące `@model` instrukcji na górze *Index.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

To `@model` dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Index.cshtml* szablonu, kod w pętli filmy wykonując `foreach` instrukcji na silnie typizowaną `Model` obiektu:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Ponieważ `Model` obiektu zdecydowanie jest wpisane (jako `IEnumerable<Movie>` obiektu), każdy `item` obiektu w pętli jest wpisana jako `Movie`. Wśród innych korzyści oznacza to, możesz uzyskać w czasie kompilacji sprawdzania kodu i pełną obsługę technologii IntelliSense w edytorze kodu:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Praca z bazą danych LocalDB programu SQL Server

Entity Framework Code First wykrył, że parametry połączenia bazy danych, który został dostarczony wskazywany `Movies` bazy danych, która nie istnieje jeszcze, więc Code First baza danych utworzona automatycznie. Możesz sprawdzić, czy został on utworzony przez wyszukiwanie *aplikacji\_danych* folderu. Jeśli nie widzisz *Movies.mdf* plików, kliknij **Pokaż wszystkie pliki** znajdujący się w **Eksploratora rozwiązań** narzędzi, kliknij przycisk **Odśwież** przycisk, a następnie rozwiń *aplikacji\_danych* folderu.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Kliknij dwukrotnie *Movies.mdf* otworzyć **EKSPLORATORA serwera**, następnie rozwiń **tabel** folder, aby wyświetlić tabelę filmów. Należy pamiętać, ikonę klucza, obok identyfikatora. Domyślnie program EF spowoduje, że właściwość o nazwie identyfikator klucza podstawowego. Aby uzyskać więcej informacji na temat struktury jednostek i MVC, zobacz samouczek doskonałą Tom Dykstra na [MVC i programem EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Kliknij prawym przyciskiem myszy `Movies` tabeli, a następnie wybierz pozycję **Pokaż dane tabeli** do wyświetlenia danych został utworzony.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Kliknij prawym przyciskiem myszy `Movies` tabeli, a następnie wybierz pozycję **Otwórz definicję tabeli** do znajdują się w tabeli struktury tego Entity Framework Code First utworzone automatycznie.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Zwróć uwagę jak schemat `Movies` mapy do tabel `Movie` klasa została utworzona wcześniej. Entity Framework Code First automatycznie tworzony w tym schemacie na podstawie Twojej `Movie` klasy.

Gdy skończysz, zamknij połączenie przez kliknięcie prawym przyciskiem myszy *MovieDBContext* i wybierając polecenie **zamknij połączenie**. (Jeśli nie zamkniesz połączenie, możesz otrzymać błąd przy następnym uruchomieniu projektu).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Masz teraz bazę danych i strony do wyświetlenia, edytowanie, aktualizowanie i usuwanie danych. W następnym samouczku utworzymy Sprawdź pozostałą część utworzony szkielet kodu i dodamy `SearchIndex` metody i `SearchIndex` widok, który umożliwia wyszukiwanie filmów w tej bazie danych. Aby uzyskać więcej informacji na temat korzystania z programu Entity Framework z MVC, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-connection-string.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)
