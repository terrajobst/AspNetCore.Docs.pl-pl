---
title: 'Samouczek: Aktualizowanie powiązanych danych — ASP.NET MVC z programem EF Core'
description: W tym samouczku będziesz aktualizowanie powiązanych danych, aktualizując pola kluczy obcych i właściwości nawigacji.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 6add725430380f0855fe660a70b90a4546ef0637
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750908"
---
# <a name="tutorial-update-related-data---aspnet-mvc-with-ef-core"></a><span data-ttu-id="e9f47-103">Samouczek: Aktualizowanie powiązanych danych — ASP.NET MVC z programem EF Core</span><span class="sxs-lookup"><span data-stu-id="e9f47-103">Tutorial: Update related data - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="e9f47-104">W poprzednim samouczku wyświetlane powiązanych danych; w tym samouczku będziesz aktualizowanie powiązanych danych, aktualizując pola kluczy obcych i właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-104">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="e9f47-105">Na poniższych ilustracjach przedstawiono niektóre stron którymi będziesz pracować.</span><span class="sxs-lookup"><span data-stu-id="e9f47-105">The following illustrations show some of the pages that you'll work with.</span></span>

![Strona edytowania kursu](update-related-data/_static/course-edit.png)

![Strona edytowania przez instruktorów](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="e9f47-108">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="e9f47-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e9f47-109">Dostosowywanie stron kursów</span><span class="sxs-lookup"><span data-stu-id="e9f47-109">Customize Courses pages</span></span>
> * <span data-ttu-id="e9f47-110">Dodaj stronę Edytuj instruktorów</span><span class="sxs-lookup"><span data-stu-id="e9f47-110">Add Instructors Edit page</span></span>
> * <span data-ttu-id="e9f47-111">Dodaj kursy na stronie edycji</span><span class="sxs-lookup"><span data-stu-id="e9f47-111">Add courses to Edit page</span></span>
> * <span data-ttu-id="e9f47-112">Strona usuwanie aktualizacji</span><span class="sxs-lookup"><span data-stu-id="e9f47-112">Update Delete page</span></span>
> * <span data-ttu-id="e9f47-113">Dodawanie lokalizacji biura i kursy do tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="e9f47-113">Add office location and courses to Create page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9f47-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e9f47-114">Prerequisites</span></span>

* [<span data-ttu-id="e9f47-115">Odczytywanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="e9f47-115">Read related data</span></span>](read-related-data.md)

## <a name="customize-courses-pages"></a><span data-ttu-id="e9f47-116">Dostosowywanie stron kursów</span><span class="sxs-lookup"><span data-stu-id="e9f47-116">Customize Courses pages</span></span>

<span data-ttu-id="e9f47-117">Po utworzeniu nowej jednostki kurs, musi on mieć relacji do istniejących działu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-117">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="e9f47-118">Aby ułatwić to zadanie, utworzony szkielet kodu zawiera metod kontrolera oraz tworzyć i edytować widoki, które zawierają listy rozwijanej, służąca do wybierania działu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-118">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="e9f47-119">Z listy rozwijanej zestawów `Course.DepartmentID` właściwości klucza obcego, i to wszystko Entity Framework wymaga, aby mogła załadować `Department` właściwość nawigacji z odpowiednią jednostką działu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-119">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="e9f47-120">Będziesz używać utworzony szkielet kodu, ale Zmień ją nieco na dodawanie obsługi błędów i sortowanie listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="e9f47-120">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="e9f47-121">W *CoursesController.cs*, Usuń cztery metody tworzyć i edytować i zastąpić je z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="e9f47-121">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="e9f47-122">Po `Edit` metody HttpPost, utworzenie nowej metody, która ładuje informacji działu dla listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="e9f47-122">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="e9f47-123">`PopulateDepartmentsDropDownList` Metoda pobiera listę wszystkich działach sortowane według nazwy, tworzy `SelectList` kolekcji dla listy rozwijanej, a następnie przekazuje kolekcji do widoku `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="e9f47-123">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="e9f47-124">Metoda przyjmuje opcjonalną `selectedDepartment` parametr, który umożliwia kod wywołujący, aby określić element, który zostanie wybrany podczas renderowania listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="e9f47-124">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="e9f47-125">Widok będzie przekazywać nazwę jednostki "DepartmentID" do `<select>` następnie wie, Pomocnik tagu i pomocnika do przeszukania `ViewBag` dla obiektu `SelectList` o nazwie "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="e9f47-125">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="e9f47-126">Narzędzia HttpGet `Create` wywołania metody `PopulateDepartmentsDropDownList` metody bez ustawienia wybranego elementu, ponieważ nowego kursu działu nie jest jeszcze ustalony:</span><span class="sxs-lookup"><span data-stu-id="e9f47-126">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="e9f47-127">Narzędzia HttpGet `Edit` metoda ustawia wybranego elementu, na podstawie Identyfikatora działu, który jest już przypisana do kursu edytowanym:</span><span class="sxs-lookup"><span data-stu-id="e9f47-127">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="e9f47-128">Obie metody HttpPost `Create` i `Edit` również dołączyć kod, który ustawia wybranego elementu, gdy ich ponownego wyświetlenia strony po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-128">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="e9f47-129">Daje to gwarancję, że po stronie zostanie wyświetlony ponownie, aby wyświetlić komunikat o błędzie, niezależnie od działu wybrano pozostaje wybrane.</span><span class="sxs-lookup"><span data-stu-id="e9f47-129">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="e9f47-130">Dodaj. AsNoTracking do szczegółów i usuwania metod</span><span class="sxs-lookup"><span data-stu-id="e9f47-130">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="e9f47-131">Aby zoptymalizować wydajność kurs szczegółów i usuwania stron, Dodaj `AsNoTracking` wywołuje `Details` i narzędzia HttpGet `Delete` metody.</span><span class="sxs-lookup"><span data-stu-id="e9f47-131">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="e9f47-132">Zmodyfikuj widoki kursu</span><span class="sxs-lookup"><span data-stu-id="e9f47-132">Modify the Course views</span></span>

<span data-ttu-id="e9f47-133">W *Views/Courses/Create.cshtml*, dodaj opcję "Wybierz dział" **działu** listy rozwijanej listy, Zmień podpis z **DepartmentID** do  **Dział**i Dodaj komunikat dotyczący sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="e9f47-133">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="e9f47-134">W *Views/Courses/Edit.cshtml*, wprowadzić zmianę tego samego pola działu, które właśnie zrobił w *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e9f47-134">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="e9f47-135">Również w *Views/Courses/Edit.cshtml*, Dodaj pole Liczba kurs przed **tytuł** pola.</span><span class="sxs-lookup"><span data-stu-id="e9f47-135">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="e9f47-136">Ponieważ liczba kurs jest klucz podstawowy, jest wyświetlana, ale nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="e9f47-136">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="e9f47-137">Istnieje już ukrytego pola (`<input type="hidden">`) dla numeru kurs w widoku do edycji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-137">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="e9f47-138">Dodawanie `<label>` Pomocnik tagu nie eliminuje potrzebę stosowania pole ukryte, ponieważ nie powoduje numer kurs, które mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **Zapisz** na **Edytuj** strony.</span><span class="sxs-lookup"><span data-stu-id="e9f47-138">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="e9f47-139">W *Views/Courses/Delete.cshtml*, Dodaj pole Liczba kurs u góry i zmienić identyfikator działu do nazwy działu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-139">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="e9f47-140">W *Views/Courses/Details.cshtml*, wprowadzić tę samą zmianę, która właśnie została ona *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e9f47-140">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="e9f47-141">Testowanie stron kursu</span><span class="sxs-lookup"><span data-stu-id="e9f47-141">Test the Course pages</span></span>

<span data-ttu-id="e9f47-142">Uruchom aplikację, wybierz **kursów** kliknij pozycję **Utwórz nowy**, a następnie wprowadź dane dla nowego kursu:</span><span class="sxs-lookup"><span data-stu-id="e9f47-142">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Strona tworzenia kursu](update-related-data/_static/course-create.png)

<span data-ttu-id="e9f47-144">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="e9f47-144">Click **Create**.</span></span> <span data-ttu-id="e9f47-145">Za pomocą nowego kursu dodany do listy zostanie wyświetlona strona indeksu kursów.</span><span class="sxs-lookup"><span data-stu-id="e9f47-145">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="e9f47-146">Nazwa działu, na liście strony indeksu pochodzi z właściwości nawigacji, pokazujący, że relacja zostało ustanowione prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="e9f47-146">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="e9f47-147">Kliknij przycisk **Edytuj** na kurs na stronie indeksu kursów.</span><span class="sxs-lookup"><span data-stu-id="e9f47-147">Click **Edit** on a course in the Courses Index page.</span></span>

![Strona edytowania kursu](update-related-data/_static/course-edit.png)

<span data-ttu-id="e9f47-149">Zmiany danych na stronie, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="e9f47-149">Change data on the page and click **Save**.</span></span> <span data-ttu-id="e9f47-150">Z danymi zaktualizowany kurs zostanie wyświetlona strona indeksu kursów.</span><span class="sxs-lookup"><span data-stu-id="e9f47-150">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-instructors-edit-page"></a><span data-ttu-id="e9f47-151">Dodaj stronę Edytuj instruktorów</span><span class="sxs-lookup"><span data-stu-id="e9f47-151">Add Instructors Edit page</span></span>

<span data-ttu-id="e9f47-152">Podczas edytowania rekordu przez instruktorów chcesz można zaktualizować przez instruktorów biuro.</span><span class="sxs-lookup"><span data-stu-id="e9f47-152">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="e9f47-153">Jednostka przez instruktorów ma relację jeden do zero lub jeden z jednostką OfficeAssignment, co oznacza, że Twój kod musi obsługiwać w następujących sytuacjach:</span><span class="sxs-lookup"><span data-stu-id="e9f47-153">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="e9f47-154">Jeśli użytkownik usunie zaznaczenie przypisania pakietu office i pierwotnie miały wartość, należy usunąć jednostki OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="e9f47-154">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="e9f47-155">Jeśli użytkownik wprowadza wartość przydziału pakietu office i pierwotnie był pusty, Utwórz nową jednostkę OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="e9f47-155">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="e9f47-156">Jeśli użytkownik zmieni wartość biuro, zmień wartość w istniejącej jednostki OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="e9f47-156">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="e9f47-157">Aktualizacja kontrolera instruktorów</span><span class="sxs-lookup"><span data-stu-id="e9f47-157">Update the Instructors controller</span></span>

<span data-ttu-id="e9f47-158">W *InstructorsController.cs*, Zmień kod w HttpGet `Edit` metodę, tak że ładuje jednostki przez instruktorów `OfficeAssignment` właściwość nawigacji i wywołania `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="e9f47-158">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=8-11&name=snippet_EditGetOA)]

<span data-ttu-id="e9f47-159">Zastąp HttpPost `Edit` metody za pomocą następującego kodu, aby obsługiwać aktualizacje przypisania pakietu office:</span><span class="sxs-lookup"><span data-stu-id="e9f47-159">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="e9f47-160">Kod wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="e9f47-160">The code does the following:</span></span>

* <span data-ttu-id="e9f47-161">Zmienia nazwę metody do `EditPost` ponieważ podpis teraz jest taka sama jak HttpGet `Edit` — metoda ( `ActionName` atrybut określa, że `/Edit/` adres URL jest nadal używana).</span><span class="sxs-lookup"><span data-stu-id="e9f47-161">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

* <span data-ttu-id="e9f47-162">Pobiera eager, ładowania dla bieżącego obiektu przez instruktorów z bazy danych przy użyciu `OfficeAssignment` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-162">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="e9f47-163">Jest to taka sama jak zrobiono w HttpGet `Edit` metody.</span><span class="sxs-lookup"><span data-stu-id="e9f47-163">This is the same as what you did in the HttpGet `Edit` method.</span></span>

* <span data-ttu-id="e9f47-164">Pobrana jednostka przez instruktorów zostaje zaktualizowana o wartości z integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-164">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="e9f47-165">`TryUpdateModel` Przeciążenie umożliwia utworzenie listy dozwolonych właściwości, które chcesz dołączyć.</span><span class="sxs-lookup"><span data-stu-id="e9f47-165">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="e9f47-166">Pozwala to uniknąć nadmiernego ogłaszania, zgodnie z objaśnieniem w [drugim samouczku](crud.md).</span><span class="sxs-lookup"><span data-stu-id="e9f47-166">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

* <span data-ttu-id="e9f47-167">Jeśli lokalizacja pakietu office jest pusta, ustawia właściwość Instructor.OfficeAssignment na wartość null, tak aby powiązanych wierszy w tabeli OfficeAssignment zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="e9f47-167">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

* <span data-ttu-id="e9f47-168">Zapisuje zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e9f47-168">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="e9f47-169">Aktualizacja widoku edycji przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="e9f47-169">Update the Instructor Edit view</span></span>

<span data-ttu-id="e9f47-170">W *Views/Instructors/Edit.cshtml*, Dodaj nowe pole do edycji w lokalizacji biura, na końcu przed **Zapisz** przycisku:</span><span class="sxs-lookup"><span data-stu-id="e9f47-170">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="e9f47-171">Uruchom aplikację, wybierz **Instruktorzy** kartę, a następnie kliknij przycisk **Edytuj** na pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="e9f47-171">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="e9f47-172">Zmiana **lokalizacji biura** i kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="e9f47-172">Change the **Office Location** and click **Save**.</span></span>

![Strona edytowania przez instruktorów](update-related-data/_static/instructor-edit-office.png)

## <a name="add-courses-to-edit-page"></a><span data-ttu-id="e9f47-174">Dodaj kursy na stronie edycji</span><span class="sxs-lookup"><span data-stu-id="e9f47-174">Add courses to Edit page</span></span>

<span data-ttu-id="e9f47-175">Instruktorzy może nauczyć dowolnej liczby kursów.</span><span class="sxs-lookup"><span data-stu-id="e9f47-175">Instructors may teach any number of courses.</span></span> <span data-ttu-id="e9f47-176">Teraz będzie ulepszenia strony edytowania przez instruktorów, dodając możliwość zmiany przypisania kurs przy użyciu grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:</span><span class="sxs-lookup"><span data-stu-id="e9f47-176">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Strona edytowania przez instruktorów dzięki kursom](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="e9f47-178">Relacja między jednostkami kurs i instruktora jest wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-178">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="e9f47-179">Aby dodawać i usuwać relacje, możesz dodawać i usuwać jednostki z zestawu jednostek CourseAssignments sprzężenia i.</span><span class="sxs-lookup"><span data-stu-id="e9f47-179">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="e9f47-180">Interfejs użytkownika, który umożliwia zmianę kursy pod kierunkiem instruktora przypisane do jest grupą pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="e9f47-180">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="e9f47-181">Pole wyboru dla każdego kursu w bazie danych jest wyświetlany, a te, które instruktora jest aktualnie przypisana do są zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="e9f47-181">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="e9f47-182">Użytkownik może zaznacz lub wyczyść pola wyboru, aby zmienić przypisania kursu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-182">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="e9f47-183">Gdyby większa liczba kursów, prawdopodobnie chcesz użyć innej metody przedstawiania danych w widoku, ale będzie używać tej samej metody, manipulowania jednostki sprzężenia, można utworzyć ani usunąć relacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-183">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="e9f47-184">Aktualizacja kontrolera instruktorów</span><span class="sxs-lookup"><span data-stu-id="e9f47-184">Update the Instructors controller</span></span>

<span data-ttu-id="e9f47-185">Aby zapewnić dane do widoku listy pól wyboru, użyjesz klasy modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="e9f47-185">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="e9f47-186">Tworzenie *AssignedCourseData.cs* w *SchoolViewModels* folder i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="e9f47-186">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="e9f47-187">W *InstructorsController.cs*, Zastąp HttpGet `Edit` metoda następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="e9f47-187">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="e9f47-188">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="e9f47-188">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="e9f47-189">Ten kod dodaje wczesne ładowanie dla `Courses` właściwość nawigacji i wywołuje nową `PopulateAssignedCourseData` metodę w celu udostępnienia informacji za pomocą tablicy pole wyboru `AssignedCourseData` wyświetlić klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-189">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="e9f47-190">Kod w `PopulateAssignedCourseData` — metoda odczytuje za pośrednictwem wszystkich jednostek kurs, aby załadować listę kursów przy użyciu klasy modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="e9f47-190">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="e9f47-191">Dla każdego kursu kod sprawdza, czy kurs istnieje w instruktora `Courses` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-191">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="e9f47-192">Aby utworzyć wydajne wyszukiwanie, podczas sprawdzania, czy kurs jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w `HashSet` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-192">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="e9f47-193">`Assigned` Właściwość jest ustawiona na wartość true dla kursów instruktora jest przypisany do.</span><span class="sxs-lookup"><span data-stu-id="e9f47-193">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="e9f47-194">Widok użyje tej właściwości w celu określenia, sprawdź, które pola muszą być wyświetlany jako zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="e9f47-194">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="e9f47-195">Na koniec listy jest przekazywany do widoku `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="e9f47-195">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="e9f47-196">Następnie dodaj kod, który jest wykonywany, gdy użytkownik kliknie **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="e9f47-196">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="e9f47-197">Zastąp `EditPost` metoda następującym kodem i Dodaj nową metodę, która aktualizuje `Courses` właściwości nawigacji jednostki instruktora.</span><span class="sxs-lookup"><span data-stu-id="e9f47-197">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="e9f47-198">Podpis metody teraz różni się od HttpGet `Edit` metody, aby nazwa metody zmieni się z `EditPost` do `Edit`.</span><span class="sxs-lookup"><span data-stu-id="e9f47-198">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="e9f47-199">Ponieważ widok nie zawiera kolekcję jednostek kurs, integratora modelu nie może automatycznie aktualizować `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-199">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="e9f47-200">Zamiast używania integratora modelu do zaktualizowania `CourseAssignments` właściwość nawigacji, możesz zrobić to w nowym `UpdateInstructorCourses` metody.</span><span class="sxs-lookup"><span data-stu-id="e9f47-200">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="e9f47-201">W związku z tym należy wykluczyć `CourseAssignments` właściwości z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-201">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="e9f47-202">To nie wymaga żadnych zmian do kodu, który wywołuje `TryUpdateModel` ponieważ używasz przeciążenia umieszczania na białej liście i `CourseAssignments` nie ma na liście include.</span><span class="sxs-lookup"><span data-stu-id="e9f47-202">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="e9f47-203">Jeśli nie zostały zaznaczone pola wyboru kod w `UpdateInstructorCourses` inicjuje `CourseAssignments` właściwości nawigacji o pustej kolekcji i zwraca:</span><span class="sxs-lookup"><span data-stu-id="e9f47-203">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="e9f47-204">Kod, a następnie przetwarza wszystkie kursy w bazie danych w pętli i sprawdza, czy każdego kursu względem nich aktualnie przypisane do przez instruktorów i te, które wybrano w widoku.</span><span class="sxs-lookup"><span data-stu-id="e9f47-204">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="e9f47-205">W celu ułatwienia wyszukiwania wydajne, ostatnie dwie kolekcje są przechowywane w `HashSet` obiektów.</span><span class="sxs-lookup"><span data-stu-id="e9f47-205">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="e9f47-206">Jeśli zaznaczono pole wyboru dla danego kursu, ale kurs nie znajduje się w `Instructor.CourseAssignments` właściwość nawigacji kurs jest dodawany do kolekcji we właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-206">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="e9f47-207">Jeśli to pole wyboru dla danego kursu nie zostało zaznaczone, ale jest kurs `Instructor.CourseAssignments` właściwość nawigacji, kursu zostanie usunięta z właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-207">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="e9f47-208">Aktualizowanie widoków przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="e9f47-208">Update the Instructor views</span></span>

<span data-ttu-id="e9f47-209">W *Views/Instructors/Edit.cshtml*, Dodaj **kursów** pole z tablicą pola wyboru przez dodanie poniższego kodu bezpośrednio po `div` elementy **pakietu Office**  pola i przed `div` elementu **Zapisz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="e9f47-209">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="e9f47-210">Po wklejeniu kodu w programie Visual Studio podziały wierszy może ulec zmianie w taki sposób, że przerwanie wykonywania kodu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-210">When you paste the code in Visual Studio, line breaks might be changed in a way that breaks the code.</span></span> <span data-ttu-id="e9f47-211">Jeśli kod wygląda inaczej po wklejania, naciśnij klawisze Ctrl + Z jeden raz do cofania, automatycznego formatowania.</span><span class="sxs-lookup"><span data-stu-id="e9f47-211">If the code looks different after pasting, press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="e9f47-212">Rozwiąże podziałów wiersza, aby wyglądają jak wyświetlanych w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-212">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="e9f47-213">Wcięcie nie musi być idealna, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@:</tr>` linie muszą być w jednym wierszu pokazany lub otrzymasz błąd w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e9f47-213">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="e9f47-214">Za pomocą bloku wybrano nowego kodu naciśnij klawisz Tab, trzy razy na wiersz w górę nowego kodu przy użyciu istniejącego kodu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-214">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="e9f47-215">Ten problem został rozwiązany w Visual Studio 2019 r.</span><span class="sxs-lookup"><span data-stu-id="e9f47-215">This problem is fixed in Visual Studio 2019.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="e9f47-216">Ten kod tworzy tabelę HTML, który ma trzy kolumny.</span><span class="sxs-lookup"><span data-stu-id="e9f47-216">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="e9f47-217">W każdej kolumnie to pole wyboru, następuje podpis, który składa się z kursu numer i tytuł.</span><span class="sxs-lookup"><span data-stu-id="e9f47-217">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="e9f47-218">Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), która informuje integratora modelu o tym, że są one traktowane jako grupa.</span><span class="sxs-lookup"><span data-stu-id="e9f47-218">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="e9f47-219">Atrybut wartości każdego pola wyboru ma ustawioną wartość `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="e9f47-219">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="e9f47-220">Gdy strona zostanie opublikowana, integratora modelu przekazuje tablicę do kontrolera, który składa się z `CourseID` wartości dla pola wyboru, które zostały wybrane.</span><span class="sxs-lookup"><span data-stu-id="e9f47-220">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="e9f47-221">Jeśli początkowo są renderowane pola wyboru, te, które są przypisane do instruktora kursów sprawdzeniu atrybutów, która wybiera je (służy do wyświetlania ich zaznaczeniu tej opcji).</span><span class="sxs-lookup"><span data-stu-id="e9f47-221">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="e9f47-222">Uruchom aplikację, wybierz **Instruktorzy** kartę, a następnie kliknij przycisk **Edytuj** na pod kierunkiem instruktora, aby zobaczyć **Edytuj** strony.</span><span class="sxs-lookup"><span data-stu-id="e9f47-222">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Strona edytowania przez instruktorów dzięki kursom](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="e9f47-224">Zmień niektóre przypisania kursów, a następnie kliknij przycisk Zapisz.</span><span class="sxs-lookup"><span data-stu-id="e9f47-224">Change some course assignments and click Save.</span></span> <span data-ttu-id="e9f47-225">Wprowadzone zmiany zostaną odzwierciedlone na stronę indeksu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-225">The changes you make are reflected on the Index page.</span></span>

> [!NOTE]
> <span data-ttu-id="e9f47-226">Tutaj podejście do edycji przez instruktorów kurs danych działa dobrze w przypadku, gdy istnieje ograniczona liczba kursów.</span><span class="sxs-lookup"><span data-stu-id="e9f47-226">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="e9f47-227">Dla kolekcji, które są znacznie większe innego interfejsu użytkownika i inną metodę aktualizacji będą wymagane.</span><span class="sxs-lookup"><span data-stu-id="e9f47-227">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-delete-page"></a><span data-ttu-id="e9f47-228">Strona usuwanie aktualizacji</span><span class="sxs-lookup"><span data-stu-id="e9f47-228">Update Delete page</span></span>

<span data-ttu-id="e9f47-229">W *InstructorsController.cs*, Usuń `DeleteConfirmed` metody i Wstaw następujący kod w jego miejscu.</span><span class="sxs-lookup"><span data-stu-id="e9f47-229">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="e9f47-230">Ten kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="e9f47-230">This code makes the following changes:</span></span>

* <span data-ttu-id="e9f47-231">Czy eager ładowania dla `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-231">Does eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="e9f47-232">Należy dołączyć ten lub EF nie wiedzieć o powiązanych `CourseAssignment` jednostek i nie będzie je usunąć.</span><span class="sxs-lookup"><span data-stu-id="e9f47-232">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span> <span data-ttu-id="e9f47-233">Aby uniknąć konieczności je odczytać w tym miejscu możesz skonfigurować usuwanie kaskadowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e9f47-233">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="e9f47-234">Jeśli przez instruktorów do usunięcia jest przypisany jako administrator działy, usuwa przypisanie instruktora z tych działów.</span><span class="sxs-lookup"><span data-stu-id="e9f47-234">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-create-page"></a><span data-ttu-id="e9f47-235">Dodawanie lokalizacji biura i kursy do tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="e9f47-235">Add office location and courses to Create page</span></span>

<span data-ttu-id="e9f47-236">W *InstructorsController.cs*, Usuń HttpGet i HttpPost `Create` metody, a następnie dodaj następujący kod w ich miejsce:</span><span class="sxs-lookup"><span data-stu-id="e9f47-236">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="e9f47-237">Ten kod jest podobny do przedstawionego na `Edit` wybrano metody z wyjątkiem który początkowo nie kursów.</span><span class="sxs-lookup"><span data-stu-id="e9f47-237">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="e9f47-238">Narzędzia HttpGet `Create` wywołania metody `PopulateAssignedCourseData` metody nie, ponieważ może to być kursy wybrano, ale w celu Podaj pustą kolekcję dla `foreach` pętli w widoku (w przeciwnym razie Wyświetl kod zgłasza wyjątek odwołania o wartości null).</span><span class="sxs-lookup"><span data-stu-id="e9f47-238">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="e9f47-239">HttpPost `Create` metoda dodaje każdego kursu wybranych do `CourseAssignments` właściwość nawigacji, zanim sprawdza, czy błędy weryfikacji i dodaje nowe przez instruktorów do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e9f47-239">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="e9f47-240">Kursy są dodawane, nawet jeśli występują błędy modelu tak, że istnieją błędy modelu (na przykład użytkownika, kluczem utworzonym na podstawie wiadomość nieprawidłową datę), gdy strona jest ponownie wyświetlony komunikat o błędzie, dowolne kurs z wybranych opcji, które zostały wprowadzone zostaną automatycznie przywrócone.</span><span class="sxs-lookup"><span data-stu-id="e9f47-240">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="e9f47-241">Należy zauważyć, że aby można było dodawać kursy, aby `CourseAssignments` właściwość nawigacji, musisz zainicjować właściwość jako pusta kolekcja:</span><span class="sxs-lookup"><span data-stu-id="e9f47-241">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="e9f47-242">Jako alternatywę w ten sposób w kodzie kontrolera nakładu pracy w modelu przez instruktorów, zmieniając metoda pobierająca właściwości, aby automatycznie tworzenie kolekcji, jeśli nie istnieje, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="e9f47-242">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

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

<span data-ttu-id="e9f47-243">Jeśli zmodyfikujesz `CourseAssignments` właściwość w ten sposób można usunąć kod inicjowania właściwości w jawnej w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="e9f47-243">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="e9f47-244">W *Views/Instructor/Create.cshtml*, Dodaj pole tekstowe lokalizacji pakietu office i Sprawdzanie pola kursy przed przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="e9f47-244">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="e9f47-245">Jak w przypadku strony edytowania [naprawić, formatowanie, jeśli program Visual Studio formatuje kodu podczas jego wklejania](#notepad).</span><span class="sxs-lookup"><span data-stu-id="e9f47-245">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="e9f47-246">Testowanie, uruchamianie aplikacji, a następnie tworząc pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="e9f47-246">Test by running the app and creating an instructor.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="e9f47-247">Obsługa transakcji</span><span class="sxs-lookup"><span data-stu-id="e9f47-247">Handling Transactions</span></span>

<span data-ttu-id="e9f47-248">Jak wyjaśniono w [samouczek CRUD](crud.md), platformy Entity Framework niejawnie wykonuje transakcji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-248">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="e9f47-249">Zobacz scenariusze, w którym możesz muszą większa kontrola — na przykład, jeśli chcesz dołączyć operacje wykonywane poza programem Entity Framework w ramach transakcji — [transakcji](/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="e9f47-249">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](/ef/core/saving/transactions).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="e9f47-250">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="e9f47-250">Get the code</span></span>

[<span data-ttu-id="e9f47-251">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e9f47-251">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="e9f47-252">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="e9f47-252">Next steps</span></span>

<span data-ttu-id="e9f47-253">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="e9f47-253">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e9f47-254">Niestandardowe strony kursów</span><span class="sxs-lookup"><span data-stu-id="e9f47-254">Customized Courses pages</span></span>
> * <span data-ttu-id="e9f47-255">Dodano Instruktorzy edytowanie strony</span><span class="sxs-lookup"><span data-stu-id="e9f47-255">Added Instructors Edit page</span></span>
> * <span data-ttu-id="e9f47-256">Dodano kursy do edycji strony</span><span class="sxs-lookup"><span data-stu-id="e9f47-256">Added courses to Edit page</span></span>
> * <span data-ttu-id="e9f47-257">Zaktualizowana strona Delete</span><span class="sxs-lookup"><span data-stu-id="e9f47-257">Updated Delete page</span></span>
> * <span data-ttu-id="e9f47-258">Lokalizacja biura dodano i kursów do tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="e9f47-258">Added office location and courses to Create page</span></span>

<span data-ttu-id="e9f47-259">Przejdź do następnego samouczka, aby dowiedzieć się, jak obsługa konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="e9f47-259">Advance to the next tutorial to learn how to handle concurrency conflicts.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e9f47-260">Obsługa konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="e9f47-260">Handle concurrency conflicts</span></span>](concurrency.md)
