---
title: Razor Pages z EF Core w ASP.NET Core-Update — powiązane dane — 7 z 8
author: rick-anderson
description: W tym samouczku należy zaktualizować powiązane dane przez zaktualizowanie pól klucza obcego i właściwości nawigacji.
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/update-related-data
ms.openlocfilehash: fdfdb14ff8414b8bf30f9b95be7ba0a6bcbd2995
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656423"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>Razor Pages z EF Core w ASP.NET Core-Update — powiązane dane — 7 z 8

Przez [Tomasz Dykstra](https://github.com/tdykstra)i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

W tym samouczku pokazano, jak zaktualizować powiązane dane. Na poniższych ilustracjach przedstawiono niektóre z ukończonych stron.

![strony edycji kursu](update-related-data/_static/course-edit30.png)
![stronie Edycja instruktora](update-related-data/_static/instructor-edit-courses30.png)

## <a name="update-the-course-create-and-edit-pages"></a>Aktualizowanie stron tworzenie i edytowanie kursu

Kod szkieletowy dla stron tworzenie i edytowanie danych zawiera listę rozwijaną dział, która zawiera identyfikator działu (liczba całkowita). Lista rozwijana powinna zawierać nazwę działu, więc obie te strony muszą mieć listę nazw działów. Aby zapewnić tę listę, użyj klasy bazowej dla stron tworzenia i edytowania.

### <a name="create-a-base-class-for-course-create-and-edit"></a>Tworzenie klasy bazowej do tworzenia i edytowania kursu

Utwórz plik *Pages/kurss/DepartmentNamePageModel. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Courses/DepartmentNamePageModel.cs)]

Poprzedni kod tworzy [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) , aby zawierał listę nazw działów. Jeśli `selectedDepartment` jest określony, ten dział jest wybierany w `SelectList`.

Klasy Utwórz i edytuj model strony będą pochodzić od `DepartmentNamePageModel`.

### <a name="update-the-course-create-page-model"></a>Aktualizuj model tworzenia strony kursu

Kurs jest przypisywany do działu. Klasa bazowa dla stron tworzenia i edytowania zawiera `SelectList` do wybierania działu. Lista rozwijana, która używa `SelectList` ustawia właściwość `Course.DepartmentID` FOREIGN KEY (obcy). EF Core używa `Course.DepartmentID` FK do załadowania właściwości nawigacji `Department`.

![Utwórz kurs](update-related-data/_static/ddl30.png)

Zaktualizuj *strony/kursy/Utwórz. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Courses/Create.cshtml.cs?highlight=7,18,27-41)]

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

Powyższy kod:

* Pochodzi od `DepartmentNamePageModel`.
* Używa `TryUpdateModelAsync`, aby [zapobiec](xref:data/ef-rp/crud#overposting)przepisywaniu.
* Usuwa `ViewData["DepartmentID"]`. `DepartmentNameSL` z klasy bazowej jest modelem o jednoznacznie określonym typie i będzie używany przez stronę Razor. Modele silnie wpisane są preferowane za pośrednictwem słabo wpisanych. Aby uzyskać więcej informacji, zobacz [słabo wpisane dane (ViewData i ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-course-create-razor-page"></a>Aktualizowanie strony Tworzenie Razor dla kursu

Zaktualizuj *strony/kursy/Utwórz. cshtml* przy użyciu następującego kodu:

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Create.cshtml?highlight=29-34)]

Poprzedni kod wprowadza następujące zmiany:

* Zmienia podpis z **DepartmentID** na **dział**.
* Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (z klasy bazowej).
* Dodaje opcję "Wybierz dział". Ta zmiana renderuje "Select Department" na liście rozwijanej, gdy nie wybrano jeszcze żadnego działu, a nie pierwszego działu.
* Dodaje komunikat weryfikacyjny, gdy nie wybrano działu.

Strona Razor używa [pomocnika wybierania tagu](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Przetestuj stronę tworzenie. Na stronie Tworzenie zostanie wyświetlona nazwa działu, a nie identyfikator działu.

### <a name="update-the-course-edit-page-model"></a>Aktualizowanie modelu strony edytowania kursu

Zaktualizuj *strony/kursy/Edytuj. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40-66)]

Zmiany są podobne do tych, które zostały wprowadzone w modelu tworzenia strony. W poprzednim kodzie `PopulateDepartmentsDropDownList` przekazuje identyfikator działu, który wybiera tego działu z listy rozwijanej.

### <a name="update-the-course-edit-razor-page"></a>Aktualizowanie strony edytowanie kursu Razor

Aktualizowanie *stron/kursów/Edit. cshtml* przy użyciu następującego kodu:

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Poprzedni kod wprowadza następujące zmiany:

* Wyświetla identyfikator kursu. Zazwyczaj klucz podstawowy (PK) jednostki nie jest wyświetlany. PKs są zwykle oznaczane przez użytkowników. W tym przypadku klucz podstawowy jest numerem kursu.
* Zmienia podpis dla listy rozwijanej działu od **DepartmentID** do **działu**.
* Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (z klasy bazowej).

Ta strona zawiera pole ukryte (`<input type="hidden">`) dla numeru kursu. Dodanie pomocnika tagu `<label>` z `asp-for="Course.CourseID"` nie eliminuje potrzeby ukrycia pola. w przypadku, gdy użytkownik kliknie przycisk **Zapisz**, `<input type="hidden">` jest wymagany do uwzględnienia numeru kursu w opublikowanych danych.

## <a name="update-the-course-details-and-delete-pages"></a>Aktualizowanie szczegółów kursu i stron usuwania

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) może zwiększyć wydajność, gdy śledzenie nie jest wymagane.

### <a name="update-the-course-page-models"></a>Aktualizowanie modeli stron kursu

Zaktualizuj *strony/kursy/Delete. cshtml. cs* przy użyciu następującego kodu, aby dodać `AsNoTracking`:

[!code-csharp[](intro/samples/cu30/Pages/Courses/Delete.cshtml.cs?highlight=29)]

Wprowadź tę samą zmianę w pliku *Pages/kurss/details. cshtml. cs* :

[!code-csharp[](intro/samples/cu30/Pages/Courses/Details.cshtml.cs?highlight=28)]

### <a name="update-the-course-razor-pages"></a>Aktualizowanie stron Razor dla kursu

Zaktualizuj *strony/kursy/Delete. cshtml* przy użyciu następującego kodu:

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Delete.cshtml?highlight=15-20,37)]

Wprowadź te same zmiany na stronie Szczegóły.

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Details.cshtml?highlight=14-19,36)]

## <a name="test-the-course-pages"></a>Testowanie stron kursów

Przetestuj strony tworzenie, edytowanie, szczegóły i usuwanie.

## <a name="update-the-instructor-create-and-edit-pages"></a>Aktualizowanie strony instruktora tworzenie i edytowanie stron

Instruktorzy mogą uczyć się dowolnej liczby kursów. Na poniższej ilustracji przedstawiono stronę Edytowanie instruktora z tablicą pól wyboru kursu.

![Instruktor strony edytowania za pomocą kursów](update-related-data/_static/instructor-edit-courses30.png)

Pola wyboru umożliwiają zmianę kursów, do których zostanie przypisany instruktor. Pole wyboru jest wyświetlane dla każdego kursu w bazie danych. Wybrane są kursy, do których przypisano instruktora. Użytkownik może zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Jeśli liczba kursów była znacznie większa, może to poprawić inny interfejs użytkownika. Jednak metoda zarządzania relacją wiele-do-wielu pokazana tutaj nie zmieniła się. Aby utworzyć lub usunąć relacje, można manipulować jednostką sprzężenia.

### <a name="create-a-class-for-assigned-courses-data"></a>Utwórz klasę dla danych przypisanych kursów

Utwórz *SchoolViewModels/AssignedCourseData. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/AssignedCourseData.cs)]

Klasa `AssignedCourseData` zawiera dane służące do tworzenia pól wyboru dla kursów przypisanych do instruktora.

### <a name="create-an-instructor-page-model-base-class"></a>Utwórz klasę bazową modelu strony instruktora

Utwórz klasę bazową *stron/instruktorów/InstructorCoursesPageModel. cs* :

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_All)]

`InstructorCoursesPageModel` jest klasą bazową, która będzie używana dla modeli stron Edycja i tworzenie. `PopulateAssignedCourseData` odczytuje wszystkie jednostki `Course` w celu wypełnienia `AssignedCourseDataList`. Dla każdego kursu kod ustawia `CourseID`, tytuł i wskazuje, czy instruktor jest przypisany do kursu. [HashSet —](/dotnet/api/system.collections.generic.hashset-1) jest używany do wydajnego wyszukiwania.

Ze względu na to, że strona Razor nie zawiera kolekcji jednostek kursu, obiekt tworzący model nie może automatycznie aktualizować właściwości nawigacji `CourseAssignments`. Zamiast używać spinacza modelu do aktualizowania właściwości nawigacji `CourseAssignments`, należy to zrobić w nowej `UpdateInstructorCourses` metodzie. W związku z tym należy wykluczyć Właściwość `CourseAssignments` z powiązania modelu. Nie wymaga to żadnych zmian w kodzie, który wywołuje `TryUpdateModel`, ponieważ jest używane Przeciążenie listy dozwolonych i `CourseAssignments` nie znajduje się na liście dołączania.

Jeśli nie wybrano żadnych pól wyboru, kod w `UpdateInstructorCourses` inicjuje właściwość nawigacji `CourseAssignments` z pustą kolekcją i zwraca:

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_IfNull)]

Kod następnie przechodzi między wszystkimi kursami w bazie danych i sprawdza każdy kurs w odniesieniu do tych, które są aktualnie przypisane do instruktora, a także do tych, które zostały wybrane na stronie. Aby ułatwić efektywne wyszukiwanie, te dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli pole wyboru dla kursu zostało wybrane, ale kurs nie znajduje się w `Instructor.CourseAssignments` właściwości nawigacji, kurs zostanie dodany do kolekcji we właściwości nawigacji.

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_UpdateCourses)]

Jeśli nie wybrano pola wyboru dla kursu, ale kurs jest we właściwości nawigacji `Instructor.CourseAssignments`, kurs zostanie usunięty z właściwości nawigacji.

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_UpdateCoursesElse)]

### <a name="handle-office-location"></a>Obsługa lokalizacji biura

Inna relacja, którą Strona Edytuj musi obsłużyć, jest relacją "jeden do zera" lub "jeden", która jest jednostką instruktora z jednostką `OfficeAssignment`. Program instruktora edytuje kod musi obsługiwać następujące scenariusze: 

* Jeśli użytkownik wyczyści przypisanie pakietu Office, Usuń jednostkę `OfficeAssignment`.
* Jeśli użytkownik wprowadzi przypisanie do pakietu Office i jest puste, należy utworzyć nową jednostkę `OfficeAssignment`.
* Jeśli użytkownik zmieni przypisanie pakietu Office, zaktualizuj jednostkę `OfficeAssignment`.

### <a name="update-the-instructor-edit-page-model"></a>Aktualizowanie modelu strony przez instruktora

Aktualizowanie *stron/instruktorów/Edit. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Edit.cshtml.cs?name=snippet_All&highlight=9,28-32,38,42-77)]

Powyższy kod:

* Pobiera bieżącą jednostkę `Instructor` z bazy danych przy użyciu eager ładowania dla `OfficeAssignment`, `CourseAssignment`i `CourseAssignment.Course` właściwości nawigacji.
* Aktualizuje pobraną jednostkę `Instructor` przy użyciu wartości ze spinacza modelu. `TryUpdateModel` uniemożliwia [załadowanie](xref:data/ef-rp/crud#overposting).
* Jeśli lokalizacja biura jest pusta, ustawia `Instructor.OfficeAssignment` na wartość null. Gdy `Instructor.OfficeAssignment` ma wartość null, wiersz powiązane w tabeli `OfficeAssignment` zostanie usunięty.
* Wywołuje `PopulateAssignedCourseData` w `OnGetAsync`, aby podać informacje dla pól wyboru przy użyciu klasy modelu widoku `AssignedCourseData`.
* Wywołuje `UpdateInstructorCourses` w `OnPostAsync`, aby zastosować informacje z pól wyboru do edytowanej jednostki instruktora.
* Wywołuje `PopulateAssignedCourseData` i `UpdateInstructorCourses` w `OnPostAsync` Jeśli `TryUpdateModel` się nie powiedzie. Te wywołania metody powodują przywrócenie przypisanych danych kursu wprowadzonych na stronie, gdy jest on ponownie wyświetlany z komunikatem o błędzie.

### <a name="update-the-instructor-edit-razor-page"></a>Aktualizuj stronę "Edytuj Razor" instruktora

Aktualizowanie *stron/instruktorów/Edit. cshtml* przy użyciu następującego kodu:

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Edit.cshtml?highlight=29-59)]

Poprzedni kod tworzy tabelę HTML, która ma trzy kolumny. Każda kolumna ma pole wyboru i podpis zawierający numer i tytuł kursu. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"). Użycie tej samej nazwy informuje spinacz modelu, aby traktować go jako grupę. Atrybut value każdego pola wyboru jest ustawiony na `CourseID`. Po opublikowaniu strony spinacz modelu przekazuje tablicę, która składa się z wartości `CourseID` tylko dla wybranych pól wyboru.

Gdy pola wyboru są początkowo renderowane, wybierane są kursy przypisane do instruktora.

Uwaga: podejście podjęte tutaj do edytowania danych kursu instruktora działa dobrze, gdy istnieje ograniczona liczba kursów. W przypadku kolekcji, które są znacznie większe, inny interfejs użytkownika i inna metoda aktualizacji byłyby bardziej użyteczny i wydajny.

Uruchom aplikację i przetestuj zaktualizowaną stronę edycji instruktorów. Zmień niektóre przypisania kursu. Zmiany zostaną odzwierciedlone na stronie indeksu.

### <a name="update-the-instructor-create-page"></a>Aktualizowanie strony tworzenia instruktora

Zaktualizuj program instruktora Tworzenie modelu strony i strony Razor przy użyciu kodu podobnego do strony edytowania:

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Create.cshtml.cs)]

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Create.cshtml)]

Przetestuj stronę tworzenie instruktora.

## <a name="update-the-instructor-delete-page"></a>Aktualizuj stronę usuwania instruktora

Zaktualizuj *strony/instruktorów/Delete. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Delete.cshtml.cs?highlight=45-61)]

Poprzedni kod wprowadza następujące zmiany:

* Używa ładowania eager dla właściwości nawigacji `CourseAssignments`. `CourseAssignments` muszą być dołączone lub nie są usuwane po usunięciu instruktora. Aby uniknąć konieczności ich odczytywania, skonfiguruj kaskadowe usuwanie w bazie danych.

* Jeśli instruktor zostanie usunięty, zostanie przypisany jako administrator jakichkolwiek działów, program usunie przypisanie instruktora z tych urzędów.

Uruchom aplikację i Przetestuj stronę usuwania.

## <a name="next-steps"></a>Następne kroki

> [!div class="step-by-step"]
> [Poprzedni samouczek](xref:data/ef-rp/read-related-data)
> [następnego samouczka](xref:data/ef-rp/concurrency)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tym samouczku pokazano, jak aktualizować powiązane dane. Jeśli występują problemy, których nie można rozwiązać, [Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

Na poniższych ilustracjach przedstawiono niektóre z ukończonych stron.

![strony edycji kursu](update-related-data/_static/course-edit.png)
![stronie Edycja instruktora](update-related-data/_static/instructor-edit-courses.png)

Sprawdź i przetestuj strony Tworzenie i edytowanie kursu. Utwórz nowy kurs. Dział jest wybierany przez jego klucz podstawowy (liczba całkowita), a nie jego nazwę. Edytuj nowy kurs. Po zakończeniu testowania Usuń nowy kurs.

## <a name="create-a-base-class-to-share-common-code"></a>Utwórz klasę bazową, aby udostępnić wspólny kod

Wszystkie strony kursy/tworzenie i kursy/Edycja muszą mieć listę nazw działów. Utwórz klasę bazową *stron/kursów/DepartmentNamePageModel. cshtml. cs* dla stron tworzenia i edytowania:

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Poprzedni kod tworzy [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) , aby zawierał listę nazw działów. Jeśli `selectedDepartment` jest określony, ten dział jest wybierany w `SelectList`.

Klasy Utwórz i edytuj model strony będą pochodzić od `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Dostosowywanie stron kursów

Po utworzeniu nowej jednostki kursu musi ona mieć relację z istniejącym działem. Aby dodać dział podczas tworzenia kursu, Klasa bazowa do tworzenia i edycji zawiera listę rozwijaną umożliwiającą wybranie działu. Lista rozwijana ustawia właściwość `Course.DepartmentID` klucz obcy (FK). EF Core używa `Course.DepartmentID` FK do załadowania właściwości nawigacji `Department`.

![Utwórz kurs](update-related-data/_static/ddl.png)

Zaktualizuj model tworzenia strony przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

Powyższy kod:

* Pochodzi od `DepartmentNamePageModel`.
* Używa `TryUpdateModelAsync`, aby [zapobiec](xref:data/ef-rp/crud#overposting)przepisywaniu.
* Zastępuje `ViewData["DepartmentID"]` z `DepartmentNameSL` (z klasy bazowej).

`ViewData["DepartmentID"]` jest zastępowana przez silnie wpisaną `DepartmentNameSL`. Modele silnie wpisane są preferowane za pośrednictwem słabo wpisanych. Aby uzyskać więcej informacji, zobacz [słabo wpisane dane (ViewData i ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Aktualizowanie strony Tworzenie kursów

Zaktualizuj *strony/kursy/Utwórz. cshtml* przy użyciu następującego kodu:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Zmienia podpis z **DepartmentID** na **dział**.
* Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (z klasy bazowej).
* Dodaje opcję "Wybierz dział". Ta zmiana renderuje "Select Department" zamiast pierwszego działu.
* Dodaje komunikat weryfikacyjny, gdy nie wybrano działu.

Strona Razor używa [pomocnika wybierania tagu](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Przetestuj stronę tworzenie. Na stronie Tworzenie zostanie wyświetlona nazwa działu, a nie identyfikator działu.

### <a name="update-the-courses-edit-page"></a>Zaktualizuj stronę Edytowanie kursów.

Zastąp kod w obszarze *Pages/kursys/Edit. cshtml. cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

Zmiany są podobne do tych, które zostały wprowadzone w modelu tworzenia strony. W poprzednim kodzie `PopulateDepartmentsDropDownList` przekazuje identyfikator działu, który wybierze dział określony na liście rozwijanej.

Aktualizowanie *stron/kursów/Edit. cshtml* przy użyciu następującego znacznika:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Wyświetla identyfikator kursu. Zazwyczaj klucz podstawowy (PK) jednostki nie jest wyświetlany. PKs są zwykle oznaczane przez użytkowników. W tym przypadku klucz podstawowy jest numerem kursu.
* Zmienia podpis z **DepartmentID** na **dział**.
* Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (z klasy bazowej).

Ta strona zawiera pole ukryte (`<input type="hidden">`) dla numeru kursu. Dodanie pomocnika tagu `<label>` z `asp-for="Course.CourseID"` nie eliminuje potrzeby ukrycia pola. w przypadku, gdy użytkownik kliknie przycisk **Zapisz**, `<input type="hidden">` jest wymagany do uwzględnienia numeru kursu w opublikowanych danych.

Przetestuj zaktualizowany kod. Tworzenie, edytowanie i usuwanie kursu.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Dodawanie AsNoTracking do modeli szczegółów i stron usuwania

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) może zwiększyć wydajność, gdy śledzenie nie jest wymagane. Dodaj `AsNoTracking` do modelu strony usuwania i szczegółów. Poniższy kod przedstawia zaktualizowany model strony usuwania:

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Zaktualizuj metodę `OnGetAsync` w pliku *Pages/kurss/details. cshtml. cs* :

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modyfikowanie stron usuwania i szczegółów

Zaktualizuj stronę usuwania Razor za pomocą następującego znacznika:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Wprowadź te same zmiany na stronie Szczegóły.

### <a name="test-the-course-pages"></a>Testowanie stron kursów

Testowanie tworzenia, edytowania, szczegółów i usuwania.

## <a name="update-the-instructor-pages"></a>Aktualizowanie stron instruktora

W poniższych sekcjach zostały zaktualizowane strony instruktora.

### <a name="add-office-location"></a>Dodaj lokalizację biura

Podczas edytowania rekordu instruktora warto zaktualizować przypisanie biura instruktora. Jednostka `Instructor` ma relację "jeden do zera" lub jeden-do-jednego z jednostką `OfficeAssignment`. Kod instruktora musi obsłużyć:

* Jeśli użytkownik wyczyści przypisanie pakietu Office, Usuń jednostkę `OfficeAssignment`.
* Jeśli użytkownik wprowadzi przypisanie do pakietu Office i jest puste, należy utworzyć nową jednostkę `OfficeAssignment`.
* Jeśli użytkownik zmieni przypisanie pakietu Office, zaktualizuj jednostkę `OfficeAssignment`.

Zaktualizuj program instruktors Edytuj model strony przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

Powyższy kod:

* Pobiera bieżącą jednostkę `Instructor` z bazy danych przy użyciu eager ładowania dla właściwości nawigacji `OfficeAssignment`.
* Aktualizuje pobraną jednostkę `Instructor` przy użyciu wartości ze spinacza modelu. `TryUpdateModel` uniemożliwia [załadowanie](xref:data/ef-rp/crud#overposting).
* Jeśli lokalizacja biura jest pusta, ustawia `Instructor.OfficeAssignment` na wartość null. Gdy `Instructor.OfficeAssignment` ma wartość null, wiersz powiązane w tabeli `OfficeAssignment` zostanie usunięty.

### <a name="update-the-instructor-edit-page"></a>Aktualizowanie strony edytowania instruktora

Aktualizowanie *stron/instruktorów/Edit. cshtml* w lokalizacji biura:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Sprawdź, czy można zmienić lokalizację biura instruktorów.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Dodawanie przypisań kursu do strony edytowania instruktora

Instruktorzy mogą uczyć się dowolnej liczby kursów. W tej sekcji dodasz możliwość zmiany przypisań kursu. Na poniższej ilustracji przedstawiono zaktualizowaną stronę edycji instruktora:

![Instruktor strony edytowania za pomocą kursów](update-related-data/_static/instructor-edit-courses.png)

`Course` i `Instructor` ma relację wiele-do-wielu. Aby dodać i usunąć relacje, należy dodać i usunąć jednostki z zestawu jednostek `CourseAssignments` sprzężenia.

Pola wyboru umożliwiają zmianę kursów, do których zostanie przypisany instruktor. Pole wyboru jest wyświetlane dla każdego kursu w bazie danych. Kursy, do których jest przypisany instruktor, są sprawdzane. Użytkownik może zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Jeśli liczba kursów była znacznie większa:

* Prawdopodobnie używasz innego interfejsu użytkownika do wyświetlania kursów.
* Metoda manipulowania jednostką sprzężenia w celu tworzenia lub usuwania relacji nie ulegnie zmianie.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Dodaj klasy do obsługi tworzenia i edytowania stron instruktorów

Utwórz *SchoolViewModels/AssignedCourseData. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

Klasa `AssignedCourseData` zawiera dane, aby utworzyć pola wyboru dla przypisanych kursów przez instruktora.

Utwórz klasę bazową *stron/instruktorów/InstructorCoursesPageModel. cshtml. cs* :

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` jest klasą bazową, która będzie używana dla modeli stron Edycja i tworzenie. `PopulateAssignedCourseData` odczytuje wszystkie jednostki `Course` w celu wypełnienia `AssignedCourseDataList`. Dla każdego kursu kod ustawia `CourseID`, tytuł i wskazuje, czy instruktor jest przypisany do kursu. [HashSet —](/dotnet/api/system.collections.generic.hashset-1) jest używany do tworzenia wydajnych wyszukiwań.

### <a name="instructors-edit-page-model"></a>Instruktorzy edytują model strony

Zaktualizuj model strony instruktora do edycji przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

Poprzedni kod obsługuje zmiany przypisania pakietu Office.

Aktualizowanie widoku Razor dla instruktora:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Gdy wkleisz kod w programie Visual Studio, podziały wierszy są zmieniane w sposób, który przerywa kod. Naciśnij klawisze Ctrl + Z po raz, aby cofnąć automatyczne formatowanie. Kombinacja klawiszy Ctrl + Z naprawia podziały wierszy, aby wyglądały tak, jak widać w tym miejscu. Wcięcia nie muszą być doskonałe, ale `@:</tr><tr>`, `@:<td>`, `@:</td>`i `@:</tr>` muszą znajdować się w jednym wierszu, jak pokazano. Po wybraniu bloku nowego kodu naciśnij klawisz Tab trzy razy, aby wyrównać nowy kod z istniejącym kodem. Zagłosuj lub Sprawdź stan tej usterki [za pomocą tego linku](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Poprzedni kod tworzy tabelę HTML, która ma trzy kolumny. Każda kolumna ma pole wyboru i podpis zawierający numer i tytuł kursu. Wszystkie pola wyboru mają tę samą nazwę ("selectedCourses"). Użycie tej samej nazwy informuje spinacz modelu, aby traktować go jako grupę. Atrybut value każdego pola wyboru jest ustawiany na `CourseID`. Po opublikowaniu strony spinacz modelu przekazuje tablicę, która składa się z wartości `CourseID` tylko dla wybranych pól wyboru.

Gdy pola wyboru są początkowo renderowane, kursy przypisane do instruktora mają zaznaczone atrybuty.

Uruchom aplikację i przetestuj zaktualizowaną stronę edycji instruktorów. Zmień niektóre przypisania kursu. Zmiany zostaną odzwierciedlone na stronie indeksu.

Uwaga: podejście podjęte tutaj do edytowania danych kursu instruktora działa dobrze, gdy istnieje ograniczona liczba kursów. W przypadku kolekcji, które są znacznie większe, inny interfejs użytkownika i inna metoda aktualizacji byłyby bardziej użyteczny i wydajny.

### <a name="update-the-instructors-create-page"></a>Aktualizuj stronę tworzenia instruktorów

Zaktualizuj program instruktora Tworzenie modelu strony przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Poprzedni kod jest podobny do kodu *stron/instruktorów/Edit. cshtml. cs* .

Zaktualizuj program instruktora Utwórz stronę Razor z następującą adiustacją:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Przetestuj stronę tworzenie instruktora.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Aktualizowanie modelu strony Usuń z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

Poprzedni kod wprowadza następujące zmiany:

* Używa ładowania eager dla właściwości nawigacji `CourseAssignments`. `CourseAssignments` muszą być dołączone lub nie są usuwane po usunięciu instruktora. Aby uniknąć konieczności ich odczytywania, skonfiguruj kaskadowe usuwanie w bazie danych.

* Jeśli instruktor zostanie usunięty, zostanie przypisany jako administrator jakichkolwiek działów, program usunie przypisanie instruktora z tych urzędów.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja usługi YouTube w tym samouczku (część 1)](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [Wersja usługi YouTube w tym samouczku (część 2)](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/read-related-data)
> [dalej](xref:data/ef-rp/concurrency)

::: moniker-end
