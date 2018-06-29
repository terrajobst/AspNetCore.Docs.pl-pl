---
title: Dane — 7 10 dotyczące platformy ASP.NET Core MVC podstawowych EF - aktualizacji
author: rick-anderson
description: W tym samouczku będziesz aktualizacji powiązanych danych, aktualizując pól klucza obcego i właściwości nawigacji.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: ef8cb3916e5d1542e4d36cad694351462b94ed32
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093062"
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a>Dane — 7 10 dotyczące platformy ASP.NET Core MVC podstawowych EF - aktualizacji

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

W poprzednich instrukcji wyświetlane powiązanych danych; w tym samouczku będziesz aktualizacji powiązanych danych, aktualizując pól klucza obcego i właściwości nawigacji.

Na poniższych ilustracjach przedstawiono niektóre stron, którymi będzie współpracować.

![Kursu edycji strony](update-related-data/_static/course-edit.png)

![Instruktora edycji strony](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Dostosowywanie tworzenie i Edycja stron kursów

Po utworzeniu nowego obiektu kursu musi mieć relacji z istniejących działu. Aby to ułatwić, kod z utworzonym szkieletem obejmuje metod kontrolera oraz tworzenie i edytowanie widoków, które zawierają listy rozwijanej wyboru działu. Ustawia listy rozwijanej `Course.DepartmentID` właściwości klucza obcego, a to wszystko programu Entity Framework wymaga, aby załadować `Department` właściwości nawigacji o odpowiednich jednostek działu. Będziesz używać szkieletu kodu, ale nieco na dodawanie obsługi błędów i sortowanie listy rozwijanej zmienić.

W *CoursesController.cs*, usunąć czterech metod tworzenia i edycji i zastąpić je z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Po `Edit` metody HttpPost utworzenie nowej metody, który ładuje informacji działu dla listy rozwijanej.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList` Metoda pobiera listę wszystkich działach sortowane według nazwy, tworzy `SelectList` kolekcji do listy rozwijanej i przekazuje do widoku w kolekcji `ViewBag`. Metoda przyjmuje opcjonalny `selectedDepartment` parametr, który umożliwia kod wywołujący określić elementu, który będzie wybierany Po wyrenderowaniu listy rozwijanej. Widok przekazuje do nazwy "DepartmentID" `<select>` pomocnika tagów i pomocnika następnie traktował Szukaj w `ViewBag` obiekt do `SelectList` o nazwie "DepartmentID".

HttpGet `Create` wywołania metody `PopulateDepartmentsDropDownList` metody bez ustawienie wybranego elementu, ponieważ dla nowego kursu działu nie jest jeszcze ustalony:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

HttpGet `Edit` metoda ustawia wybrany element na podstawie Identyfikatora działu, który jest już przypisany do kursu edytowany:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Obie metody HttpPost `Create` i `Edit` również obejmować kod, który ustawia wybranego elementu, gdy ich ponownie wyświetlić stronę po wystąpieniu błędu. Dzięki temu, że po stronie zostanie wyświetlony ponownie, aby wyświetlić komunikat o błędzie, niezależnie od działu wybrano jest wybrane.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Dodaj. AsNoTracking do szczegółów i usuwania metod

Aby zoptymalizować wydajność, szczegóły dotyczące kursu i usuwanie stron, Dodaj `AsNoTracking` odwołuje się `Details` i HttpGet `Delete` metody.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Modyfikowanie widoków kursu

W *Views/Courses/Create.cshtml*, dodaj opcję "Wybierz dział" **działu** listy rozwijanej pozycję Zmienianie podpisu z **DepartmentID** do  **Dział**i Dodaj komunikat dotyczący sprawdzania poprawności.

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

W *Views/Courses/Edit.cshtml*, wprowadzić w polu działu, jak w *Create.cshtml*.

Również w *Views/Courses/Edit.cshtml*, Dodaj pole Liczba kursu przed **tytuł** pola. Ponieważ numer kursu jest to klucz podstawowy, jest on wyświetlany, ale nie można zmienić.

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Istnieje już ukryte pole (`<input type="hidden">`) numeru kursu w widoku edycji. Dodawanie `<label>` pomocnika tagów nie eliminuje potrzebę stosowania ukryte pola, ponieważ nie powoduje numer kursu mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **zapisać** na **Edytuj** strony.

W *Views/Courses/Delete.cshtml*, Dodaj pole Liczba kursu u góry i Zmień nazwę Dział dział identyfikator.

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

W *Views/Courses/Details.cshtml*, wprowadzić zmiany sam, jak w *Delete.cshtml*.

### <a name="test-the-course-pages"></a>Kursu stron w teście

Uruchom aplikację, wybierz **kursów** , kliknij pozycję **Utwórz nowy**i wprowadź dane dla porach nowe:

![Plan tworzenia strony](update-related-data/_static/course-create.png)

Kliknij przycisk **Utwórz**. Z nowego kursu ma na liście zostanie wyświetlona strona kursy indeksu. Nazwa działu na liście strony indeksu pochodzi z właściwości nawigacji, przedstawiający został poprawnie ustanowić relacji.

Kliknij przycisk **Edytuj** na kursu na stronie kursy indeksu.

![Kursu edycji strony](update-related-data/_static/course-edit.png)

Zmień dane na stronie, a następnie kliknij przycisk **zapisać**. Z danymi zaktualizowanych kursów zostanie wyświetlona strona kursy indeksu.

## <a name="add-an-edit-page-for-instructors"></a>Dodaj stronę edycji dla instruktorów

Podczas edytowania rekordu instruktora chcesz była możliwa aktualizacja instruktora office przypisania. Jednostka Instructor ma relacji jeden do zero lub jeden z jednostką OfficeAssignment, co oznacza, że ma swój kod obsługi w następujących sytuacjach:

* Jeśli pierwotnie miał wartość wyczyszczenie przypisania pakietu office, usunąć jednostkę OfficeAssignment.

* Jeśli użytkownik wprowadzi wartość przydziału pakietu office i początkowo był pusty, Utwórz nową jednostkę OfficeAssignment.

* Jeśli użytkownik zmieni wartość przydziału pakietu office, zmień wartość w istniejącej jednostki OfficeAssignment.

### <a name="update-the-instructors-controller"></a>Aktualizacja kontrolera instruktorów

W *InstructorsController.cs*, Zmień kod w HttpGet `Edit` metodę, tak aby ładuje Jednostka Instructor `OfficeAssignment` właściwość nawigacji i wywołania `AsNoTracking`:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

Zastąp HttpPost `Edit` metody z następujący kod, aby obsługiwać aktualizacje przypisania pakietu office:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

Kod wykonuje następujące czynności:

-  Zmienia nazwę metody, aby `EditPost` ponieważ podpis teraz jest taka sama jak HttpGet `Edit` — metoda ( `ActionName` atrybut określa, że `/Edit/` nadal używany jest adres URL).

-  Pobiera wczesny ładowania dla bieżącego obiektu instruktora z bazy danych przy użyciu `OfficeAssignment` właściwości nawigacji. To jest identyczny jak w HttpGet `Edit` metody.

-  Aktualizuje pobraną jednostkę instruktora wartościami z integratora modelu. `TryUpdateModel` Przeciążenie umożliwia utworzenie listy dozwolonych właściwości, które chcesz dołączyć. Pozwala to uniknąć nadmiernego publikowanie zgodnie z objaśnieniem w [drugi samouczek](crud.md).

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

-   Jeśli w lokalizacji biura jest puste, ustawia właściwość Instructor.OfficeAssignment równej null, dzięki czemu powiązanego wiersza w tabeli OfficeAssignment zostaną usunięte.

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Zapisuje zmiany w bazie danych.

### <a name="update-the-instructor-edit-view"></a>Aktualizacja instruktora Edytuj widok

W *Views/Instructors/Edit.cshtml*, Dodaj nowe pole do edycji lokalizacji biura, przed upływem **zapisać** przycisk:

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Uruchom aplikację, wybierz **instruktorów** , a następnie kliknij pozycję **Edytuj** na instruktora. Zmień **oddział** i kliknij przycisk **zapisać**.

![Instruktora edycji strony](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Na stronie edycji instruktora Dodaj kursu przypisania

Instruktorów może nauczyć dowolną liczbę kursów. Teraz będzie zwiększenia strony Edytuj instruktora przez dodanie możliwości zmiany przypisania kursu za pomocą grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:

![Instruktora edycji strony z kursów](update-related-data/_static/instructor-edit-courses.png)

Relacja między jednostkami przebiegu i instruktora jest wiele do wielu. Aby dodać lub usunąć relacji, dodawania i usuwania jednostek z zestawu jednostek CourseAssignments sprzężenia i.

Interfejs użytkownika, który umożliwia zmianę które kursy instruktora jest przypisane do to grupa pól wyboru. Wyświetlane jest pole wyboru dla porach co w bazie danych, a te, które instruktora jest aktualnie przypisana do są zaznaczone. Użytkownika można zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Gdyby większa liczba kursów będzie prawdopodobnie chcesz użyć innej metody prezentacji danych w widoku, ale sama metoda manipulowania jednostki sprzężenia użyje do tworzenia lub usuwania relacji.

### <a name="update-the-instructors-controller"></a>Aktualizacja kontrolera instruktorów

Aby zapewnić dane do widoku listy pól wyboru, użyjesz klasy modelu widoku.

Utwórz *AssignedCourseData.cs* w *SchoolViewModels* folderu i Zamień istniejący kod następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

W *InstructorsController.cs*, Zastąp HttpGet `Edit` metodę z następującym kodem. Zmiany zostały wyróżnione.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

Ten kod dodaje wczesny ładowania dla `Courses` właściwość nawigacji i wywołuje nowe `PopulateAssignedCourseData` metody, aby podać informacje dotyczące używania tablicy pole wyboru `AssignedCourseData` wyświetlić klasy modelu.

Kod w `PopulateAssignedCourseData` — metoda odczytuje za pośrednictwem wszystkich jednostek kursu, aby załadować listę kursów przy użyciu klasy modelu widoku. Dla każdego kursu kodu sprawdza, czy porach występuje w instruktora `Courses` właściwości nawigacji. Aby utworzyć wydajne wyszukiwanie podczas sprawdzania, czy kursu jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w `HashSet` kolekcji. `Assigned` Właściwość jest ustawiona na wartość true dla kursów instruktora jest przypisany do. Widok użyje tej właściwości w celu określenia, które wyboru pola musi być wyświetlane jako wybrane. Na koniec listy jest przekazywany do widoku w `ViewData`.

Następnie dodaj kod, który jest wykonywany, gdy użytkownik kliknie **zapisać**. Zastąp `EditPost` metodę z następującym kodem, a następnie dodaj nową metodę, która aktualizuje `Courses` właściwości nawigacji jednostki instruktora.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

Podpis metody teraz różni się od HttpGet `Edit` metody, więc zmienia nazwę metody z `EditPost` do `Edit`.

Ponieważ widok nie zawiera kolekcji jednostek kursu, integratora modelu nie można automatycznie zaktualizować `CourseAssignments` właściwości nawigacji. Zamiast integratora modelu do zaktualizowania `CourseAssignments` właściwość nawigacji, można to zrobić w nowym `UpdateInstructorCourses` metody. Dlatego należy wyłączyć `CourseAssignments` właściwości z powiązania modelu. To nie wymaga żadnych zmian do kodu, który wywołuje `TryUpdateModel` ponieważ używasz przeciążenia listę dozwolonych podobnej i `CourseAssignments` nie ma na liście include.

Jeśli nie zostały zaznaczone pola wyboru kod w `UpdateInstructorCourses` inicjuje `CourseAssignments` właściwości nawigacji o pustej kolekcji i zwraca:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

Kod, a następnie przetwarza w pętli wszystkich kursów w bazie danych i sprawdza każdego kursu względem nich aktualnie przypisane do instruktora od komputerów, które wybrano w widoku. W celu ułatwienia wyszukiwania wydajne, ostatnie dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli zaznaczono pole wyboru dla porach, ale kursu nie znajduje się w `Instructor.CourseAssignments` właściwości nawigacji kursu zostanie dodany do kolekcji we właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Jeśli nie zostało zaznaczone pole wyboru dla porach, ale znajduje się w trakcie `Instructor.CourseAssignments` właściwość nawigacji, kursu zostanie usunięta z właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Aktualizowanie widoków instruktora

W *Views/Instructors/Edit.cshtml*, Dodaj **kursy** pole z tablicą pola wyboru przez dodanie poniższego kodu bezpośrednio po `div` elementy **pakietu Office**  pola i przed `div` elementu **zapisać** przycisku.

<a id="notepad"></a>
> [!NOTE]
> Po wklejeniu kodu w programie Visual Studio, podziały wiersza zostanie zmieniony w taki sposób, który dzieli kod.  Naciśnij klawisze Ctrl + Z jeden raz, aby cofnąć automatycznego formatowania.  Naprawi podziały wiersza, aby wyglądają tu wyświetlić. Wcięcie nie musi być idealne, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@:</tr>` linie muszą być w jednym wierszu pokazany lub zostanie wyświetlony błąd w czasie wykonywania. Z bloku nowy kod zaznaczone naciśnij klawisz Tab trzykrotnie Aby wyrównać nowy kod z istniejącym kodem. Stan tego problemu można sprawdzić [tutaj](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Ten kod tworzy tabelę HTML, który zawiera trzy kolumny. W każdej kolumnie ma postać pola wyboru, a po nim tekstem, który składa się z kursu numer i tytuł. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), które informuje integrator modelu o tym, że są one traktowane jako grupa. Atrybut wartość każdego pola wyboru jest ustawiony na wartość `CourseID`. Gdy strona jest przesyłana, integratora modelu przekazuje tablicy do kontrolera, który składa się z `CourseID` wartości dla pola wyboru, które zostały wybrane.

Początkowo renderowany na pola wyboru, te, które są przypisane do instruktora kursów zaznaczono atrybuty, które wybiera je (wyświetlanych jest zaewidencjonowania je).

Uruchom aplikację, wybierz **instruktorów** , a następnie kliknij pozycję **Edytuj** na instruktora, aby wyświetlić **Edytuj** strony.

![Instruktora edycji strony z kursów](update-related-data/_static/instructor-edit-courses.png)

Zmień przypisania pewnych porach i kliknij przycisk Zapisz. Wprowadzone zmiany zostaną odzwierciedlone na stronie indeksu.

> [!NOTE]
> Podejście przyjęte tutaj, aby edytować dane kursu instruktora działa również w przypadku, gdy istnieje ograniczona liczba kursów. Kolekcje, które są znacznie większe innego interfejsu użytkownika i różne metody aktualizacji będą wymagane.

## <a name="update-the-delete-page"></a>Zaktualizuj strony usuwania

W *InstructorsController.cs*, Usuń `DeleteConfirmed` — metoda i wstaw poniższy kod w jego miejscu.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Ten kod wprowadza następujące zmiany:

* Eager czy ładowanie dla `CourseAssignments` właściwości nawigacji.  Należy to uwzględnić lub EF nie będzie wiedzieć o powiązanych `CourseAssignment` jednostek i nie będzie je usunąć.  Aby uniknąć konieczności je odczytać w tym miejscu możesz może skonfigurować usuwanie kaskadowe w bazie danych.

* Jeśli instruktora do usunięcia jest przypisany jako administrator działy, usuwa przypisanie instruktora z tych działów.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Dodaj lokalizację pakietu office i kursy do tworzenia strony

W *InstructorsController.cs*, Usuń HttpGet i HttpPost `Create` metod, a następnie dodaj następujący kod w ich miejscu:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Ten kod jest podobna do instrukcji dotyczących `Edit` metody z wyjątkiem to początkowo nie kursy są wybrane. HttpGet `Create` wywołania metody `PopulateAssignedCourseData` — metoda nie może być kursy wybrane, ale w celu Podaj pustą kolekcję dla `foreach` pętli w widoku (w przeciwnym razie kod widoku spowoduje zgłoszenie wyjątku odwołanie o wartości null).

HttpPost `Create` metoda dodaje każdego wybranego kursu do `CourseAssignments` właściwość nawigacji przed sprawdza błędy weryfikacji i dodaje nowe instruktora do bazy danych. Kursy są dodawane, nawet jeśli istnieją błędy modelu tak, aby po występują błędy modelu (na przykład użytkownik z kluczem usługi nieprawidłową datę), a strona jest ponownie wyświetlony komunikat o błędzie, wprowadzone wybrane opcje kursu automatycznie zostaną przywrócone.

Zwróć uwagę, że aby można było dodać kursy do `CourseAssignments` właściwość nawigacji, należy zainicjować właściwość jako pustej kolekcji:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Alternatywą wobec tej czynności w kodzie kontrolera nakładu pracy w modelu instruktora zmieniając metody pobierającej właściwości do automatycznego tworzenia kolekcji, jeśli nie istnieje, jak pokazano w poniższym przykładzie:

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

Jeśli zmodyfikujesz `CourseAssignments` właściwości w ten sposób można usunąć kod inicjujący jawną właściwość w kontrolerze.

W *Views/Instructor/Create.cshtml*, Dodaj pole tekstowe lokalizacji pakietu office i sprawdź pola kursów przed przycisku Prześlij. Jak w przypadku edycji strony [Popraw formatowanie, jeśli program Visual Studio formatuje kod po wklejeniu go](#notepad).

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Przetestuj aplikację i tworzenie instruktora.

## <a name="handling-transactions"></a>Obsługa transakcji

Zgodnie z objaśnieniem w [samouczek CRUD](crud.md), Entity Framework niejawnie implementuje transakcji. Scenariusze, w którym należy więcej kontrolujesz — na przykład, jeśli chcesz dołączyć operacje wykonywane poza Entity Framework w transakcji — można znaleźć [transakcji](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="summary"></a>Podsumowanie

Wprowadzenie do pracy z powiązanych danych zostało zakończone. W następnym samouczku zobaczysz sposób obsługi konfliktom współbieżności.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](read-related-data.md)
> [dalej](concurrency.md)
