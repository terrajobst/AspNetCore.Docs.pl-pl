---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tym samouczku dodasz, sortowanie, filtrowanie i stronicowanie funkcje **studentów** strony indeksu. Możesz również utworzyć strony proste grupowanie.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: b7b5d3d3931f752f2effc044ca8cc52eab22da0a
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889863"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC

W [poprzedniego samouczka](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), zaimplementowane zestaw stron sieci web w przypadku podstawowych operacji CRUD dla `Student` jednostek. W tym samouczku dodasz, sortowanie, filtrowanie i stronicowanie funkcje **studentów** strony indeksu. Możesz również utworzyć strony proste grupowanie.

Na poniższej ilustracji przedstawiono, jak będzie wyglądać stronę po zakończeniu. Nagłówki kolumn odnoszą się łącza, które użytkownik może kliknąć, aby posortować według tej kolumny. Klikając kolumnę nagłówka wielokrotnie przełącza między malejącą i rosnącą porządek sortowania.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodaj kolumnę sortowania łącza
> * Dodawanie pola wyszukiwania
> * Dodawanie stronicowania
> * Tworzenie strony informacje

## <a name="prerequisites"></a>Wymagania wstępne

* [Implementowanie podstawowych funkcji CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Dodaj kolumnę sortowania łącza

Aby dodać sortowanie do strony indeksu dla uczniów, zmienisz `Index` metody `Student` kontrolera i Dodaj kod do `Student` indeksować widoku.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodawanie funkcji sortowania do Index — metoda

- W *Controllers\StudentController.cs*, Zastąp `Index` metoda następującym kodem:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Wartość ciągu zapytania jest dostarczane przez platformę ASP.NET MVC jako parametr do metody akcji. Parametr jest ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia i ciąg "desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

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

Metoda używa [składnik LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) określić kolumnę sortowania. Ten kod tworzy <xref:System.Linq.IQueryable%601> zmiennej przed `switch` instrukcji, modyfikuje je w `switch` instrukcji i wywołania `ToList` metody `switch` instrukcji. Podczas tworzenia i modyfikowania `IQueryable` zmiennych, bez określenia zapytania są wysyłane do bazy danych. Zapytanie nie jest wykonywane, dopóki nie można przekonwertować `IQueryable` obiektu w kolekcji przez wywołanie metody, takie jak `ToList`. W związku z tym, ten kod powoduje pojedyncze zapytanie, które nie jest wykonywane, dopóki `return View` instrukcji.

Jako alternatywę do pisania różnych instrukcji LINQ dla każdego kolejność sortowania można dynamicznie utworzyć instrukcji LINQ. Aby uzyskać informacji na temat dynamicznej LINQ, zobacz [dynamiczne LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodawanie hiperłączy nagłówek kolumny do widoku indeksu dla uczniów

1. W *Views\Student\Index.cshtml*, Zastąp `<tr>` i `<th>` elementy wiersz nagłówka z wyróżniony kod:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Ten kod używa tych informacji w `ViewBag` właściwości, aby skonfigurować hiperlinki z zapytaniem odpowiednie wartości ciągu.

2. Uruchom stronę, a następnie kliknij przycisk **nazwisko** i **Data rejestracji** nagłówków kolumn, aby zweryfikować, że sortowanie działa.

   Po kliknięciu **nazwisko** nagłówka, studentów są wyświetlane w ostatniej malejącej nazwy.

## <a name="add-a-search-box"></a>Dodawanie pola wyszukiwania

Aby dodać filtrowanie do strony indeksu studentów, dodasz pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody. Pole tekstowe pozwala wprowadzić ciąg do wyszukania w imię pola imienia i nazwiska.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

- W *Controllers\StudentController.cs*, Zastąp `Index` metoda następującym kodem (zmiany zostały wyróżnione):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Ten kod dodaje `searchString` parametr `Index` metody. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, które należy dodać do widoku indeksu. Dodaje także `where` klauzuli do instrukcji LINQ, który wybiera tylko uczniowie, w których imię lub nazwisko zawiera ciąg wyszukiwania. Instrukcja, która dodaje <xref:System.Linq.Queryable.Where%2A> klauzula jest wykonywany tylko wtedy, gdy wartość do wyszukania.

> [!NOTE]
> W wielu przypadkach można wywołać tej samej metody, zestaw jednostek platformy Entity Framework lub jako metodę rozszerzenia w kolekcji w pamięci. Wyniki są zazwyczaj takie same, ale w niektórych przypadkach może być inna.
>
> Na przykład implementacji .NET Framework z `Contains` metoda zwraca wszystkie wiersze, w przypadku przekazania pustego ciągu do niego, ale dostawcy środowiska Entity Framework dla programu SQL Server Compact 4.0 zwraca zero wierszy obecność pustych ciągów. W związku z tym kodem w przykładzie (umieszczanie `Where` instrukcji wewnątrz `if` instrukcji) zapewnia, że, Pobierz te same wyniki dla wszystkich wersji programu SQL Server. Ponadto wdrożenia programu .NET Framework z `Contains` metoda wykonuje porównania uwzględniającego wielkość liter, domyślnie, ale dostawcy programu Entity Framework SQL Server wykonania porównania bez uwzględniania wielkości liter, domyślnie. Dlatego wywołanie `ToUpper` metody testu jawnie bez uwzględniania wielkości liter zapewnia wyniki nie należy zmieniać po zmianie kodu później, aby korzystać z repozytorium, które zwróci `IEnumerable` zbiór zamiast `IQueryable` obiektu. (Gdy wywołujesz `Contains` metody `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; Jeśli wywołasz ją na `IQueryable` obiektu, możesz uzyskać implementację dostawcy bazy danych.)
>
> Obsługa wartości null, również mogą być różne dla dostawców innej bazy danych lub jeśli używasz `IQueryable` względem obiektu, gdy używasz `IEnumerable` kolekcji. Na przykład w niektórych scenariuszach `Where` warunków, takich jak `table.Column != 0` mogą nie zwracać kolumn, które mają `null` jako wartość. Aby uzyskać więcej informacji, zobacz [nieprawidłowa obsługa zmiennych o wartości null w klauzuli "where"](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).

### <a name="add-a-search-box-to-the-student-index-view"></a>Dodaj pole wyszukiwania do widoku indeksu dla uczniów

1. W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed otwierającym `table` tag, aby można było utworzyć podpis, pola tekstowego, a **wyszukiwania** przycisku.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Uruchom strony, wprowadź wyszukiwany ciąg, a następnie kliknij przycisk **wyszukiwania** Aby sprawdzić, czy działa filtrowanie.

   Należy zauważyć, że adres URL nie zawiera "" ciąg wyszukiwania, co oznacza, że jeśli Oznacz tę stronę zakładką nie otrzymasz filtrowana lista korzystając z zakładki. Dotyczy to również łączy sortowania kolumn, ponieważ zostaną one posortowane całej listy. Poznasz, jak zmienić **wyszukiwania** przycisk, aby użyć ciągów zapytań do kryteriów filtrowania w dalszej części tego samouczka.

## <a name="add-paging"></a>Dodawanie stronicowania

Dodawanie stronicowania do strony indeksu studentów, zostanie najpierw należy zainstalować **PagedList.Mvc** pakietu NuGet. Będzie wprowadzić dodatkowe zmiany w `Index` metody i dodawanie stronicowania łącza do `Index` widoku. **PagedList.Mvc** jest jednym z wielu dobre stronicowania i sortowania pakietów dla platformy ASP.NET MVC i jego użycia w tym miejscu jest przeznaczona tylko na potrzeby przykładu nie jako zalecenie dla niego inne opcje.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Zainstaluj pakiet PagedList.MVC NuGet

NuGet **PagedList.Mvc** pakiet automatycznie instaluje **PagedList** pakietu jako zależność. **PagedList** pakiet instaluje `PagedList` kolekcji typu i rozszerzenie metody `IQueryable` i `IEnumerable` kolekcji. Metody rozszerzające tworzenia pojedynczej strony danych w `PagedList` kolekcji z Twojej `IQueryable` lub `IEnumerable`i `PagedList` kolekcji zawiera kilka właściwości i metod, które ułatwiają stronicowania. **PagedList.Mvc** pakiet Instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.

1. Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** i następnie **Konsola Menedżera pakietów**.

2. W **Konsola Menedżera pakietów** okna, upewnij się, że **źródła pakietu** jest **nuget.org** i **projekt domyślny** jest **ContosoUniversity**, a następnie wprowadź następujące polecenie:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Skompiluj projekt.

### <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do Index — metoda

1. W *Controllers\StudentController.cs*, Dodaj `using` poufności informacji dotyczące `PagedList` przestrzeni nazw:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Zastąp `Index` metoda następującym kodem:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Ten kod dodaje `page` parametr, bieżący parametr kolejność sortowania i bieżący parametr filtru do sygnatury metody:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknie, stronicowanie i sortowanie łącza, wszystkie parametry mają wartość null. Po kliknięciu łącza stronicowania `page` zmienna zawiera numer strony, aby wyświetlić.

   A `ViewBag` właściwości zapewnia widok z bieżącej kolejności sortowania, ponieważ ten musi być uwzględniona w łącza stronicowania Aby zachować porządek sortowania, taki sam, podczas stronicowania:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Inna właściwość `ViewBag.CurrentFilter`, zawiera widok bieżący ciąg filtru. Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i musi on zostać przywrócony do pola tekstowego po stronie zostanie wyświetlony ponownie. Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strony musi być ustawiony na 1, nowy filtr może powodować różne dane do wyświetlenia. Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i naciśnięciu przycisku Prześlij. W takim przypadku `searchString` parametru nie ma wartości null.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   Na końcu metody `ToPagedList` metody rozszerzenia na uczniów `IQueryable` obiektu konwertuje zapytań dla uczniów na pojedynczej strony uczniów na typ kolekcji, który obsługuje stronicowanie. Tego jednostronicowej studentów są następnie przekazywane do widoku:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   `ToPagedList` Metoda przyjmuje numeru strony. Dwa znaki zapytania reprezentują [operatorem łączenia wartości null](/dotnet/csharp/language-reference/operators/null-coalescing-operator). Operator łączenia wartości null określa wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(page ?? 1)` oznacza, że zwracają wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.

### <a name="add-paging-links-to-the-student-index-view"></a>Dodawanie stronicowania łączy do widoku indeksu dla uczniów

1. W *Views\Student\Index.cshtml*, Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   `@model` Instrukcji w górnej części strony określa widok teraz pobiera `PagedList` zamiast obiektu `List` obiektu.

   `using` Poufności informacji dotyczące `PagedList.Mvc` daje dostęp do pomocy MVC przycisków stronicowania.

   Kod używa przeciążenia [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) umożliwiająca, aby określić [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   Wartość domyślna [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) przesyła dane formularza z WPISEM, co oznacza, że parametry są przekazywane w treści komunikatu HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu HTTP GET, dane formularza jest przekazywany w adresie URL jako ciągi zapytań, która umożliwia użytkownikom utworzyć zakładkę ma adres URL. [W3C wytyczne dotyczące użytkowania HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) zaleca się, powinien być używany GET akcja nie powoduje aktualizacji.

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

2. Uruchom stronę.

   Po kliknięciu łączy stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania. Następnie wprowadź wyszukiwany ciąg i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie przy użyciu sortowania i filtrowania.

## <a name="create-an-about-page"></a>Tworzenie strony informacje

Dla firmy Contoso University witryny internetowej informacje o stronie będą wyświetlane, jak wiele studentów zostały zarejestrowane dla każdego dnia rejestracji. Wymaga to obliczeń grupowania i prostych grup. Aby to osiągnąć, wykonasz następujące czynności:

- Utwórz klasę modelu widoku danych potrzebnych do przekazania do widoku.
- Modyfikowanie `About` method in Class metoda `Home` kontrolera.
- Modyfikowanie `About` widoku.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Tworzenie *modele widoków* folderu w folderze projektu. W tym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu poniższym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie kontrolera głównego

1. W *HomeController.cs*, Dodaj następujący kod `using` instrukcji w górnej części pliku:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Zastąp `About` metoda następującym kodem:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   Instrukcji LINQ grup jednostek uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w zbiorze `EnrollmentDateGroup` wyświetlić obiekty w modelu.

4. Dodaj `Dispose` metody:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Zmodyfikuj widok — informacje

1. Zastąp kod w *Views\Home\About.cshtml* pliku następującym kodem:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Uruchom aplikację, a następnie kliknij przycisk **o** łącza.

   Liczba studentów każdej daty rejestracji wyświetla się w tabeli.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Pobierz kod

[Pobieranie ukończone projektu](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodaj kolumnę sortowania łącza
> * Dodawanie pola wyszukiwania
> * Dodawanie stronicowania
> * Tworzenie strony informacje

Przejdź do następnego artykułu, aby dowiedzieć się, jak używać połączenia przejmowanie odporność i polecenia.
> [!div class="nextstepaction"]
> [Przejmowanie odporność i polecenia połączenia](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)