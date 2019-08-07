---
title: Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8
author: rick-anderson
description: W tym samouczku odczytasz i wyświetlasz powiązane dane, czyli dane, które Entity Framework ładowane do właściwości nawigacji.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 93fbb4741f476368d75d23162d6e2597de7b263e
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819916"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8

Autorzy [Dykstra](https://github.com/tdykstra), [Jan P Kowalski](https://twitter.com/thereformedprog)i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

W tym samouczku dane pokrewne są odczytywane i wyświetlane. Powiązane dane to dane, które EF Core ładowane do właściwości nawigacji.

Jeśli napotkasz problemy, nie można rozwiązać, [pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

Na poniższych ilustracjach przedstawiono ukończone strony dla tego samouczka:

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Eager, jawne i opóźnione ładowanie powiązanych danych

Istnieje kilka sposobów, EF Core mogą ładować powiązane dane do właściwości nawigacji jednostki:

* [Ładowanie eager](/ef/core/querying/related-data#eager-loading). Ładowanie eager jest, gdy zapytanie dla jednego typu jednostki również ładuje powiązane jednostki. Po odczytaniu jednostki są pobierane powiązane dane. Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne. EF Core będzie wystawiał wiele zapytań dla niektórych typów ładowania eager. Wygenerowanie wielu zapytań może być bardziej wydajne niż w przypadku niektórych zapytań w EF6, w których wystąpiło pojedyncze zapytanie. Ładowanie eager jest określone przy użyciu `Include` metod `ThenInclude` i.

  ![Przykład ładowania eager](read-related-data/_static/eager-loading.png)
 
  Podczas ładowania eager są wysyłane wiele zapytań, gdy zostanie dołączona Nawigacja kolekcji:

  * Jedno zapytanie dotyczące głównej kwerendy 
  * Jedno zapytanie dla każdej kolekcji "Edge" w drzewie ładowania.

* Oddziel zapytania `Load`z: Dane można pobrać w oddzielnych zapytaniach, a EF Core "naprawia" właściwości nawigacji. "rozwiązuje" oznacza, że EF Core automatycznie wypełnia właściwości nawigacji. Osobne zapytania `Load` i są bardziej podobne do EXPLICT ładowania niż ładowanie eager.

  ![Przykład oddzielnych zapytań](read-related-data/_static/separate-queries.png)

  Uwaga: EF Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu. Nawet jeśli dane dla właściwości nawigacji *nie* są jawnie uwzględniane, właściwość można nadal wypełnić, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.

* [Jawne ładowanie](/ef/core/querying/related-data#explicit-loading). Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Kod musi być zapisany, aby można było pobrać powiązane dane, gdy jest to konieczne. Jawne ładowanie z oddzielnymi zapytaniami powoduje wysłanie wielu zapytań do bazy danych. W przypadku jawnego ładowania kod określa właściwości nawigacji do załadowania. Użyj metody `Load` , aby przeprowadzić jawne ładowanie. Na przykład:

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* [Ładowanie](/ef/core/querying/related-data#lazy-loading)z opóźnieniem. [Ładowanie z opóźnieniem zostało dodane do EF Core w wersji 2,1](/ef/core/querying/related-data#lazy-loading). Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie. Zapytanie jest wysyłane do bazy danych przy każdym dostępie do właściwości nawigacji po raz pierwszy.

* `Select` Operator ładuje tylko powiązane dane.

## <a name="create-a-course-page-that-displays-department-name"></a>Tworzenie strony kursu, która wyświetla nazwę działu

Jednostka kursu zawiera właściwość nawigacji, która zawiera `Department` jednostkę. `Department` Jednostka zawiera dział, do którego jest przypisany kurs.

Aby wyświetlić nazwę przypisanego działu na liście kursów:

* `Name` Pobierz Właściwość`Department` z jednostki.
* `Department` Jednostka pochodzi `Course.Department` z właściwości nawigacji.

![Kurs. Dział](read-related-data/_static/dep-crs.png)

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

Otwórz *stronę/kursy/index. cshtml. cs* i Przeanalizuj `OnGetAsync` metodę. Aparat tworzenia szkieletów określił eager ładowania dla `Department` właściwości nawigacji. `Include` Metoda określa eager ładowania.

Uruchom aplikację i wybierz łącze **kursy** . W kolumnie dział zostanie wyświetlona wartość `DepartmentID`, która nie jest przydatna.

`OnGetAsync` Zaktualizuj metodę przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Poprzedni kod dodaje `AsNoTracking`. `AsNoTracking`zwiększa wydajność, ponieważ zwrócone jednostki nie są śledzone. Jednostki nie są śledzone, ponieważ nie są aktualizowane w bieżącym kontekście.

Aktualizuj *strony/kursy/index. cshtml* z następującymi wyróżnionymi znacznikami:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

W kodzie szkieletowym wprowadzono następujące zmiany:

* Zmieniono nagłówek z indeksu na kursy.
* Dodano kolumnę **liczbową** , która wyświetla `CourseID` wartość właściwości. Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są oznaczane przez użytkowników końcowych. Jednak w tym przypadku klucz podstawowy ma znaczenie.
* Zmieniono kolumnę **działu** , aby wyświetlić nazwę działu. Kod wyświetla `Name` Właściwość `Department` jednostki `Department` , która jest ładowana do właściwości nawigacji:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uruchom aplikację i wybierz kartę **kursy** , aby wyświetlić listę z nazwami działów.

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a>Ładowanie powiązanych danych przy użyciu opcji Select

Metoda ładuje powiązane dane `Include` przy użyciu metody: `OnGetAsync`

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` Operator ładuje tylko powiązane dane. Dla pojedynczych elementów, podobnie jak `Department.Name` w przypadku użycia sprzężenia wewnętrznego SQL. W przypadku kolekcji program używa innego dostępu do bazy danych, ale w `Include` związku z tym wykonuje operator dla kolekcji.

Poniższy kod ładuje powiązane dane przy użyciu `Select` metody:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Zobacz [IndexSelect. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) , aby zapoznać się z kompletnym przykładem.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Utwórz stronę instruktorów pokazującą kursy i rejestracje

W tej sekcji zostanie utworzona strona instruktorzy.

<a name="IP"></a>
![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:

* Lista instruktorów wyświetla powiązane dane z `OfficeAssignment` jednostki (Office na powyższym obrazie). Jednostki `Instructor` i`OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego. Eager ładowania jest używany dla `OfficeAssignment` jednostek. Ładowanie eager jest zwykle wydajniejsze, gdy wymagane jest wyświetlenie powiązanych danych. W takim przypadku wyświetlane są przypisania pakietu Office dla instruktorów.
* Gdy użytkownik wybierze instruktora (Harui na poprzednim obrazie), zostaną wyświetlone `Course` powiązane jednostki. Jednostki `Instructor` i`Course` znajdują się w relacji wiele-do-wielu. Ładowanie eager jest używane dla `Course` jednostek i ich powiązanych `Department` jednostek. W takim przypadku oddzielne zapytania mogą być bardziej wydajne, ponieważ potrzebują tylko kursów dla wybranego instruktora. Ten przykład pokazuje, jak używać eager ładowania dla właściwości nawigacji w jednostkach, które są we właściwościach nawigacji.
* Gdy użytkownik wybierze kurs (chemia na powyższym obrazie), zostaną wyświetlone `Enrollments` powiązane dane z jednostki. Na powyższym obrazie wyświetlana jest nazwa ucznia i jej klasy. Jednostki `Course` i`Enrollment` znajdują się w relacji jeden do wielu.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Utwórz model widoku dla widoku indeksu instruktora

Na stronie instruktorzy są wyświetlane dane z trzech różnych tabel. Tworzony jest model widoku, który zawiera trzy jednostki reprezentujące trzy tabele.

W folderze *SchoolViewModels* Utwórz *InstructorIndexData.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Tworzenie szkieletu modelu instruktora

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Instructor` dla klasy modelu.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

Poprzedni szkielety mechanizmów polecenia `Instructor` modelu. Uruchom aplikację i przejdź do strony instruktorzy.

Zamień *strony/instruktorów/index. cshtml. cs* na następujący kod:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

`OnGetAsync` Metoda akceptuje opcjonalne dane trasy dla identyfikatora wybranego instruktora.

Zbadaj zapytanie w pliku */instruktors/index. cshtml. cs* :

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Zapytanie zawiera dwa:

* `OfficeAssignment`: Wyświetlane w [widoku instruktorów](#IP).
* `CourseAssignments`: Które przynosi kursy.

### <a name="update-the-instructors-index-page"></a>Aktualizowanie strony indeksu instruktorów

Aktualizowanie *stron/instruktorów/index. cshtml* przy użyciu następującego znacznika:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`. `"{id:int?}"`jest szablonem trasy. Szablon trasy zmienia ciągi zapytań liczb całkowitych w adresie URL, aby przesyłać dane. Na przykład kliknięcie linku **Wybierz** dla instruktora z tylko `@page` dyrektywą spowoduje utworzenie adresu URL w następujący sposób:

  `http://localhost:1234/Instructors?id=2`

  Gdy dyrektywa Page ma `@page "{id:int?}"`wartość, poprzedni adres URL to:

  `http://localhost:1234/Instructors/2`

* Tytuł strony to **Instruktorzy**.
* Dodano kolumnę **pakietu Office** , która `item.OfficeAssignment.Location` jest wyświetlana `item.OfficeAssignment` tylko wtedy, gdy nie ma wartości null. Ponieważ jest to relacja "jeden do zera" lub "jeden", nie może być powiązana jednostka OfficeAssignment.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Dodano kolumnę **kursów** , która wyświetla nauczanie kursów przez każdego instruktora. Aby uzyskać więcej informacji na temat składni Razor, zobacz [ `@:` jawne przejście liniowe](xref:mvc/views/razor#explicit-line-transition-with-) .

* Dodano kod, który dynamicznie `class="success"` dodaje `tr` do elementu wybranego instruktora. Ustawia kolor tła dla wybranego wiersza przy użyciu klasy Bootstrap.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Dodano nowe hiperłącze z etykietą **SELECT**. Ten link powoduje wysłanie wybranego identyfikatora instruktora do `Index` metody i ustawienie koloru tła.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie zostanie wyświetlona `Location` strona (Biuro) z jednostki powiązanej. `OfficeAssignment` Jeśli OfficeAssignment ' ma wartość null, zostanie wyświetlona pusta komórka tabeli.

![Nie wybrano niczego ze strony indeksu instruktorów](read-related-data/_static/instructors-index-no-selection.png)

Kliknij link **Wybierz** . Styl wiersza zmienia się.

### <a name="add-courses-taught-by-selected-instructor"></a>Dodaj nauczanie kursów według wybranych instruktorów

Zaktualizuj metodę na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu: `OnGetAsync`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Add `public int CourseID { get; set; }`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

Zbadaj zaktualizowane zapytanie:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Poprzednie zapytanie dodaje `Department` jednostki.

Poniższy kod jest wykonywany po wybraniu instruktora (`id != null`). Wybrany instruktor jest pobierany z listy instruktorów w modelu widoku. `Courses` Właściwość widoku modelu jest ładowana `Course` z jednostkami `CourseAssignments` z tej właściwości nawigacji instruktora.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` Metoda zwraca kolekcję. W poprzedniej `Where` metodzie zwracana jest tylko pojedyncza `Instructor` jednostka. Metoda konwertuje kolekcję na jedną `Instructor` jednostkę. `Single` Jednostka zapewnia dostęp `CourseAssignments` do właściwości. `Instructor` `CourseAssignments`zapewnia dostęp do powiązanych `Course` jednostek.

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

`Single` Metoda jest używana w kolekcji, gdy kolekcja zawiera tylko jeden element. `Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (null w tym przypadku), jeśli kolekcja jest pusta. Używanie `SingleOrDefault` w pustej kolekcji:

* Wynikiem jest wyjątek (od próby znalezienia `Courses` właściwości w odwołaniu o wartości null).
* Komunikat o wyjątku będzie mniej jasno wskazywał przyczynę problemu.

Poniższy kod wypełnia `Enrollments` właściwość modelu widoku w przypadku wybrania kursu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Dodaj następujący znacznik na końcu strony */instruktorów/index. cshtml* Razor:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Powyższy znacznik wyświetla listę kursów związanych z instruktorem w przypadku wybrania instruktora.

Przetestuj aplikację. Kliknij link **Wybierz** na stronie instruktorów.

![Wybrany instruktor strony indeksu instruktora](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Pokaż dane ucznia

W tej sekcji aplikacja zostanie zaktualizowana, aby wyświetlić dane uczniów dla wybranego kursu.

Zaktualizuj zapytanie w `OnGetAsync` metodzie na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Aktualizowanie *stron/instruktorów/index. cshtml*. Dodaj następujący znacznik na końcu pliku:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Powyższy znacznik wyświetla listę studentów, którzy są zarejestrowani w wybranym kursie.

Odśwież stronę i wybierz instruktora. Wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.

![Wybrany instruktor i kurs dla instruktorów](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Korzystanie z jednego

Metoda może `Where` przekazać`Where` warunek zamiast wywołania metody osobno: `Single`

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

Poprzednie `Single` podejście nie zapewnia żadnych korzyści w porównaniu `Where`z korzystaniem z programu. Niektórzy deweloperzy preferują `Single` styl podejścia.

## <a name="explicit-loading"></a>Jawne ładowanie

Bieżący kod określa eager ładowania dla `Enrollments` i: `Students`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Załóżmy, że użytkownicy rzadko chcą widzieć rejestracje w kursie. W takim przypadku Optymalizacja będzie ładować dane rejestracji tylko wtedy, gdy jest to wymagane. W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawnego `Enrollments` ładowania i `Students`.

`OnGetAsync` Zaktualizuj program przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Poprzedni kod odrzuca metody *ThenInclude* w celu uzyskania danych dotyczących rejestracji i uczniów. W przypadku wybrania kursu wyróżniony kod pobiera:

* `Enrollment` Jednostki wybranego kursu.
* Jednostki `Student` dla każdej z `Enrollment`nich.

Zwróć uwagę na powyższe Komentarze `.AsNoTracking()`w kodzie. Właściwości nawigacji można jawnie załadować tylko dla śledzonych jednostek.

Przetestuj aplikację. Z perspektywy użytkowników aplikacja zachowuje się identycznie z poprzednią wersją.

W następnym samouczku pokazano, jak zaktualizować powiązane dane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube (part1)](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [Wersja tego samouczka usługi YouTube (part2)](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
>[Poprzedni](xref:data/ef-rp/complex-data-model)Następny
>[](xref:data/ef-rp/update-related-data)
