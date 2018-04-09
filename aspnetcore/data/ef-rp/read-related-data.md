---
title: Stron razor podstawowych EF w platformy ASP.NET Core - odczytanie danych powiązanych — 6, 8
author: rick-anderson
description: W tym samouczku odczytu i wyświetlanie powiązanych danych — to znaczy dane programu Entity Framework wczytywane właściwości nawigacji.
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 55d9b6743c7d97dc9a354bae218b1fac69d7b6bc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>Stron razor podstawowych EF w platformy ASP.NET Core - odczytanie danych powiązanych — 6, 8

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

W tym samouczku powiązanych danych do odczytu i wyświetlić. Dane dotyczące to dane EF Core ładuje do właściwości nawigacji.

Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

Na poniższych ilustracjach przedstawiono stron ukończonych w tym samouczku:

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Wczesny jawne i opóźnionego ładowania powiązanych danych

Istnieje kilka sposobów EF Core może ładować powiązanych danych do właściwości nawigacji jednostki:

* [Ładowanie wczesny](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Ładowanie wczesny jest podczas zapytania dla jednego typu jednostki ładowania również powiązanych jednostek. Podczas odczytywania jednostki powiązane dane są pobierane. Powoduje to zwykle w zapytaniu sprzężenia jednej, która pobiera wszystkie dane potrzebne. Podstawowe EF będzie wystawiać wielu zapytań w przypadku niektórych typów wczesny ładowania. Wystawianie wielu zapytań może być bardziej efektywne niż w przypadku niektórych kwerend w EF6 było pojedynczego zapytania. Ładowanie wczesny zostanie określony z `Include` i `ThenInclude` metody.

  ![Przykład wczesny ładowania](read-related-data/_static/eager-loading.png)
 
  Ładowanie wczesny wysyła wielu zapytań podczas nawigacji kolekcji znajduje się:

  * Jedno zapytanie dla głównego zapytania 
  * Jednej kwerendzie dla każdej kolekcji "krawędzi" w drzewie obciążenia.

* Oddzielne zapytania z `Load`: w oddzielne zapytania można pobrać dane i EF Core "rozwiązuje" właściwości nawigacji. oznacza "poprawki w górę" EF Core będzie automatycznie wypełni właściwości nawigacji. Oddzielne zapytania z `Load` przypomina explict ładowania niż wczesny ładowania.

  ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

  Uwaga: EF Core automatycznie naprawia właściwości nawigacji z innymi obiektami, które wcześniej zostały załadowane do wystąpienia kontekstu. Nawet jeśli dane dla właściwości nawigacji *nie* jawnie uwzględnione właściwość nadal może być wypełniana w przypadku niektórych lub wszystkich powiązanych jednostek wcześniej załadowane.

* [Jawne ładowania](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane. Kod musi być przystosowana do pobierania powiązanych danych, gdy jest to potrzebne. Jawne ładowanie z oddzielne zapytania powoduje wielu zapytań wysłanych do bazy danych. Z jawnego ładowania kodu określa właściwości nawigacji do załadowania. Użyj `Load` metody w celu jawnego ładowania. Na przykład:

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* [Powolne ładowanie](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [EF Core aktualnie nie obsługuje ładowania opóźnionego](https://github.com/aspnet/EntityFrameworkCore/issues/3797). Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane. Właściwość nawigacji jest dostępny, po raz pierwszy jest automatycznie pobierany wymagane dane dla tej właściwości nawigacji. Zapytanie jest wysyłane do bazy danych zawsze właściwość nawigacji jest dostępny po raz pierwszy.

* `Select` Operator ładuje tylko powiązane dane potrzebne.

## <a name="create-a-courses-page-that-displays-department-name"></a>Utwórz stronę kursy Wyświetla nazwę działu

Jednostka kursu zawiera właściwość nawigacji, który zawiera `Department` jednostki. `Department` Jednostka zawiera kursu przypisany do działu.

Aby wyświetlić listę kursów nazwa przypisanej działu:

* Pobierz `Name` właściwość z `Department` jednostki.
* `Department` Jednostki pochodzi z `Course.Department` właściwości nawigacji.

![ourse. Dział](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Tworzenie szkieletu modelu kursu

* Zamknij program Visual Studio.
* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

Poprzedni rusztowania polecenia `Course` modelu. Otwórz projekt w programie Visual Studio.

Skompiluj projekt. Kompilacja generuje błędy podobne do następujących:

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Globalnie zmienić `_context.Course` do `_context.Courses` (to znaczy dodania "s" do `Course`). 7 wystąpienia są odnaleźć i zaktualizować.

Otwórz *Pages/Courses/Index.cshtml.cs* i sprawdź, czy `OnGetAsync` metody. Aparat szkieletów określony wczesny ładowania dla `Department` właściwości nawigacji. `Include` Metody określa wczesny ładowania.

Uruchom aplikację i wybierz **kursów** łącza. Przedstawia kolumnę Dział `DepartmentID`, który nie jest użyteczne.

Aktualizacja `OnGetAsync` metodę z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Poprzedni kod dodaje `AsNoTracking`. `AsNoTracking` zwiększa wydajność, ponieważ zwróconych nie są śledzone. Jednostek nie są śledzone, ponieważ nie są one aktualizowane w bieżącym kontekście.

Aktualizacja *Views/Courses/Index.cshtml* z następujący wyróżniony kod znaczników:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Kod z utworzonym szkieletem wprowadzono następujące zmiany:

* Zmieniony nagłówek od indeksu do kursów.
* Dodaje **numer** kolumny, która zawiera `CourseID` wartości właściwości. Domyślnie nie są szkieletu kluczy podstawowych, ponieważ zwykle są bezużyteczne dla użytkowników końcowych. Jednak w takim przypadku klucz podstawowy jest łatwy do rozpoznania.
* Zmienione **działu** kolumny, aby wyświetlić nazwę działu. Wyświetla kod `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę z nazwami działu.

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Trwa ładowanie powiązanych danych za pomocą wybierz

`OnGetAsync` Metody ładuje dane powiązane z `Include` metody:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` Operator ładuje tylko powiązane dane potrzebne. Dla pojedynczego elementów takich jak `Department.Name` używa SQL INNER JOIN. Dla kolekcji, używa innego dostęp do bazy danych, lecz to samo `Include` operatora w kolekcjach.

Poniższy kod ładuje dane powiązane z `Select` metody:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Zobacz [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) pełny przykład.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Utwórz stronę instruktorów kursów i rejestracji

W tej sekcji strony instruktorów jest tworzony.

<a name="IP"></a>
![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:

* Lista instruktorów powiązane dane z `OfficeAssignment` jednostki (Office powyższej ilustracji). `Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden. Ładowanie wczesny służy do `OfficeAssignment` jednostek. Ładowanie wczesny jest zazwyczaj bardziej wydajne, gdy powiązane dane powinny być wyświetlane. W takim przypadku office przydziałów Instruktorzy są wyświetlane.
* Gdy użytkownik wybierze instruktora (Harui powyższej ilustracji) powiązane `Course` wyświetlania obiektów. `Instructor` i `Course` jednostki są w relacji wiele do wielu. Ładowanie wczesny służy do `Course` jednostek i ich pokrewnych `Department` jednostek. W takim przypadku oddzielne zapytania może być bardziej wydajne, ponieważ wymagane są tylko szkoleń dla wybranego instruktora. Ten przykład przedstawia sposób użycia wczesny ładowania dla właściwości nawigacji w obiektach, które znajdują się w właściwości nawigacji.
* Gdy użytkownik wybierze kursu (chemia powyższej ilustracji), dane z dotyczące `Enrollments` wyświetlania obiektu. Na poprzedniej ilustracji są wyświetlane nazwy dla użytkowników domowych i klasy. `Course` i `Enrollment` jednostki są w relacji jeden do wielu.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Utwórz model widoku dla widoku indeksu instruktora

Na stronie instruktorów znajdują się dane z trzech różnych tabel. Model widoku o utworzeniu obejmuje trzy jednostki reprezentujący trzy tabele.

W *SchoolViewModels* folderu, Utwórz *InstructorIndexData.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Tworzenie szkieletu modelu instruktora

* Zamknij program Visual Studio.
* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

Poprzedni rusztowania polecenia `Instructor` modelu. Otwórz projekt w programie Visual Studio.

Skompiluj projekt. Kompilacja generuje błędy.

Globalnie zmienić `_context.Instructor` do `_context.Instructors` (to znaczy dodania "s" do `Instructor`). 7 wystąpienia są odnaleźć i zaktualizować.

Uruchom aplikację i przejdź do strony instruktorów.

Zastąp *Pages/Instructors/Index.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

`OnGetAsync` — Metoda akceptuje dane trasy opcjonalny identyfikator wybranego instruktora.

Sprawdź zapytania na *Pages/Instructors/Index.cshtml* strony:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Zapytanie zawiera dwa obejmuje:

* `OfficeAssignment`: Wyświetlanych w [widoku instruktorów](#IP).
* `CourseAssignments`: Które wprowadzono w kursy nauczanych.


### <a name="update-the-instructors-index-page"></a>Aktualizacja instruktorów strony indeksu

Aktualizacja *Pages/Instructors/Index.cshtml* z następujący kod:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`. `"{id:int?}"` to jest szablon trasy. Szablon trasy zmiany liczby całkowitej ciągów zapytania w adresie URL w danych trasy. Na przykład kliknięcie **wybierz** łącze instruktora tylko z `@page` dyrektywy tworzy adres URL podobnie do następującej:

    `http://localhost:1234/Instructors?id=2`

    Po dyrektywie page `@page "{id:int?}"`, jest poprzedniego adresu URL:

    `http://localhost:1234/Instructors/2`

* Tytuł strony jest **instruktorów**.
* Dodaje **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. Ponieważ jest to relacji jeden do zero lub jeden, może nie być OfficeAssignment powiązanej jednostki.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Dodaje **kursów** kolumnę wyświetlającą kursy nauczanych przy każdym instruktora. Zobacz [jawne przejścia wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji o tej składni razor.

* Dodano kod, który dynamicznie dodaje `class="success"` do `tr` elementu wybranego instruktora. To ustawia kolor tła dla wybranego wiersza za pomocą klasy ładowania początkowego.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Dodano nowe hiperłącze etykietą **wybierz**. To łącze wysyła identyfikator wybranego instruktora `Index` — metoda i ustawia kolor tła.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uruchom aplikację i wybierz **instruktorów** kartę. Na stronie są wyświetlane `Location` (office) z odnośnych `OfficeAssignment` jednostki. Jeśli OfficeAssignment "jest wartość null, jest wyświetlana pusta tabela komórki.

![Strona indeksu instruktorów nic nie wybrane](read-related-data/_static/instructors-index-no-selection.png)

Polecenie **wybierz** łącza. Zmiany stylu wiersza.

### <a name="add-courses-taught-by-selected-instructor"></a>Dodaj kursy nauczanych przy wybranym instruktorze

Aktualizacja `OnGetAsync` metody w *Pages/Instructors/Index.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Sprawdź zaktualizowane zapytania:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Dodaje poprzedniego zapytania `Department` jednostek.

Poniższy kod wykonywany po wybraniu instruktora (`id != null`). Wybranym instruktorze są pobierane z listy instruktorów w modelu widoku. Model widoku `Courses` właściwości jest ładowany z `Course` jednostek z tego instruktora `CourseAssignments` właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` Metoda zwraca kolekcję. W poprzednim `Where` metody, tylko jeden `Instructor` jest zwracana jednostka. `Single` Metoda konwertuje kolekcję do postaci jednej `Instructor` jednostki. `Instructor` Jednostki zapewnia dostęp do `CourseAssignments` właściwości. `CourseAssignments` zapewnia dostęp do pokrewnych `Course` jednostek.

![M:M instruktora do szkolenia](complex-data-model/_static/courseassignment.png)

`Single` Metoda jest używana w kolekcji, jeśli kolekcja zawiera tylko jeden element. `Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku), jeśli kolekcja jest pusta. Przy użyciu `SingleOrDefault` w pustej kolekcji:

* Powoduje wygenerowanie wyjątku (z próby znalezienia `Courses` właściwości na odwołanie o wartości null).
* Komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu.

Poniższy kod umożliwia wypełnienie modelu widoku `Enrollments` właściwości po wybraniu kursu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Dodaj następujący kod na końcu *Pages/Courses/Index.cshtml* Razor strony:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Poprzedni kod znaczników zostanie wyświetlona lista kursów związane z instruktora po wybraniu instruktora.

Testowanie aplikacji. Polecenie **wybierz** łącze na stronie instruktorów.

![Instruktora strony indeksu instruktorów wybrane](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Pokaż dane dla użytkowników domowych

W tej części aplikacji jest aktualizowana w celu wyświetlenia danych uczniów dla wybranego kursu.

Zaktualizuj zapytanie w `OnGetAsync` metody w *Pages/Instructors/Index.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Aktualizacja *Pages/Instructors/Index.cshtml*. Dodaj następujący kod na końcu pliku:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Poprzedni kod znaczników Wyświetla listę studentów, którzy są rejestrowane w toku wybrane.

Odśwież stronę i wybierz instruktora. Wybierz plan, aby wyświetlić listę zarejestrowanych studentów i ich klas.

![Instruktora strony indeksu instruktorów i kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Za pomocą pojedynczego

`Single` Metody można przekazać `Where` warunku zamiast wywoływać metodę `Where` metody oddzielnie:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

Poprzedni `Single` podejście zapewnia nie korzyści w przypadku `Where`. Niektórzy deweloperzy preferowane `Single` podejścia stylu.

## <a name="explicit-loading"></a>Ładowanie jawne

Bieżący kod określa wczesny ładowania dla `Enrollments` i `Students`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Załóżmy, że użytkownik chce rzadko Zobacz rejestracji w toku. W takim przypadku optymalizacji byłoby tylko, jeśli wymagane jest, ładowanie danych rejestracji. W tej sekcji `OnGetAsync` jest aktualizowana w celu użyj jawnego ładowania `Enrollments` i `Students`.

Aktualizacja `OnGetAsync` następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Poprzedni kod porzuca *ThenInclude* metoda wymaga rejestracji i uczniów danych. Jeśli wybrano kursu, pobiera wyróżniony kod:

* `Enrollment` Jednostki dla porach wybrane.
* `Student` Jednostek dla każdego `Enrollment`.

Zwróć uwagę, poprzedni kod komentarze `.AsNoTracking()`. Właściwości nawigacji można jawnie ładować tylko dla śledzonych jednostek.

Testowanie aplikacji. Z punktu widzenia użytkowników aplikacji zachowuje się tak samo poprzedniej wersji.

Następny samouczek przedstawia sposób aktualizacji powiązanych danych.

>[!div class="step-by-step"]
>[Poprzednie](xref:data/ef-rp/complex-data-model)
>[dalej](xref:data/ef-rp/update-related-data)
