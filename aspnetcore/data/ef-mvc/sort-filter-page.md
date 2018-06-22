---
title: Platformy ASP.NET Core MVC EF podstawowych — sortowanie, filtrowanie, stronicowania - 3 10
author: rick-anderson
description: W tym samouczku zostanie dodana sortowanie, filtrowanie i stronicowania funkcji na stronę przy użyciu programu Entity Framework Core i ASP.NET Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 34097eacad16c0ffb989efb3b6a8656be4a076cd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273653"
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a>Platformy ASP.NET Core MVC EF podstawowych — sortowanie, filtrowanie, stronicowania - 3 10

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

W samouczku poprzedniej zaimplementowano zestaw stron sieci web dla podstawowych operacji CRUD dla uczniów jednostek. W tym samouczku zostanie dodany do strony indeksu uczniów lub studentów sortowanie, filtrowanie i funkcje stronicowania. Utworzysz także strona, która jest prosta grupa.

Na poniższej ilustracji przedstawiono wygląd strony po zakończeniu. Nagłówki kolumn są łącza, które użytkownik może kliknąć, aby sortować według tej kolumny. Kliknięcie nagłówka wielokrotnie kolumny przełączanie się między kolumny sortowania.

![Strona indeksu uczniów lub studentów](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Dodaj kolumny sortowania łącza do strony indeksu uczniów lub studentów

Aby dodać sortowanie do strony indeksu dla użytkowników domowych, zostanie zmieniona `Index` kontrolera studentów i Dodaj kod do widoku indeksu studenta.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodaj sortowania funkcji metody indeksu

W *StudentsController.cs*, Zastąp `Index` metodę z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Wartość ciągu kwerendy są udostępniane przez program ASP.NET Core MVC jako parametr do metody akcji. Parametr będzie ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia, a ciąg "desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

Żądana strona indeksu, po raz pierwszy nie jest Brak ciągu zapytania. Studenci są wyświetlane w kolejności rosnącej przez nazwisko, która jest ustawiona domyślnie zgodnie z ustaleniami w przypadku fall-through `switch` instrukcji. Gdy użytkownik klika hiperłącze nagłówek kolumny, odpowiedni `sortOrder` wartość jest podana w ciągu zapytania.

Dwa `ViewData` elementów (NameSortParm i DateSortParm) są używane przez widok skonfigurowanie hiperłącza nagłówek kolumny z wartości typu QueryString odpowiednie.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Są to trójargumentowy instrukcje. Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub pusty, NameSortParm powinien być ustawiony na "name_desc"; w przeciwnym razie powinien być ustawiony na pusty ciąg. Te dwie instrukcje włączyć widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:

|  Bieżącej kolejności sortowania  | Ostatnia nazwa hiperłącza | Data hiperłącza |
|:--------------------:|:-------------------:|:--------------:|
| Ostatni nazwy, rosnąco  | descending          | ascending      |
| Ostatni nazwy, malejąco | ascending           | ascending      |
| Data w kolejności rosnącej       | ascending           | descending     |
| Data, malejąco      | ascending           | ascending      |

Metoda używa do składnika LINQ to Entities Określ kolumny, aby posortować według. Kod tworzy `IQueryable` zmiennej przed instrukcją switch modyfikuje go w instrukcji switch i wywołania `ToListAsync` metody po `switch` instrukcji. Podczas tworzenia i modyfikowania `IQueryable` zmiennych, nie zapytanie jest wysyłane do bazy danych. Zapytanie nie jest wykonywany do momentu konwersji `IQueryable` obiektu do kolekcji, wywołując metodę, takich jak `ToListAsync`. W związku z tym powoduje ten kod w jednym zapytaniu, która nie jest wykonywana do czasu `return View` instrukcji.

Ten kod może pobrać verbose z dużą liczbą kolumn. [Ostatni samouczku tej serii](advanced.md#dynamic-linq) pokazano, jak napisać kod, który umożliwia przekazywania nazwy `OrderBy` kolumny w zmiennej ciągu.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodawanie hiperłączy nagłówek kolumny do widoku indeksu dla użytkowników domowych

Zastąp kod w *Views/Students/Index.cshtml*, z następujący kod, aby dodać hiperłącza nagłówka kolumny. Wyróżniono zmienionych wierszy.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Ten kod używa tych informacji w `ViewData` właściwości do skonfigurowania hiperłącza z zapytaniem odpowiednie ciągu wartości.

Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **nazwisko** i **Data rejestracji** działa nagłówki kolumn, aby zweryfikować, że sortowanie.

![Strona indeksu studentów według nazwy](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Dodaj pole wyszukiwania do strony indeksu uczniów lub studentów

Aby dodać filtrowanie do strony indeksu studentów, będzie Dodaj pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody. Pole tekstowe umożliwi wprowadź ciąg do wyszukania w imię pola imienia i nazwiska.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

W *StudentsController.cs*, Zastąp `Index` metodę z następującym kodem (zmiany zostały wyróżnione).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Dodano `searchString` parametr `Index` metody. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, która zostanie dodana do widoku indeksu. Również dodane do instrukcji LINQ where klauzuli, który wybiera tylko studentów, w których imię lub nazwisko zawiera ciąg wyszukiwania. Instrukcja, która dodaje where klauzuli jest wykonywane tylko wtedy, gdy wartość do wyszukania.

> [!NOTE]
> W tym miejscu są nawiązywane `Where` metoda `IQueryable` obiektu i filtr będą przetwarzane na serwerze. W niektórych scenariuszach może być wywołanie `Where` metody jako metodę rozszerzenie w kolekcji w pamięci. (Na przykład, załóżmy, że możesz zmienić odwołanie do `_context.Students` tak to zamiast elementu EF `DbSet` odwołuje się do metody repozytorium, która zwraca `IEnumerable` kolekcji.) Wynik zazwyczaj będzie taki sam, ale w niektórych przypadkach może się różnić.
>
>Na przykład, .NET Framework wykonania `Contains` metoda przeprowadza porównanie z uwzględnieniem wielkości liter domyślnie, ale w programie SQL Server jest to określane przez ustawienie sortowania wystąpienia programu SQL Server. To ustawienie domyślne pozycji bez uwzględniania wielkości liter. Można wywołać `ToUpper` metody, aby jawnie bez uwzględniania wielkości liter testu: *gdzie (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Które zapewniają, że wyniki nie zmieniają się po zmianie kod później do korzystania z repozytorium, która zwraca `IEnumerable` kolekcji zamiast `IQueryable` obiektu. (Podczas wywoływania `Contains` metoda `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; podczas wywoływania go na `IQueryable` obiektu, możesz uzyskać implementacji dostawcy bazy danych.) Istnieje jednak zmniejszenie wydajności dla tego rozwiązania. `ToUpper` Kod spowodowałaby funkcji w klauzuli WHERE instrukcji TSQL SELECT. Które uniemożliwiłyby Optymalizator przy użyciu indeksu. Biorąc pod uwagę, że program SQL jest zainstalowany przede wszystkim jako bez uwzględniania wielkości liter, jest unikanie `ToUpper` kodu do momentu migracji w magazynie danych z uwzględnieniem wielkości liter.

### <a name="add-a-search-box-to-the-student-index-view"></a>Dodaj pole wyszukiwania do widoku indeksu dla użytkowników domowych

W *Views/Student/Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed otwarcia tag tabeli, aby można było utworzyć podpis pola tekstowego, a **wyszukiwania** przycisku.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Ten kod zawiera `<form>` [pomocnika tagów](xref:mvc/views/tag-helpers/intro) można dodać pola tekstowego wyszukiwania i przycisku. Domyślnie `<form>` pomocnika tagów przesyła dane formularza przy użyciu metody POST, co oznacza, że parametry są przekazywane w treści wiadomości HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu HTTP GET, formularza dane są przekazywane w adresie URL jako ciągi zapytania, który umożliwia użytkownikom zakładki adres URL. Akcja nie powoduje aktualizacji UZYSKAĆ zalecane wytyczne W3C, która powinna być używana.

Uruchom aplikację, wybierz **studentów** karcie, wprowadź ciąg wyszukiwania, a następnie kliknij przycisk Wyszukaj, aby zweryfikować filtrowania.

![Strona indeksu studentów z filtrowania](sort-filter-page/_static/filtering.png)

Zwróć uwagę, że adres URL zawiera ciąg wyszukiwania.

```html
http://localhost:5813/Students?SearchString=an
```

Jeśli Oznacz tę stronę zakładką uzyskasz wyfiltrowanej listy używania zakładki. Dodawanie `method="get"` do `form` tag jest przyczyn ciąg zapytania do wygenerowania.

Na tym etapie po kliknięciu łącza sortowania nagłówka kolumny zostanie utracony wartości filtru, wprowadzony w **wyszukiwania** pole. Użytkownik będzie rozwiązać ten problem w następnej sekcji.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Dodawanie funkcji stronicowania do strony indeksu uczniów lub studentów

Aby dodać stronicowania do strony indeksu studentów, należy utworzyć `PaginatedList` klasy, która używa `Skip` i `Take` instrukcje, aby filtrować dane na serwerze zamiast zawsze pobierania wszystkie wiersze w tabeli. Następnie będzie można wprowadzić dodatkowe zmiany w `Index` — metoda i dodać przyciski stronicowania `Index` widoku. Na poniższej ilustracji przedstawiono przyciski stronicowania.

![Strona indeksu studentów z łączami stronicowania](sort-filter-page/_static/paging.png)

W folderze projektu Utwórz `PaginatedList.cs`, a następnie Zastąp kod szablonu z następującym kodem.

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

`CreateAsync` Metoda ten kod pobiera rozmiar strony i numer strony i zastosowanie odpowiednich `Skip` i `Take` instrukcje `IQueryable`. Gdy `ToListAsync` jest wywoływana na `IQueryable`, to zostanie zwrócona lista zawierająca żądanej strony. Właściwości `HasPreviousPage` i `HasNextPage` pozwala włączyć lub wyłączyć **Wstecz** i **dalej** stronicowania przycisków.

A `CreateAsync` zamiast konstruktora metodę, aby utworzyć `PaginatedList<T>` obiektu, ponieważ konstruktorów nie można uruchomić kod asynchroniczny.

## <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do Index — metoda

W *StudentsController.cs*, Zastąp `Index` metodę z następującym kodem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Ten kod dodaje parametr numer strony, bieżącego parametru kolejność sortowania i bieżącego parametru filtru w podpisie metody.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknął stronicowania i sortowania łącza, wszystkie parametry będzie miała wartość null.  Po kliknięciu łącza stronicowania zmienną strony będzie zawierać numer strony do wyświetlenia.

`ViewData` Elementu o nazwie CurrentSort zapewnia widok z bieżącej kolejności sortowania, ponieważ to musi być uwzględniona w łącza stronicowania aby zapewnić porządek sortowania, taka sama podczas stronicowania.

`ViewData` Elementu o nazwie BieżącyFiltr zawiera widok z bieżącym ciąg filtru. Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i go muszą być przywrócone do pola tekstowego, gdy strona zostanie wyświetlony ponownie.

Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strona musi zresetowana do wartości 1, nowy filtr może powodować różnych danych do wyświetlenia. Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i kliknięciu przycisku Prześlij. W takim przypadku `searchString` parametru nie ma wartości null.

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

Na koniec `Index` metody `PaginatedList.CreateAsync` metoda konwertuje zapytania uczniów do pojedynczej strony studentów w typ kolekcji, który obsługuje stronicowanie. Tej stronie studentów są następnie przekazywane do widoku.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync` Metoda przyjmuje numer strony. Dwa znaki zapytania reprezentowania operatora łączenia wartości null. Wartość domyślna dla typu dopuszczającego wartość null; definiuje operator łączenia wartości null wyrażenie `(page ?? 1)` oznacza zwrócić wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.

## <a name="add-paging-links-to-the-student-index-view"></a>Dodawanie łączy stronicowania w widoku indeksu dla użytkowników domowych

W *Views/Students/Index.cshtml*, Zastąp istniejący kod następującym kodem. Zmiany zostały wyróżnione.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

`@model` Instrukcji w górnej części strony określa, czy widok teraz pobiera `PaginatedList<T>` obiekt zamiast `List<T>` obiektu.

Łącza nagłówka kolumny Użyj ciągu zapytania do przekazania do kontrolera aktualnie wyszukiwanego ciągu, dzięki czemu użytkownik może sortować w wynikach filtrowania:

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

Uruchom aplikację i przejdź do strony studenta.

![Strona indeksu studentów z łączami stronicowania](sort-filter-page/_static/paging.png)

Kliknij łącza stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania. Następnie wprowadź ciąg wyszukiwania i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie z sortowania i filtrowania.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Tworzenie strony informacje, które znajdują się dane statystyczne uczniów

Dla witryny internetowej firmy Contoso University **o** strony, będzie wyświetlane, jak wiele studentów zostały zarejestrowane dla każdego dnia rejestracji. Wymaga to prosty i grupowania obliczeń grup. W tym celu będzie wykonaj następujące czynności:

* Utwórz klasę modelu widoku danych, która należy do przekazania do widoku.

* Zmodyfikuj metodę informacje w kontrolerze głównej.

* Zmodyfikuj widok informacje.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Utwórz *SchoolViewModels* folderu w *modele* folderu.

W nowym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu z następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie macierzystego kontrolera

W *HomeController.cs*, Dodaj następujące instrukcje using na początku pliku:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym klasy i pobrać wystąpienia kontekstu z platformy ASP.NET Core Podpisane.

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Zastąp `About` metodę z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

Instrukcja LINQ grupy jednostek uczniowie według daty rejestracji oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w kolekcji z `EnrollmentDateGroup` wyświetlić obiekty modelu.
> [!NOTE] 
> W wersji 1.0 programu Entity Framework Core cały zestaw wyników jest zwracana do klienta, a operacji grupowania na kliencie. W niektórych scenariuszach to można utworzyć problemy z wydajnością. Pamiętaj testu wydajności przy użyciu produkcyjnego ilości danych, a w razie potrzeby umożliwia wykonaj grupowanie na serwerze raw SQL. Informacje o sposobie używania pierwotnych SQL, zobacz [ostatniego samouczku tej serii](advanced.md).

### <a name="modify-the-about-view"></a>Zmodyfikuj widok — informacje

Zastąp kod w *Views/Home/About.cshtml* pliku następującym kodem:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Uruchom aplikację i przejdź do strony informacje. Liczba studentów dla każdego dnia rejestracji jest wyświetlane w formie tabeli.

![Strona — informacje](sort-filter-page/_static/about.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób wykonywania sortowanie, filtrowanie, stronicowania i grupowania. W następnym samouczku nauczysz się, jak obsługiwać zmiany modelu danych przy użyciu migracji.

> [!div class="step-by-step"]
> [Poprzednie](crud.md)
> [dalej](migrations.md)  
