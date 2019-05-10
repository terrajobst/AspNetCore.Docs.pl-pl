---
title: Strony razor z programem EF Core w programie ASP.NET Core — odczytanie powiązanych danych — 6 8
author: rick-anderson
description: W tym samouczku należy przeczytać i wyświetlanie powiązanych danych — oznacza to, że dane programu Entity Framework wczytywane właściwości nawigacji.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: e98f6dddc727bb78a411fbd0a5014bcee87c7aeb
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901066"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>Strony razor z programem EF Core w programie ASP.NET Core — odczytanie powiązanych danych — 6 8

Przez [Tom Dykstra](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

W tym samouczku powiązanych danych odczytu i wyświetlić. Powiązane dane to dane programu EF Core ładuje się do właściwości nawigacji.

Jeśli napotkasz problemy, nie można rozwiązać, [pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

Na poniższych ilustracjach przedstawiono stron ukończonych w ramach tego samouczka:

![Kursy strony indeksu](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Zapoznamy wyraźne i powolne ładowanie powiązanych danych

Istnieje kilka sposobów, że programu EF Core można załadować powiązane dane do właściwości nawigacji jednostki:

* [Wczesne ładowanie](/ef/core/querying/related-data#eager-loading). Wczesne ładowanie jest, gdy zapytanie dla jednego typu obiektu również ładuje powiązanych jednostek. Podczas odczytywania jednostki powiązane dane są pobierane. Powoduje to zwykle w zapytaniu sprzężenia jednego, który pobiera wszystkie dane potrzebne. EF Core będzie wystawiać wielu zapytań dla niektórych rodzajów wczesne ładowanie. Wystawianie wielu zapytań może być bardziej efektywne niż w przypadku niektórych zapytań w EF6 było pojedynczego zapytania. Wczesne ładowanie jest określony za pomocą `Include` i `ThenInclude` metody.

  ![Przykład wczesne ładowanie](read-related-data/_static/eager-loading.png)
 
  Wczesne ładowanie wysyła wielu zapytań, gdy wchodzi nawigacji kolekcji:

  * Jednej kwerendzie dla głównego zapytania 
  * Jednej kwerendzie dla każdej kolekcji "brzegu", w drzewie obciążenia.

* Oddziel zapytań przy użyciu `Load`: Dane można pobrać w oddzielne zapytania, a programem EF Core termin "poprawki" właściwości nawigacji. oznacza "poprawki w górę" EF Core będzie automatycznie wypełnia właściwości nawigacji. Oddziel zapytań przy użyciu `Load` więcej takich jak ładowanie niż wczesne ładowanie stosowany jawny.

  ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

  Uwaga: EF Core automatycznie rozwiązuje się właściwości nawigacji do innych jednostek, które wcześniej zostały załadowane do wystąpienia kontekstu. Nawet jeśli dane dla właściwości nawigacyjnej *nie* jawnie uwzględnione, nadal można wypełnić właściwość, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.

* [Jawne ładowanie](/ef/core/querying/related-data#explicit-loading). Podczas odczytywania jednostki powiązane dane nie są pobierane. Musi być napisany kod można pobrać powiązanych danych, gdy jest to konieczne. Jawne ładowanie w oddzielnych zapytań powoduje wielu zapytań wysyłanych do bazy danych. Przy użyciu jawne ładowanie kod określa właściwości nawigacji, aby go załadować. Użyj `Load` celu jawne ładowanie metodę. Na przykład:

  ![Przykład jawne ładowanie](read-related-data/_static/explicit-loading.png)

* [Powolne ładowanie](/ef/core/querying/related-data#lazy-loading). [Powolne ładowanie został dodany do programu EF Core w wersji 2.1](/ef/core/querying/related-data#lazy-loading). Podczas odczytywania jednostki powiązane dane nie są pobierane. Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji jest automatycznie pobierany wymagane dane dla tej właściwości nawigacji. Zapytanie jest wysyłane do bazy danych, w każdym właściwość nawigacji jest dostępny po raz pierwszy.

* `Select` Operator ładuje tylko powiązane dane potrzebne.

## <a name="create-a-course-page-that-displays-department-name"></a>Tworzenie strony kursu, który wyświetla nazwy działu

Jednostki kurs zawiera właściwość nawigacji, która zawiera `Department` jednostki. `Department` Działu, przypisana do kursu w jednostce.

Aby wyświetlić listę kursów nazwa działu przypisane:

* Pobierz `Name` właściwość `Department` jednostki.
* `Department` Jednostki pochodzi z `Course.Department` właściwości nawigacji.

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a>Tworzenie szkieletu modelu kursu

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Course` dla klasy modelu.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

Poprzedni szkielety mechanizmów polecenia `Course` modelu. Otwórz projekt w programie Visual Studio.

Otwórz *Pages/Courses/Index.cshtml.cs* i zbadaj `OnGetAsync` metody. Aparat tworzenia szkieletów określony wczesne ładowanie dla `Department` właściwości nawigacji. `Include` Metody określa wczesne ładowanie.

Uruchom aplikację i wybierz **kursów** łącza. Wyświetla kolumnę Dział `DepartmentID`, który nie jest przydatne.

Aktualizacja `OnGetAsync` metoda następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Poprzedzający kod dodaje `AsNoTracking`. `AsNoTracking` zwiększa wydajność, ponieważ nie są śledzone obiektów zwracanych. Jednostki nie są śledzone, ponieważ nie są one aktualizowane w bieżącym kontekście.

Aktualizacja *Pages/Courses/Index.cshtml* z następujący wyróżniony kod znaczników:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Utworzony szkielet kodu wprowadzono następujące zmiany:

* Zmienić nagłówek z indeksu do kursów.
* Dodano **numer** kolumna, która pokazuje `CourseID` wartości właściwości. Domyślnie klucze podstawowe nie są szkieletu, ponieważ zwykle są bez znaczenia dla użytkowników końcowych. Jednak w takim przypadku klucz podstawowy jest znaczący.
* Zmienione **działu** kolumny, aby wyświetlić nazwę działu. Ten kod wyświetla `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę nazw działów.

![Kursy strony indeksu](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a>Trwa ładowanie powiązanych danych z wybranymi

`OnGetAsync` Metoda ładuje dane powiązane z `Include` metody:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` Operator ładuje tylko powiązane dane potrzebne. Dla pojedynczego elementów takich jak `Department.Name` używa SQL INNER JOIN. Dla kolekcji, jego używa innego dostępu do bazy danych, ale to samo dotyczy `Include` operatora w kolekcjach.

Poniższy kod ładuje dane powiązane z `Select` metody:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Zobacz [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) pełny przykład.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Utwórz stronę instruktorów, pokazujący kursów i rejestracji

W tej sekcji Strona Instruktorzy jest tworzona.

<a name="IP"></a>
![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:

* Lista Instruktorzy dane związane z `OfficeAssignment` jednostki (Office na poprzedniej ilustracji). `Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden. Wczesne ładowanie służy do `OfficeAssignment` jednostek. Wczesne ładowanie jest zazwyczaj bardziej efektywne, gdy powiązane dane mają być wyświetlone. W tym przypadku Instruktorzy przypisania pakietu office są wyświetlane.
* Gdy użytkownik wybierze instruktora (Harui na poprzedniej ilustracji) związane z `Course` jednostki są wyświetlane. `Instructor` i `Course` jednostki są w relacji wiele do wielu. Wczesne ładowanie służy do `Course` jednostek i ich powiązane `Department` jednostek. W tym przypadku oddzielne zapytania może być bardziej efektywne, ponieważ potrzebne są tylko kursy dla wybranej przez instruktorów. W tym przykładzie pokazano, jak używać wczesne ładowanie dla właściwości nawigacji w jednostkach, które znajdują się w właściwości nawigacji.
* Gdy użytkownik wybierze kurs (chemia na poprzedniej ilustracji), związane z danymi z `Enrollments` jednostka jest wyświetlana. Na wcześniejszej ilustracji są wyświetlane nazwy dla uczniów i klasy korporacyjnej. `Course` i `Enrollment` jednostki są w relacji jeden do wielu.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Tworzenie modelu widoku dla widoku indeksu przez instruktorów

Na stronie Instruktorzy wyświetlane dane z trzech różnych tabelach. Tworzony jest model widoku, który zawiera trzy obiekty reprezentujące trzy tabele.

W *SchoolViewModels* folderze utwórz *InstructorIndexData.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Tworzenie szkieletu modelu przez instruktorów

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Instructor` dla klasy modelu.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

Poprzedni szkielety mechanizmów polecenia `Instructor` modelu. Uruchom aplikację i przejdź do strony instruktorów.

Zastąp *Pages/Instructors/Index.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

`OnGetAsync` Metoda przyjmuje dane opcjonalne trasy dla Identyfikatora wybranego instruktora.

Sprawdź zapytania w *Pages/Instructors/Index.cshtml.cs* pliku:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Zapytanie ma dwa obejmuje:

* `OfficeAssignment`: Wyświetlane w [widoku Instruktorzy](#IP).
* `CourseAssignments`: Który udostępnia w kursom prowadzonym.

### <a name="update-the-instructors-index-page"></a>Aktualizuj stronę indeksu instruktorów

Aktualizacja *Pages/Instructors/Index.cshtml* następującym kodem:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`. `"{id:int?}"` jest szablon trasy. Szablon trasy zmiany liczby całkowitej ciągów zapytania w adresie URL danych trasy. Na przykład, klikając **wybierz** link pod kierunkiem instruktora tylko z `@page` dyrektywa generuje adres URL podobnie do poniższego:

  `http://localhost:1234/Instructors?id=2`

  Po dyrektywie page `@page "{id:int?}"`, jest poprzedniego adresu URL:

  `http://localhost:1234/Instructors/2`

* Tytuł strony jest **Instruktorzy**.
* Dodano **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. Ponieważ jest to relacja jeden do zero lub jeden, może nie być powiązana jednostka OfficeAssignment.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Dodano **kursów** kolumnę wyświetlającą kursy prowadzone przez instruktora każdego. Zobacz [jawne przejście wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji na temat tej składni razor.

* Dodano kod, w której są dodawane dynamicznie `class="success"` do `tr` elementu wybranego przez instruktorów. To ustawienie jako kolor tła wybranego wiersza przy użyciu ładowania klasy.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Dodano nowe hiperłącze etykietą **wybierz**. To łącze wysyła identyfikator wybranego przez instruktorów `Index` metody i ustawia kolor tła.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uruchom aplikację i wybierz **Instruktorzy** kartę. Zostanie wyświetlona strona `Location` (office) z powiązane `OfficeAssignment` jednostki. Jeśli OfficeAssignment "jest komórka pustej tabeli ma wartość null, jest wyświetlana.

![Strona indeksu instruktorów, którą nic nie zaznaczone](read-related-data/_static/instructors-index-no-selection.png)

Kliknij pozycję **wybierz** łącza. Zmiany stylów wierszy.

### <a name="add-courses-taught-by-selected-instructor"></a>Dodaj kursom prowadzonym przez instruktora wybrane

Aktualizacja `OnGetAsync` method in Class metoda *Pages/Instructors/Index.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Add `public int CourseID { get; set; }`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

Sprawdź zaktualizowane zapytanie:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Poprzednie zapytanie dodaje `Department` jednostek.

Poniższy kod jest wykonywany po wybraniu pod kierunkiem instruktora (`id != null`). Wybrane przez instruktorów są pobierane z listy w modelu widoku instruktorów. Model widoku `Courses` właściwość jest ładowany z `Course` jednostek z tego instruktora `CourseAssignments` właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` Metoda zwraca kolekcję. W poprzednim `Where` metody, tylko jeden `Instructor` jednostki jest zwracana. `Single` Metoda konwertuje kolekcję z jedną `Instructor` jednostki. `Instructor` Jednostka zapewnia dostęp do `CourseAssignments` właściwości. `CourseAssignments` zapewnia dostęp do odnośnych `Course` jednostek.

![M:M przez instruktorów do kursów](complex-data-model/_static/courseassignment.png)

`Single` Metoda jest używana w kolekcji, jeśli kolekcja zawiera tylko jeden element. `Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub ma więcej niż jeden element. Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku) Jeśli kolekcja jest pusta. Za pomocą `SingleOrDefault` w pustej kolekcji:

* Powoduje wygenerowanie wyjątku (z próby znalezienia `Courses` właściwość odwołanie o wartości null).
* Komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu.

Poniższy kod powoduje wypełnienie modelu widoku `Enrollments` właściwości po wybraniu kursu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Dodaj następujący kod na końcu *Pages/Instructors/Index.cshtml* strona Razor:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Poprzedni kod znaczników Wyświetla listę kursów związane z kierunkiem instruktora, po wybraniu pod kierunkiem instruktora.

Testowanie aplikacji. Kliknij pozycję **wybierz** łącze na stronie instruktorów.

![Wybrane przez instruktorów indeks strony instruktorów](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Pokaż dane dla uczniów

W tej sekcji gdy aplikacja jest zaktualizowana do wyświetlenia danych edukacyjnych do wybranych kursów.

Zaktualizuj zapytanie w `OnGetAsync` method in Class metoda *Pages/Instructors/Index.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Aktualizacja *Pages/Instructors/Index.cshtml*. Dodaj następujący kod na końcu pliku:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Poprzedni kod znaczników Wyświetla listę uczniów, którzy są rejestrowane w wybranych kursów.

Odśwież stronę, a następnie wybierz pozycję pod kierunkiem instruktora. Wybierz kurs, aby wyświetlić listę zarejestrowanych studentów i ich klas.

![Instruktorzy indeks strony instruktora oraz przypisane kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Za pomocą pojedynczego

`Single` Przekazać metoda `Where` warunku, zamiast wywoływać metodę `Where` metoda oddzielnie:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

Poprzedni `Single` podejście zapewnia nie korzyści za pośrednictwem za pomocą `Where`. Niektórzy deweloperzy Preferuj `Single` podejście stylu.

## <a name="explicit-loading"></a>jawne ładowanie

Bieżący kod określa wczesne ładowanie dla `Enrollments` i `Students`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Załóżmy, że użytkownicy rzadko mają być wyświetlane rejestracji w szkoleniu. W takim przypadku danych rejestracji należy załadowywać tylko, jeśli żądanie jest optymalizacji. W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawne ładowanie `Enrollments` i `Students`.

Aktualizacja `OnGetAsync` następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Powyższy kod spada *ThenInclude* metoda wywołuje danych rejestracji i uczniów. Jeśli kurs jest zaznaczone, umożliwia pobranie wyróżniony kod:

* `Enrollment` Jednostki dla wybranych kursów.
* `Student` Jednostki dla każdej `Enrollment`.

Zwróć uwagę, poprzedni kod komentarze `.AsNoTracking()`. Właściwości nawigacji można jawnie ładować tylko dla obiektów śledzonych.

Testowanie aplikacji. Z punktu widzenia użytkowników aplikacji działa identycznie do poprzedniej wersji.

Następny samouczek pokazuje, jak aktualizowanie powiązanych danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja usługi YouTube w tym samouczku (część 1)](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [Wersja usługi YouTube w tym samouczku (część 2)](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
>[Poprzednie](xref:data/ef-rp/complex-data-model)
>[dalej](xref:data/ef-rp/update-related-data)
