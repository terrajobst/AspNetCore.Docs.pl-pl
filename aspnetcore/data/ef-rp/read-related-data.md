---
title: Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8
author: rick-anderson
description: W tym samouczku odczytasz i wyświetlasz powiązane dane, czyli dane, które Entity Framework ładowane do właściwości nawigacji.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 93fbb4741f476368d75d23162d6e2597de7b263e
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819916"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="bc713-103">Razor Pages z EF Core w ASP.NET Core odczytu danych powiązanych — 6 z 8</span><span class="sxs-lookup"><span data-stu-id="bc713-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="bc713-104">Autorzy [Dykstra](https://github.com/tdykstra), [Jan P Kowalski](https://twitter.com/thereformedprog)i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc713-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="bc713-105">W tym samouczku dane pokrewne są odczytywane i wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="bc713-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="bc713-106">Powiązane dane to dane, które EF Core ładowane do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="bc713-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="bc713-107">Jeśli napotkasz problemy, nie można rozwiązać, [pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="bc713-107">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="bc713-108">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="bc713-108">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="bc713-109">Na poniższych ilustracjach przedstawiono ukończone strony dla tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="bc713-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="bc713-112">Eager, jawne i opóźnione ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="bc713-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="bc713-113">Istnieje kilka sposobów, EF Core mogą ładować powiązane dane do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="bc713-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="bc713-114">[Ładowanie eager](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="bc713-114">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="bc713-115">Ładowanie eager jest, gdy zapytanie dla jednego typu jednostki również ładuje powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="bc713-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="bc713-116">Po odczytaniu jednostki są pobierane powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="bc713-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="bc713-117">Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne.</span><span class="sxs-lookup"><span data-stu-id="bc713-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="bc713-118">EF Core będzie wystawiał wiele zapytań dla niektórych typów ładowania eager.</span><span class="sxs-lookup"><span data-stu-id="bc713-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="bc713-119">Wygenerowanie wielu zapytań może być bardziej wydajne niż w przypadku niektórych zapytań w EF6, w których wystąpiło pojedyncze zapytanie.</span><span class="sxs-lookup"><span data-stu-id="bc713-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="bc713-120">Ładowanie eager jest określone przy użyciu `Include` metod `ThenInclude` i.</span><span class="sxs-lookup"><span data-stu-id="bc713-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Przykład ładowania eager](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="bc713-122">Podczas ładowania eager są wysyłane wiele zapytań, gdy zostanie dołączona Nawigacja kolekcji:</span><span class="sxs-lookup"><span data-stu-id="bc713-122">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="bc713-123">Jedno zapytanie dotyczące głównej kwerendy</span><span class="sxs-lookup"><span data-stu-id="bc713-123">One query for the main query</span></span> 
  * <span data-ttu-id="bc713-124">Jedno zapytanie dla każdej kolekcji "Edge" w drzewie ładowania.</span><span class="sxs-lookup"><span data-stu-id="bc713-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="bc713-125">Oddziel zapytania `Load`z: Dane można pobrać w oddzielnych zapytaniach, a EF Core "naprawia" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="bc713-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="bc713-126">"rozwiązuje" oznacza, że EF Core automatycznie wypełnia właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="bc713-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="bc713-127">Osobne zapytania `Load` i są bardziej podobne do EXPLICT ładowania niż ładowanie eager.</span><span class="sxs-lookup"><span data-stu-id="bc713-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Przykład oddzielnych zapytań](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="bc713-129">Uwaga: EF Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="bc713-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="bc713-130">Nawet jeśli dane dla właściwości nawigacji *nie* są jawnie uwzględniane, właściwość można nadal wypełnić, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="bc713-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="bc713-131">[Jawne ładowanie](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="bc713-131">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="bc713-132">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="bc713-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="bc713-133">Kod musi być zapisany, aby można było pobrać powiązane dane, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="bc713-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="bc713-134">Jawne ładowanie z oddzielnymi zapytaniami powoduje wysłanie wielu zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bc713-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="bc713-135">W przypadku jawnego ładowania kod określa właściwości nawigacji do załadowania.</span><span class="sxs-lookup"><span data-stu-id="bc713-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="bc713-136">Użyj metody `Load` , aby przeprowadzić jawne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="bc713-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="bc713-137">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bc713-137">For example:</span></span>

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="bc713-139">[Ładowanie](/ef/core/querying/related-data#lazy-loading)z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="bc713-139">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="bc713-140">[Ładowanie z opóźnieniem zostało dodane do EF Core w wersji 2,1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="bc713-140">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="bc713-141">Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="bc713-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="bc713-142">Podczas pierwszego uzyskiwania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="bc713-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="bc713-143">Zapytanie jest wysyłane do bazy danych przy każdym dostępie do właściwości nawigacji po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="bc713-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="bc713-144">`Select` Operator ładuje tylko powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="bc713-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="bc713-145">Tworzenie strony kursu, która wyświetla nazwę działu</span><span class="sxs-lookup"><span data-stu-id="bc713-145">Create a Course page that displays department name</span></span>

<span data-ttu-id="bc713-146">Jednostka kursu zawiera właściwość nawigacji, która zawiera `Department` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="bc713-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="bc713-147">`Department` Jednostka zawiera dział, do którego jest przypisany kurs.</span><span class="sxs-lookup"><span data-stu-id="bc713-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="bc713-148">Aby wyświetlić nazwę przypisanego działu na liście kursów:</span><span class="sxs-lookup"><span data-stu-id="bc713-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="bc713-149">`Name` Pobierz Właściwość`Department` z jednostki.</span><span class="sxs-lookup"><span data-stu-id="bc713-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="bc713-150">`Department` Jednostka pochodzi `Course.Department` z właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="bc713-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Kurs. Dział](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="bc713-152">Tworzenie szkieletu modelu kursu</span><span class="sxs-lookup"><span data-stu-id="bc713-152">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc713-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc713-153">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="bc713-154">Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Course` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="bc713-154">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bc713-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bc713-155">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="bc713-156">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bc713-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="bc713-157">Poprzedni szkielety mechanizmów polecenia `Course` modelu.</span><span class="sxs-lookup"><span data-stu-id="bc713-157">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="bc713-158">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc713-158">Open the project in Visual Studio.</span></span>

<span data-ttu-id="bc713-159">Otwórz *stronę/kursy/index. cshtml. cs* i Przeanalizuj `OnGetAsync` metodę.</span><span class="sxs-lookup"><span data-stu-id="bc713-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="bc713-160">Aparat tworzenia szkieletów określił eager ładowania dla `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="bc713-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="bc713-161">`Include` Metoda określa eager ładowania.</span><span class="sxs-lookup"><span data-stu-id="bc713-161">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="bc713-162">Uruchom aplikację i wybierz łącze **kursy** .</span><span class="sxs-lookup"><span data-stu-id="bc713-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="bc713-163">W kolumnie dział zostanie wyświetlona wartość `DepartmentID`, która nie jest przydatna.</span><span class="sxs-lookup"><span data-stu-id="bc713-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="bc713-164">`OnGetAsync` Zaktualizuj metodę przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="bc713-164">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="bc713-165">Poprzedni kod dodaje `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="bc713-165">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="bc713-166">`AsNoTracking`zwiększa wydajność, ponieważ zwrócone jednostki nie są śledzone.</span><span class="sxs-lookup"><span data-stu-id="bc713-166">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="bc713-167">Jednostki nie są śledzone, ponieważ nie są aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="bc713-167">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="bc713-168">Aktualizuj *strony/kursy/index. cshtml* z następującymi wyróżnionymi znacznikami:</span><span class="sxs-lookup"><span data-stu-id="bc713-168">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="bc713-169">W kodzie szkieletowym wprowadzono następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="bc713-169">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="bc713-170">Zmieniono nagłówek z indeksu na kursy.</span><span class="sxs-lookup"><span data-stu-id="bc713-170">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="bc713-171">Dodano kolumnę **liczbową** , która wyświetla `CourseID` wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="bc713-171">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="bc713-172">Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są oznaczane przez użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="bc713-172">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="bc713-173">Jednak w tym przypadku klucz podstawowy ma znaczenie.</span><span class="sxs-lookup"><span data-stu-id="bc713-173">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="bc713-174">Zmieniono kolumnę **działu** , aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="bc713-174">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="bc713-175">Kod wyświetla `Name` Właściwość `Department` jednostki `Department` , która jest ładowana do właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="bc713-175">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="bc713-176">Uruchom aplikację i wybierz kartę **kursy** , aby wyświetlić listę z nazwami działów.</span><span class="sxs-lookup"><span data-stu-id="bc713-176">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Strona indeksu kursów](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="bc713-178">Ładowanie powiązanych danych przy użyciu opcji Select</span><span class="sxs-lookup"><span data-stu-id="bc713-178">Loading related data with Select</span></span>

<span data-ttu-id="bc713-179">Metoda ładuje powiązane dane `Include` przy użyciu metody: `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="bc713-179">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="bc713-180">`Select` Operator ładuje tylko powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="bc713-180">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="bc713-181">Dla pojedynczych elementów, podobnie jak `Department.Name` w przypadku użycia sprzężenia wewnętrznego SQL.</span><span class="sxs-lookup"><span data-stu-id="bc713-181">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="bc713-182">W przypadku kolekcji program używa innego dostępu do bazy danych, ale w `Include` związku z tym wykonuje operator dla kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bc713-182">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="bc713-183">Poniższy kod ładuje powiązane dane przy użyciu `Select` metody:</span><span class="sxs-lookup"><span data-stu-id="bc713-183">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="bc713-184">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="bc713-184">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="bc713-185">Zobacz [IndexSelect. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) i [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) , aby zapoznać się z kompletnym przykładem.</span><span class="sxs-lookup"><span data-stu-id="bc713-185">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="bc713-186">Utwórz stronę instruktorów pokazującą kursy i rejestracje</span><span class="sxs-lookup"><span data-stu-id="bc713-186">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="bc713-187">W tej sekcji zostanie utworzona strona instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="bc713-187">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="bc713-188">![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="bc713-188">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="bc713-189">Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bc713-189">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="bc713-190">Lista instruktorów wyświetla powiązane dane z `OfficeAssignment` jednostki (Office na powyższym obrazie).</span><span class="sxs-lookup"><span data-stu-id="bc713-190">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="bc713-191">Jednostki `Instructor` i`OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego.</span><span class="sxs-lookup"><span data-stu-id="bc713-191">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="bc713-192">Eager ładowania jest używany dla `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="bc713-192">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="bc713-193">Ładowanie eager jest zwykle wydajniejsze, gdy wymagane jest wyświetlenie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="bc713-193">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="bc713-194">W takim przypadku wyświetlane są przypisania pakietu Office dla instruktorów.</span><span class="sxs-lookup"><span data-stu-id="bc713-194">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="bc713-195">Gdy użytkownik wybierze instruktora (Harui na poprzednim obrazie), zostaną wyświetlone `Course` powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="bc713-195">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="bc713-196">Jednostki `Instructor` i`Course` znajdują się w relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="bc713-196">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="bc713-197">Ładowanie eager jest używane dla `Course` jednostek i ich powiązanych `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="bc713-197">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="bc713-198">W takim przypadku oddzielne zapytania mogą być bardziej wydajne, ponieważ potrzebują tylko kursów dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="bc713-198">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="bc713-199">Ten przykład pokazuje, jak używać eager ładowania dla właściwości nawigacji w jednostkach, które są we właściwościach nawigacji.</span><span class="sxs-lookup"><span data-stu-id="bc713-199">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="bc713-200">Gdy użytkownik wybierze kurs (chemia na powyższym obrazie), zostaną wyświetlone `Enrollments` powiązane dane z jednostki.</span><span class="sxs-lookup"><span data-stu-id="bc713-200">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="bc713-201">Na powyższym obrazie wyświetlana jest nazwa ucznia i jej klasy.</span><span class="sxs-lookup"><span data-stu-id="bc713-201">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="bc713-202">Jednostki `Course` i`Enrollment` znajdują się w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="bc713-202">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="bc713-203">Utwórz model widoku dla widoku indeksu instruktora</span><span class="sxs-lookup"><span data-stu-id="bc713-203">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="bc713-204">Na stronie instruktorzy są wyświetlane dane z trzech różnych tabel.</span><span class="sxs-lookup"><span data-stu-id="bc713-204">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="bc713-205">Tworzony jest model widoku, który zawiera trzy jednostki reprezentujące trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="bc713-205">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="bc713-206">W folderze *SchoolViewModels* Utwórz *InstructorIndexData.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="bc713-206">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="bc713-207">Tworzenie szkieletu modelu instruktora</span><span class="sxs-lookup"><span data-stu-id="bc713-207">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc713-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc713-208">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="bc713-209">Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Instructor` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="bc713-209">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bc713-210">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bc713-210">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="bc713-211">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bc713-211">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="bc713-212">Poprzedni szkielety mechanizmów polecenia `Instructor` modelu.</span><span class="sxs-lookup"><span data-stu-id="bc713-212">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="bc713-213">Uruchom aplikację i przejdź do strony instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="bc713-213">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="bc713-214">Zamień *strony/instruktorów/index. cshtml. cs* na następujący kod:</span><span class="sxs-lookup"><span data-stu-id="bc713-214">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="bc713-215">`OnGetAsync` Metoda akceptuje opcjonalne dane trasy dla identyfikatora wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="bc713-215">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="bc713-216">Zbadaj zapytanie w pliku */instruktors/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="bc713-216">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="bc713-217">Zapytanie zawiera dwa:</span><span class="sxs-lookup"><span data-stu-id="bc713-217">The query has two includes:</span></span>

* <span data-ttu-id="bc713-218">`OfficeAssignment`: Wyświetlane w [widoku instruktorów](#IP).</span><span class="sxs-lookup"><span data-stu-id="bc713-218">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="bc713-219">`CourseAssignments`: Które przynosi kursy.</span><span class="sxs-lookup"><span data-stu-id="bc713-219">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="bc713-220">Aktualizowanie strony indeksu instruktorów</span><span class="sxs-lookup"><span data-stu-id="bc713-220">Update the instructors Index page</span></span>

<span data-ttu-id="bc713-221">Aktualizowanie *stron/instruktorów/index. cshtml* przy użyciu następującego znacznika:</span><span class="sxs-lookup"><span data-stu-id="bc713-221">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="bc713-222">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="bc713-222">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="bc713-223">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="bc713-223">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="bc713-224">`"{id:int?}"`jest szablonem trasy.</span><span class="sxs-lookup"><span data-stu-id="bc713-224">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="bc713-225">Szablon trasy zmienia ciągi zapytań liczb całkowitych w adresie URL, aby przesyłać dane.</span><span class="sxs-lookup"><span data-stu-id="bc713-225">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="bc713-226">Na przykład kliknięcie linku **Wybierz** dla instruktora z tylko `@page` dyrektywą spowoduje utworzenie adresu URL w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bc713-226">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="bc713-227">Gdy dyrektywa Page ma `@page "{id:int?}"`wartość, poprzedni adres URL to:</span><span class="sxs-lookup"><span data-stu-id="bc713-227">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="bc713-228">Tytuł strony to **Instruktorzy**.</span><span class="sxs-lookup"><span data-stu-id="bc713-228">Page title is **Instructors**.</span></span>
* <span data-ttu-id="bc713-229">Dodano kolumnę **pakietu Office** , która `item.OfficeAssignment.Location` jest wyświetlana `item.OfficeAssignment` tylko wtedy, gdy nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="bc713-229">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="bc713-230">Ponieważ jest to relacja "jeden do zera" lub "jeden", nie może być powiązana jednostka OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="bc713-230">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="bc713-231">Dodano kolumnę **kursów** , która wyświetla nauczanie kursów przez każdego instruktora.</span><span class="sxs-lookup"><span data-stu-id="bc713-231">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="bc713-232">Aby uzyskać więcej informacji na temat składni Razor, zobacz [ `@:` jawne przejście liniowe](xref:mvc/views/razor#explicit-line-transition-with-) .</span><span class="sxs-lookup"><span data-stu-id="bc713-232">See [Explicit line transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="bc713-233">Dodano kod, który dynamicznie `class="success"` dodaje `tr` do elementu wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="bc713-233">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="bc713-234">Ustawia kolor tła dla wybranego wiersza przy użyciu klasy Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="bc713-234">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="bc713-235">Dodano nowe hiperłącze z etykietą **SELECT**.</span><span class="sxs-lookup"><span data-stu-id="bc713-235">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="bc713-236">Ten link powoduje wysłanie wybranego identyfikatora instruktora do `Index` metody i ustawienie koloru tła.</span><span class="sxs-lookup"><span data-stu-id="bc713-236">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="bc713-237">Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie zostanie wyświetlona `Location` strona (Biuro) z jednostki powiązanej. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="bc713-237">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="bc713-238">Jeśli OfficeAssignment ' ma wartość null, zostanie wyświetlona pusta komórka tabeli.</span><span class="sxs-lookup"><span data-stu-id="bc713-238">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Nie wybrano niczego ze strony indeksu instruktorów](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="bc713-240">Kliknij link **Wybierz** .</span><span class="sxs-lookup"><span data-stu-id="bc713-240">Click on the **Select** link.</span></span> <span data-ttu-id="bc713-241">Styl wiersza zmienia się.</span><span class="sxs-lookup"><span data-stu-id="bc713-241">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="bc713-242">Dodaj nauczanie kursów według wybranych instruktorów</span><span class="sxs-lookup"><span data-stu-id="bc713-242">Add courses taught by selected instructor</span></span>

<span data-ttu-id="bc713-243">Zaktualizuj metodę na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu: `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="bc713-243">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="bc713-244">Add `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="bc713-244">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="bc713-245">Zbadaj zaktualizowane zapytanie:</span><span class="sxs-lookup"><span data-stu-id="bc713-245">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="bc713-246">Poprzednie zapytanie dodaje `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="bc713-246">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="bc713-247">Poniższy kod jest wykonywany po wybraniu instruktora (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="bc713-247">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="bc713-248">Wybrany instruktor jest pobierany z listy instruktorów w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="bc713-248">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="bc713-249">`Courses` Właściwość widoku modelu jest ładowana `Course` z jednostkami `CourseAssignments` z tej właściwości nawigacji instruktora.</span><span class="sxs-lookup"><span data-stu-id="bc713-249">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="bc713-250">`Where` Metoda zwraca kolekcję.</span><span class="sxs-lookup"><span data-stu-id="bc713-250">The `Where` method returns a collection.</span></span> <span data-ttu-id="bc713-251">W poprzedniej `Where` metodzie zwracana jest tylko pojedyncza `Instructor` jednostka.</span><span class="sxs-lookup"><span data-stu-id="bc713-251">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="bc713-252">Metoda konwertuje kolekcję na jedną `Instructor` jednostkę. `Single`</span><span class="sxs-lookup"><span data-stu-id="bc713-252">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="bc713-253">Jednostka zapewnia dostęp `CourseAssignments` do właściwości. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="bc713-253">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="bc713-254">`CourseAssignments`zapewnia dostęp do powiązanych `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="bc713-254">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="bc713-256">`Single` Metoda jest używana w kolekcji, gdy kolekcja zawiera tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="bc713-256">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="bc713-257">`Single` Metoda zgłasza wyjątek, jeśli kolekcja jest pusta lub jeśli istnieje więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="bc713-257">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="bc713-258">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (null w tym przypadku), jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="bc713-258">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="bc713-259">Używanie `SingleOrDefault` w pustej kolekcji:</span><span class="sxs-lookup"><span data-stu-id="bc713-259">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="bc713-260">Wynikiem jest wyjątek (od próby znalezienia `Courses` właściwości w odwołaniu o wartości null).</span><span class="sxs-lookup"><span data-stu-id="bc713-260">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="bc713-261">Komunikat o wyjątku będzie mniej jasno wskazywał przyczynę problemu.</span><span class="sxs-lookup"><span data-stu-id="bc713-261">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="bc713-262">Poniższy kod wypełnia `Enrollments` właściwość modelu widoku w przypadku wybrania kursu:</span><span class="sxs-lookup"><span data-stu-id="bc713-262">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="bc713-263">Dodaj następujący znacznik na końcu strony */instruktorów/index. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="bc713-263">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="bc713-264">Powyższy znacznik wyświetla listę kursów związanych z instruktorem w przypadku wybrania instruktora.</span><span class="sxs-lookup"><span data-stu-id="bc713-264">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="bc713-265">Przetestuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="bc713-265">Test the app.</span></span> <span data-ttu-id="bc713-266">Kliknij link **Wybierz** na stronie instruktorów.</span><span class="sxs-lookup"><span data-stu-id="bc713-266">Click on a **Select** link on the instructors page.</span></span>

![Wybrany instruktor strony indeksu instruktora](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="bc713-268">Pokaż dane ucznia</span><span class="sxs-lookup"><span data-stu-id="bc713-268">Show student data</span></span>

<span data-ttu-id="bc713-269">W tej sekcji aplikacja zostanie zaktualizowana, aby wyświetlić dane uczniów dla wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="bc713-269">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="bc713-270">Zaktualizuj zapytanie w `OnGetAsync` metodzie na *stronach/instruktorów/index. cshtml. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="bc713-270">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="bc713-271">Aktualizowanie *stron/instruktorów/index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bc713-271">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="bc713-272">Dodaj następujący znacznik na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="bc713-272">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="bc713-273">Powyższy znacznik wyświetla listę studentów, którzy są zarejestrowani w wybranym kursie.</span><span class="sxs-lookup"><span data-stu-id="bc713-273">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="bc713-274">Odśwież stronę i wybierz instruktora.</span><span class="sxs-lookup"><span data-stu-id="bc713-274">Refresh the page and select an instructor.</span></span> <span data-ttu-id="bc713-275">Wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.</span><span class="sxs-lookup"><span data-stu-id="bc713-275">Select a course to see the list of enrolled students and their grades.</span></span>

![Wybrany instruktor i kurs dla instruktorów](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="bc713-277">Korzystanie z jednego</span><span class="sxs-lookup"><span data-stu-id="bc713-277">Using Single</span></span>

<span data-ttu-id="bc713-278">Metoda może `Where` przekazać`Where` warunek zamiast wywołania metody osobno: `Single`</span><span class="sxs-lookup"><span data-stu-id="bc713-278">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="bc713-279">Poprzednie `Single` podejście nie zapewnia żadnych korzyści w porównaniu `Where`z korzystaniem z programu.</span><span class="sxs-lookup"><span data-stu-id="bc713-279">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="bc713-280">Niektórzy deweloperzy preferują `Single` styl podejścia.</span><span class="sxs-lookup"><span data-stu-id="bc713-280">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="bc713-281">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="bc713-281">Explicit loading</span></span>

<span data-ttu-id="bc713-282">Bieżący kod określa eager ładowania dla `Enrollments` i: `Students`</span><span class="sxs-lookup"><span data-stu-id="bc713-282">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="bc713-283">Załóżmy, że użytkownicy rzadko chcą widzieć rejestracje w kursie.</span><span class="sxs-lookup"><span data-stu-id="bc713-283">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="bc713-284">W takim przypadku Optymalizacja będzie ładować dane rejestracji tylko wtedy, gdy jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="bc713-284">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="bc713-285">W tej sekcji `OnGetAsync` zostanie zaktualizowany w celu użycia jawnego `Enrollments` ładowania i `Students`.</span><span class="sxs-lookup"><span data-stu-id="bc713-285">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="bc713-286">`OnGetAsync` Zaktualizuj program przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="bc713-286">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="bc713-287">Poprzedni kod odrzuca metody *ThenInclude* w celu uzyskania danych dotyczących rejestracji i uczniów.</span><span class="sxs-lookup"><span data-stu-id="bc713-287">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="bc713-288">W przypadku wybrania kursu wyróżniony kod pobiera:</span><span class="sxs-lookup"><span data-stu-id="bc713-288">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="bc713-289">`Enrollment` Jednostki wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="bc713-289">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="bc713-290">Jednostki `Student` dla każdej z `Enrollment`nich.</span><span class="sxs-lookup"><span data-stu-id="bc713-290">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="bc713-291">Zwróć uwagę na powyższe Komentarze `.AsNoTracking()`w kodzie.</span><span class="sxs-lookup"><span data-stu-id="bc713-291">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="bc713-292">Właściwości nawigacji można jawnie załadować tylko dla śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="bc713-292">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="bc713-293">Przetestuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="bc713-293">Test the app.</span></span> <span data-ttu-id="bc713-294">Z perspektywy użytkowników aplikacja zachowuje się identycznie z poprzednią wersją.</span><span class="sxs-lookup"><span data-stu-id="bc713-294">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="bc713-295">W następnym samouczku pokazano, jak zaktualizować powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="bc713-295">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc713-296">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bc713-296">Additional resources</span></span>

* [<span data-ttu-id="bc713-297">Wersja tego samouczka usługi YouTube (part1)</span><span class="sxs-lookup"><span data-stu-id="bc713-297">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="bc713-298">Wersja tego samouczka usługi YouTube (part2)</span><span class="sxs-lookup"><span data-stu-id="bc713-298">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="bc713-299">[Poprzedni](xref:data/ef-rp/complex-data-model)Następny
>[](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="bc713-299">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
