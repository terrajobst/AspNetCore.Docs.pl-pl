---
title: 'Samouczek: Aktualizowanie powiązanych danych — ASP.NET MVC z programem EF Core'
description: W tym samouczku będziesz aktualizowanie powiązanych danych, aktualizując pola kluczy obcych i właściwości nawigacji.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/05/2019
ms.topic: tutorial
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: ac94f2e2876c2d8d571a451e4641787ffe37b3d2
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56103036"
---
# <a name="tutorial-update-related-data---aspnet-mvc-with-ef-core"></a>Samouczek: Aktualizowanie powiązanych danych — ASP.NET MVC z programem EF Core

W poprzednim samouczku wyświetlane powiązanych danych; w tym samouczku będziesz aktualizowanie powiązanych danych, aktualizując pola kluczy obcych i właściwości nawigacji.

Na poniższych ilustracjach przedstawiono niektóre stron którymi będziesz pracować.

![Strona edytowania kursu](update-related-data/_static/course-edit.png)

![Strona edytowania przez instruktorów](update-related-data/_static/instructor-edit-courses.png)

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dostosowywanie stron kursów
> * Dodaj stronę Edytuj instruktorów
> * Dodaj kursy na stronie edycji
> * Strona usuwanie aktualizacji
> * Dodawanie lokalizacji biura i kursy do tworzenia strony

## <a name="prerequisites"></a>Wymagania wstępne

* [Odczytywanie powiązanych danych z programem EF Core dla aplikacji internetowej ASP.NET Core MVC](read-related-data.md)

## <a name="customize-courses-pages"></a>Dostosowywanie stron kursów

Po utworzeniu nowej jednostki kurs, musi on mieć relacji do istniejących działu. Aby ułatwić to zadanie, utworzony szkielet kodu zawiera metod kontrolera oraz tworzyć i edytować widoki, które zawierają listy rozwijanej, służąca do wybierania działu. Z listy rozwijanej zestawów `Course.DepartmentID` właściwości klucza obcego, i to wszystko Entity Framework wymaga, aby mogła załadować `Department` właściwość nawigacji z odpowiednią jednostką działu. Będziesz używać utworzony szkielet kodu, ale Zmień ją nieco na dodawanie obsługi błędów i sortowanie listy rozwijanej.

W *CoursesController.cs*, Usuń cztery metody tworzyć i edytować i zastąpić je z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Po `Edit` metody HttpPost, utworzenie nowej metody, która ładuje informacji działu dla listy rozwijanej.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList` Metoda pobiera listę wszystkich działach sortowane według nazwy, tworzy `SelectList` kolekcji dla listy rozwijanej, a następnie przekazuje kolekcji do widoku `ViewBag`. Metoda przyjmuje opcjonalną `selectedDepartment` parametr, który umożliwia kod wywołujący, aby określić element, który zostanie wybrany podczas renderowania listy rozwijanej. Widok będzie przekazywać nazwę jednostki "DepartmentID" do `<select>` następnie wie, Pomocnik tagu i pomocnika do przeszukania `ViewBag` dla obiektu `SelectList` o nazwie "DepartmentID".

Narzędzia HttpGet `Create` wywołania metody `PopulateDepartmentsDropDownList` metody bez ustawienia wybranego elementu, ponieważ nowego kursu działu nie jest jeszcze ustalony:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

Narzędzia HttpGet `Edit` metoda ustawia wybranego elementu, na podstawie Identyfikatora działu, który jest już przypisana do kursu edytowanym:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Obie metody HttpPost `Create` i `Edit` również dołączyć kod, który ustawia wybranego elementu, gdy ich ponownego wyświetlenia strony po wystąpieniu błędu. Daje to gwarancję, że po stronie zostanie wyświetlony ponownie, aby wyświetlić komunikat o błędzie, niezależnie od działu wybrano pozostaje wybrane.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Dodaj. AsNoTracking do szczegółów i usuwania metod

Aby zoptymalizować wydajność kurs szczegółów i usuwania stron, Dodaj `AsNoTracking` wywołuje `Details` i narzędzia HttpGet `Delete` metody.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Zmodyfikuj widoki kursu

W *Views/Courses/Create.cshtml*, dodaj opcję "Wybierz dział" **działu** listy rozwijanej listy, Zmień podpis z **DepartmentID** do  **Dział**i Dodaj komunikat dotyczący sprawdzania poprawności.

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

W *Views/Courses/Edit.cshtml*, wprowadzić zmianę tego samego pola działu, które właśnie zrobił w *Create.cshtml*.

Również w *Views/Courses/Edit.cshtml*, Dodaj pole Liczba kurs przed **tytuł** pola. Ponieważ liczba kurs jest klucz podstawowy, jest wyświetlana, ale nie można jej zmienić.

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Istnieje już ukrytego pola (`<input type="hidden">`) dla numeru kurs w widoku do edycji. Dodawanie `<label>` Pomocnik tagu nie eliminuje potrzebę stosowania pole ukryte, ponieważ nie powoduje numer kurs, które mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **Zapisz** na **Edytuj** strony.

W *Views/Courses/Delete.cshtml*, Dodaj pole Liczba kurs u góry i zmienić identyfikator działu do nazwy działu.

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

W *Views/Courses/Details.cshtml*, wprowadzić tę samą zmianę, która właśnie została ona *Delete.cshtml*.

### <a name="test-the-course-pages"></a>Testowanie stron kursu

Uruchom aplikację, wybierz **kursów** kliknij pozycję **Utwórz nowy**, a następnie wprowadź dane dla nowego kursu:

![Strona tworzenia kursu](update-related-data/_static/course-create.png)

Kliknij przycisk **Utwórz**. Za pomocą nowego kursu dodany do listy zostanie wyświetlona strona indeksu kursów. Nazwa działu, na liście strony indeksu pochodzi z właściwości nawigacji, pokazujący, że relacja zostało ustanowione prawidłowo.

Kliknij przycisk **Edytuj** na kurs na stronie indeksu kursów.

![Strona edytowania kursu](update-related-data/_static/course-edit.png)

Zmiany danych na stronie, a następnie kliknij przycisk **Zapisz**. Z danymi zaktualizowany kurs zostanie wyświetlona strona indeksu kursów.

## <a name="add-instructors-edit-page"></a>Dodaj stronę Edytuj instruktorów

Podczas edytowania rekordu przez instruktorów chcesz można zaktualizować przez instruktorów biuro. Jednostka przez instruktorów ma relację jeden do zero lub jeden z jednostką OfficeAssignment, co oznacza, że Twój kod musi obsługiwać w następujących sytuacjach:

* Jeśli użytkownik usunie zaznaczenie przypisania pakietu office i pierwotnie miały wartość, należy usunąć jednostki OfficeAssignment.

* Jeśli użytkownik wprowadza wartość przydziału pakietu office i pierwotnie był pusty, Utwórz nową jednostkę OfficeAssignment.

* Jeśli użytkownik zmieni wartość biuro, zmień wartość w istniejącej jednostki OfficeAssignment.

### <a name="update-the-instructors-controller"></a>Aktualizacja kontrolera instruktorów

W *InstructorsController.cs*, Zmień kod w HttpGet `Edit` metodę, tak że ładuje jednostki przez instruktorów `OfficeAssignment` właściwość nawigacji i wywołania `AsNoTracking`:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

Zastąp HttpPost `Edit` metody za pomocą następującego kodu, aby obsługiwać aktualizacje przypisania pakietu office:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

Kod wykonuje następujące czynności:

-  Zmienia nazwę metody do `EditPost` ponieważ podpis teraz jest taka sama jak HttpGet `Edit` — metoda ( `ActionName` atrybut określa, że `/Edit/` adres URL jest nadal używana).

-  Pobiera eager, ładowania dla bieżącego obiektu przez instruktorów z bazy danych przy użyciu `OfficeAssignment` właściwości nawigacji. Jest to taka sama jak zrobiono w HttpGet `Edit` metody.

-  Pobrana jednostka przez instruktorów zostaje zaktualizowana o wartości z integratora modelu. `TryUpdateModel` Przeciążenie umożliwia utworzenie listy dozwolonych właściwości, które chcesz dołączyć. Pozwala to uniknąć nadmiernego ogłaszania, zgodnie z objaśnieniem w [drugim samouczku](crud.md).

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

-   Jeśli lokalizacja pakietu office jest pusta, ustawia właściwość Instructor.OfficeAssignment na wartość null, tak aby powiązanych wierszy w tabeli OfficeAssignment zostaną usunięte.

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Zapisuje zmiany w bazie danych.

### <a name="update-the-instructor-edit-view"></a>Aktualizacja widoku edycji przez instruktorów

W *Views/Instructors/Edit.cshtml*, Dodaj nowe pole do edycji w lokalizacji biura, na końcu przed **Zapisz** przycisku:

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Uruchom aplikację, wybierz **Instruktorzy** kartę, a następnie kliknij przycisk **Edytuj** na pod kierunkiem instruktora. Zmiana **lokalizacji biura** i kliknij przycisk **Zapisz**.

![Strona edytowania przez instruktorów](update-related-data/_static/instructor-edit-office.png)

## <a name="add-courses-to-edit-page"></a>Dodaj kursy na stronie edycji

Instruktorzy może nauczyć dowolnej liczby kursów. Teraz będzie ulepszenia strony edytowania przez instruktorów, dodając możliwość zmiany przypisania kurs przy użyciu grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:

![Strona edytowania przez instruktorów dzięki kursom](update-related-data/_static/instructor-edit-courses.png)

Relacja między jednostkami kurs i instruktora jest wiele do wielu. Aby dodawać i usuwać relacje, możesz dodawać i usuwać jednostki z zestawu jednostek CourseAssignments sprzężenia i.

Interfejs użytkownika, który umożliwia zmianę kursy pod kierunkiem instruktora przypisane do jest grupą pola wyboru. Pole wyboru dla każdego kursu w bazie danych jest wyświetlany, a te, które instruktora jest aktualnie przypisana do są zaznaczone. Użytkownik może zaznacz lub wyczyść pola wyboru, aby zmienić przypisania kursu. Gdyby większa liczba kursów, prawdopodobnie chcesz użyć innej metody przedstawiania danych w widoku, ale będzie używać tej samej metody, manipulowania jednostki sprzężenia, można utworzyć ani usunąć relacji.

### <a name="update-the-instructors-controller"></a>Aktualizacja kontrolera instruktorów

Aby zapewnić dane do widoku listy pól wyboru, użyjesz klasy modelu widoku.

Tworzenie *AssignedCourseData.cs* w *SchoolViewModels* folder i Zastąp istniejący kod następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

W *InstructorsController.cs*, Zastąp HttpGet `Edit` metoda następującym kodem. Zmiany są wyróżnione.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

Ten kod dodaje wczesne ładowanie dla `Courses` właściwość nawigacji i wywołuje nową `PopulateAssignedCourseData` metodę w celu udostępnienia informacji za pomocą tablicy pole wyboru `AssignedCourseData` wyświetlić klasy modelu.

Kod w `PopulateAssignedCourseData` — metoda odczytuje za pośrednictwem wszystkich jednostek kurs, aby załadować listę kursów przy użyciu klasy modelu widoku. Dla każdego kursu kod sprawdza, czy kurs istnieje w instruktora `Courses` właściwości nawigacji. Aby utworzyć wydajne wyszukiwanie, podczas sprawdzania, czy kurs jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w `HashSet` kolekcji. `Assigned` Właściwość jest ustawiona na wartość true dla kursów instruktora jest przypisany do. Widok użyje tej właściwości w celu określenia, sprawdź, które pola muszą być wyświetlany jako zaznaczone. Na koniec listy jest przekazywany do widoku `ViewData`.

Następnie dodaj kod, który jest wykonywany, gdy użytkownik kliknie **Zapisz**. Zastąp `EditPost` metoda następującym kodem i Dodaj nową metodę, która aktualizuje `Courses` właściwości nawigacji jednostki instruktora.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

Podpis metody teraz różni się od HttpGet `Edit` metody, aby nazwa metody zmieni się z `EditPost` do `Edit`.

Ponieważ widok nie zawiera kolekcję jednostek kurs, integratora modelu nie może automatycznie aktualizować `CourseAssignments` właściwości nawigacji. Zamiast używania integratora modelu do zaktualizowania `CourseAssignments` właściwość nawigacji, możesz zrobić to w nowym `UpdateInstructorCourses` metody. W związku z tym należy wykluczyć `CourseAssignments` właściwości z wiązania modelu. To nie wymaga żadnych zmian do kodu, który wywołuje `TryUpdateModel` ponieważ używasz przeciążenia umieszczania na białej liście i `CourseAssignments` nie ma na liście include.

Jeśli nie zostały zaznaczone pola wyboru kod w `UpdateInstructorCourses` inicjuje `CourseAssignments` właściwości nawigacji o pustej kolekcji i zwraca:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

Kod, a następnie przetwarza wszystkie kursy w bazie danych w pętli i sprawdza, czy każdego kursu względem nich aktualnie przypisane do przez instruktorów i te, które wybrano w widoku. W celu ułatwienia wyszukiwania wydajne, ostatnie dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli zaznaczono pole wyboru dla danego kursu, ale kurs nie znajduje się w `Instructor.CourseAssignments` właściwość nawigacji kurs jest dodawany do kolekcji we właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Jeśli to pole wyboru dla danego kursu nie zostało zaznaczone, ale jest kurs `Instructor.CourseAssignments` właściwość nawigacji, kursu zostanie usunięta z właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Aktualizowanie widoków przez instruktorów

W *Views/Instructors/Edit.cshtml*, Dodaj **kursów** pole z tablicą pola wyboru przez dodanie poniższego kodu bezpośrednio po `div` elementy **pakietu Office**  pola i przed `div` elementu **Zapisz** przycisku.

<a id="notepad"></a>
> [!NOTE]
> Po wklejeniu kodu w programie Visual Studio podziały wierszy zostanie zmieniona w taki sposób, że przerwanie wykonywania kodu.  Naciśnij klawisze Ctrl + Z jeden raz, aby cofnąć, automatycznego formatowania.  Rozwiąże podziałów wiersza, aby wyglądają jak wyświetlanych w tym miejscu. Wcięcie nie musi być idealna, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@:</tr>` linie muszą być w jednym wierszu pokazany lub otrzymasz błąd w czasie wykonywania. Za pomocą bloku wybrano nowego kodu naciśnij klawisz Tab, trzy razy na wiersz w górę nowego kodu przy użyciu istniejącego kodu. Możesz sprawdzić stan tego problemu [tutaj](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Ten kod tworzy tabelę HTML, który ma trzy kolumny. W każdej kolumnie to pole wyboru, następuje podpis, który składa się z kursu numer i tytuł. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), która informuje integratora modelu o tym, że są one traktowane jako grupa. Atrybut wartości każdego pola wyboru ma ustawioną wartość `CourseID`. Gdy strona zostanie opublikowana, integratora modelu przekazuje tablicę do kontrolera, który składa się z `CourseID` wartości dla pola wyboru, które zostały wybrane.

Jeśli początkowo są renderowane pola wyboru, te, które są przypisane do instruktora kursów sprawdzeniu atrybutów, która wybiera je (służy do wyświetlania ich zaznaczeniu tej opcji).

Uruchom aplikację, wybierz **Instruktorzy** kartę, a następnie kliknij przycisk **Edytuj** na pod kierunkiem instruktora, aby zobaczyć **Edytuj** strony.

![Strona edytowania przez instruktorów dzięki kursom](update-related-data/_static/instructor-edit-courses.png)

Zmień niektóre przypisania kursów, a następnie kliknij przycisk Zapisz. Wprowadzone zmiany zostaną odzwierciedlone na stronę indeksu.

> [!NOTE]
> Tutaj podejście do edycji przez instruktorów kurs danych działa dobrze w przypadku, gdy istnieje ograniczona liczba kursów. Dla kolekcji, które są znacznie większe innego interfejsu użytkownika i inną metodę aktualizacji będą wymagane.

## <a name="update-delete-page"></a>Strona usuwanie aktualizacji

W *InstructorsController.cs*, Usuń `DeleteConfirmed` metody i Wstaw następujący kod w jego miejscu.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Ten kod wprowadza następujące zmiany:

* Czy eager ładowania dla `CourseAssignments` właściwości nawigacji.  Należy dołączyć ten lub EF nie wiedzieć o powiązanych `CourseAssignment` jednostek i nie będzie je usunąć.  Aby uniknąć konieczności je odczytać w tym miejscu możesz skonfigurować usuwanie kaskadowe w bazie danych.

* Jeśli przez instruktorów do usunięcia jest przypisany jako administrator działy, usuwa przypisanie instruktora z tych działów.

## <a name="add-office-location-and-courses-to-create-page"></a>Dodawanie lokalizacji biura i kursy do tworzenia strony

W *InstructorsController.cs*, Usuń HttpGet i HttpPost `Create` metody, a następnie dodaj następujący kod w ich miejsce:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Ten kod jest podobny do przedstawionego na `Edit` wybrano metody z wyjątkiem który początkowo nie kursów. Narzędzia HttpGet `Create` wywołania metody `PopulateAssignedCourseData` metody nie, ponieważ może to być kursy wybrano, ale w celu Podaj pustą kolekcję dla `foreach` pętli w widoku (w przeciwnym razie Wyświetl kod zgłasza wyjątek odwołania o wartości null).

HttpPost `Create` metoda dodaje każdego kursu wybranych do `CourseAssignments` właściwość nawigacji, zanim sprawdza, czy błędy weryfikacji i dodaje nowe przez instruktorów do bazy danych. Kursy są dodawane, nawet jeśli występują błędy modelu tak, że istnieją błędy modelu (na przykład użytkownika, kluczem utworzonym na podstawie wiadomość nieprawidłową datę), gdy strona jest ponownie wyświetlony komunikat o błędzie, dowolne kurs z wybranych opcji, które zostały wprowadzone zostaną automatycznie przywrócone.

Należy zauważyć, że aby można było dodawać kursy, aby `CourseAssignments` właściwość nawigacji, musisz zainicjować właściwość jako pusta kolekcja:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Jako alternatywę w ten sposób w kodzie kontrolera nakładu pracy w modelu przez instruktorów, zmieniając metoda pobierająca właściwości, aby automatycznie tworzenie kolekcji, jeśli nie istnieje, jak pokazano w poniższym przykładzie:

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

Jeśli zmodyfikujesz `CourseAssignments` właściwość w ten sposób można usunąć kod inicjowania właściwości w jawnej w kontrolerze.

W *Views/Instructor/Create.cshtml*, Dodaj pole tekstowe lokalizacji pakietu office i Sprawdzanie pola kursy przed przycisk Prześlij. Jak w przypadku strony edytowania [naprawić, formatowanie, jeśli program Visual Studio formatuje kodu podczas jego wklejania](#notepad).

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Testowanie, uruchamianie aplikacji, a następnie tworząc pod kierunkiem instruktora.

## <a name="handling-transactions"></a>Obsługa transakcji

Jak wyjaśniono w [samouczek CRUD](crud.md), platformy Entity Framework niejawnie wykonuje transakcji. Zobacz scenariusze, w którym możesz muszą większa kontrola — na przykład, jeśli chcesz dołączyć operacje wykonywane poza programem Entity Framework w ramach transakcji — [transakcji](/ef/core/saving/transactions).

## <a name="get-the-code"></a>Pobierz kod

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Niestandardowe strony kursów
> * Dodano Instruktorzy edytowanie strony
> * Dodano kursy do edycji strony
> * Zaktualizowana strona Delete
> * Lokalizacja biura dodano i kursów do tworzenia strony

Przejdź do następnego artykułu, aby dowiedzieć się, jak obsługa konfliktów współbieżności.
> [!div class="nextstepaction"]
> [Obsługa konfliktów współbieżności](concurrency.md)
