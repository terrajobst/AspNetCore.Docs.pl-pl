---
title: "Stron razor podstawowych EF — sortowanie, filtrowanie, stronicowania - 3 8"
author: rick-anderson
description: "W tym samouczku zostanie dodana sortowanie, filtrowanie i stronicowania funkcji na stronę przy użyciu programu Entity Framework Core i ASP.NET Core."
ms.author: riande
ms.date: 10/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 6fc25df7eab3ab6e44d99ec687bc0ba738d9cfb8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-razor-pages-3-of-8"></a>Sortowanie, filtrowanie, stronicowania i grupowanie — podstawowe EF Razor strony (3 8)

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Jan Kowalski P](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Ten samouczek, sortowanie, filtrowanie, grupowanie i stronicowania funkcje jest dodane.

Na poniższej ilustracji przedstawiono ukończone strony. Nagłówki kolumn są aktywne łącza, aby posortować kolumnę. Kliknięcie nagłówka wielokrotnie kolumny przełącza między kolumny sortowania.

![Strona indeksu uczniów lub studentów](sort-filter-page/_static/paging.png)

Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

## <a name="add-sorting-to-the-index-page"></a>Dodaj sortowanie do strony indeksu

Dodawanie ciągów *Students/Index.cshtml.cs* `PageModel` zawiera sortowania ma parametrów:

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


Aktualizacja *Students/Index.cshtml.cs* `OnGetAsync` następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Poprzedni kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Adres URL (w tym ciągu zapytania) jest generowany przez [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

`sortOrder` Parametr ma wartość "Name" lub "Date". `sortOrder` Parametr opcjonalnie następuje "_desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

Gdy zażąda strony indeksu **studentów** połączyć, jest nie ciągu zapytania. Studenci są wyświetlane w porządku rosnącym według nazwiska. Porządku rosnącym według nazwisko jest ustawieniem domyślnym (fall-through przypadek) w `switch` instrukcji. Gdy użytkownik kliknie łącze nagłówek kolumny, odpowiedni `sortOrder` wartość jest podana w wartości ciągu zapytania.

`NameSort` i `DateSort` są używane przez stronę Razor skonfigurowanie hiperłącza nagłówek kolumny z wartości typu QueryString odpowiednie:

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Poniższy kod zawiera C# [?: operator](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

Pierwszy wiersz określa, że w przypadku `sortOrder` ma wartość null lub jest pusta, `NameSort` ma ustawioną wartość "name_desc." Jeśli `sortOrder` jest **nie** wartości null ani być pusta, `NameSort` jest ustawiony na pusty ciąg.

`?: operator` Jest także znana jako operator trójargumentowy.

Te dwie instrukcje włączyć widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:

| Bieżącej kolejności sortowania | Ostatnia nazwa hiperłącza | Data hiperłącza |
|:--------------------:|:-------------------:|:--------------:|
| Ostatni nazwy, rosnąco | descending        | ascending      |
| Ostatni nazwy, malejąco | ascending           | ascending      |
| Data w kolejności rosnącej       | ascending           | descending     |
| Data, malejąco      | ascending           | ascending      |

Metoda używa do składnika LINQ to Entities Określ kolumny, aby posortować według. Inicjuje kod `IQueryable<Student> ` przed instrukcją switch i modyfikuje je w instrukcji switch:

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 Gdy`IQueryable` tworzenia lub modyfikowania, nie zapytanie jest wysyłane do bazy danych. Zapytanie nie jest wykonywane przed `IQueryable` obiektu jest konwertowany na kolekcję. `IQueryable` są konwertowane na kolekcję przez wywołanie metody, takie jak `ToListAsync`. W związku z tym `IQueryable` kodu wyników w ramach jednego zapytania, który nie jest wykonywany do momentu następująca instrukcja:

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` można pobrać verbose z dużą liczbą kolumn.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodawanie hiperłączy nagłówek kolumny do widoku indeksu dla użytkowników domowych

Zastąp kod w *Students/Index.cshtml*, z następującymi wyróżniony kod:

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Poprzedni kod:

* Dodaje hiperłącza do `LastName` i `EnrollmentDate` nagłówki kolumn.
* Używa tych informacji w `NameSort` i `DateSort` do skonfigurowania hiperłącza bieżącymi wartościami kolejności sortowania.

Aby sprawdzić, który działa sortowania:

* Uruchom aplikację i wybierz **studentów** kartę.
* Kliknij przycisk **nazwisko**.
* Kliknij przycisk **Data rejestracji**.

Aby uzyskać lepsze zrozumienie kodu:

* W *Student/Index.cshtml.cs*, ustaw punkt przerwania na `switch (sortOrder)`.
* Dodaj wyrażenie kontrolne dla `NameSort` i `DateSort`.
* W *Student/Index.cshtml*, ustaw punkt przerwania na `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Kroków opisanych w debugerze.

## <a name="add-a-search-box-to-the-students-index-page"></a>Dodaj pole wyszukiwania do strony indeksu uczniów lub studentów

Aby dodać filtrowanie do strony indeksu studentów:

* Pole tekstowe i przycisk Prześlij jest dodawany do strony Razor. Pole tekstowe dostarcza wyszukiwany ciąg na imię lub nazwisko.
* Aby użyć wartości pole tekstowe zaktualizowania modelu strony.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

Aktualizacja *Students/Index.cshtml.cs* `OnGetAsync` następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Poprzedni kod:

* Dodaje `searchString` parametr `OnGetAsync` metody. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, który jest dodawany w następnej sekcji.
* Dodane do instrukcji LINQ `Where` klauzuli. `Where` Klauzuli wybiera tylko studentów, w których imię lub nazwisko zawiera ciąg wyszukiwania. Instrukcja LINQ jest wykonywana tylko wtedy, gdy wartość do wyszukania.

Uwaga: Poprzedniego wywołania kodu `Where` metoda `IQueryable` obiektu, a filtr jest przetwarzane na serwerze. W niektórych scenariuszach może być wywoływania określona aplikacja `Where` metody jako metodę rozszerzenie w kolekcji w pamięci. Załóżmy na przykład, `_context.Students` zmieni się z podstawowej EF `DbSet` do metody repozytorium, która zwraca `IEnumerable` kolekcji. Wynik zazwyczaj będzie taki sam, ale w niektórych przypadkach może się różnić.

Na przykład, .NET Framework wykonania `Contains` wykonuje domyślnie porównania z uwzględnieniem wielkości liter. W programie SQL Server `Contains` uwzględnianie wielkości liter, jest określany przez ustawienie sortowania wystąpienia programu SQL Server. SQL służą domyślnie do bez uwzględniania wielkości liter. `ToUpper` może być wywołane w celu należy jawnie bez uwzględniania wielkości liter testu:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Poprzedni kod będzie upewnij się, że wyniki są bez uwzględniania wielkości liter w przypadku zmiany kodu do użycia `IEnumerable`. Gdy `Contains` jest wywoływana na `IEnumerable` kolekcji .NET Core implementacji jest używany. Gdy `Contains` jest wywoływana na `IQueryable` obiektu, jest używana implementacja bazy danych. Zwracanie `IEnumerable` z repozytorium może mieć penality znaczne wydajności:

1. Zwracane są wszystkie wiersze z serwera bazy danych.
1. Filtr jest stosowane do wszystkich wierszy zwracanych w aplikacji.

Brak zmniejszenie wydajności dla wywołania `ToUpper`. `ToUpper` Kod dodaje funkcję w klauzuli WHERE instrukcji TSQL SELECT. Funkcja dodana zapobiega Optymalizator przy użyciu indeksu. Biorąc pod uwagę, że program SQL jest zainstalowany jako bez uwzględniania wielkości liter, jest unikanie `ToUpper` wywołania, gdy nie jest wymagana.

### <a name="add-a-search-box-to-the-student-index-view"></a>Dodaj pole wyszukiwania do widoku indeksu dla użytkowników domowych

W *Views/Student/Index.cshtml*, Dodaj następujący wyróżniony kod do utworzenia **wyszukiwania** przycisk i różne chrome.

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

W poprzednim kodzie użyto `<form>` [pomocnika tagów](xref:mvc/views/tag-helpers/intro) można dodać pola tekstowego wyszukiwania i przycisku. Domyślnie `<form>` pomocnika tagów przesyła dane formularza z ogłoszenie (POST). Przy użyciu metody POST parametry są przekazywane w treści wiadomości HTTP, a nie w adresie URL. W przypadku HTTP GET dane formularza jest przekazywany jako ciągi zapytania w adresie URL. Przekazywanie danych z ciągami zapytań umożliwia użytkownikom zakładki adres URL. [Wytyczne W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) zaleca się, że GET powinny być używane, gdy akcja nie powoduje aktualizacji.

Testowanie aplikacji:

* Wybierz **studentów** karcie, a następnie wprowadź ciąg wyszukiwania.
* Wybierz **wyszukiwania**.

Zwróć uwagę, że adres URL zawiera ciąg wyszukiwania.

```html
http://localhost:5000/Students?SearchString=an
```

Jeśli strona jest zakładki, zakładki zawiera adres URL strony i `SearchString` ciągu zapytania. `method="get"` w `form` tag jest przyczyn ciąg zapytania do wygenerowania.

Obecnie, gdy łącze sortowania nagłówka kolumny jest zaznaczona, wartość filtru z **wyszukiwania** pole zostało utracone. Wartość filtru utracone zostanie rozwiązany w następnej sekcji.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Dodawanie funkcji stronicowania do strony indeksu uczniów lub studentów

W tej sekcji `PaginatedList` utworzeniu klasy do obsługi stronicowania. `PaginatedList` Klasy używa `Skip` i `Take` instrukcje, aby filtrować dane na serwerze zamiast pobierania wszystkie wiersze w tabeli. Na poniższej ilustracji przedstawiono przyciski stronicowania.

![Strona indeksu studentów z łączami stronicowania](sort-filter-page/_static/paging.png)

W folderze projektu Utwórz `PaginatedList.cs` następującym kodem:

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

`CreateAsync` w powyższym kodzie metody ma rozmiar strony i numer strony i stosuje odpowiednie `Skip` i `Take` instrukcje `IQueryable`. Gdy `ToListAsync` jest wywoływana na `IQueryable`, zwraca listę zawierającą tylko żądanej strony. Właściwości `HasPreviousPage` i `HasNextPage` są używane, aby włączyć lub wyłączyć **Wstecz** i **dalej** stronicowania przycisków.

`CreateAsync` Metoda służy do tworzenia `PaginatedList<T>`. Nie można utworzyć konstruktora `PaginatedList<T>` obiekt konstruktorów nie można uruchomić kod asynchroniczny.

## <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do Index — metoda

W *Students/Index.cshtml.cs*, zaktualizować typu `Student` z `IList<Student>` do `PaginatedList<Student>`:

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Aktualizacja *Students/Index.cshtml.cs* `OnGetAsync` następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

Poprzedni kod dodaje bieżącego indeksu strony `sortOrder`i `currentFilter` w podpisie metody.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Wszystkie parametry mają wartość null podczas:

* Strona jest wywoływana z **studentów** łącza.
* Użytkownik nie kliknął stronicowania i sortowania łącza.

Po kliknięciu łącza stronicowania, zmienna index strony zawiera numer strony do wyświetlenia.

`CurrentSort` zapewnia stronę Razor bieżącej kolejności sortowania. Bieżący porządek sortowania musi być uwzględniona w łączach stronicowania, aby zachować kolejność sortowania podczas stronicowania.

`CurrentFilter` zapewnia stronę Razor bieżącego ciąg filtru. `CurrentFilter` Wartość:

* Muszą być zawarte w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania.
* Muszą być przywrócone do pola tekstowego, gdy strona zostanie wyświetlony ponownie.

Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strony jest ustawiony na 1. Strona ma zresetowanie do 1 nowy filtr może powodować różnych danych do wyświetlenia. Po wprowadzeniu wartości wyszukiwania i **przesyłania** wybrano:

* Ciąg wyszukiwania zostanie zmieniona.
* `searchString` Parametr nie ma wartości null.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync` Metoda konwertuje zapytania uczniów do pojedynczej strony studentów w typ kolekcji, który obsługuje stronicowanie. Tej stronie studentów są przekazywane do strony Razor.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

Dwa znaki zapytania w `PaginatedList.CreateAsync` reprezentują [łączenie null operator](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator). Operator łączenie null określa wartość domyślną dla typu dopuszczającego wartość null. Wyrażenie `(pageIndex ?? 1)` sposób zwracania wartości `pageIndex` jeśli ma ona wartość. Jeśli `pageIndex` nie ma wartości, zwraca 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Dodawanie łączy stronicowania do uczniów Razor strony

Aktualizowanie kodu znaczników w *Students/Index.cshtml*. Zmiany są wyróżnione:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

Łącza nagłówka kolumny Użyj ciągu zapytania do przekazania aktualnie wyszukiwanego ciągu do `OnGetAsync` metody, dzięki czemu użytkownik może sortować w wynikach filtrowania:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

Przyciski stronicowania są wyświetlane przez pomocników tagów:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

Uruchom aplikację i przejdź do strony studenta.

* Aby upewnić się, że działa stronicowania, po kliknięciu łączy stronicowania w poszczególnych kolejności sortowania.
* Aby sprawdzić, czy stronicowania działa poprawnie z sortowania i filtrowania, wprowadź ciąg wyszukiwania, a następnie spróbuj stronicowania.

![Strona indeksu studentów z łączami stronicowania](sort-filter-page/_static/paging.png)

Aby uzyskać lepsze zrozumienie kodu:

* W *Student/Index.cshtml.cs*, ustaw punkt przerwania na `switch (sortOrder)`.
* Dodaj wyrażenie kontrolne dla `NameSort`, `DateSort`, `CurrentSort`, i `Model.Student.PageIndex`.
* W *Student/Index.cshtml*, ustaw punkt przerwania na `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Kroków opisanych w debugerze.

## <a name="update-the-about-page-to-show-student-statistics"></a>Zaktualizuj strony informacje, aby wyświetlić statystyki dla użytkowników domowych

W tym kroku *Pages/About.cshtml* jest aktualizowana w celu wyświetlenia liczby studentów zostały zarejestrowane dla każdego dnia rejestracji. Aktualizacja używa grupowania i obejmuje następujące kroki:

* Utwórz klasę modelu widoku dla danych używanych przez **o** strony.
* Modyfikowanie modelu o Razor i strony.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Utwórz *SchoolViewModels* folderu w *modele* folderu.

W *SchoolViewModels* folderu, Dodaj *EnrollmentDateGroup.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Aktualizacja modelu strony informacje

Aktualizacja *Pages/About.cshtml.cs* pliku następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/About.cshtml.cs)]

Instrukcja LINQ grupy jednostek uczniowie według daty rejestracji oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w kolekcji z `EnrollmentDateGroup` wyświetlić obiekty modelu.

Uwaga: LINQ `group` polecenia nie jest obecnie obsługiwany przez EF rdzeń. W powyższym kodzie zwracane są wszystkie rekordy uczniów z programu SQL Server. `group` Polecenia jest stosowany w aplikacji stron Razor, nie na serwerze SQL. Podstawowe EF 2.1 będzie obsługiwać ten LINQ `group` operatora i grupowanie występuje na serwerze SQL. Zobacz [relacyjne: obsługuje tłumaczenia GroupBy() SQL](https://github.com/aspnet/EntityFrameworkCore/issues/2341). [Podstawowe EF 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) zostanie wydana przy użyciu zestawu .NET Core 2.1. Aby uzyskać więcej informacji, zobacz [.NET Core plan](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="modify-the-about-razor-page"></a>Modyfikowanie o stronie aparatu Razor

Zastąp kod w *Views/Home/About.cshtml* pliku następującym kodem:

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

Uruchom aplikację i przejdź do strony informacje. Liczba studentów dla każdego dnia rejestracji jest wyświetlane w formie tabeli.

Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Strona — informacje](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Debugowanie ASP.NET Core 2.x źródła](https://github.com/aspnet/Docs/issues/4155)

W następnym samouczku aplikacja używa migracji można zaktualizować modelu danych.

>[!div class="step-by-step"]
[Poprzednie](xref:data/ef-rp/crud)
[dalej](xref:data/ef-rp/migrations)
