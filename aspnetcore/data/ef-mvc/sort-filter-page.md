---
title: 'Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie — ASP.NET MVC z programem EF Core'
description: W tym samouczku dodasz sortowanie, filtrowanie i stronicowanie funkcji do strony indeksu studentów. Utworzysz też strony, która wykonuje prostą grupowania.
author: rick-anderson
ms.author: tdykstra
ms.date: 02/04/2019
ms.topic: tutorial
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 51b6b08d2410652f93427371aec299eb4c8789f1
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56103062"
---
# <a name="tutorial-add-sorting-filtering-and-paging---aspnet-mvc-with-ef-core"></a>Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie — ASP.NET MVC z programem EF Core

W poprzednim samouczku wdrożono zestaw stron sieci web w przypadku podstawowych operacji CRUD dla jednostek dla uczniów. W tym samouczku dodasz sortowanie, filtrowanie i stronicowanie funkcji do strony indeksu studentów. Utworzysz też strony, która wykonuje prostą grupowania.

Na poniższej ilustracji przedstawiono wygląd strony po zakończeniu. Nagłówki kolumn odnoszą się łącza, które użytkownik może kliknąć, aby posortować według tej kolumny. Klikając kolumnę nagłówka wielokrotnie przełącza między malejącą i rosnącą porządek sortowania.

![Strona indeksu uczniów](sort-filter-page/_static/paging.png)

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodaj kolumnę sortowania łącza
> * Dodawanie pola wyszukiwania
> * Dodawanie stronicowania do indeksu uczniów
> * Dodawanie stronicowania do Index — metoda
> * Dodawanie łączy stronicowania
> * Tworzenie strony informacje

## <a name="prerequisites"></a>Wymagania wstępne

* [Implementowanie funkcji CRUD z programem EF Core w aplikacji internetowej ASP.NET Core MVC](crud.md)

## <a name="add-column-sort-links"></a>Dodaj kolumnę sortowania łącza

Aby dodać sortowanie do strony indeksu dla uczniów, zmienisz `Index` metody kontrolera studentów i Dodaj kod do widoku indeksu dla uczniów.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodaj sortowanie funkcjonalność do Index — metoda

W *StudentsController.cs*, Zastąp `Index` metoda następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Wartość ciągu zapytania jest dostarczane przez platformę ASP.NET Core MVC jako parametr do metody akcji. Parametr będzie ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia i ciąg "desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

Przy pierwszym żądaniu strony indeksu nie jest Brak ciągu zapytania. Studenci są wyświetlane w kolejności rosnącej według nazwiska, co jest ustawieniem domyślnym, zgodnie z ustaleniami w należą do przypadku `switch` instrukcji. Kiedy użytkownik kliknie hiperlink nagłówek kolumny, odpowiednie `sortOrder` podana w ciągu zapytania.

Dwa `ViewData` do konfigurowania hiperłącza nagłówek kolumny przy użyciu wartości ciągu zapytania odpowiednie elementy (NameSortParm i DateSortParm) są używane w widoku.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Są to trójargumentowy instrukcji. Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub pusty, NameSortParm powinna być równa "name_desc"; w przeciwnym razie powinien być ustawiony na pusty ciąg. Te dwie instrukcje Włącz widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:

|  Bieżący kolejność sortowania  | Ostatnia nazwa hiperłącza | Data hiperłącza |
|:--------------------:|:-------------------:|:--------------:|
| Ostatnie nazwy, rosnąco  | descending          | ascending      |
| Ostatnie nazwy, malejąco | ascending           | ascending      |
| Data, w kolejności rosnącej       | ascending           | descending     |
| Data, malejąco      | ascending           | ascending      |

Metoda używa składnik LINQ to Entities, aby określić kolumnę sortowania. Ten kod tworzy `IQueryable` zmiennej przed instrukcją switch modyfikuje go w instrukcji switch i wywołania `ToListAsync` metody `switch` instrukcji. Podczas tworzenia i modyfikowania `IQueryable` zmiennych, bez określenia zapytania są wysyłane do bazy danych. Zapytanie nie jest wykonywane, dopóki konwersji `IQueryable` obiektu w kolekcji przez wywołanie metody, takie jak `ToListAsync`. W związku z tym, ten kod powoduje pojedyncze zapytanie, które nie jest wykonywane, dopóki `return View` instrukcji.

Ten kod można pobrać pełny z dużą liczbą kolumn. [Ostatni samouczek z tej serii](advanced.md#dynamic-linq) pokazuje, jak napisać kod, który pozwala przekazywać nazwę jednostki `OrderBy` kolumny w zmiennej ciągu.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodawanie hiperłączy nagłówek kolumny do widoku indeksu dla uczniów

Zastąp kod w *Views/Students/Index.cshtml*, z następującym kodem, aby dodać hiperlinki nagłówek kolumny. Zmienione wiersze są wyróżnione.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Ten kod używa tych informacji w `ViewData` właściwości, aby skonfigurować hiperlinki z zapytaniem odpowiednie wartości ciągu.

Uruchom aplikację, wybierz **studentów** kartę, a następnie kliknij przycisk **nazwisko** i **Data rejestracji** nagłówków kolumn, aby zweryfikować, że sortowanie działa.

![Strona indeksu studentów według nazwy](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box"></a>Dodawanie pola wyszukiwania

Aby dodać filtrowanie do strony indeksu studentów, dodasz pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody. Pole tekstowe umożliwi wprowadź ciąg do wyszukania w imię pola imienia i nazwiska.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

W *StudentsController.cs*, Zastąp `Index` metoda następującym kodem (zmiany zostały wyróżnione).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Po dodaniu `searchString` parametr `Index` metody. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, które należy dodać do widoku indeksu. Możesz również dodane do instrukcji LINQ where — klauzula, który wybiera tylko uczniowie, w których imię lub nazwisko zawiera ciąg wyszukiwania. Instrukcja, która dodaje where — klauzula jest wykonywany tylko wtedy, gdy wartość do wyszukania.

> [!NOTE]
> W tym miejscu wywołujesz `Where` metody `IQueryable` obiektu i filtr będą przetwarzane na serwerze. W niektórych scenariuszach może być wywołanie `Where` metodę jako metodę rozszerzenia w kolekcji w pamięci. (Na przykład, załóżmy, że możesz zmienić odwołanie do `_context.Students` tak, to zamiast elementu EF `DbSet` odwołuje się do metody repozytorium, która zwraca `IEnumerable` kolekcji.) Wynik będzie zazwyczaj taki sam, ale w niektórych przypadkach może być inna.
>
>Na przykład implementacji .NET Framework z `Contains` metoda wykonuje porównania uwzględniającego wielkość liter, domyślnie, ale w programie SQL Server jest to określane przez ustawienia sortowania wystąpienia programu SQL Server. To ustawienie domyślne pozycji bez uwzględniania wielkości liter. Można wywołać `ToUpper` metody testu jawnie bez uwzględniania wielkości liter:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*. Które zapewniają, że wyniki pozostają takie same, w przypadku zmiany kodu później, aby korzystać z repozytorium, które zwraca `IEnumerable` zbiór zamiast `IQueryable` obiektu. (Gdy wywołujesz `Contains` metody `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; Jeśli wywołasz ją na `IQueryable` obiektu, możesz uzyskać implementację dostawcy bazy danych.) Jednak jest zmniejszenie wydajności dla tego rozwiązania. `ToUpper` Kodu umieścić funkcję w klauzuli WHERE w instrukcji TSQL SELECT. Które uniemożliwiłyby Optymalizator przy użyciu indeksu. Biorąc pod uwagę, że program SQL przede wszystkim jest zainstalowany jako bez uwzględniania wielkości liter, zaleca się unikać `ToUpper` kodu do momentu migracji do magazynu danych z uwzględnieniem wielkości liter.

### <a name="add-a-search-box-to-the-student-index-view"></a>Dodaj pole wyszukiwania do widoku indeksu dla uczniów

W *Views/Student/Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed wykonaniem otwarcia tag tabeli, aby można było utworzyć podpis, pola tekstowego, a **wyszukiwania** przycisku.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Ten kod używa `<form>` [pomocnika tagów](xref:mvc/views/tag-helpers/intro) można dodać pole tekstowe wyszukiwania i przycisku. Domyślnie `<form>` Pomocnik tagu przesyła dane formularza z WPISEM, co oznacza, że parametry są przekazywane w treści komunikatu HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu HTTP GET, dane formularza jest przekazywany w adresie URL jako ciągi zapytań, która umożliwia użytkownikom utworzyć zakładkę ma adres URL. Zaleca się wytyczne dotyczące W3C, która powinna być używana korzystać z niezrównanej akcja nie powoduje aktualizacji.

Uruchom aplikację, wybierz **studentów** kartę, wprowadź ciąg wyszukiwania i kliknij przycisk Wyszukaj, aby sprawdzić, czy działa filtrowanie.

![Strona indeksu studentów z filtrowaniem](sort-filter-page/_static/filtering.png)

Należy zauważyć, że adres URL zawiera ciąg wyszukiwania.

```html
http://localhost:5813/Students?SearchString=an
```

Jeśli Oznacz tę stronę zakładką uzyskasz filtrowana lista korzystając z zakładki. Dodawanie `method="get"` do `form` tag jest, co spowodowało ciąg zapytania do wygenerowania.

Na tym etapie, po kliknięciu łącza sortowania nagłówka kolumny utracisz wartość filtru, które wprowadziłeś w **wyszukiwania** pole. Można to naprawić w następnej sekcji.

## <a name="add-paging-to-students-index"></a>Dodawanie stronicowania do indeksu uczniów

Aby dodać stronicowania do strony indeksu uczniów, należy utworzyć `PaginatedList` klasy, która używa `Skip` i `Take` instrukcje, aby filtrować dane na serwerze zamiast zawsze pobierać wszystkie wiersze z tabeli. Będzie wprowadzić dodatkowe zmiany w `Index` metody i dodać przyciski stronicowania `Index` widoku. Poniższa ilustracja przedstawia przyciski stronicowania.

![Studenci indeksu stronę linkami stronicowania](sort-filter-page/_static/paging.png)

W folderze projektu, należy utworzyć `PaginatedList.cs`, a następnie Zastąp kod szablonu poniższym kodem.

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

`CreateAsync` Metoda ten kod pobiera rozmiar strony i numer strony i stosuje odpowiednie `Skip` i `Take` instrukcje `IQueryable`. Gdy `ToListAsync` jest wywoływana w `IQueryable`, to zostanie zwrócona lista zawierająca tylko żądanej strony. Właściwości `HasPreviousPage` i `HasNextPage` pozwala włączyć lub wyłączyć **Wstecz** i **dalej** stronicowania przycisków.

A `CreateAsync` metoda jest używana zamiast konstruktora, aby utworzyć `PaginatedList<T>` obiektu, ponieważ konstruktory nie można uruchomić kod asynchroniczny.

## <a name="add-paging-to-index-method"></a>Dodawanie stronicowania do Index — metoda

W *StudentsController.cs*, Zastąp `Index` metoda następującym kodem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Ten kod dodaje parametr numer strony, bieżący parametr kolejność sortowania i bieżącego parametru filtru w podpisie metody.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknie, stronicowanie i sortowanie łącza, wszystkich parametrów będzie pusta.  Po kliknięciu łącza stronicowania zmienną strony będzie zawierać numer strony, aby wyświetlić.

`ViewData` Elementu o nazwie CurrentSort zapewnia widok z bieżącej kolejności sortowania, ponieważ ten musi być uwzględniona w łącza stronicowania Aby zachować porządek sortowania, taki sam, podczas stronicowania.

`ViewData` Elementu o nazwie BieżącyFiltr zawiera widok bieżący ciąg filtru. Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i musi on zostać przywrócony do pola tekstowego po stronie zostanie wyświetlony ponownie.

Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strony musi być ustawiony na 1, nowy filtr może powodować różne dane do wyświetlenia. Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i naciśnięciu przycisku Prześlij. W takim przypadku `searchString` parametru nie ma wartości null.

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

Na koniec `Index` metody `PaginatedList.CreateAsync` metoda konwertuje zapytań dla uczniów na pojedynczej strony uczniów na typ kolekcji, który obsługuje stronicowanie. Tego jednostronicowej studentów są następnie przekazywane do widoku.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync` Metoda przyjmuje numeru strony. Dwa znaki zapytania reprezentują operatora łączenia wartości null. Operator łączenia wartości null określa wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(page ?? 1)` oznacza, że zwracają wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.

## <a name="add-paging-links"></a>Dodawanie łączy stronicowania

W *Views/Students/Index.cshtml*, Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

`@model` Instrukcji w górnej części strony określa widok teraz pobiera `PaginatedList<T>` zamiast obiektu `List<T>` obiektu.

Linki nagłówek kolumny, użyj ciągu zapytania do przekazania aktualnie wyszukiwanego ciągu do kontrolera, dzięki czemu użytkownik może sortować w ramach wyników filtrowania:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Przyciski stronicowania są wyświetlane przez pomocników tagów:

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Uruchom aplikację, a następnie przejdź do strony studentów.

![Studenci indeksu stronę linkami stronicowania](sort-filter-page/_static/paging.png)

Po kliknięciu łączy stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania. Następnie wprowadź wyszukiwany ciąg i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie przy użyciu sortowania i filtrowania.

## <a name="create-an-about-page"></a>Tworzenie strony informacje

Dla witryny internetowej firmy Contoso University **o** stronie będą wyświetlane, jak wiele studentów zostały zarejestrowane dla każdego dnia rejestracji. Wymaga to obliczeń grupowania i prostych grup. Aby to osiągnąć, wykonasz następujące czynności:

* Utwórz klasę modelu widoku danych potrzebnych do przekazania do widoku.

* Zmodyfikuj metodę informacje w kontrolera głównego.

* Zmodyfikuj widok informacje.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Tworzenie *SchoolViewModels* folderu w *modeli* folderu.

W nowym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu poniższym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie kontrolera głównego

W *HomeController.cs*, Dodaj następujące instrukcje using na początku pliku:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy i uzyskiwanie wystąpienia kontekstu platformy ASP.NET Core DI:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Zastąp `About` metoda następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

Instrukcji LINQ grup jednostek uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w zbiorze `EnrollmentDateGroup` wyświetlić obiekty w modelu.
> [!NOTE]
> W wersji 1.0 programu Entity Framework Core cały zestaw wyników jest zwracany do klienta, a grupowania odbywa się na komputerze klienckim. W niektórych scenariuszach utworzyć to, problemy z wydajnością. Należy przetestować wydajność produkcji ilości danych i w razie potrzeby użyj pierwotne SQL celu grupowania na serwerze. Uzyskać informacji o sposobie używania pierwotne SQL, zobacz [ostatni samouczek z tej serii](advanced.md).

### <a name="modify-the-about-view"></a>Zmodyfikuj widok — informacje

Zastąp kod w *Views/Home/About.cshtml* pliku następującym kodem:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Uruchom aplikację, a następnie przejdź do strony informacje. Liczba studentów każdej daty rejestracji jest wyświetlany w tabeli.

## <a name="get-the-code"></a>Pobierz kod

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodana kolumna sortowania łącza
> * Dodano pole wyszukiwania
> * Dodano stronicowania do indeksu uczniów
> * Dodano stronicowania do Index — metoda
> * Dodano linki stronicowania
> * Utworzona na stronie informacje

Przejdź do następnego artykułu, aby dowiedzieć się, jak obsługiwać zmiany w modelu danych przy użyciu migracji.
> [!div class="nextstepaction"]
> [Obsługa zmiany w modelu danych](migrations.md)
