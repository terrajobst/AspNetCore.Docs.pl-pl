---
title: Stron razor podstawowych EF - CRUD - 2 8
author: rick-anderson
description: "Przedstawia sposób tworzenia, odczytu, aktualizowanie i usuwanie EF podstawowych"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: 757aeb713b645cea0fe633b150784184d2d3571e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/31/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="77989-103">Tworzenia, odczytu, aktualizacji i usuwania - Core EF Razor strony (2 8)</span><span class="sxs-lookup"><span data-stu-id="77989-103">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="77989-104">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="77989-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="77989-105">W tym samouczku CRUD szkieletu (tworzenia, odczytu, aktualizowanie i usuwanie) kodu jest przejrzeć i dostosować.</span><span class="sxs-lookup"><span data-stu-id="77989-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="77989-106">Uwaga: Aby zminimalizować złożoność i zachować te samouczki koncentruje się na podstawowych EF, EF Core kod jest używany w modelach strony stron Razor.</span><span class="sxs-lookup"><span data-stu-id="77989-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="77989-107">Niektórzy deweloperzy Użyj wzorca usługi warstwy lub repozytorium w, aby utworzyć warstwę abstrakcji między interfejsu użytkownika (Razor strony) i warstwa dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="77989-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="77989-108">W w tym samouczku, tworzenia, edycji, usuwania i szczegóły stron Razor w *uczniowie* folderu zostaną zmodyfikowane.</span><span class="sxs-lookup"><span data-stu-id="77989-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="77989-109">Kod z utworzonym szkieletem korzysta ze wzorca następujące tworzenie, edytowanie i usuwanie stron:</span><span class="sxs-lookup"><span data-stu-id="77989-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="77989-110">Pobierz i Wyświetl żądanych danych z metodą HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="77989-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="77989-111">Zapisz zmiany w danych z metodą HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="77989-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="77989-112">Strony indeksu i szczegóły Pobierz i Wyświetl żądanych danych z metodą HTTP GET`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="77989-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="77989-113">Zamień SingleOrDefaultAsync FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="77989-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="77989-114">Wygenerowany kod używa [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) można pobrać żądanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="77989-114">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="77989-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) jest bardziej wydajny w pobieranie jedną jednostkę:</span><span class="sxs-lookup"><span data-stu-id="77989-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="77989-116">Jeśli kod należy sprawdzić, czy nie ma więcej niż jednej jednostki zwróconych przez kwerendę.</span><span class="sxs-lookup"><span data-stu-id="77989-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="77989-117">`SingleOrDefaultAsync`pobiera dane i niepotrzebne działa.</span><span class="sxs-lookup"><span data-stu-id="77989-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="77989-118">`SingleOrDefaultAsync`zgłasza wyjątek, jeśli istnieje więcej niż jednej jednostki, która pasuje do części filtru.</span><span class="sxs-lookup"><span data-stu-id="77989-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="77989-119">`FirstOrDefaultAsync`nie zgłoszenia, jeśli istnieje więcej niż jednej jednostki, która pasuje do części filtru.</span><span class="sxs-lookup"><span data-stu-id="77989-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="77989-120">Zastąp globalnie `SingleOrDefaultAsync` z `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="77989-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="77989-121">`SingleOrDefaultAsync`jest używany w miejscach 5:</span><span class="sxs-lookup"><span data-stu-id="77989-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="77989-122">`OnGetAsync`na stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="77989-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="77989-123">`OnGetAsync`i `OnPostAsync` na edytowanie i usuwanie stron.</span><span class="sxs-lookup"><span data-stu-id="77989-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="77989-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="77989-124">FindAsync</span></span>

<span data-ttu-id="77989-125">W bardzo szkieletu kodu [metoda FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) można użyć zamiast `FirstOrDefaultAsync` lub `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="77989-125">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="77989-126">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="77989-126">`FindAsync`:</span></span>

* <span data-ttu-id="77989-127">Znajduje obiekt z (klucz podstawowy) klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="77989-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="77989-128">Jeśli jednostka entity z atrybutem na PK jest śledzony przez kontekst, jest zwracany bez żądania z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="77989-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="77989-129">To proste i zwięzłe.</span><span class="sxs-lookup"><span data-stu-id="77989-129">Is simple and concise.</span></span>
* <span data-ttu-id="77989-130">Jest zoptymalizowany do odszukania pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="77989-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="77989-131">Może korzystnie wydajności w niektórych sytuacjach, ale rzadko pochodzą do gry dla scenariuszy normalne sieci web.</span><span class="sxs-lookup"><span data-stu-id="77989-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="77989-132">Niejawnie wykorzystuje [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) zamiast [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="77989-132">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="77989-133">Ale jeśli chcesz dołączyć inne jednostki, a następnie znajdź nie jest już właściwe.</span><span class="sxs-lookup"><span data-stu-id="77989-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="77989-134">Oznacza to, że może być konieczne porzucenie Znajdź i przejdź do zapytania w miarę postępów Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77989-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="77989-135">Dostosowywanie strony szczegółów</span><span class="sxs-lookup"><span data-stu-id="77989-135">Customize the Details page</span></span>

<span data-ttu-id="77989-136">Przejdź do `Pages/Students` strony.</span><span class="sxs-lookup"><span data-stu-id="77989-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="77989-137">**Edytuj**, **szczegóły**, i **usunąć** łącza są generowane przez [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/uczniów lub studentów / Index.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="77989-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="77989-138">Wybierz łącze Szczegóły.</span><span class="sxs-lookup"><span data-stu-id="77989-138">Select a Details link.</span></span> <span data-ttu-id="77989-139">Adres URL ma postać `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="77989-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="77989-140">Identyfikator uczniów jest przekazywany za pomocą ciągu zapytania (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="77989-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="77989-141">Zaktualizuj edycji, szczegóły i usuwanie stron Razor, aby użyć `"{id:int}"` szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="77989-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="77989-142">Zmiany w dyrektywie page dla każdego z tych stron z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="77989-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="77989-143">Żądanie do strony przy użyciu szablonu trasy "{identyfikator: int}", który wykonuje **nie** obejmują całkowitą trasy wartość zwraca HTTP (nie znaleziono) błąd 404.</span><span class="sxs-lookup"><span data-stu-id="77989-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="77989-144">Na przykład `http://localhost:5000/Students/Details` zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="77989-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="77989-145">Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="77989-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="77989-146">Uruchom aplikację, kliknij łącze Szczegóły i sprawdź adres URL jest przekazanie identyfikator jako dane trasy (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="77989-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="77989-147">Nie zmieniaj globalnie `@page` do `@page "{id:int}"`, sposób podziału tak łącza do strony głównej i tworzenie stron.</span><span class="sxs-lookup"><span data-stu-id="77989-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="77989-148">Dodawanie danych powiązanych</span><span class="sxs-lookup"><span data-stu-id="77989-148">Add related data</span></span>

<span data-ttu-id="77989-149">Nie zawiera utworzony szkielet kodu strony indeksu studentów `Enrollments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="77989-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="77989-150">W tej sekcji zawartości `Enrollments` Kolekcja zostanie wyświetlona na stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="77989-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="77989-151">`OnGetAsync` Metody *Pages/Students/Details.cshtml.cs* używa `FirstOrDefaultAsync` metoda pobierania pojedynczy `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="77989-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="77989-152">Dodaj następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="77989-152">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="77989-153">`Include` i `ThenInclude` metody powodują kontekstu ładowania `Student.Enrollments` właściwość nawigacji i w obrębie każdej rejestracji `Enrollment.Course` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="77989-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="77989-154">Te metody są examinied szczegółowo w samouczku dane dotyczące odczytu.</span><span class="sxs-lookup"><span data-stu-id="77989-154">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="77989-155">`AsNoTracking` — Metoda zwiększa wydajność w scenariuszach, gdy zwracany jednostek nie są aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="77989-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="77989-156">`AsNoTracking`omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="77989-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="77989-157">Na stronie Szczegóły są wyświetlane powiązane rejestracji</span><span class="sxs-lookup"><span data-stu-id="77989-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="77989-158">Otwórz *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="77989-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="77989-159">Dodaj następujący wyróżniony kod, aby wyświetlić listę rejestracji:</span><span class="sxs-lookup"><span data-stu-id="77989-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="77989-160">Jeśli po wklejeniu kod wcięcia kodu o nieprawidłowej naciśnij kombinację klawiszy CTRL-K-D, aby naprawić błąd.</span><span class="sxs-lookup"><span data-stu-id="77989-160">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="77989-161">Poprzedni kod w pętli jednostek w `Enrollments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="77989-161">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="77989-162">Dla każdego rejestracji Wyświetla tytuł kursu i kategorii.</span><span class="sxs-lookup"><span data-stu-id="77989-162">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="77989-163">Tytuł kursu są pobierane z jednostki kursu, która jest przechowywana w `Course` właściwości nawigacji jednostki rejestracji.</span><span class="sxs-lookup"><span data-stu-id="77989-163">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="77989-164">Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **szczegóły** łącze studenta.</span><span class="sxs-lookup"><span data-stu-id="77989-164">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="77989-165">Zostanie wyświetlona lista kursów i klasy dla wybranego uczniów.</span><span class="sxs-lookup"><span data-stu-id="77989-165">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="77989-166">Utwórz stronę aktualizacji</span><span class="sxs-lookup"><span data-stu-id="77989-166">Update the Create page</span></span>

<span data-ttu-id="77989-167">Aktualizacja `OnPostAsync` metody w *Pages/Students/Create.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="77989-167">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="77989-168">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="77989-168">TryUpdateModelAsync</span></span>

<span data-ttu-id="77989-169">Sprawdź [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kodu:</span><span class="sxs-lookup"><span data-stu-id="77989-169">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="77989-170">W powyższym kodzie `TryUpdateModelAsync<Student>` próbuje zaktualizować `emptyStudent` przy użyciu wartości przesłanego formularza z [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) właściwości w [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="77989-170">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="77989-171">`TryUpdateModelAsync`tylko aktualizuje właściwości wymienionych (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="77989-171">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="77989-172">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="77989-172">In the preceding sample:</span></span>

* <span data-ttu-id="77989-173">Drugi argument (` "student", // Prefix`) to prefiks używany do odszukania wartości.</span><span class="sxs-lookup"><span data-stu-id="77989-173">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="77989-174">Nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="77989-174">It's not case sensitive.</span></span>
* <span data-ttu-id="77989-175">Wartości przesłanego formularza są konwertowane na typy w `Student` modelu przy użyciu [modelu powiązania](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="77989-175">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="77989-176">Overposting</span><span class="sxs-lookup"><span data-stu-id="77989-176">Overposting</span></span>

<span data-ttu-id="77989-177">Przy użyciu `TryUpdateModel` można zaktualizować pola z wartościami oczekujących na opublikowanie jest ze względów bezpieczeństwa, ponieważ nie dopuszcza overposting.</span><span class="sxs-lookup"><span data-stu-id="77989-177">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="77989-178">Na przykład, załóżmy, że zawiera jednostki uczniów `Secret` właściwości, które ta strona sieci web nie należy zaktualizować lub dodać:</span><span class="sxs-lookup"><span data-stu-id="77989-178">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="77989-179">Nawet jeśli aplikacja nie ma `Secret` można ustawić za pomocą pola Utwórz/Aktualizuj Razor strony, haker `Secret` przez overposting wartość.</span><span class="sxs-lookup"><span data-stu-id="77989-179">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="77989-180">Haker można użyć narzędzia, takiego jak Fiddler lub zapisu fragmentów kodu JavaScript można opublikować `Secret` tworzą wartość.</span><span class="sxs-lookup"><span data-stu-id="77989-180">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="77989-181">Oryginalny kod nie ograniczają pola, które podczas tworzenia wystąpienia uczniów używa integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="77989-181">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="77989-182">Niezależnie od wartości haker określony dla `Secret` pola formularza jest zaktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="77989-182">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="77989-183">Na poniższej ilustracji przedstawiono dodawanie narzędzie Fiddler `Secret` pola (o wartości "OverPost") do wartości przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="77989-183">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Dodawanie pola tajny fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="77989-185">Wartość "OverPost" został pomyślnie dodany do `Secret` właściwości wstawionego wiersza.</span><span class="sxs-lookup"><span data-stu-id="77989-185">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="77989-186">Projektant aplikacji nie są przeznaczone `Secret` ustawionej z tworzenia strony właściwości.</span><span class="sxs-lookup"><span data-stu-id="77989-186">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="77989-187">Model widoku</span><span class="sxs-lookup"><span data-stu-id="77989-187">View model</span></span>

<span data-ttu-id="77989-188">Model widoku zwykle zawiera podzbiór właściwości zawarte w modelu używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="77989-188">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="77989-189">Model aplikacji jest często nazywana modelu domeny.</span><span class="sxs-lookup"><span data-stu-id="77989-189">The application model is often called the domain model.</span></span> <span data-ttu-id="77989-190">Model domeny zwykle zawiera wszystkie właściwości wymagane przez odpowiednie jednostkę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="77989-190">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="77989-191">Model widoku zawiera tylko właściwości, które są potrzebne dla warstwy interfejsu użytkownika (na przykład tworzenia strony).</span><span class="sxs-lookup"><span data-stu-id="77989-191">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="77989-192">Oprócz modelu widoku niektóre aplikacje za pomocą powiązania modelu lub modelu wejściowych przekazywania danych między klasy modelu stron Razor strony i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="77989-192">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="77989-193">Należy rozważyć `Student` model widoku:</span><span class="sxs-lookup"><span data-stu-id="77989-193">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="77989-194">Wyświetl modele umożliwiają alternatywny sposób, aby zapobiec overposting.</span><span class="sxs-lookup"><span data-stu-id="77989-194">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="77989-195">Model widoku zawiera tylko właściwości, aby wyświetlić (wyświetlanie) lub zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="77989-195">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="77989-196">Poniższy kod używa `StudentVM` modelu widoku, aby utworzyć nowy uczniów:</span><span class="sxs-lookup"><span data-stu-id="77989-196">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="77989-197">[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metody ustawia wartości tego obiektu, odczytując wartości z innej [PropertyValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) obiektu.</span><span class="sxs-lookup"><span data-stu-id="77989-197">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="77989-198">`SetValues`używa dopasowania nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="77989-198">`SetValues` uses property name matching.</span></span> <span data-ttu-id="77989-199">Typ modelu widoku nie musi być powiązany typ modelu, po prostu musi mieć właściwości, które pasują.</span><span class="sxs-lookup"><span data-stu-id="77989-199">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="77989-200">Przy użyciu `StudentVM` wymaga [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) zaktualizować w celu zastosowania `StudentVM` zamiast `Student`.</span><span class="sxs-lookup"><span data-stu-id="77989-200">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="77989-201">Na stronach Razor `PageModel` klasy pochodnej jest model widoku.</span><span class="sxs-lookup"><span data-stu-id="77989-201">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="77989-202">Aktualizacja edycji strony</span><span class="sxs-lookup"><span data-stu-id="77989-202">Update the Edit page</span></span>

<span data-ttu-id="77989-203">Aktualizacja modelu strony dla strony edycji:</span><span class="sxs-lookup"><span data-stu-id="77989-203">Update the page model for the Edit page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="77989-204">Zmiany kodu są podobne do tworzenia strony kilka wyjątków:</span><span class="sxs-lookup"><span data-stu-id="77989-204">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="77989-205">`OnPostAsync`ma opcjonalny `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="77989-205">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="77989-206">Bieżący uczniów jest pobierana z bazy danych, zamiast tworzenia uczniów puste.</span><span class="sxs-lookup"><span data-stu-id="77989-206">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="77989-207">`FirstOrDefaultAsync`zostało zastąpione [metoda FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="77989-207">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="77989-208">`FindAsync`jest to dobry wybór, wybierając jednostkę z klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="77989-208">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="77989-209">Zobacz [metoda FindAsync](#FindAsync) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="77989-209">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="77989-210">Testowanie edytować i tworzyć strony</span><span class="sxs-lookup"><span data-stu-id="77989-210">Test the Edit and Create pages</span></span>

<span data-ttu-id="77989-211">Tworzenie i edytowanie kilku jednostek studenta.</span><span class="sxs-lookup"><span data-stu-id="77989-211">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="77989-212">Stany jednostki</span><span class="sxs-lookup"><span data-stu-id="77989-212">Entity States</span></span>

<span data-ttu-id="77989-213">Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednich wierszy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="77989-213">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="77989-214">Informacje o synchronizacji kontekst bazy danych określa, co się stanie po `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="77989-214">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="77989-215">Na przykład, gdy nowy obiekt jest przekazywana do `Add` — metoda, która stanu jednostki jest ustawiona na `Added`.</span><span class="sxs-lookup"><span data-stu-id="77989-215">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="77989-216">Gdy `SaveChanges` jest nazywany bazę danych kontekstu wydaje polecenia SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="77989-216">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="77989-217">Jednostka może być w jednym z następujących stanów:</span><span class="sxs-lookup"><span data-stu-id="77989-217">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="77989-218">`Added`: Jednostka jeszcze nie istnieje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="77989-218">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="77989-219">`SaveChanges` Metody wystawia instrukcji INSERT.</span><span class="sxs-lookup"><span data-stu-id="77989-219">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="77989-220">`Unchanged`: Brak zmian muszą być zapisane przy użyciu tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="77989-220">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="77989-221">Jednostka ma taki stan, gdy jest do odczytu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77989-221">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="77989-222">`Modified`: Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="77989-222">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="77989-223">`SaveChanges` Metody wystawia instrukcji UPDATE.</span><span class="sxs-lookup"><span data-stu-id="77989-223">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="77989-224">`Deleted`: Jednostka została oznaczona do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="77989-224">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="77989-225">`SaveChanges` Metody wystawia instrukcji DELETE.</span><span class="sxs-lookup"><span data-stu-id="77989-225">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="77989-226">`Detached`: Jednostka nie jest śledzony przez kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77989-226">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="77989-227">W aplikacji komputerowej zmian stanu zwykle są ustawiane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="77989-227">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="77989-228">Jednostka jest do odczytu, wprowadzono zmiany i stan jednostki automatycznie zmieniona na `Modified`.</span><span class="sxs-lookup"><span data-stu-id="77989-228">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="77989-229">Wywoływanie `SaveChanges` generuje instrukcję aktualizacji programu SQL, która aktualizuje tylko zmienionych właściwości.</span><span class="sxs-lookup"><span data-stu-id="77989-229">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="77989-230">W aplikacji sieci web `DbContext` które odczytuje jednostki i wyświetla dane zostanie usunięty po renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="77989-230">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="77989-231">Gdy stron `OnPostAsync` metoda jest wywoływana, nowych żądań sieci web oraz nowe wystąpienie klasy `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="77989-231">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="77989-232">Ponowne czytanie jednostki, w tym kontekście nowe symuluje przetwarzania pulpitu.</span><span class="sxs-lookup"><span data-stu-id="77989-232">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="77989-233">Zaktualizuj strony usuwania</span><span class="sxs-lookup"><span data-stu-id="77989-233">Update the Delete page</span></span>

<span data-ttu-id="77989-234">W tej sekcji kod zostanie dodany do wdrożenia błędu niestandardowego komunikatu, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="77989-234">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="77989-235">Dodaj ciąg zawierający komunikatów o błędach:</span><span class="sxs-lookup"><span data-stu-id="77989-235">Add a string to contain possible error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="77989-236">Zastąp `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="77989-236">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="77989-237">Poprzedni kod zawiera parametr opcjonalny `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="77989-237">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="77989-238">`saveChangesError`Wskazuje, czy metoda została wywołana po awarii można usunąć obiektu studenta.</span><span class="sxs-lookup"><span data-stu-id="77989-238">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="77989-239">Operacja usuwania może zakończyć się niepowodzeniem z powodu przejściowych problemów z siecią.</span><span class="sxs-lookup"><span data-stu-id="77989-239">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="77989-240">Błędy sieci są bardziej prawdopodobne w chmurze.</span><span class="sxs-lookup"><span data-stu-id="77989-240">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="77989-241">`saveChangesError`ma wartość false, gdy strona usuwania `OnGetAsync` jest wywoływana z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="77989-241">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="77989-242">Gdy `OnGetAsync` jest wywoływana przez `OnPostAsync` (ponieważ operacja usunięcia nie powiodła się), `saveChangesError` parametr ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="77989-242">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="77989-243">Metody Delete OnPostAsync stron</span><span class="sxs-lookup"><span data-stu-id="77989-243">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="77989-244">Zastąp `OnPostAsync` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="77989-244">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="77989-245">Poprzedni kod pobiera wybranej jednostki, następnie wywołuje `Remove` metody w celu ustawienia stanu jednostki `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="77989-245">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="77989-246">Gdy `SaveChanges` jest nazywany SQL DELETE wygenerowaniu polecenia.</span><span class="sxs-lookup"><span data-stu-id="77989-246">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="77989-247">Jeśli `Remove` nie powiedzie się:</span><span class="sxs-lookup"><span data-stu-id="77989-247">If `Remove` fails:</span></span>

* <span data-ttu-id="77989-248">Zostanie przechwycony wyjątek bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77989-248">The DB exception is caught.</span></span>
* <span data-ttu-id="77989-249">Usuń strony `OnGetAsync` metoda jest wywoływana z `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="77989-249">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="77989-250">Zaktualizuj strony Razor usuwania</span><span class="sxs-lookup"><span data-stu-id="77989-250">Update the Delete Razor Page</span></span>

<span data-ttu-id="77989-251">Dodaj komunikat o błędzie wyróżnione do strony Usuń Razor.</span><span class="sxs-lookup"><span data-stu-id="77989-251">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="77989-252">Przetestuj Delete.</span><span class="sxs-lookup"><span data-stu-id="77989-252">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="77989-253">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="77989-253">Common errors</span></span>

<span data-ttu-id="77989-254">Uczniów domowych lub inne łącza nie działa:</span><span class="sxs-lookup"><span data-stu-id="77989-254">Student/Home or other links don't work:</span></span>

<span data-ttu-id="77989-255">Sprawdź stronę Razor zawiera poprawny `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="77989-255">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="77989-256">Na przykład, należy na stronie aparatu Razor uczniów domowych **nie** zawiera szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="77989-256">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="77989-257">Musi zawierać każdej stronie aparatu Razor `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="77989-257">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="77989-258">[Poprzednie](xref:data/ef-rp/intro)
[dalej](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="77989-258">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
