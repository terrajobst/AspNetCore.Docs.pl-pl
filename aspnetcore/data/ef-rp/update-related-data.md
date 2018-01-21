---
title: "Stron razor podstawowych EF - aktualizacji powiązanych danych - 7, 8"
author: rick-anderson
description: "W tym samouczku będziesz aktualizacji powiązanych danych, aktualizując pól klucza obcego i właściwości nawigacji."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 817bfd48dce94e7dbad96cb6f822494e3adfae1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>Aktualizowanie danych powiązanych - stron Razor EF Core (7, 8)

Przez [Dykstra Tomasz](https://github.com/tdykstra), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Ten samouczek pokazuje, aktualizowanie powiązanych danych. Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

Poniższych ilustracjach przedstawiono niektóre ukończone stron.

![Kursu edycji strony](update-related-data/_static/course-edit.png)
![instruktora edytowanie strony](update-related-data/_static/instructor-edit-courses.png)

Sprawdź i przetestować, tworzenie i edytowanie ciągu. Utwórz nowy plan. Dział został wybrany za pomocą klucza podstawowego (liczba całkowita), nie jego nazwy. Edytuj nowego kursu. Po zakończeniu testowania Usuń nowego kursu.

## <a name="create-a-base-class-to-share-common-code"></a>Utwórz klasę podstawową, aby udostępnić typowy kod

Kursy/Utwórz kursy i edytowanie strony i każdego muszą listę nazw działów. Utwórz *Pages/Courses/DepartmentNamePageModel.cshtml.cs* klasę podstawową dla tworzenie i edytowanie strony:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Poprzedni kod tworzy [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) zawierać listę nazw działu. Jeśli `selectedDepartment` określono, że dział został wybrany w `SelectList`.

Klasy modeli tworzenie i edytowanie strony będzie dziedziczyć `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Dostosowywanie stron kursy

Po utworzeniu nowego obiektu kursu musi mieć relacji z istniejących działu. Dział podczas tworzenia kursu, klasę podstawową dla tworzenie i edytowanie zawiera listy rozwijanej wyboru działu. Ustawia listy rozwijanej `Course.DepartmentID` właściwości klucza obcego (klucz OBCY). Używa EF Core `Course.DepartmentID` klucz OBCY załadować `Department` właściwości nawigacji.

![Utwórz plan](update-related-data/_static/ddl.png)

Aktualizacja modelu strony Utwórz następującym kodem:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

Poprzedni kod:

* Pochodną `DepartmentNamePageModel`.
* Używa `TryUpdateModelAsync` zapobiegające [overposting](xref:data/ef-rp/crud#overposting).
* Zastępuje `ViewData["DepartmentID"]` z `DepartmentNameSL` (od klasy podstawowej).

`ViewData["DepartmentID"]`zastępuje z silnie typizowaną `DepartmentNameSL`. Modele jednoznacznie są preferowane za pośrednictwem lekko wpisany. Aby uzyskać więcej informacji, zobacz [lekko typu danych (ViewData i obiekt ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Zaktualizuj strony tworzenia kursów

Aktualizacja *Pages/Courses/Create.cshtml* z następujący kod:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Zmienia podpisu z **DepartmentID** do **działu**.
* Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (od klasy podstawowej).
* Dodaje opcję "Wybierz dział". Ta zmiana powoduje "Wybierz dział" zamiast pierwszy działu.
* Dodaje komunikat dotyczący sprawdzania poprawności, gdy nie wybrano działu.

Strona Razor używa [wybierz pomocnika tagów](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Przetestuj tworzenia strony. Utwórz stronę Wyświetla nazwę działu zamiast identyfikatora działu.

### <a name="update-the-courses-edit-page"></a>Zaktualizuj strony Edytuj kursy.

Aktualizacja modelu strony Edytuj następującym kodem:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

Zmiany są podobne do tych wprowadzone w modelu strony Utwórz. W powyższym kodzie `PopulateDepartmentsDropDownList` przebiegów w identyfikatorze działu, które dział określonych na liście rozwijanej.

Aktualizacja *Pages/Courses/Edit.cshtml* z następujący kod:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Wyświetla identyfikator kursu. Ogólnie rzecz biorąc podstawowego klucza (PK) z jednostki nie jest wyświetlana. PKs są zazwyczaj bezużyteczne dla użytkowników. W takim przypadku PK jest to liczba kursu.
* Zmienia podpisu z **DepartmentID** do **działu**.
* Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (od klasy podstawowej).
* Dodaje opcję "Wybierz dział". Ta zmiana powoduje "Wybierz dział" zamiast pierwszy działu.
* Dodaje komunikat dotyczący sprawdzania poprawności, gdy nie wybrano działu.

Strona zawiera pola ukrytego (`<input type="hidden">`) dla porach numeru. Dodawanie `<label>` pomocnika za pomocą tagów `asp-for="Course.CourseID"` nie eliminuje potrzebę stosowania ukryte pole. `<input type="hidden">`jest wymagany dla numeru kursu mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **zapisać**.

Przetestuj zaktualizowanego kodu. Tworzenie, edytowanie i usuwanie kursu.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Dodaj AsNoTracking do szczegółów i usuń modele strony

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) może poprawić wydajność podczas śledzenia nie jest wymagana. Dodaj `AsNoTracking` do modelu strony Delete i szczegóły. Poniższy kod przedstawia w zaktualizowanym modelu. Usuń strony:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Aktualizacja `OnGetAsync` metody w *Pages/Courses/Details.cshtml.cs* pliku:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modyfikowanie stron Delete i szczegóły

Zaktualizuj strony Usuń Razor o następujący kod:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Wprowadzenie identycznych zmian do strony szczegółów.

### <a name="test-the-course-pages"></a>Kursu stron w teście

Test tworzenia, edytowania, uzyskać szczegółowe informacje i usuwania.

## <a name="update-the-instructor-pages"></a>Aktualizowanie stron instruktora

Poniższe sekcje aktualizacji instruktora stron.

### <a name="add-office-location"></a>Dodaj lokalizację pakietu office

Edytowanie rekordu instruktora, można zaktualizować przypisania office instruktora. `Instructor` Jednostka ma relacji jeden do zero lub jeden z `OfficeAssignment` jednostki. Kod instruktora musi obsługiwać:

* Jeśli użytkownik usuwa przypisanie pakietu office, Usuń `OfficeAssignment` jednostki.
* Jeśli użytkownik wprowadzi przypisania pakietu office i był on pusty, Utwórz nową `OfficeAssignment` jednostki.
* Jeśli użytkownik zmieni przypisania pakietu office, należy zaktualizować `OfficeAssignment` jednostki.

Aktualizacja modelu strony Edytuj instruktorów następującym kodem:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

Poprzedni kod:

- Pobiera bieżący `Instructor` jednostki z bazy danych przy użyciu wczesny ładowania dla `OfficeAssignment` właściwości nawigacji.
- Aktualizuje pobranej `Instructor` jednostki wartościami z integratora modelu. `TryUpdateModel`Zapobiega [overposting](xref:data/ef-rp/crud#overposting).
- Jeśli w lokalizacji biura jest puste, ustawia `Instructor.OfficeAssignment` na wartość null. Gdy `Instructor.OfficeAssignment` jest null, powiązane wiersza w `OfficeAssignment` usunięcia tabeli.

### <a name="update-the-instructor-edit-page"></a>Zaktualizuj instruktora edycji strony

Aktualizacja *Pages/Instructors/Edit.cshtml* z lokalizacji pakietu office:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Sprawdź, czy można zmienić lokalizacji biura instruktorów.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Na stronie edycji instruktora Dodaj kursu przypisania

Instruktorów może nauczyć dowolną liczbę kursów. W tej sekcji możesz dodać możliwości zmiany przypisania kursu. Na poniższej ilustracji przedstawiono zaktualizowane instruktora edycji strony:

![Instruktora edycji strony z kursów](update-related-data/_static/instructor-edit-courses.png)

`Course`i `Instructor` ma relację wiele do wielu. Aby dodać lub usunąć relacji, dodawania i usuwania jednostek z `CourseAssignments` Dołącz do zestawu jednostek.

Pola wyboru Włącz zmiany kursów przydzielonej instruktora. Pole wyboru jest wyświetlane dla porach co w bazie danych. Kursy przypisane instruktora są sprawdzane. Użytkownika można zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Jeśli liczba kursów była większa:

* Interfejs inny użytkownik użyje prawdopodobnie do wyświetlenia kursów.
* Metoda manipulowania jednostki sprzężenia, można utworzyć ani usunąć relacji nie ulegnie zmianie.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Dodawanie klasy do obsługi tworzyć i edytować instruktora strony

Utwórz *SchoolViewModels/AssignedCourseData.cs* następującym kodem:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` Klasa zawiera danych, aby utworzyć pól wyboru dla przypisanych kursy przez instruktora.

Utwórz *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* klasy podstawowej:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` Jest klasą podstawową będzie używane do edycji i tworzenia modeli strony. `PopulateAssignedCourseData`odczytuje wszystkie `Course` jednostek do wypełnienia `AssignedCourseDataList`. Dla każdego kursu kod ustawia `CourseID`, tytuł i czy nie jest przypisany do kursu instruktora. A [zestaw HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) służy do tworzenia wydajne wyszukiwań.

### <a name="instructors-edit-page-model"></a>Instruktorów edycji strony modelu

Aktualizacja modelu strony Edytuj instruktora następującym kodem:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

Poprzedni kod obsługi zmian w przypisaniu pakietu office.

Aktualizacja instruktora widoku Razor:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Po wklejeniu kodu w programie Visual Studio, podziały wiersza są zmieniane w taki sposób, który dzieli kod. Naciśnij klawisze Ctrl + Z jeden raz, aby cofnąć automatycznego formatowania. CTRL + Z rozwiązuje podziały wierszy, dzięki czemu wyglądają tu wyświetlić. Wcięcie nie musi być idealne, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@:</tr>` linie muszą być w jednym wierszu jak pokazano. Z bloku nowy kod zaznaczone naciśnij klawisz Tab trzykrotnie Aby wyrównać nowy kod z istniejącym kodem. Głosowania lub sprawdzić stan tej usterki [z tym łączem](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Poprzedni kod tworzy tabelę HTML, który zawiera trzy kolumny. Każda kolumna ma postać pola wyboru i podpis zawierający kursu numer i tytuł. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"). Przy użyciu tej samej nazwie informuje integratora modelu je traktować jako grupa. Ustawiono atrybut wartość każdego pola wyboru `CourseID`. Gdy strona jest przesyłana, integratora modelu przekazuje tablicę, która składa się z `CourseID` wartości dla pola wyboru, które są wybrane.

Gdy pola wyboru początkowo są renderowane, kursy przypisane do instruktora zaznaczono atrybutów.

Uruchom aplikację i przetestować zaktualizowane instruktorów edycji strony. Zmiana przypisania niektórych kursu. Zmiany zostaną odzwierciedlone na stronie indeksu.

Uwaga: w tym miejscu podejście do edytowania danych kursu instruktora dobrze działa w przypadku istnieje ograniczona liczba kursów. Kolekcje, które są znacznie większe innego interfejsu użytkownika i różne metody aktualizacji będzie bardziej użyteczny i efektywny.

### <a name="update-the-instructors-create-page"></a>Aktualizacja instruktorów tworzenia strony

Aktualizacja modelu strony Utwórz instruktora następującym kodem:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Poprzedni kod jest podobny do *Pages/Instructors/Edit.cshtml.cs* kodu.

Zaktualizuj strony tworzenia Razor instruktora o następujący kod:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Przetestuj instruktora tworzenia strony.

## <a name="update-the-delete-page"></a>Zaktualizuj strony usuwania

Aktualizacja modelu strony Usuń z następującym kodem:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

Poprzedni kod wprowadza następujące zmiany:

* Używa wczesny ładowania dla `CourseAssignments` właściwości nawigacji. `CourseAssignments`muszą być włączone lub nie są one usuwane po usunięciu instruktora. Aby uniknąć konieczności je odczytać, należy skonfigurować usuwanie kaskadowe w bazie danych.

* Jeśli instruktora do usunięcia jest przypisany jako administrator działy, usuwa przypisanie instruktora z tych działów.

>[!div class="step-by-step"]
[Poprzednie](xref:data/ef-rp/read-related-data)
[dalej](xref:data/ef-rp/concurrency)
