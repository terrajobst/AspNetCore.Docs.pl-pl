---
title: Strony razor z programem EF Core w programie ASP.NET Core — sortowanie, filtrowanie, stronicowania - 3, 8
author: rick-anderson
description: W tym samouczku dodasz, sortowanie, filtrowanie i stronicowanie funkcjonalność do strony, przy użyciu platformy ASP.NET Core i Entity Framework Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 19fe24e0f901c50e8425db7665b5b2257b608146
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090888"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>Strony razor z programem EF Core w programie ASP.NET Core — sortowanie, filtrowanie, stronicowania - 3, 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Jan Kowalski P](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Ten samouczek, sortowanie, filtrowanie, grupowanie i stronicowania funkcje dodawana jest.

Poniższa ilustracja przedstawia strony ukończone. Nagłówki kolumn odnoszą się łącza możesz klikać, aby posortować kolumnę. Klikając kolumnę nagłówka wielokrotnie przełącza między malejącą i rosnącą porządek sortowania.

![Strona indeksu uczniów](sort-filter-page/_static/paging.png)

Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="add-sorting-to-the-index-page"></a>Dodaj sortowanie do strony indeksu

Dodaj parametry do *Students/Index.cshtml.cs* `PageModel` zawierać parametry sortowania:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]

Aktualizacja *Students/Index.cshtml.cs* `OnGetAsync` następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Powyższy kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Adres URL (w tym ciągu zapytania) jest generowany przez [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

`sortOrder` Parametr jest "Name" i "Date". `sortOrder` Parametr opcjonalnie następuje "_desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

Po żądaniu strony indeksu z **studentów** połączyć, nie ma żadnych ciągu zapytania. Studenci są wyświetlane w kolejności rosnącej według nazwiska. Kolejności rosnącej według nazwiska jest ustawieniem domyślnym ("należą do wielkości liter) w `switch` instrukcji. Kiedy użytkownik kliknie łącze nagłówek kolumny, odpowiednie `sortOrder` wartość znajduje się w wartości ciągu zapytania.

`NameSort` i `DateSort` są używane przez stronę Razor do skonfigurowania hiperłącza nagłówek kolumny przy użyciu wartości ciągu zapytania odpowiednie:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Poniższy kod zawiera warunkowe C# [?: operator](/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

Pierwszy wiersz określa, że w przypadku `sortOrder` ma wartość null lub jest pusta, `NameSort` jest ustawiona na "name_desc." Jeśli `sortOrder` jest **nie** o wartości null lub jest pusty, `NameSort` jest ustawiony na pusty ciąg.

`?: operator` Jest także znana jako operator trójargumentowy.

Te dwie instrukcje Włącz strony Aby ustawić hiperłącza nagłówek kolumny w następujący sposób:

| Bieżący kolejność sortowania | Ostatnia nazwa hiperłącza | Data hiperłącza |
|:--------------------:|:-------------------:|:--------------:|
| Ostatnie nazwy, rosnąco | descending        | ascending      |
| Ostatnie nazwy, malejąco | ascending           | ascending      |
| Data, w kolejności rosnącej       | ascending           | descending     |
| Data, malejąco      | ascending           | ascending      |

Metoda używa składnik LINQ to Entities, aby określić kolumnę sortowania. Inicjuje kod `IQueryable<Student>` przed instrukcją switch i modyfikuje go w instrukcji switch:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 Gdy`IQueryable` zostanie utworzony lub zmodyfikowany, bez określenia zapytania są wysyłane do bazy danych. Zapytanie nie jest wykonywane aż do `IQueryable` obiekt jest konwertowany na kolekcję. `IQueryable` są konwertowane do kolekcji przez wywołanie metody, takie jak `ToListAsync`. W związku z tym `IQueryable` kodu wyników w ramach jednego zapytania, który nie jest wykonywane, dopóki następującą instrukcję:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` można pobrać pełny z dużą liczbą sortowalne kolumny.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Dodawanie hiperłączy nagłówek kolumny do strony indeksu dla uczniów

Zastąp kod w *Students/Index.cshtml*, z następującymi wyróżniony kod:

[!code-html[](intro/samples/cu21/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Powyższy kod:

* Dodaje hiperłącza do `LastName` i `EnrollmentDate` nagłówki kolumn.
* Używa tych informacji w `NameSort` i `DateSort` skonfigurować hiperłącza bieżącymi wartościami kolejności sortowania.

Aby sprawdzić, które działa sortowania:

* Uruchom aplikację i wybierz **studentów** kartę.
* Kliknij przycisk **nazwisko**.
* Kliknij przycisk **Data rejestracji**.

Aby uzyskać lepsze zrozumienie kodu:

* W *Students/Index.cshtml.cs*, ustaw punkt przerwania na `switch (sortOrder)`.
* Dodaj wyrażenie kontrolne, aby uzyskać `NameSort` i `DateSort`.
* W *Students/Index.cshtml*, ustaw punkt przerwania na `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Przejść przez debuger.

## <a name="add-a-search-box-to-the-students-index-page"></a>Dodaj pole wyszukiwania do strony indeksu uczniów

Aby dodać filtrowanie do strony indeksu studentów:

* Pole tekstowe i przycisk przesyłania, zostanie dodany do strony Razor. Pole tekstowe dostarcza wyszukiwany ciąg na imię lub nazwisko.
* Modelu strony jest aktualizowana, aby użyć wartości pola tekstowe.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

Aktualizacja *Students/Index.cshtml.cs* `OnGetAsync` następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Powyższy kod:

* Dodaje `searchString` parametr `OnGetAsync` metody. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, który zostanie dodany do następnej sekcji.
* Dodane do instrukcji LINQ `Where` klauzuli. `Where` Klauzuli wybiera tylko uczniowie, w których imię lub nazwisko zawiera ciąg wyszukiwania. Instrukcji LINQ jest wykonywany tylko wtedy, gdy wartość do wyszukania.

Uwaga: Poprzedni kod wywołuje `Where` metody `IQueryable` obiektu i filtr jest przetwarzany na serwerze. W niektórych scenariuszach może być wywołanie aplikacji `Where` metodę jako metodę rozszerzenia w kolekcji w pamięci. Na przykład, załóżmy, że `_context.Students` zmieni się z programem EF Core `DbSet` metody repozytorium, które zwraca `IEnumerable` kolekcji. Wynik będzie zazwyczaj taki sam, ale w niektórych przypadkach może być inna.

Na przykład implementacji .NET Framework z `Contains` wykonuje porównania uwzględniającego wielkość liter, domyślnie. W programie SQL Server `Contains` uwzględnianie wielkości liter, jest określany przez ustawienie sortowania wystąpienia programu SQL Server. Program SQL Server domyślnie do bez uwzględniania wielkości liter. `ToUpper` może być wywoływana się jawnie bez uwzględniania wielkości liter testu:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Powyższy kod będzie upewnij się, że wyniki bez uwzględniania wielkości liter w przypadku zmian kodu, aby użyć `IEnumerable`. Gdy `Contains` jest wywoływana w `IEnumerable` kolekcji, implementacja jest użyta w programie .NET Core. Gdy `Contains` jest wywoływana w `IQueryable` obiektu jest używana implementacja bazy danych. Zwracanie `IEnumerable` z repozytorium może mieć penality istotnie poprawiającą wydajność:

1. Wszystkie wiersze są zwracane z serwera bazy danych.
1. Filtr jest stosowany do wszystkich wierszy zwróconych w aplikacji.

Występuje spadek wydajności, do wywoływania `ToUpper`. `ToUpper` Kod dodaje funkcję w klauzuli WHERE w instrukcji TSQL SELECT. Dodano funkcję zapobiega Optymalizator przy użyciu indeksu. Biorąc pod uwagę, że SQL jest zainstalowany jako bez uwzględniania wielkości liter, zaleca się unikać `ToUpper` wywołania, gdy nie jest potrzebna.

### <a name="add-a-search-box-to-the-student-index-page"></a>Dodaj pole wyszukiwania do strony indeksu dla uczniów

W *Pages/Students/Index.cshtml*, Dodaj następujący wyróżniony kod, aby utworzyć **wyszukiwania** przycisku i są formatowane przy chrome.

[!code-html[](intro/samples/cu21/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

W poprzednim kodzie użyto `<form>` [pomocnika tagów](xref:mvc/views/tag-helpers/intro) można dodać pole tekstowe wyszukiwania i przycisku. Domyślnie `<form>` Pomocnik tagu przesyła dane formularza z WPISEM. Wpis parametry są przekazywane w treści komunikatu HTTP, a nie w adresie URL. W przypadku HTTP GET dane formularza jest przekazywany w adresie URL jako ciągi zapytań. Przekazywanie danych z ciągami zapytań umożliwia użytkownikom utworzyć zakładkę ma adres URL. [Wytycznych W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) zaleca się, że GET powinny być używane w przypadku, gdy akcja nie powoduje aktualizacji.

Testowanie aplikacji:

* Wybierz **studentów** kartę, a następnie wprowadź ciąg wyszukiwania.
* Wybierz **wyszukiwania**.

Należy zauważyć, że adres URL zawiera ciąg wyszukiwania.

```html
http://localhost:5000/Students?SearchString=an
```

Jeśli strona jest zakładek, zakładki zawiera adres URL strony i `SearchString` ciągu zapytania. `method="get"` w `form` tag jest, co spowodowało ciąg zapytania do wygenerowania.

Obecnie po wybraniu łącza sortowania nagłówka kolumny filtru z wartości **wyszukiwania** pole zostanie utracony. Wartość filtru utracone został rozwiązany w następnej sekcji.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Dodawanie funkcji stronicowania do strony indeksu uczniów

W tej sekcji `PaginatedList` klasa jest tworzona do obsługi stronicowania. `PaginatedList` Klasy używa `Skip` i `Take` instrukcje, aby filtrować dane na serwerze, zamiast pobierać wszystkie wiersze z tabeli. Poniższa ilustracja przedstawia przyciski stronicowania.

![Studenci indeksu stronę linkami stronicowania](sort-filter-page/_static/paging.png)

W folderze projektu, należy utworzyć `PaginatedList.cs` następującym kodem:

[!code-csharp[](intro/samples/cu21/PaginatedList.cs)]

`CreateAsync` Metoda powyższy kod przyjmuje rozmiaru strony i numer strony i stosuje odpowiednie `Skip` i `Take` instrukcje `IQueryable`. Gdy `ToListAsync` jest wywoływana w `IQueryable`, funkcja zwraca listę zawierającą żądanej strony. Właściwości `HasPreviousPage` i `HasNextPage` są używane do włączania lub wyłączania **Wstecz** i **dalej** stronicowania przycisków.

`CreateAsync` Metoda służy do tworzenia `PaginatedList<T>`. Nie można utworzyć konstruktora `PaginatedList<T>` obiektu konstruktory nie można uruchomić kod asynchroniczny.

## <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do Index — metoda

W *Students/Index.cshtml.cs*, zaktualizuj typ `Student` z `IList<Student>` do `PaginatedList<Student>`:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Aktualizacja *Students/Index.cshtml.cs* `OnGetAsync` następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

Poprzedzający kod dodaje indeks strony bieżącego `sortOrder`i `currentFilter` w podpisie metody.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Wszystkie parametry mają wartość null jeśli:

* Strona jest wywoływana z **studentów** łącza.
* Użytkownik nie kliknie stronicowanie i sortowanie łącze.

Po kliknięciu łącza stronicowania, zmienna indeks strony zawiera numer strony, aby wyświetlić.

`CurrentSort` zawiera strony Razor za pomocą bieżącej kolejności sortowania. Bieżącej kolejności sortowania muszą być uwzględnione w linkach stronicowania, aby zachować porządek sortowania podczas stronicowania.

`CurrentFilter` Strona Razor zapewnia bieżący ciąg filtru. `CurrentFilter` Wartość:

* Aby zachować ustawienia filtru podczas stronicowania, muszą być zawarte w łącza stronicowania.
* Konieczne jest przywrócenie do pola tekstowego po stronie zostanie wyświetlony ponownie.

Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strona jest resetowany do 1. Strona musi można zresetować do 1, ponieważ nowy filtr może spowodować różnych danych do wyświetlenia. Po wprowadzeniu wartości wyszukiwania i **przesyłania** wybrano:

* Ciąg wyszukiwania została zmieniona.
* `searchString` Parametru nie ma wartości null.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync` Metoda konwertuje zapytań dla uczniów na pojedynczej strony uczniów na typ kolekcji, który obsługuje stronicowanie. Tego jednostronicowej studentów są przekazywane do stron Razor.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

Dwa znaki zapytania w `PaginatedList.CreateAsync` reprezentują [operatorem łączenia wartości null](/dotnet/csharp/language-reference/operators/null-conditional-operator). Operator łączenia wartości null określa wartość domyślną dla typu dopuszczającego wartość null. Wyrażenie `(pageIndex ?? 1)` oznacza, że zwracają wartość `pageIndex` ma wartość. Jeśli `pageIndex` nie ma wartości, zwraca 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Dodawanie stronicowania łączy do uczniów strony Razor

Aktualizowanie kodu znaczników w *Students/Index.cshtml*. Zmiany są wyróżnione:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

Linki nagłówek kolumny, użyj ciągu zapytania do przekazania aktualnie wyszukiwanego ciągu do `OnGetAsync` metody, dzięki czemu użytkownik może sortować w ramach wyników filtrowania:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=28-31)]

Przyciski stronicowania są wyświetlane przez pomocników tagów:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=72-)]

Uruchom aplikację i przejdź do strony studentów.

* Aby upewnić się, że działa stronicowania, po kliknięciu łączy stronicowania w poszczególnych kolejności sortowania.
* Aby sprawdzić, czy stronicowania działa poprawnie przy użyciu sortowania i filtrowania, wprowadź ciąg wyszukiwania, a następnie spróbuj stronicowania.

![Studenci indeksu stronę linkami stronicowania](sort-filter-page/_static/paging.png)

Aby uzyskać lepsze zrozumienie kodu:

* W *Students/Index.cshtml.cs*, ustaw punkt przerwania na `switch (sortOrder)`.
* Dodaj wyrażenie kontrolne, aby uzyskać `NameSort`, `DateSort`, `CurrentSort`, i `Model.Student.PageIndex`.
* W *Students/Index.cshtml*, ustaw punkt przerwania na `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Przejść przez debuger.

## <a name="update-the-about-page-to-show-student-statistics"></a>Aktualizuj stronę informacje, aby wyświetlić statystyki dla uczniów

W tym kroku *Pages/About.cshtml* zostanie zaktualizowany w celu wyświetlenia liczby studentów zostały zarejestrowane dla każdego dnia rejestracji. Aktualizacja używa grupowania i obejmuje następujące kroki:

* Tworzenie modelu widoku dla danych używanych przez **o** strony.
* Aktualizuj stronę informacje, aby korzystać z modelu widoku.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Tworzenie *SchoolViewModels* folderu w *modeli* folderu.

W *SchoolViewModels* folderu, Dodaj *EnrollmentDateGroup.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Aktualizowanie modelu strony informacje

Aktualizacja *Pages/About.cshtml.cs* pliku następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/About.cshtml.cs)]

Instrukcji LINQ grup jednostek uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w zbiorze `EnrollmentDateGroup` wyświetlić obiekty w modelu.

### <a name="modify-the-about-razor-page"></a>Modyfikowanie dotyczące strony Razor

Zastąp kod w *Pages/About.cshtml* pliku następującym kodem:

[!code-html[](intro/samples/cu21/Pages/About.cshtml)]

Uruchom aplikację i przejdź do strony informacje. Liczba studentów każdej daty rejestracji jest wyświetlany w tabeli.

Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Informacje o stronie](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Debugowanie ASP.NET Core 2.x źródła](https://github.com/aspnet/Docs/issues/4155)

W następnym samouczku aplikacja używa migracji do aktualizacji modelu danych.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/crud)
> [dalej](xref:data/ef-rp/migrations)
