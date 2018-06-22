---
title: Stron razor podstawowych EF w platformy ASP.NET Core - aktualizacji powiązanych danych - 7, 8
author: rick-anderson
description: W tym samouczku będziesz aktualizacji powiązanych danych, aktualizując pól klucza obcego i właściwości nawigacji.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: e987971f60e5c5a9fb79e30440c7c986df64447e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275297"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="f8971-103">Stron razor podstawowych EF w platformy ASP.NET Core - aktualizacji powiązanych danych - 7, 8</span><span class="sxs-lookup"><span data-stu-id="f8971-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="f8971-104">Przez [Dykstra Tomasz](https://github.com/tdykstra), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8971-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="f8971-105">Ten samouczek pokazuje, aktualizowanie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="f8971-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="f8971-106">Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="f8971-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="f8971-107">Poniższych ilustracjach przedstawiono niektóre ukończone stron.</span><span class="sxs-lookup"><span data-stu-id="f8971-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="f8971-108">![Kursu edycji strony](update-related-data/_static/course-edit.png)
![instruktora edytowanie strony](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="f8971-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="f8971-109">Sprawdź i przetestować, tworzenie i edytowanie ciągu.</span><span class="sxs-lookup"><span data-stu-id="f8971-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="f8971-110">Utwórz nowy plan.</span><span class="sxs-lookup"><span data-stu-id="f8971-110">Create a new course.</span></span> <span data-ttu-id="f8971-111">Dział został wybrany za pomocą klucza podstawowego (liczba całkowita), nie jego nazwy.</span><span class="sxs-lookup"><span data-stu-id="f8971-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="f8971-112">Edytuj nowego kursu.</span><span class="sxs-lookup"><span data-stu-id="f8971-112">Edit the new course.</span></span> <span data-ttu-id="f8971-113">Po zakończeniu testowania Usuń nowego kursu.</span><span class="sxs-lookup"><span data-stu-id="f8971-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="f8971-114">Utwórz klasę podstawową, aby udostępnić typowy kod</span><span class="sxs-lookup"><span data-stu-id="f8971-114">Create a base class to share common code</span></span>

<span data-ttu-id="f8971-115">Kursy/Utwórz kursy i edytowanie strony i każdego muszą listę nazw działów.</span><span class="sxs-lookup"><span data-stu-id="f8971-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="f8971-116">Utwórz *Pages/Courses/DepartmentNamePageModel.cshtml.cs* klasę podstawową dla tworzenie i edytowanie strony:</span><span class="sxs-lookup"><span data-stu-id="f8971-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="f8971-117">Poprzedni kod tworzy [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) zawierać listę nazw działu.</span><span class="sxs-lookup"><span data-stu-id="f8971-117">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="f8971-118">Jeśli `selectedDepartment` określono, że dział został wybrany w `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="f8971-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="f8971-119">Klasy modeli tworzenie i edytowanie strony będzie dziedziczyć `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="f8971-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="f8971-120">Dostosowywanie stron kursy</span><span class="sxs-lookup"><span data-stu-id="f8971-120">Customize the Courses Pages</span></span>

<span data-ttu-id="f8971-121">Po utworzeniu nowego obiektu kursu musi mieć relacji z istniejących działu.</span><span class="sxs-lookup"><span data-stu-id="f8971-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="f8971-122">Dział podczas tworzenia kursu, klasę podstawową dla tworzenie i edytowanie zawiera listy rozwijanej wyboru działu.</span><span class="sxs-lookup"><span data-stu-id="f8971-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="f8971-123">Ustawia listy rozwijanej `Course.DepartmentID` właściwości klucza obcego (klucz OBCY).</span><span class="sxs-lookup"><span data-stu-id="f8971-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="f8971-124">Używa EF Core `Course.DepartmentID` klucz OBCY załadować `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="f8971-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Utwórz plan](update-related-data/_static/ddl.png)

<span data-ttu-id="f8971-126">Aktualizacja modelu strony Utwórz następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f8971-126">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="f8971-127">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="f8971-127">The preceding code:</span></span>

* <span data-ttu-id="f8971-128">Pochodną `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="f8971-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="f8971-129">Używa `TryUpdateModelAsync` zapobiegające [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="f8971-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="f8971-130">Zastępuje `ViewData["DepartmentID"]` z `DepartmentNameSL` (od klasy podstawowej).</span><span class="sxs-lookup"><span data-stu-id="f8971-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="f8971-131">`ViewData["DepartmentID"]` zastępuje z silnie typizowaną `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="f8971-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="f8971-132">Modele jednoznacznie są preferowane za pośrednictwem lekko wpisany.</span><span class="sxs-lookup"><span data-stu-id="f8971-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="f8971-133">Aby uzyskać więcej informacji, zobacz [lekko typu danych (ViewData i obiekt ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="f8971-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="f8971-134">Zaktualizuj strony tworzenia kursów</span><span class="sxs-lookup"><span data-stu-id="f8971-134">Update the Courses Create page</span></span>

<span data-ttu-id="f8971-135">Aktualizacja *Pages/Courses/Create.cshtml* z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="f8971-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="f8971-136">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="f8971-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="f8971-137">Zmienia podpisu z **DepartmentID** do **działu**.</span><span class="sxs-lookup"><span data-stu-id="f8971-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="f8971-138">Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (od klasy podstawowej).</span><span class="sxs-lookup"><span data-stu-id="f8971-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="f8971-139">Dodaje opcję "Wybierz dział".</span><span class="sxs-lookup"><span data-stu-id="f8971-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="f8971-140">Ta zmiana powoduje "Wybierz dział" zamiast pierwszy działu.</span><span class="sxs-lookup"><span data-stu-id="f8971-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="f8971-141">Dodaje komunikat dotyczący sprawdzania poprawności, gdy dział nie jest wybrany.</span><span class="sxs-lookup"><span data-stu-id="f8971-141">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="f8971-142">Strona Razor używa [wybierz pomocnika tagów](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="f8971-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="f8971-143">Przetestuj tworzenia strony.</span><span class="sxs-lookup"><span data-stu-id="f8971-143">Test the Create page.</span></span> <span data-ttu-id="f8971-144">Utwórz stronę Wyświetla nazwę działu zamiast identyfikatora działu.</span><span class="sxs-lookup"><span data-stu-id="f8971-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="f8971-145">Zaktualizuj strony Edytuj kursy.</span><span class="sxs-lookup"><span data-stu-id="f8971-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="f8971-146">Aktualizacja modelu strony Edytuj następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f8971-146">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="f8971-147">Zmiany są podobne do tych wprowadzone w modelu strony Utwórz.</span><span class="sxs-lookup"><span data-stu-id="f8971-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="f8971-148">W powyższym kodzie `PopulateDepartmentsDropDownList` przebiegów w identyfikatorze działu, które dział określonych na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="f8971-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="f8971-149">Aktualizacja *Pages/Courses/Edit.cshtml* z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="f8971-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="f8971-150">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="f8971-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="f8971-151">Wyświetla identyfikator kursu.</span><span class="sxs-lookup"><span data-stu-id="f8971-151">Displays the course ID.</span></span> <span data-ttu-id="f8971-152">Zazwyczaj nie są wyświetlane podstawowego klucza (PK) z jednostki.</span><span class="sxs-lookup"><span data-stu-id="f8971-152">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="f8971-153">PKs są zazwyczaj bezużyteczne dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="f8971-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="f8971-154">W takim przypadku PK jest to liczba kursu.</span><span class="sxs-lookup"><span data-stu-id="f8971-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="f8971-155">Zmienia podpisu z **DepartmentID** do **działu**.</span><span class="sxs-lookup"><span data-stu-id="f8971-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="f8971-156">Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (od klasy podstawowej).</span><span class="sxs-lookup"><span data-stu-id="f8971-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="f8971-157">Strona zawiera pola ukrytego (`<input type="hidden">`) dla porach numeru.</span><span class="sxs-lookup"><span data-stu-id="f8971-157">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="f8971-158">Dodawanie `<label>` pomocnika za pomocą tagów `asp-for="Course.CourseID"` nie eliminuje potrzebę stosowania ukryte pole.</span><span class="sxs-lookup"><span data-stu-id="f8971-158">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="f8971-159">`<input type="hidden">` jest wymagany dla numeru kursu mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="f8971-159">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="f8971-160">Przetestuj zaktualizowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="f8971-160">Test the updated code.</span></span> <span data-ttu-id="f8971-161">Tworzenie, edytowanie i usuwanie kursu.</span><span class="sxs-lookup"><span data-stu-id="f8971-161">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="f8971-162">Dodaj AsNoTracking do szczegółów i usuń modele strony</span><span class="sxs-lookup"><span data-stu-id="f8971-162">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="f8971-163">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) może poprawić wydajność podczas śledzenia nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="f8971-163">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="f8971-164">Dodaj `AsNoTracking` do modelu strony Delete i szczegóły.</span><span class="sxs-lookup"><span data-stu-id="f8971-164">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="f8971-165">Poniższy kod przedstawia w zaktualizowanym modelu. Usuń strony:</span><span class="sxs-lookup"><span data-stu-id="f8971-165">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="f8971-166">Aktualizacja `OnGetAsync` metody w *Pages/Courses/Details.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="f8971-166">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="f8971-167">Modyfikowanie stron Delete i szczegóły</span><span class="sxs-lookup"><span data-stu-id="f8971-167">Modify the Delete and Details pages</span></span>

<span data-ttu-id="f8971-168">Zaktualizuj strony Usuń Razor o następujący kod:</span><span class="sxs-lookup"><span data-stu-id="f8971-168">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="f8971-169">Wprowadzenie identycznych zmian do strony szczegółów.</span><span class="sxs-lookup"><span data-stu-id="f8971-169">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="f8971-170">Kursu stron w teście</span><span class="sxs-lookup"><span data-stu-id="f8971-170">Test the Course pages</span></span>

<span data-ttu-id="f8971-171">Test tworzenia, edytowania, uzyskać szczegółowe informacje i usuwania.</span><span class="sxs-lookup"><span data-stu-id="f8971-171">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="f8971-172">Aktualizowanie stron instruktora</span><span class="sxs-lookup"><span data-stu-id="f8971-172">Update the instructor pages</span></span>

<span data-ttu-id="f8971-173">Poniższe sekcje aktualizacji instruktora stron.</span><span class="sxs-lookup"><span data-stu-id="f8971-173">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="f8971-174">Dodaj lokalizację pakietu office</span><span class="sxs-lookup"><span data-stu-id="f8971-174">Add office location</span></span>

<span data-ttu-id="f8971-175">Edytowanie rekordu instruktora, można zaktualizować przypisania office instruktora.</span><span class="sxs-lookup"><span data-stu-id="f8971-175">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="f8971-176">`Instructor` Jednostka ma relacji jeden do zero lub jeden z `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="f8971-176">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="f8971-177">Kod instruktora musi obsługiwać:</span><span class="sxs-lookup"><span data-stu-id="f8971-177">The instructor code must handle:</span></span>

* <span data-ttu-id="f8971-178">Jeśli użytkownik usuwa przypisanie pakietu office, Usuń `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="f8971-178">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="f8971-179">Jeśli użytkownik wprowadzi przypisania pakietu office i był on pusty, Utwórz nową `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="f8971-179">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="f8971-180">Jeśli użytkownik zmieni przypisania pakietu office, należy zaktualizować `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="f8971-180">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="f8971-181">Aktualizacja modelu strony Edytuj instruktorów następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f8971-181">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="f8971-182">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="f8971-182">The preceding code:</span></span>

- <span data-ttu-id="f8971-183">Pobiera bieżący `Instructor` jednostki z bazy danych przy użyciu wczesny ładowania dla `OfficeAssignment` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="f8971-183">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="f8971-184">Aktualizuje pobranej `Instructor` jednostki wartościami z integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="f8971-184">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="f8971-185">`TryUpdateModel` Zapobiega [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="f8971-185">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="f8971-186">Jeśli w lokalizacji biura jest puste, ustawia `Instructor.OfficeAssignment` na wartość null.</span><span class="sxs-lookup"><span data-stu-id="f8971-186">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="f8971-187">Gdy `Instructor.OfficeAssignment` jest null, powiązane wiersza w `OfficeAssignment` usunięcia tabeli.</span><span class="sxs-lookup"><span data-stu-id="f8971-187">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="f8971-188">Zaktualizuj instruktora edycji strony</span><span class="sxs-lookup"><span data-stu-id="f8971-188">Update the instructor Edit page</span></span>

<span data-ttu-id="f8971-189">Aktualizacja *Pages/Instructors/Edit.cshtml* z lokalizacji pakietu office:</span><span class="sxs-lookup"><span data-stu-id="f8971-189">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="f8971-190">Sprawdź, czy można zmienić lokalizacji biura instruktorów.</span><span class="sxs-lookup"><span data-stu-id="f8971-190">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="f8971-191">Na stronie edycji instruktora Dodaj kursu przypisania</span><span class="sxs-lookup"><span data-stu-id="f8971-191">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="f8971-192">Instruktorów może nauczyć dowolną liczbę kursów.</span><span class="sxs-lookup"><span data-stu-id="f8971-192">Instructors may teach any number of courses.</span></span> <span data-ttu-id="f8971-193">W tej sekcji możesz dodać możliwości zmiany przypisania kursu.</span><span class="sxs-lookup"><span data-stu-id="f8971-193">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="f8971-194">Na poniższej ilustracji przedstawiono zaktualizowane instruktora edycji strony:</span><span class="sxs-lookup"><span data-stu-id="f8971-194">The following image shows the updated instructor Edit page:</span></span>

![Instruktora edycji strony z kursów](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="f8971-196">`Course` i `Instructor` ma relację wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="f8971-196">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="f8971-197">Aby dodać lub usunąć relacji, dodawania i usuwania jednostek z `CourseAssignments` Dołącz do zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="f8971-197">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="f8971-198">Pola wyboru Włącz zmiany kursów przydzielonej instruktora.</span><span class="sxs-lookup"><span data-stu-id="f8971-198">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="f8971-199">Pole wyboru jest wyświetlane dla porach co w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8971-199">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="f8971-200">Kursy przypisane instruktora są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="f8971-200">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="f8971-201">Użytkownika można zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu.</span><span class="sxs-lookup"><span data-stu-id="f8971-201">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="f8971-202">Jeśli liczba kursów była większa:</span><span class="sxs-lookup"><span data-stu-id="f8971-202">If the number of courses were much greater:</span></span>

* <span data-ttu-id="f8971-203">Interfejs inny użytkownik użyje prawdopodobnie do wyświetlenia kursów.</span><span class="sxs-lookup"><span data-stu-id="f8971-203">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="f8971-204">Metoda manipulowania jednostki sprzężenia, można utworzyć ani usunąć relacji nie zmieniać.</span><span class="sxs-lookup"><span data-stu-id="f8971-204">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="f8971-205">Dodawanie klasy do obsługi tworzyć i edytować instruktora strony</span><span class="sxs-lookup"><span data-stu-id="f8971-205">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="f8971-206">Utwórz *SchoolViewModels/AssignedCourseData.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f8971-206">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="f8971-207">`AssignedCourseData` Klasa zawiera danych, aby utworzyć pól wyboru dla przypisanych kursy przez instruktora.</span><span class="sxs-lookup"><span data-stu-id="f8971-207">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="f8971-208">Utwórz *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* klasy podstawowej:</span><span class="sxs-lookup"><span data-stu-id="f8971-208">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="f8971-209">`InstructorCoursesPageModel` Jest klasą podstawową będzie używane do edycji i tworzenia modeli strony.</span><span class="sxs-lookup"><span data-stu-id="f8971-209">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="f8971-210">`PopulateAssignedCourseData` odczytuje wszystkie `Course` jednostek do wypełnienia `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="f8971-210">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="f8971-211">Dla każdego kursu kod ustawia `CourseID`, tytuł i czy nie jest przypisany do kursu instruktora.</span><span class="sxs-lookup"><span data-stu-id="f8971-211">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="f8971-212">A [zestaw HashSet](/dotnet/api/system.collections.generic.hashset-1) służy do tworzenia wydajne wyszukiwań.</span><span class="sxs-lookup"><span data-stu-id="f8971-212">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="f8971-213">Instruktorów edycji strony modelu</span><span class="sxs-lookup"><span data-stu-id="f8971-213">Instructors Edit page model</span></span>

<span data-ttu-id="f8971-214">Aktualizacja modelu strony Edytuj instruktora następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f8971-214">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="f8971-215">Poprzedni kod obsługi zmian w przypisaniu pakietu office.</span><span class="sxs-lookup"><span data-stu-id="f8971-215">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="f8971-216">Aktualizacja instruktora widoku Razor:</span><span class="sxs-lookup"><span data-stu-id="f8971-216">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="f8971-217">Po wklejeniu kodu w programie Visual Studio, podziały wiersza są zmieniane w taki sposób, który dzieli kod.</span><span class="sxs-lookup"><span data-stu-id="f8971-217">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="f8971-218">Naciśnij klawisze Ctrl + Z jeden raz, aby cofnąć automatycznego formatowania.</span><span class="sxs-lookup"><span data-stu-id="f8971-218">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="f8971-219">CTRL + Z rozwiązuje podziały wierszy, dzięki czemu wyglądają tu wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="f8971-219">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="f8971-220">Wcięcie nie musi być idealne, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@:</tr>` linie muszą być w jednym wierszu jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="f8971-220">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="f8971-221">Z bloku nowy kod zaznaczone naciśnij klawisz Tab trzykrotnie Aby wyrównać nowy kod z istniejącym kodem.</span><span class="sxs-lookup"><span data-stu-id="f8971-221">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="f8971-222">Głosowania lub sprawdzić stan tej usterki [z tym łączem](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="f8971-222">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="f8971-223">Poprzedni kod tworzy tabelę HTML, który zawiera trzy kolumny.</span><span class="sxs-lookup"><span data-stu-id="f8971-223">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="f8971-224">Każda kolumna ma postać pola wyboru i podpis zawierający kursu numer i tytuł.</span><span class="sxs-lookup"><span data-stu-id="f8971-224">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="f8971-225">Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="f8971-225">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="f8971-226">Przy użyciu tej samej nazwie informuje integratora modelu je traktować jako grupa.</span><span class="sxs-lookup"><span data-stu-id="f8971-226">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="f8971-227">Ustawiono atrybut wartość każdego pola wyboru `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="f8971-227">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="f8971-228">Gdy strona jest przesyłana, integratora modelu przekazuje tablicę, która składa się z `CourseID` wartości dla pola wyboru, które są wybrane.</span><span class="sxs-lookup"><span data-stu-id="f8971-228">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="f8971-229">Gdy pola wyboru początkowo są renderowane, kursy przypisane do instruktora zaznaczono atrybutów.</span><span class="sxs-lookup"><span data-stu-id="f8971-229">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="f8971-230">Uruchom aplikację i przetestować zaktualizowane instruktorów edycji strony.</span><span class="sxs-lookup"><span data-stu-id="f8971-230">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="f8971-231">Zmiana przypisania niektórych kursu.</span><span class="sxs-lookup"><span data-stu-id="f8971-231">Change some course assignments.</span></span> <span data-ttu-id="f8971-232">Zmiany zostaną odzwierciedlone na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="f8971-232">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="f8971-233">Uwaga: w tym miejscu podejście do edytowania danych kursu instruktora dobrze działa w przypadku istnieje ograniczona liczba kursów.</span><span class="sxs-lookup"><span data-stu-id="f8971-233">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="f8971-234">Kolekcje, które są znacznie większe innego interfejsu użytkownika i różne metody aktualizacji będzie bardziej użyteczny i efektywny.</span><span class="sxs-lookup"><span data-stu-id="f8971-234">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="f8971-235">Aktualizacja instruktorów tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="f8971-235">Update the instructors Create page</span></span>

<span data-ttu-id="f8971-236">Aktualizacja modelu strony Utwórz instruktora następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f8971-236">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="f8971-237">Poprzedni kod jest podobny do *Pages/Instructors/Edit.cshtml.cs* kodu.</span><span class="sxs-lookup"><span data-stu-id="f8971-237">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="f8971-238">Zaktualizuj strony tworzenia Razor instruktora o następujący kod:</span><span class="sxs-lookup"><span data-stu-id="f8971-238">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="f8971-239">Przetestuj instruktora tworzenia strony.</span><span class="sxs-lookup"><span data-stu-id="f8971-239">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="f8971-240">Zaktualizuj strony usuwania</span><span class="sxs-lookup"><span data-stu-id="f8971-240">Update the Delete page</span></span>

<span data-ttu-id="f8971-241">Aktualizacja modelu strony Usuń z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f8971-241">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="f8971-242">Poprzedni kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="f8971-242">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="f8971-243">Używa wczesny ładowania dla `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="f8971-243">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="f8971-244">`CourseAssignments` muszą być włączone lub nie są one usuwane po usunięciu instruktora.</span><span class="sxs-lookup"><span data-stu-id="f8971-244">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="f8971-245">Aby uniknąć konieczności je odczytać, należy skonfigurować usuwanie kaskadowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8971-245">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="f8971-246">Jeśli instruktora do usunięcia jest przypisany jako administrator działy, usuwa przypisanie instruktora z tych działów.</span><span class="sxs-lookup"><span data-stu-id="f8971-246">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8971-247">[Poprzednie](xref:data/ef-rp/read-related-data)
> [dalej](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="f8971-247">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
