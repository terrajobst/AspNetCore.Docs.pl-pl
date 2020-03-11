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
# <a name="tutorial-update-related-data---aspnet-mvc-with-ef-core"></a><span data-ttu-id="0f2ac-103">Samouczek: aktualizowanie powiązanych danych — ASP.NET MVC z EF Core</span><span class="sxs-lookup"><span data-stu-id="0f2ac-103">Tutorial: Update related data - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="0f2ac-104">W poprzednim samouczku Wyświetlono powiązane dane; w tym samouczku opisano aktualizowanie powiązanych danych przez aktualizację pól kluczy obcych i właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-104">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="0f2ac-105">Na poniższych ilustracjach przedstawiono niektóre ze stron, z którymi będziesz korzystać.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-105">The following illustrations show some of the pages that you'll work with.</span></span>

![Strona edytowania kursu](update-related-data/_static/course-edit.png)

![Strona edycji instruktora](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0f2ac-108">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0f2ac-109">Dostosowywanie stron kursów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-109">Customize Courses pages</span></span>
> * <span data-ttu-id="0f2ac-110">Dodaj stronę edycji instruktorów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-110">Add Instructors Edit page</span></span>
> * <span data-ttu-id="0f2ac-111">Dodawanie kursów do strony edycji</span><span class="sxs-lookup"><span data-stu-id="0f2ac-111">Add courses to Edit page</span></span>
> * <span data-ttu-id="0f2ac-112">Aktualizuj stronę usuwania</span><span class="sxs-lookup"><span data-stu-id="0f2ac-112">Update Delete page</span></span>
> * <span data-ttu-id="0f2ac-113">Dodawanie lokalizacji i kursów biura do tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="0f2ac-113">Add office location and courses to Create page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f2ac-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="0f2ac-114">Prerequisites</span></span>

* [<span data-ttu-id="0f2ac-115">Odczytywanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="0f2ac-115">Read related data</span></span>](read-related-data.md)

## <a name="customize-courses-pages"></a><span data-ttu-id="0f2ac-116">Dostosowywanie stron kursów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-116">Customize Courses pages</span></span>

<span data-ttu-id="0f2ac-117">Po utworzeniu nowej jednostki kursu musi ona mieć relację z istniejącym działem.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-117">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="0f2ac-118">Aby to ułatwić, kod szkieletowy obejmuje metody kontrolera oraz tworzenie i edytowanie widoków zawierających listę rozwijaną umożliwiającą wybranie działu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-118">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="0f2ac-119">Lista rozwijana ustawia `Course.DepartmentID` właściwość klucza obcego i to wszystko Entity Framework potrzeby, aby załadować właściwość nawigacji `Department` z odpowiednią jednostką działu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-119">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="0f2ac-120">Użyjesz kodu szkieletowego, ale nieco zmień go, aby dodać obsługę błędów i posortować listę rozwijaną.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-120">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="0f2ac-121">W *CoursesController.cs*Usuń cztery metody tworzenia i edycji i zastąp je następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-121">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="0f2ac-122">Po metodzie `Edit` HttpPost Utwórz nową metodę, która ładuje informacje działu dla listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-122">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="0f2ac-123">Metoda `PopulateDepartmentsDropDownList` pobiera listę wszystkich działów posortowanych według nazwy, tworzy kolekcję `SelectList` dla listy rozwijanej i przekazuje kolekcję do widoku w `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-123">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="0f2ac-124">Metoda akceptuje opcjonalny parametr `selectedDepartment`, który umożliwia kod wywołujący, aby określić element, który zostanie wybrany, gdy zostanie wyrenderowana lista rozwijana.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-124">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="0f2ac-125">Widok przekaże nazwę "DepartmentID" do pomocnika tagów `<select>`, a pomocnik wie, że szuka w obiekcie `ViewBag` `SelectList` o nazwie "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="0f2ac-125">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="0f2ac-126">Metoda `Create` narzędzia HttpGet wywołuje metodę `PopulateDepartmentsDropDownList` bez ustawiania wybranego elementu, ponieważ dla nowego kursu nie został jeszcze ustanowiony dział:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-126">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="0f2ac-127">Metoda `Edit` narzędzia HttpGet ustawia wybrany element na podstawie identyfikatora działu, który jest już przypisany do edytowanego kursu:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-127">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="0f2ac-128">Metody HttpPost dla obu `Create` i `Edit` obejmują również kod, który ustawia wybrany element, gdy ponownie wyświetla stronę po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-128">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="0f2ac-129">Dzięki temu gdy zostanie wyświetlona strona, aby wyświetlić komunikat o błędzie, zostanie wybrany dowolny dział.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-129">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="0f2ac-130">Dodana. AsNoTracking do metod Details i DELETE</span><span class="sxs-lookup"><span data-stu-id="0f2ac-130">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="0f2ac-131">Aby zoptymalizować wydajność szczegółów kursu i stron usuwania, Dodaj wywołania `AsNoTracking` w metodach `Delete` `Details` i narzędzia HttpGet.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-131">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="0f2ac-132">Modyfikowanie widoków kursów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-132">Modify the Course views</span></span>

<span data-ttu-id="0f2ac-133">W obszarze *widoki/kursy/Utwórz. cshtml*Dodaj opcję "Wybierz dział" do listy rozwijanej **dział** , Zmień podpis z **DepartmentID** na **Wydział**i Dodaj komunikat weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-133">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="0f2ac-134">W obszarze *widoki/kursy/Edytuj. cshtml*wprowadź tę samą zmianę dla pola działu, który właśnie został *utworzony. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-134">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="0f2ac-135">Ponadto w obszarze *widoki/kursy/Edytuj. cshtml*Dodaj pole numer kursu przed polem **tytuł** .</span><span class="sxs-lookup"><span data-stu-id="0f2ac-135">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="0f2ac-136">Ponieważ numer kursu jest kluczem podstawowym, jest wyświetlany, ale nie można go zmienić.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-136">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="0f2ac-137">Istnieje już pole ukryte (`<input type="hidden">`) dla numeru kursu w widoku edycji.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-137">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="0f2ac-138">Dodanie pomocnika tagów `<label>` nie eliminuje potrzeby ukrytego pola, ponieważ nie powoduje, że numer kursu ma być uwzględniony w opublikowanych danych, gdy użytkownik kliknie przycisk **Zapisz** na stronie **Edycja** .</span><span class="sxs-lookup"><span data-stu-id="0f2ac-138">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="0f2ac-139">W obszarze *widoki/kursy/Usuń. cshtml*Dodaj pole numer kursu z góry i zmień identyfikator działu na nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-139">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="0f2ac-140">W obszarze *widoki/kursy/szczegóły. cshtml*wprowadź tę samą zmianę, która właśnie została wykonana dla elementu *DELETE. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-140">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="0f2ac-141">Testowanie stron kursów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-141">Test the Course pages</span></span>

<span data-ttu-id="0f2ac-142">Uruchom aplikację, wybierz kartę **kursy** , kliknij pozycję **Utwórz nową**, a następnie wprowadź dane, aby utworzyć nowy kurs:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-142">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Strona tworzenia kursu](update-related-data/_static/course-create.png)

<span data-ttu-id="0f2ac-144">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-144">Click **Create**.</span></span> <span data-ttu-id="0f2ac-145">Zostanie wyświetlona strona indeks kursów z nowym kursem, który został dodany do listy.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-145">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="0f2ac-146">Nazwa działu na liście stron indeksu pochodzi z właściwości nawigacji, co oznacza, że relacja została prawidłowo ustanowiona.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-146">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="0f2ac-147">Kliknij pozycję **Edytuj** na kursie na stronie indeks kursów.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-147">Click **Edit** on a course in the Courses Index page.</span></span>

![Strona edytowania kursu](update-related-data/_static/course-edit.png)

<span data-ttu-id="0f2ac-149">Zmień dane na stronie i kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-149">Change data on the page and click **Save**.</span></span> <span data-ttu-id="0f2ac-150">Zostanie wyświetlona strona indeks kursów z zaktualizowanymi danymi kursu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-150">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-instructors-edit-page"></a><span data-ttu-id="0f2ac-151">Dodaj stronę edycji instruktorów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-151">Add Instructors Edit page</span></span>

<span data-ttu-id="0f2ac-152">Podczas edytowania rekordu instruktora chcesz mieć możliwość aktualizowania przypisania biura instruktora.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-152">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="0f2ac-153">Jednostka instruktora ma relację "jeden do zera" lub jeden-do-jednego z jednostką OfficeAssignment, co oznacza, że kod musi obsługiwać następujące sytuacje:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-153">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="0f2ac-154">Jeśli użytkownik wyczyści przypisanie pakietu Office i początkowo miał wartość, Usuń jednostkę OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-154">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="0f2ac-155">Jeśli użytkownik wprowadzi wartość przypisania pakietu Office i początkowo była pusta, należy utworzyć nową jednostkę OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-155">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="0f2ac-156">Jeśli użytkownik zmieni wartość przypisania pakietu Office, Zmień wartość w istniejącej jednostce OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-156">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="0f2ac-157">Aktualizowanie kontrolera instruktorów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-157">Update the Instructors controller</span></span>

<span data-ttu-id="0f2ac-158">W *InstructorsController.cs*Zmień kod w metodzie narzędzia HttpGet `Edit` tak, aby ładował właściwość nawigacji `OfficeAssignment` jednostki instruktora i `AsNoTracking`wywołania:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-158">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=8-11&name=snippet_EditGetOA)]

<span data-ttu-id="0f2ac-159">Zastąp metodę `Edit` HttpPost następującym kodem do obsługi aktualizacji przypisywania pakietu Office:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-159">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="0f2ac-160">Kod wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-160">The code does the following:</span></span>

* <span data-ttu-id="0f2ac-161">Zmienia nazwę metody na `EditPost`, ponieważ sygnatura jest teraz taka sama jak Metoda `Edit` narzędzia HttpGet (atrybut `ActionName` określa, że adres URL `/Edit/` jest nadal używany).</span><span class="sxs-lookup"><span data-stu-id="0f2ac-161">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

* <span data-ttu-id="0f2ac-162">Pobiera bieżącą jednostkę instruktora z bazy danych przy użyciu eager ładującego dla właściwości nawigacji `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-162">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="0f2ac-163">Jest to takie samo, jak w przypadku metody `Edit` narzędzia HttpGet.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-163">This is the same as what you did in the HttpGet `Edit` method.</span></span>

* <span data-ttu-id="0f2ac-164">Aktualizuje pobraną jednostkę instruktora o wartości ze spinacza modelu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-164">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="0f2ac-165">Przeciążenie `TryUpdateModel` pozwala dozwolonych właściwości, które mają zostać uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-165">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="0f2ac-166">Pozwala to uniknąć nadmiernego księgowania, jak wyjaśniono w [drugim samouczku](crud.md).</span><span class="sxs-lookup"><span data-stu-id="0f2ac-166">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

* <span data-ttu-id="0f2ac-167">Jeśli lokalizacja biura jest pusta, ustawia właściwość instruktor. OfficeAssignment na wartość null, aby pokrewny wiersz w tabeli OfficeAssignment zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-167">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

* <span data-ttu-id="0f2ac-168">Zapisuje zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-168">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="0f2ac-169">Aktualizuj widok do edycji instruktora</span><span class="sxs-lookup"><span data-stu-id="0f2ac-169">Update the Instructor Edit view</span></span>

<span data-ttu-id="0f2ac-170">W obszarze *widoki/instruktorzy/Edit. cshtml*Dodaj nowe pole do edytowania lokalizacji biura na końcu przed przyciskiem **Zapisz** :</span><span class="sxs-lookup"><span data-stu-id="0f2ac-170">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="0f2ac-171">Uruchom aplikację, wybierz kartę **Instruktorzy** , a następnie kliknij przycisk **Edytuj** na instruktorze.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-171">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="0f2ac-172">Zmień **lokalizację biura** , a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-172">Change the **Office Location** and click **Save**.</span></span>

![Strona edycji instruktora](update-related-data/_static/instructor-edit-office.png)

## <a name="add-courses-to-edit-page"></a><span data-ttu-id="0f2ac-174">Dodawanie kursów do strony edycji</span><span class="sxs-lookup"><span data-stu-id="0f2ac-174">Add courses to Edit page</span></span>

<span data-ttu-id="0f2ac-175">Instruktorzy mogą uczyć się dowolnej liczby kursów.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-175">Instructors may teach any number of courses.</span></span> <span data-ttu-id="0f2ac-176">Teraz poprawisz stronę Edytowanie instruktora, dodając możliwość zmiany przypisań kursu przy użyciu grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-176">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instruktor strony edytowania za pomocą kursów](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0f2ac-178">Relacja między jednostkami kursu i instruktora jest wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-178">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="0f2ac-179">Aby dodać i usunąć relacje, należy dodać i usunąć jednostki do i z zestawu jednostek sprzężenia CourseAssignments.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-179">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="0f2ac-180">Interfejs użytkownika, który umożliwia zmianę kursów, do których jest przypisany instruktor, jest grupą pól wyboru.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-180">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="0f2ac-181">Zostanie wyświetlone pole wyboru dla każdego kursu w bazie danych, a są wybrane te, do których jest przypisany instruktor.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-181">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="0f2ac-182">Użytkownik może zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-182">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="0f2ac-183">Jeśli liczba kursów była znacznie większa, prawdopodobnie chcesz użyć innej metody przedstawiania danych w widoku, ale w celu utworzenia lub usunięcia relacji należy użyć tej samej metody manipulowania jednostką sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-183">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="0f2ac-184">Aktualizowanie kontrolera instruktorów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-184">Update the Instructors controller</span></span>

<span data-ttu-id="0f2ac-185">Aby zapewnić dane do widoku listy pól wyboru, należy użyć klasy model widoku.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-185">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="0f2ac-186">Utwórz *AssignedCourseData.cs* w folderze *SchoolViewModels* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-186">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="0f2ac-187">W *InstructorsController.cs*Zastąp metodę narzędzia HttpGet `Edit` poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-187">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="0f2ac-188">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-188">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="0f2ac-189">Kod dodaje eager ładowania dla właściwości nawigacji `Courses` i wywołuje nową metodę `PopulateAssignedCourseData`, aby podać informacje dla tablicy pól wyboru przy użyciu klasy modelu widoku `AssignedCourseData`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-189">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="0f2ac-190">Kod w metodzie `PopulateAssignedCourseData` odczytuje wszystkie jednostki kursu w celu załadowania listy kursów przy użyciu klasy model widoku.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-190">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="0f2ac-191">Dla każdego kursu kod sprawdza, czy kurs istnieje we właściwości nawigacji `Courses` instruktora.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-191">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="0f2ac-192">Aby utworzyć efektywne wyszukiwanie podczas sprawdzania, czy kurs jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w kolekcji `HashSet`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-192">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="0f2ac-193">Właściwość `Assigned` jest ustawiona na wartość true dla kursów, do których jest przypisany instruktor.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-193">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="0f2ac-194">Widok użyje tej właściwości, aby określić, które pola wyboru muszą być wyświetlane jako wybrane.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-194">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="0f2ac-195">Na koniec lista jest przenoszona do widoku w `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-195">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="0f2ac-196">Następnie Dodaj kod, który jest wykonywany, gdy użytkownik kliknie przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-196">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="0f2ac-197">Zastąp metodę `EditPost` poniższym kodem i Dodaj nową metodę, która aktualizuje `Courses` właściwość nawigacji jednostki instruktora.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-197">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="0f2ac-198">Sygnatura metody jest teraz inna niż Metoda narzędzia HttpGet `Edit`, więc nazwa metody zmienia się z `EditPost` z powrotem na `Edit`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-198">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="0f2ac-199">Ponieważ widok nie zawiera kolekcji jednostek kursu, spinacz modelu nie może automatycznie zaktualizować właściwości nawigacji `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-199">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="0f2ac-200">Zamiast używać spinacza modelu do aktualizowania właściwości nawigacji `CourseAssignments`, należy to zrobić w nowej `UpdateInstructorCourses` metodzie.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-200">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="0f2ac-201">W związku z tym należy wykluczyć Właściwość `CourseAssignments` z powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-201">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="0f2ac-202">Nie wymaga to żadnych zmian w kodzie, który wywołuje `TryUpdateModel`, ponieważ jest używane Przeciążenie listy dozwolonych i `CourseAssignments` nie znajduje się na liście dołączania.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-202">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="0f2ac-203">Jeśli nie wybrano żadnych pól wyboru, kod w `UpdateInstructorCourses` inicjuje właściwość nawigacji `CourseAssignments` z pustą kolekcją i zwraca:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-203">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="0f2ac-204">Kod następnie przechodzi między wszystkimi kursami w bazie danych i sprawdza każdy kurs w odniesieniu do tych, które są aktualnie przypisane do instruktora, a także do tych, które zostały wybrane w widoku.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-204">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="0f2ac-205">Aby ułatwić efektywne wyszukiwanie, te dwie kolekcje są przechowywane w `HashSet` obiektów.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-205">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="0f2ac-206">Jeśli pole wyboru dla kursu zostało wybrane, ale kurs nie znajduje się w `Instructor.CourseAssignments` właściwości nawigacji, kurs zostanie dodany do kolekcji we właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-206">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="0f2ac-207">Jeśli nie wybrano pola wyboru dla kursu, ale kurs jest we właściwości nawigacji `Instructor.CourseAssignments`, kurs zostanie usunięty z właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-207">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="0f2ac-208">Aktualizowanie widoków instruktora</span><span class="sxs-lookup"><span data-stu-id="0f2ac-208">Update the Instructor views</span></span>

<span data-ttu-id="0f2ac-209">W obszarze *widoki/instruktorzy/Edit. cshtml*Dodaj pole **kursów** z tablicą pól wyboru, dodając następujący kod bezpośrednio po `div` elementów dla pola **Office** i przed elementem `div` dla przycisku **Zapisz** .</span><span class="sxs-lookup"><span data-stu-id="0f2ac-209">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="0f2ac-210">Po wklejeniu kodu w programie Visual Studio podziały wierszy mogą być zmieniane w sposób, który przerywa kod.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-210">When you paste the code in Visual Studio, line breaks might be changed in a way that breaks the code.</span></span> <span data-ttu-id="0f2ac-211">Jeśli kod wygląda inaczej po wklejeniu, naciśnij klawisze Ctrl + Z jednokrotne, aby cofnąć automatyczne formatowanie.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-211">If the code looks different after pasting, press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="0f2ac-212">Spowoduje to naprawienie podziałów wierszy w taki sposób, aby wyglądały jak w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-212">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="0f2ac-213">Wcięcia nie muszą być doskonałe, ale `@</tr><tr>`, `@:<td>`, `@:</td>`i `@:</tr>` muszą znajdować się w jednym wierszu, jak pokazano, lub wystąpi błąd w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-213">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="0f2ac-214">Po wybraniu bloku nowego kodu naciśnij klawisz Tab trzy razy, aby wyrównać nowy kod z istniejącym kodem.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-214">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="0f2ac-215">Ten problem został rozwiązany w programie Visual Studio 2019.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-215">This problem is fixed in Visual Studio 2019.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="0f2ac-216">Ten kod tworzy tabelę HTML z trzema kolumnami.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-216">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="0f2ac-217">W każdej kolumnie jest pole wyboru z podpisem zawierającym numer i tytuł kursu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-217">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="0f2ac-218">Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), która informuje spinacz modelu, że są one traktowane jako Grupa.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-218">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="0f2ac-219">Atrybut value każdego pola wyboru jest ustawiony na wartość `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-219">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="0f2ac-220">Po opublikowaniu strony spinacz modelu przekazuje tablicę do kontrolera, który składa się z wartości `CourseID` tylko dla pól wyboru, które są zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-220">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="0f2ac-221">Gdy pola wyboru są początkowo renderowane, te, które są przeznaczone dla kursów przypisanych do instruktora, mają zaznaczone atrybuty, które wybierają je (sprawdza zaznaczone).</span><span class="sxs-lookup"><span data-stu-id="0f2ac-221">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="0f2ac-222">Uruchom aplikację, wybierz kartę **Instruktorzy** , a następnie kliknij pozycję **Edytuj** na instruktorze, aby wyświetlić stronę **Edycja** .</span><span class="sxs-lookup"><span data-stu-id="0f2ac-222">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Instruktor strony edytowania za pomocą kursów](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0f2ac-224">Zmień niektóre przypisania kursu, a następnie kliknij przycisk Zapisz.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-224">Change some course assignments and click Save.</span></span> <span data-ttu-id="0f2ac-225">Wprowadzone zmiany zostaną odzwierciedlone na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-225">The changes you make are reflected on the Index page.</span></span>

> [!NOTE]
> <span data-ttu-id="0f2ac-226">Podejście podjęte tutaj do edytowania danych kursu instruktora działa dobrze, gdy istnieje ograniczona liczba kursów.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-226">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="0f2ac-227">W przypadku kolekcji, które są znacznie większe, będzie wymagane inne interfejs użytkownika i inna metoda aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-227">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-delete-page"></a><span data-ttu-id="0f2ac-228">Aktualizuj stronę usuwania</span><span class="sxs-lookup"><span data-stu-id="0f2ac-228">Update Delete page</span></span>

<span data-ttu-id="0f2ac-229">W *InstructorsController.cs*usuń metodę `DeleteConfirmed` i Wstaw w jej miejscu następujący kod.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-229">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="0f2ac-230">Ten kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-230">This code makes the following changes:</span></span>

* <span data-ttu-id="0f2ac-231">Wykonuje eager podczas ładowania dla właściwości nawigacji `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-231">Does eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="0f2ac-232">Musisz dołączyć ten element lub EF nie wie o pokrewnych jednostkach `CourseAssignment` i nie zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-232">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span> <span data-ttu-id="0f2ac-233">Aby uniknąć konieczności odczytywania ich w tym miejscu, można skonfigurować kaskadowe usuwanie w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-233">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="0f2ac-234">Jeśli instruktor zostanie usunięty, zostanie przypisany jako administrator jakichkolwiek działów, program usunie przypisanie instruktora z tych urzędów.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-234">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-create-page"></a><span data-ttu-id="0f2ac-235">Dodawanie lokalizacji i kursów biura do tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="0f2ac-235">Add office location and courses to Create page</span></span>

<span data-ttu-id="0f2ac-236">W *InstructorsController.cs*Usuń metody narzędzia httpget i `Create` HTTPPOST, a następnie Dodaj następujący kod w ich miejscu:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-236">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="0f2ac-237">Ten kod jest podobny do tego, co wydano dla metod `Edit`, z wyjątkiem tego, że nie wybrano kursów.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-237">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="0f2ac-238">Metoda `Create` narzędzia HttpGet wywołuje metodę `PopulateAssignedCourseData`, ponieważ mogą istnieć wybrane kursy, ale w celu udostępnienia pustej kolekcji dla pętli `foreach` w widoku (w przeciwnym razie kod widoku zgłosi wyjątek odwołania o wartości null).</span><span class="sxs-lookup"><span data-stu-id="0f2ac-238">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="0f2ac-239">Metoda HttpPost `Create` dodaje każdy wybrany kurs do właściwości nawigacji `CourseAssignments` przed sprawdzeniem poprawności błędów walidacji i dodaniem nowego instruktora do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-239">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="0f2ac-240">Kursy są dodawane nawet w przypadku błędów modelu, aby w przypadku wystąpienia błędów modelu (na przykład użytkownik określił nieprawidłową datę), a strona jest ponownie wyświetlana z komunikatem o błędzie, wszystkie wybrane wybory kursów zostaną automatycznie przywrócone.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-240">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="0f2ac-241">Zwróć uwagę, że w celu dodania kursów do właściwości nawigacji `CourseAssignments` musisz zainicjować właściwość jako pustą kolekcję:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-241">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="0f2ac-242">Alternatywnie, aby to zrobić w kodzie kontrolera, można to zrobić w modelu instruktora poprzez zmianę metody pobierającej właściwości na automatyczne utworzenie kolekcji, jeśli nie istnieje, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-242">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

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

<span data-ttu-id="0f2ac-243">Jeśli zmodyfikujesz Właściwość `CourseAssignments` w ten sposób, możesz usunąć jawny kod inicjalizacji właściwości w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-243">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="0f2ac-244">W obszarze *widoki/instruktor/Create. cshtml*Dodaj pole tekstowe Lokalizacja biura i pola wyboru dla kursów przed przyciskiem Prześlij.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-244">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="0f2ac-245">Tak jak w przypadku strony edytowania [Popraw formatowanie, jeśli program Visual Studio ponownie sformatuje kod podczas jego wklejania](#notepad).</span><span class="sxs-lookup"><span data-stu-id="0f2ac-245">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="0f2ac-246">Przetestuj aplikację i Utwórz instruktora.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-246">Test by running the app and creating an instructor.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="0f2ac-247">Obsługa transakcji</span><span class="sxs-lookup"><span data-stu-id="0f2ac-247">Handling Transactions</span></span>

<span data-ttu-id="0f2ac-248">Zgodnie z opisem w [samouczku CRUD](crud.md)Entity Framework niejawnie implementuje transakcje.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-248">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="0f2ac-249">W przypadku scenariuszy, w których potrzebna jest większa kontrola — na przykład jeśli chcesz uwzględnić operacje wykonywane poza Entity Framework w transakcji — zobacz [transakcje](/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="0f2ac-249">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](/ef/core/saving/transactions).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="0f2ac-250">Uzyskiwanie kodu</span><span class="sxs-lookup"><span data-stu-id="0f2ac-250">Get the code</span></span>

[<span data-ttu-id="0f2ac-251">Pobierz lub Wyświetl ukończoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-251">Download or view the completed application.</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="0f2ac-252">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="0f2ac-252">Next steps</span></span>

<span data-ttu-id="0f2ac-253">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0f2ac-253">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0f2ac-254">Strony kursów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="0f2ac-254">Customized Courses pages</span></span>
> * <span data-ttu-id="0f2ac-255">Dodano stronę edycji instruktorów</span><span class="sxs-lookup"><span data-stu-id="0f2ac-255">Added Instructors Edit page</span></span>
> * <span data-ttu-id="0f2ac-256">Dodano kursy do strony edycji</span><span class="sxs-lookup"><span data-stu-id="0f2ac-256">Added courses to Edit page</span></span>
> * <span data-ttu-id="0f2ac-257">Zaktualizowana strona usuwania</span><span class="sxs-lookup"><span data-stu-id="0f2ac-257">Updated Delete page</span></span>
> * <span data-ttu-id="0f2ac-258">Dodano lokalizację i kursy biura do tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="0f2ac-258">Added office location and courses to Create page</span></span>

<span data-ttu-id="0f2ac-259">Przejdź do następnego samouczka, aby dowiedzieć się, jak obsłużyć konflikty współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0f2ac-259">Advance to the next tutorial to learn how to handle concurrency conflicts.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0f2ac-260">Obsługa konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="0f2ac-260">Handle concurrency conflicts</span></span>](concurrency.md)
