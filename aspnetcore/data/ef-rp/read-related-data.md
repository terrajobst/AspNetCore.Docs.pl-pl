---
title: Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8
author: tdykstra
description: W tym samouczku odczytasz i wyświetlasz powiązane dane, czyli dane, które Entity Framework ładowane do właściwości nawigacji.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 62224312aa9b7f3e0164b5300e491f59b0832acd
ms.sourcegitcommit: 776598f71da0d1e4c9e923b3b395d3c3b5825796
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/26/2019
ms.locfileid: "70024721"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="df95d-103">Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8</span><span class="sxs-lookup"><span data-stu-id="df95d-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="df95d-104">Autorzy [Dykstra](https://github.com/tdykstra), [Jan P Kowalski](https://twitter.com/thereformedprog)i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="df95d-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="df95d-105">W tym samouczku pokazano, jak odczytywać i wyświetlać powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="df95d-106">Powiązane dane to dane, które EF Core ładowane do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="df95d-107">Na poniższych ilustracjach przedstawiono ukończone strony dla tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="df95d-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index30.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="df95d-110">Eager, jawne i ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="df95d-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="df95d-111">Istnieje kilka sposobów, EF Core mogą ładować powiązane dane do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="df95d-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="df95d-112">[Ładowanie eager](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="df95d-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="df95d-113">Ładowanie eager jest, gdy zapytanie dla jednego typu jednostki również ładuje powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="df95d-114">Po odczytaniu jednostki pobierane są powiązane z nią dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="df95d-115">Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne.</span><span class="sxs-lookup"><span data-stu-id="df95d-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="df95d-116">EF Core będzie wystawiał wiele zapytań dla niektórych typów ładowania eager.</span><span class="sxs-lookup"><span data-stu-id="df95d-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="df95d-117">Wygenerowanie wielu zapytań może być bardziej wydajne niż bardzo duże pojedyncze zapytanie.</span><span class="sxs-lookup"><span data-stu-id="df95d-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="df95d-118">Ładowanie eager jest określone przy użyciu `Include` metod `ThenInclude` i.</span><span class="sxs-lookup"><span data-stu-id="df95d-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Przykład ładowania eager](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="df95d-120">Podczas ładowania eager są wysyłane wiele zapytań, gdy zostanie dołączona Nawigacja kolekcji:</span><span class="sxs-lookup"><span data-stu-id="df95d-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="df95d-121">Jedno zapytanie dotyczące głównej kwerendy</span><span class="sxs-lookup"><span data-stu-id="df95d-121">One query for the main query</span></span> 
  * <span data-ttu-id="df95d-122">Jedno zapytanie dla każdej kolekcji "Edge" w drzewie ładowania.</span><span class="sxs-lookup"><span data-stu-id="df95d-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="df95d-123">Oddziel zapytania `Load`z: Dane można pobrać w oddzielnych zapytaniach, a EF Core "naprawia" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="df95d-124">"Rozwiązuje" oznacza, że EF Core automatycznie wypełnia właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="df95d-125">Osobne zapytania `Load` i są bardziej podobne do jawnego ładowania niż ładowanie eager.</span><span class="sxs-lookup"><span data-stu-id="df95d-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Przykład oddzielnych zapytań](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="df95d-127">Uwaga: EF Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="df95d-127">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="df95d-128">Nawet jeśli dane dla właściwości nawigacji *nie* są jawnie uwzględniane, właściwość można nadal wypełnić, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="df95d-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="df95d-129">[Jawne ładowanie](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="df95d-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="df95d-130">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="df95d-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="df95d-131">Kod musi być zapisany, aby można było pobrać powiązane dane, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="df95d-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="df95d-132">Jawne ładowanie z oddzielnymi zapytania powoduje wysłanie wielu zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="df95d-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="df95d-133">W przypadku jawnego ładowania kod określa właściwości nawigacji do załadowania.</span><span class="sxs-lookup"><span data-stu-id="df95d-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="df95d-134">Użyj metody `Load` , aby przeprowadzić jawne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="df95d-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="df95d-135">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df95d-135">For example:</span></span>

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="df95d-137">[Ładowanie](/ef/core/querying/related-data#lazy-loading)z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="df95d-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="df95d-138">[Ładowanie z opóźnieniem zostało dodane do EF Core w wersji 2,1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="df95d-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="df95d-139">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="df95d-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="df95d-140">Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="df95d-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="df95d-141">Zapytanie jest wysyłane do bazy danych przy każdym dostępie do właściwości nawigacji po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="df95d-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="df95d-142">Tworzenie stron kursu</span><span class="sxs-lookup"><span data-stu-id="df95d-142">Create Course pages</span></span>

<span data-ttu-id="df95d-143">Jednostka zawiera właściwość nawigacji, która zawiera powiązaną `Department` jednostkę. `Course`</span><span class="sxs-lookup"><span data-stu-id="df95d-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Kurs. Dział](read-related-data/_static/dep-crs.png)

<span data-ttu-id="df95d-145">Aby wyświetlić nazwę przypisanego działu dla kursu:</span><span class="sxs-lookup"><span data-stu-id="df95d-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="df95d-146">Załaduj powiązaną `Department` jednostkę `Course.Department` do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="df95d-147">Pobierz nazwę z `Department` `Name` właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="df95d-148">Strony kursu szkieletowego</span><span class="sxs-lookup"><span data-stu-id="df95d-148">Scaffold Course pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="df95d-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="df95d-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="df95d-150">Postępuj zgodnie z instrukcjami na [stronach uczniów](xref:data/ef-rp/intro#scaffold-student-pages) tworzenia szkieletów z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="df95d-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="df95d-151">Utwórz folder *strony/kursy* .</span><span class="sxs-lookup"><span data-stu-id="df95d-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="df95d-152">Użyj `Course` dla klasy model.</span><span class="sxs-lookup"><span data-stu-id="df95d-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="df95d-153">Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.</span><span class="sxs-lookup"><span data-stu-id="df95d-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="df95d-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="df95d-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="df95d-155">Utwórz folder *strony/kursy* .</span><span class="sxs-lookup"><span data-stu-id="df95d-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="df95d-156">Uruchom następujące polecenie, aby połączyć strony kursu.</span><span class="sxs-lookup"><span data-stu-id="df95d-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="df95d-157">**Na Windows:**</span><span class="sxs-lookup"><span data-stu-id="df95d-157">**On Windows:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="df95d-158">**W systemie Linux lub macOS:**</span><span class="sxs-lookup"><span data-stu-id="df95d-158">**On Linux or macOS:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="df95d-159">Otwórz *stronę/kursy/index. cshtml. cs* i Przeanalizuj `OnGetAsync` metodę.</span><span class="sxs-lookup"><span data-stu-id="df95d-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="df95d-160">Aparat tworzenia szkieletów określił eager ładowania dla `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="df95d-161">`Include` Metoda określa eager ładowania.</span><span class="sxs-lookup"><span data-stu-id="df95d-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="df95d-162">Uruchom aplikację i wybierz łącze **kursy** .</span><span class="sxs-lookup"><span data-stu-id="df95d-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="df95d-163">W kolumnie dział zostanie wyświetlona wartość `DepartmentID`, która nie jest przydatna.</span><span class="sxs-lookup"><span data-stu-id="df95d-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="df95d-164">Wyświetl nazwę działu</span><span class="sxs-lookup"><span data-stu-id="df95d-164">Display the department name</span></span>

<span data-ttu-id="df95d-165">Zaktualizuj strony/kursy/index. cshtml. cs przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="df95d-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="df95d-166">Poprzedni kod zmienia `Course` właściwość na `Courses` i dodaje `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="df95d-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="df95d-167">`AsNoTracking`zwiększa wydajność, ponieważ zwrócone jednostki nie są śledzone.</span><span class="sxs-lookup"><span data-stu-id="df95d-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="df95d-168">Nie trzeba śledzić jednostek, ponieważ nie są one aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="df95d-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="df95d-169">Zaktualizuj *strony/kursy/index. cshtml* przy użyciu następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="df95d-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="df95d-170">W kodzie szkieletowym wprowadzono następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="df95d-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="df95d-171">`Courses`Zmieniono nazwę `Course` właściwości na.</span><span class="sxs-lookup"><span data-stu-id="df95d-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="df95d-172">Dodano kolumnę **liczbową** , która wyświetla `CourseID` wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="df95d-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="df95d-173">Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są oznaczane przez użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="df95d-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="df95d-174">Jednak w tym przypadku klucz podstawowy ma znaczenie.</span><span class="sxs-lookup"><span data-stu-id="df95d-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="df95d-175">Zmieniono kolumnę **działu** , aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="df95d-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="df95d-176">Kod wyświetla `Name` Właściwość `Department` jednostki `Department` , która jest ładowana do właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="df95d-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="df95d-177">Uruchom aplikację i wybierz kartę **kursy** , aby wyświetlić listę z nazwami działów.</span><span class="sxs-lookup"><span data-stu-id="df95d-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="df95d-179">Ładowanie powiązanych danych przy użyciu opcji Select</span><span class="sxs-lookup"><span data-stu-id="df95d-179">Loading related data with Select</span></span>

<span data-ttu-id="df95d-180">Metoda ładuje powiązane dane `Include` przy użyciu metody. `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="df95d-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="df95d-181">`Select` Metoda jest alternatywą, która ładuje tylko powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="df95d-182">Dla pojedynczych elementów, podobnie jak `Department.Name` w przypadku użycia sprzężenia wewnętrznego SQL.</span><span class="sxs-lookup"><span data-stu-id="df95d-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="df95d-183">W przypadku kolekcji program używa innego dostępu do bazy danych, ale w `Include` związku z tym wykonuje operator dla kolekcji.</span><span class="sxs-lookup"><span data-stu-id="df95d-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="df95d-184">Poniższy kod ładuje powiązane dane przy użyciu `Select` metody:</span><span class="sxs-lookup"><span data-stu-id="df95d-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="df95d-185">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="df95d-185">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="df95d-186">Zobacz [IndexSelect. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) , aby zapoznać się z kompletnym przykładem.</span><span class="sxs-lookup"><span data-stu-id="df95d-186">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="df95d-187">Utwórz strony instruktora</span><span class="sxs-lookup"><span data-stu-id="df95d-187">Create Instructor pages</span></span>

<span data-ttu-id="df95d-188">Ta sekcja szkieletuje strony instruktorów i dodaje powiązane kursy i rejestracje do strony indeksu instruktorów.</span><span class="sxs-lookup"><span data-stu-id="df95d-188">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="df95d-189">![Strona indeksu instruktorów](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="df95d-189">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="df95d-190">Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="df95d-190">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="df95d-191">Lista instruktorów wyświetla powiązane dane z `OfficeAssignment` jednostki (Office na powyższym obrazie).</span><span class="sxs-lookup"><span data-stu-id="df95d-191">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="df95d-192">Jednostki `Instructor` i`OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego.</span><span class="sxs-lookup"><span data-stu-id="df95d-192">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="df95d-193">Eager ładowania jest używany dla `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="df95d-193">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="df95d-194">Ładowanie eager jest zwykle wydajniejsze, gdy wymagane jest wyświetlenie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="df95d-194">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="df95d-195">W takim przypadku wyświetlane są przypisania pakietu Office dla instruktorów.</span><span class="sxs-lookup"><span data-stu-id="df95d-195">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="df95d-196">Gdy użytkownik wybierze instruktora, wyświetlane są `Course` powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-196">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="df95d-197">Jednostki `Instructor` i`Course` znajdują się w relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="df95d-197">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="df95d-198">Ładowanie eager jest używane dla `Course` jednostek i ich powiązanych `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="df95d-198">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="df95d-199">W takim przypadku oddzielne zapytania mogą być bardziej wydajne, ponieważ potrzebują tylko kursów dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-199">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="df95d-200">Ten przykład pokazuje, jak używać eager ładowania dla właściwości nawigacji w jednostkach, które są we właściwościach nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-200">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="df95d-201">Gdy użytkownik wybierze kurs, zostaną wyświetlone powiązane dane z `Enrollments` jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-201">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="df95d-202">Na powyższym obrazie wyświetlana jest nazwa ucznia i jej klasy.</span><span class="sxs-lookup"><span data-stu-id="df95d-202">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="df95d-203">Jednostki `Course` i`Enrollment` znajdują się w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="df95d-203">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="df95d-204">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="df95d-204">Create a view model</span></span>

<span data-ttu-id="df95d-205">Na stronie instruktorzy są wyświetlane dane z trzech różnych tabel.</span><span class="sxs-lookup"><span data-stu-id="df95d-205">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="df95d-206">Wymagany jest model widoku, który zawiera trzy właściwości reprezentujące trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="df95d-206">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="df95d-207">Utwórz *SchoolViewModels/InstructorIndexData. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="df95d-207">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="df95d-208">Strony instruktorów dla szkieletów</span><span class="sxs-lookup"><span data-stu-id="df95d-208">Scaffold Instructor pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="df95d-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="df95d-209">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="df95d-210">Postępuj zgodnie z instrukcjami w temacie Tworzenie [szkieletu stron uczniów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="df95d-210">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="df95d-211">Utwórz folder *stron/instruktorów* .</span><span class="sxs-lookup"><span data-stu-id="df95d-211">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="df95d-212">Użyj `Instructor` dla klasy model.</span><span class="sxs-lookup"><span data-stu-id="df95d-212">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="df95d-213">Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.</span><span class="sxs-lookup"><span data-stu-id="df95d-213">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="df95d-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="df95d-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="df95d-215">Utwórz folder *stron/instruktorów* .</span><span class="sxs-lookup"><span data-stu-id="df95d-215">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="df95d-216">Uruchom następujące polecenie, aby połączyć strony instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-216">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="df95d-217">**Na Windows:**</span><span class="sxs-lookup"><span data-stu-id="df95d-217">**On Windows:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="df95d-218">**W systemie Linux lub macOS:**</span><span class="sxs-lookup"><span data-stu-id="df95d-218">**On Linux or macOS:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="df95d-219">Aby sprawdzić, jak wygląda strona szkieletowa przed aktualizacją, uruchom aplikację i przejdź do strony instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="df95d-219">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="df95d-220">Aktualizowanie *stron/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="df95d-220">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="df95d-221">`OnGetAsync` Metoda akceptuje opcjonalne dane trasy dla identyfikatora wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="df95d-222">Zbadaj zapytanie w pliku */instruktors/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="df95d-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="df95d-223">Kod określa eager ładowania dla następujących właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="df95d-223">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="df95d-224">Zwróć `Include` uwagę na powtarzanie i `ThenInclude` metody dla `CourseAssignments` i `Course`.</span><span class="sxs-lookup"><span data-stu-id="df95d-224">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="df95d-225">To powtórzenie jest niezbędne do określenia ładowania eager dla dwóch właściwości `Course` nawigacji jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-225">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="df95d-226">Poniższy kod jest wykonywany po wybraniu instruktora (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="df95d-226">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="df95d-227">Wybrany instruktor jest pobierany z listy instruktorów w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="df95d-227">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="df95d-228">`Courses` Właściwość widoku modelu jest ładowana `Course` z jednostkami `CourseAssignments` z tej właściwości nawigacji instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-228">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="df95d-229">`Where` Metoda zwraca kolekcję.</span><span class="sxs-lookup"><span data-stu-id="df95d-229">The `Where` method returns a collection.</span></span> <span data-ttu-id="df95d-230">Ale w tym przypadku filtr wybierze pojedynczą jednostkę.</span><span class="sxs-lookup"><span data-stu-id="df95d-230">But in this case, the filter will select a single entity.</span></span> <span data-ttu-id="df95d-231">Dlatego metoda jest wywoływana w celu przekonwertowania kolekcji na jedną `Instructor` jednostkę. `Single`</span><span class="sxs-lookup"><span data-stu-id="df95d-231">so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="df95d-232">Jednostka zapewnia dostęp `CourseAssignments` do właściwości. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="df95d-232">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="df95d-233">`CourseAssignments`zapewnia dostęp do powiązanych `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="df95d-233">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="df95d-235">`Single` Metoda jest używana w kolekcji, gdy kolekcja zawiera tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="df95d-235">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="df95d-236">`Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="df95d-236">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="df95d-237">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (null w tym przypadku), jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="df95d-237">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="df95d-238">Poniższy kod wypełnia `Enrollments` właściwość modelu widoku w przypadku wybrania kursu:</span><span class="sxs-lookup"><span data-stu-id="df95d-238">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="df95d-239">Aktualizowanie strony indeksu instruktorów</span><span class="sxs-lookup"><span data-stu-id="df95d-239">Update the instructors Index page</span></span>

<span data-ttu-id="df95d-240">Aktualizowanie *stron/instruktorów/index. cshtml* przy użyciu następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="df95d-240">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="df95d-241">Poprzedni kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="df95d-241">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="df95d-242">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="df95d-242">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="df95d-243">`"{id:int?}"`jest szablonem trasy.</span><span class="sxs-lookup"><span data-stu-id="df95d-243">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="df95d-244">Szablon trasy zmienia ciągi zapytań liczb całkowitych w adresie URL, aby przesyłać dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-244">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="df95d-245">Na przykład kliknięcie linku **Wybierz** dla instruktora z tylko `@page` dyrektywą spowoduje utworzenie adresu URL w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="df95d-245">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="df95d-246">Gdy dyrektywa Page ma `@page "{id:int?}"`wartość, adres URL to:</span><span class="sxs-lookup"><span data-stu-id="df95d-246">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="df95d-247">Dodaje kolumnę **pakietu Office** , która `item.OfficeAssignment.Location` jest wyświetlana `item.OfficeAssignment` tylko wtedy, gdy nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="df95d-247">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="df95d-248">Ponieważ jest to relacja "jeden do zera" lub "jeden", nie może być powiązana jednostka OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="df95d-248">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="df95d-249">Dodaje kolumnę **kursów** , która wyświetla nauczanie kursów przez każdego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-249">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="df95d-250">Aby uzyskać więcej informacji na temat składni Razor, zobacz [ `@:` jawne przejście liniowe](xref:mvc/views/razor#explicit-line-transition-with-) .</span><span class="sxs-lookup"><span data-stu-id="df95d-250">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="df95d-251">Dodaje kod, który dynamicznie `class="success"` dodaje `tr` do elementu wybranego instruktora i kursu.</span><span class="sxs-lookup"><span data-stu-id="df95d-251">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="df95d-252">Ustawia kolor tła dla wybranego wiersza przy użyciu klasy Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="df95d-252">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="df95d-253">Dodaje nowe hiperłącze z etykietą **SELECT**.</span><span class="sxs-lookup"><span data-stu-id="df95d-253">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="df95d-254">Ten link powoduje wysłanie wybranego identyfikatora instruktora do `Index` metody i ustawienie koloru tła.</span><span class="sxs-lookup"><span data-stu-id="df95d-254">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="df95d-255">Dodaje tabelę kursów dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-255">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="df95d-256">Dodaje tabelę rejestracji uczniów dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="df95d-256">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="df95d-257">Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie zostanie wyświetlona `Location` strona (Biuro) z jednostki powiązanej. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="df95d-257">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="df95d-258">Jeśli `OfficeAssignment` ma wartość null, zostanie wyświetlona pusta komórka tabeli.</span><span class="sxs-lookup"><span data-stu-id="df95d-258">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="df95d-259">Kliknij link **Wybierz** dla instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-259">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="df95d-260">Zostaną wyświetlone zmiany w stylu wiersza i kursy przypisane do tego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-260">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="df95d-261">Wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.</span><span class="sxs-lookup"><span data-stu-id="df95d-261">Select a course to see the list of enrolled students and their grades.</span></span>

![Wybrany instruktor i kurs dla instruktorów](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="df95d-263">Korzystanie z jednego</span><span class="sxs-lookup"><span data-stu-id="df95d-263">Using Single</span></span>

<span data-ttu-id="df95d-264">Metoda może `Where` przekazać`Where` warunek zamiast wywołania metody osobno: `Single`</span><span class="sxs-lookup"><span data-stu-id="df95d-264">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="df95d-265">`Single` Użycie z warunkiem WHERE jest kwestią preferencji osobistych.</span><span class="sxs-lookup"><span data-stu-id="df95d-265">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="df95d-266">Nie zapewnia żadnych korzyści z używania `Where` metody.</span><span class="sxs-lookup"><span data-stu-id="df95d-266">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="df95d-267">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="df95d-267">Explicit loading</span></span>

<span data-ttu-id="df95d-268">Bieżący kod określa eager ładowania dla `Enrollments` i: `Students`</span><span class="sxs-lookup"><span data-stu-id="df95d-268">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="df95d-269">Załóżmy, że użytkownicy rzadko chcą widzieć rejestracje w kursie.</span><span class="sxs-lookup"><span data-stu-id="df95d-269">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="df95d-270">W takim przypadku Optymalizacja będzie ładować dane rejestracji tylko wtedy, gdy jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="df95d-270">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="df95d-271">W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawnego `Enrollments` ładowania i `Students`.</span><span class="sxs-lookup"><span data-stu-id="df95d-271">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="df95d-272">Aktualizowanie *stron/instruktorów/index. cshtml. cs* przy użyciu następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="df95d-272">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="df95d-273">Poprzedni kod odrzuca metody *ThenInclude* w celu uzyskania danych dotyczących rejestracji i uczniów.</span><span class="sxs-lookup"><span data-stu-id="df95d-273">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="df95d-274">W przypadku wybrania kursu jawny kod ładowania pobiera:</span><span class="sxs-lookup"><span data-stu-id="df95d-274">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="df95d-275">`Enrollment` Jednostki wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="df95d-275">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="df95d-276">Jednostki `Student` dla każdej z `Enrollment`nich.</span><span class="sxs-lookup"><span data-stu-id="df95d-276">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="df95d-277">Zwróć uwagę, że poprzedzający kod `.AsNoTracking()`komentarz ma wartość.</span><span class="sxs-lookup"><span data-stu-id="df95d-277">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="df95d-278">Właściwości nawigacji można jawnie załadować tylko dla śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="df95d-278">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="df95d-279">Przetestuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="df95d-279">Test the app.</span></span> <span data-ttu-id="df95d-280">Z perspektywy użytkownika aplikacja zachowuje się identycznie z poprzednią wersją.</span><span class="sxs-lookup"><span data-stu-id="df95d-280">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df95d-281">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="df95d-281">Next steps</span></span>

<span data-ttu-id="df95d-282">W następnym samouczku pokazano, jak zaktualizować powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-282">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="df95d-283">[Poprzedni](xref:data/ef-rp/complex-data-model)
>samouczek w[następnym](xref:data/ef-rp/update-related-data) samouczku</span><span class="sxs-lookup"><span data-stu-id="df95d-283">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="df95d-284">W tym samouczku dane pokrewne są odczytywane i wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="df95d-284">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="df95d-285">Powiązane dane to dane, które EF Core ładowane do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-285">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="df95d-286">Jeśli napotkasz problemy, nie można rozwiązać, [pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="df95d-286">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="df95d-287">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="df95d-287">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="df95d-288">Na poniższych ilustracjach przedstawiono ukończone strony dla tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="df95d-288">The following illustrations show the completed pages for this tutorial:</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="df95d-291">Eager, jawne i opóźnione ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="df95d-291">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="df95d-292">Istnieje kilka sposobów, EF Core mogą ładować powiązane dane do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="df95d-292">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="df95d-293">[Ładowanie eager](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="df95d-293">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="df95d-294">Ładowanie eager jest, gdy zapytanie dla jednego typu jednostki również ładuje powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-294">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="df95d-295">Po odczytaniu jednostki są pobierane powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-295">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="df95d-296">Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne.</span><span class="sxs-lookup"><span data-stu-id="df95d-296">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="df95d-297">EF Core będzie wystawiał wiele zapytań dla niektórych typów ładowania eager.</span><span class="sxs-lookup"><span data-stu-id="df95d-297">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="df95d-298">Wygenerowanie wielu zapytań może być bardziej wydajne niż w przypadku niektórych zapytań w EF6, w których wystąpiło pojedyncze zapytanie.</span><span class="sxs-lookup"><span data-stu-id="df95d-298">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="df95d-299">Ładowanie eager jest określone przy użyciu `Include` metod `ThenInclude` i.</span><span class="sxs-lookup"><span data-stu-id="df95d-299">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Przykład ładowania eager](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="df95d-301">Podczas ładowania eager są wysyłane wiele zapytań, gdy zostanie dołączona Nawigacja kolekcji:</span><span class="sxs-lookup"><span data-stu-id="df95d-301">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="df95d-302">Jedno zapytanie dotyczące głównej kwerendy</span><span class="sxs-lookup"><span data-stu-id="df95d-302">One query for the main query</span></span> 
  * <span data-ttu-id="df95d-303">Jedno zapytanie dla każdej kolekcji "Edge" w drzewie ładowania.</span><span class="sxs-lookup"><span data-stu-id="df95d-303">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="df95d-304">Oddziel zapytania `Load`z: Dane można pobrać w oddzielnych zapytaniach, a EF Core "naprawia" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-304">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="df95d-305">"rozwiązuje" oznacza, że EF Core automatycznie wypełnia właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-305">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="df95d-306">Osobne zapytania `Load` i są bardziej podobne do jawnego ładowania niż ładowanie eager.</span><span class="sxs-lookup"><span data-stu-id="df95d-306">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Przykład oddzielnych zapytań](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="df95d-308">Uwaga: EF Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="df95d-308">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="df95d-309">Nawet jeśli dane dla właściwości nawigacji *nie* są jawnie uwzględniane, właściwość można nadal wypełnić, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="df95d-309">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="df95d-310">[Jawne ładowanie](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="df95d-310">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="df95d-311">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="df95d-311">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="df95d-312">Kod musi być zapisany, aby można było pobrać powiązane dane, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="df95d-312">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="df95d-313">Jawne ładowanie z oddzielnymi zapytaniami powoduje wysłanie wielu zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="df95d-313">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="df95d-314">W przypadku jawnego ładowania kod określa właściwości nawigacji do załadowania.</span><span class="sxs-lookup"><span data-stu-id="df95d-314">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="df95d-315">Użyj metody `Load` , aby przeprowadzić jawne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="df95d-315">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="df95d-316">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="df95d-316">For example:</span></span>

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="df95d-318">[Ładowanie](/ef/core/querying/related-data#lazy-loading)z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="df95d-318">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="df95d-319">[Ładowanie z opóźnieniem zostało dodane do EF Core w wersji 2,1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="df95d-319">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="df95d-320">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="df95d-320">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="df95d-321">Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="df95d-321">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="df95d-322">Zapytanie jest wysyłane do bazy danych przy każdym dostępie do właściwości nawigacji po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="df95d-322">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="df95d-323">`Select` Operator ładuje tylko powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-323">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="df95d-324">Tworzenie strony kursu, która wyświetla nazwę działu</span><span class="sxs-lookup"><span data-stu-id="df95d-324">Create a Course page that displays department name</span></span>

<span data-ttu-id="df95d-325">Jednostka kursu zawiera właściwość nawigacji, która zawiera `Department` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="df95d-325">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="df95d-326">`Department` Jednostka zawiera dział, do którego jest przypisany kurs.</span><span class="sxs-lookup"><span data-stu-id="df95d-326">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="df95d-327">Aby wyświetlić nazwę przypisanego działu na liście kursów:</span><span class="sxs-lookup"><span data-stu-id="df95d-327">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="df95d-328">`Name` Pobierz Właściwość`Department` z jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-328">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="df95d-329">`Department` Jednostka pochodzi `Course.Department` z właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-329">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Kurs. Dział](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="df95d-331">Tworzenie szkieletu modelu kursu</span><span class="sxs-lookup"><span data-stu-id="df95d-331">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="df95d-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="df95d-332">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="df95d-333">Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Course` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="df95d-333">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="df95d-334">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="df95d-334">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="df95d-335">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="df95d-335">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="df95d-336">Poprzedni szkielety mechanizmów polecenia `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="df95d-336">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="df95d-337">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df95d-337">Open the project in Visual Studio.</span></span>

<span data-ttu-id="df95d-338">Otwórz *stronę/kursy/index. cshtml. cs* i Przeanalizuj `OnGetAsync` metodę.</span><span class="sxs-lookup"><span data-stu-id="df95d-338">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="df95d-339">Aparat tworzenia szkieletów określił eager ładowania dla `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-339">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="df95d-340">`Include` Metoda określa eager ładowania.</span><span class="sxs-lookup"><span data-stu-id="df95d-340">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="df95d-341">Uruchom aplikację i wybierz łącze **kursy** .</span><span class="sxs-lookup"><span data-stu-id="df95d-341">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="df95d-342">W kolumnie dział zostanie wyświetlona wartość `DepartmentID`, która nie jest przydatna.</span><span class="sxs-lookup"><span data-stu-id="df95d-342">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="df95d-343">`OnGetAsync` Zaktualizuj metodę przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="df95d-343">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="df95d-344">Poprzedni kod dodaje `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="df95d-344">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="df95d-345">`AsNoTracking`zwiększa wydajność, ponieważ zwrócone jednostki nie są śledzone.</span><span class="sxs-lookup"><span data-stu-id="df95d-345">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="df95d-346">Jednostki nie są śledzone, ponieważ nie są aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="df95d-346">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="df95d-347">Aktualizuj *strony/kursy/index. cshtml* z następującymi wyróżnionymi znacznikami:</span><span class="sxs-lookup"><span data-stu-id="df95d-347">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="df95d-348">W kodzie szkieletowym wprowadzono następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="df95d-348">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="df95d-349">Zmieniono nagłówek z indeksu na kursy.</span><span class="sxs-lookup"><span data-stu-id="df95d-349">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="df95d-350">Dodano kolumnę **liczbową** , która wyświetla `CourseID` wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="df95d-350">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="df95d-351">Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są oznaczane przez użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="df95d-351">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="df95d-352">Jednak w tym przypadku klucz podstawowy ma znaczenie.</span><span class="sxs-lookup"><span data-stu-id="df95d-352">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="df95d-353">Zmieniono kolumnę **działu** , aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="df95d-353">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="df95d-354">Kod wyświetla `Name` Właściwość `Department` jednostki `Department` , która jest ładowana do właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="df95d-354">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="df95d-355">Uruchom aplikację i wybierz kartę **kursy** , aby wyświetlić listę z nazwami działów.</span><span class="sxs-lookup"><span data-stu-id="df95d-355">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="df95d-357">Ładowanie powiązanych danych przy użyciu opcji Select</span><span class="sxs-lookup"><span data-stu-id="df95d-357">Loading related data with Select</span></span>

<span data-ttu-id="df95d-358">Metoda ładuje powiązane dane `Include` przy użyciu metody: `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="df95d-358">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="df95d-359">`Select` Operator ładuje tylko powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-359">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="df95d-360">Dla pojedynczych elementów, podobnie jak `Department.Name` w przypadku użycia sprzężenia wewnętrznego SQL.</span><span class="sxs-lookup"><span data-stu-id="df95d-360">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="df95d-361">W przypadku kolekcji program używa innego dostępu do bazy danych, ale w `Include` związku z tym wykonuje operator dla kolekcji.</span><span class="sxs-lookup"><span data-stu-id="df95d-361">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="df95d-362">Poniższy kod ładuje powiązane dane przy użyciu `Select` metody:</span><span class="sxs-lookup"><span data-stu-id="df95d-362">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="df95d-363">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="df95d-363">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="df95d-364">Zobacz [IndexSelect. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) , aby zapoznać się z kompletnym przykładem.</span><span class="sxs-lookup"><span data-stu-id="df95d-364">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="df95d-365">Utwórz stronę instruktorów pokazującą kursy i rejestracje</span><span class="sxs-lookup"><span data-stu-id="df95d-365">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="df95d-366">W tej sekcji zostanie utworzona strona instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="df95d-366">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="df95d-367">![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="df95d-367">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="df95d-368">Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="df95d-368">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="df95d-369">Lista instruktorów wyświetla powiązane dane z `OfficeAssignment` jednostki (Office na powyższym obrazie).</span><span class="sxs-lookup"><span data-stu-id="df95d-369">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="df95d-370">Jednostki `Instructor` i`OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego.</span><span class="sxs-lookup"><span data-stu-id="df95d-370">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="df95d-371">Eager ładowania jest używany dla `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="df95d-371">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="df95d-372">Ładowanie eager jest zwykle wydajniejsze, gdy wymagane jest wyświetlenie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="df95d-372">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="df95d-373">W takim przypadku wyświetlane są przypisania pakietu Office dla instruktorów.</span><span class="sxs-lookup"><span data-stu-id="df95d-373">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="df95d-374">Gdy użytkownik wybierze instruktora (Harui na poprzednim obrazie), zostaną wyświetlone `Course` powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-374">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="df95d-375">Jednostki `Instructor` i`Course` znajdują się w relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="df95d-375">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="df95d-376">Ładowanie eager jest używane dla `Course` jednostek i ich powiązanych `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="df95d-376">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="df95d-377">W takim przypadku oddzielne zapytania mogą być bardziej wydajne, ponieważ potrzebują tylko kursów dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-377">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="df95d-378">Ten przykład pokazuje, jak używać eager ładowania dla właściwości nawigacji w jednostkach, które są we właściwościach nawigacji.</span><span class="sxs-lookup"><span data-stu-id="df95d-378">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="df95d-379">Gdy użytkownik wybierze kurs (chemia na powyższym obrazie), zostaną wyświetlone `Enrollments` powiązane dane z jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-379">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="df95d-380">Na powyższym obrazie wyświetlana jest nazwa ucznia i jej klasy.</span><span class="sxs-lookup"><span data-stu-id="df95d-380">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="df95d-381">Jednostki `Course` i`Enrollment` znajdują się w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="df95d-381">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="df95d-382">Utwórz model widoku dla widoku indeksu instruktora</span><span class="sxs-lookup"><span data-stu-id="df95d-382">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="df95d-383">Na stronie instruktorzy są wyświetlane dane z trzech różnych tabel.</span><span class="sxs-lookup"><span data-stu-id="df95d-383">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="df95d-384">Tworzony jest model widoku, który zawiera trzy jednostki reprezentujące trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="df95d-384">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="df95d-385">W folderze *SchoolViewModels* Utwórz *InstructorIndexData.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="df95d-385">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="df95d-386">Tworzenie szkieletu modelu instruktora</span><span class="sxs-lookup"><span data-stu-id="df95d-386">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="df95d-387">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="df95d-387">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="df95d-388">Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Instructor` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="df95d-388">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="df95d-389">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="df95d-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="df95d-390">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="df95d-390">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="df95d-391">Poprzedni szkielety mechanizmów polecenia `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="df95d-391">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="df95d-392">Uruchom aplikację i przejdź do strony instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="df95d-392">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="df95d-393">Zamień *strony/instruktorów/index. cshtml. cs* na następujący kod:</span><span class="sxs-lookup"><span data-stu-id="df95d-393">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="df95d-394">`OnGetAsync` Metoda akceptuje opcjonalne dane trasy dla identyfikatora wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-394">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="df95d-395">Zbadaj zapytanie w pliku */instruktors/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="df95d-395">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="df95d-396">Zapytanie zawiera dwa:</span><span class="sxs-lookup"><span data-stu-id="df95d-396">The query has two includes:</span></span>

* <span data-ttu-id="df95d-397">`OfficeAssignment`: Wyświetlane w [widoku instruktorów](#IP).</span><span class="sxs-lookup"><span data-stu-id="df95d-397">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="df95d-398">`CourseAssignments`: Które przynosi kursy.</span><span class="sxs-lookup"><span data-stu-id="df95d-398">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="df95d-399">Aktualizowanie strony indeksu instruktorów</span><span class="sxs-lookup"><span data-stu-id="df95d-399">Update the instructors Index page</span></span>

<span data-ttu-id="df95d-400">Aktualizowanie *stron/instruktorów/index. cshtml* przy użyciu następującego znacznika:</span><span class="sxs-lookup"><span data-stu-id="df95d-400">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="df95d-401">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="df95d-401">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="df95d-402">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="df95d-402">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="df95d-403">`"{id:int?}"`jest szablonem trasy.</span><span class="sxs-lookup"><span data-stu-id="df95d-403">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="df95d-404">Szablon trasy zmienia ciągi zapytań liczb całkowitych w adresie URL, aby przesyłać dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-404">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="df95d-405">Na przykład kliknięcie linku **Wybierz** dla instruktora z tylko `@page` dyrektywą spowoduje utworzenie adresu URL w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="df95d-405">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="df95d-406">Gdy dyrektywa Page ma `@page "{id:int?}"`wartość, poprzedni adres URL to:</span><span class="sxs-lookup"><span data-stu-id="df95d-406">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="df95d-407">Tytuł strony to **Instruktorzy**.</span><span class="sxs-lookup"><span data-stu-id="df95d-407">Page title is **Instructors**.</span></span>
* <span data-ttu-id="df95d-408">Dodano kolumnę **pakietu Office** , która `item.OfficeAssignment.Location` jest wyświetlana `item.OfficeAssignment` tylko wtedy, gdy nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="df95d-408">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="df95d-409">Ponieważ jest to relacja "jeden do zera" lub "jeden", nie może być powiązana jednostka OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="df95d-409">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="df95d-410">Dodano kolumnę **kursów** , która wyświetla nauczanie kursów przez każdego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-410">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="df95d-411">Aby uzyskać więcej informacji na temat składni Razor, zobacz [ `@:` jawne przejście liniowe](xref:mvc/views/razor#explicit-line-transition-with-) .</span><span class="sxs-lookup"><span data-stu-id="df95d-411">See [Explicit line transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="df95d-412">Dodano kod, który dynamicznie `class="success"` dodaje `tr` do elementu wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-412">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="df95d-413">Ustawia kolor tła dla wybranego wiersza przy użyciu klasy Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="df95d-413">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="df95d-414">Dodano nowe hiperłącze z etykietą **SELECT**.</span><span class="sxs-lookup"><span data-stu-id="df95d-414">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="df95d-415">Ten link powoduje wysłanie wybranego identyfikatora instruktora do `Index` metody i ustawienie koloru tła.</span><span class="sxs-lookup"><span data-stu-id="df95d-415">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="df95d-416">Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie zostanie wyświetlona `Location` strona (Biuro) z jednostki powiązanej. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="df95d-416">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="df95d-417">Jeśli OfficeAssignment ' ma wartość null, zostanie wyświetlona pusta komórka tabeli.</span><span class="sxs-lookup"><span data-stu-id="df95d-417">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="df95d-418">Kliknij link **Wybierz** .</span><span class="sxs-lookup"><span data-stu-id="df95d-418">Click on the **Select** link.</span></span> <span data-ttu-id="df95d-419">Styl wiersza zmienia się.</span><span class="sxs-lookup"><span data-stu-id="df95d-419">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="df95d-420">Dodaj nauczanie kursów według wybranych instruktorów</span><span class="sxs-lookup"><span data-stu-id="df95d-420">Add courses taught by selected instructor</span></span>

<span data-ttu-id="df95d-421">Zaktualizuj metodę na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu: `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="df95d-421">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="df95d-422">Add `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="df95d-422">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="df95d-423">Zbadaj zaktualizowane zapytanie:</span><span class="sxs-lookup"><span data-stu-id="df95d-423">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="df95d-424">Poprzednie zapytanie dodaje `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="df95d-424">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="df95d-425">Poniższy kod jest wykonywany po wybraniu instruktora (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="df95d-425">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="df95d-426">Wybrany instruktor jest pobierany z listy instruktorów w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="df95d-426">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="df95d-427">`Courses` Właściwość widoku modelu jest ładowana `Course` z jednostkami `CourseAssignments` z tej właściwości nawigacji instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-427">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="df95d-428">`Where` Metoda zwraca kolekcję.</span><span class="sxs-lookup"><span data-stu-id="df95d-428">The `Where` method returns a collection.</span></span> <span data-ttu-id="df95d-429">W poprzedniej `Where` metodzie zwracana jest tylko pojedyncza `Instructor` jednostka.</span><span class="sxs-lookup"><span data-stu-id="df95d-429">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="df95d-430">Metoda konwertuje kolekcję na jedną `Instructor` jednostkę. `Single`</span><span class="sxs-lookup"><span data-stu-id="df95d-430">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="df95d-431">Jednostka zapewnia dostęp `CourseAssignments` do właściwości. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="df95d-431">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="df95d-432">`CourseAssignments`zapewnia dostęp do powiązanych `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="df95d-432">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="df95d-434">`Single` Metoda jest używana w kolekcji, gdy kolekcja zawiera tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="df95d-434">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="df95d-435">`Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="df95d-435">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="df95d-436">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (null w tym przypadku), jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="df95d-436">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="df95d-437">Używanie `SingleOrDefault` w pustej kolekcji:</span><span class="sxs-lookup"><span data-stu-id="df95d-437">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="df95d-438">Wynikiem jest wyjątek (od próby znalezienia `Courses` właściwości w odwołaniu o wartości null).</span><span class="sxs-lookup"><span data-stu-id="df95d-438">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="df95d-439">Komunikat o wyjątku będzie mniej jasno wskazywał przyczynę problemu.</span><span class="sxs-lookup"><span data-stu-id="df95d-439">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="df95d-440">Poniższy kod wypełnia `Enrollments` właściwość modelu widoku w przypadku wybrania kursu:</span><span class="sxs-lookup"><span data-stu-id="df95d-440">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="df95d-441">Dodaj następujący znacznik na końcu strony */instruktorów/index. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="df95d-441">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="df95d-442">Powyższy znacznik wyświetla listę kursów związanych z instruktorem w przypadku wybrania instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-442">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="df95d-443">Przetestuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="df95d-443">Test the app.</span></span> <span data-ttu-id="df95d-444">Kliknij link **Wybierz** na stronie instruktorów.</span><span class="sxs-lookup"><span data-stu-id="df95d-444">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="df95d-445">Pokaż dane ucznia</span><span class="sxs-lookup"><span data-stu-id="df95d-445">Show student data</span></span>

<span data-ttu-id="df95d-446">W tej sekcji aplikacja zostanie zaktualizowana, aby wyświetlić dane uczniów dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="df95d-446">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="df95d-447">Zaktualizuj zapytanie w `OnGetAsync` metodzie na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="df95d-447">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="df95d-448">Aktualizowanie *stron/instruktorów/index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="df95d-448">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="df95d-449">Dodaj następujący znacznik na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="df95d-449">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="df95d-450">Powyższy znacznik wyświetla listę studentów, którzy są zarejestrowani w wybranym kursie.</span><span class="sxs-lookup"><span data-stu-id="df95d-450">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="df95d-451">Odśwież stronę i wybierz instruktora.</span><span class="sxs-lookup"><span data-stu-id="df95d-451">Refresh the page and select an instructor.</span></span> <span data-ttu-id="df95d-452">Wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.</span><span class="sxs-lookup"><span data-stu-id="df95d-452">Select a course to see the list of enrolled students and their grades.</span></span>

![Wybrany instruktor i kurs dla instruktorów](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="df95d-454">Korzystanie z jednego</span><span class="sxs-lookup"><span data-stu-id="df95d-454">Using Single</span></span>

<span data-ttu-id="df95d-455">Metoda może `Where` przekazać`Where` warunek zamiast wywołania metody osobno: `Single`</span><span class="sxs-lookup"><span data-stu-id="df95d-455">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="df95d-456">Poprzednie `Single` podejście nie zapewnia żadnych korzyści w porównaniu `Where`z korzystaniem z programu.</span><span class="sxs-lookup"><span data-stu-id="df95d-456">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="df95d-457">Niektórzy deweloperzy preferują `Single` styl podejścia.</span><span class="sxs-lookup"><span data-stu-id="df95d-457">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="df95d-458">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="df95d-458">Explicit loading</span></span>

<span data-ttu-id="df95d-459">Bieżący kod określa eager ładowania dla `Enrollments` i: `Students`</span><span class="sxs-lookup"><span data-stu-id="df95d-459">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="df95d-460">Załóżmy, że użytkownicy rzadko chcą widzieć rejestracje w kursie.</span><span class="sxs-lookup"><span data-stu-id="df95d-460">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="df95d-461">W takim przypadku Optymalizacja będzie ładować dane rejestracji tylko wtedy, gdy jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="df95d-461">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="df95d-462">W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawnego `Enrollments` ładowania i `Students`.</span><span class="sxs-lookup"><span data-stu-id="df95d-462">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="df95d-463">`OnGetAsync` Zaktualizuj program przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="df95d-463">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="df95d-464">Poprzedni kod odrzuca metody *ThenInclude* w celu uzyskania danych dotyczących rejestracji i uczniów.</span><span class="sxs-lookup"><span data-stu-id="df95d-464">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="df95d-465">W przypadku wybrania kursu wyróżniony kod pobiera:</span><span class="sxs-lookup"><span data-stu-id="df95d-465">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="df95d-466">`Enrollment` Jednostki wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="df95d-466">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="df95d-467">Jednostki `Student` dla każdej z `Enrollment`nich.</span><span class="sxs-lookup"><span data-stu-id="df95d-467">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="df95d-468">Zwróć uwagę na powyższe Komentarze `.AsNoTracking()`w kodzie.</span><span class="sxs-lookup"><span data-stu-id="df95d-468">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="df95d-469">Właściwości nawigacji można jawnie załadować tylko dla śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="df95d-469">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="df95d-470">Przetestuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="df95d-470">Test the app.</span></span> <span data-ttu-id="df95d-471">Z perspektywy użytkowników aplikacja zachowuje się identycznie z poprzednią wersją.</span><span class="sxs-lookup"><span data-stu-id="df95d-471">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="df95d-472">W następnym samouczku pokazano, jak zaktualizować powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="df95d-472">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df95d-473">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="df95d-473">Additional resources</span></span>

* [<span data-ttu-id="df95d-474">Wersja tego samouczka usługi YouTube (part1)</span><span class="sxs-lookup"><span data-stu-id="df95d-474">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="df95d-475">Wersja tego samouczka usługi YouTube (part2)</span><span class="sxs-lookup"><span data-stu-id="df95d-475">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="df95d-476">[Poprzedni](xref:data/ef-rp/complex-data-model)Następny
>[](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="df95d-476">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end
