---
title: Stron razor podstawowych EF - CRUD - 2 8
author: rick-anderson
description: "Przedstawia sposób tworzenia, odczytu, aktualizowanie i usuwanie EF podstawowych"
keywords: Platformy ASP.NET Core, Entity Framework Core, CRUD, tworzenia, odczytu, aktualizowanie i usuwanie
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 410829608540697ac4563f1399c8e72d28a13cf2
ms.sourcegitcommit: 703593d5fd14076e79be2ba75a5b8da12a60ab15
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="845df-104">Tworzenia, odczytu, aktualizacji i usuwania - Core EF Razor strony (2 8)</span><span class="sxs-lookup"><span data-stu-id="845df-104">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="845df-105">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="845df-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="845df-106">W tym samouczku CRUD szkieletu (tworzenia, odczytu, aktualizowanie i usuwanie) kodu jest przejrzeć i dostosować.</span><span class="sxs-lookup"><span data-stu-id="845df-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="845df-107">Uwaga: Aby zminimalizować złożoność i zachować te samouczki koncentruje się na podstawowych EF, EF Core kod jest używany w plikach Razor strony związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="845df-107">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="845df-108">Niektórzy deweloperzy Użyj wzorca usługi warstwy lub repozytorium w, aby utworzyć warstwę abstrakcji między interfejsu użytkownika (Razor strony) i warstwa dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="845df-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="845df-109">W w tym samouczku, tworzenia, edycji, usuwania i szczegóły stron Razor w *uczniowie* folderu zostaną zmodyfikowane.</span><span class="sxs-lookup"><span data-stu-id="845df-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="845df-110">Kod z utworzonym szkieletem korzysta ze wzorca następujące tworzenie, edytowanie i usuwanie stron:</span><span class="sxs-lookup"><span data-stu-id="845df-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="845df-111">Pobierz i Wyświetl żądanych danych z metodą HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="845df-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="845df-112">Zapisz zmiany w danych z metodą HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="845df-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="845df-113">Strony indeksu i szczegóły Pobierz i Wyświetl żądanych danych z metodą HTTP GET`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="845df-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="845df-114">Zamień SingleOrDefaultAsync FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="845df-114">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="845df-115">Wygenerowany kod używa [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) można pobrać żądanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="845df-115">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="845df-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) jest bardziej wydajny w pobieranie jedną jednostkę:</span><span class="sxs-lookup"><span data-stu-id="845df-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="845df-117">Jeśli kod należy sprawdzić, czy nie ma więcej niż jednej jednostki zwróconych przez kwerendę.</span><span class="sxs-lookup"><span data-stu-id="845df-117">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="845df-118">`SingleOrDefaultAsync`pobiera dane i niepotrzebne działa.</span><span class="sxs-lookup"><span data-stu-id="845df-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="845df-119">`SingleOrDefaultAsync`zgłasza wyjątek, jeśli istnieje więcej niż jednej jednostki, która pasuje do części filtru.</span><span class="sxs-lookup"><span data-stu-id="845df-119">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="845df-120">`FirstOrDefaultAsync`nie zgłoszenia, jeśli istnieje więcej niż jednej jednostki, która pasuje do części filtru.</span><span class="sxs-lookup"><span data-stu-id="845df-120">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="845df-121">Zastąp globalnie `SingleOrDefaultAsync` z `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="845df-121">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="845df-122">`SingleOrDefaultAsync`jest używany w miejscach 5:</span><span class="sxs-lookup"><span data-stu-id="845df-122">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="845df-123">`OnGetAsync`na stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="845df-123">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="845df-124">`OnGetAsync`i `OnPostAsync` na edytowanie i usuwanie stron.</span><span class="sxs-lookup"><span data-stu-id="845df-124">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="845df-125">Metoda FindAsync</span><span class="sxs-lookup"><span data-stu-id="845df-125">FindAsync</span></span>

<span data-ttu-id="845df-126">W bardzo szkieletu kodu [metoda FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) można użyć zamiast `FirstOrDefaultAsync` lub `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="845df-126">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="845df-127">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="845df-127">`FindAsync`:</span></span>

* <span data-ttu-id="845df-128">Znajduje obiekt z (klucz podstawowy) klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="845df-128">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="845df-129">Jeśli jednostka entity z atrybutem na PK jest śledzony przez kontekst, jest zwracany bez żądania z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="845df-129">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="845df-130">To proste i zwięzłe.</span><span class="sxs-lookup"><span data-stu-id="845df-130">Is simple and concise.</span></span>
* <span data-ttu-id="845df-131">Jest zoptymalizowany do odszukania pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="845df-131">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="845df-132">Może korzystnie wydajności w niektórych sytuacjach, ale rzadko pochodzą do gry dla scenariuszy normalne sieci web.</span><span class="sxs-lookup"><span data-stu-id="845df-132">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="845df-133">Niejawnie wykorzystuje [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) zamiast [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="845df-133">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="845df-134">Ale jeśli chcesz dołączyć inne jednostki, a następnie znajdź nie jest już właściwe.</span><span class="sxs-lookup"><span data-stu-id="845df-134">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="845df-135">Oznacza to, że może być konieczne porzucenie Znajdź i przejdź do zapytania w miarę postępów Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="845df-135">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="845df-136">Dostosowywanie strony szczegółów</span><span class="sxs-lookup"><span data-stu-id="845df-136">Customize the Details page</span></span>

<span data-ttu-id="845df-137">Przejdź do `Pages/Students` strony.</span><span class="sxs-lookup"><span data-stu-id="845df-137">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="845df-138">**Edytuj**, **szczegóły**, i **usunąć** łącza są generowane przez [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/uczniów lub studentów / Index.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="845df-138">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="845df-139">Wybierz łącze Szczegóły.</span><span class="sxs-lookup"><span data-stu-id="845df-139">Select a Details link.</span></span> <span data-ttu-id="845df-140">Adres URL ma postać `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="845df-140">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="845df-141">Identyfikator uczniów jest przekazywany za pomocą ciągu zapytania (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="845df-141">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="845df-142">Zaktualizuj edycji, szczegóły i usuwanie stron Razor, aby użyć `"{id:int}"` szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="845df-142">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="845df-143">Zmiany w dyrektywie page dla każdego z tych stron z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="845df-143">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="845df-144">Żądanie do strony przy użyciu szablonu trasy "{identyfikator: int}", który wykonuje **nie** obejmują całkowitą trasy wartość zwraca HTTP (nie znaleziono) błąd 404.</span><span class="sxs-lookup"><span data-stu-id="845df-144">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="845df-145">Na przykład `http://localhost:5000/Students/Details` zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="845df-145">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="845df-146">Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="845df-146">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="845df-147">Uruchom aplikację, kliknij łącze Szczegóły i sprawdź adres URL jest przekazanie identyfikator jako dane trasy (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="845df-147">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="845df-148">Nie zmieniaj globalnie `@page` do `@page "{id:int}"`, sposób podziału tak łącza do strony głównej i tworzenie stron.</span><span class="sxs-lookup"><span data-stu-id="845df-148">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="845df-149">Dodawanie danych powiązanych</span><span class="sxs-lookup"><span data-stu-id="845df-149">Add related data</span></span>

<span data-ttu-id="845df-150">Nie ma szkieletu kodu strony indeksu studentów `Enrollments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="845df-150">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="845df-151">W tej sekcji zawartości `Enrollments` Kolekcja zostanie wyświetlona na stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="845df-151">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="845df-152">`OnGetAsync` Metody *Pages/Students/Details.cshtml.cs* używa `FirstOrDefaultAsync` metoda pobierania pojedynczy `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="845df-152">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="845df-153">Dodaj następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="845df-153">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="845df-154">`Include` i `ThenInclude` metody powodują kontekstu ładowania `Student.Enrollments` właściwość nawigacji i w obrębie każdej rejestracji `Enrollment.Course` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="845df-154">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="845df-155">Te metody są examinied szczegółowo w samouczku dane dotyczące odczytu.</span><span class="sxs-lookup"><span data-stu-id="845df-155">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="845df-156">`AsNoTracking` — Metoda zwiększa wydajność w scenariuszach, gdy zwracany jednostek nie są aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="845df-156">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="845df-157">`AsNoTracking`omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="845df-157">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="845df-158">Na stronie Szczegóły są wyświetlane powiązane rejestracji</span><span class="sxs-lookup"><span data-stu-id="845df-158">Display related enrollments on the Details page</span></span>

<span data-ttu-id="845df-159">Otwórz *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="845df-159">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="845df-160">Dodaj następujący wyróżniony kod, aby wyświetlić listę rejestracji:</span><span class="sxs-lookup"><span data-stu-id="845df-160">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=35-53)]

<span data-ttu-id="845df-161">Jeśli po wklejeniu kod wcięcia kodu o nieprawidłowej naciśnij kombinację klawiszy CTRL-K-D, aby naprawić błąd.</span><span class="sxs-lookup"><span data-stu-id="845df-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="845df-162">Poprzedni kod w pętli jednostek w `Enrollments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="845df-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="845df-163">Dla każdego rejestracji Wyświetla tytuł kursu i kategorii.</span><span class="sxs-lookup"><span data-stu-id="845df-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="845df-164">Tytuł kursu są pobierane z jednostki kursu, która jest przechowywana w `Course` właściwości nawigacji jednostki rejestracji.</span><span class="sxs-lookup"><span data-stu-id="845df-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="845df-165">Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **szczegóły** łącze studenta.</span><span class="sxs-lookup"><span data-stu-id="845df-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="845df-166">Zostanie wyświetlona lista kursów i klasy dla wybranego uczniów.</span><span class="sxs-lookup"><span data-stu-id="845df-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="845df-167">Utwórz stronę aktualizacji</span><span class="sxs-lookup"><span data-stu-id="845df-167">Update the Create page</span></span>

<span data-ttu-id="845df-168">Aktualizacja `OnPostAsync` metody w *Pages/Students/Create.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="845df-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="845df-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="845df-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="845df-170">Sprawdź [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kodu:</span><span class="sxs-lookup"><span data-stu-id="845df-170">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="845df-171">W powyższym kodzie `TryUpdateModelAsync<Student>` próbuje zaktualizować `emptyStudent` przy użyciu wartości przesłanego formularza z [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) właściwości w [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="845df-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="845df-172">`TryUpdateModelAsync`tylko aktualizuje właściwości wymienionych (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="845df-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="845df-173">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="845df-173">In the preceding sample:</span></span>

* <span data-ttu-id="845df-174">Drugi argument (` "student", // Prefix`) to prefiks używany do odszukania wartości.</span><span class="sxs-lookup"><span data-stu-id="845df-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="845df-175">Nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="845df-175">It's not case sensitive.</span></span>
* <span data-ttu-id="845df-176">Wartości przesłanego formularza są konwertowane na typy w `Student` modelu przy użyciu [modelu powiązania](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="845df-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="845df-177">Overposting</span><span class="sxs-lookup"><span data-stu-id="845df-177">Overposting</span></span>

<span data-ttu-id="845df-178">Przy użyciu `TryUpdateModel` można zaktualizować pola z wartościami oczekujących na opublikowanie jest ze względów bezpieczeństwa, ponieważ nie dopuszcza overposting.</span><span class="sxs-lookup"><span data-stu-id="845df-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="845df-179">Na przykład, załóżmy, że zawiera jednostki uczniów `Secret` właściwości, które ta strona sieci web nie może zaktualizować lub dodać:</span><span class="sxs-lookup"><span data-stu-id="845df-179">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="845df-180">Nawet jeśli aplikacja nie ma `Secret` można ustawić za pomocą pola Utwórz/Aktualizuj Razor strony, haker `Secret` przez overposting wartość.</span><span class="sxs-lookup"><span data-stu-id="845df-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="845df-181">Haker można użyć narzędzia, takiego jak Fiddler lub zapisu fragmentów kodu JavaScript można opublikować `Secret` tworzą wartość.</span><span class="sxs-lookup"><span data-stu-id="845df-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="845df-182">Oryginalny kod nie ograniczają pola, które podczas tworzenia wystąpienia uczniów używa integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="845df-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="845df-183">Niezależnie od wartości haker określony dla `Secret` pola formularza jest zaktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="845df-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="845df-184">Na poniższej ilustracji przedstawiono dodawanie narzędzie Fiddler `Secret` pola (o wartości "OverPost") do wartości przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="845df-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Dodawanie pola tajny fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="845df-186">Wartość "OverPost" został pomyślnie dodany do `Secret` właściwości wstawionego wiersza.</span><span class="sxs-lookup"><span data-stu-id="845df-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="845df-187">Projektant aplikacji nie są przeznaczone `Secret` ustawionej z tworzenia strony właściwości.</span><span class="sxs-lookup"><span data-stu-id="845df-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="845df-188">Model widoku</span><span class="sxs-lookup"><span data-stu-id="845df-188">View model</span></span>

<span data-ttu-id="845df-189">Model widoku zwykle zawiera podzbiór właściwości zawarte w modelu używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="845df-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="845df-190">Model aplikacji jest często nazywana modelu domeny.</span><span class="sxs-lookup"><span data-stu-id="845df-190">The application model is often called the domain model.</span></span> <span data-ttu-id="845df-191">Model domeny zwykle zawiera wszystkie właściwości wymagane przez odpowiednie jednostkę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="845df-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="845df-192">Model widoku zawiera tylko właściwości, które są potrzebne dla warstwy interfejsu użytkownika (na przykład tworzenia strony).</span><span class="sxs-lookup"><span data-stu-id="845df-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="845df-193">Oprócz modelu widoku niektóre aplikacje za pomocą powiązania modelu lub modelu wejściowych przekazywania danych między klasę CodeBehind stron Razor i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="845df-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="845df-194">Należy rozważyć `Student` model widoku:</span><span class="sxs-lookup"><span data-stu-id="845df-194">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="845df-195">Wyświetl modele umożliwiają alternatywny sposób, aby zapobiec overposting.</span><span class="sxs-lookup"><span data-stu-id="845df-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="845df-196">Model widoku zawiera tylko właściwości, aby wyświetlić (wyświetlanie) lub zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="845df-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="845df-197">Poniższy kod używa `StudentVM` modelu widoku, aby utworzyć nowy uczniów:</span><span class="sxs-lookup"><span data-stu-id="845df-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="845df-198">[SetValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metody ustawia wartości tego obiektu, odczytując wartości z innej [PropertyValue](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) obiektu.</span><span class="sxs-lookup"><span data-stu-id="845df-198">The [SetValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="845df-199">`SetValues`używa dopasowania nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="845df-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="845df-200">Typ modelu widoku nie musi być powiązany typ modelu, po prostu musi mieć właściwości, które pasują.</span><span class="sxs-lookup"><span data-stu-id="845df-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="845df-201">Przy użyciu `StudentVM` wymaga [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) zaktualizować w celu zastosowania `StudentVM` zamiast `Student`.</span><span class="sxs-lookup"><span data-stu-id="845df-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="845df-202">Na stronach Razor `PageModel` klasy pochodnej jest model widoku.</span><span class="sxs-lookup"><span data-stu-id="845df-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="845df-203">Aktualizacja edycji strony</span><span class="sxs-lookup"><span data-stu-id="845df-203">Update the Edit page</span></span>

<span data-ttu-id="845df-204">Zaktualizuj plik CodeBehind edycji strony:</span><span class="sxs-lookup"><span data-stu-id="845df-204">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="845df-205">Zmiany kodu są podobne do tworzenia strony kilka wyjątków:</span><span class="sxs-lookup"><span data-stu-id="845df-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="845df-206">`OnPostAsync`ma opcjonalny `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="845df-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="845df-207">Bieżący uczniów jest pobierana z bazy danych, zamiast tworzenia uczniów puste.</span><span class="sxs-lookup"><span data-stu-id="845df-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="845df-208">`FirstOrDefaultAsync`zostało zastąpione [metoda FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="845df-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="845df-209">`FindAsync`jest to dobry wybór, wybierając jednostkę z klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="845df-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="845df-210">Zobacz [metoda FindAsync](#FindAsync) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="845df-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="845df-211">Testowanie edytować i tworzyć strony</span><span class="sxs-lookup"><span data-stu-id="845df-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="845df-212">Tworzenie i edytowanie kilku jednostek studenta.</span><span class="sxs-lookup"><span data-stu-id="845df-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="845df-213">Stany jednostki</span><span class="sxs-lookup"><span data-stu-id="845df-213">Entity States</span></span>

<span data-ttu-id="845df-214">Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednich wierszy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="845df-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="845df-215">Informacje o synchronizacji kontekst bazy danych określa, co się stanie po `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="845df-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="845df-216">Na przykład, gdy nowy obiekt jest przekazywana do `Add` — metoda, która stanu jednostki jest ustawiona na `Added`.</span><span class="sxs-lookup"><span data-stu-id="845df-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="845df-217">Gdy `SaveChanges` jest nazywany bazę danych kontekstu wydaje polecenia SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="845df-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="845df-218">Jednostka może być w jednym z następujących stanów:</span><span class="sxs-lookup"><span data-stu-id="845df-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="845df-219">`Added`: Jednostka jeszcze nie istnieje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="845df-219">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="845df-220">`SaveChanges` Metody wystawia instrukcji INSERT.</span><span class="sxs-lookup"><span data-stu-id="845df-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="845df-221">`Unchanged`: Brak zmian muszą być zapisane przy użyciu tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="845df-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="845df-222">Jednostka ma taki stan, gdy jest do odczytu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="845df-222">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="845df-223">`Modified`: Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="845df-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="845df-224">`SaveChanges` Metody wystawia instrukcji UPDATE.</span><span class="sxs-lookup"><span data-stu-id="845df-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="845df-225">`Deleted`: Jednostka została oznaczona do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="845df-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="845df-226">`SaveChanges` Metody wystawia instrukcji DELETE.</span><span class="sxs-lookup"><span data-stu-id="845df-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="845df-227">`Detached`: Jednostka nie jest śledzony przez kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="845df-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="845df-228">W aplikacji komputerowej zmian stanu zwykle są ustawiane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="845df-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="845df-229">Jednostka jest do odczytu, wprowadzono zmiany i stan jednostki automatycznie zmieniona na `Modified`.</span><span class="sxs-lookup"><span data-stu-id="845df-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="845df-230">Wywoływanie `SaveChanges` generuje instrukcję aktualizacji programu SQL, która aktualizuje tylko zmienionych właściwości.</span><span class="sxs-lookup"><span data-stu-id="845df-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="845df-231">W aplikacji sieci web `DbContext` które odczytuje jednostki i wyświetla dane zostanie usunięty po renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="845df-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="845df-232">Gdy stron `OnPostAsync` metoda jest wywoływana, nowych żądań sieci web oraz nowe wystąpienie klasy `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="845df-232">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="845df-233">Ponowne czytanie jednostki, w tym kontekście nowe symuluje przetwarzania pulpitu.</span><span class="sxs-lookup"><span data-stu-id="845df-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="845df-234">Zaktualizuj strony usuwania</span><span class="sxs-lookup"><span data-stu-id="845df-234">Update the Delete page</span></span>

<span data-ttu-id="845df-235">W tej sekcji kod zostanie dodany do wdrożenia błędu niestandardowego komunikatu, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="845df-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="845df-236">Dodaj ciąg zawiera komunikaty o błędach possile:</span><span class="sxs-lookup"><span data-stu-id="845df-236">Add a string to contain possile error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="845df-237">Zastąp `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="845df-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="845df-238">Poprzedni kod zawiera parametr opcjonalny `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="845df-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="845df-239">`saveChangesError`Wskazuje, czy metoda została wywołana po awarii można usunąć obiektu studenta.</span><span class="sxs-lookup"><span data-stu-id="845df-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="845df-240">Operacja usuwania może zakończyć się niepowodzeniem z powodu przejściowych problemów z siecią.</span><span class="sxs-lookup"><span data-stu-id="845df-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="845df-241">Błędy sieci są bardziej prawdopodobne w chmurze.</span><span class="sxs-lookup"><span data-stu-id="845df-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="845df-242">`saveChangesError`ma wartość false, gdy strona usuwania `OnGetAsync` jest wywoływana z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="845df-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="845df-243">Gdy `OnGetAsync` jest wywoływana przez `OnPostAsync` (ponieważ operacja usunięcia nie powiodła się), `saveChangesError` parametr ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="845df-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="845df-244">Metody Delete OnPostAsync stron</span><span class="sxs-lookup"><span data-stu-id="845df-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="845df-245">Zastąp `OnPostAsync` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="845df-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="845df-246">Poprzedni kod pobiera wybranej jednostki, następnie wywołuje `Remove` metody w celu ustawienia stanu jednostki `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="845df-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="845df-247">Gdy `SaveChanges` jest nazywany SQL DELETE wygenerowaniu polecenia.</span><span class="sxs-lookup"><span data-stu-id="845df-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="845df-248">Jeśli `Remove` nie powiedzie się:</span><span class="sxs-lookup"><span data-stu-id="845df-248">If `Remove` fails:</span></span>

* <span data-ttu-id="845df-249">Zostanie przechwycony wyjątek bazy danych.</span><span class="sxs-lookup"><span data-stu-id="845df-249">The DB exception is caught.</span></span>
* <span data-ttu-id="845df-250">Usuń strony `OnGetAsync` metoda jest wywoływana z `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="845df-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="845df-251">Zaktualizuj strony Razor usuwania</span><span class="sxs-lookup"><span data-stu-id="845df-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="845df-252">Dodaj komunikat o błędzie wyróżnione do strony Usuń Razor.</span><span class="sxs-lookup"><span data-stu-id="845df-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="845df-253">Przetestuj Delete.</span><span class="sxs-lookup"><span data-stu-id="845df-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="845df-254">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="845df-254">Common errors</span></span>

<span data-ttu-id="845df-255">Uczniów domowych lub inne łącza nie działa:</span><span class="sxs-lookup"><span data-stu-id="845df-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="845df-256">Sprawdź stronę Razor zawiera poprawny `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="845df-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="845df-257">Na przykład, należy na stronie aparatu Razor uczniów domowych **nie** zawiera szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="845df-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="845df-258">Musi zawierać każdej stronie aparatu Razor `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="845df-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="845df-259">[Poprzednie](xref:data/ef-rp/intro)
[dalej](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="845df-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>