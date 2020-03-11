---
title: 'Samouczek: aktualizowanie powiązanych danych — ASP.NET MVC z EF Core'
description: W tym samouczku opisano aktualizowanie powiązanych danych przez aktualizację pól kluczy obcych i właściwości nawigacji.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 83d662659fb4bc7a2867be563e4e36927d2adafe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657144"
---
# <a name="tutorial-update-related-data---aspnet-mvc-with-ef-core"></a>Samouczek: aktualizowanie powiązanych danych — ASP.NET MVC z EF Core

W poprzednim samouczku Wyświetlono powiązane dane; w tym samouczku opisano aktualizowanie powiązanych danych przez aktualizację pól kluczy obcych i właściwości nawigacji.

Na poniższych ilustracjach przedstawiono niektóre ze stron, z którymi będziesz korzystać.

![Strona edytowania kursu](update-related-data/_static/course-edit.png)

![Strona edycji instruktora](update-related-data/_static/instructor-edit-courses.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dostosowywanie stron kursów
> * Dodaj stronę edycji instruktorów
> * Dodawanie kursów do strony edycji
> * Aktualizuj stronę usuwania
> * Dodawanie lokalizacji i kursów biura do tworzenia strony

## <a name="prerequisites"></a>Wymagania wstępne

* [Odczytywanie powiązanych danych](read-related-data.md)

## <a name="customize-courses-pages"></a>Dostosowywanie stron kursów

Po utworzeniu nowej jednostki kursu musi ona mieć relację z istniejącym działem. Aby to ułatwić, kod szkieletowy obejmuje metody kontrolera oraz tworzenie i edytowanie widoków zawierających listę rozwijaną umożliwiającą wybranie działu. Lista rozwijana ustawia `Course.DepartmentID` właściwość klucza obcego i to wszystko Entity Framework potrzeby, aby załadować właściwość nawigacji `Department` z odpowiednią jednostką działu. Użyjesz kodu szkieletowego, ale nieco zmień go, aby dodać obsługę błędów i posortować listę rozwijaną.

W *CoursesController.cs*Usuń cztery metody tworzenia i edycji i zastąp je następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Po metodzie `Edit` HttpPost Utwórz nową metodę, która ładuje informacje działu dla listy rozwijanej.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

Metoda `PopulateDepartmentsDropDownList` pobiera listę wszystkich działów posortowanych według nazwy, tworzy kolekcję `SelectList` dla listy rozwijanej i przekazuje kolekcję do widoku w `ViewBag`. Metoda akceptuje opcjonalny parametr `selectedDepartment`, który umożliwia kod wywołujący, aby określić element, który zostanie wybrany, gdy zostanie wyrenderowana lista rozwijana. Widok przekaże nazwę "DepartmentID" do pomocnika tagów `<select>`, a pomocnik wie, że szuka w obiekcie `ViewBag` `SelectList` o nazwie "DepartmentID".

Metoda `Create` narzędzia HttpGet wywołuje metodę `PopulateDepartmentsDropDownList` bez ustawiania wybranego elementu, ponieważ dla nowego kursu nie został jeszcze ustanowiony dział:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

Metoda `Edit` narzędzia HttpGet ustawia wybrany element na podstawie identyfikatora działu, który jest już przypisany do edytowanego kursu:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Metody HttpPost dla obu `Create` i `Edit` obejmują również kod, który ustawia wybrany element, gdy ponownie wyświetla stronę po wystąpieniu błędu. Dzięki temu gdy zostanie wyświetlona strona, aby wyświetlić komunikat o błędzie, zostanie wybrany dowolny dział.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Dodana. AsNoTracking do metod Details i DELETE

Aby zoptymalizować wydajność szczegółów kursu i stron usuwania, Dodaj wywołania `AsNoTracking` w metodach `Delete` `Details` i narzędzia HttpGet.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Modyfikowanie widoków kursów

W obszarze *widoki/kursy/Utwórz. cshtml*Dodaj opcję "Wybierz dział" do listy rozwijanej **dział** , Zmień podpis z **DepartmentID** na **Wydział**i Dodaj komunikat weryfikacji.

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

W obszarze *widoki/kursy/Edytuj. cshtml*wprowadź tę samą zmianę dla pola działu, który właśnie został *utworzony. cshtml*.

Ponadto w obszarze *widoki/kursy/Edytuj. cshtml*Dodaj pole numer kursu przed polem **tytuł** . Ponieważ numer kursu jest kluczem podstawowym, jest wyświetlany, ale nie można go zmienić.

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Istnieje już pole ukryte (`<input type="hidden">`) dla numeru kursu w widoku edycji. Dodanie pomocnika tagów `<label>` nie eliminuje potrzeby ukrytego pola, ponieważ nie powoduje, że numer kursu ma być uwzględniony w opublikowanych danych, gdy użytkownik kliknie przycisk **Zapisz** na stronie **Edycja** .

W obszarze *widoki/kursy/Usuń. cshtml*Dodaj pole numer kursu z góry i zmień identyfikator działu na nazwę działu.

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

W obszarze *widoki/kursy/szczegóły. cshtml*wprowadź tę samą zmianę, która właśnie została wykonana dla elementu *DELETE. cshtml*.

### <a name="test-the-course-pages"></a>Testowanie stron kursów

Uruchom aplikację, wybierz kartę **kursy** , kliknij pozycję **Utwórz nową**, a następnie wprowadź dane, aby utworzyć nowy kurs:

![Strona tworzenia kursu](update-related-data/_static/course-create.png)

Kliknij przycisk **Utwórz**. Zostanie wyświetlona strona indeks kursów z nowym kursem, który został dodany do listy. Nazwa działu na liście stron indeksu pochodzi z właściwości nawigacji, co oznacza, że relacja została prawidłowo ustanowiona.

Kliknij pozycję **Edytuj** na kursie na stronie indeks kursów.

![Strona edytowania kursu](update-related-data/_static/course-edit.png)

Zmień dane na stronie i kliknij przycisk **Zapisz**. Zostanie wyświetlona strona indeks kursów z zaktualizowanymi danymi kursu.

## <a name="add-instructors-edit-page"></a>Dodaj stronę edycji instruktorów

Podczas edytowania rekordu instruktora chcesz mieć możliwość aktualizowania przypisania biura instruktora. Jednostka instruktora ma relację "jeden do zera" lub jeden-do-jednego z jednostką OfficeAssignment, co oznacza, że kod musi obsługiwać następujące sytuacje:

* Jeśli użytkownik wyczyści przypisanie pakietu Office i początkowo miał wartość, Usuń jednostkę OfficeAssignment.

* Jeśli użytkownik wprowadzi wartość przypisania pakietu Office i początkowo była pusta, należy utworzyć nową jednostkę OfficeAssignment.

* Jeśli użytkownik zmieni wartość przypisania pakietu Office, Zmień wartość w istniejącej jednostce OfficeAssignment.

### <a name="update-the-instructors-controller"></a>Aktualizowanie kontrolera instruktorów

W *InstructorsController.cs*Zmień kod w metodzie narzędzia HttpGet `Edit` tak, aby ładował właściwość nawigacji `OfficeAssignment` jednostki instruktora i `AsNoTracking`wywołania:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=8-11&name=snippet_EditGetOA)]

Zastąp metodę `Edit` HttpPost następującym kodem do obsługi aktualizacji przypisywania pakietu Office:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

Kod wykonuje następujące czynności:

* Zmienia nazwę metody na `EditPost`, ponieważ sygnatura jest teraz taka sama jak Metoda `Edit` narzędzia HttpGet (atrybut `ActionName` określa, że adres URL `/Edit/` jest nadal używany).

* Pobiera bieżącą jednostkę instruktora z bazy danych przy użyciu eager ładującego dla właściwości nawigacji `OfficeAssignment`. Jest to takie samo, jak w przypadku metody `Edit` narzędzia HttpGet.

* Aktualizuje pobraną jednostkę instruktora o wartości ze spinacza modelu. Przeciążenie `TryUpdateModel` pozwala dozwolonych właściwości, które mają zostać uwzględnione. Pozwala to uniknąć nadmiernego księgowania, jak wyjaśniono w [drugim samouczku](crud.md).

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

* Jeśli lokalizacja biura jest pusta, ustawia właściwość instruktor. OfficeAssignment na wartość null, aby pokrewny wiersz w tabeli OfficeAssignment zostanie usunięty.

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

* Zapisuje zmiany w bazie danych.

### <a name="update-the-instructor-edit-view"></a>Aktualizuj widok do edycji instruktora

W obszarze *widoki/instruktorzy/Edit. cshtml*Dodaj nowe pole do edytowania lokalizacji biura na końcu przed przyciskiem **Zapisz** :

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Uruchom aplikację, wybierz kartę **Instruktorzy** , a następnie kliknij przycisk **Edytuj** na instruktorze. Zmień **lokalizację biura** , a następnie kliknij przycisk **Zapisz**.

![Strona edycji instruktora](update-related-data/_static/instructor-edit-office.png)

## <a name="add-courses-to-edit-page"></a>Dodawanie kursów do strony edycji

Instruktorzy mogą uczyć się dowolnej liczby kursów. Teraz poprawisz stronę Edytowanie instruktora, dodając możliwość zmiany przypisań kursu przy użyciu grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:

![Instruktor strony edytowania za pomocą kursów](update-related-data/_static/instructor-edit-courses.png)

Relacja między jednostkami kursu i instruktora jest wiele-do-wielu. Aby dodać i usunąć relacje, należy dodać i usunąć jednostki do i z zestawu jednostek sprzężenia CourseAssignments.

Interfejs użytkownika, który umożliwia zmianę kursów, do których jest przypisany instruktor, jest grupą pól wyboru. Zostanie wyświetlone pole wyboru dla każdego kursu w bazie danych, a są wybrane te, do których jest przypisany instruktor. Użytkownik może zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Jeśli liczba kursów była znacznie większa, prawdopodobnie chcesz użyć innej metody przedstawiania danych w widoku, ale w celu utworzenia lub usunięcia relacji należy użyć tej samej metody manipulowania jednostką sprzężenia.

### <a name="update-the-instructors-controller"></a>Aktualizowanie kontrolera instruktorów

Aby zapewnić dane do widoku listy pól wyboru, należy użyć klasy model widoku.

Utwórz *AssignedCourseData.cs* w folderze *SchoolViewModels* i Zastąp istniejący kod następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

W *InstructorsController.cs*Zastąp metodę narzędzia HttpGet `Edit` poniższym kodem. Zmiany są wyróżnione.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

Kod dodaje eager ładowania dla właściwości nawigacji `Courses` i wywołuje nową metodę `PopulateAssignedCourseData`, aby podać informacje dla tablicy pól wyboru przy użyciu klasy modelu widoku `AssignedCourseData`.

Kod w metodzie `PopulateAssignedCourseData` odczytuje wszystkie jednostki kursu w celu załadowania listy kursów przy użyciu klasy model widoku. Dla każdego kursu kod sprawdza, czy kurs istnieje we właściwości nawigacji `Courses` instruktora. Aby utworzyć efektywne wyszukiwanie podczas sprawdzania, czy kurs jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w kolekcji `HashSet`. Właściwość `Assigned` jest ustawiona na wartość true dla kursów, do których jest przypisany instruktor. Widok użyje tej właściwości, aby określić, które pola wyboru muszą być wyświetlane jako wybrane. Na koniec lista jest przenoszona do widoku w `ViewData`.

Następnie Dodaj kod, który jest wykonywany, gdy użytkownik kliknie przycisk **Zapisz**. Zastąp metodę `EditPost` poniższym kodem i Dodaj nową metodę, która aktualizuje `Courses` właściwość nawigacji jednostki instruktora.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

Sygnatura metody jest teraz inna niż Metoda narzędzia HttpGet `Edit`, więc nazwa metody zmienia się z `EditPost` z powrotem na `Edit`.

Ponieważ widok nie zawiera kolekcji jednostek kursu, spinacz modelu nie może automatycznie zaktualizować właściwości nawigacji `CourseAssignments`. Zamiast używać spinacza modelu do aktualizowania właściwości nawigacji `CourseAssignments`, należy to zrobić w nowej `UpdateInstructorCourses` metodzie. W związku z tym należy wykluczyć Właściwość `CourseAssignments` z powiązania modelu. Nie wymaga to żadnych zmian w kodzie, który wywołuje `TryUpdateModel`, ponieważ jest używane Przeciążenie listy dozwolonych i `CourseAssignments` nie znajduje się na liście dołączania.

Jeśli nie wybrano żadnych pól wyboru, kod w `UpdateInstructorCourses` inicjuje właściwość nawigacji `CourseAssignments` z pustą kolekcją i zwraca:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

Kod następnie przechodzi między wszystkimi kursami w bazie danych i sprawdza każdy kurs w odniesieniu do tych, które są aktualnie przypisane do instruktora, a także do tych, które zostały wybrane w widoku. Aby ułatwić efektywne wyszukiwanie, te dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli pole wyboru dla kursu zostało wybrane, ale kurs nie znajduje się w `Instructor.CourseAssignments` właściwości nawigacji, kurs zostanie dodany do kolekcji we właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Jeśli nie wybrano pola wyboru dla kursu, ale kurs jest we właściwości nawigacji `Instructor.CourseAssignments`, kurs zostanie usunięty z właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Aktualizowanie widoków instruktora

W obszarze *widoki/instruktorzy/Edit. cshtml*Dodaj pole **kursów** z tablicą pól wyboru, dodając następujący kod bezpośrednio po `div` elementów dla pola **Office** i przed elementem `div` dla przycisku **Zapisz** .

<a id="notepad"></a>
> [!NOTE]
> Po wklejeniu kodu w programie Visual Studio podziały wierszy mogą być zmieniane w sposób, który przerywa kod. Jeśli kod wygląda inaczej po wklejeniu, naciśnij klawisze Ctrl + Z jednokrotne, aby cofnąć automatyczne formatowanie. Spowoduje to naprawienie podziałów wierszy w taki sposób, aby wyglądały jak w tym miejscu. Wcięcia nie muszą być doskonałe, ale `@</tr><tr>`, `@:<td>`, `@:</td>`i `@:</tr>` muszą znajdować się w jednym wierszu, jak pokazano, lub wystąpi błąd w czasie wykonywania. Po wybraniu bloku nowego kodu naciśnij klawisz Tab trzy razy, aby wyrównać nowy kod z istniejącym kodem. Ten problem został rozwiązany w programie Visual Studio 2019.

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Ten kod tworzy tabelę HTML z trzema kolumnami. W każdej kolumnie jest pole wyboru z podpisem zawierającym numer i tytuł kursu. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), która informuje spinacz modelu, że są one traktowane jako Grupa. Atrybut value każdego pola wyboru jest ustawiony na wartość `CourseID`. Po opublikowaniu strony spinacz modelu przekazuje tablicę do kontrolera, który składa się z wartości `CourseID` tylko dla pól wyboru, które są zaznaczone.

Gdy pola wyboru są początkowo renderowane, te, które są przeznaczone dla kursów przypisanych do instruktora, mają zaznaczone atrybuty, które wybierają je (sprawdza zaznaczone).

Uruchom aplikację, wybierz kartę **Instruktorzy** , a następnie kliknij pozycję **Edytuj** na instruktorze, aby wyświetlić stronę **Edycja** .

![Instruktor strony edytowania za pomocą kursów](update-related-data/_static/instructor-edit-courses.png)

Zmień niektóre przypisania kursu, a następnie kliknij przycisk Zapisz. Wprowadzone zmiany zostaną odzwierciedlone na stronie indeksu.

> [!NOTE]
> Podejście podjęte tutaj do edytowania danych kursu instruktora działa dobrze, gdy istnieje ograniczona liczba kursów. W przypadku kolekcji, które są znacznie większe, będzie wymagane inne interfejs użytkownika i inna metoda aktualizacji.

## <a name="update-delete-page"></a>Aktualizuj stronę usuwania

W *InstructorsController.cs*usuń metodę `DeleteConfirmed` i Wstaw w jej miejscu następujący kod.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Ten kod wprowadza następujące zmiany:

* Wykonuje eager podczas ładowania dla właściwości nawigacji `CourseAssignments`. Musisz dołączyć ten element lub EF nie wie o pokrewnych jednostkach `CourseAssignment` i nie zostaną usunięte. Aby uniknąć konieczności odczytywania ich w tym miejscu, można skonfigurować kaskadowe usuwanie w bazie danych.

* Jeśli instruktor zostanie usunięty, zostanie przypisany jako administrator jakichkolwiek działów, program usunie przypisanie instruktora z tych urzędów.

## <a name="add-office-location-and-courses-to-create-page"></a>Dodawanie lokalizacji i kursów biura do tworzenia strony

W *InstructorsController.cs*Usuń metody narzędzia httpget i `Create` HTTPPOST, a następnie Dodaj następujący kod w ich miejscu:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Ten kod jest podobny do tego, co wydano dla metod `Edit`, z wyjątkiem tego, że nie wybrano kursów. Metoda `Create` narzędzia HttpGet wywołuje metodę `PopulateAssignedCourseData`, ponieważ mogą istnieć wybrane kursy, ale w celu udostępnienia pustej kolekcji dla pętli `foreach` w widoku (w przeciwnym razie kod widoku zgłosi wyjątek odwołania o wartości null).

Metoda HttpPost `Create` dodaje każdy wybrany kurs do właściwości nawigacji `CourseAssignments` przed sprawdzeniem poprawności błędów walidacji i dodaniem nowego instruktora do bazy danych. Kursy są dodawane nawet w przypadku błędów modelu, aby w przypadku wystąpienia błędów modelu (na przykład użytkownik określił nieprawidłową datę), a strona jest ponownie wyświetlana z komunikatem o błędzie, wszystkie wybrane wybory kursów zostaną automatycznie przywrócone.

Zwróć uwagę, że w celu dodania kursów do właściwości nawigacji `CourseAssignments` musisz zainicjować właściwość jako pustą kolekcję:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Alternatywnie, aby to zrobić w kodzie kontrolera, można to zrobić w modelu instruktora poprzez zmianę metody pobierającej właściwości na automatyczne utworzenie kolekcji, jeśli nie istnieje, jak pokazano w następującym przykładzie:

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

Jeśli zmodyfikujesz Właściwość `CourseAssignments` w ten sposób, możesz usunąć jawny kod inicjalizacji właściwości w kontrolerze.

W obszarze *widoki/instruktor/Create. cshtml*Dodaj pole tekstowe Lokalizacja biura i pola wyboru dla kursów przed przyciskiem Prześlij. Tak jak w przypadku strony edytowania [Popraw formatowanie, jeśli program Visual Studio ponownie sformatuje kod podczas jego wklejania](#notepad).

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Przetestuj aplikację i Utwórz instruktora.

## <a name="handling-transactions"></a>Obsługa transakcji

Zgodnie z opisem w [samouczku CRUD](crud.md)Entity Framework niejawnie implementuje transakcje. W przypadku scenariuszy, w których potrzebna jest większa kontrola — na przykład jeśli chcesz uwzględnić operacje wykonywane poza Entity Framework w transakcji — zobacz [transakcje](/ef/core/saving/transactions).

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Strony kursów niestandardowych
> * Dodano stronę edycji instruktorów
> * Dodano kursy do strony edycji
> * Zaktualizowana strona usuwania
> * Dodano lokalizację i kursy biura do tworzenia strony

Przejdź do następnego samouczka, aby dowiedzieć się, jak obsłużyć konflikty współbieżności.

> [!div class="nextstepaction"]
> [Obsługa konfliktów współbieżności](concurrency.md)
