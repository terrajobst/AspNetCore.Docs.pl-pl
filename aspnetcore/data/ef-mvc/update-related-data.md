---
title: Dane — 7 10 dotyczące platformy ASP.NET Core MVC podstawowych EF - aktualizacji
author: tdykstra
description: W tym samouczku będziesz aktualizacji powiązanych danych, aktualizując pól klucza obcego i właściwości nawigacji.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 2501f4c4abdadd47b4910909205a5c798f1b938f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a><span data-ttu-id="52fb4-103">Dane — 7 10 dotyczące platformy ASP.NET Core MVC podstawowych EF - aktualizacji</span><span class="sxs-lookup"><span data-stu-id="52fb4-103">ASP.NET Core MVC with EF Core - Update Related Data - 7 of 10</span></span>

<span data-ttu-id="52fb4-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="52fb4-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="52fb4-105">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="52fb4-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="52fb4-106">Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="52fb4-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="52fb4-107">W poprzednich instrukcji wyświetlane powiązanych danych; w tym samouczku będziesz aktualizacji powiązanych danych, aktualizując pól klucza obcego i właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="52fb4-108">Na poniższych ilustracjach przedstawiono niektóre stron, którymi będzie współpracować.</span><span class="sxs-lookup"><span data-stu-id="52fb4-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Kursu edycji strony](update-related-data/_static/course-edit.png)

![Instruktora edycji strony](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="52fb4-111">Dostosowywanie tworzenie i Edycja stron kursów</span><span class="sxs-lookup"><span data-stu-id="52fb4-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="52fb4-112">Po utworzeniu nowego obiektu kursu musi mieć relacji z istniejących działu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="52fb4-113">Aby to ułatwić, kod z utworzonym szkieletem obejmuje metod kontrolera oraz tworzenie i edytowanie widoków, które zawierają listy rozwijanej wyboru działu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="52fb4-114">Ustawia listy rozwijanej `Course.DepartmentID` właściwości klucza obcego, a to wszystko programu Entity Framework wymaga, aby załadować `Department` właściwości nawigacji o odpowiednich jednostek działu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="52fb4-115">Będziesz używać szkieletu kodu, ale nieco na dodawanie obsługi błędów i sortowanie listy rozwijanej zmienić.</span><span class="sxs-lookup"><span data-stu-id="52fb4-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="52fb4-116">W *CoursesController.cs*, usunąć czterech metod tworzenia i edycji i zastąpić je z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="52fb4-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="52fb4-117">Po `Edit` metody HttpPost utworzenie nowej metody, który ładuje informacji działu dla listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="52fb4-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="52fb4-118">`PopulateDepartmentsDropDownList` Metoda pobiera listę wszystkich działach sortowane według nazwy, tworzy `SelectList` kolekcji do listy rozwijanej i przekazuje do widoku w kolekcji `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="52fb4-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="52fb4-119">Metoda przyjmuje opcjonalny `selectedDepartment` parametr, który umożliwia kod wywołujący określić elementu, który będzie wybierany Po wyrenderowaniu listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="52fb4-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="52fb4-120">Widok przekazuje do nazwy "DepartmentID" `<select>` pomocnika tagów i pomocnika następnie traktował Szukaj w `ViewBag` obiekt do `SelectList` o nazwie "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="52fb4-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="52fb4-121">HttpGet `Create` wywołania metody `PopulateDepartmentsDropDownList` metody bez ustawienie wybranego elementu, ponieważ dla nowego kursu działu nie jest jeszcze ustalony:</span><span class="sxs-lookup"><span data-stu-id="52fb4-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="52fb4-122">HttpGet `Edit` metoda ustawia wybrany element na podstawie Identyfikatora działu, który jest już przypisany do kursu edytowany:</span><span class="sxs-lookup"><span data-stu-id="52fb4-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="52fb4-123">Obie metody HttpPost `Create` i `Edit` również obejmować kod, który ustawia wybranego elementu, gdy ich ponownie wyświetlić stronę po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="52fb4-124">Dzięki temu, że po stronie zostanie wyświetlony ponownie, aby wyświetlić komunikat o błędzie, niezależnie od działu wybrano jest wybrane.</span><span class="sxs-lookup"><span data-stu-id="52fb4-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="52fb4-125">Dodaj. AsNoTracking do szczegółów i usuwania metod</span><span class="sxs-lookup"><span data-stu-id="52fb4-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="52fb4-126">Aby zoptymalizować wydajność, szczegóły dotyczące kursu i usuwanie stron, Dodaj `AsNoTracking` odwołuje się `Details` i HttpGet `Delete` metody.</span><span class="sxs-lookup"><span data-stu-id="52fb4-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="52fb4-127">Modyfikowanie widoków kursu</span><span class="sxs-lookup"><span data-stu-id="52fb4-127">Modify the Course views</span></span>

<span data-ttu-id="52fb4-128">W *Views/Courses/Create.cshtml*, dodaj opcję "Wybierz dział" **działu** listy rozwijanej pozycję Zmienianie podpisu z **DepartmentID** do  **Dział**i Dodaj komunikat dotyczący sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="52fb4-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="52fb4-129">W *Views/Courses/Edit.cshtml*, wprowadzić w polu działu, jak w *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="52fb4-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="52fb4-130">Również w *Views/Courses/Edit.cshtml*, Dodaj pole Liczba kursu przed **tytuł** pola.</span><span class="sxs-lookup"><span data-stu-id="52fb4-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="52fb4-131">Ponieważ numer kursu jest to klucz podstawowy, jest on wyświetlany, ale nie można zmienić.</span><span class="sxs-lookup"><span data-stu-id="52fb4-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="52fb4-132">Istnieje już ukryte pole (`<input type="hidden">`) numeru kursu w widoku edycji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="52fb4-133">Dodawanie `<label>` pomocnika tagów nie eliminuje potrzebę stosowania ukryte pola, ponieważ nie powoduje numer kursu mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **zapisać** na **Edytuj** strony.</span><span class="sxs-lookup"><span data-stu-id="52fb4-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="52fb4-134">W *Views/Courses/Delete.cshtml*, Dodaj pole Liczba kursu u góry i Zmień nazwę Dział dział identyfikator.</span><span class="sxs-lookup"><span data-stu-id="52fb4-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="52fb4-135">W *Views/Courses/Details.cshtml*, wprowadzić zmiany sam, jak w *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="52fb4-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="52fb4-136">Kursu stron w teście</span><span class="sxs-lookup"><span data-stu-id="52fb4-136">Test the Course pages</span></span>

<span data-ttu-id="52fb4-137">Uruchom aplikację, wybierz **kursów** , kliknij pozycję **Utwórz nowy**i wprowadź dane dla porach nowe:</span><span class="sxs-lookup"><span data-stu-id="52fb4-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Plan tworzenia strony](update-related-data/_static/course-create.png)

<span data-ttu-id="52fb4-139">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="52fb4-139">Click **Create**.</span></span> <span data-ttu-id="52fb4-140">Z nowego kursu ma na liście zostanie wyświetlona strona kursy indeksu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="52fb4-141">Nazwa działu na liście strony indeksu pochodzi z właściwości nawigacji, przedstawiający został poprawnie ustanowić relacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="52fb4-142">Kliknij przycisk **Edytuj** na kursu na stronie kursy indeksu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Kursu edycji strony](update-related-data/_static/course-edit.png)

<span data-ttu-id="52fb4-144">Zmień dane na stronie, a następnie kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="52fb4-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="52fb4-145">Z danymi zaktualizowanych kursów zostanie wyświetlona strona kursy indeksu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="52fb4-146">Dodaj stronę edycji dla instruktorów</span><span class="sxs-lookup"><span data-stu-id="52fb4-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="52fb4-147">Podczas edytowania rekordu instruktora chcesz była możliwa aktualizacja instruktora office przypisania.</span><span class="sxs-lookup"><span data-stu-id="52fb4-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="52fb4-148">Jednostka Instructor ma relacji jeden do zero lub jeden z jednostką OfficeAssignment, co oznacza, że ma swój kod obsługi w następujących sytuacjach:</span><span class="sxs-lookup"><span data-stu-id="52fb4-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="52fb4-149">Jeśli pierwotnie miał wartość wyczyszczenie przypisania pakietu office, usunąć jednostkę OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="52fb4-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="52fb4-150">Jeśli użytkownik wprowadzi wartość przydziału pakietu office i początkowo był pusty, Utwórz nową jednostkę OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="52fb4-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="52fb4-151">Jeśli użytkownik zmieni wartość przydziału pakietu office, zmień wartość w istniejącej jednostki OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="52fb4-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="52fb4-152">Aktualizacja kontrolera instruktorów</span><span class="sxs-lookup"><span data-stu-id="52fb4-152">Update the Instructors controller</span></span>

<span data-ttu-id="52fb4-153">W *InstructorsController.cs*, Zmień kod w HttpGet `Edit` metodę, tak aby ładuje Jednostka Instructor `OfficeAssignment` właściwość nawigacji i wywołania `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="52fb4-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="52fb4-154">Zastąp HttpPost `Edit` metody z następujący kod, aby obsługiwać aktualizacje przypisania pakietu office:</span><span class="sxs-lookup"><span data-stu-id="52fb4-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="52fb4-155">Kod wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="52fb4-155">The code does the following:</span></span>

-  <span data-ttu-id="52fb4-156">Zmienia nazwę metody, aby `EditPost` ponieważ podpis teraz jest taka sama jak HttpGet `Edit` — metoda ( `ActionName` atrybut określa, że `/Edit/` nadal używany jest adres URL).</span><span class="sxs-lookup"><span data-stu-id="52fb4-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="52fb4-157">Pobiera wczesny ładowania dla bieżącego obiektu instruktora z bazy danych przy użyciu `OfficeAssignment` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="52fb4-158">To jest identyczny jak w HttpGet `Edit` metody.</span><span class="sxs-lookup"><span data-stu-id="52fb4-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="52fb4-159">Aktualizuje pobraną jednostkę instruktora wartościami z integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="52fb4-160">`TryUpdateModel` Przeciążenie umożliwia utworzenie listy dozwolonych właściwości, które chcesz dołączyć.</span><span class="sxs-lookup"><span data-stu-id="52fb4-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="52fb4-161">Pozwala to uniknąć nadmiernego publikowanie zgodnie z objaśnieniem w [drugi samouczek](crud.md).</span><span class="sxs-lookup"><span data-stu-id="52fb4-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="52fb4-162">Jeśli w lokalizacji biura jest puste, ustawia właściwość Instructor.OfficeAssignment równej null, dzięki czemu powiązanego wiersza w tabeli OfficeAssignment zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="52fb4-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="52fb4-163">Zapisuje zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="52fb4-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="52fb4-164">Aktualizacja instruktora Edytuj widok</span><span class="sxs-lookup"><span data-stu-id="52fb4-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="52fb4-165">W *Views/Instructors/Edit.cshtml*, Dodaj nowe pole do edycji lokalizacji biura, przed upływem **zapisać** przycisk:</span><span class="sxs-lookup"><span data-stu-id="52fb4-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="52fb4-166">Uruchom aplikację, wybierz **instruktorów** , a następnie kliknij pozycję **Edytuj** na instruktora.</span><span class="sxs-lookup"><span data-stu-id="52fb4-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="52fb4-167">Zmień **oddział** i kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="52fb4-167">Change the **Office Location** and click **Save**.</span></span>

![Instruktora edycji strony](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="52fb4-169">Na stronie edycji instruktora Dodaj kursu przypisania</span><span class="sxs-lookup"><span data-stu-id="52fb4-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="52fb4-170">Instruktorów może nauczyć dowolną liczbę kursów.</span><span class="sxs-lookup"><span data-stu-id="52fb4-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="52fb4-171">Teraz będzie zwiększenia strony Edytuj instruktora przez dodanie możliwości zmiany przypisania kursu za pomocą grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:</span><span class="sxs-lookup"><span data-stu-id="52fb4-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instruktora edycji strony z kursów](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="52fb4-173">Relacja między jednostkami przebiegu i instruktora jest wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="52fb4-174">Aby dodać lub usunąć relacji, dodawania i usuwania jednostek z zestawu jednostek CourseAssignments sprzężenia i.</span><span class="sxs-lookup"><span data-stu-id="52fb4-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="52fb4-175">Interfejs użytkownika, który umożliwia zmianę które kursy instruktora jest przypisane do to grupa pól wyboru.</span><span class="sxs-lookup"><span data-stu-id="52fb4-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="52fb4-176">Wyświetlane jest pole wyboru dla porach co w bazie danych, a te, które instruktora jest aktualnie przypisana do są zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="52fb4-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="52fb4-177">Użytkownika można zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="52fb4-178">Gdyby większa liczba kursów będzie prawdopodobnie chcesz użyć innej metody prezentacji danych w widoku, ale sama metoda manipulowania jednostki sprzężenia użyje do tworzenia lub usuwania relacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="52fb4-179">Aktualizacja kontrolera instruktorów</span><span class="sxs-lookup"><span data-stu-id="52fb4-179">Update the Instructors controller</span></span>

<span data-ttu-id="52fb4-180">Aby zapewnić dane do widoku listy pól wyboru, użyjesz klasy modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="52fb4-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="52fb4-181">Utwórz *AssignedCourseData.cs* w *SchoolViewModels* folderu i Zamień istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="52fb4-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="52fb4-182">W *InstructorsController.cs*, Zastąp HttpGet `Edit` metodę z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="52fb4-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="52fb4-183">Zmiany zostały wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="52fb4-183">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="52fb4-184">Ten kod dodaje wczesny ładowania dla `Courses` właściwość nawigacji i wywołuje nowe `PopulateAssignedCourseData` metody, aby podać informacje dotyczące używania tablicy pole wyboru `AssignedCourseData` wyświetlić klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="52fb4-185">Kod w `PopulateAssignedCourseData` — metoda odczytuje za pośrednictwem wszystkich jednostek kursu, aby załadować listę kursów przy użyciu klasy modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="52fb4-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="52fb4-186">Dla każdego kursu kodu sprawdza, czy porach występuje w instruktora `Courses` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="52fb4-187">Aby utworzyć wydajne wyszukiwanie podczas sprawdzania, czy kursu jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w `HashSet` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="52fb4-188">`Assigned` Właściwość jest ustawiona na wartość true dla kursów instruktora jest przypisany do.</span><span class="sxs-lookup"><span data-stu-id="52fb4-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="52fb4-189">Widok użyje tej właściwości w celu określenia, które wyboru pola musi być wyświetlane jako wybrane.</span><span class="sxs-lookup"><span data-stu-id="52fb4-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="52fb4-190">Na koniec listy jest przekazywany do widoku w `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="52fb4-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="52fb4-191">Następnie dodaj kod, który jest wykonywany, gdy użytkownik kliknie **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="52fb4-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="52fb4-192">Zastąp `EditPost` metodę z następującym kodem, a następnie dodaj nową metodę, która aktualizuje `Courses` właściwości nawigacji jednostki instruktora.</span><span class="sxs-lookup"><span data-stu-id="52fb4-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="52fb4-193">Podpis metody teraz różni się od HttpGet `Edit` metody, więc zmienia nazwę metody z `EditPost` do `Edit`.</span><span class="sxs-lookup"><span data-stu-id="52fb4-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="52fb4-194">Ponieważ widok nie zawiera kolekcji jednostek kursu, integratora modelu nie można automatycznie zaktualizować `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="52fb4-195">Zamiast integratora modelu do zaktualizowania `CourseAssignments` właściwość nawigacji, można to zrobić w nowym `UpdateInstructorCourses` metody.</span><span class="sxs-lookup"><span data-stu-id="52fb4-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="52fb4-196">Dlatego należy wyłączyć `CourseAssignments` właściwości z powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="52fb4-197">To nie wymaga żadnych zmian do kodu, który wywołuje `TryUpdateModel` ponieważ używasz przeciążenia listę dozwolonych podobnej i `CourseAssignments` nie ma na liście include.</span><span class="sxs-lookup"><span data-stu-id="52fb4-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="52fb4-198">Jeśli nie zostały zaznaczone pola wyboru kod w `UpdateInstructorCourses` inicjuje `CourseAssignments` właściwości nawigacji o pustej kolekcji i zwraca:</span><span class="sxs-lookup"><span data-stu-id="52fb4-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="52fb4-199">Kod, a następnie przetwarza w pętli wszystkich kursów w bazie danych i sprawdza każdego kursu względem nich aktualnie przypisane do instruktora od komputerów, które wybrano w widoku.</span><span class="sxs-lookup"><span data-stu-id="52fb4-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="52fb4-200">W celu ułatwienia wyszukiwania wydajne, ostatnie dwie kolekcje są przechowywane w `HashSet` obiektów.</span><span class="sxs-lookup"><span data-stu-id="52fb4-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="52fb4-201">Jeśli zaznaczono pole wyboru dla porach, ale kursu nie znajduje się w `Instructor.CourseAssignments` właściwości nawigacji kursu zostanie dodany do kolekcji we właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="52fb4-202">Jeśli nie zostało zaznaczone pole wyboru dla porach, ale znajduje się w trakcie `Instructor.CourseAssignments` właściwość nawigacji, kursu zostanie usunięta z właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="52fb4-203">Aktualizowanie widoków instruktora</span><span class="sxs-lookup"><span data-stu-id="52fb4-203">Update the Instructor views</span></span>

<span data-ttu-id="52fb4-204">W *Views/Instructors/Edit.cshtml*, Dodaj **kursy** pole z tablicą pola wyboru przez dodanie poniższego kodu bezpośrednio po `div` elementy **pakietu Office**  pola i przed `div` elementu **zapisać** przycisku.</span><span class="sxs-lookup"><span data-stu-id="52fb4-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="52fb4-205">Po wklejeniu kodu w programie Visual Studio, podziały wiersza zostanie zmieniony w taki sposób, który dzieli kod.</span><span class="sxs-lookup"><span data-stu-id="52fb4-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="52fb4-206">Naciśnij klawisze Ctrl + Z jeden raz, aby cofnąć automatycznego formatowania.</span><span class="sxs-lookup"><span data-stu-id="52fb4-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="52fb4-207">Naprawi podziały wiersza, aby wyglądają tu wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="52fb4-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="52fb4-208">Wcięcie nie musi być idealne, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@:</tr>` linie muszą być w jednym wierszu pokazany lub zostanie wyświetlony błąd w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="52fb4-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="52fb4-209">Z bloku nowy kod zaznaczone naciśnij klawisz Tab trzykrotnie Aby wyrównać nowy kod z istniejącym kodem.</span><span class="sxs-lookup"><span data-stu-id="52fb4-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="52fb4-210">Stan tego problemu można sprawdzić [tutaj](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="52fb4-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="52fb4-211">Ten kod tworzy tabelę HTML, który zawiera trzy kolumny.</span><span class="sxs-lookup"><span data-stu-id="52fb4-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="52fb4-212">W każdej kolumnie ma postać pola wyboru, a po nim tekstem, który składa się z kursu numer i tytuł.</span><span class="sxs-lookup"><span data-stu-id="52fb4-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="52fb4-213">Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), które informuje integrator modelu o tym, że są one traktowane jako grupa.</span><span class="sxs-lookup"><span data-stu-id="52fb4-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="52fb4-214">Atrybut wartość każdego pola wyboru jest ustawiony na wartość `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="52fb4-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="52fb4-215">Gdy strona jest przesyłana, integratora modelu przekazuje tablicy do kontrolera, który składa się z `CourseID` wartości dla pola wyboru, które zostały wybrane.</span><span class="sxs-lookup"><span data-stu-id="52fb4-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="52fb4-216">Początkowo renderowany na pola wyboru, te, które są przypisane do instruktora kursów zaznaczono atrybuty, które wybiera je (wyświetlanych jest zaewidencjonowania je).</span><span class="sxs-lookup"><span data-stu-id="52fb4-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="52fb4-217">Uruchom aplikację, wybierz **instruktorów** , a następnie kliknij pozycję **Edytuj** na instruktora, aby wyświetlić **Edytuj** strony.</span><span class="sxs-lookup"><span data-stu-id="52fb4-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Instruktora edycji strony z kursów](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="52fb4-219">Zmień przypisania pewnych porach i kliknij przycisk Zapisz.</span><span class="sxs-lookup"><span data-stu-id="52fb4-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="52fb4-220">Wprowadzone zmiany zostaną odzwierciedlone na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="52fb4-221">Podejście przyjęte tutaj, aby edytować dane kursu instruktora działa również w przypadku, gdy istnieje ograniczona liczba kursów.</span><span class="sxs-lookup"><span data-stu-id="52fb4-221">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="52fb4-222">Kolekcje, które są znacznie większe innego interfejsu użytkownika i różne metody aktualizacji będą wymagane.</span><span class="sxs-lookup"><span data-stu-id="52fb4-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="52fb4-223">Zaktualizuj strony usuwania</span><span class="sxs-lookup"><span data-stu-id="52fb4-223">Update the Delete page</span></span>

<span data-ttu-id="52fb4-224">W *InstructorsController.cs*, Usuń `DeleteConfirmed` — metoda i wstaw poniższy kod w jego miejscu.</span><span class="sxs-lookup"><span data-stu-id="52fb4-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="52fb4-225">Ten kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="52fb4-225">This code makes the following changes:</span></span>

* <span data-ttu-id="52fb4-226">Eager czy ładowanie dla `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="52fb4-227">Należy to uwzględnić lub EF nie będzie wiedzieć o powiązanych `CourseAssignment` jednostek i nie będzie je usunąć.</span><span class="sxs-lookup"><span data-stu-id="52fb4-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="52fb4-228">Aby uniknąć konieczności je odczytać w tym miejscu możesz może skonfigurować usuwanie kaskadowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="52fb4-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="52fb4-229">Jeśli instruktora do usunięcia jest przypisany jako administrator działy, usuwa przypisanie instruktora z tych działów.</span><span class="sxs-lookup"><span data-stu-id="52fb4-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="52fb4-230">Dodaj lokalizację pakietu office i kursy do tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="52fb4-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="52fb4-231">W *InstructorsController.cs*, Usuń HttpGet i HttpPost `Create` metod, a następnie dodaj następujący kod w ich miejscu:</span><span class="sxs-lookup"><span data-stu-id="52fb4-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="52fb4-232">Ten kod jest podobna do instrukcji dotyczących `Edit` metody z wyjątkiem to początkowo nie kursy są wybrane.</span><span class="sxs-lookup"><span data-stu-id="52fb4-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="52fb4-233">HttpGet `Create` wywołania metody `PopulateAssignedCourseData` — metoda nie może być kursy wybrane, ale w celu Podaj pustą kolekcję dla `foreach` pętli w widoku (w przeciwnym razie kod widoku spowoduje zgłoszenie wyjątku odwołanie o wartości null).</span><span class="sxs-lookup"><span data-stu-id="52fb4-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="52fb4-234">HttpPost `Create` metoda dodaje każdego wybranego kursu do `CourseAssignments` właściwość nawigacji przed sprawdza błędy weryfikacji i dodaje nowe instruktora do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="52fb4-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="52fb4-235">Kursy są dodawane, nawet jeśli istnieją błędy modelu tak, aby po występują błędy modelu (na przykład użytkownik z kluczem usługi nieprawidłową datę), a strona jest ponownie wyświetlony komunikat o błędzie, wprowadzone wybrane opcje kursu automatycznie zostaną przywrócone.</span><span class="sxs-lookup"><span data-stu-id="52fb4-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="52fb4-236">Zwróć uwagę, że aby można było dodać kursy do `CourseAssignments` właściwość nawigacji, należy zainicjować właściwość jako pustej kolekcji:</span><span class="sxs-lookup"><span data-stu-id="52fb4-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="52fb4-237">Alternatywą wobec tej czynności w kodzie kontrolera nakładu pracy w modelu instruktora zmieniając metody pobierającej właściwości do automatycznego tworzenia kolekcji, jeśli nie istnieje, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="52fb4-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

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

<span data-ttu-id="52fb4-238">Jeśli zmodyfikujesz `CourseAssignments` właściwości w ten sposób można usunąć kod inicjujący jawną właściwość w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="52fb4-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="52fb4-239">W *Views/Instructor/Create.cshtml*, Dodaj pole tekstowe lokalizacji pakietu office i sprawdź pola kursów przed przycisku Prześlij.</span><span class="sxs-lookup"><span data-stu-id="52fb4-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="52fb4-240">Jak w przypadku edycji strony [Popraw formatowanie, jeśli program Visual Studio formatuje kod po wklejeniu go](#notepad).</span><span class="sxs-lookup"><span data-stu-id="52fb4-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="52fb4-241">Przetestuj aplikację i tworzenie instruktora.</span><span class="sxs-lookup"><span data-stu-id="52fb4-241">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="52fb4-242">Obsługa transakcji</span><span class="sxs-lookup"><span data-stu-id="52fb4-242">Handling Transactions</span></span>

<span data-ttu-id="52fb4-243">Zgodnie z objaśnieniem w [samouczek CRUD](crud.md), Entity Framework niejawnie implementuje transakcji.</span><span class="sxs-lookup"><span data-stu-id="52fb4-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="52fb4-244">Scenariusze, w którym należy więcej kontrolujesz — na przykład, jeśli chcesz dołączyć operacje wykonywane poza Entity Framework w transakcji — można znaleźć [transakcji](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="52fb4-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="52fb4-245">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="52fb4-245">Summary</span></span>

<span data-ttu-id="52fb4-246">Wprowadzenie do pracy z powiązanych danych zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="52fb4-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="52fb4-247">W następnym samouczku zobaczysz sposób obsługi konfliktom współbieżności.</span><span class="sxs-lookup"><span data-stu-id="52fb4-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="52fb4-248">[Poprzednie](read-related-data.md)
> [dalej](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="52fb4-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
