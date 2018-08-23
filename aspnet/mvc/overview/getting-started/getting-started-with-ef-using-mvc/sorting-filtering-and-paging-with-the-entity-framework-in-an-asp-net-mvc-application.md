---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a72d6cbce0e5f20360e1e9bfc20f4a75bfeb3343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752080"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [Pobierz plik PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i Visual Studio 2013. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednim samouczku wdrożono zestaw stron sieci web w przypadku podstawowych operacji CRUD dla `Student` jednostek. W tym samouczku dodasz, sortowanie, filtrowanie i stronicowanie funkcje **studentów** strony indeksu. Utworzysz też strony, która wykonuje prostą grupowania.

Na poniższej ilustracji przedstawiono wygląd strony po zakończeniu. Nagłówki kolumn odnoszą się łącza, które użytkownik może kliknąć, aby posortować według tej kolumny. Klikając kolumnę nagłówka wielokrotnie przełącza między malejącą i rosnącą porządek sortowania.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Dodaj kolumnę sortowania łącza do strony indeksu uczniów

Aby dodać sortowanie do strony indeksu dla uczniów, zmienisz `Index` metody `Student` kontrolera i Dodaj kod do `Student` indeksować widoku.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodaj sortowanie funkcji Index — metoda

W *Controllers\StudentController.cs*, Zastąp `Index` metoda następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Wartość ciągu zapytania jest dostarczane przez platformę ASP.NET MVC jako parametr do metody akcji. Parametr będzie ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia i ciąg "desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

Przy pierwszym żądaniu strony indeksu nie jest Brak ciągu zapytania. Uczniów są wyświetlane w porządku rosnącym według `LastName`, co jest ustawieniem domyślnym, zgodnie z ustaleniami w należą do przypadku `switch` instrukcji. Kiedy użytkownik kliknie hiperlink nagłówek kolumny, odpowiednie `sortOrder` podana w ciągu zapytania.

Dwa `ViewBag` zmienne są używane, tak aby widok można skonfigurować hiperłącza nagłówków kolumn za pomocą wartości ciągu zapytania odpowiednie:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Są to trójargumentowy instrukcji. Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub jest pusta, `ViewBag.NameSortParm` powinna być ustawiona na "nazwa\_desc"; w przeciwnym razie należy ją ustawić na pusty ciąg. Te dwie instrukcje Włącz widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:

| Bieżący kolejność sortowania | Ostatnia nazwa hiperłącza | Data hiperłącza |
| --- | --- | --- |
| Ostatnie nazwy, rosnąco | descending | ascending |
| Ostatnie nazwy, malejąco | ascending | ascending |
| Data, w kolejności rosnącej | ascending | descending |
| Data, malejąco | ascending | ascending |

Metoda używa [składnik LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) określić kolumnę sortowania. Ten kod tworzy [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) zmiennej przed `switch` instrukcji, modyfikuje je w `switch` instrukcji i wywołania `ToList` metody `switch` instrukcji. Podczas tworzenia i modyfikowania `IQueryable` zmiennych, bez określenia zapytania są wysyłane do bazy danych. Zapytanie nie jest wykonywane, dopóki nie można przekonwertować `IQueryable` obiektu w kolekcji przez wywołanie metody, takie jak `ToList`. W związku z tym, ten kod powoduje pojedyncze zapytanie, które nie jest wykonywane, dopóki `return View` instrukcji.

Jako alternatywę do pisania różnych instrukcji LINQ dla każdego kolejność sortowania można dynamicznie utworzyć instrukcji LINQ. Aby uzyskać informacji na temat dynamicznej LINQ, zobacz [dynamiczne LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodaj kolumnę nagłówka hiperłącza do widoku indeksu dla uczniów

W *Views\Student\Index.cshtml*, Zastąp `<tr>` i `<th>` elementy wiersz nagłówka z wyróżniony kod:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Ten kod używa tych informacji w `ViewBag` właściwości, aby skonfigurować hiperlinki z zapytaniem odpowiednie wartości ciągu.

Uruchom stronę, a następnie kliknij przycisk **nazwisko** i **Data rejestracji** nagłówków kolumn, aby zweryfikować, że sortowanie działa.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Po kliknięciu **nazwisko** nagłówka, studentów są wyświetlane w ostatniej malejącej nazwy.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Dodaj pole wyszukiwania do strony indeksu uczniów

Aby dodać filtrowanie do strony indeksu studentów, dodasz pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody. Pole tekstowe umożliwi wprowadź ciąg do wyszukania w imię pola imienia i nazwiska.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

W *Controllers\StudentController.cs*, Zastąp `Index` metoda następującym kodem (zmiany zostały wyróżnione):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Po dodaniu `searchString` parametr `Index` metody. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, które należy dodać do widoku indeksu. Możesz również zostały dodane do instrukcji LINQ `where` klauzula, która wybiera tylko uczniowie, w których imię lub nazwisko zawiera ciąg wyszukiwania. Instrukcja, która dodaje [gdzie](https://msdn.microsoft.com/library/bb535040.aspx) klauzula jest wykonywany tylko wtedy, gdy wartość do wyszukania.

> [!NOTE]
> W wielu przypadkach można wywołać tej samej metody, zestaw jednostek platformy Entity Framework lub jako metodę rozszerzenia w kolekcji w pamięci. Wyniki są zazwyczaj takie same, ale w niektórych przypadkach może być inna.
> 
> Na przykład implementacji .NET Framework z `Contains` metoda zwraca wszystkie wiersze, w przypadku przekazania pustego ciągu do niego, ale dostawcy środowiska Entity Framework dla programu SQL Server Compact 4.0 zwraca zero wierszy obecność pustych ciągów. W związku z tym kodem w przykładzie (umieszczanie `Where` instrukcji wewnątrz `if` instrukcji) zapewnia, że, Pobierz te same wyniki dla wszystkich wersji programu SQL Server. Ponadto wdrożenia programu .NET Framework z `Contains` metoda wykonuje porównania uwzględniającego wielkość liter, domyślnie, ale dostawcy programu Entity Framework SQL Server wykonania porównania bez uwzględniania wielkości liter, domyślnie. Dlatego wywołanie `ToUpper` metody testu jawnie bez uwzględniania wielkości liter zapewnia wyniki nie należy zmieniać po zmianie kodu później, aby korzystać z repozytorium, które zwróci `IEnumerable` zbiór zamiast `IQueryable` obiektu. (Gdy wywołujesz `Contains` metody `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; Jeśli wywołasz ją na `IQueryable` obiektu, możesz uzyskać implementację dostawcy bazy danych.)
> 
> Obsługa wartości null, również mogą być różne dla dostawców innej bazy danych lub jeśli używasz `IQueryable` względem obiektu, gdy używasz `IEnumerable` kolekcji. Na przykład w niektórych scenariuszach `Where` warunków, takich jak `table.Column != 0` mogą nie zwracać kolumn, które mają `null` jako wartość. Aby uzyskać więcej informacji, zobacz [nieprawidłowa obsługa zmiennych o wartości null w klauzuli "where"](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Dodaj pole wyszukiwania do widoku indeksu dla uczniów

W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed otwierającym `table` tag, aby można było utworzyć podpis, pola tekstowego, a **wyszukiwania** przycisku.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Uruchom strony, wprowadź wyszukiwany ciąg, a następnie kliknij przycisk **wyszukiwania** Aby sprawdzić, czy działa filtrowanie.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Należy zauważyć, że adres URL nie zawiera "" ciąg wyszukiwania, co oznacza, że jeśli Oznacz tę stronę zakładką nie otrzymasz filtrowana lista korzystając z zakładki. Dotyczy to również łączy sortowania kolumn, ponieważ zostaną one posortowane całej listy. Poznasz, jak zmienić **wyszukiwania** przycisk, aby użyć ciągów zapytań do kryteriów filtrowania w dalszej części tego samouczka.

## <a name="add-paging-to-the-students-index-page"></a>Dodawanie stronicowania do strony indeksu uczniów

Dodawanie stronicowania do strony indeksu studentów, zostanie najpierw należy zainstalować **PagedList.Mvc** pakietu NuGet. Będzie wprowadzić dodatkowe zmiany w `Index` metody i dodawanie stronicowania łącza do `Index` widoku. **PagedList.Mvc** jest jednym z wielu dobre stronicowania i sortowania pakietów dla platformy ASP.NET MVC i jego użycia w tym miejscu jest przeznaczona tylko na potrzeby przykładu nie jako zalecenie dla niego inne opcje. Poniższa ilustracja przedstawia łącza stronicowania.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Zainstaluj pakiet PagedList.MVC NuGet

NuGet **PagedList.Mvc** pakiet automatycznie instaluje **PagedList** pakietu jako zależność. **PagedList** pakiet instaluje `PagedList` kolekcji typu i rozszerzenie metody `IQueryable` i `IEnumerable` kolekcji. Metody rozszerzające tworzenia pojedynczej strony danych w `PagedList` kolekcji z Twojej `IQueryable` lub `IEnumerable`i `PagedList` kolekcji zawiera kilka właściwości i metod, które ułatwiają stronicowania. **PagedList.Mvc** pakiet Instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** i następnie **Konsola Menedżera pakietów**.

W **Konsola Menedżera pakietów** okna, upewnij się, że **źródła pakietu** jest **nuget.org** i **projekt domyślny** jest **ContosoUniversity**, a następnie wprowadź następujące polecenie:

`Install-Package PagedList.Mvc`

![Zainstaluj PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Skompiluj projekt. 

### <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do Index — metoda

W *Controllers\StudentController.cs*, Dodaj `using` poufności informacji dotyczące `PagedList` przestrzeni nazw:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Zastąp `Index` metoda następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Ten kod dodaje `page` parametr, bieżący parametr kolejność sortowania i bieżący parametr filtru do sygnatury metody:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknie, stronicowanie i sortowanie łącza, wszystkich parametrów będzie pusta. Po kliknięciu łącza stronicowania `page` zmienna będzie zawierać numer strony, aby wyświetlić.

A `ViewBag` właściwości zapewnia widok z bieżącej kolejności sortowania, ponieważ ten musi być uwzględniona w łącza stronicowania Aby zachować porządek sortowania, taki sam, podczas stronicowania:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Inna właściwość `ViewBag.CurrentFilter`, zawiera widok bieżący ciąg filtru. Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i musi on zostać przywrócony do pola tekstowego po stronie zostanie wyświetlony ponownie. Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strony musi być ustawiony na 1, nowy filtr może powodować różne dane do wyświetlenia. Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i naciśnięciu przycisku Prześlij. W takim przypadku `searchString` parametru nie ma wartości null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Na końcu metody `ToPagedList` metody rozszerzenia na uczniów `IQueryable` obiektu konwertuje zapytań dla uczniów na pojedynczej strony uczniów na typ kolekcji, który obsługuje stronicowanie. Tego jednostronicowej studentów są następnie przekazywane do widoku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Metoda przyjmuje numeru strony. Dwa znaki zapytania reprezentują [operatorem łączenia wartości null](https://msdn.microsoft.com/library/ms173224.aspx). Operator łączenia wartości null określa wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(page ?? 1)` oznacza, że zwracają wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.

### <a name="add-paging-links-to-the-student-index-view"></a>Dodawanie stronicowania łączy do widoku indeksu dla uczniów

W *Views\Student\Index.cshtml*, Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

`@model` Instrukcji w górnej części strony określa widok teraz pobiera `PagedList` zamiast obiektu `List` obiektu.

`using` Poufności informacji dotyczące `PagedList.Mvc` daje dostęp do pomocy MVC przycisków stronicowania.

Kod używa przeciążenia [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) umożliwiająca, aby określić [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Wartość domyślna [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) przesyła dane formularza z WPISEM, co oznacza, że parametry są przekazywane w treści komunikatu HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu HTTP GET, dane formularza jest przekazywany w adresie URL jako ciągi zapytań, która umożliwia użytkownikom utworzyć zakładkę ma adres URL. [W3C wytyczne dotyczące użytkowania HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) zaleca się, powinien być używany GET akcja nie powoduje aktualizacji.

Pole tekstowe jest inicjowany z aktualnie wyszukiwanego ciągu, dzięki czemu po kliknięciu nowej strony można przeglądać aktualnie wyszukiwanego ciągu.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Linki nagłówek kolumny, użyj ciągu zapytania do przekazania aktualnie wyszukiwanego ciągu do kontrolera, dzięki czemu użytkownik może sortować w ramach wyników filtrowania:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Bieżącej strony i łączna liczba stron są wyświetlane.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

W przypadku stron do wyświetlania jest wyświetlany "Page 0 0". (W tym przypadku jest większa niż liczba stron na numer strony ponieważ `Model.PageNumber` wynosi 1, a `Model.PageCount` wynosi 0.)

Przyciski stronicowania są wyświetlane przez `PagedListPager` pomocy:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Pomocnik zapewnia kilka opcji, które można dostosować, w tym adresy URL i ustawianie stylów. Aby uzyskać więcej informacji, zobacz [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) w witrynie GitHub.

Uruchom stronę.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Po kliknięciu łączy stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania. Następnie wprowadź wyszukiwany ciąg i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie przy użyciu sortowania i filtrowania.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Utwórz informacje o stronie, które znajdują się dane statystyczne dla uczniów

Dla firmy Contoso University witryny internetowej informacje o stronie będą wyświetlane, jak wiele studentów zostały zarejestrowane dla każdego dnia rejestracji. Wymaga to obliczeń grupowania i prostych grup. Aby to osiągnąć, wykonasz następujące czynności:

- Utwórz klasę modelu widoku danych potrzebnych do przekazania do widoku.
- Modyfikowanie `About` method in Class metoda `Home` kontrolera.
- Modyfikowanie `About` widoku.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Tworzenie *modele widoków* folderu w folderze projektu. W tym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu poniższym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie kontrolera głównego

W *HomeController.cs*, Dodaj następujący kod `using` instrukcji w górnej części pliku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Zastąp `About` metoda następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Instrukcji LINQ grup jednostek uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w zbiorze `EnrollmentDateGroup` wyświetlić obiekty w modelu.

Dodaj `Dispose` metody:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Zmodyfikuj widok — informacje

Zastąp kod w *Views\Home\About.cshtml* pliku następującym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Uruchom aplikację, a następnie kliknij przycisk **o** łącza. Liczba studentów każdej daty rejestracji jest wyświetlany w tabeli.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia modelu danych i wdrażania podstawowych operacji CRUD, sortowanie, filtrowanie, stronicowanie i grupowanie funkcji. W następnym samouczku rozpocznie się spojrzenie na bardziej zaawansowanych tematów, rozwijając modelu danych.

Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię. Możesz również poprosić o nowe tematy w [Pokaż mi jak za pomocą kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [dalej](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
