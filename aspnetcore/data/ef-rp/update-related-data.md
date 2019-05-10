---
title: Strony razor z programem EF Core w programie ASP.NET Core — aktualizowanie powiązanych danych - 7, 8
author: rick-anderson
description: W tym samouczku będziesz aktualizowanie powiązanych danych, aktualizując pola kluczy obcych i właściwości nawigacji.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: b996e7f9c8e422060f6378a6004a53906e2af7c7
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087098"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>Strony razor z programem EF Core w programie ASP.NET Core — aktualizowanie powiązanych danych - 7, 8

Przez [Tom Dykstra](https://github.com/tdykstra), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Ten samouczek pokazuje, aktualizowanie powiązanych danych. Jeśli napotkasz problemy, nie można rozwiązać, [pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

Poniższa ilustracja przedstawia niektóre strony ukończone.

![Strona edytowania kurs](update-related-data/_static/course-edit.png)
![przez instruktorów edytowanie strony](update-related-data/_static/instructor-edit-courses.png)

Sprawdź, aby ją przetestować, tworzyć i edytować kursu. Tworzenie nowego kursu. Dział jest wybierany za pomocą klucza podstawowego (liczba całkowita), nie jego nazwę. Edytowanie nowego kursu. Po zakończeniu testowania Usuń nowego kursu.

## <a name="create-a-base-class-to-share-common-code"></a>Utwórz klasę bazową, aby udostępnić wspólny kod

Kursy/tworzenia kursy/edytowanie strony i każdy muszą listę nazw działów. Utwórz *Pages/Courses/DepartmentNamePageModel.cshtml.cs* klasa podstawowa dla stron tworzyć i edytować:

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Powyższy kod tworzy [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) zawierać listę nazw działów. Jeśli `selectedDepartment` określono wybrano działu `SelectList`.

Tworzenie i edytowanie klas modelu strony będzie dziedziczyć `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Dostosowywanie stron kursów

Po utworzeniu nowej jednostki kurs, musi on mieć relacji do istniejących działu. Dział podczas tworzenia kurs, klasa bazowa dla tworzyć i edytować zawiera listy rozwijanej, służąca do wybierania działu. Z listy rozwijanej zestawów `Course.DepartmentID` właściwości klucza obcego (klucz OBCY). Używa programu EF Core `Course.DepartmentID` klucza Obcego załadować `Department` właściwości nawigacji.

![Utwórz kurs](update-related-data/_static/ddl.png)

Aktualizowanie modelu strony Utwórz następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

Powyższy kod:

* Pochodzi od klasy `DepartmentNamePageModel`.
* Używa `TryUpdateModelAsync` zapobiegające [polegającymi](xref:data/ef-rp/crud#overposting).
* Zastępuje `ViewData["DepartmentID"]` z `DepartmentNameSL` (z klasy bazowej).

`ViewData["DepartmentID"]` zastępuje z silnie typizowaną `DepartmentNameSL`. Silnie typizowane modeli są preferowane przez słabo wpisane. Aby uzyskać więcej informacji, zobacz [słabo wpisanych danych (z podejścia ViewData i obiekt ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Aktualizacja kursy — Tworzenie strony

Aktualizacja *Pages/Courses/Create.cshtml* następującym kodem:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Zmienia podpis z **DepartmentID** do **działu**.
* Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (z klasy bazowej).
* Dodaje opcję "Wybierz dział". Ta zmiana powoduje wyświetlenie "Wybierz dział" zamiast pierwszy działu.
* Dodaje komunikat dotyczący sprawdzania poprawności, gdy dział nie jest wybrany.

Strona Razor używa [wybierz Pomocnik tagu](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Przetestuj strona tworzenia. Strona tworzenia Wyświetla nazwę działu, a nie identyfikatora działu.

### <a name="update-the-courses-edit-page"></a>Zaktualizuj strony Edytuj kursów.

Aktualizowanie modelu strony edycji z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

Zmiany są podobne do wprowadzonych w modelu strony Utwórz. W poprzednim kodzie `PopulateDepartmentsDropDownList` przebiegów w identyfikatorze działu, które dział określonych na liście rozwijanej.

Aktualizacja *Pages/Courses/Edit.cshtml* następującym kodem:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Wyświetla identyfikator kursu. Ogólnie nie jest wyświetlany podstawowego klucza (PK) z jednostki. PKs są zazwyczaj znaczenia dla użytkowników. W tym przypadku PK jest to liczba kursu.
* Zmienia podpis z **DepartmentID** do **działu**.
* Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (z klasy bazowej).

Ta strona zawiera pole ukryte (`<input type="hidden">`) dla liczby kursów. Dodawanie `<label>` tag Pomocnik ze `asp-for="Course.CourseID"` nie eliminuje potrzebę stosowania ukryte pole. `<input type="hidden">` jest wymagany dla numeru kurs, które mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **Zapisz**.

Przetestuj zaktualizowany kod. Tworzenie, edytowanie i usuwanie kursu.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Dodaj AsNoTracking do szczegółów i usuwania modeli strony

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) może poprawić wydajność, gdy śledzenie nie jest wymagane. Dodaj `AsNoTracking` z modelem strony Delete i szczegółowe informacje. Poniższy kod pokazuje aktualizowanego modelu strony usuwania:

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Aktualizacja `OnGetAsync` method in Class metoda *Pages/Courses/Details.cshtml.cs* pliku:

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modyfikowanie stron Delete i szczegóły

Zaktualizuj strony Razor Usuń następującym kodem:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Wprowadzenie identycznych zmian do strony szczegółów.

### <a name="test-the-course-pages"></a>Testowanie stron kursu

Test tworzenia, edytowania szczegółów i usuwania.

## <a name="update-the-instructor-pages"></a>Aktualizowanie stron przez instruktorów

Poniższe sekcje aktualizacji stron instruktora.

### <a name="add-office-location"></a>Dodaj lokalizację pakietu office

Podczas edytowania rekordu przez instruktorów, można zaktualizować przez instruktorów biuro. `Instructor` Jednostka ma relacji jeden do zero lub jeden z `OfficeAssignment` jednostki. Kod przez instruktorów musi obsługiwać:

* Jeśli użytkownik usunie zaznaczenie przypisania pakietu office, Usuń `OfficeAssignment` jednostki.
* Jeśli użytkownik wprowadzi biuro i był on pusty, Utwórz nowe `OfficeAssignment` jednostki.
* Jeśli użytkownik zmieni przypisania pakietu office, należy zaktualizować `OfficeAssignment` jednostki.

Aktualizowanie modelu strony edytowania Instruktorzy z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

Powyższy kod:

* Pobiera bieżący `Instructor` jednostki z bazy danych przy użyciu wczesne ładowanie dla `OfficeAssignment` właściwości nawigacji.
* Aktualizuje pobrany `Instructor` jednostki z wartościami z integratora modelu. `TryUpdateModel` Zapobiega [polegającymi](xref:data/ef-rp/crud#overposting).
* Jeśli lokalizacja pakietu office jest pusta, ustawia `Instructor.OfficeAssignment` na wartość null. Gdy `Instructor.OfficeAssignment` jest null, powiązane wiersza w `OfficeAssignment` tabeli zostanie usunięty.

### <a name="update-the-instructor-edit-page"></a>Zaktualizuj strony edytowania przez instruktorów

Aktualizacja *Pages/Instructors/Edit.cshtml* z lokalizacji biura:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Sprawdź, czy można zmienić lokalizacji biura instruktorów.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Dodaj kurs przypisania do strony edytowania przez instruktorów

Instruktorzy może nauczyć dowolnej liczby kursów. W tej sekcji dodasz możliwość zmiany przypisania kursu. Na poniższej ilustracji przedstawiono zaktualizowane przez instruktorów strony edytowania:

![Strona edytowania przez instruktorów dzięki kursom](update-related-data/_static/instructor-edit-courses.png)

`Course` i `Instructor` jest w relacji wiele do wielu. Aby dodawać i usuwać relacje, w przypadku dodawania i usuwania jednostek z `CourseAssignments` Dołącz do zestawu jednostek.

Pola wyboru Włącz zmiany do kursów, przypisanego pod kierunkiem instruktora. Pole wyboru jest wyświetlane dla każdego kursu w bazie danych. Kursy, przypisanych do instruktora są sprawdzane. Użytkownik może zaznacz lub wyczyść pola wyboru, aby zmienić przypisania kursu. Gdyby znacznie większą liczbę kursy:

* Interfejs inny użytkownik będzie prawdopodobnie umożliwia wyświetlenie kursy.
* W takich sytuacjach przydałaby zmienić metodę manipulowanie jednostki sprzężenia, można utworzyć ani usunąć relacji.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Dodawanie klasy do obsługi tworzenia i edycji stron przez instruktorów

Tworzenie *SchoolViewModels/AssignedCourseData.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` Klasa zawiera danych, aby utworzyć pól wyboru dla przypisanych kursy przez instruktora.

Tworzenie *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* klasa bazowa:

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` Jest klasą bazową, będzie używane do edycji i tworzyć modele strony. `PopulateAssignedCourseData` odczytuje wszystkie `Course` jednostek, aby wypełnić `AssignedCourseDataList`. Dla każdego kursu kod ustawia `CourseID`, tytuł i czy nie jest przypisany do kursu instruktora. A [hashset —](/dotnet/api/system.collections.generic.hashset-1) służy do tworzenia wyszukiwań wydajne.

### <a name="instructors-edit-page-model"></a>Instruktorzy edycji strony modelu

Aktualizowanie modelu strony edycji przez instruktorów, z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

Powyższy kod obsługuje zmiany przypisania pakietu office.

Aktualizacja instruktora widoku Razor:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Po wklejeniu kodu w programie Visual Studio podziały wierszy zostały zmienione w taki sposób, że przerwanie wykonywania kodu. Naciśnij klawisze Ctrl + Z jeden raz, aby cofnąć, automatycznego formatowania. CTRL + Z rozwiązuje podziałów wiersza, aby wyglądają jak tutaj zobaczyć. Wcięcie nie musi być idealna, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@:</tr>` linie muszą być w jednym wierszu jak pokazano. Za pomocą bloku wybrano nowego kodu naciśnij klawisz Tab, trzy razy na wiersz w górę nowego kodu przy użyciu istniejącego kodu. Sprawdź stan tej usterki lub Głosuj na [za pomocą tego łącza](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Powyższy kod tworzy tabelę HTML, który ma trzy kolumny. Każda kolumna ma postać pola wyboru i podpis zawierający kurs numer i tytuł. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"). Przy użyciu tej samej nazwie informuje integratora modelu je traktować jako grupa. Ma ustawioną wartość atrybutu wartości każdego pola wyboru `CourseID`. Po opublikowaniu strony, integratora modelu przekazuje tablicę, która składa się z `CourseID` wartości dla pola wyboru, które są wybrane.

Gdy pole wyboru początkowo są renderowane, kursy przypisane do instruktora sprawdzeniu atrybutów.

Uruchamianie aplikacji i przetestuj strony edytowania Instruktorzy zaktualizowane. Zmienić niektóre przypisania kursu. Zmiany zostaną odzwierciedlone na stronę indeksu.

Uwaga: Tutaj podejście do edycji przez instruktorów kurs danych działa dobrze w przypadku, gdy istnieje ograniczona liczba kursów. Kolekcje, które są znacznie większe innego interfejsu użytkownika i inną metodę aktualizacji będzie bardziej użyteczny i efektywny.

### <a name="update-the-instructors-create-page"></a>Strona tworzenia Instruktorzy aktualizacji

Aktualizowanie modelu strony Utwórz przez instruktorów, z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Powyższy kod jest podobny do *Pages/Instructors/Edit.cshtml.cs* kodu.

Zaktualizuj strony Razor tworzenia przez instruktorów następującym kodem:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Przetestuj strona tworzenia instruktora.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Aktualizowanie modelu strony Usuń z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

Poprzedzający kod wprowadza następujące zmiany:

* Używa wczesne ładowanie dla `CourseAssignments` właściwości nawigacji. `CourseAssignments` muszą być włączone lub nie zostały usunięte po usunięciu instruktora. Aby uniknąć konieczności je odczytać, należy skonfigurować usuwanie kaskadowe w bazie danych.

* Jeśli przez instruktorów do usunięcia jest przypisany jako administrator działy, usuwa przypisanie instruktora z tych działów.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja usługi YouTube w tym samouczku (część 1)](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [Wersja usługi YouTube w tym samouczku (część 2)](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/read-related-data)
> [dalej](xref:data/ef-rp/concurrency)
