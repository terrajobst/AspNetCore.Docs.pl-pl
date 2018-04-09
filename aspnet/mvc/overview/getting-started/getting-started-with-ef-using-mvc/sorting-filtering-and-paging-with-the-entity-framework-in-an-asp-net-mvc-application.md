---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortowanie, filtrowanie i stronicowania Entity Framework w aplikacji platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 02b7d988202966dc0011eeed32cd632c6e0565b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Sortowanie, filtrowanie i stronicowania Entity Framework w aplikacji platformy ASP.NET MVC
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednich instrukcji zaimplementowana zestaw stron sieci web dla podstawowe operacje CRUD na `Student` jednostek. W tym samouczku można dodać sortowanie, filtrowanie i funkcje stronicowania **studentów** strony indeksu. Utworzysz także strona, która jest prosta grupa.

Na poniższej ilustracji przedstawiono wygląd strony po zakończeniu. Nagłówki kolumn są łącza, które użytkownik może kliknąć, aby sortować według tej kolumny. Kliknięcie nagłówka wielokrotnie kolumny przełączanie się między kolumny sortowania.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Dodaj kolumny sortowania łącza do strony indeksu uczniów lub studentów

Aby dodać sortowanie do strony indeksu dla użytkowników domowych, zostanie zmieniona `Index` metody `Student` kontrolera i Dodaj kod, aby `Student` Indeksuj zawartości widoku.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodawanie funkcji do metody indeksu sortowania

W *Controllers\StudentController.cs*, Zastąp `Index` metodę z następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Wartość ciągu kwerendy są udostępniane przez program ASP.NET MVC jako parametr do metody akcji. Parametr będzie ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia, a ciąg "desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

Żądana strona indeksu, po raz pierwszy nie jest Brak ciągu zapytania. Studenci są wyświetlane w rosnącej kolejności przez `LastName`, co jest ustawieniem domyślnym, zgodnie z ustaleniami w przypadku fall-through `switch` instrukcji. Gdy użytkownik klika hiperłącze nagłówek kolumny, odpowiedni `sortOrder` wartość jest podana w ciągu zapytania.

Dwa `ViewBag` zmienne są używane, aby widok było skonfigurowanie hiperłącza nagłówek kolumny z wartości typu QueryString odpowiednie:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Są to trójargumentowy instrukcje. Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub pusty, `ViewBag.NameSortParm` powinien być ustawiony na "nazwa\_desc"; w przeciwnym razie należy ustawić pustego ciągu. Te dwie instrukcje włączyć widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:

| Bieżącej kolejności sortowania | Ostatnia nazwa hiperłącza | Data hiperłącza |
| --- | --- | --- |
| Ostatni nazwy, rosnąco | descending | ascending |
| Ostatni nazwy, malejąco | ascending | ascending |
| Data w kolejności rosnącej | ascending | descending |
| Data, malejąco | ascending | ascending |

W metodzie [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) określić kolumnę sortowania. Kod tworzy [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) zmiennej przed `switch` instrukcji, modyfikuje go w `switch` instrukcji i wywołania `ToList` metody po `switch` instrukcji. Podczas tworzenia i modyfikowania `IQueryable` zmiennych, nie zapytanie jest wysyłane do bazy danych. Kwerenda nie została wykonana, do momentu konwersji `IQueryable` obiektu do kolekcji, wywołując metodę, takich jak `ToList`. W związku z tym powoduje ten kod w jednym zapytaniu, która nie jest wykonywana do czasu `return View` instrukcji.

Jako alternatywę do zapisywania różnych instrukcje LINQ dla każdego porządek sortowania można dynamicznie utworzyć instrukcję LINQ. Uzyskać informacji o dynamicznych LINQ, zobacz [dynamiczne LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodawanie hiperłącza do widoku indeksu uczniów nagłówek kolumny

W *Views\Student\Index.cshtml*, Zastąp `<tr>` i `<th>` elementy wiersz nagłówka z wyróżniony kod:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Ten kod używa tych informacji w `ViewBag` właściwości do skonfigurowania hiperłącza z zapytaniem odpowiednie ciągu wartości.

Uruchom strony, a następnie kliknij przycisk **nazwisko** i **Data rejestracji** działa nagłówki kolumn, aby zweryfikować, że sortowanie.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Po kliknięciu **nazwisko** nagłówek, studentów są wyświetlane w porządku malejącym ostatnia nazwa.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Dodaj pole wyszukiwania do strony indeksu uczniów lub studentów

Aby dodać filtrowanie do strony indeksu studentów, będzie Dodaj pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody. Pole tekstowe umożliwi wprowadź ciąg do wyszukania w imię pola imienia i nazwiska.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

W *Controllers\StudentController.cs*, Zastąp `Index` metodę z następującym kodem (zmiany zostały wyróżnione):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Dodano `searchString` parametr `Index` metody. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, która zostanie dodana do widoku indeksu. Również dodane do instrukcji LINQ `where` klauzuli, który wybiera tylko studentów, w których imię lub nazwisko zawiera ciąg wyszukiwania. Instrukcja, która dodaje [gdzie](https://msdn.microsoft.com/library/bb535040.aspx) klauzuli jest wykonywane tylko wtedy, gdy wartość do wyszukania.

> [!NOTE]
> W wielu przypadkach można wywołać tej samej metody zestaw jednostek Entity Framework lub jako metodę rozszerzenie w kolekcji w pamięci. Wyniki są zazwyczaj takie same, ale w niektórych przypadkach może się różnić.
> 
> Na przykład, .NET Framework wykonania `Contains` metoda zwraca wszystkie wiersze, gdy przekazać pusty ciąg do niego, ale dostawcy programu Entity Framework dla programu SQL Server Compact 4.0 nie zwraca żadnych wierszy obecność pustych ciągów. W związku z tym kod w przykładzie (umieszczanie `Where` instrukcja wewnątrz `if` instrukcji) zapewnia uzyskać ten sam rezultat dla wszystkich wersji programu SQL Server. Ponadto wdrożenia programu .NET Framework z `Contains` metoda wykonuje porównania z uwzględnieniem wielkości liter domyślnie, ale dostawcy programu Entity Framework SQL Server wykonania porównania bez uwzględniania wielkości liter, domyślnie. W związku z tym wywołaniem `ToUpper` metody, aby jawnie bez uwzględniania wielkości liter testu zapewnia wyników nie należy zmieniać po zmianie kod później do korzystania z repozytorium, która będzie zwracać `IEnumerable` kolekcji zamiast `IQueryable` obiektu. (Podczas wywoływania `Contains` metoda `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; podczas wywoływania go na `IQueryable` obiektu, możesz uzyskać implementacji dostawcy bazy danych.)
> 
> Obsługa null również mogą być inne dla dostawców innej bazy danych lub jeśli używasz `IQueryable` porównywany obiekt, korzystając z `IEnumerable` kolekcji. Na przykład w niektórych scenariuszach `Where` warunku, takich jak `table.Column != 0` nie może zwracać kolumn, które mają `null` jako wartość. Aby uzyskać więcej informacji, zobacz [nieprawidłowa obsługa zmiennych wartości null w klauzuli 'where'](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Dodaj pole wyszukiwania do widoku indeksu dla użytkowników domowych

W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed rozpoczęciem `table` tag, aby można było utworzyć podpis pola tekstowego, a **wyszukiwania** przycisku.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Uruchom strony, wprowadź ciąg wyszukiwania, a następnie kliknij przycisk **wyszukiwania** Aby sprawdzić, czy działa filtrowania.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Należy zauważyć, że adres URL nie zawiera "" ciąg wyszukiwania, co oznacza zakładki tej strony, nie będzie uzyskanie wyfiltrowanej listy używania zakładki. Dotyczy to również łącza sortowania kolumn, ponieważ sortowania całą listę. Użytkownik zmieni **wyszukiwania** przycisk, aby użyć ciągów zapytania dla kryteriów filtru później w samouczku.

## <a name="add-paging-to-the-students-index-page"></a>Dodaj stronicowania do strony indeksu uczniów lub studentów

Aby dodać stronicowania do strony indeksu studentów, rozpocznie instalując **PagedList.Mvc** pakietu NuGet. Następnie będzie można wprowadzić dodatkowe zmiany w `Index` — metoda i Dodaj stronicowania łącza do `Index` widoku. **PagedList.Mvc** jest jednym z wielu dobrej stronicowania i sortowania pakietów dla platformy ASP.NET MVC i użyć w tym polu jest przeznaczona tylko na przykład nie jako zalecenie dla niego za pośrednictwem innych opcji. Na poniższej ilustracji przedstawiono łącza stronicowania.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Zainstaluj pakiet PagedList.MVC NuGet

NuGet **PagedList.Mvc** pakiet automatycznie instaluje **PagedList** pakietu jako zależność. **PagedList** pakiet instaluje `PagedList` metody typu i rozszerzenia dla kolekcji `IQueryable` i `IEnumerable` kolekcji. Metody rozszerzenia tworzenia pojedynczej strony danych w `PagedList` kolekcji z Twojej `IQueryable` lub `IEnumerable`i `PagedList` kolekcji zawiera kilka właściwości i metody, które ułatwiają stronicowania. **PagedList.Mvc** pakiet Instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** , a następnie **Konsola Menedżera pakietów**.

W **Konsola Menedżera pakietów** okna, upewnij się, że **źródła pakietu** jest **nuget.org** i **domyślny projekt** jest **ContosoUniversity**, a następnie wprowadź następujące polecenie:

`Install-Package PagedList.Mvc`

![Zainstaluj PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Skompiluj projekt. 

### <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do Index — metoda

W *Controllers\StudentController.cs*, Dodaj `using` instrukcji dla `PagedList` przestrzeni nazw:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Zastąp `Index` metodę z następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Ten kod dodaje `page` parametru bieżącego parametru kolejność sortowania i bieżącego parametru filtru w podpisie metody:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknął stronicowania i sortowania łącza, wszystkie parametry będzie miała wartość null. Po kliknięciu łącza stronicowania `page` zmienna będzie zawierać numer strony do wyświetlenia.

A `ViewBag` właściwości zapewnia widok z bieżącej kolejności sortowania, ponieważ to musi być uwzględniona w łącza stronicowania aby zapewnić porządek sortowania, taka sama podczas stronicowania:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Inna właściwość `ViewBag.CurrentFilter`, zapewnia widok bieżący ciąg filtru. Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i go muszą być przywrócone do pola tekstowego, gdy strona zostanie wyświetlony ponownie. Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strona musi zresetowana do wartości 1, nowy filtr może powodować różnych danych do wyświetlenia. Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i kliknięciu przycisku Prześlij. W takim przypadku `searchString` parametru nie jest zerowa.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Na końcu metody `ToPagedList` metody rozszerzenia dla uczniów lub studentów `IQueryable` obiektu konwertuje zapytań uczniów do pojedynczej strony studentów w typ kolekcji, który obsługuje stronicowanie. Tej stronie studentów są następnie przekazywane do widoku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Metoda przyjmuje numer strony. Reprezentuje dwa znaki zapytania [łączenie null operator](https://msdn.microsoft.com/library/ms173224.aspx). Wartość domyślna dla typu dopuszczającego wartość null; definiuje operator łączenia wartości null wyrażenie `(page ?? 1)` oznacza zwrócić wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.

### <a name="add-paging-links-to-the-student-index-view"></a>Dodawania łączy stronicowania w widoku indeksu dla użytkowników domowych

W *Views\Student\Index.cshtml*, Zastąp istniejący kod następującym kodem. Zmiany zostały wyróżnione.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

`@model` Instrukcji w górnej części strony określa, czy widok teraz pobiera `PagedList` obiekt zamiast `List` obiektu.

`using` Instrukcji dla `PagedList.Mvc` zapewnia dostęp do pomocniczego MVC przycisków stronicowania.

Kod używane jest przeciążenie [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) umożliwiająca, aby określić [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Wartość domyślna [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) przesyła dane formularza przy użyciu metody POST, co oznacza, że parametry są przekazywane w treści wiadomości HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu HTTP GET, formularza dane są przekazywane w adresie URL jako ciągi zapytania, który umożliwia użytkownikom zakładki adres URL. [W3C wytyczne dotyczące stosowania HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) zaleca, aby powinny używać GET, gdy akcja nie powoduje aktualizacji.

Pole tekstowe jest inicjowany z aktualnie wyszukiwanego ciągu, więc po kliknięciu nowej strony można zobaczyć aktualnie wyszukiwanego ciągu.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Łącza nagłówka kolumny Użyj ciągu zapytania do przekazania do kontrolera aktualnie wyszukiwanego ciągu, dzięki czemu użytkownik może sortować w wynikach filtrowania:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Bieżącej strony i łączna liczba stron wyświetlanych.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Jeśli nie ma żadnych stron do wyświetlenia, wyświetlany jest "Page 0 0". (W takim przypadku numer strony jest większa niż liczba stron ponieważ `Model.PageNumber` 1, a `Model.PageCount` to 0.)

Wyświetlane przyciski stronicowania przez `PagedListPager` pomocy:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Pomocnika udostępnia wiele opcji, które można dostosować, w tym adresy URL i style. Aby uzyskać więcej informacji, zobacz [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) w witrynie GitHub.

Uruchom strony.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Kliknij łącza stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania. Następnie wprowadź ciąg wyszukiwania i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie z sortowania i filtrowania.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Utwórz o stronie, które znajdują się dane statystyczne uczniów

Contoso University witryny sieci Web przez strony będzie wyświetlane liczby studentów zostały zarejestrowane dla każdego dnia rejestracji. Wymaga to prosty i grupowania obliczeń grup. W tym celu będzie wykonaj następujące czynności:

- Utwórz klasę modelu widoku danych, która należy do przekazania do widoku.
- Modyfikowanie `About` metoda `Home` kontrolera.
- Modyfikowanie `About` widoku.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Utwórz *ViewModels* folderu w folderze projektu. W tym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu z następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie macierzystego kontrolera

W *HomeController.cs*, Dodaj następujący `using` instrukcje w górnej części pliku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym klasy:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Zastąp `About` metodę z następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Instrukcja LINQ grupy jednostek uczniowie według daty rejestracji oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w kolekcji z `EnrollmentDateGroup` wyświetlić obiekty modelu.

Dodaj `Dispose` metody:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Zmodyfikuj widok — informacje

Zastąp kod w *Views\Home\About.cshtml* pliku następującym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Uruchom aplikację i kliknij przycisk **o** łącza. Liczba studentów dla każdego dnia rejestracji jest wyświetlane w formie tabeli.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia modelu danych i implementować basic CRUD, sortowanie, filtrowanie, stronicowania i funkcja grupowania. W następnym samouczku zostaną patrzeć bardziej zaawansowanych tematów, rozwijając modelu danych.

Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć. Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [dalej](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
