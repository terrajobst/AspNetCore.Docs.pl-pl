---
title: Strony razor z programem EF Core w programie ASP.NET Core — odczytanie powiązanych danych — 6 8
author: rick-anderson
description: W tym samouczku należy przeczytać i wyświetlanie powiązanych danych — oznacza to, że dane programu Entity Framework wczytywane właściwości nawigacji.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: fa3147cc4ad121784911eef802e04ca91f16448f
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063315"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="edb32-103">Strony razor z programem EF Core w programie ASP.NET Core — odczytanie powiązanych danych — 6 8</span><span class="sxs-lookup"><span data-stu-id="edb32-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="edb32-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="edb32-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="edb32-105">W tym samouczku powiązanych danych odczytu i wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="edb32-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="edb32-106">Powiązane dane to dane programu EF Core ładuje się do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="edb32-107">Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="edb32-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="edb32-108">Na poniższych ilustracjach przedstawiono stron ukończonych w ramach tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="edb32-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Kursy strony indeksu](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="edb32-111">Zapoznamy wyraźne i powolne ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="edb32-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="edb32-112">Istnieje kilka sposobów, że programu EF Core można załadować powiązane dane do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="edb32-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="edb32-113">[Wczesne ładowanie](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="edb32-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="edb32-114">Wczesne ładowanie jest, gdy zapytanie dla jednego typu obiektu również ładuje powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="edb32-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="edb32-115">Podczas odczytywania jednostki powiązane dane są pobierane.</span><span class="sxs-lookup"><span data-stu-id="edb32-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="edb32-116">Powoduje to zwykle w zapytaniu sprzężenia jednego, który pobiera wszystkie dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="edb32-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="edb32-117">EF Core będzie wystawiać wielu zapytań dla niektórych rodzajów wczesne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="edb32-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="edb32-118">Wystawianie wielu zapytań może być bardziej efektywne niż w przypadku niektórych zapytań w EF6 było pojedynczego zapytania.</span><span class="sxs-lookup"><span data-stu-id="edb32-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="edb32-119">Wczesne ładowanie jest określony za pomocą `Include` i `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="edb32-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Przykład wczesne ładowanie](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="edb32-121">Wczesne ładowanie wysyła wielu zapytań, gdy wchodzi nawigacji kolekcji:</span><span class="sxs-lookup"><span data-stu-id="edb32-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="edb32-122">Jednej kwerendzie dla głównego zapytania</span><span class="sxs-lookup"><span data-stu-id="edb32-122">One query for the main query</span></span> 
  * <span data-ttu-id="edb32-123">Jednej kwerendzie dla każdej kolekcji "brzegu", w drzewie obciążenia.</span><span class="sxs-lookup"><span data-stu-id="edb32-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="edb32-124">Oddziel zapytań przy użyciu `Load`: dane można pobrać w oddzielne zapytania i programem EF Core termin "poprawki" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="edb32-125">oznacza "poprawki w górę" EF Core będzie automatycznie wypełnia właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="edb32-126">Oddziel zapytań przy użyciu `Load` więcej takich jak ładowanie niż wczesne ładowanie stosowany jawny.</span><span class="sxs-lookup"><span data-stu-id="edb32-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="edb32-128">Uwaga: Programu EF Core jest automatycznie rozwiązuje właściwości nawigacji do innych jednostek, które wcześniej zostały załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="edb32-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="edb32-129">Nawet jeśli dane dla właściwości nawigacyjnej *nie* jawnie uwzględnione, nadal można wypełnić właściwość, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="edb32-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="edb32-130">[Jawne ładowanie](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="edb32-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="edb32-131">Podczas odczytywania jednostki powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="edb32-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="edb32-132">Musi być napisany kod można pobrać powiązanych danych, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="edb32-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="edb32-133">Jawne ładowanie w oddzielnych zapytań powoduje wielu zapytań wysyłanych do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="edb32-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="edb32-134">Przy użyciu jawne ładowanie kod określa właściwości nawigacji, aby go załadować.</span><span class="sxs-lookup"><span data-stu-id="edb32-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="edb32-135">Użyj `Load` celu jawne ładowanie metodę.</span><span class="sxs-lookup"><span data-stu-id="edb32-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="edb32-136">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="edb32-136">For example:</span></span>

  ![Przykład jawne ładowanie](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="edb32-138">[Powolne ładowanie](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="edb32-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="edb32-139">[EF Core nie obsługuje obecnie powolne ładowanie](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="edb32-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="edb32-140">Podczas odczytywania jednostki powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="edb32-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="edb32-141">Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji jest automatycznie pobierany wymagane dane dla tej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="edb32-142">Zapytanie jest wysyłane do bazy danych, w każdym właściwość nawigacji jest dostępny po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="edb32-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="edb32-143">`Select` Operator ładuje tylko powiązane dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="edb32-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="edb32-144">Tworzenie strony kursów, który wyświetla nazwy działu</span><span class="sxs-lookup"><span data-stu-id="edb32-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="edb32-145">Jednostki kurs zawiera właściwość nawigacji, która zawiera `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="edb32-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="edb32-146">`Department` Działu, przypisana do kursu w jednostce.</span><span class="sxs-lookup"><span data-stu-id="edb32-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="edb32-147">Aby wyświetlić listę kursów nazwa działu przypisane:</span><span class="sxs-lookup"><span data-stu-id="edb32-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="edb32-148">Pobierz `Name` właściwość `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="edb32-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="edb32-149">`Department` Jednostki pochodzi z `Course.Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Dział](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="edb32-151">Tworzenie szkieletu modelu kursu</span><span class="sxs-lookup"><span data-stu-id="edb32-151">Scaffold the Course model</span></span>

* <span data-ttu-id="edb32-152">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="edb32-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="edb32-153">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="edb32-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="edb32-154">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="edb32-154">Run the following command:</span></span>

  ```console
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="edb32-155">Poprzedni szkielety mechanizmów polecenia `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="edb32-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="edb32-156">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="edb32-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="edb32-157">Otwórz *Pages/Courses/Index.cshtml.cs* i zbadaj `OnGetAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="edb32-157">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="edb32-158">Aparat tworzenia szkieletów określony wczesne ładowanie dla `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-158">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="edb32-159">`Include` Metody określa wczesne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="edb32-159">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="edb32-160">Uruchom aplikację i wybierz **kursów** łącza.</span><span class="sxs-lookup"><span data-stu-id="edb32-160">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="edb32-161">Wyświetla kolumnę Dział `DepartmentID`, który nie jest przydatne.</span><span class="sxs-lookup"><span data-stu-id="edb32-161">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="edb32-162">Aktualizacja `OnGetAsync` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edb32-162">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="edb32-163">Poprzedzający kod dodaje `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="edb32-163">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="edb32-164">`AsNoTracking` zwiększa wydajność, ponieważ nie są śledzone obiektów zwracanych.</span><span class="sxs-lookup"><span data-stu-id="edb32-164">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="edb32-165">Jednostki nie są śledzone, ponieważ nie są one aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="edb32-165">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="edb32-166">Aktualizacja *Pages/Courses/Index.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="edb32-166">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="edb32-167">Utworzony szkielet kodu wprowadzono następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="edb32-167">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="edb32-168">Zmienić nagłówek z indeksu do kursów.</span><span class="sxs-lookup"><span data-stu-id="edb32-168">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="edb32-169">Dodano **numer** kolumna, która pokazuje `CourseID` wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="edb32-169">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="edb32-170">Domyślnie klucze podstawowe nie są szkieletu, ponieważ zwykle są bez znaczenia dla użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="edb32-170">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="edb32-171">Jednak w takim przypadku klucz podstawowy jest znaczący.</span><span class="sxs-lookup"><span data-stu-id="edb32-171">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="edb32-172">Zmienione **działu** kolumny, aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="edb32-172">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="edb32-173">Ten kod wyświetla `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="edb32-173">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="edb32-174">Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę nazw działów.</span><span class="sxs-lookup"><span data-stu-id="edb32-174">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kursy strony indeksu](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="edb32-176">Trwa ładowanie powiązanych danych z wybranymi</span><span class="sxs-lookup"><span data-stu-id="edb32-176">Loading related data with Select</span></span>

<span data-ttu-id="edb32-177">`OnGetAsync` Metoda ładuje dane powiązane z `Include` metody:</span><span class="sxs-lookup"><span data-stu-id="edb32-177">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="edb32-178">`Select` Operator ładuje tylko powiązane dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="edb32-178">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="edb32-179">Dla pojedynczego elementów takich jak `Department.Name` używa SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="edb32-179">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="edb32-180">Dla kolekcji, jego używa innego dostępu do bazy danych, ale to samo dotyczy `Include` operatora w kolekcjach.</span><span class="sxs-lookup"><span data-stu-id="edb32-180">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="edb32-181">Poniższy kod ładuje dane powiązane z `Select` metody:</span><span class="sxs-lookup"><span data-stu-id="edb32-181">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="edb32-182">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="edb32-182">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="edb32-183">Zobacz [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) pełny przykład.</span><span class="sxs-lookup"><span data-stu-id="edb32-183">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="edb32-184">Utwórz stronę instruktorów, pokazujący kursów i rejestracji</span><span class="sxs-lookup"><span data-stu-id="edb32-184">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="edb32-185">W tej sekcji Strona Instruktorzy jest tworzona.</span><span class="sxs-lookup"><span data-stu-id="edb32-185">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="edb32-186">![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="edb32-186">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="edb32-187">Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="edb32-187">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="edb32-188">Lista Instruktorzy dane związane z `OfficeAssignment` jednostki (Office na poprzedniej ilustracji).</span><span class="sxs-lookup"><span data-stu-id="edb32-188">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="edb32-189">`Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden.</span><span class="sxs-lookup"><span data-stu-id="edb32-189">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="edb32-190">Wczesne ładowanie służy do `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="edb32-190">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="edb32-191">Wczesne ładowanie jest zazwyczaj bardziej efektywne, gdy powiązane dane mają być wyświetlone.</span><span class="sxs-lookup"><span data-stu-id="edb32-191">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="edb32-192">W tym przypadku Instruktorzy przypisania pakietu office są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="edb32-192">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="edb32-193">Gdy użytkownik wybierze instruktora (Harui na poprzedniej ilustracji) związane z `Course` jednostki są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="edb32-193">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="edb32-194">`Instructor` i `Course` jednostki są w relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="edb32-194">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="edb32-195">Wczesne ładowanie służy do `Course` jednostek i ich powiązane `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="edb32-195">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="edb32-196">W tym przypadku oddzielne zapytania może być bardziej efektywne, ponieważ potrzebne są tylko kursy dla wybranej przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="edb32-196">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="edb32-197">W tym przykładzie pokazano, jak używać wczesne ładowanie dla właściwości nawigacji w jednostkach, które znajdują się w właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-197">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="edb32-198">Gdy użytkownik wybierze kurs (chemia na poprzedniej ilustracji), związane z danymi z `Enrollments` jednostka jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="edb32-198">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="edb32-199">Na wcześniejszej ilustracji są wyświetlane nazwy dla uczniów i klasy korporacyjnej.</span><span class="sxs-lookup"><span data-stu-id="edb32-199">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="edb32-200">`Course` i `Enrollment` jednostki są w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="edb32-200">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="edb32-201">Tworzenie modelu widoku dla widoku indeksu przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="edb32-201">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="edb32-202">Na stronie Instruktorzy wyświetlane dane z trzech różnych tabelach.</span><span class="sxs-lookup"><span data-stu-id="edb32-202">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="edb32-203">Tworzony jest model widoku, który zawiera trzy obiekty reprezentujące trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="edb32-203">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="edb32-204">W *SchoolViewModels* folderze utwórz *InstructorIndexData.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edb32-204">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="edb32-205">Tworzenie szkieletu modelu przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="edb32-205">Scaffold the Instructor model</span></span>

* <span data-ttu-id="edb32-206">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="edb32-206">Exit Visual Studio.</span></span>
* <span data-ttu-id="edb32-207">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="edb32-207">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="edb32-208">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="edb32-208">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="edb32-209">Poprzedni szkielety mechanizmów polecenia `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="edb32-209">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="edb32-210">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="edb32-210">Open the project in Visual Studio.</span></span>

<span data-ttu-id="edb32-211">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="edb32-211">Build the project.</span></span> <span data-ttu-id="edb32-212">Kompilacja generuje błędy.</span><span class="sxs-lookup"><span data-stu-id="edb32-212">The build generates errors.</span></span>

<span data-ttu-id="edb32-213">Globalnie zmienić `_context.Instructor` do `_context.Instructors` (oznacza to, Dodaj znak "s" do `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="edb32-213">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="edb32-214">7 wystąpień są znaleźć i zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="edb32-214">7 occurrences are found and updated.</span></span>

<span data-ttu-id="edb32-215">Uruchom aplikację i przejdź do strony instruktorów.</span><span class="sxs-lookup"><span data-stu-id="edb32-215">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="edb32-216">Zastąp *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edb32-216">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

<span data-ttu-id="edb32-217">`OnGetAsync` Metoda przyjmuje dane opcjonalne trasy dla Identyfikatora wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="edb32-217">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="edb32-218">Sprawdź zapytania w *Pages/Instructors/Index.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="edb32-218">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="edb32-219">Zapytanie ma dwa obejmuje:</span><span class="sxs-lookup"><span data-stu-id="edb32-219">The query has two includes:</span></span>

* <span data-ttu-id="edb32-220">`OfficeAssignment`: Wyświetlane w [widoku Instruktorzy](#IP).</span><span class="sxs-lookup"><span data-stu-id="edb32-220">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="edb32-221">`CourseAssignments`: Wiąże które kursy prowadzone.</span><span class="sxs-lookup"><span data-stu-id="edb32-221">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="edb32-222">Aktualizuj stronę indeksu instruktorów</span><span class="sxs-lookup"><span data-stu-id="edb32-222">Update the instructors Index page</span></span>

<span data-ttu-id="edb32-223">Aktualizacja *Pages/Instructors/Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edb32-223">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="edb32-224">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="edb32-224">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="edb32-225">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="edb32-225">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="edb32-226">`"{id:int?}"` jest szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="edb32-226">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="edb32-227">Szablon trasy zmiany liczby całkowitej ciągów zapytania w adresie URL danych trasy.</span><span class="sxs-lookup"><span data-stu-id="edb32-227">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="edb32-228">Na przykład, klikając **wybierz** link pod kierunkiem instruktora tylko z `@page` dyrektywa generuje adres URL podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="edb32-228">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="edb32-229">Po dyrektywie page `@page "{id:int?}"`, jest poprzedniego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="edb32-229">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="edb32-230">Tytuł strony jest **Instruktorzy**.</span><span class="sxs-lookup"><span data-stu-id="edb32-230">Page title is **Instructors**.</span></span>
* <span data-ttu-id="edb32-231">Dodano **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="edb32-231">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="edb32-232">Ponieważ jest to relacja jeden do zero lub jeden, może nie być powiązana jednostka OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="edb32-232">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="edb32-233">Dodano **kursów** kolumnę wyświetlającą kursy prowadzone przez instruktora każdego.</span><span class="sxs-lookup"><span data-stu-id="edb32-233">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="edb32-234">Zobacz [jawne przejście wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji na temat tej składni razor.</span><span class="sxs-lookup"><span data-stu-id="edb32-234">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="edb32-235">Dodano kod, w której są dodawane dynamicznie `class="success"` do `tr` elementu wybranego przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="edb32-235">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="edb32-236">To ustawienie jako kolor tła wybranego wiersza przy użyciu ładowania klasy.</span><span class="sxs-lookup"><span data-stu-id="edb32-236">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="edb32-237">Dodano nowe hiperłącze etykietą **wybierz**.</span><span class="sxs-lookup"><span data-stu-id="edb32-237">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="edb32-238">To łącze wysyła identyfikator wybranego przez instruktorów `Index` metody i ustawia kolor tła.</span><span class="sxs-lookup"><span data-stu-id="edb32-238">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="edb32-239">Uruchom aplikację i wybierz **Instruktorzy** kartę. Zostanie wyświetlona strona `Location` (office) z powiązane `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="edb32-239">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="edb32-240">Jeśli OfficeAssignment "jest komórka pustej tabeli ma wartość null, jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="edb32-240">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Strona indeksu instruktorów, którą nic nie zaznaczone](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="edb32-242">Kliknij pozycję **wybierz** łącza.</span><span class="sxs-lookup"><span data-stu-id="edb32-242">Click on the **Select** link.</span></span> <span data-ttu-id="edb32-243">Zmiany stylów wierszy.</span><span class="sxs-lookup"><span data-stu-id="edb32-243">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="edb32-244">Dodaj kursom prowadzonym przez instruktora wybrane</span><span class="sxs-lookup"><span data-stu-id="edb32-244">Add courses taught by selected instructor</span></span>

<span data-ttu-id="edb32-245">Aktualizacja `OnGetAsync` method in Class metoda *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edb32-245">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="edb32-246">Add `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="edb32-246">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="edb32-247">Sprawdź zaktualizowane zapytanie:</span><span class="sxs-lookup"><span data-stu-id="edb32-247">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="edb32-248">Poprzednie zapytanie dodaje `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="edb32-248">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="edb32-249">Poniższy kod jest wykonywany po wybraniu pod kierunkiem instruktora (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="edb32-249">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="edb32-250">Wybrane przez instruktorów są pobierane z listy w modelu widoku instruktorów.</span><span class="sxs-lookup"><span data-stu-id="edb32-250">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="edb32-251">Model widoku `Courses` właściwość jest ładowany z `Course` jednostek z tego instruktora `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-251">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="edb32-252">`Where` Metoda zwraca kolekcję.</span><span class="sxs-lookup"><span data-stu-id="edb32-252">The `Where` method returns a collection.</span></span> <span data-ttu-id="edb32-253">W poprzednim `Where` metody, tylko jeden `Instructor` jednostki jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="edb32-253">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="edb32-254">`Single` Metoda konwertuje kolekcję z jedną `Instructor` jednostki.</span><span class="sxs-lookup"><span data-stu-id="edb32-254">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="edb32-255">`Instructor` Jednostka zapewnia dostęp do `CourseAssignments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="edb32-255">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="edb32-256">`CourseAssignments` zapewnia dostęp do odnośnych `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="edb32-256">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M przez instruktorów do kursów](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="edb32-258">`Single` Metoda jest używana w kolekcji, jeśli kolekcja zawiera tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="edb32-258">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="edb32-259">`Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub ma więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="edb32-259">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="edb32-260">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku) Jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="edb32-260">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="edb32-261">Za pomocą `SingleOrDefault` w pustej kolekcji:</span><span class="sxs-lookup"><span data-stu-id="edb32-261">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="edb32-262">Powoduje wygenerowanie wyjątku (z próby znalezienia `Courses` właściwość odwołanie o wartości null).</span><span class="sxs-lookup"><span data-stu-id="edb32-262">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="edb32-263">Komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu.</span><span class="sxs-lookup"><span data-stu-id="edb32-263">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="edb32-264">Poniższy kod powoduje wypełnienie modelu widoku `Enrollments` właściwości po wybraniu kursu:</span><span class="sxs-lookup"><span data-stu-id="edb32-264">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="edb32-265">Dodaj następujący kod na końcu *Pages/Instructors/Index.cshtml* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="edb32-265">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="edb32-266">Poprzedni kod znaczników Wyświetla listę kursów związane z kierunkiem instruktora, po wybraniu pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="edb32-266">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="edb32-267">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-267">Test the app.</span></span> <span data-ttu-id="edb32-268">Kliknij pozycję **wybierz** łącze na stronie instruktorów.</span><span class="sxs-lookup"><span data-stu-id="edb32-268">Click on a **Select** link on the instructors page.</span></span>

![Wybrane przez instruktorów indeks strony instruktorów](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="edb32-270">Pokaż dane dla uczniów</span><span class="sxs-lookup"><span data-stu-id="edb32-270">Show student data</span></span>

<span data-ttu-id="edb32-271">W tej sekcji gdy aplikacja jest zaktualizowana do wyświetlenia danych edukacyjnych do wybranych kursów.</span><span class="sxs-lookup"><span data-stu-id="edb32-271">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="edb32-272">Zaktualizuj zapytanie w `OnGetAsync` method in Class metoda *Pages/Instructors/Index.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edb32-272">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="edb32-273">Aktualizacja *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="edb32-273">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="edb32-274">Dodaj następujący kod na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="edb32-274">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="edb32-275">Poprzedni kod znaczników Wyświetla listę uczniów, którzy są rejestrowane w wybranych kursów.</span><span class="sxs-lookup"><span data-stu-id="edb32-275">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="edb32-276">Odśwież stronę, a następnie wybierz pozycję pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="edb32-276">Refresh the page and select an instructor.</span></span> <span data-ttu-id="edb32-277">Wybierz kurs, aby wyświetlić listę zarejestrowanych studentów i ich klas.</span><span class="sxs-lookup"><span data-stu-id="edb32-277">Select a course to see the list of enrolled students and their grades.</span></span>

![Instruktorzy indeks strony instruktora oraz przypisane kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="edb32-279">Za pomocą pojedynczego</span><span class="sxs-lookup"><span data-stu-id="edb32-279">Using Single</span></span>

<span data-ttu-id="edb32-280">`Single` Przekazać metoda `Where` warunku, zamiast wywoływać metodę `Where` metoda oddzielnie:</span><span class="sxs-lookup"><span data-stu-id="edb32-280">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="edb32-281">Poprzedni `Single` podejście zapewnia nie korzyści za pośrednictwem za pomocą `Where`.</span><span class="sxs-lookup"><span data-stu-id="edb32-281">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="edb32-282">Niektórzy deweloperzy Preferuj `Single` podejście stylu.</span><span class="sxs-lookup"><span data-stu-id="edb32-282">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="edb32-283">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="edb32-283">Explicit loading</span></span>

<span data-ttu-id="edb32-284">Bieżący kod określa wczesne ładowanie dla `Enrollments` i `Students`:</span><span class="sxs-lookup"><span data-stu-id="edb32-284">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="edb32-285">Załóżmy, że użytkownicy rzadko mają być wyświetlane rejestracji w szkoleniu.</span><span class="sxs-lookup"><span data-stu-id="edb32-285">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="edb32-286">W takim przypadku danych rejestracji należy załadowywać tylko, jeśli żądanie jest optymalizacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-286">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="edb32-287">W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawne ładowanie `Enrollments` i `Students`.</span><span class="sxs-lookup"><span data-stu-id="edb32-287">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="edb32-288">Aktualizacja `OnGetAsync` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="edb32-288">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="edb32-289">Powyższy kod spada *ThenInclude* metoda wywołuje danych rejestracji i uczniów.</span><span class="sxs-lookup"><span data-stu-id="edb32-289">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="edb32-290">Jeśli kurs jest zaznaczone, umożliwia pobranie wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="edb32-290">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="edb32-291">`Enrollment` Jednostki dla wybranych kursów.</span><span class="sxs-lookup"><span data-stu-id="edb32-291">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="edb32-292">`Student` Jednostki dla każdej `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="edb32-292">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="edb32-293">Zwróć uwagę, poprzedni kod komentarze `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="edb32-293">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="edb32-294">Właściwości nawigacji można jawnie ładować tylko dla obiektów śledzonych.</span><span class="sxs-lookup"><span data-stu-id="edb32-294">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="edb32-295">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="edb32-295">Test the app.</span></span> <span data-ttu-id="edb32-296">Z punktu widzenia użytkowników aplikacji działa identycznie do poprzedniej wersji.</span><span class="sxs-lookup"><span data-stu-id="edb32-296">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="edb32-297">Następny samouczek pokazuje, jak aktualizowanie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="edb32-297">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="edb32-298">[Poprzednie](xref:data/ef-rp/complex-data-model)
>[dalej](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="edb32-298">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
