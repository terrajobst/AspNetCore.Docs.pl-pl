---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: "Uzyskiwanie dostępu do modelu danych z kontrolera | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b60913cef4b62745cf167e6074834bf7d0c228d1
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/19/2017
---
<a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do modelu danych z kontrolera
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

W tej sekcji utworzysz nową `MoviesController` klasy i pisania kodu, który pobiera dane filmu i wyświetla je w przeglądarce, za pomocą szablonu widoku.

**Tworzenie aplikacji** przed przejściem do następnego kroku. Jeśli nie można utworzyć aplikacji, zostanie wyświetlony błąd podczas dodawania kontrolera.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie kliknij przycisk **Dodaj**, następnie **kontrolera**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

W **Dodawanie szkieletu** okno dialogowe, kliknij przycisk **kontroler MVC 5 z widokami używający narzędzia Entity Framework**, a następnie kliknij przycisk **Dodaj**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Wybierz **Movie (MvcMovie.Models)** dla klasy modelu.
- Wybierz **MovieDBContext (MvcMovie.Models)** dla klasy kontekstu danych.
- Wprowadź nazwę kontrolera **MoviesController**.

 Na poniższym obrazie pokazano ukończone okna dialogowego.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Kliknij przycisk **Dodaj**. (Jeśli wystąpi błąd, prawdopodobnie nie kompilowania aplikacji przed rozpoczęciem dodawania kontrolera.) Program Visual Studio tworzy następujące pliki i foldery:

- *MoviesController.cs* w pliku *kontrolerów* folderu.
- A *Views\Movies* folderu.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, i *Index.cshtml* w nowym *Views\Movies* folderu.

Visual Studio automatycznie utworzony [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoków (automatyczne tworzenie widoków i metod akcji CRUD nosi nazwę szkieletów). Masz teraz aplikacji sieci web w pełni funkcjonalne, która umożliwia tworzenie, listy, edytować i usuwać wpisy filmu.

Uruchom aplikację i wybierz polecenie **MVC Movie** link (lub Przeglądaj w celu `Movies` kontrolera, dodając */Movies* do adresu URL na pasku adresu przeglądarki). Ponieważ aplikacja jest polegania na domyślny routing (zdefiniowany w *aplikacji\_Start\RouteConfig.cs* pliku), żądanie `http://localhost:xxxxx/Movies` jest kierowany do domyślnej `Index` metody akcji `Movies` kontrolera. Innymi słowy żądanie `http://localhost:xxxxx/Movies` jest taka sama jak żądanie `http://localhost:xxxxx/Movies/Index`. Wynik jest wyświetlana pusta lista filmów, ponieważ nie dodano żadnych serwerów jeszcze.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz **Utwórz nowy** łącza. Wprowadź niektóre szczegóły na temat film, a następnie kliknij przycisk **Utwórz** przycisku.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Nie można wprowadzić w polu Cena kropki i przecinki. W celu obsługi weryfikacji jQuery dla ustawień regionalnych innych niż angielskie, które użyj przecinka (&quot;,&quot;) dla punktu dziesiętnego i formaty daty z systemem innym niż angielski, należy wprowadzić *globalize.js* i konkretnej  *cultures/globalize.cultures.js* pliku (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) i JavaScript, aby użyć `Globalize.parseFloat`. I będzie pokazują, jak to zrobić w następnym samouczku. Teraz wprowadź tylko liczby całkowite, takie jak 10.


Kliknięcie przycisku **Utwórz** kliknięcie przycisku powoduje formularza można opublikować na serwerze, gdzie informacje filmu są zapisywane w bazie danych. Użytkownik jest przekierowywane do */Movies* adres URL, w którym można obejrzeć film nowo utworzony na liście.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Utwórz kilka więcej wpisów filmu. Spróbuj **Edytuj**, **szczegóły**, i **usunąć** łącza, które są wszystkie funkcjonalności.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz *Controllers\MoviesController.cs* pliku i sprawdź, czy wygenerowany `Index` metody. Część kontroler film z `Index` metody są wyświetlane poniżej.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Żądanie `Movies` kontrolera zwraca wszystkie wpisy w `Movies` tabeli, a następnie przekazuje wyniki w `Index` widoku. Poniższy wiersz z `MoviesController` klasy tworzy filmu kontekst bazy danych, jak opisano wcześniej. Film kontekst bazy danych służy do zapytań, edytowanie i usuwanie filmów.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silnie Typizowane modeli i @model — słowo kluczowe

Wcześniej w tym samouczku widać, jak kontroler może przekazać dane i obiekty do szablonu widoku przy użyciu `ViewBag` obiektu. `ViewBag` Jest to obiekt dynamiczny, które oferują wygodny sposób późnym wiązaniem do przekazywania informacji do widoku.

MVC udostępnia również możliwość przekazywania *silnie* typizowanych obiektów szablonu widoku. Takie rozwiązanie jednoznacznie zapewnia lepszą kompilacji kontroli kodu i zaawansowaną [IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b(v=vs.120).aspx) w edytorze programu Visual Studio. Takie podejście używany mechanizm szkieletów w programie Visual Studio (czyli przekazywanie *silnie* modelu typu) z `MoviesController` szablonów klasy i widoku podczas jego tworzenia widoków i metod.

W *Controllers\MoviesController.cs* zbadać wygenerowany plik `Details` metody. `Details` Metody są wyświetlane poniżej.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id` Parametr jest zwykle przekazywany jako dane trasy, na przykład `http://localhost:1234/movies/details/1` ustawi kontrolera do kontrolera film, Akcja `details` i `id` do 1. Można również przekazujesz w identyfikatorze z ciągu zapytania w następujący sposób:

`http://localhost:1234/movies/details?id=1`

Jeśli `Movie` zostanie znaleziony, wystąpienie `Movie` modelu jest przekazywana do `Details` widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Sprawdź zawartość *Views\Movies\Details.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

W tym `@model` instrukcji w górnej części pliku szablonu widoku, można określić typu obiektu, który oczekuje widoku. Podczas tworzenia kontrolera film, Visual Studio automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Details.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

To `@model` dyrektywy umożliwia dostęp do filmów, który kontroler przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Details.cshtml* szablonu, kod przekazuje każde pole film, aby `DisplayNameFor` i [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocników HTML z silnie typizowaną `Model` obiektu. `Create` i `Edit` metody i Wyświetl szablony również przekazać film obiekt modelu.

Sprawdź *Index.cshtml* Wyświetl szablon i `Index` metody w *MoviesController.cs* pliku. Zwróć uwagę, jak kod tworzy [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) obiektu, gdy wywołuje `View` metody pomocnika w `Index` metody akcji. Kod następnie przekazuje to `Movies` z listy `Index` metody akcji w widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Podczas tworzenia kontrolera film, Visual Studio automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Index.cshtml* pliku:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

To `@model` dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Index.cshtml* szablonu, kod w pętli filmów za pomocą ćwiczeń `foreach` instrukcji na silnie typizowaną `Model` obiektu:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Ponieważ `Model` obiektu jest silnie typizowane (jako `IEnumerable<Movie>` obiektu), każdy `item` jest typu obiektu w pętli `Movie`. Wśród innych korzyści oznacza to, Pobierz sprawdzanie kompilacji kodu, a następnie pełne obsługę funkcji IntelliSense w edytorze kodu:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Praca z bazy danych LocalDB programu SQL Server

Entity Framework Code First wykrył, że podany ciąg połączenia bazy danych wskazywanego `Movies` bazy danych, która nie istnieje jeszcze, więc Code First baza danych utworzona automatycznie. Możesz sprawdzić, czy jest on utworzony przez wyszukiwanie *aplikacji\_danych* folderu. Jeśli nie widzisz *Movies.mdf* plików, kliknij **Pokaż wszystkie pliki** przycisk **Eksploratora rozwiązań** narzędzi, kliknij przycisk **Odśwież** przycisk, a następnie rozwiń *aplikacji\_danych* folderu.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Kliknij dwukrotnie *Movies.mdf* otworzyć **EKSPLORATORA serwera**, następnie rozwiń węzeł **tabel** folder, aby wyświetlić tabelę filmów. Należy pamiętać, ikonę klucza, obok identyfikatora. Domyślnie EF spowoduje, że właściwość o nazwie identyfikator klucza podstawowego. Aby uzyskać więcej informacji na EF i MVC, zobacz samouczek znakomity Tomasz Dykstra na [MVC i EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Kliknij prawym przyciskiem myszy `Movies` tabeli i wybierz **Pokaż dane tabeli** do wyświetlania danych został utworzony.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Kliknij prawym przyciskiem myszy `Movies` tabeli i wybierz **Otwórz definicję tabeli** do tabeli struktury tego Entity Framework Code First utworzone automatycznie.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Powiadomienie jak schemat `Movies` mapy do tabel `Movie` klasy utworzony wcześniej. Entity Framework Code First automatycznie utworzyć ten schemat na podstawie Twojej `Movie` klasy.

Po zakończeniu zamknij połączenie przez kliknięcie prawym przyciskiem myszy *MovieDBContext* i wybierając **zamknąć połączenia**. (Jeśli nie zamkniesz połączenie, może być błąd pojawia się przy następnym uruchomieniu projektu).

![](accessing-your-models-data-from-a-controller/_static/image15.png "Metody")

Masz teraz bazę danych i stron do wyświetlania, edytowania, aktualizacji i usuwania danych. W następnym samouczku będzie firma Microsoft bada reszty szkieletu kodu i dodać `SearchIndex` — metoda i `SearchIndex` widoku, który umożliwia wyszukiwanie filmów w tej bazie danych. Aby uzyskać więcej informacji o korzystaniu z programu Entity Framework z MVC, zobacz [tworzenia modelu danych struktury jednostek dla aplikacji platformy ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

>[!div class="step-by-step"]
[Poprzednie](creating-a-connection-string.md)
[dalej](examining-the-edit-methods-and-edit-view.md)
