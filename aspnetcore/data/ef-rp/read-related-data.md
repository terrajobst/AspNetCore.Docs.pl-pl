---
title: Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8
author: rick-anderson
description: W tym samouczku odczytasz i wyświetlasz powiązane dane, czyli dane, które Entity Framework ładowane do właściwości nawigacji.
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: d244ce1527486466bcbc6557ec35869aa206bc4f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656577"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8

Autorzy [Dykstra](https://github.com/tdykstra), [Jan P Kowalski](https://twitter.com/thereformedprog)i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

W tym samouczku pokazano, jak odczytywać i wyświetlać powiązane dane. Powiązane dane to dane, które EF Core ładowane do właściwości nawigacji.

Na poniższych ilustracjach przedstawiono ukończone strony dla tego samouczka:

![Strona indeksu kursów](read-related-data/_static/courses-index30.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a>Eager, jawne i ładowanie z opóźnieniem

Istnieje kilka sposobów, EF Core mogą ładować powiązane dane do właściwości nawigacji jednostki:

* [Ładowanie eager](/ef/core/querying/related-data#eager-loading). Ładowanie eager jest, gdy zapytanie dla jednego typu jednostki również ładuje powiązane jednostki. Po odczytaniu jednostki pobierane są powiązane z nią dane. Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne. EF Core będzie wystawiał wiele zapytań dla niektórych typów ładowania eager. Wygenerowanie wielu zapytań może być bardziej wydajne niż bardzo duże pojedyncze zapytanie. Ładowanie eager jest określone przy użyciu metod `Include` i `ThenInclude`.

  ![Przykład ładowania eager](read-related-data/_static/eager-loading.png)
 
  Podczas ładowania eager są wysyłane wiele zapytań, gdy zostanie dołączona Nawigacja kolekcji:

  * Jedno zapytanie dotyczące głównej kwerendy 
  * Jedno zapytanie dla każdej kolekcji "Edge" w drzewie ładowania.

* Oddziel zapytania z `Load`: dane można pobrać w oddzielnych zapytaniach, a EF Core "naprawia" właściwości nawigacji. "Rozwiązuje" oznacza, że EF Core automatycznie wypełnia właściwości nawigacji. Oddzielne zapytania o `Load` są bardziej podobne do jawnego ładowania niż ładowanie eager.

  ![Przykład oddzielnych zapytań](read-related-data/_static/separate-queries.png)

  Uwaga: EF Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu. Nawet jeśli dane dla właściwości nawigacji *nie* są jawnie uwzględniane, właściwość można nadal wypełnić, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.

* [Jawne ładowanie](/ef/core/querying/related-data#explicit-loading). Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Kod musi być zapisany, aby można było pobrać powiązane dane, gdy jest to konieczne. Jawne ładowanie z oddzielnymi zapytania powoduje wysłanie wielu zapytań do bazy danych. W przypadku jawnego ładowania kod określa właściwości nawigacji do załadowania. Użyj metody `Load`, aby przeprowadzić jawne ładowanie. Na przykład:

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* [Ładowanie z opóźnieniem](/ef/core/querying/related-data#lazy-loading). [Ładowanie z opóźnieniem zostało dodane do EF Core w wersji 2,1](/ef/core/querying/related-data#lazy-loading). Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie. Zapytanie jest wysyłane do bazy danych przy każdym dostępie do właściwości nawigacji po raz pierwszy.

## <a name="create-course-pages"></a>Tworzenie stron kursu

Jednostka `Course` zawiera właściwość nawigacji, która zawiera powiązaną `Department` jednostkę.

![Kurs. Dział](read-related-data/_static/dep-crs.png)

Aby wyświetlić nazwę przypisanego działu dla kursu:

* Załaduj powiązaną jednostkę `Department` do właściwości nawigacji `Course.Department`.
* Pobierz nazwę z właściwości `Name` jednostki `Department`.

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a>Strony kursu szkieletowego

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Postępuj zgodnie z instrukcjami na [stronach uczniów tworzenia szkieletów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:

  * Utwórz folder *strony/kursy* .
  * Użyj `Course` dla klasy modelu.
  * Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Utwórz folder *strony/kursy* .

* Uruchom następujące polecenie, aby połączyć strony kursu.

  **W systemie Windows:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  **W systemie Linux lub macOS:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* Otwórz *stronę/kursy/index. cshtml. cs* i Przeanalizuj metodę `OnGetAsync`. Aparat szkieletu określony eager ładowania dla właściwości nawigacji `Department`. Metoda `Include` określa ładowanie eager.

* Uruchom aplikację i wybierz łącze **kursy** . W kolumnie dział zostanie wyświetlona `DepartmentID`, co nie jest przydatne.

### <a name="display-the-department-name"></a>Wyświetl nazwę działu

Zaktualizuj strony/kursy/index. cshtml. cs przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

Poprzedni kod zmienia właściwość `Course` na `Courses` i dodaje `AsNoTracking`. `AsNoTracking` zwiększa wydajność, ponieważ zwrócone jednostki nie są śledzone. Nie trzeba śledzić jednostek, ponieważ nie są one aktualizowane w bieżącym kontekście.

Zaktualizuj *strony/kursy/index. cshtml* przy użyciu następującego kodu.

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

W kodzie szkieletowym wprowadzono następujące zmiany:

* Zmieniono nazwę właściwości `Course` na `Courses`.
* Dodano kolumnę **liczbową** , która wyświetla wartość właściwości `CourseID`. Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są oznaczane przez użytkowników końcowych. Jednak w tym przypadku klucz podstawowy ma znaczenie.
* Zmieniono kolumnę **działu** , aby wyświetlić nazwę działu. Kod wyświetla Właściwość `Name` jednostki `Department`, która jest ładowana do właściwości nawigacji `Department`:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uruchom aplikację i wybierz kartę **kursy** , aby wyświetlić listę z nazwami działów.

![Strona indeksu kursów](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a>Ładowanie powiązanych danych przy użyciu opcji Select

Metoda `OnGetAsync` ładuje powiązane dane przy użyciu metody `Include`. Metoda `Select` jest alternatywą, która ładuje tylko powiązane dane. Dla pojedynczych elementów, takich jak `Department.Name` używa SPRZĘŻENIa wewnętrznego SQL. W przypadku kolekcji jest on wykorzystywany przez inny dostęp do bazy danych, ale w związku z tym wykonuje operator `Include` w kolekcjach.

Poniższy kod ładuje powiązane dane przy użyciu metody `Select`:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Zobacz [IndexSelect. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) , aby zapoznać się z kompletnym przykładem.

## <a name="create-instructor-pages"></a>Utwórz strony instruktora

Ta sekcja szkieletuje strony instruktorów i dodaje powiązane kursy i rejestracje do strony indeksu instruktorów.

<a name="IP"></a>
![strony indeksu instruktorów](read-related-data/_static/instructors-index30.png)

Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:

* Lista instruktorów wyświetla powiązane dane z jednostki `OfficeAssignment` (Office na powyższym obrazie). Jednostki `Instructor` i `OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego. Ładowanie eager jest używane dla jednostek `OfficeAssignment`. Ładowanie eager jest zwykle wydajniejsze, gdy wymagane jest wyświetlenie powiązanych danych. W takim przypadku wyświetlane są przypisania pakietu Office dla instruktorów.
* Gdy użytkownik wybierze instruktora, zostaną wyświetlone powiązane jednostki `Course`. Jednostki `Instructor` i `Course` znajdują się w relacji wiele-do-wielu. Ładowanie eager jest używane dla jednostek `Course` i powiązanych `Department` jednostek. W takim przypadku oddzielne zapytania mogą być bardziej wydajne, ponieważ potrzebują tylko kursów dla wybranego instruktora. Ten przykład pokazuje, jak używać eager ładowania dla właściwości nawigacji w jednostkach, które są we właściwościach nawigacji.
* Gdy użytkownik wybierze kurs, wyświetlane są powiązane dane z jednostki `Enrollments`. Na powyższym obrazie wyświetlana jest nazwa ucznia i jej klasy. Jednostki `Course` i `Enrollment` znajdują się w relacji jeden do wielu.

### <a name="create-a-view-model"></a>Tworzenie modelu widoku

Na stronie instruktorzy są wyświetlane dane z trzech różnych tabel. Wymagany jest model widoku, który zawiera trzy właściwości reprezentujące trzy tabele.

Utwórz *SchoolViewModels/InstructorIndexData. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a>Strony instruktorów dla szkieletów

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Postępuj zgodnie z instrukcjami w temacie Tworzenie [szkieletu stron uczniów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:

  * Utwórz folder *stron/instruktorów* .
  * Użyj `Instructor` dla klasy modelu.
  * Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Utwórz folder *stron/instruktorów* .

* Uruchom następujące polecenie, aby połączyć strony instruktora.

  **W systemie Windows:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  **W systemie Linux lub macOS:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

Aby sprawdzić, jak wygląda strona szkieletowa przed aktualizacją, uruchom aplikację i przejdź do strony instruktorzy.

Aktualizowanie *stron/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

Metoda `OnGetAsync` akceptuje opcjonalne dane trasy dla identyfikatora wybranego instruktora.

Zbadaj zapytanie w pliku */instruktors/index. cshtml. cs* :

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

Kod określa eager ładowania dla następujących właściwości nawigacji:

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

Zwróć uwagę na powtarzanie `Include` i `ThenInclude` metod `CourseAssignments` i `Course`. To powtórzenie jest niezbędne do określenia eager ładowania dla dwóch właściwości nawigacji jednostki `Course`.

Poniższy kod jest wykonywany po wybraniu instruktora (`id != null`).

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

Wybrany instruktor jest pobierany z listy instruktorów w modelu widoku. Właściwość `Courses` modelu widoku jest ładowana z jednostkami `Course`, które są `CourseAssignments` właściwości nawigacji tego instruktora.

Metoda `Where` zwraca kolekcję. Ale w tym przypadku filtr wybierze pojedynczą jednostkę. Dlatego metoda `Single` jest wywoływana w celu przekonwertowania kolekcji na jedną jednostkę `Instructor`. Jednostka `Instructor` zapewnia dostęp do właściwości `CourseAssignments`. `CourseAssignments` zapewnia dostęp do powiązanych jednostek `Course`.

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

Metoda `Single` jest używana w kolekcji, gdy kolekcja zawiera tylko jeden element. Metoda `Single` zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (null w tym przypadku), jeśli kolekcja jest pusta.

Poniższy kod wypełnia Właściwość `Enrollments` modelu widoku w przypadku wybrania kursu:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a>Aktualizowanie strony indeksu instruktorów

Aktualizowanie *stron/instruktorów/index. cshtml* przy użyciu następującego kodu.

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

Poprzedni kod wprowadza następujące zmiany:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int?}"`. `"{id:int?}"` jest szablonem trasy. Szablon trasy zmienia ciągi zapytań liczb całkowitych w adresie URL, aby przesyłać dane. Na przykład kliknięcie linku **Wybierz** dla instruktora z tylko dyrektywą `@page` generuje adres URL podobny do następującego:

  `https://localhost:5001/Instructors?id=2`

  Gdy dyrektywa Page jest `@page "{id:int?}"`, adres URL to:

  `https://localhost:5001/Instructors/2`

* Dodaje kolumnę **pakietu Office** , która wyświetla `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. Ponieważ jest to relacja "jeden do zera" lub "jeden", nie może być powiązana jednostka OfficeAssignment.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Dodaje kolumnę **kursów** , która wyświetla nauczanie kursów przez każdego instruktora. Aby uzyskać więcej informacji na temat tej składni Razor, zobacz [jawne przejście liniowe](xref:mvc/views/razor#explicit-line-transition) .

* Dodaje kod, który dynamicznie dodaje `class="success"` do elementu `tr` wybranego instruktora i kursu. Ustawia kolor tła dla wybranego wiersza przy użyciu klasy Bootstrap.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Dodaje nowe hiperłącze z etykietą **SELECT**. Ten link powoduje wysłanie wybranego identyfikatora instruktora do metody `Index` i ustawienie koloru tła.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* Dodaje tabelę kursów dla wybranego instruktora.

* Dodaje tabelę rejestracji uczniów dla wybranego kursu.

Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie zostanie wyświetlona `Location` (Biuro) z jednostki powiązanej `OfficeAssignment`. Jeśli `OfficeAssignment` ma wartość null, zostanie wyświetlona pusta komórka tabeli.

Kliknij link **Wybierz** dla instruktora. Zostaną wyświetlone zmiany w stylu wiersza i kursy przypisane do tego instruktora.

Wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.

![Wybrany instruktor i kurs dla instruktorów](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a>Korzystanie z jednego

Metoda `Single` może przekazać warunek `Where` zamiast wywoływania `Where` metody oddzielnie:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

Użycie `Single` z warunkiem WHERE jest kwestią preferencji osobistych. Nie oferuje żadnych korzyści z używania metody `Where`.

## <a name="explicit-loading"></a>jawne ładowanie

Bieżący kod określa eager ładowania dla `Enrollments` i `Students`:

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

Załóżmy, że użytkownicy rzadko chcą widzieć rejestracje w kursie. W takim przypadku Optymalizacja będzie ładować dane rejestracji tylko wtedy, gdy jest to wymagane. W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawnego ładowania `Enrollments` i `Students`.

Aktualizowanie *stron/instruktorów/index. cshtml. cs* przy użyciu następującego kodu.

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

Poprzedni kod odrzuca metody *ThenInclude* w celu uzyskania danych dotyczących rejestracji i uczniów. W przypadku wybrania kursu jawny kod ładowania pobiera:

* Jednostki `Enrollment` dla wybranego kursu.
* `Student` jednostek dla każdej `Enrollment`.

Zwróć uwagę, że powyższy kod komentarz ma `.AsNoTracking()`. Właściwości nawigacji można jawnie załadować tylko dla śledzonych jednostek.

Testowanie aplikacji. Z perspektywy użytkownika aplikacja zachowuje się identycznie z poprzednią wersją.

## <a name="next-steps"></a>Następne kroki

W następnym samouczku pokazano, jak zaktualizować powiązane dane.

>[!div class="step-by-step"]
>[Poprzedni samouczek](xref:data/ef-rp/complex-data-model)
>[następnego samouczka](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tym samouczku dane pokrewne są odczytywane i wyświetlane. Powiązane dane to dane, które EF Core ładowane do właściwości nawigacji.

Jeśli występują problemy, których nie można rozwiązać, [Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

Na poniższych ilustracjach przedstawiono ukończone strony dla tego samouczka:

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Eager, jawne i opóźnione ładowanie powiązanych danych

Istnieje kilka sposobów, EF Core mogą ładować powiązane dane do właściwości nawigacji jednostki:

* [Ładowanie eager](/ef/core/querying/related-data#eager-loading). Ładowanie eager jest, gdy zapytanie dla jednego typu jednostki również ładuje powiązane jednostki. Po odczytaniu jednostki są pobierane powiązane dane. Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne. EF Core będzie wystawiał wiele zapytań dla niektórych typów ładowania eager. Wygenerowanie wielu zapytań może być bardziej wydajne niż w przypadku niektórych zapytań w EF6, w których wystąpiło pojedyncze zapytanie. Ładowanie eager jest określone przy użyciu metod `Include` i `ThenInclude`.

  ![Przykład ładowania eager](read-related-data/_static/eager-loading.png)
 
  Podczas ładowania eager są wysyłane wiele zapytań, gdy zostanie dołączona Nawigacja kolekcji:

  * Jedno zapytanie dotyczące głównej kwerendy 
  * Jedno zapytanie dla każdej kolekcji "Edge" w drzewie ładowania.

* Oddziel zapytania z `Load`: dane można pobrać w oddzielnych zapytaniach, a EF Core "naprawia" właściwości nawigacji. "rozwiązuje" oznacza, że EF Core automatycznie wypełnia właściwości nawigacji. Oddzielne zapytania o `Load` są bardziej podobne do jawnego ładowania niż ładowanie eager.

  ![Przykład oddzielnych zapytań](read-related-data/_static/separate-queries.png)

  Uwaga: EF Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu. Nawet jeśli dane dla właściwości nawigacji *nie* są jawnie uwzględniane, właściwość można nadal wypełnić, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.

* [Jawne ładowanie](/ef/core/querying/related-data#explicit-loading). Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Kod musi być zapisany, aby można było pobrać powiązane dane, gdy jest to konieczne. Jawne ładowanie z oddzielnymi zapytaniami powoduje wysłanie wielu zapytań do bazy danych. W przypadku jawnego ładowania kod określa właściwości nawigacji do załadowania. Użyj metody `Load`, aby przeprowadzić jawne ładowanie. Na przykład:

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* [Ładowanie z opóźnieniem](/ef/core/querying/related-data#lazy-loading). [Ładowanie z opóźnieniem zostało dodane do EF Core w wersji 2,1](/ef/core/querying/related-data#lazy-loading). Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie. Zapytanie jest wysyłane do bazy danych przy każdym dostępie do właściwości nawigacji po raz pierwszy.

* Operator `Select` ładuje tylko powiązane dane.

## <a name="create-a-course-page-that-displays-department-name"></a>Tworzenie strony kursu, która wyświetla nazwę działu

Jednostka kursu zawiera właściwość nawigacji, która zawiera jednostkę `Department`. Jednostka `Department` zawiera dział, do którego jest przypisany kurs.

Aby wyświetlić nazwę przypisanego działu na liście kursów:

* Pobierz Właściwość `Name` z jednostki `Department`.
* Jednostka `Department` pochodzi z właściwości nawigacji `Course.Department`.

![Kurs. Dział](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a>Tworzenie szkieletu modelu kursu

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio) 

Postępuj zgodnie z instrukcjami w obszarze [szkieletem model studenta](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Course` dla klasy model.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

Poprzednie polecenie szkieletuje model `Course`. Otwórz projekt w programie Visual Studio.

Otwórz *stronę/kursy/index. cshtml. cs* i Przeanalizuj metodę `OnGetAsync`. Aparat szkieletu określony eager ładowania dla właściwości nawigacji `Department`. Metoda `Include` określa ładowanie eager.

Uruchom aplikację i wybierz łącze **kursy** . W kolumnie dział zostanie wyświetlona `DepartmentID`, co nie jest przydatne.

Zaktualizuj metodę `OnGetAsync` przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Poprzedni kod dodaje `AsNoTracking`. `AsNoTracking` zwiększa wydajność, ponieważ zwrócone jednostki nie są śledzone. Jednostki nie są śledzone, ponieważ nie są aktualizowane w bieżącym kontekście.

Aktualizuj *strony/kursy/index. cshtml* z następującymi wyróżnionymi znacznikami:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

W kodzie szkieletowym wprowadzono następujące zmiany:

* Zmieniono nagłówek z indeksu na kursy.
* Dodano kolumnę **liczbową** , która wyświetla wartość właściwości `CourseID`. Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są oznaczane przez użytkowników końcowych. Jednak w tym przypadku klucz podstawowy ma znaczenie.
* Zmieniono kolumnę **działu** , aby wyświetlić nazwę działu. Kod wyświetla Właściwość `Name` jednostki `Department`, która jest ładowana do właściwości nawigacji `Department`:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uruchom aplikację i wybierz kartę **kursy** , aby wyświetlić listę z nazwami działów.

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a>Ładowanie powiązanych danych przy użyciu opcji Select

Metoda `OnGetAsync` ładuje powiązane dane przy użyciu metody `Include`:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

Operator `Select` ładuje tylko powiązane dane. Dla pojedynczych elementów, takich jak `Department.Name` używa SPRZĘŻENIa wewnętrznego SQL. W przypadku kolekcji jest on wykorzystywany przez inny dostęp do bazy danych, ale w związku z tym wykonuje operator `Include` w kolekcjach.

Poniższy kod ładuje powiązane dane przy użyciu metody `Select`:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Zobacz [IndexSelect. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) , aby zapoznać się z kompletnym przykładem.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Utwórz stronę instruktorów pokazującą kursy i rejestracje

W tej sekcji zostanie utworzona strona instruktorzy.

<a name="IP"></a>
![strony indeksu instruktorów](read-related-data/_static/instructors-index.png)

Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:

* Lista instruktorów wyświetla powiązane dane z jednostki `OfficeAssignment` (Office na powyższym obrazie). Jednostki `Instructor` i `OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego. Ładowanie eager jest używane dla jednostek `OfficeAssignment`. Ładowanie eager jest zwykle wydajniejsze, gdy wymagane jest wyświetlenie powiązanych danych. W takim przypadku wyświetlane są przypisania pakietu Office dla instruktorów.
* Gdy użytkownik wybierze instruktora (Harui na poprzednim obrazie), zostaną wyświetlone powiązane jednostki `Course`. Jednostki `Instructor` i `Course` znajdują się w relacji wiele-do-wielu. Ładowanie eager jest używane dla jednostek `Course` i powiązanych `Department` jednostek. W takim przypadku oddzielne zapytania mogą być bardziej wydajne, ponieważ potrzebują tylko kursów dla wybranego instruktora. Ten przykład pokazuje, jak używać eager ładowania dla właściwości nawigacji w jednostkach, które są we właściwościach nawigacji.
* Gdy użytkownik wybierze kurs (chemia na powyższym obrazie), zostaną wyświetlone powiązane dane z jednostki `Enrollments`. Na powyższym obrazie wyświetlana jest nazwa ucznia i jej klasy. Jednostki `Course` i `Enrollment` znajdują się w relacji jeden do wielu.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Utwórz model widoku dla widoku indeksu instruktora

Na stronie instruktorzy są wyświetlane dane z trzech różnych tabel. Tworzony jest model widoku, który zawiera trzy jednostki reprezentujące trzy tabele.

W folderze *SchoolViewModels* Utwórz *InstructorIndexData.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Tworzenie szkieletu modelu instruktora

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio) 

Postępuj zgodnie z instrukcjami w obszarze [szkieletem model studenta](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Instructor` dla klasy model.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

Poprzednie polecenie szkieletuje model `Instructor`. 
Uruchom aplikację i przejdź do strony instruktorzy.

Zamień *strony/instruktorów/index. cshtml. cs* na następujący kod:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

Metoda `OnGetAsync` akceptuje opcjonalne dane trasy dla identyfikatora wybranego instruktora.

Zbadaj zapytanie w pliku */instruktors/index. cshtml. cs* :

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Zapytanie zawiera dwa:

* `OfficeAssignment`: wyświetlane w [widoku instruktorów](#IP).
* `CourseAssignments`: co to jest w nauczaniu kursów.

### <a name="update-the-instructors-index-page"></a>Aktualizowanie strony indeksu instruktorów

Aktualizowanie *stron/instruktorów/index. cshtml* przy użyciu następującego znacznika:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int?}"`. `"{id:int?}"` jest szablonem trasy. Szablon trasy zmienia ciągi zapytań liczb całkowitych w adresie URL, aby przesyłać dane. Na przykład kliknięcie linku **Wybierz** dla instruktora z tylko dyrektywą `@page` generuje adres URL podobny do następującego:

  `http://localhost:1234/Instructors?id=2`

  Gdy dyrektywa Page jest `@page "{id:int?}"`, poprzedni adres URL to:

  `http://localhost:1234/Instructors/2`

* Tytuł strony to **Instruktorzy**.
* Dodano kolumnę **pakietu Office** , która wyświetla `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. Ponieważ jest to relacja "jeden do zera" lub "jeden", nie może być powiązana jednostka OfficeAssignment.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Dodano kolumnę **kursów** , która wyświetla nauczanie kursów przez każdego instruktora. Aby uzyskać więcej informacji na temat tej składni Razor, zobacz [jawne przejście liniowe](xref:mvc/views/razor#explicit-line-transition) .

* Dodano kod, który dynamicznie dodaje `class="success"` do elementu `tr` wybranego instruktora. Ustawia kolor tła dla wybranego wiersza przy użyciu klasy Bootstrap.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Dodano nowe hiperłącze z etykietą **SELECT**. Ten link powoduje wysłanie wybranego identyfikatora instruktora do metody `Index` i ustawienie koloru tła.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie zostanie wyświetlona `Location` (Biuro) z jednostki powiązanej `OfficeAssignment`. Jeśli OfficeAssignment ' ma wartość null, zostanie wyświetlona pusta komórka tabeli.

Kliknij link **Wybierz** . Styl wiersza zmienia się.

### <a name="add-courses-taught-by-selected-instructor"></a>Dodaj nauczanie kursów według wybranych instruktorów

Zaktualizuj metodę `OnGetAsync` na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Dodaj `public int CourseID { get; set; }`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

Zbadaj zaktualizowane zapytanie:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Poprzednie zapytanie dodaje jednostki `Department`.

Poniższy kod jest wykonywany po wybraniu instruktora (`id != null`). Wybrany instruktor jest pobierany z listy instruktorów w modelu widoku. Właściwość `Courses` modelu widoku jest ładowana z jednostkami `Course`, które są `CourseAssignments` właściwości nawigacji tego instruktora.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

Metoda `Where` zwraca kolekcję. W poprzedniej metodzie `Where` zwracana jest tylko jedna jednostka `Instructor`. Metoda `Single` konwertuje kolekcję na jedną jednostkę `Instructor`. Jednostka `Instructor` zapewnia dostęp do właściwości `CourseAssignments`. `CourseAssignments` zapewnia dostęp do powiązanych jednostek `Course`.

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

Metoda `Single` jest używana w kolekcji, gdy kolekcja zawiera tylko jeden element. Metoda `Single` zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (null w tym przypadku), jeśli kolekcja jest pusta. Używanie `SingleOrDefault` w pustej kolekcji:

* Wynikiem jest wyjątek (od próby znalezienia właściwości `Courses` w odwołaniu o wartości null).
* Komunikat o wyjątku będzie mniej jasno wskazywał przyczynę problemu.

Poniższy kod wypełnia Właściwość `Enrollments` modelu widoku w przypadku wybrania kursu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Dodaj następujący znacznik na końcu strony */instruktorów/index. cshtml* Razor:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Powyższy znacznik wyświetla listę kursów związanych z instruktorem w przypadku wybrania instruktora.

Testowanie aplikacji. Kliknij link **Wybierz** na stronie instruktorów.

### <a name="show-student-data"></a>Pokaż dane ucznia

W tej sekcji aplikacja zostanie zaktualizowana, aby wyświetlić dane uczniów dla wybranego kursu.

Zaktualizuj zapytanie w metodzie `OnGetAsync` na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Aktualizowanie *stron/instruktorów/index. cshtml*. Dodaj następujący znacznik na końcu pliku:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Powyższy znacznik wyświetla listę studentów, którzy są zarejestrowani w wybranym kursie.

Odśwież stronę i wybierz instruktora. Wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.

![Wybrany instruktor i kurs dla instruktorów](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Korzystanie z jednego

Metoda `Single` może przekazać warunek `Where` zamiast wywoływania `Where` metody oddzielnie:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

Poprzednie podejście `Single` nie zapewnia żadnych korzyści z używania `Where`. Niektórzy deweloperzy preferują styl podejścia `Single`owego.

## <a name="explicit-loading"></a>jawne ładowanie

Bieżący kod określa eager ładowania dla `Enrollments` i `Students`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Załóżmy, że użytkownicy rzadko chcą widzieć rejestracje w kursie. W takim przypadku Optymalizacja będzie ładować dane rejestracji tylko wtedy, gdy jest to wymagane. W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawnego ładowania `Enrollments` i `Students`.

Zaktualizuj `OnGetAsync` przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Poprzedni kod odrzuca metody *ThenInclude* w celu uzyskania danych dotyczących rejestracji i uczniów. W przypadku wybrania kursu wyróżniony kod pobiera:

* Jednostki `Enrollment` dla wybranego kursu.
* `Student` jednostek dla każdej `Enrollment`.

Zwróć uwagę na powyższy kod komentarza `.AsNoTracking()`. Właściwości nawigacji można jawnie załadować tylko dla śledzonych jednostek.

Testowanie aplikacji. Z perspektywy użytkowników aplikacja zachowuje się identycznie z poprzednią wersją.

W następnym samouczku pokazano, jak zaktualizować powiązane dane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube (part1)](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [Wersja tego samouczka usługi YouTube (part2)](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
>[Poprzednie](xref:data/ef-rp/complex-data-model)
>[dalej](xref:data/ef-rp/update-related-data)

::: moniker-end
