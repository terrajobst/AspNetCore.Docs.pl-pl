---
title: Stron razor podstawowych EF w platformy ASP.NET Core - CRUD - 2 8
author: rick-anderson
description: Przedstawia sposób tworzenia, odczytu, aktualizowanie i usuwanie EF podstawowych
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 157257d10306ded3456cd66c186a82edf0ba5d65
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961324"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="0cdc9-103">Stron razor podstawowych EF w platformy ASP.NET Core - CRUD - 2 8</span><span class="sxs-lookup"><span data-stu-id="0cdc9-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="0cdc9-104">Wersja platformy ASP.NET Core 2.0 tego samouczka można znaleźć w [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-104">The ASP.NET Core 2.0 version of this tutorial can be found in [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0cdc9-105">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0cdc9-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="0cdc9-106">W tym samouczku CRUD szkieletu (tworzenia, odczytu, aktualizowanie i usuwanie) kodu jest przejrzeć i dostosować.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="0cdc9-107">Aby zminimalizować złożoność i zachować te samouczki koncentruje się na podstawowych EF, EF Core kod jest używany w modelach strony.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-107">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="0cdc9-108">Niektórzy deweloperzy Użyj wzorca usługi warstwy lub repozytorium w, aby utworzyć warstwę abstrakcji między interfejsu użytkownika (Razor strony) i warstwa dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="0cdc9-109">W w tym samouczku, tworzenia, edycji, usuwania i szczegóły stron Razor w *uczniowie* folderu są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are examined.</span></span>

<span data-ttu-id="0cdc9-110">Kod z utworzonym szkieletem korzysta ze wzorca następujące tworzenie, edytowanie i usuwanie stron:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="0cdc9-111">Pobierz i Wyświetl żądanych danych z metodą HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="0cdc9-112">Zapisz zmiany w danych z metodą HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="0cdc9-113">Strony indeksu i szczegóły Pobierz i Wyświetl żądanych danych z metodą HTTP GET `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="0cdc9-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="0cdc9-114">SingleOrDefaultAsync vs FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="0cdc9-114">SingleOrDefaultAsync vs FirstOrDefaultAsync</span></span>

<span data-ttu-id="0cdc9-115">Wygenerowany kod używa [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), która jest zazwyczaj preferowany nad [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-115">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="0cdc9-116">`FirstOrDefaultAsync` jest bardziej efektywne niż `SingleOrDefaultAsync` na jedną jednostkę pobierania:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-116">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="0cdc9-117">Jeśli kod należy sprawdzić, czy nie ma więcej niż jednej jednostki zwróconych przez kwerendę.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-117">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="0cdc9-118">`SingleOrDefaultAsync` pobiera dane i niepotrzebne działa.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="0cdc9-119">`SingleOrDefaultAsync` zgłasza wyjątek, jeśli istnieje więcej niż jednej jednostki, która pasuje do części filtru.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-119">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="0cdc9-120">`FirstOrDefaultAsync` nie zgłoszenia, jeśli istnieje więcej niż jednej jednostki, która pasuje do części filtru.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-120">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="0cdc9-121">FindAsync</span><span class="sxs-lookup"><span data-stu-id="0cdc9-121">FindAsync</span></span>

<span data-ttu-id="0cdc9-122">W bardzo szkieletu kodu [metoda FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) można użyć zamiast `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-122">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="0cdc9-123">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-123">`FindAsync`:</span></span>

* <span data-ttu-id="0cdc9-124">Znajduje obiekt z (klucz podstawowy) klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-124">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="0cdc9-125">Jeśli jednostka entity z atrybutem na PK jest śledzony przez kontekst, jest zwracany bez żądania z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-125">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="0cdc9-126">To proste i zwięzłe.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-126">Is simple and concise.</span></span>
* <span data-ttu-id="0cdc9-127">Jest zoptymalizowany do odszukania pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-127">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="0cdc9-128">Może korzystnie wydajności w niektórych sytuacjach, ale rzadko pochodzą do gry dla aplikacji sieci web typowych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-128">Can have perf benefits in some situations, but they rarely come into play for typical web apps.</span></span>
* <span data-ttu-id="0cdc9-129">Niejawnie wykorzystuje [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) zamiast [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-129">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="0cdc9-130">Ale jeśli chcesz `Include` inne jednostki, następnie `FindAsync` nie jest właściwe.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-130">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="0cdc9-131">Oznacza to, że może być konieczne porzucenie `FindAsync` i przejdź do zapytania w trakcie Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-131">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="0cdc9-132">Dostosowywanie strony szczegółów</span><span class="sxs-lookup"><span data-stu-id="0cdc9-132">Customize the Details page</span></span>

<span data-ttu-id="0cdc9-133">Przejdź do `Pages/Students` strony.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-133">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="0cdc9-134">**Edytuj**, **szczegóły**, i **usunąć** łącza są generowane przez [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/uczniów lub studentów / Index.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-134">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="0cdc9-135">Uruchom aplikację i wybierz **szczegóły** łącza.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-135">Run the app and select a **Details** link.</span></span> <span data-ttu-id="0cdc9-136">Adres URL ma postać `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-136">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="0cdc9-137">Identyfikator uczniów jest przekazywany za pomocą ciągu zapytania (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-137">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="0cdc9-138">Zaktualizuj edycji, szczegóły i usuwanie stron Razor, aby użyć `"{id:int}"` szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-138">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="0cdc9-139">Zmiany w dyrektywie page dla każdego z tych stron z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-139">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="0cdc9-140">Żądanie do strony przy użyciu szablonu trasy "{identyfikator: int}", który wykonuje **nie** obejmują całkowitą trasy wartość zwraca HTTP (nie znaleziono) błąd 404.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-140">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="0cdc9-141">Na przykład `http://localhost:5000/Students/Details` zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-141">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="0cdc9-142">Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-142">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="0cdc9-143">Uruchom aplikację, kliknij łącze Szczegóły i sprawdź adres URL jest przekazanie identyfikator jako dane trasy (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-143">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="0cdc9-144">Nie zmieniaj globalnie `@page` do `@page "{id:int}"`, sposób podziału tak łącza do strony głównej i tworzenie stron.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-144">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="0cdc9-145">Dodawanie danych powiązanych</span><span class="sxs-lookup"><span data-stu-id="0cdc9-145">Add related data</span></span>

<span data-ttu-id="0cdc9-146">Nie zawiera utworzony szkielet kodu strony indeksu studentów `Enrollments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-146">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="0cdc9-147">W tej sekcji zawartości `Enrollments` Kolekcja zostanie wyświetlona na stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-147">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="0cdc9-148">`OnGetAsync` Metody *Pages/Students/Details.cshtml.cs* używa `FirstOrDefaultAsync` metoda pobierania pojedynczy `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-148">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="0cdc9-149">Dodaj następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-149">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="0cdc9-150">[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) i [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) metody powodują kontekstu ładowania `Student.Enrollments` właściwość nawigacji i w obrębie każdej rejestracji `Enrollment.Course` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-150">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="0cdc9-151">Te metody są sprawdzane szczegółowo w samouczku dane dotyczące odczytu.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-151">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="0cdc9-152">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) — metoda zwiększa wydajność w scenariuszach, gdy zwracany jednostek nie są aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-152">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="0cdc9-153">`AsNoTracking` omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-153">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="0cdc9-154">Na stronie Szczegóły są wyświetlane powiązane rejestracji</span><span class="sxs-lookup"><span data-stu-id="0cdc9-154">Display related enrollments on the Details page</span></span>

<span data-ttu-id="0cdc9-155">Otwórz *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-155">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="0cdc9-156">Dodaj następujący wyróżniony kod, aby wyświetlić listę rejestracji:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-156">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="0cdc9-157">Jeśli po wklejeniu kod wcięcia kodu o nieprawidłowej naciśnij kombinację klawiszy CTRL-K-D, aby naprawić błąd.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-157">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="0cdc9-158">Poprzedni kod w pętli jednostek w `Enrollments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-158">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="0cdc9-159">Dla każdego rejestracji Wyświetla tytuł kursu i kategorii.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-159">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="0cdc9-160">Tytuł kursu są pobierane z jednostki kursu, która jest przechowywana w `Course` właściwości nawigacji jednostki rejestracji.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-160">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="0cdc9-161">Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **szczegóły** łącze studenta.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-161">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="0cdc9-162">Zostanie wyświetlona lista kursów i klasy dla wybranego uczniów.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-162">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="0cdc9-163">Utwórz stronę aktualizacji</span><span class="sxs-lookup"><span data-stu-id="0cdc9-163">Update the Create page</span></span>

<span data-ttu-id="0cdc9-164">Aktualizacja `OnPostAsync` metody w *Pages/Students/Create.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-164">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="0cdc9-165">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="0cdc9-165">TryUpdateModelAsync</span></span>

<span data-ttu-id="0cdc9-166">Sprawdź [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kodu:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-166">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="0cdc9-167">W powyższym kodzie `TryUpdateModelAsync<Student>` próbuje zaktualizować `emptyStudent` przy użyciu wartości przesłanego formularza z [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) właściwości w [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-167">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="0cdc9-168">`TryUpdateModelAsync` tylko aktualizuje właściwości wymienionych (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-168">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="0cdc9-169">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-169">In the preceding sample:</span></span>

* <span data-ttu-id="0cdc9-170">Drugi argument (` "student", // Prefix`) to prefiks używany do odszukania wartości.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-170">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="0cdc9-171">Nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-171">It's not case sensitive.</span></span>
* <span data-ttu-id="0cdc9-172">Wartości przesłanego formularza są konwertowane na typy w `Student` modelu przy użyciu [modelu powiązania](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-172">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="0cdc9-173">Overposting</span><span class="sxs-lookup"><span data-stu-id="0cdc9-173">Overposting</span></span>

<span data-ttu-id="0cdc9-174">Przy użyciu `TryUpdateModel` można zaktualizować pola z wartościami oczekujących na opublikowanie jest ze względów bezpieczeństwa, ponieważ nie dopuszcza overposting.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-174">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="0cdc9-175">Na przykład, załóżmy, że zawiera jednostki uczniów `Secret` właściwości, które ta strona sieci web nie należy zaktualizować lub dodać:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-175">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="0cdc9-176">Nawet jeśli aplikacja nie ma `Secret` można ustawić za pomocą pola Utwórz/Aktualizuj Razor strony, haker `Secret` przez overposting wartość.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-176">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="0cdc9-177">Haker można użyć narzędzia, takiego jak Fiddler lub zapisu fragmentów kodu JavaScript można opublikować `Secret` tworzą wartość.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-177">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="0cdc9-178">Oryginalny kod nie ograniczają pola, które podczas tworzenia wystąpienia uczniów używa integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-178">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="0cdc9-179">Niezależnie od wartości haker określony dla `Secret` pola formularza jest zaktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-179">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="0cdc9-180">Na poniższej ilustracji przedstawiono dodawanie narzędzie Fiddler `Secret` pola (o wartości "OverPost") do wartości przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-180">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Dodawanie pola tajny fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="0cdc9-182">Wartość "OverPost" został pomyślnie dodany do `Secret` właściwości wstawionego wiersza.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-182">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="0cdc9-183">Projektant aplikacji nie są przeznaczone `Secret` ustawionej z tworzenia strony właściwości.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-183">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="0cdc9-184">Model widoku</span><span class="sxs-lookup"><span data-stu-id="0cdc9-184">View model</span></span>

<span data-ttu-id="0cdc9-185">Model widoku zwykle zawiera podzbiór właściwości zawarte w modelu używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-185">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="0cdc9-186">Model aplikacji jest często nazywana modelu domeny.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-186">The application model is often called the domain model.</span></span> <span data-ttu-id="0cdc9-187">Model domeny zwykle zawiera wszystkie właściwości wymagane przez odpowiednie jednostkę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-187">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="0cdc9-188">Model widoku zawiera tylko właściwości, które są potrzebne dla warstwy interfejsu użytkownika (na przykład tworzenia strony).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-188">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="0cdc9-189">Oprócz modelu widoku niektóre aplikacje za pomocą powiązania modelu lub modelu wejściowych przekazywania danych między klasy modelu stron Razor strony i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-189">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="0cdc9-190">Należy rozważyć `Student` model widoku:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-190">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="0cdc9-191">Wyświetl modele umożliwiają alternatywny sposób, aby zapobiec overposting.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-191">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="0cdc9-192">Model widoku zawiera tylko właściwości, aby wyświetlić (wyświetlanie) lub zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-192">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="0cdc9-193">Poniższy kod używa `StudentVM` modelu widoku, aby utworzyć nowy uczniów:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-193">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="0cdc9-194">[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metody ustawia wartości tego obiektu, odczytując wartości z innej [PropertyValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) obiektu.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-194">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="0cdc9-195">`SetValues` używa dopasowania nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-195">`SetValues` uses property name matching.</span></span> <span data-ttu-id="0cdc9-196">Typ modelu widoku nie musi być powiązany typ modelu, po prostu musi mieć właściwości, które pasują.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-196">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="0cdc9-197">Przy użyciu `StudentVM` wymaga [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) zaktualizować w celu zastosowania `StudentVM` zamiast `Student`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-197">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="0cdc9-198">Na stronach Razor `PageModel` klasy pochodnej jest model widoku.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-198">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="0cdc9-199">Aktualizacja edycji strony</span><span class="sxs-lookup"><span data-stu-id="0cdc9-199">Update the Edit page</span></span>

<span data-ttu-id="0cdc9-200">Aktualizuj model strony dla strony edycji.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-200">Update the page model for the Edit page.</span></span> <span data-ttu-id="0cdc9-201">Istotne zmiany są wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-201">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="0cdc9-202">Zmiany kodu są podobne do tworzenia strony kilka wyjątków:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-202">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="0cdc9-203">`OnPostAsync` ma opcjonalny `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-203">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="0cdc9-204">Bieżący uczniów jest pobierana z bazy danych, zamiast tworzenia uczniów puste.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-204">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="0cdc9-205">`FirstOrDefaultAsync` zostało zastąpione [metoda FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-205">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="0cdc9-206">`FindAsync` jest to dobry wybór, wybierając jednostkę z klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-206">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="0cdc9-207">Zobacz [metoda FindAsync](#FindAsync) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-207">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="0cdc9-208">Testowanie edytować i tworzyć strony</span><span class="sxs-lookup"><span data-stu-id="0cdc9-208">Test the Edit and Create pages</span></span>

<span data-ttu-id="0cdc9-209">Tworzenie i edytowanie kilku jednostek studenta.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-209">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="0cdc9-210">Stany jednostki</span><span class="sxs-lookup"><span data-stu-id="0cdc9-210">Entity States</span></span>

<span data-ttu-id="0cdc9-211">Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednich wierszy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-211">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="0cdc9-212">Informacje o synchronizacji kontekst bazy danych określa, co się stanie po [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-212">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="0cdc9-213">Na przykład, gdy nowy obiekt jest przekazywana do [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) — metoda, która stanu jednostki jest ustawiona na [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span><span class="sxs-lookup"><span data-stu-id="0cdc9-213">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="0cdc9-214">Gdy `SaveChangesAsync` jest nazywany bazę danych kontekstu wydaje polecenia SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-214">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="0cdc9-215">Jednostka może działać w jednym z [następujące stany](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span><span class="sxs-lookup"><span data-stu-id="0cdc9-215">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="0cdc9-216">`Added`: Jednostka jeszcze nie istnieje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-216">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="0cdc9-217">`SaveChanges` Metody wystawia instrukcji INSERT.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-217">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="0cdc9-218">`Unchanged`: Brak zmian muszą być zapisane przy użyciu tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-218">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="0cdc9-219">Jednostka ma taki stan, gdy jest do odczytu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-219">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="0cdc9-220">`Modified`: Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-220">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="0cdc9-221">`SaveChanges` Metody wystawia instrukcji UPDATE.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-221">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="0cdc9-222">`Deleted`: Jednostka została oznaczona do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-222">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="0cdc9-223">`SaveChanges` Metody wystawia instrukcji DELETE.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-223">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="0cdc9-224">`Detached`: Jednostka nie jest śledzony przez kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-224">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="0cdc9-225">W aplikacji komputerowej zmian stanu zwykle są ustawiane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-225">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="0cdc9-226">Jednostka jest do odczytu, wprowadzono zmiany i stan jednostki automatycznie zmieniona na `Modified`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-226">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="0cdc9-227">Wywoływanie `SaveChanges` generuje instrukcję aktualizacji programu SQL, która aktualizuje tylko zmienionych właściwości.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-227">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="0cdc9-228">W aplikacji sieci web `DbContext` które odczytuje jednostki i wyświetla dane zostanie usunięty po renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-228">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="0cdc9-229">Jeśli na stronie `OnPostAsync` metoda jest wywoływana, nowych żądań sieci web oraz nowe wystąpienie klasy `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-229">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="0cdc9-230">Ponowne czytanie jednostki, w tym kontekście nowe symuluje przetwarzania pulpitu.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-230">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="0cdc9-231">Zaktualizuj strony usuwania</span><span class="sxs-lookup"><span data-stu-id="0cdc9-231">Update the Delete page</span></span>

<span data-ttu-id="0cdc9-232">W tej sekcji kod zostanie dodany do wdrożenia błędu niestandardowego komunikatu, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-232">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="0cdc9-233">Dodaj ciąg zawierający komunikatów o błędach:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-233">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="0cdc9-234">Zastąp `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-234">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="0cdc9-235">Poprzedni kod zawiera parametr opcjonalny `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-235">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="0cdc9-236">`saveChangesError` Wskazuje, czy metoda została wywołana po awarii można usunąć obiektu studenta.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-236">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="0cdc9-237">Operacja usuwania może zakończyć się niepowodzeniem z powodu przejściowych problemów z siecią.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-237">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="0cdc9-238">Błędy sieci są bardziej prawdopodobne w chmurze.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-238">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="0cdc9-239">`saveChangesError`ma wartość false, gdy strona usuwania `OnGetAsync` jest wywoływana z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-239">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="0cdc9-240">Gdy `OnGetAsync` jest wywoływana przez `OnPostAsync` (ponieważ operacja usunięcia nie powiodła się), `saveChangesError` parametr ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-240">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="0cdc9-241">Metody Delete OnPostAsync stron</span><span class="sxs-lookup"><span data-stu-id="0cdc9-241">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="0cdc9-242">Zastąp `OnPostAsync` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-242">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="0cdc9-243">Poprzedni kod pobiera wybranej jednostki, następnie wywołuje [Usuń](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) metody w celu ustawienia stanu jednostki `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-243">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="0cdc9-244">Gdy `SaveChanges` jest nazywany SQL DELETE wygenerowaniu polecenia.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-244">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="0cdc9-245">Jeśli `Remove` nie powiedzie się:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-245">If `Remove` fails:</span></span>

* <span data-ttu-id="0cdc9-246">Zostanie przechwycony wyjątek bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-246">The DB exception is caught.</span></span>
* <span data-ttu-id="0cdc9-247">Usuń strony `OnGetAsync` metoda jest wywoływana z `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-247">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="0cdc9-248">Zaktualizuj strony Razor usuwania</span><span class="sxs-lookup"><span data-stu-id="0cdc9-248">Update the Delete Razor Page</span></span>

<span data-ttu-id="0cdc9-249">Dodaj komunikat o błędzie wyróżnione do strony Usuń Razor.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-249">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="0cdc9-250">Przetestuj Delete.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-250">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="0cdc9-251">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="0cdc9-251">Common errors</span></span>

<span data-ttu-id="0cdc9-252">Uczniów domowych lub inne łącza nie działa:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-252">Student/Home or other links don't work:</span></span>

<span data-ttu-id="0cdc9-253">Sprawdź stronę Razor zawiera poprawny `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-253">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="0cdc9-254">Na przykład, należy na stronie aparatu Razor uczniów domowych **nie** zawiera szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="0cdc9-254">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="0cdc9-255">Musi zawierać każdej stronie aparatu Razor `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="0cdc9-255">Each Razor Page must include the `@page` directive.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="0cdc9-256">[Poprzednie](xref:data/ef-rp/intro)
> [dalej](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="0cdc9-256">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
