---
title: "Stron razor podstawowych EF - odczytanie danych powiązanych — 6, 8"
author: rick-anderson
description: "W tym samouczku odczytu i wyświetlanie powiązanych danych — to znaczy dane programu Entity Framework wczytywane właściwości nawigacji."
keywords: "Tworzy sprzężenie platformy ASP.NET Core Entity Framework Core, powiązane dane"
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ba9b17ecdcb605d39117d03230b1db37e8e4d0dd
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="dcd01-104">Odczytywanie powiązane dane - Core EF Razor strony (6 8)</span><span class="sxs-lookup"><span data-stu-id="dcd01-104">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="dcd01-105">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dcd01-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="dcd01-106">W tym samouczku powiązanych danych do odczytu i wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="dcd01-106">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="dcd01-107">Dane dotyczące to dane EF Core ładuje do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-107">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="dcd01-108">Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="dcd01-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="dcd01-109">Na poniższych ilustracjach przedstawiono stron ukończonych w tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="dcd01-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="dcd01-112">Wczesny jawne i opóźnionego ładowania powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="dcd01-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="dcd01-113">Istnieje kilka sposobów EF Core może ładować powiązanych danych do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="dcd01-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="dcd01-114">[Ładowanie wczesny](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="dcd01-114">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="dcd01-115">Ładowanie wczesny jest podczas zapytania dla jednego typu jednostki ładowania również powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="dcd01-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="dcd01-116">Podczas odczytywania jednostki powiązane dane są pobierane.</span><span class="sxs-lookup"><span data-stu-id="dcd01-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="dcd01-117">Powoduje to zwykle w zapytaniu sprzężenia jednej, która pobiera wszystkie dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="dcd01-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="dcd01-118">Podstawowe EF będzie wystawiać wielu zapytań w przypadku niektórych typów wczesny ładowania.</span><span class="sxs-lookup"><span data-stu-id="dcd01-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="dcd01-119">Wystawianie wielu zapytań może być bardziej efektywne niż w przypadku niektórych kwerend w EF6 było pojedynczego zapytania.</span><span class="sxs-lookup"><span data-stu-id="dcd01-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="dcd01-120">Ładowanie wczesny zostanie określony z `Include` i `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="dcd01-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Przykład wczesny ładowania](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="dcd01-122">Ładowanie wczesny wysyła wielu zapytań wchodzi nvavigation kolekcji:</span><span class="sxs-lookup"><span data-stu-id="dcd01-122">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="dcd01-123">Jedno zapytanie dla głównego zapytania</span><span class="sxs-lookup"><span data-stu-id="dcd01-123">One query for the main query</span></span> 
 * <span data-ttu-id="dcd01-124">Jednej kwerendzie dla każdej kolekcji "krawędzi" w drzewie obciążenia.</span><span class="sxs-lookup"><span data-stu-id="dcd01-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="dcd01-125">Oddzielne zapytania z `Load`: w oddzielne zapytania można pobrać dane i EF Core "rozwiązuje" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="dcd01-126">oznacza "poprawki w górę" EF Core będzie automatycznie wypełni właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="dcd01-127">Oddzielne zapytania z `Load` przypomina explict ładowania niż wczesny ładowania.</span><span class="sxs-lookup"><span data-stu-id="dcd01-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="dcd01-129">Uwaga: EF Core automatycznie naprawia właściwości nawigacji z innymi obiektami, które wcześniej zostały załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="dcd01-130">Nawet jeśli dane dla właściwości nawigacji *nie* jawnie uwzględnione właściwość nadal może być wypełniana w przypadku niektórych lub wszystkich powiązanych jednostek wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="dcd01-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="dcd01-131">[Jawne ładowania](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="dcd01-131">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="dcd01-132">Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="dcd01-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="dcd01-133">Kod musi być przystosowana do pobierania powiązanych danych, gdy jest to potrzebne.</span><span class="sxs-lookup"><span data-stu-id="dcd01-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="dcd01-134">Jawne ładowanie z oddzielne zapytania powoduje wielu zapytań wysłanych do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="dcd01-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="dcd01-135">Z jawnego ładowania kodu określa właściwości nawigacji do załadowania.</span><span class="sxs-lookup"><span data-stu-id="dcd01-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="dcd01-136">Użyj `Load` metody w celu jawnego ładowania.</span><span class="sxs-lookup"><span data-stu-id="dcd01-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="dcd01-137">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dcd01-137">For example:</span></span>

 ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="dcd01-139">[Powolne ładowanie](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="dcd01-139">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="dcd01-140">[Podstawowe EF aktualnie nie obsługuje ładowania opóźnionego](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="dcd01-140">[EF Core does not currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="dcd01-141">Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="dcd01-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="dcd01-142">Właściwość nawigacji jest dostępny, po raz pierwszy jest automatycznie pobierany wymagane dane dla tej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="dcd01-143">Zapytanie jest wysyłane do bazy danych zawsze właściwość nawigacji jest dostępny po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="dcd01-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="dcd01-144">`Select` Operator ładuje tylko powiązane dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="dcd01-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="dcd01-145">Utwórz stronę kursy Wyświetla nazwę działu</span><span class="sxs-lookup"><span data-stu-id="dcd01-145">Create a Courses page that displays department name</span></span>

<span data-ttu-id="dcd01-146">Jednostka kursu zawiera właściwość nawigacji, który zawiera `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="dcd01-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="dcd01-147">`Department` Jednostka zawiera kursu przypisany do działu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="dcd01-148">Aby wyświetlić listę kursów nazwa przypisanej działu:</span><span class="sxs-lookup"><span data-stu-id="dcd01-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="dcd01-149">Pobierz `Name` właściwość z `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="dcd01-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="dcd01-150">`Department` Jednostki pochodzi z `Course.Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Dział](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="dcd01-152">Tworzenie szkieletu modelu kursu</span><span class="sxs-lookup"><span data-stu-id="dcd01-152">Scaffold the Course model</span></span>

* <span data-ttu-id="dcd01-153">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcd01-153">Exit Visual Studio.</span></span>
* <span data-ttu-id="dcd01-154">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="dcd01-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="dcd01-155">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="dcd01-155">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="dcd01-156">Poprzedni rusztowania polecenia `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-156">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="dcd01-157">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcd01-157">Open the project in Visual Studio.</span></span>

<span data-ttu-id="dcd01-158">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="dcd01-158">Build the project.</span></span> <span data-ttu-id="dcd01-159">Kompilacja generuje błędy podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="dcd01-159">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="dcd01-160">Globalnie zmienić `_context.Course` do `_context.Courses` (to znaczy dodania "s" do `Course`).</span><span class="sxs-lookup"><span data-stu-id="dcd01-160">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="dcd01-161">7 wystąpienia są odnaleźć i zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="dcd01-161">7 occurrences are found and updated.</span></span>

<span data-ttu-id="dcd01-162">Otwórz *Pages/Courses/Index.cshtml.cs* i sprawdź, czy `OnGetAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="dcd01-162">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="dcd01-163">Aparat szkieletów określony wczesny ładowania dla `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-163">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="dcd01-164">`Include` Metody określa wczesny ładowania.</span><span class="sxs-lookup"><span data-stu-id="dcd01-164">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="dcd01-165">Uruchom aplikację i wybierz **kursów** łącza.</span><span class="sxs-lookup"><span data-stu-id="dcd01-165">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="dcd01-166">Przedstawia kolumnę Dział `DepartmentID`, która nie jest użyteczne.</span><span class="sxs-lookup"><span data-stu-id="dcd01-166">The department column displays the `DepartmentID`, which is not useful.</span></span>

<span data-ttu-id="dcd01-167">Aktualizacja `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="dcd01-167">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="dcd01-168">Poprzedni kod dodaje `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="dcd01-168">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="dcd01-169">`AsNoTracking`zwiększa wydajność, ponieważ zwróconych nie są śledzone.</span><span class="sxs-lookup"><span data-stu-id="dcd01-169">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="dcd01-170">Jednostek nie są śledzone, ponieważ nie są one aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="dcd01-170">The entities are not tracked because they are not updated in the current context.</span></span>

<span data-ttu-id="dcd01-171">Aktualizacja *Views/Courses/Index.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="dcd01-171">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="dcd01-172">Kod z utworzonym szkieletem wprowadzono następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="dcd01-172">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="dcd01-173">Zmieniony nagłówek od indeksu do kursów.</span><span class="sxs-lookup"><span data-stu-id="dcd01-173">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="dcd01-174">Dodaje **numer** kolumny, która zawiera `CourseID` wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="dcd01-174">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="dcd01-175">Domyślnie nie są szkieletu kluczy podstawowych, ponieważ zwykle są bezużyteczne dla użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="dcd01-175">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="dcd01-176">Jednak w takim przypadku klucz podstawowy jest łatwy do rozpoznania.</span><span class="sxs-lookup"><span data-stu-id="dcd01-176">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="dcd01-177">Zmienione **działu** kolumny, aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-177">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="dcd01-178">Wyświetla kod `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="dcd01-178">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="dcd01-179">Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę z nazwami działu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-179">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="dcd01-181">Trwa ładowanie powiązanych danych za pomocą wybierz</span><span class="sxs-lookup"><span data-stu-id="dcd01-181">Loading related data with Select</span></span>

<span data-ttu-id="dcd01-182">`OnGetAsync` Metody ładuje dane powiązane z `Include` metody:</span><span class="sxs-lookup"><span data-stu-id="dcd01-182">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="dcd01-183">`Select` Operator ładuje tylko powiązane dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="dcd01-183">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="dcd01-184">Dla pojedynczego elementów takich jak `Department.Name` używa SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="dcd01-184">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="dcd01-185">Dla kolekcji używa innego dostęp do bazy danych, lecz to samo.`Include`</span><span class="sxs-lookup"><span data-stu-id="dcd01-185">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="dcd01-186">operator w kolekcjach.</span><span class="sxs-lookup"><span data-stu-id="dcd01-186">operator on collections.</span></span>

<span data-ttu-id="dcd01-187">Poniższy kod ładuje dane powiązane z `Select` metody:</span><span class="sxs-lookup"><span data-stu-id="dcd01-187">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="dcd01-188">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="dcd01-188">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="dcd01-189">Zobacz [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) pełny przykład.</span><span class="sxs-lookup"><span data-stu-id="dcd01-189">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="dcd01-190">Utwórz stronę instruktorów kursów i rejestracji</span><span class="sxs-lookup"><span data-stu-id="dcd01-190">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="dcd01-191">W tej sekcji strony instruktorów jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="dcd01-191">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="dcd01-192">![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="dcd01-192">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="dcd01-193">Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="dcd01-193">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="dcd01-194">Lista instruktorów powiązane dane z `OfficeAssignment` jednostki (Office powyższej ilustracji).</span><span class="sxs-lookup"><span data-stu-id="dcd01-194">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="dcd01-195">`Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden.</span><span class="sxs-lookup"><span data-stu-id="dcd01-195">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="dcd01-196">Ładowanie wczesny służy do `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="dcd01-196">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="dcd01-197">Ładowanie wczesny jest zazwyczaj bardziej wydajne, gdy powiązane dane powinny być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="dcd01-197">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="dcd01-198">W takim przypadku office przydziałów Instruktorzy są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="dcd01-198">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="dcd01-199">Gdy użytkownik wybierze instruktora (Harui powyższej ilustracji) powiązane `Course` wyświetlania obiektów.</span><span class="sxs-lookup"><span data-stu-id="dcd01-199">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="dcd01-200">`Instructor` i `Course` jednostki są w relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-200">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="dcd01-201">Eager ładowania dla `Course` jednostek i ich pokrewnych `Department` jednostek jest używany.</span><span class="sxs-lookup"><span data-stu-id="dcd01-201">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="dcd01-202">W takim przypadku oddzielne zapytania może być bardziej wydajne, ponieważ wymagane są tylko szkoleń dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="dcd01-202">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="dcd01-203">Ten przykład przedstawia sposób użycia wczesny ładowania dla właściwości nawigacji w obiektach, które znajdują się w właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-203">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="dcd01-204">Gdy użytkownik wybierze kursu (chemia powyższej ilustracji), dane z dotyczące `Enrollments` wyświetlania obiektu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-204">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="dcd01-205">Na poprzedniej ilustracji są wyświetlane nazwy dla użytkowników domowych i klasy.</span><span class="sxs-lookup"><span data-stu-id="dcd01-205">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="dcd01-206">`Course` i `Enrollment` jednostki są w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-206">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="dcd01-207">Utwórz model widoku dla widoku indeksu instruktora</span><span class="sxs-lookup"><span data-stu-id="dcd01-207">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="dcd01-208">Na stronie instruktorów znajdują się dane z trzech różnych tabel.</span><span class="sxs-lookup"><span data-stu-id="dcd01-208">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="dcd01-209">Model widoku o utworzeniu obejmuje trzy jednostki reprezentujący trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="dcd01-209">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="dcd01-210">W *SchoolViewModels* folderu, Utwórz *InstructorIndexData.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="dcd01-210">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="dcd01-211">Tworzenie szkieletu modelu instruktora</span><span class="sxs-lookup"><span data-stu-id="dcd01-211">Scaffold the Instructor model</span></span>

* <span data-ttu-id="dcd01-212">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcd01-212">Exit Visual Studio.</span></span>
* <span data-ttu-id="dcd01-213">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="dcd01-213">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="dcd01-214">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="dcd01-214">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="dcd01-215">Poprzedni rusztowania polecenia `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-215">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="dcd01-216">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcd01-216">Open the project in Visual Studio.</span></span>

<span data-ttu-id="dcd01-217">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="dcd01-217">Build the project.</span></span> <span data-ttu-id="dcd01-218">Kompilacja generuje błędy.</span><span class="sxs-lookup"><span data-stu-id="dcd01-218">The build generates errors.</span></span>

<span data-ttu-id="dcd01-219">Globalnie zmienić `_context.Instructor` do `_context.Instructors` (to znaczy dodania "s" do `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="dcd01-219">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="dcd01-220">7 wystąpienia są odnaleźć i zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="dcd01-220">7 occurrences are found and updated.</span></span>

<span data-ttu-id="dcd01-221">Uruchom aplikację i przejdź do strony instruktorów.</span><span class="sxs-lookup"><span data-stu-id="dcd01-221">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="dcd01-222">Zastąp *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="dcd01-222">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="dcd01-223">`OnGetAsync` — Metoda akceptuje dane trasy opcjonalny identyfikator wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="dcd01-223">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="dcd01-224">Sprawdź zapytania na *Pages/Instructors/Index.cshtml* strony:</span><span class="sxs-lookup"><span data-stu-id="dcd01-224">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="dcd01-225">Zapytanie zawiera dwa obejmuje:</span><span class="sxs-lookup"><span data-stu-id="dcd01-225">The query has two includes:</span></span>

* <span data-ttu-id="dcd01-226">`OfficeAssignment`: Wyświetlanych w [widoku instruktorów](#IP).</span><span class="sxs-lookup"><span data-stu-id="dcd01-226">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="dcd01-227">`CourseAssignments`: Które wprowadzono w kursy nauczanych.</span><span class="sxs-lookup"><span data-stu-id="dcd01-227">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="dcd01-228">Aktualizacja instruktorów strony indeksu</span><span class="sxs-lookup"><span data-stu-id="dcd01-228">Update the instructors Index page</span></span>

<span data-ttu-id="dcd01-229">Aktualizacja *Pages/Instructors/Index.cshtml* z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="dcd01-229">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="dcd01-230">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="dcd01-230">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="dcd01-231">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="dcd01-231">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="dcd01-232">`"{id:int?}"`to jest szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="dcd01-232">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="dcd01-233">Szablon trasy zmiany liczby całkowitej ciągów zapytania w adresie URL w danych trasy.</span><span class="sxs-lookup"><span data-stu-id="dcd01-233">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="dcd01-234">Na przykład kliknięcie **wybierz** łącze instruktora po dyrektywie page tworzy adres URL podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="dcd01-234">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="dcd01-235">Po dyrektywie page `@page "{id:int?}"`, jest poprzedniego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="dcd01-235">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="dcd01-236">Tytuł strony jest **instruktorów**.</span><span class="sxs-lookup"><span data-stu-id="dcd01-236">Page title is **Instructors**.</span></span>
* <span data-ttu-id="dcd01-237">Dodaje **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie jest zerowa.</span><span class="sxs-lookup"><span data-stu-id="dcd01-237">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="dcd01-238">Ponieważ jest to relacji jeden do zero lub jeden, może nie być OfficeAssignment powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="dcd01-238">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="dcd01-239">Dodaje **kursów** kolumnę wyświetlającą kursy nauczanych przy każdym instruktora.</span><span class="sxs-lookup"><span data-stu-id="dcd01-239">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="dcd01-240">Zobacz [jawne przejścia wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji o tej składni razor.</span><span class="sxs-lookup"><span data-stu-id="dcd01-240">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="dcd01-241">Dodano kod, który dynamicznie dodaje `class="success"` do `tr` elementu wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="dcd01-241">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="dcd01-242">To ustawia kolor tła dla wybranego wiersza za pomocą klasy ładowania początkowego.</span><span class="sxs-lookup"><span data-stu-id="dcd01-242">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="dcd01-243">Dodano nowe hiperłącze etykietą **wybierz**.</span><span class="sxs-lookup"><span data-stu-id="dcd01-243">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="dcd01-244">To łącze wysyła identyfikator wybranego instruktora `Index` — metoda i ustawia kolor tła.</span><span class="sxs-lookup"><span data-stu-id="dcd01-244">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="dcd01-245">Uruchom aplikację i wybierz **instruktorów** kartę. Na stronie są wyświetlane `Location` (office) z odnośnych `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="dcd01-245">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="dcd01-246">Jeśli OfficeAssignment "jest wartość null, jest wyświetlana pusta tabela komórki.</span><span class="sxs-lookup"><span data-stu-id="dcd01-246">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Strona indeksu instruktorów nic nie wybrane](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="dcd01-248">Polecenie **wybierz** łącza.</span><span class="sxs-lookup"><span data-stu-id="dcd01-248">Click on the **Select** link.</span></span> <span data-ttu-id="dcd01-249">Zmiany stylu wiersza.</span><span class="sxs-lookup"><span data-stu-id="dcd01-249">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="dcd01-250">Dodaj kursy nauczanych przy wybranym instruktorze</span><span class="sxs-lookup"><span data-stu-id="dcd01-250">Add courses taught by selected instructor</span></span>

<span data-ttu-id="dcd01-251">Aktualizacja `OnGetAsync` metody w *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="dcd01-251">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="dcd01-252">Sprawdź zaktualizowane zapytania:</span><span class="sxs-lookup"><span data-stu-id="dcd01-252">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="dcd01-253">Dodaje poprzedniego zapytania `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="dcd01-253">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="dcd01-254">Poniższy kod wykonywany po wybraniu instruktora (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="dcd01-254">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="dcd01-255">Wybranym instruktorze są pobierane z listy instruktorów w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="dcd01-255">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="dcd01-256">Model widoku `Courses` właściwości jest ładowany z `Course` jednostek z tego instruktora `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-256">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="dcd01-257">`Where` Metoda zwraca kolekcję.</span><span class="sxs-lookup"><span data-stu-id="dcd01-257">The `Where` method returns a collection.</span></span> <span data-ttu-id="dcd01-258">W poprzednim `Where` metody, tylko jeden `Instructor` jest zwracana jednostka.</span><span class="sxs-lookup"><span data-stu-id="dcd01-258">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="dcd01-259">`Single` Metoda konwertuje kolekcję do postaci jednej `Instructor` jednostki.</span><span class="sxs-lookup"><span data-stu-id="dcd01-259">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="dcd01-260">`Instructor` Jednostki zapewnia dostęp do `CourseAssignments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="dcd01-260">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="dcd01-261">`CourseAssignments`zapewnia dostęp do pokrewnych `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="dcd01-261">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M instruktora do szkolenia](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="dcd01-263">`Single` Metoda jest używana w kolekcji, jeśli kolekcja zawiera tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="dcd01-263">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="dcd01-264">`Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="dcd01-264">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="dcd01-265">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku), jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="dcd01-265">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="dcd01-266">Przy użyciu `SingleOrDefault` w pustej kolekcji:</span><span class="sxs-lookup"><span data-stu-id="dcd01-266">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="dcd01-267">Powoduje wygenerowanie wyjątku (z próby znalezienia `Courses` właściwości na odwołanie o wartości null).</span><span class="sxs-lookup"><span data-stu-id="dcd01-267">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="dcd01-268">Komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-268">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="dcd01-269">Poniższy kod umożliwia wypełnienie modelu widoku `Enrollments` właściwości po wybraniu kursu:</span><span class="sxs-lookup"><span data-stu-id="dcd01-269">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="dcd01-270">Dodaj następujący kod na końcu *Pages/Courses/Index.cshtml* Razor strony:</span><span class="sxs-lookup"><span data-stu-id="dcd01-270">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="dcd01-271">Poprzedni kod znaczników zostanie wyświetlona lista kursów związane z instruktora po wybraniu instruktora.</span><span class="sxs-lookup"><span data-stu-id="dcd01-271">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="dcd01-272">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-272">Test the app.</span></span> <span data-ttu-id="dcd01-273">Polecenie **wybierz** łącze na stronie instruktorów.</span><span class="sxs-lookup"><span data-stu-id="dcd01-273">Click on a **Select** link on the instructors page.</span></span>

![Instruktora strony indeksu instruktorów wybrane](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="dcd01-275">Pokaż dane dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="dcd01-275">Show student data</span></span>

<span data-ttu-id="dcd01-276">W tej części aplikacji jest aktualizowana w celu wyświetlenia danych uczniów dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-276">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="dcd01-277">Zaktualizuj zapytanie w `OnGetAsync` metody w *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="dcd01-277">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="dcd01-278">Aktualizacja *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dcd01-278">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="dcd01-279">Dodaj następujący kod na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="dcd01-279">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="dcd01-280">Poprzedni kod znaczników Wyświetla listę studentów, którzy są rejestrowane w toku wybrane.</span><span class="sxs-lookup"><span data-stu-id="dcd01-280">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="dcd01-281">Odśwież stronę i wybierz instruktora.</span><span class="sxs-lookup"><span data-stu-id="dcd01-281">Refresh the page and select an instructor.</span></span> <span data-ttu-id="dcd01-282">Wybierz plan, aby wyświetlić listę zarejestrowanych studentów i ich klas.</span><span class="sxs-lookup"><span data-stu-id="dcd01-282">Select a course to see the list of enrolled students and their grades.</span></span>

![Instruktora strony indeksu instruktorów i kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="dcd01-284">Za pomocą pojedynczego</span><span class="sxs-lookup"><span data-stu-id="dcd01-284">Using Single</span></span>

<span data-ttu-id="dcd01-285">`Single` Metody można przekazać `Where` warunku zamiast wywoływać metodę `Where` metody oddzielnie:</span><span class="sxs-lookup"><span data-stu-id="dcd01-285">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="dcd01-286">Poprzedni `Single` podejście zapewnia nie korzyści w przypadku `Where`.</span><span class="sxs-lookup"><span data-stu-id="dcd01-286">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="dcd01-287">Niektórzy deweloperzy preferowane `Single` podejścia stylu.</span><span class="sxs-lookup"><span data-stu-id="dcd01-287">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="dcd01-288">Ładowanie jawne</span><span class="sxs-lookup"><span data-stu-id="dcd01-288">Explicit loading</span></span>

<span data-ttu-id="dcd01-289">Bieżący kod określa wczesny ładowania dla `Enrollments` i `Students`:</span><span class="sxs-lookup"><span data-stu-id="dcd01-289">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="dcd01-290">Załóżmy, że użytkownik chce rzadko Zobacz rejestracji w toku.</span><span class="sxs-lookup"><span data-stu-id="dcd01-290">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="dcd01-291">W takim przypadku optymalizacji byłoby tylko, jeśli wymagane jest, ładowanie danych rejestracji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-291">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="dcd01-292">W tej sekcji `OnGetAsync` jest aktualizowana w celu użyj jawnego ładowania `Enrollments` i `Students`.</span><span class="sxs-lookup"><span data-stu-id="dcd01-292">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="dcd01-293">Aktualizacja `OnGetAsync` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="dcd01-293">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="dcd01-294">Poprzedni kod porzuca *ThenInclude* metoda wymaga rejestracji i uczniów danych.</span><span class="sxs-lookup"><span data-stu-id="dcd01-294">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="dcd01-295">Jeśli wybrano kursu, pobiera wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="dcd01-295">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="dcd01-296">`Enrollment` Jednostki dla porach wybrane.</span><span class="sxs-lookup"><span data-stu-id="dcd01-296">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="dcd01-297">`Student` Jednostek dla każdego `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="dcd01-297">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="dcd01-298">Zwróć uwagę, poprzedni kod komentarze `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="dcd01-298">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="dcd01-299">Właściwości nawigacji można jawnie ładować tylko dla śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="dcd01-299">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="dcd01-300">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-300">Test the app.</span></span> <span data-ttu-id="dcd01-301">Z punktu widzenia użytkowników aplikacji zachowuje się tak samo poprzedniej wersji.</span><span class="sxs-lookup"><span data-stu-id="dcd01-301">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="dcd01-302">Następny samouczek przedstawia sposób aktualizacji powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="dcd01-302">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="dcd01-303">[Poprzednie](xref:data/ef-rp/complex-data-model)
>[dalej](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="dcd01-303">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>