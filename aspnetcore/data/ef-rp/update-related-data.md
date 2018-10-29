---
title: Strony razor z programem EF Core w programie ASP.NET Core — aktualizowanie powiązanych danych - 7, 8
author: rick-anderson
description: W tym samouczku będziesz aktualizowanie powiązanych danych, aktualizując pola kluczy obcych i właściwości nawigacji.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 4306118240c052585a5c2eeb2053ce03534b547c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207546"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="ab57a-103">Strony razor z programem EF Core w programie ASP.NET Core — aktualizowanie powiązanych danych - 7, 8</span><span class="sxs-lookup"><span data-stu-id="ab57a-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="ab57a-104">Przez [Tom Dykstra](https://github.com/tdykstra), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab57a-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="ab57a-105">Ten samouczek pokazuje, aktualizowanie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="ab57a-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="ab57a-106">Jeśli napotkasz problemy, nie można rozwiązać, [pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="ab57a-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="ab57a-107">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="ab57a-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="ab57a-108">Poniższa ilustracja przedstawia niektóre strony ukończone.</span><span class="sxs-lookup"><span data-stu-id="ab57a-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="ab57a-109">![Strona edytowania kurs](update-related-data/_static/course-edit.png)
![przez instruktorów edytowanie strony](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="ab57a-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="ab57a-110">Sprawdź, aby ją przetestować, tworzyć i edytować kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="ab57a-111">Tworzenie nowego kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-111">Create a new course.</span></span> <span data-ttu-id="ab57a-112">Dział jest wybierany za pomocą klucza podstawowego (liczba całkowita), nie jego nazwę.</span><span class="sxs-lookup"><span data-stu-id="ab57a-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="ab57a-113">Edytowanie nowego kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-113">Edit the new course.</span></span> <span data-ttu-id="ab57a-114">Po zakończeniu testowania Usuń nowego kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="ab57a-115">Utwórz klasę bazową, aby udostępnić wspólny kod</span><span class="sxs-lookup"><span data-stu-id="ab57a-115">Create a base class to share common code</span></span>

<span data-ttu-id="ab57a-116">Kursy/tworzenia kursy/edytowanie strony i każdy muszą listę nazw działów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="ab57a-117">Utwórz *Pages/Courses/DepartmentNamePageModel.cshtml.cs* klasa podstawowa dla stron tworzyć i edytować:</span><span class="sxs-lookup"><span data-stu-id="ab57a-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="ab57a-118">Powyższy kod tworzy [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) zawierać listę nazw działów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-118">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="ab57a-119">Jeśli `selectedDepartment` określono wybrano działu `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="ab57a-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="ab57a-120">Tworzenie i edytowanie klas modelu strony będzie dziedziczyć `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="ab57a-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="ab57a-121">Dostosowywanie stron kursów</span><span class="sxs-lookup"><span data-stu-id="ab57a-121">Customize the Courses Pages</span></span>

<span data-ttu-id="ab57a-122">Po utworzeniu nowej jednostki kurs, musi on mieć relacji do istniejących działu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="ab57a-123">Dział podczas tworzenia kurs, klasa bazowa dla tworzyć i edytować zawiera listy rozwijanej, służąca do wybierania działu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="ab57a-124">Z listy rozwijanej zestawów `Course.DepartmentID` właściwości klucza obcego (klucz OBCY).</span><span class="sxs-lookup"><span data-stu-id="ab57a-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="ab57a-125">Używa programu EF Core `Course.DepartmentID` klucza Obcego załadować `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ab57a-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Utwórz kurs](update-related-data/_static/ddl.png)

<span data-ttu-id="ab57a-127">Aktualizowanie modelu strony Utwórz następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-127">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="ab57a-128">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="ab57a-128">The preceding code:</span></span>

* <span data-ttu-id="ab57a-129">Pochodzi od klasy `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="ab57a-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="ab57a-130">Używa `TryUpdateModelAsync` zapobiegające [polegającymi](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="ab57a-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="ab57a-131">Zastępuje `ViewData["DepartmentID"]` z `DepartmentNameSL` (z klasy bazowej).</span><span class="sxs-lookup"><span data-stu-id="ab57a-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="ab57a-132">`ViewData["DepartmentID"]` zastępuje z silnie typizowaną `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="ab57a-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="ab57a-133">Silnie typizowane modeli są preferowane przez słabo wpisane.</span><span class="sxs-lookup"><span data-stu-id="ab57a-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="ab57a-134">Aby uzyskać więcej informacji, zobacz [słabo wpisanych danych (z podejścia ViewData i obiekt ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="ab57a-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="ab57a-135">Aktualizacja kursy — Tworzenie strony</span><span class="sxs-lookup"><span data-stu-id="ab57a-135">Update the Courses Create page</span></span>

<span data-ttu-id="ab57a-136">Aktualizacja *Pages/Courses/Create.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="ab57a-137">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="ab57a-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="ab57a-138">Zmienia podpis z **DepartmentID** do **działu**.</span><span class="sxs-lookup"><span data-stu-id="ab57a-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="ab57a-139">Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (z klasy bazowej).</span><span class="sxs-lookup"><span data-stu-id="ab57a-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="ab57a-140">Dodaje opcję "Wybierz dział".</span><span class="sxs-lookup"><span data-stu-id="ab57a-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="ab57a-141">Ta zmiana powoduje wyświetlenie "Wybierz dział" zamiast pierwszy działu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="ab57a-142">Dodaje komunikat dotyczący sprawdzania poprawności, gdy dział nie jest wybrany.</span><span class="sxs-lookup"><span data-stu-id="ab57a-142">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="ab57a-143">Strona Razor używa [wybierz Pomocnik tagu](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="ab57a-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="ab57a-144">Przetestuj strona tworzenia.</span><span class="sxs-lookup"><span data-stu-id="ab57a-144">Test the Create page.</span></span> <span data-ttu-id="ab57a-145">Strona tworzenia Wyświetla nazwę działu, a nie identyfikatora działu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="ab57a-146">Zaktualizuj strony Edytuj kursów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="ab57a-147">Aktualizowanie modelu strony edycji z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-147">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="ab57a-148">Zmiany są podobne do wprowadzonych w modelu strony Utwórz.</span><span class="sxs-lookup"><span data-stu-id="ab57a-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="ab57a-149">W poprzednim kodzie `PopulateDepartmentsDropDownList` przebiegów w identyfikatorze działu, które dział określonych na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="ab57a-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="ab57a-150">Aktualizacja *Pages/Courses/Edit.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="ab57a-151">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="ab57a-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="ab57a-152">Wyświetla identyfikator kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-152">Displays the course ID.</span></span> <span data-ttu-id="ab57a-153">Ogólnie nie jest wyświetlany podstawowego klucza (PK) z jednostki.</span><span class="sxs-lookup"><span data-stu-id="ab57a-153">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="ab57a-154">PKs są zazwyczaj znaczenia dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ab57a-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="ab57a-155">W tym przypadku PK jest to liczba kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="ab57a-156">Zmienia podpis z **DepartmentID** do **działu**.</span><span class="sxs-lookup"><span data-stu-id="ab57a-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="ab57a-157">Zastępuje `"ViewBag.DepartmentID"` z `DepartmentNameSL` (z klasy bazowej).</span><span class="sxs-lookup"><span data-stu-id="ab57a-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="ab57a-158">Ta strona zawiera pole ukryte (`<input type="hidden">`) dla liczby kursów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-158">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="ab57a-159">Dodawanie `<label>` tag Pomocnik ze `asp-for="Course.CourseID"` nie eliminuje potrzebę stosowania ukryte pole.</span><span class="sxs-lookup"><span data-stu-id="ab57a-159">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="ab57a-160">`<input type="hidden">` jest wymagany dla numeru kurs, które mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="ab57a-160">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="ab57a-161">Przetestuj zaktualizowany kod.</span><span class="sxs-lookup"><span data-stu-id="ab57a-161">Test the updated code.</span></span> <span data-ttu-id="ab57a-162">Tworzenie, edytowanie i usuwanie kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-162">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="ab57a-163">Dodaj AsNoTracking do szczegółów i usuwania modeli strony</span><span class="sxs-lookup"><span data-stu-id="ab57a-163">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="ab57a-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) może poprawić wydajność, gdy śledzenie nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="ab57a-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="ab57a-165">Dodaj `AsNoTracking` z modelem strony Delete i szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="ab57a-165">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="ab57a-166">Poniższy kod pokazuje aktualizowanego modelu strony usuwania:</span><span class="sxs-lookup"><span data-stu-id="ab57a-166">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="ab57a-167">Aktualizacja `OnGetAsync` method in Class metoda *Pages/Courses/Details.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="ab57a-167">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="ab57a-168">Modyfikowanie stron Delete i szczegóły</span><span class="sxs-lookup"><span data-stu-id="ab57a-168">Modify the Delete and Details pages</span></span>

<span data-ttu-id="ab57a-169">Zaktualizuj strony Razor Usuń następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-169">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="ab57a-170">Wprowadzenie identycznych zmian do strony szczegółów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-170">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="ab57a-171">Testowanie stron kursu</span><span class="sxs-lookup"><span data-stu-id="ab57a-171">Test the Course pages</span></span>

<span data-ttu-id="ab57a-172">Test tworzenia, edytowania szczegółów i usuwania.</span><span class="sxs-lookup"><span data-stu-id="ab57a-172">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="ab57a-173">Aktualizowanie stron przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="ab57a-173">Update the instructor pages</span></span>

<span data-ttu-id="ab57a-174">Poniższe sekcje aktualizacji stron instruktora.</span><span class="sxs-lookup"><span data-stu-id="ab57a-174">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="ab57a-175">Dodaj lokalizację pakietu office</span><span class="sxs-lookup"><span data-stu-id="ab57a-175">Add office location</span></span>

<span data-ttu-id="ab57a-176">Podczas edytowania rekordu przez instruktorów, można zaktualizować przez instruktorów biuro.</span><span class="sxs-lookup"><span data-stu-id="ab57a-176">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="ab57a-177">`Instructor` Jednostka ma relacji jeden do zero lub jeden z `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="ab57a-177">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="ab57a-178">Kod przez instruktorów musi obsługiwać:</span><span class="sxs-lookup"><span data-stu-id="ab57a-178">The instructor code must handle:</span></span>

* <span data-ttu-id="ab57a-179">Jeśli użytkownik usunie zaznaczenie przypisania pakietu office, Usuń `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="ab57a-179">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="ab57a-180">Jeśli użytkownik wprowadzi biuro i był on pusty, Utwórz nowe `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="ab57a-180">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="ab57a-181">Jeśli użytkownik zmieni przypisania pakietu office, należy zaktualizować `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="ab57a-181">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="ab57a-182">Aktualizowanie modelu strony edytowania Instruktorzy z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-182">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="ab57a-183">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="ab57a-183">The preceding code:</span></span>

- <span data-ttu-id="ab57a-184">Pobiera bieżący `Instructor` jednostki z bazy danych przy użyciu wczesne ładowanie dla `OfficeAssignment` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ab57a-184">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="ab57a-185">Aktualizuje pobrany `Instructor` jednostki z wartościami z integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="ab57a-186">`TryUpdateModel` Zapobiega [polegającymi](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="ab57a-186">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="ab57a-187">Jeśli lokalizacja pakietu office jest pusta, ustawia `Instructor.OfficeAssignment` na wartość null.</span><span class="sxs-lookup"><span data-stu-id="ab57a-187">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="ab57a-188">Gdy `Instructor.OfficeAssignment` jest null, powiązane wiersza w `OfficeAssignment` tabeli zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="ab57a-188">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="ab57a-189">Zaktualizuj strony edytowania przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="ab57a-189">Update the instructor Edit page</span></span>

<span data-ttu-id="ab57a-190">Aktualizacja *Pages/Instructors/Edit.cshtml* z lokalizacji biura:</span><span class="sxs-lookup"><span data-stu-id="ab57a-190">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="ab57a-191">Sprawdź, czy można zmienić lokalizacji biura instruktorów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-191">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="ab57a-192">Dodaj kurs przypisania do strony edytowania przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="ab57a-192">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="ab57a-193">Instruktorzy może nauczyć dowolnej liczby kursów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-193">Instructors may teach any number of courses.</span></span> <span data-ttu-id="ab57a-194">W tej sekcji dodasz możliwość zmiany przypisania kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-194">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="ab57a-195">Na poniższej ilustracji przedstawiono zaktualizowane przez instruktorów strony edytowania:</span><span class="sxs-lookup"><span data-stu-id="ab57a-195">The following image shows the updated instructor Edit page:</span></span>

![Strona edytowania przez instruktorów dzięki kursom](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="ab57a-197">`Course` i `Instructor` jest w relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-197">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="ab57a-198">Aby dodawać i usuwać relacje, w przypadku dodawania i usuwania jednostek z `CourseAssignments` Dołącz do zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="ab57a-198">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="ab57a-199">Pola wyboru Włącz zmiany do kursów, przypisanego pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="ab57a-199">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="ab57a-200">Pole wyboru jest wyświetlane dla każdego kursu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ab57a-200">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="ab57a-201">Kursy, przypisanych do instruktora są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="ab57a-201">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="ab57a-202">Użytkownik może zaznacz lub wyczyść pola wyboru, aby zmienić przypisania kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-202">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="ab57a-203">Gdyby znacznie większą liczbę kursy:</span><span class="sxs-lookup"><span data-stu-id="ab57a-203">If the number of courses were much greater:</span></span>

* <span data-ttu-id="ab57a-204">Interfejs inny użytkownik będzie prawdopodobnie umożliwia wyświetlenie kursy.</span><span class="sxs-lookup"><span data-stu-id="ab57a-204">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="ab57a-205">W takich sytuacjach przydałaby zmienić metodę manipulowanie jednostki sprzężenia, można utworzyć ani usunąć relacji.</span><span class="sxs-lookup"><span data-stu-id="ab57a-205">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="ab57a-206">Dodawanie klasy do obsługi tworzenia i edycji stron przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="ab57a-206">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="ab57a-207">Tworzenie *SchoolViewModels/AssignedCourseData.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-207">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="ab57a-208">`AssignedCourseData` Klasa zawiera danych, aby utworzyć pól wyboru dla przypisanych kursy przez instruktora.</span><span class="sxs-lookup"><span data-stu-id="ab57a-208">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="ab57a-209">Tworzenie *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* klasa bazowa:</span><span class="sxs-lookup"><span data-stu-id="ab57a-209">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="ab57a-210">`InstructorCoursesPageModel` Jest klasą bazową, będzie używane do edycji i tworzyć modele strony.</span><span class="sxs-lookup"><span data-stu-id="ab57a-210">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="ab57a-211">`PopulateAssignedCourseData` odczytuje wszystkie `Course` jednostek, aby wypełnić `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="ab57a-211">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="ab57a-212">Dla każdego kursu kod ustawia `CourseID`, tytuł i czy nie jest przypisany do kursu instruktora.</span><span class="sxs-lookup"><span data-stu-id="ab57a-212">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="ab57a-213">A [hashset —](/dotnet/api/system.collections.generic.hashset-1) służy do tworzenia wyszukiwań wydajne.</span><span class="sxs-lookup"><span data-stu-id="ab57a-213">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="ab57a-214">Instruktorzy edycji strony modelu</span><span class="sxs-lookup"><span data-stu-id="ab57a-214">Instructors Edit page model</span></span>

<span data-ttu-id="ab57a-215">Aktualizowanie modelu strony edycji przez instruktorów, z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-215">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="ab57a-216">Powyższy kod obsługuje zmiany przypisania pakietu office.</span><span class="sxs-lookup"><span data-stu-id="ab57a-216">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="ab57a-217">Aktualizacja instruktora widoku Razor:</span><span class="sxs-lookup"><span data-stu-id="ab57a-217">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="ab57a-218">Po wklejeniu kodu w programie Visual Studio podziały wierszy zostały zmienione w taki sposób, że przerwanie wykonywania kodu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-218">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="ab57a-219">Naciśnij klawisze Ctrl + Z jeden raz, aby cofnąć, automatycznego formatowania.</span><span class="sxs-lookup"><span data-stu-id="ab57a-219">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="ab57a-220">CTRL + Z rozwiązuje podziałów wiersza, aby wyglądają jak tutaj zobaczyć.</span><span class="sxs-lookup"><span data-stu-id="ab57a-220">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="ab57a-221">Wcięcie nie musi być idealna, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@:</tr>` linie muszą być w jednym wierszu jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="ab57a-221">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="ab57a-222">Za pomocą bloku wybrano nowego kodu naciśnij klawisz Tab, trzy razy na wiersz w górę nowego kodu przy użyciu istniejącego kodu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-222">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="ab57a-223">Sprawdź stan tej usterki lub Głosuj na [za pomocą tego łącza](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="ab57a-223">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="ab57a-224">Powyższy kod tworzy tabelę HTML, który ma trzy kolumny.</span><span class="sxs-lookup"><span data-stu-id="ab57a-224">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="ab57a-225">Każda kolumna ma postać pola wyboru i podpis zawierający kurs numer i tytuł.</span><span class="sxs-lookup"><span data-stu-id="ab57a-225">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="ab57a-226">Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="ab57a-226">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="ab57a-227">Przy użyciu tej samej nazwie informuje integratora modelu je traktować jako grupa.</span><span class="sxs-lookup"><span data-stu-id="ab57a-227">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="ab57a-228">Ma ustawioną wartość atrybutu wartości każdego pola wyboru `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="ab57a-228">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="ab57a-229">Po opublikowaniu strony, integratora modelu przekazuje tablicę, która składa się z `CourseID` wartości dla pola wyboru, które są wybrane.</span><span class="sxs-lookup"><span data-stu-id="ab57a-229">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="ab57a-230">Gdy pole wyboru początkowo są renderowane, kursy przypisane do instruktora sprawdzeniu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-230">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="ab57a-231">Uruchamianie aplikacji i przetestuj strony edytowania Instruktorzy zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="ab57a-231">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="ab57a-232">Zmienić niektóre przypisania kursu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-232">Change some course assignments.</span></span> <span data-ttu-id="ab57a-233">Zmiany zostaną odzwierciedlone na stronę indeksu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-233">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="ab57a-234">Uwaga: w tym miejscu podejście do edycji danych kurs przez instruktorów dobrze działa w przypadku istnieje ograniczona liczba kursów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-234">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="ab57a-235">Kolekcje, które są znacznie większe innego interfejsu użytkownika i inną metodę aktualizacji będzie bardziej użyteczny i efektywny.</span><span class="sxs-lookup"><span data-stu-id="ab57a-235">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="ab57a-236">Strona tworzenia Instruktorzy aktualizacji</span><span class="sxs-lookup"><span data-stu-id="ab57a-236">Update the instructors Create page</span></span>

<span data-ttu-id="ab57a-237">Aktualizowanie modelu strony Utwórz przez instruktorów, z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-237">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="ab57a-238">Powyższy kod jest podobny do *Pages/Instructors/Edit.cshtml.cs* kodu.</span><span class="sxs-lookup"><span data-stu-id="ab57a-238">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="ab57a-239">Zaktualizuj strony Razor tworzenia przez instruktorów następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-239">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="ab57a-240">Przetestuj strona tworzenia instruktora.</span><span class="sxs-lookup"><span data-stu-id="ab57a-240">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="ab57a-241">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="ab57a-241">Update the Delete page</span></span>

<span data-ttu-id="ab57a-242">Aktualizowanie modelu strony Usuń z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ab57a-242">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="ab57a-243">Poprzedzający kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="ab57a-243">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="ab57a-244">Używa wczesne ładowanie dla `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ab57a-244">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="ab57a-245">`CourseAssignments` muszą być włączone lub nie zostały usunięte po usunięciu instruktora.</span><span class="sxs-lookup"><span data-stu-id="ab57a-245">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="ab57a-246">Aby uniknąć konieczności je odczytać, należy skonfigurować usuwanie kaskadowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ab57a-246">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="ab57a-247">Jeśli przez instruktorów do usunięcia jest przypisany jako administrator działy, usuwa przypisanie instruktora z tych działów.</span><span class="sxs-lookup"><span data-stu-id="ab57a-247">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ab57a-248">[Poprzednie](xref:data/ef-rp/read-related-data)
> [dalej](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="ab57a-248">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
