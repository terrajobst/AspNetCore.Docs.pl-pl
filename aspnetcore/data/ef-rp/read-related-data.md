---
title: Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8
author: rick-anderson
description: W tym samouczku odczytasz i wyświetlasz powiązane dane, czyli dane, które Entity Framework ładowane do właściwości nawigacji.
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: d244ce1527486466bcbc6557ec35869aa206bc4f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656577"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="1ec67-103">Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8</span><span class="sxs-lookup"><span data-stu-id="1ec67-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="1ec67-104">Autorzy [Dykstra](https://github.com/tdykstra), [Jan P Kowalski](https://twitter.com/thereformedprog)i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1ec67-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1ec67-105">W tym samouczku pokazano, jak odczytywać i wyświetlać powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="1ec67-106">Powiązane dane to dane, które EF Core ładowane do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="1ec67-107">Na poniższych ilustracjach przedstawiono ukończone strony dla tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="1ec67-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index30.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="1ec67-110">Eager, jawne i ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="1ec67-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="1ec67-111">Istnieje kilka sposobów, EF Core mogą ładować powiązane dane do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="1ec67-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="1ec67-112">[Ładowanie eager](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="1ec67-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="1ec67-113">Ładowanie eager jest, gdy zapytanie dla jednego typu jednostki również ładuje powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="1ec67-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="1ec67-114">Po odczytaniu jednostki pobierane są powiązane z nią dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="1ec67-115">Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne.</span><span class="sxs-lookup"><span data-stu-id="1ec67-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="1ec67-116">EF Core będzie wystawiał wiele zapytań dla niektórych typów ładowania eager.</span><span class="sxs-lookup"><span data-stu-id="1ec67-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="1ec67-117">Wygenerowanie wielu zapytań może być bardziej wydajne niż bardzo duże pojedyncze zapytanie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="1ec67-118">Ładowanie eager jest określone przy użyciu metod `Include` i `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Przykład ładowania eager](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="1ec67-120">Podczas ładowania eager są wysyłane wiele zapytań, gdy zostanie dołączona Nawigacja kolekcji:</span><span class="sxs-lookup"><span data-stu-id="1ec67-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="1ec67-121">Jedno zapytanie dotyczące głównej kwerendy</span><span class="sxs-lookup"><span data-stu-id="1ec67-121">One query for the main query</span></span> 
  * <span data-ttu-id="1ec67-122">Jedno zapytanie dla każdej kolekcji "Edge" w drzewie ładowania.</span><span class="sxs-lookup"><span data-stu-id="1ec67-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="1ec67-123">Oddziel zapytania z `Load`: dane można pobrać w oddzielnych zapytaniach, a EF Core "naprawia" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="1ec67-124">"Rozwiązuje" oznacza, że EF Core automatycznie wypełnia właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="1ec67-125">Oddzielne zapytania o `Load` są bardziej podobne do jawnego ładowania niż ładowanie eager.</span><span class="sxs-lookup"><span data-stu-id="1ec67-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Przykład oddzielnych zapytań](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="1ec67-127">Uwaga: EF Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-127">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="1ec67-128">Nawet jeśli dane dla właściwości nawigacji *nie* są jawnie uwzględniane, właściwość można nadal wypełnić, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="1ec67-129">[Jawne ładowanie](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="1ec67-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="1ec67-130">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="1ec67-131">Kod musi być zapisany, aby można było pobrać powiązane dane, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="1ec67-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="1ec67-132">Jawne ładowanie z oddzielnymi zapytania powoduje wysłanie wielu zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1ec67-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="1ec67-133">W przypadku jawnego ładowania kod określa właściwości nawigacji do załadowania.</span><span class="sxs-lookup"><span data-stu-id="1ec67-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="1ec67-134">Użyj metody `Load`, aby przeprowadzić jawne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="1ec67-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1ec67-135">For example:</span></span>

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="1ec67-137">[Ładowanie z opóźnieniem](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="1ec67-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="1ec67-138">[Ładowanie z opóźnieniem zostało dodane do EF Core w wersji 2,1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="1ec67-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="1ec67-139">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="1ec67-140">Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="1ec67-141">Zapytanie jest wysyłane do bazy danych przy każdym dostępie do właściwości nawigacji po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="1ec67-142">Tworzenie stron kursu</span><span class="sxs-lookup"><span data-stu-id="1ec67-142">Create Course pages</span></span>

<span data-ttu-id="1ec67-143">Jednostka `Course` zawiera właściwość nawigacji, która zawiera powiązaną `Department` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="1ec67-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Kurs. Dział](read-related-data/_static/dep-crs.png)

<span data-ttu-id="1ec67-145">Aby wyświetlić nazwę przypisanego działu dla kursu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="1ec67-146">Załaduj powiązaną jednostkę `Department` do właściwości nawigacji `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="1ec67-147">Pobierz nazwę z właściwości `Name` jednostki `Department`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="1ec67-148">Strony kursu szkieletowego</span><span class="sxs-lookup"><span data-stu-id="1ec67-148">Scaffold Course pages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="1ec67-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ec67-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1ec67-150">Postępuj zgodnie z instrukcjami na [stronach uczniów tworzenia szkieletów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="1ec67-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="1ec67-151">Utwórz folder *strony/kursy* .</span><span class="sxs-lookup"><span data-stu-id="1ec67-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="1ec67-152">Użyj `Course` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="1ec67-153">Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.</span><span class="sxs-lookup"><span data-stu-id="1ec67-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="1ec67-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1ec67-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1ec67-155">Utwórz folder *strony/kursy* .</span><span class="sxs-lookup"><span data-stu-id="1ec67-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="1ec67-156">Uruchom następujące polecenie, aby połączyć strony kursu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="1ec67-157">**W systemie Windows:**</span><span class="sxs-lookup"><span data-stu-id="1ec67-157">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="1ec67-158">**W systemie Linux lub macOS:**</span><span class="sxs-lookup"><span data-stu-id="1ec67-158">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="1ec67-159">Otwórz *stronę/kursy/index. cshtml. cs* i Przeanalizuj metodę `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="1ec67-160">Aparat szkieletu określony eager ładowania dla właściwości nawigacji `Department`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="1ec67-161">Metoda `Include` określa ładowanie eager.</span><span class="sxs-lookup"><span data-stu-id="1ec67-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="1ec67-162">Uruchom aplikację i wybierz łącze **kursy** .</span><span class="sxs-lookup"><span data-stu-id="1ec67-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="1ec67-163">W kolumnie dział zostanie wyświetlona `DepartmentID`, co nie jest przydatne.</span><span class="sxs-lookup"><span data-stu-id="1ec67-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="1ec67-164">Wyświetl nazwę działu</span><span class="sxs-lookup"><span data-stu-id="1ec67-164">Display the department name</span></span>

<span data-ttu-id="1ec67-165">Zaktualizuj strony/kursy/index. cshtml. cs przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="1ec67-166">Poprzedni kod zmienia właściwość `Course` na `Courses` i dodaje `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="1ec67-167">`AsNoTracking` zwiększa wydajność, ponieważ zwrócone jednostki nie są śledzone.</span><span class="sxs-lookup"><span data-stu-id="1ec67-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="1ec67-168">Nie trzeba śledzić jednostek, ponieważ nie są one aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="1ec67-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="1ec67-169">Zaktualizuj *strony/kursy/index. cshtml* przy użyciu następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="1ec67-170">W kodzie szkieletowym wprowadzono następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="1ec67-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="1ec67-171">Zmieniono nazwę właściwości `Course` na `Courses`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="1ec67-172">Dodano kolumnę **liczbową** , która wyświetla wartość właściwości `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="1ec67-173">Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są oznaczane przez użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="1ec67-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="1ec67-174">Jednak w tym przypadku klucz podstawowy ma znaczenie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="1ec67-175">Zmieniono kolumnę **działu** , aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="1ec67-176">Kod wyświetla Właściwość `Name` jednostki `Department`, która jest ładowana do właściwości nawigacji `Department`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="1ec67-177">Uruchom aplikację i wybierz kartę **kursy** , aby wyświetlić listę z nazwami działów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="1ec67-179">Ładowanie powiązanych danych przy użyciu opcji Select</span><span class="sxs-lookup"><span data-stu-id="1ec67-179">Loading related data with Select</span></span>

<span data-ttu-id="1ec67-180">Metoda `OnGetAsync` ładuje powiązane dane przy użyciu metody `Include`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="1ec67-181">Metoda `Select` jest alternatywą, która ładuje tylko powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="1ec67-182">Dla pojedynczych elementów, takich jak `Department.Name` używa SPRZĘŻENIa wewnętrznego SQL.</span><span class="sxs-lookup"><span data-stu-id="1ec67-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="1ec67-183">W przypadku kolekcji jest on wykorzystywany przez inny dostęp do bazy danych, ale w związku z tym wykonuje operator `Include` w kolekcjach.</span><span class="sxs-lookup"><span data-stu-id="1ec67-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="1ec67-184">Poniższy kod ładuje powiązane dane przy użyciu metody `Select`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="1ec67-185">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-185">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="1ec67-186">Zobacz [IndexSelect. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) , aby zapoznać się z kompletnym przykładem.</span><span class="sxs-lookup"><span data-stu-id="1ec67-186">See [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="1ec67-187">Utwórz strony instruktora</span><span class="sxs-lookup"><span data-stu-id="1ec67-187">Create Instructor pages</span></span>

<span data-ttu-id="1ec67-188">Ta sekcja szkieletuje strony instruktorów i dodaje powiązane kursy i rejestracje do strony indeksu instruktorów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-188">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="1ec67-189">![strony indeksu instruktorów](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="1ec67-189">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="1ec67-190">Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1ec67-190">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="1ec67-191">Lista instruktorów wyświetla powiązane dane z jednostki `OfficeAssignment` (Office na powyższym obrazie).</span><span class="sxs-lookup"><span data-stu-id="1ec67-191">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="1ec67-192">Jednostki `Instructor` i `OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego.</span><span class="sxs-lookup"><span data-stu-id="1ec67-192">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="1ec67-193">Ładowanie eager jest używane dla jednostek `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-193">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="1ec67-194">Ładowanie eager jest zwykle wydajniejsze, gdy wymagane jest wyświetlenie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="1ec67-194">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="1ec67-195">W takim przypadku wyświetlane są przypisania pakietu Office dla instruktorów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-195">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="1ec67-196">Gdy użytkownik wybierze instruktora, zostaną wyświetlone powiązane jednostki `Course`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-196">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="1ec67-197">Jednostki `Instructor` i `Course` znajdują się w relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-197">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="1ec67-198">Ładowanie eager jest używane dla jednostek `Course` i powiązanych `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="1ec67-198">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="1ec67-199">W takim przypadku oddzielne zapytania mogą być bardziej wydajne, ponieważ potrzebują tylko kursów dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-199">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="1ec67-200">Ten przykład pokazuje, jak używać eager ładowania dla właściwości nawigacji w jednostkach, które są we właściwościach nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-200">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="1ec67-201">Gdy użytkownik wybierze kurs, wyświetlane są powiązane dane z jednostki `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-201">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="1ec67-202">Na powyższym obrazie wyświetlana jest nazwa ucznia i jej klasy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-202">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="1ec67-203">Jednostki `Course` i `Enrollment` znajdują się w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-203">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="1ec67-204">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="1ec67-204">Create a view model</span></span>

<span data-ttu-id="1ec67-205">Na stronie instruktorzy są wyświetlane dane z trzech różnych tabel.</span><span class="sxs-lookup"><span data-stu-id="1ec67-205">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="1ec67-206">Wymagany jest model widoku, który zawiera trzy właściwości reprezentujące trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="1ec67-206">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="1ec67-207">Utwórz *SchoolViewModels/InstructorIndexData. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-207">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="1ec67-208">Strony instruktorów dla szkieletów</span><span class="sxs-lookup"><span data-stu-id="1ec67-208">Scaffold Instructor pages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="1ec67-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ec67-209">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1ec67-210">Postępuj zgodnie z instrukcjami w temacie Tworzenie [szkieletu stron uczniów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="1ec67-210">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="1ec67-211">Utwórz folder *stron/instruktorów* .</span><span class="sxs-lookup"><span data-stu-id="1ec67-211">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="1ec67-212">Użyj `Instructor` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-212">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="1ec67-213">Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.</span><span class="sxs-lookup"><span data-stu-id="1ec67-213">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="1ec67-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1ec67-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1ec67-215">Utwórz folder *stron/instruktorów* .</span><span class="sxs-lookup"><span data-stu-id="1ec67-215">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="1ec67-216">Uruchom następujące polecenie, aby połączyć strony instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-216">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="1ec67-217">**W systemie Windows:**</span><span class="sxs-lookup"><span data-stu-id="1ec67-217">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="1ec67-218">**W systemie Linux lub macOS:**</span><span class="sxs-lookup"><span data-stu-id="1ec67-218">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="1ec67-219">Aby sprawdzić, jak wygląda strona szkieletowa przed aktualizacją, uruchom aplikację i przejdź do strony instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-219">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="1ec67-220">Aktualizowanie *stron/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-220">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="1ec67-221">Metoda `OnGetAsync` akceptuje opcjonalne dane trasy dla identyfikatora wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="1ec67-222">Zbadaj zapytanie w pliku */instruktors/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="1ec67-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="1ec67-223">Kod określa eager ładowania dla następujących właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="1ec67-223">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="1ec67-224">Zwróć uwagę na powtarzanie `Include` i `ThenInclude` metod `CourseAssignments` i `Course`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-224">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="1ec67-225">To powtórzenie jest niezbędne do określenia eager ładowania dla dwóch właściwości nawigacji jednostki `Course`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-225">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="1ec67-226">Poniższy kod jest wykonywany po wybraniu instruktora (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="1ec67-226">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="1ec67-227">Wybrany instruktor jest pobierany z listy instruktorów w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="1ec67-227">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="1ec67-228">Właściwość `Courses` modelu widoku jest ładowana z jednostkami `Course`, które są `CourseAssignments` właściwości nawigacji tego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-228">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="1ec67-229">Metoda `Where` zwraca kolekcję.</span><span class="sxs-lookup"><span data-stu-id="1ec67-229">The `Where` method returns a collection.</span></span> <span data-ttu-id="1ec67-230">Ale w tym przypadku filtr wybierze pojedynczą jednostkę.</span><span class="sxs-lookup"><span data-stu-id="1ec67-230">But in this case, the filter will select a single entity.</span></span> <span data-ttu-id="1ec67-231">Dlatego metoda `Single` jest wywoływana w celu przekonwertowania kolekcji na jedną jednostkę `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-231">so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="1ec67-232">Jednostka `Instructor` zapewnia dostęp do właściwości `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-232">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="1ec67-233">`CourseAssignments` zapewnia dostęp do powiązanych jednostek `Course`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-233">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="1ec67-235">Metoda `Single` jest używana w kolekcji, gdy kolekcja zawiera tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="1ec67-235">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="1ec67-236">Metoda `Single` zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="1ec67-236">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="1ec67-237">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (null w tym przypadku), jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="1ec67-237">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="1ec67-238">Poniższy kod wypełnia Właściwość `Enrollments` modelu widoku w przypadku wybrania kursu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-238">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="1ec67-239">Aktualizowanie strony indeksu instruktorów</span><span class="sxs-lookup"><span data-stu-id="1ec67-239">Update the instructors Index page</span></span>

<span data-ttu-id="1ec67-240">Aktualizowanie *stron/instruktorów/index. cshtml* przy użyciu następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-240">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="1ec67-241">Poprzedni kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="1ec67-241">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="1ec67-242">Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-242">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="1ec67-243">`"{id:int?}"` jest szablonem trasy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-243">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="1ec67-244">Szablon trasy zmienia ciągi zapytań liczb całkowitych w adresie URL, aby przesyłać dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-244">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="1ec67-245">Na przykład kliknięcie linku **Wybierz** dla instruktora z tylko dyrektywą `@page` generuje adres URL podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="1ec67-245">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="1ec67-246">Gdy dyrektywa Page jest `@page "{id:int?}"`, adres URL to:</span><span class="sxs-lookup"><span data-stu-id="1ec67-246">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="1ec67-247">Dodaje kolumnę **pakietu Office** , która wyświetla `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="1ec67-247">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="1ec67-248">Ponieważ jest to relacja "jeden do zera" lub "jeden", nie może być powiązana jednostka OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="1ec67-248">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="1ec67-249">Dodaje kolumnę **kursów** , która wyświetla nauczanie kursów przez każdego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-249">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="1ec67-250">Aby uzyskać więcej informacji na temat tej składni Razor, zobacz [jawne przejście liniowe](xref:mvc/views/razor#explicit-line-transition) .</span><span class="sxs-lookup"><span data-stu-id="1ec67-250">See [Explicit line transition](xref:mvc/views/razor#explicit-line-transition) for more about this razor syntax.</span></span>

* <span data-ttu-id="1ec67-251">Dodaje kod, który dynamicznie dodaje `class="success"` do elementu `tr` wybranego instruktora i kursu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-251">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="1ec67-252">Ustawia kolor tła dla wybranego wiersza przy użyciu klasy Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="1ec67-252">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="1ec67-253">Dodaje nowe hiperłącze z etykietą **SELECT**.</span><span class="sxs-lookup"><span data-stu-id="1ec67-253">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="1ec67-254">Ten link powoduje wysłanie wybranego identyfikatora instruktora do metody `Index` i ustawienie koloru tła.</span><span class="sxs-lookup"><span data-stu-id="1ec67-254">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="1ec67-255">Dodaje tabelę kursów dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-255">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="1ec67-256">Dodaje tabelę rejestracji uczniów dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-256">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="1ec67-257">Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie zostanie wyświetlona `Location` (Biuro) z jednostki powiązanej `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-257">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="1ec67-258">Jeśli `OfficeAssignment` ma wartość null, zostanie wyświetlona pusta komórka tabeli.</span><span class="sxs-lookup"><span data-stu-id="1ec67-258">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="1ec67-259">Kliknij link **Wybierz** dla instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-259">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="1ec67-260">Zostaną wyświetlone zmiany w stylu wiersza i kursy przypisane do tego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-260">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="1ec67-261">Wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-261">Select a course to see the list of enrolled students and their grades.</span></span>

![Wybrany instruktor i kurs dla instruktorów](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="1ec67-263">Korzystanie z jednego</span><span class="sxs-lookup"><span data-stu-id="1ec67-263">Using Single</span></span>

<span data-ttu-id="1ec67-264">Metoda `Single` może przekazać warunek `Where` zamiast wywoływania `Where` metody oddzielnie:</span><span class="sxs-lookup"><span data-stu-id="1ec67-264">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="1ec67-265">Użycie `Single` z warunkiem WHERE jest kwestią preferencji osobistych.</span><span class="sxs-lookup"><span data-stu-id="1ec67-265">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="1ec67-266">Nie oferuje żadnych korzyści z używania metody `Where`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-266">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="1ec67-267">jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="1ec67-267">Explicit loading</span></span>

<span data-ttu-id="1ec67-268">Bieżący kod określa eager ładowania dla `Enrollments` i `Students`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-268">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="1ec67-269">Załóżmy, że użytkownicy rzadko chcą widzieć rejestracje w kursie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-269">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="1ec67-270">W takim przypadku Optymalizacja będzie ładować dane rejestracji tylko wtedy, gdy jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-270">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="1ec67-271">W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawnego ładowania `Enrollments` i `Students`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-271">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="1ec67-272">Aktualizowanie *stron/instruktorów/index. cshtml. cs* przy użyciu następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-272">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="1ec67-273">Poprzedni kod odrzuca metody *ThenInclude* w celu uzyskania danych dotyczących rejestracji i uczniów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-273">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="1ec67-274">W przypadku wybrania kursu jawny kod ładowania pobiera:</span><span class="sxs-lookup"><span data-stu-id="1ec67-274">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="1ec67-275">Jednostki `Enrollment` dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-275">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="1ec67-276">`Student` jednostek dla każdej `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-276">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="1ec67-277">Zwróć uwagę, że powyższy kod komentarz ma `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-277">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="1ec67-278">Właściwości nawigacji można jawnie załadować tylko dla śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="1ec67-278">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="1ec67-279">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-279">Test the app.</span></span> <span data-ttu-id="1ec67-280">Z perspektywy użytkownika aplikacja zachowuje się identycznie z poprzednią wersją.</span><span class="sxs-lookup"><span data-stu-id="1ec67-280">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ec67-281">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="1ec67-281">Next steps</span></span>

<span data-ttu-id="1ec67-282">W następnym samouczku pokazano, jak zaktualizować powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-282">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="1ec67-283">[Poprzedni samouczek](xref:data/ef-rp/complex-data-model)
>[następnego samouczka](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="1ec67-283">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1ec67-284">W tym samouczku dane pokrewne są odczytywane i wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-284">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="1ec67-285">Powiązane dane to dane, które EF Core ładowane do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-285">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="1ec67-286">Jeśli występują problemy, których nie można rozwiązać, [Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="1ec67-286">If you run into problems you can't solve, [download or view the completed app.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="1ec67-287">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="1ec67-287">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="1ec67-288">Na poniższych ilustracjach przedstawiono ukończone strony dla tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="1ec67-288">The following illustrations show the completed pages for this tutorial:</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="1ec67-291">Eager, jawne i opóźnione ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="1ec67-291">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="1ec67-292">Istnieje kilka sposobów, EF Core mogą ładować powiązane dane do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="1ec67-292">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="1ec67-293">[Ładowanie eager](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="1ec67-293">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="1ec67-294">Ładowanie eager jest, gdy zapytanie dla jednego typu jednostki również ładuje powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="1ec67-294">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="1ec67-295">Po odczytaniu jednostki są pobierane powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-295">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="1ec67-296">Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne.</span><span class="sxs-lookup"><span data-stu-id="1ec67-296">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="1ec67-297">EF Core będzie wystawiał wiele zapytań dla niektórych typów ładowania eager.</span><span class="sxs-lookup"><span data-stu-id="1ec67-297">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="1ec67-298">Wygenerowanie wielu zapytań może być bardziej wydajne niż w przypadku niektórych zapytań w EF6, w których wystąpiło pojedyncze zapytanie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-298">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="1ec67-299">Ładowanie eager jest określone przy użyciu metod `Include` i `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-299">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Przykład ładowania eager](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="1ec67-301">Podczas ładowania eager są wysyłane wiele zapytań, gdy zostanie dołączona Nawigacja kolekcji:</span><span class="sxs-lookup"><span data-stu-id="1ec67-301">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="1ec67-302">Jedno zapytanie dotyczące głównej kwerendy</span><span class="sxs-lookup"><span data-stu-id="1ec67-302">One query for the main query</span></span> 
  * <span data-ttu-id="1ec67-303">Jedno zapytanie dla każdej kolekcji "Edge" w drzewie ładowania.</span><span class="sxs-lookup"><span data-stu-id="1ec67-303">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="1ec67-304">Oddziel zapytania z `Load`: dane można pobrać w oddzielnych zapytaniach, a EF Core "naprawia" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-304">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="1ec67-305">"rozwiązuje" oznacza, że EF Core automatycznie wypełnia właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-305">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="1ec67-306">Oddzielne zapytania o `Load` są bardziej podobne do jawnego ładowania niż ładowanie eager.</span><span class="sxs-lookup"><span data-stu-id="1ec67-306">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Przykład oddzielnych zapytań](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="1ec67-308">Uwaga: EF Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-308">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="1ec67-309">Nawet jeśli dane dla właściwości nawigacji *nie* są jawnie uwzględniane, właściwość można nadal wypełnić, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-309">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="1ec67-310">[Jawne ładowanie](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="1ec67-310">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="1ec67-311">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-311">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="1ec67-312">Kod musi być zapisany, aby można było pobrać powiązane dane, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="1ec67-312">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="1ec67-313">Jawne ładowanie z oddzielnymi zapytaniami powoduje wysłanie wielu zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1ec67-313">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="1ec67-314">W przypadku jawnego ładowania kod określa właściwości nawigacji do załadowania.</span><span class="sxs-lookup"><span data-stu-id="1ec67-314">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="1ec67-315">Użyj metody `Load`, aby przeprowadzić jawne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-315">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="1ec67-316">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1ec67-316">For example:</span></span>

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="1ec67-318">[Ładowanie z opóźnieniem](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="1ec67-318">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="1ec67-319">[Ładowanie z opóźnieniem zostało dodane do EF Core w wersji 2,1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="1ec67-319">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="1ec67-320">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-320">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="1ec67-321">Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-321">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="1ec67-322">Zapytanie jest wysyłane do bazy danych przy każdym dostępie do właściwości nawigacji po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-322">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="1ec67-323">Operator `Select` ładuje tylko powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-323">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="1ec67-324">Tworzenie strony kursu, która wyświetla nazwę działu</span><span class="sxs-lookup"><span data-stu-id="1ec67-324">Create a Course page that displays department name</span></span>

<span data-ttu-id="1ec67-325">Jednostka kursu zawiera właściwość nawigacji, która zawiera jednostkę `Department`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-325">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="1ec67-326">Jednostka `Department` zawiera dział, do którego jest przypisany kurs.</span><span class="sxs-lookup"><span data-stu-id="1ec67-326">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="1ec67-327">Aby wyświetlić nazwę przypisanego działu na liście kursów:</span><span class="sxs-lookup"><span data-stu-id="1ec67-327">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="1ec67-328">Pobierz Właściwość `Name` z jednostki `Department`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-328">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="1ec67-329">Jednostka `Department` pochodzi z właściwości nawigacji `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-329">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Kurs. Dział](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="1ec67-331">Tworzenie szkieletu modelu kursu</span><span class="sxs-lookup"><span data-stu-id="1ec67-331">Scaffold the Course model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="1ec67-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ec67-332">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="1ec67-333">Postępuj zgodnie z instrukcjami w obszarze [szkieletem model studenta](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Course` dla klasy model.</span><span class="sxs-lookup"><span data-stu-id="1ec67-333">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="1ec67-334">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1ec67-334">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="1ec67-335">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1ec67-335">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="1ec67-336">Poprzednie polecenie szkieletuje model `Course`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-336">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="1ec67-337">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ec67-337">Open the project in Visual Studio.</span></span>

<span data-ttu-id="1ec67-338">Otwórz *stronę/kursy/index. cshtml. cs* i Przeanalizuj metodę `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-338">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="1ec67-339">Aparat szkieletu określony eager ładowania dla właściwości nawigacji `Department`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-339">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="1ec67-340">Metoda `Include` określa ładowanie eager.</span><span class="sxs-lookup"><span data-stu-id="1ec67-340">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="1ec67-341">Uruchom aplikację i wybierz łącze **kursy** .</span><span class="sxs-lookup"><span data-stu-id="1ec67-341">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="1ec67-342">W kolumnie dział zostanie wyświetlona `DepartmentID`, co nie jest przydatne.</span><span class="sxs-lookup"><span data-stu-id="1ec67-342">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="1ec67-343">Zaktualizuj metodę `OnGetAsync` przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-343">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="1ec67-344">Poprzedni kod dodaje `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-344">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="1ec67-345">`AsNoTracking` zwiększa wydajność, ponieważ zwrócone jednostki nie są śledzone.</span><span class="sxs-lookup"><span data-stu-id="1ec67-345">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="1ec67-346">Jednostki nie są śledzone, ponieważ nie są aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="1ec67-346">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="1ec67-347">Aktualizuj *strony/kursy/index. cshtml* z następującymi wyróżnionymi znacznikami:</span><span class="sxs-lookup"><span data-stu-id="1ec67-347">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="1ec67-348">W kodzie szkieletowym wprowadzono następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="1ec67-348">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="1ec67-349">Zmieniono nagłówek z indeksu na kursy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-349">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="1ec67-350">Dodano kolumnę **liczbową** , która wyświetla wartość właściwości `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-350">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="1ec67-351">Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są oznaczane przez użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="1ec67-351">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="1ec67-352">Jednak w tym przypadku klucz podstawowy ma znaczenie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-352">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="1ec67-353">Zmieniono kolumnę **działu** , aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-353">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="1ec67-354">Kod wyświetla Właściwość `Name` jednostki `Department`, która jest ładowana do właściwości nawigacji `Department`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-354">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="1ec67-355">Uruchom aplikację i wybierz kartę **kursy** , aby wyświetlić listę z nazwami działów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-355">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="1ec67-357">Ładowanie powiązanych danych przy użyciu opcji Select</span><span class="sxs-lookup"><span data-stu-id="1ec67-357">Loading related data with Select</span></span>

<span data-ttu-id="1ec67-358">Metoda `OnGetAsync` ładuje powiązane dane przy użyciu metody `Include`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-358">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="1ec67-359">Operator `Select` ładuje tylko powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-359">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="1ec67-360">Dla pojedynczych elementów, takich jak `Department.Name` używa SPRZĘŻENIa wewnętrznego SQL.</span><span class="sxs-lookup"><span data-stu-id="1ec67-360">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="1ec67-361">W przypadku kolekcji jest on wykorzystywany przez inny dostęp do bazy danych, ale w związku z tym wykonuje operator `Include` w kolekcjach.</span><span class="sxs-lookup"><span data-stu-id="1ec67-361">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="1ec67-362">Poniższy kod ładuje powiązane dane przy użyciu metody `Select`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-362">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="1ec67-363">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-363">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="1ec67-364">Zobacz [IndexSelect. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) , aby zapoznać się z kompletnym przykładem.</span><span class="sxs-lookup"><span data-stu-id="1ec67-364">See [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="1ec67-365">Utwórz stronę instruktorów pokazującą kursy i rejestracje</span><span class="sxs-lookup"><span data-stu-id="1ec67-365">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="1ec67-366">W tej sekcji zostanie utworzona strona instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-366">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="1ec67-367">![strony indeksu instruktorów](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="1ec67-367">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="1ec67-368">Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1ec67-368">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="1ec67-369">Lista instruktorów wyświetla powiązane dane z jednostki `OfficeAssignment` (Office na powyższym obrazie).</span><span class="sxs-lookup"><span data-stu-id="1ec67-369">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="1ec67-370">Jednostki `Instructor` i `OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego.</span><span class="sxs-lookup"><span data-stu-id="1ec67-370">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="1ec67-371">Ładowanie eager jest używane dla jednostek `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-371">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="1ec67-372">Ładowanie eager jest zwykle wydajniejsze, gdy wymagane jest wyświetlenie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="1ec67-372">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="1ec67-373">W takim przypadku wyświetlane są przypisania pakietu Office dla instruktorów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-373">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="1ec67-374">Gdy użytkownik wybierze instruktora (Harui na poprzednim obrazie), zostaną wyświetlone powiązane jednostki `Course`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-374">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="1ec67-375">Jednostki `Instructor` i `Course` znajdują się w relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-375">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="1ec67-376">Ładowanie eager jest używane dla jednostek `Course` i powiązanych `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="1ec67-376">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="1ec67-377">W takim przypadku oddzielne zapytania mogą być bardziej wydajne, ponieważ potrzebują tylko kursów dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-377">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="1ec67-378">Ten przykład pokazuje, jak używać eager ładowania dla właściwości nawigacji w jednostkach, które są we właściwościach nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-378">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="1ec67-379">Gdy użytkownik wybierze kurs (chemia na powyższym obrazie), zostaną wyświetlone powiązane dane z jednostki `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-379">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="1ec67-380">Na powyższym obrazie wyświetlana jest nazwa ucznia i jej klasy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-380">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="1ec67-381">Jednostki `Course` i `Enrollment` znajdują się w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-381">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="1ec67-382">Utwórz model widoku dla widoku indeksu instruktora</span><span class="sxs-lookup"><span data-stu-id="1ec67-382">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="1ec67-383">Na stronie instruktorzy są wyświetlane dane z trzech różnych tabel.</span><span class="sxs-lookup"><span data-stu-id="1ec67-383">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="1ec67-384">Tworzony jest model widoku, który zawiera trzy jednostki reprezentujące trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="1ec67-384">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="1ec67-385">W folderze *SchoolViewModels* Utwórz *InstructorIndexData.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-385">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="1ec67-386">Tworzenie szkieletu modelu instruktora</span><span class="sxs-lookup"><span data-stu-id="1ec67-386">Scaffold the Instructor model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="1ec67-387">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ec67-387">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="1ec67-388">Postępuj zgodnie z instrukcjami w obszarze [szkieletem model studenta](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Instructor` dla klasy model.</span><span class="sxs-lookup"><span data-stu-id="1ec67-388">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="1ec67-389">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1ec67-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="1ec67-390">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1ec67-390">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="1ec67-391">Poprzednie polecenie szkieletuje model `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-391">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="1ec67-392">Uruchom aplikację i przejdź do strony instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-392">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="1ec67-393">Zamień *strony/instruktorów/index. cshtml. cs* na następujący kod:</span><span class="sxs-lookup"><span data-stu-id="1ec67-393">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="1ec67-394">Metoda `OnGetAsync` akceptuje opcjonalne dane trasy dla identyfikatora wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-394">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="1ec67-395">Zbadaj zapytanie w pliku */instruktors/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="1ec67-395">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="1ec67-396">Zapytanie zawiera dwa:</span><span class="sxs-lookup"><span data-stu-id="1ec67-396">The query has two includes:</span></span>

* <span data-ttu-id="1ec67-397">`OfficeAssignment`: wyświetlane w [widoku instruktorów](#IP).</span><span class="sxs-lookup"><span data-stu-id="1ec67-397">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="1ec67-398">`CourseAssignments`: co to jest w nauczaniu kursów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-398">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="1ec67-399">Aktualizowanie strony indeksu instruktorów</span><span class="sxs-lookup"><span data-stu-id="1ec67-399">Update the instructors Index page</span></span>

<span data-ttu-id="1ec67-400">Aktualizowanie *stron/instruktorów/index. cshtml* przy użyciu następującego znacznika:</span><span class="sxs-lookup"><span data-stu-id="1ec67-400">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="1ec67-401">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="1ec67-401">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="1ec67-402">Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-402">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="1ec67-403">`"{id:int?}"` jest szablonem trasy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-403">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="1ec67-404">Szablon trasy zmienia ciągi zapytań liczb całkowitych w adresie URL, aby przesyłać dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-404">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="1ec67-405">Na przykład kliknięcie linku **Wybierz** dla instruktora z tylko dyrektywą `@page` generuje adres URL podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="1ec67-405">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="1ec67-406">Gdy dyrektywa Page jest `@page "{id:int?}"`, poprzedni adres URL to:</span><span class="sxs-lookup"><span data-stu-id="1ec67-406">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="1ec67-407">Tytuł strony to **Instruktorzy**.</span><span class="sxs-lookup"><span data-stu-id="1ec67-407">Page title is **Instructors**.</span></span>
* <span data-ttu-id="1ec67-408">Dodano kolumnę **pakietu Office** , która wyświetla `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="1ec67-408">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="1ec67-409">Ponieważ jest to relacja "jeden do zera" lub "jeden", nie może być powiązana jednostka OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="1ec67-409">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="1ec67-410">Dodano kolumnę **kursów** , która wyświetla nauczanie kursów przez każdego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-410">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="1ec67-411">Aby uzyskać więcej informacji na temat tej składni Razor, zobacz [jawne przejście liniowe](xref:mvc/views/razor#explicit-line-transition) .</span><span class="sxs-lookup"><span data-stu-id="1ec67-411">See [Explicit line transition](xref:mvc/views/razor#explicit-line-transition) for more about this razor syntax.</span></span>

* <span data-ttu-id="1ec67-412">Dodano kod, który dynamicznie dodaje `class="success"` do elementu `tr` wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-412">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="1ec67-413">Ustawia kolor tła dla wybranego wiersza przy użyciu klasy Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="1ec67-413">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="1ec67-414">Dodano nowe hiperłącze z etykietą **SELECT**.</span><span class="sxs-lookup"><span data-stu-id="1ec67-414">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="1ec67-415">Ten link powoduje wysłanie wybranego identyfikatora instruktora do metody `Index` i ustawienie koloru tła.</span><span class="sxs-lookup"><span data-stu-id="1ec67-415">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="1ec67-416">Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie zostanie wyświetlona `Location` (Biuro) z jednostki powiązanej `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-416">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="1ec67-417">Jeśli OfficeAssignment ' ma wartość null, zostanie wyświetlona pusta komórka tabeli.</span><span class="sxs-lookup"><span data-stu-id="1ec67-417">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="1ec67-418">Kliknij link **Wybierz** .</span><span class="sxs-lookup"><span data-stu-id="1ec67-418">Click on the **Select** link.</span></span> <span data-ttu-id="1ec67-419">Styl wiersza zmienia się.</span><span class="sxs-lookup"><span data-stu-id="1ec67-419">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="1ec67-420">Dodaj nauczanie kursów według wybranych instruktorów</span><span class="sxs-lookup"><span data-stu-id="1ec67-420">Add courses taught by selected instructor</span></span>

<span data-ttu-id="1ec67-421">Zaktualizuj metodę `OnGetAsync` na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-421">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="1ec67-422">Dodaj `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="1ec67-422">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="1ec67-423">Zbadaj zaktualizowane zapytanie:</span><span class="sxs-lookup"><span data-stu-id="1ec67-423">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="1ec67-424">Poprzednie zapytanie dodaje jednostki `Department`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-424">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="1ec67-425">Poniższy kod jest wykonywany po wybraniu instruktora (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="1ec67-425">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="1ec67-426">Wybrany instruktor jest pobierany z listy instruktorów w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="1ec67-426">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="1ec67-427">Właściwość `Courses` modelu widoku jest ładowana z jednostkami `Course`, które są `CourseAssignments` właściwości nawigacji tego instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-427">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="1ec67-428">Metoda `Where` zwraca kolekcję.</span><span class="sxs-lookup"><span data-stu-id="1ec67-428">The `Where` method returns a collection.</span></span> <span data-ttu-id="1ec67-429">W poprzedniej metodzie `Where` zwracana jest tylko jedna jednostka `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-429">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="1ec67-430">Metoda `Single` konwertuje kolekcję na jedną jednostkę `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-430">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="1ec67-431">Jednostka `Instructor` zapewnia dostęp do właściwości `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-431">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="1ec67-432">`CourseAssignments` zapewnia dostęp do powiązanych jednostek `Course`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-432">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="1ec67-434">Metoda `Single` jest używana w kolekcji, gdy kolekcja zawiera tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="1ec67-434">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="1ec67-435">Metoda `Single` zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="1ec67-435">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="1ec67-436">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (null w tym przypadku), jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="1ec67-436">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="1ec67-437">Używanie `SingleOrDefault` w pustej kolekcji:</span><span class="sxs-lookup"><span data-stu-id="1ec67-437">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="1ec67-438">Wynikiem jest wyjątek (od próby znalezienia właściwości `Courses` w odwołaniu o wartości null).</span><span class="sxs-lookup"><span data-stu-id="1ec67-438">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="1ec67-439">Komunikat o wyjątku będzie mniej jasno wskazywał przyczynę problemu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-439">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="1ec67-440">Poniższy kod wypełnia Właściwość `Enrollments` modelu widoku w przypadku wybrania kursu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-440">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="1ec67-441">Dodaj następujący znacznik na końcu strony */instruktorów/index. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="1ec67-441">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="1ec67-442">Powyższy znacznik wyświetla listę kursów związanych z instruktorem w przypadku wybrania instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-442">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="1ec67-443">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-443">Test the app.</span></span> <span data-ttu-id="1ec67-444">Kliknij link **Wybierz** na stronie instruktorów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-444">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="1ec67-445">Pokaż dane ucznia</span><span class="sxs-lookup"><span data-stu-id="1ec67-445">Show student data</span></span>

<span data-ttu-id="1ec67-446">W tej sekcji aplikacja zostanie zaktualizowana, aby wyświetlić dane uczniów dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-446">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="1ec67-447">Zaktualizuj zapytanie w metodzie `OnGetAsync` na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-447">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="1ec67-448">Aktualizowanie *stron/instruktorów/index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1ec67-448">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="1ec67-449">Dodaj następujący znacznik na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="1ec67-449">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="1ec67-450">Powyższy znacznik wyświetla listę studentów, którzy są zarejestrowani w wybranym kursie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-450">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="1ec67-451">Odśwież stronę i wybierz instruktora.</span><span class="sxs-lookup"><span data-stu-id="1ec67-451">Refresh the page and select an instructor.</span></span> <span data-ttu-id="1ec67-452">Wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.</span><span class="sxs-lookup"><span data-stu-id="1ec67-452">Select a course to see the list of enrolled students and their grades.</span></span>

![Wybrany instruktor i kurs dla instruktorów](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="1ec67-454">Korzystanie z jednego</span><span class="sxs-lookup"><span data-stu-id="1ec67-454">Using Single</span></span>

<span data-ttu-id="1ec67-455">Metoda `Single` może przekazać warunek `Where` zamiast wywoływania `Where` metody oddzielnie:</span><span class="sxs-lookup"><span data-stu-id="1ec67-455">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="1ec67-456">Poprzednie podejście `Single` nie zapewnia żadnych korzyści z używania `Where`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-456">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="1ec67-457">Niektórzy deweloperzy preferują styl podejścia `Single`owego.</span><span class="sxs-lookup"><span data-stu-id="1ec67-457">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="1ec67-458">jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="1ec67-458">Explicit loading</span></span>

<span data-ttu-id="1ec67-459">Bieżący kod określa eager ładowania dla `Enrollments` i `Students`:</span><span class="sxs-lookup"><span data-stu-id="1ec67-459">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="1ec67-460">Załóżmy, że użytkownicy rzadko chcą widzieć rejestracje w kursie.</span><span class="sxs-lookup"><span data-stu-id="1ec67-460">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="1ec67-461">W takim przypadku Optymalizacja będzie ładować dane rejestracji tylko wtedy, gdy jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-461">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="1ec67-462">W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawnego ładowania `Enrollments` i `Students`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-462">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="1ec67-463">Zaktualizuj `OnGetAsync` przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1ec67-463">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="1ec67-464">Poprzedni kod odrzuca metody *ThenInclude* w celu uzyskania danych dotyczących rejestracji i uczniów.</span><span class="sxs-lookup"><span data-stu-id="1ec67-464">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="1ec67-465">W przypadku wybrania kursu wyróżniony kod pobiera:</span><span class="sxs-lookup"><span data-stu-id="1ec67-465">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="1ec67-466">Jednostki `Enrollment` dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="1ec67-466">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="1ec67-467">`Student` jednostek dla każdej `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-467">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="1ec67-468">Zwróć uwagę na powyższy kod komentarza `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="1ec67-468">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="1ec67-469">Właściwości nawigacji można jawnie załadować tylko dla śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="1ec67-469">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="1ec67-470">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ec67-470">Test the app.</span></span> <span data-ttu-id="1ec67-471">Z perspektywy użytkowników aplikacja zachowuje się identycznie z poprzednią wersją.</span><span class="sxs-lookup"><span data-stu-id="1ec67-471">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="1ec67-472">W następnym samouczku pokazano, jak zaktualizować powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="1ec67-472">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ec67-473">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1ec67-473">Additional resources</span></span>

* [<span data-ttu-id="1ec67-474">Wersja tego samouczka usługi YouTube (part1)</span><span class="sxs-lookup"><span data-stu-id="1ec67-474">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="1ec67-475">Wersja tego samouczka usługi YouTube (part2)</span><span class="sxs-lookup"><span data-stu-id="1ec67-475">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="1ec67-476">[Poprzednie](xref:data/ef-rp/complex-data-model)
>[dalej](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="1ec67-476">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end
