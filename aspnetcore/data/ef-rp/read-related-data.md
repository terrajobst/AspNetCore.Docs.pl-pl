---
title: "Stron razor podstawowych EF - odczytanie danych powiązanych — 6, 8"
author: rick-anderson
description: "W tym samouczku odczytu i wyświetlanie powiązanych danych — to znaczy dane programu Entity Framework wczytywane właściwości nawigacji."
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 6e71e9c01a58c3f60dacce8959ac4502a3690690
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="7d6f5-103">Odczytywanie powiązane dane - Core EF Razor strony (6 8)</span><span class="sxs-lookup"><span data-stu-id="7d6f5-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="7d6f5-104">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d6f5-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="7d6f5-105">W tym samouczku powiązanych danych do odczytu i wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="7d6f5-106">Dane dotyczące to dane EF Core ładuje do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="7d6f5-107">Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="7d6f5-108">Na poniższych ilustracjach przedstawiono stron ukończonych w tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="7d6f5-111">Wczesny jawne i opóźnionego ładowania powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="7d6f5-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="7d6f5-112">Istnieje kilka sposobów EF Core może ładować powiązanych danych do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="7d6f5-113">[Ładowanie wczesny](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="7d6f5-114">Ładowanie wczesny jest podczas zapytania dla jednego typu jednostki ładowania również powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="7d6f5-115">Podczas odczytywania jednostki powiązane dane są pobierane.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="7d6f5-116">Powoduje to zwykle w zapytaniu sprzężenia jednej, która pobiera wszystkie dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="7d6f5-117">Podstawowe EF będzie wystawiać wielu zapytań w przypadku niektórych typów wczesny ładowania.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="7d6f5-118">Wystawianie wielu zapytań może być bardziej efektywne niż w przypadku niektórych kwerend w EF6 było pojedynczego zapytania.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="7d6f5-119">Ładowanie wczesny zostanie określony z `Include` i `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Przykład wczesny ładowania](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="7d6f5-121">Ładowanie wczesny wysyła wielu zapytań wchodzi nvavigation kolekcji:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="7d6f5-122">Jedno zapytanie dla głównego zapytania</span><span class="sxs-lookup"><span data-stu-id="7d6f5-122">One query for the main query</span></span> 
 * <span data-ttu-id="7d6f5-123">Jednej kwerendzie dla każdej kolekcji "krawędzi" w drzewie obciążenia.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="7d6f5-124">Oddzielne zapytania z `Load`: w oddzielne zapytania można pobrać dane i EF Core "rozwiązuje" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="7d6f5-125">oznacza "poprawki w górę" EF Core będzie automatycznie wypełni właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="7d6f5-126">Oddzielne zapytania z `Load` przypomina explict ładowania niż wczesny ładowania.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="7d6f5-128">Uwaga: EF Core automatycznie naprawia właściwości nawigacji z innymi obiektami, które wcześniej zostały załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="7d6f5-129">Nawet jeśli dane dla właściwości nawigacji *nie* jawnie uwzględnione właściwość nadal może być wypełniana w przypadku niektórych lub wszystkich powiązanych jednostek wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="7d6f5-130">[Jawne ładowania](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="7d6f5-131">Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="7d6f5-132">Kod musi być przystosowana do pobierania powiązanych danych, gdy jest to potrzebne.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="7d6f5-133">Jawne ładowanie z oddzielne zapytania powoduje wielu zapytań wysłanych do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="7d6f5-134">Z jawnego ładowania kodu określa właściwości nawigacji do załadowania.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="7d6f5-135">Użyj `Load` metody w celu jawnego ładowania.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="7d6f5-136">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-136">For example:</span></span>

 ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="7d6f5-138">[Powolne ładowanie](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="7d6f5-139">[EF Core aktualnie nie obsługuje ładowania opóźnionego](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="7d6f5-140">Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="7d6f5-141">Właściwość nawigacji jest dostępny, po raz pierwszy jest automatycznie pobierany wymagane dane dla tej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="7d6f5-142">Zapytanie jest wysyłane do bazy danych zawsze właściwość nawigacji jest dostępny po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="7d6f5-143">`Select` Operator ładuje tylko powiązane dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="7d6f5-144">Utwórz stronę kursy Wyświetla nazwę działu</span><span class="sxs-lookup"><span data-stu-id="7d6f5-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="7d6f5-145">Jednostka kursu zawiera właściwość nawigacji, który zawiera `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="7d6f5-146">`Department` Jednostka zawiera kursu przypisany do działu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="7d6f5-147">Aby wyświetlić listę kursów nazwa przypisanej działu:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="7d6f5-148">Pobierz `Name` właściwość z `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="7d6f5-149">`Department` Jednostki pochodzi z `Course.Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Dział](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="7d6f5-151">Tworzenie szkieletu modelu kursu</span><span class="sxs-lookup"><span data-stu-id="7d6f5-151">Scaffold the Course model</span></span>

* <span data-ttu-id="7d6f5-152">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="7d6f5-153">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7d6f5-154">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="7d6f5-155">Poprzedni rusztowania polecenia `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="7d6f5-156">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="7d6f5-157">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-157">Build the project.</span></span> <span data-ttu-id="7d6f5-158">Kompilacja generuje błędy podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="7d6f5-159">Globalnie zmienić `_context.Course` do `_context.Courses` (to znaczy dodania "s" do `Course`).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="7d6f5-160">7 wystąpienia są odnaleźć i zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="7d6f5-161">Otwórz *Pages/Courses/Index.cshtml.cs* i sprawdź, czy `OnGetAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="7d6f5-162">Aparat szkieletów określony wczesny ładowania dla `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="7d6f5-163">`Include` Metody określa wczesny ładowania.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="7d6f5-164">Uruchom aplikację i wybierz **kursów** łącza.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="7d6f5-165">Przedstawia kolumnę Dział `DepartmentID`, który nie jest użyteczne.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="7d6f5-166">Aktualizacja `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="7d6f5-167">Poprzedni kod dodaje `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="7d6f5-168">`AsNoTracking` zwiększa wydajność, ponieważ zwróconych nie są śledzone.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="7d6f5-169">Jednostek nie są śledzone, ponieważ nie są one aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="7d6f5-170">Aktualizacja *Views/Courses/Index.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="7d6f5-171">Kod z utworzonym szkieletem wprowadzono następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="7d6f5-172">Zmieniony nagłówek od indeksu do kursów.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="7d6f5-173">Dodaje **numer** kolumny, która zawiera `CourseID` wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="7d6f5-174">Domyślnie nie są szkieletu kluczy podstawowych, ponieważ zwykle są bezużyteczne dla użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="7d6f5-175">Jednak w takim przypadku klucz podstawowy jest łatwy do rozpoznania.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="7d6f5-176">Zmienione **działu** kolumny, aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="7d6f5-177">Wyświetla kod `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="7d6f5-178">Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę z nazwami działu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="7d6f5-180">Trwa ładowanie powiązanych danych za pomocą wybierz</span><span class="sxs-lookup"><span data-stu-id="7d6f5-180">Loading related data with Select</span></span>

<span data-ttu-id="7d6f5-181">`OnGetAsync` Metody ładuje dane powiązane z `Include` metody:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="7d6f5-182">`Select` Operator ładuje tylko powiązane dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="7d6f5-183">Dla pojedynczego elementów takich jak `Department.Name` używa SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="7d6f5-184">Dla kolekcji, używa innego dostęp do bazy danych, lecz to samo `Include` operatora w kolekcjach.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-184">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="7d6f5-185">Poniższy kod ładuje dane powiązane z `Select` metody:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-185">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="7d6f5-186">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-186">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="7d6f5-187">Zobacz [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) pełny przykład.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-187">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="7d6f5-188">Utwórz stronę instruktorów kursów i rejestracji</span><span class="sxs-lookup"><span data-stu-id="7d6f5-188">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="7d6f5-189">W tej sekcji strony instruktorów jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-189">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="7d6f5-190">![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="7d6f5-190">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="7d6f5-191">Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-191">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="7d6f5-192">Lista instruktorów powiązane dane z `OfficeAssignment` jednostki (Office powyższej ilustracji).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-192">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="7d6f5-193">`Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-193">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="7d6f5-194">Ładowanie wczesny służy do `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-194">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="7d6f5-195">Ładowanie wczesny jest zazwyczaj bardziej wydajne, gdy powiązane dane powinny być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-195">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="7d6f5-196">W takim przypadku office przydziałów Instruktorzy są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-196">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="7d6f5-197">Gdy użytkownik wybierze instruktora (Harui powyższej ilustracji) powiązane `Course` wyświetlania obiektów.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-197">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="7d6f5-198">`Instructor` i `Course` jednostki są w relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-198">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="7d6f5-199">Ładowanie wczesny służy do `Course` jednostek i ich pokrewnych `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-199">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="7d6f5-200">W takim przypadku oddzielne zapytania może być bardziej wydajne, ponieważ wymagane są tylko szkoleń dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-200">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="7d6f5-201">Ten przykład przedstawia sposób użycia wczesny ładowania dla właściwości nawigacji w obiektach, które znajdują się w właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-201">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="7d6f5-202">Gdy użytkownik wybierze kursu (chemia powyższej ilustracji), dane z dotyczące `Enrollments` wyświetlania obiektu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-202">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="7d6f5-203">Na poprzedniej ilustracji są wyświetlane nazwy dla użytkowników domowych i klasy.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-203">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="7d6f5-204">`Course` i `Enrollment` jednostki są w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-204">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="7d6f5-205">Utwórz model widoku dla widoku indeksu instruktora</span><span class="sxs-lookup"><span data-stu-id="7d6f5-205">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="7d6f5-206">Na stronie instruktorów znajdują się dane z trzech różnych tabel.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-206">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="7d6f5-207">Model widoku o utworzeniu obejmuje trzy jednostki reprezentujący trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-207">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="7d6f5-208">W *SchoolViewModels* folderu, Utwórz *InstructorIndexData.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-208">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="7d6f5-209">Tworzenie szkieletu modelu instruktora</span><span class="sxs-lookup"><span data-stu-id="7d6f5-209">Scaffold the Instructor model</span></span>

* <span data-ttu-id="7d6f5-210">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-210">Exit Visual Studio.</span></span>
* <span data-ttu-id="7d6f5-211">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-211">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7d6f5-212">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-212">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="7d6f5-213">Poprzedni rusztowania polecenia `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-213">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="7d6f5-214">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-214">Open the project in Visual Studio.</span></span>

<span data-ttu-id="7d6f5-215">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-215">Build the project.</span></span> <span data-ttu-id="7d6f5-216">Kompilacja generuje błędy.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-216">The build generates errors.</span></span>

<span data-ttu-id="7d6f5-217">Globalnie zmienić `_context.Instructor` do `_context.Instructors` (to znaczy dodania "s" do `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-217">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="7d6f5-218">7 wystąpienia są odnaleźć i zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-218">7 occurrences are found and updated.</span></span>

<span data-ttu-id="7d6f5-219">Uruchom aplikację i przejdź do strony instruktorów.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-219">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="7d6f5-220">Zastąp *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-220">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

<span data-ttu-id="7d6f5-221">`OnGetAsync` — Metoda akceptuje dane trasy opcjonalny identyfikator wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="7d6f5-222">Sprawdź zapytania na *Pages/Instructors/Index.cshtml* strony:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-222">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="7d6f5-223">Zapytanie zawiera dwa obejmuje:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-223">The query has two includes:</span></span>

* <span data-ttu-id="7d6f5-224">`OfficeAssignment`: Wyświetlanych w [widoku instruktorów](#IP).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-224">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="7d6f5-225">`CourseAssignments`: Które wprowadzono w kursy nauczanych.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-225">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="7d6f5-226">Aktualizacja instruktorów strony indeksu</span><span class="sxs-lookup"><span data-stu-id="7d6f5-226">Update the instructors Index page</span></span>

<span data-ttu-id="7d6f5-227">Aktualizacja *Pages/Instructors/Index.cshtml* z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-227">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="7d6f5-228">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-228">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="7d6f5-229">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-229">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="7d6f5-230">`"{id:int?}"` to jest szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-230">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="7d6f5-231">Szablon trasy zmiany liczby całkowitej ciągów zapytania w adresie URL w danych trasy.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-231">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="7d6f5-232">Na przykład kliknięcie **wybierz** łącze instruktora tylko z `@page` dyrektywy tworzy adres URL podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-232">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="7d6f5-233">Po dyrektywie page `@page "{id:int?}"`, jest poprzedniego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-233">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="7d6f5-234">Tytuł strony jest **instruktorów**.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-234">Page title is **Instructors**.</span></span>
* <span data-ttu-id="7d6f5-235">Dodaje **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-235">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="7d6f5-236">Ponieważ jest to relacji jeden do zero lub jeden, może nie być OfficeAssignment powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-236">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="7d6f5-237">Dodaje **kursów** kolumnę wyświetlającą kursy nauczanych przy każdym instruktora.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-237">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="7d6f5-238">Zobacz [jawne przejścia wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji o tej składni razor.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-238">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="7d6f5-239">Dodano kod, który dynamicznie dodaje `class="success"` do `tr` elementu wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-239">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="7d6f5-240">To ustawia kolor tła dla wybranego wiersza za pomocą klasy ładowania początkowego.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-240">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="7d6f5-241">Dodano nowe hiperłącze etykietą **wybierz**.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-241">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="7d6f5-242">To łącze wysyła identyfikator wybranego instruktora `Index` — metoda i ustawia kolor tła.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-242">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="7d6f5-243">Uruchom aplikację i wybierz **instruktorów** kartę. Na stronie są wyświetlane `Location` (office) z odnośnych `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-243">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="7d6f5-244">Jeśli OfficeAssignment "jest wartość null, jest wyświetlana pusta tabela komórki.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-244">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Strona indeksu instruktorów nic nie wybrane](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="7d6f5-246">Polecenie **wybierz** łącza.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-246">Click on the **Select** link.</span></span> <span data-ttu-id="7d6f5-247">Zmiany stylu wiersza.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-247">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="7d6f5-248">Dodaj kursy nauczanych przy wybranym instruktorze</span><span class="sxs-lookup"><span data-stu-id="7d6f5-248">Add courses taught by selected instructor</span></span>

<span data-ttu-id="7d6f5-249">Aktualizacja `OnGetAsync` metody w *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-249">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="7d6f5-250">Sprawdź zaktualizowane zapytania:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-250">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="7d6f5-251">Dodaje poprzedniego zapytania `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-251">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="7d6f5-252">Poniższy kod wykonywany po wybraniu instruktora (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-252">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="7d6f5-253">Wybranym instruktorze są pobierane z listy instruktorów w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-253">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="7d6f5-254">Model widoku `Courses` właściwości jest ładowany z `Course` jednostek z tego instruktora `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-254">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="7d6f5-255">`Where` Metoda zwraca kolekcję.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-255">The `Where` method returns a collection.</span></span> <span data-ttu-id="7d6f5-256">W poprzednim `Where` metody, tylko jeden `Instructor` jest zwracana jednostka.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-256">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="7d6f5-257">`Single` Metoda konwertuje kolekcję do postaci jednej `Instructor` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-257">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="7d6f5-258">`Instructor` Jednostki zapewnia dostęp do `CourseAssignments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-258">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="7d6f5-259">`CourseAssignments` zapewnia dostęp do pokrewnych `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-259">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M instruktora do szkolenia](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="7d6f5-261">`Single` Metoda jest używana w kolekcji, jeśli kolekcja zawiera tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-261">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="7d6f5-262">`Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-262">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="7d6f5-263">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku), jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-263">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="7d6f5-264">Przy użyciu `SingleOrDefault` w pustej kolekcji:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-264">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="7d6f5-265">Powoduje wygenerowanie wyjątku (z próby znalezienia `Courses` właściwości na odwołanie o wartości null).</span><span class="sxs-lookup"><span data-stu-id="7d6f5-265">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="7d6f5-266">Komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-266">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="7d6f5-267">Poniższy kod umożliwia wypełnienie modelu widoku `Enrollments` właściwości po wybraniu kursu:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-267">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="7d6f5-268">Dodaj następujący kod na końcu *Pages/Courses/Index.cshtml* Razor strony:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-268">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="7d6f5-269">Poprzedni kod znaczników zostanie wyświetlona lista kursów związane z instruktora po wybraniu instruktora.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-269">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="7d6f5-270">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-270">Test the app.</span></span> <span data-ttu-id="7d6f5-271">Polecenie **wybierz** łącze na stronie instruktorów.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-271">Click on a **Select** link on the instructors page.</span></span>

![Instruktora strony indeksu instruktorów wybrane](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="7d6f5-273">Pokaż dane dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="7d6f5-273">Show student data</span></span>

<span data-ttu-id="7d6f5-274">W tej części aplikacji jest aktualizowana w celu wyświetlenia danych uczniów dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-274">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="7d6f5-275">Zaktualizuj zapytanie w `OnGetAsync` metody w *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-275">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="7d6f5-276">Aktualizacja *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-276">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="7d6f5-277">Dodaj następujący kod na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-277">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="7d6f5-278">Poprzedni kod znaczników Wyświetla listę studentów, którzy są rejestrowane w toku wybrane.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-278">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="7d6f5-279">Odśwież stronę i wybierz instruktora.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-279">Refresh the page and select an instructor.</span></span> <span data-ttu-id="7d6f5-280">Wybierz plan, aby wyświetlić listę zarejestrowanych studentów i ich klas.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-280">Select a course to see the list of enrolled students and their grades.</span></span>

![Instruktora strony indeksu instruktorów i kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="7d6f5-282">Za pomocą pojedynczego</span><span class="sxs-lookup"><span data-stu-id="7d6f5-282">Using Single</span></span>

<span data-ttu-id="7d6f5-283">`Single` Metody można przekazać `Where` warunku zamiast wywoływać metodę `Where` metody oddzielnie:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-283">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="7d6f5-284">Poprzedni `Single` podejście zapewnia nie korzyści w przypadku `Where`.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-284">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="7d6f5-285">Niektórzy deweloperzy preferowane `Single` podejścia stylu.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-285">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="7d6f5-286">Ładowanie jawne</span><span class="sxs-lookup"><span data-stu-id="7d6f5-286">Explicit loading</span></span>

<span data-ttu-id="7d6f5-287">Bieżący kod określa wczesny ładowania dla `Enrollments` i `Students`:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-287">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="7d6f5-288">Załóżmy, że użytkownik chce rzadko Zobacz rejestracji w toku.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-288">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="7d6f5-289">W takim przypadku optymalizacji byłoby tylko, jeśli wymagane jest, ładowanie danych rejestracji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-289">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="7d6f5-290">W tej sekcji `OnGetAsync` jest aktualizowana w celu użyj jawnego ładowania `Enrollments` i `Students`.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-290">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="7d6f5-291">Aktualizacja `OnGetAsync` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-291">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="7d6f5-292">Poprzedni kod porzuca *ThenInclude* metoda wymaga rejestracji i uczniów danych.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-292">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="7d6f5-293">Jeśli wybrano kursu, pobiera wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="7d6f5-293">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="7d6f5-294">`Enrollment` Jednostki dla porach wybrane.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-294">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="7d6f5-295">`Student` Jednostek dla każdego `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-295">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="7d6f5-296">Zwróć uwagę, poprzedni kod komentarze `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-296">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="7d6f5-297">Właściwości nawigacji można jawnie ładować tylko dla śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-297">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="7d6f5-298">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-298">Test the app.</span></span> <span data-ttu-id="7d6f5-299">Z punktu widzenia użytkowników aplikacji zachowuje się tak samo poprzedniej wersji.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-299">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="7d6f5-300">Następny samouczek przedstawia sposób aktualizacji powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="7d6f5-300">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="7d6f5-301">[Poprzednie](xref:data/ef-rp/complex-data-model)
>[dalej](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="7d6f5-301">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
