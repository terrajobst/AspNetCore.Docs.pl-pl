---
title: Strony razor z programem EF Core w programie ASP.NET Core — CRUD - 2, 8
author: rick-anderson
description: Pokazuje, jak tworzyć, odczytywać, aktualizować, usuwać z programem EF Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 0a8d386104d9c62f37d0a94e66344d3f1e23bae9
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063354"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="f7f7e-103">Strony razor z programem EF Core w programie ASP.NET Core — CRUD - 2, 8</span><span class="sxs-lookup"><span data-stu-id="f7f7e-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f7f7e-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f7f7e-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="f7f7e-105">W tym samouczku szkieletu CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) przejrzeniu i dostosowywać kod.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="f7f7e-106">Aby zminimalizować złożoność, zachowując tych samouczków skupia się na programu EF Core, kod programem EF Core jest używany w modelach strony.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-106">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="f7f7e-107">Niektórzy deweloperzy Użyj wzorca usługi warstwy lub repozytorium, w, aby utworzyć warstwę abstrakcji między interfejsu użytkownika (Razor strony) i warstwy dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="f7f7e-108">W tym samouczku, Utwórz, Edytuj, Usuń i szczegóły stron Razor w *uczniów* są sprawdzane w folderze.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are examined.</span></span>

<span data-ttu-id="f7f7e-109">Tworzenie, edytowanie i usuwanie stron utworzony szkielet kodu korzysta z następującego wzorca:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="f7f7e-110">Pobieranie i wyświetlanie żądanych danych za pomocą metody HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="f7f7e-111">Zapisz zmiany w danych za pomocą metody POST protokołu HTTP `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="f7f7e-112">Strony indeksu i szczegóły get i wyświetlić żądanych danych z metodą HTTP GET `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="f7f7e-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="f7f7e-113">SingleOrDefaultAsync vs FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="f7f7e-113">SingleOrDefaultAsync vs FirstOrDefaultAsync</span></span>

<span data-ttu-id="f7f7e-114">Wygenerowany kod używa [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), która jest ogólnie metoda preferowana nad [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-114">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="f7f7e-115">`FirstOrDefaultAsync` jest bardziej wydajne niż `SingleOrDefaultAsync` na pobranie jednej jednostki:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-115">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="f7f7e-116">Jeśli kod wymaga sprawdzić, czy nie ma więcej niż jednej jednostki zwróconych przez kwerendę.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="f7f7e-117">`SingleOrDefaultAsync` Pobieranie większej ilości danych, a niepotrzebne działa.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="f7f7e-118">`SingleOrDefaultAsync` zgłasza wyjątek, jeśli istnieje więcej niż jednej jednostce, która pasuje do część filtru.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="f7f7e-119">`FirstOrDefaultAsync` nie wyrzuca, jeśli istnieje więcej niż jednej jednostce, która pasuje do część filtru.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>

### <a name="findasync"></a><span data-ttu-id="f7f7e-120">FindAsync</span><span class="sxs-lookup"><span data-stu-id="f7f7e-120">FindAsync</span></span>

<span data-ttu-id="f7f7e-121">W bardzo utworzony szkielet kodu [metoda findasync dla](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) mogą być używane zamiast `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-121">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="f7f7e-122">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-122">`FindAsync`:</span></span>

* <span data-ttu-id="f7f7e-123">Umożliwia znalezienie jednostki za pomocą kluczowi podstawowemu (PK).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-123">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="f7f7e-124">Jeśli jednostki z PK jest śledzony przez kontekst, jest zwracany bez żądania z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-124">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="f7f7e-125">Jest proste i zwięzłe.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-125">Is simple and concise.</span></span>
* <span data-ttu-id="f7f7e-126">Jest zoptymalizowana pod kątem wyszukiwania pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-126">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="f7f7e-127">Może korzystnie wpływać na wydajności w niektórych sytuacjach, ale są rzadko się dzieje w przypadku aplikacji sieci web typowy.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-127">Can have perf benefits in some situations, but they rarely happens for typical web apps.</span></span>
* <span data-ttu-id="f7f7e-128">Niejawnie wykorzystuje [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) zamiast [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-128">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="f7f7e-129">Ale jeśli chcesz `Include` innych podmiotów, następnie `FindAsync` nie jest właściwe.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-129">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="f7f7e-130">Oznacza to, że może być konieczne porzucenie `FindAsync` i Zacznij korzystać z zapytania w miarę postępów Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-130">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="f7f7e-131">Dostosowywanie strony szczegółów</span><span class="sxs-lookup"><span data-stu-id="f7f7e-131">Customize the Details page</span></span>

<span data-ttu-id="f7f7e-132">Przejdź do `Pages/Students` strony.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-132">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="f7f7e-133">**Edytuj**, **szczegóły**, i **Usuń** łącza są generowane przez [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/studentów / Index.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-133">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="f7f7e-134">Uruchom aplikację i wybierz **szczegóły** łącza.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-134">Run the app and select a **Details** link.</span></span> <span data-ttu-id="f7f7e-135">Adres URL ma postać `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-135">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="f7f7e-136">Identyfikator dla uczniów jest przekazywany przy użyciu ciągu zapytania (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-136">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="f7f7e-137">Aktualizuj edycji, szczegóły i usuwanie stron Razor, aby użyć `"{id:int}"` szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-137">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="f7f7e-138">Zmiany strony dyrektyw dla każdej z tych stron z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-138">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="f7f7e-139">Żądanie strony za pomocą szablonu trasy "{id: int}", który wykonuje **nie** obejmują całkowitą trasy wartość zwraca HTTP (nie znaleziono) błąd 404.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-139">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="f7f7e-140">Na przykład `http://localhost:5000/Students/Details` zwróci błąd 404.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-140">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="f7f7e-141">Aby wprowadzić identyfikator jest opcjonalne, należy dołączyć `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-141">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="f7f7e-142">Uruchom aplikację, kliknij łącze, szczegóły i sprawdź adres URL jest przekazanie identyfikator jako dane trasy (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-142">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="f7f7e-143">Nie należy zmieniać globalnie `@page` do `@page "{id:int}"`, wykonując podział tak łącza do strony głównej i tworzenie stron.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-143">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="f7f7e-144">Dodawanie danych powiązanych</span><span class="sxs-lookup"><span data-stu-id="f7f7e-144">Add related data</span></span>

<span data-ttu-id="f7f7e-145">Nie zawiera utworzony szkielet kodu strony indeksu studentów `Enrollments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-145">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="f7f7e-146">W tej sekcji zawartości `Enrollments` Kolekcja zostanie wyświetlona na stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-146">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="f7f7e-147">`OnGetAsync` Metody *Pages/Students/Details.cshtml.cs* używa `FirstOrDefaultAsync` metodę, aby pobrać jeden `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-147">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="f7f7e-148">Dodaj następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-148">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="f7f7e-149">[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) i [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) metody powodują kontekst można załadować `Student.Enrollments` właściwość nawigacji i w ramach każdej rejestracji `Enrollment.Course` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-149">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="f7f7e-150">Te metody są badane szczegółowo w tym samouczku dane dotyczące odczytu.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-150">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="f7f7e-151">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) metody zwiększa wydajność w scenariuszach, gdy jednostki nie są aktualizowane w bieżącym kontekście.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-151">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="f7f7e-152">`AsNoTracking` omówiono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-152">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="f7f7e-153">Wyświetl powiązane rejestracje na stronie szczegółów</span><span class="sxs-lookup"><span data-stu-id="f7f7e-153">Display related enrollments on the Details page</span></span>

<span data-ttu-id="f7f7e-154">Otwórz *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-154">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="f7f7e-155">Dodaj następujący wyróżniony kod do wyświetlania listy rejestracji:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-155">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="f7f7e-156">W przypadku wcięcia kodu o szerokość problem po wklejeniu kodu, naciśnij kombinację klawiszy CTRL-K-D go poprawić.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-156">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="f7f7e-157">Powyższy kod pętli jednostek w `Enrollments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-157">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="f7f7e-158">Do każdej rejestracji Wyświetla tytuł kursu i klasy korporacyjnej.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-158">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="f7f7e-159">Tytuł kursu jest pobierana z kursu jednostki, która jest przechowywana w `Course` właściwości nawigacji jednostki rejestracji.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-159">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="f7f7e-160">Uruchom aplikację, wybierz **studentów** kartę, a następnie kliknij przycisk **szczegóły** link dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-160">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="f7f7e-161">Zostanie wyświetlona lista kursów i ocen dla wybranych uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-161">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="f7f7e-162">Utwórz stronę aktualizacji</span><span class="sxs-lookup"><span data-stu-id="f7f7e-162">Update the Create page</span></span>

<span data-ttu-id="f7f7e-163">Aktualizacja `OnPostAsync` method in Class metoda *Pages/Students/Create.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-163">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a><span data-ttu-id="f7f7e-164">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="f7f7e-164">TryUpdateModelAsync</span></span>

<span data-ttu-id="f7f7e-165">Sprawdź [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kodu:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-165">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="f7f7e-166">W poprzednim kodzie `TryUpdateModelAsync<Student>` próbuje zaktualizować `emptyStudent` przy użyciu wartości przesłanego formularza z [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) właściwość [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-166">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="f7f7e-167">`TryUpdateModelAsync` tylko zaktualizowanie właściwości podanych (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-167">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="f7f7e-168">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-168">In the preceding sample:</span></span>

* <span data-ttu-id="f7f7e-169">Drugi argument (`"student", // Prefix`) jest prefiksem używa do wyszukiwania wartości.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-169">The second argument (`"student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="f7f7e-170">Nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-170">It's not case sensitive.</span></span>
* <span data-ttu-id="f7f7e-171">Wartości przesłanego formularza są konwertowane na typy w `Student` modelu przy użyciu [wiązanie modelu](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-171">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>

### <a name="overposting"></a><span data-ttu-id="f7f7e-172">Polegającymi</span><span class="sxs-lookup"><span data-stu-id="f7f7e-172">Overposting</span></span>

<span data-ttu-id="f7f7e-173">Za pomocą `TryUpdateModel` można zaktualizować pola z wartościami przesłanych jest ze względów bezpieczeństwa, ponieważ zapobiega overposting.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-173">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="f7f7e-174">Załóżmy, że zawiera jednostki dla uczniów `Secret` właściwość, która nie należy zaktualizować tę stronę sieci web, czy dodać:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-174">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="f7f7e-175">Nawet jeśli aplikacja nie ma `Secret` można ustawić pole na stronę Razor, haker tworzenia/aktualizacji `Secret` przez polegającymi wartość.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-175">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="f7f7e-176">Haker może użyć narzędzia, takiego jak Fiddler lub zapisu fragmentów kodu JavaScript do opublikowania `Secret` stanowią wartość.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-176">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="f7f7e-177">Oryginalny kod nie są ograniczone pola, które podczas tworzenia wystąpienia dla uczniów używa integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-177">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="f7f7e-178">Niezależnie od wartości haker, określony dla `Secret` pola formularza jest zaktualizowana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-178">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="f7f7e-179">Na poniższej ilustracji przedstawiono dodawanie narzędzie Fiddler `Secret` pola (z wartością "OverPost") do wartości przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-179">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Dodawanie pola wpisu tajnego programu fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="f7f7e-181">Wartość "OverPost" został pomyślnie dodany do `Secret` właściwość wstawionego wiersza.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-181">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="f7f7e-182">Projektant aplikacji nie są przeznaczone `Secret` właściwość można ustawić za pomocą tworzenia strony.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-182">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="f7f7e-183">Model widoku</span><span class="sxs-lookup"><span data-stu-id="f7f7e-183">View model</span></span>

<span data-ttu-id="f7f7e-184">Model widoku zazwyczaj zawiera podzbiór właściwości zawarte w modelu, używanych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-184">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="f7f7e-185">Model aplikacji jest często określany mianem modelu domeny.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-185">The application model is often called the domain model.</span></span> <span data-ttu-id="f7f7e-186">Model domeny zwykle zawiera wszystkie właściwości, które są wymagane przez odpowiednie jednostki w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-186">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="f7f7e-187">Model widoku zawiera tylko właściwości potrzebne dla warstwy interfejsu użytkownika (na przykład, Utwórz stronę).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-187">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="f7f7e-188">Oprócz modelu widoku niektóre aplikacje używają do przesyłania danych między klasy modelu strony stron Razor i przeglądarka powiązanie modelu lub model danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-188">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="f7f7e-189">Należy wziąć pod uwagę następujące `Student` model widoku:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-189">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="f7f7e-190">Wyświetl modele zapewniają alternatywny sposób, aby zapobiec overposting.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-190">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="f7f7e-191">Model widoku zawiera tylko właściwości można wyświetlić (wyświetlanie) ani zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-191">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="f7f7e-192">Poniższy kod używa `StudentVM` model widoku, aby utworzyć nowego studenta:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-192">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="f7f7e-193">[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metody ustawia wartości tego obiektu, odczytując wartości z innej [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) obiektu.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-193">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="f7f7e-194">`SetValues` używa Dopasowywanie nazw właściwości.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-194">`SetValues` uses property name matching.</span></span> <span data-ttu-id="f7f7e-195">Typ modelu widoku nie musi być powiązany typ modelu, po prostu musi ona mieć właściwości, które odpowiadają.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-195">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="f7f7e-196">Za pomocą `StudentVM` wymaga [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) zaktualizowana w celu używania `StudentVM` zamiast `Student`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-196">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="f7f7e-197">W przypadku stron Razor `PageModel` klasy pochodnej jest model widoku.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-197">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="f7f7e-198">Zaktualizuj strony edytowania</span><span class="sxs-lookup"><span data-stu-id="f7f7e-198">Update the Edit page</span></span>

<span data-ttu-id="f7f7e-199">Aktualizowanie modelu strony dla strony edytowania.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-199">Update the page model for the Edit page.</span></span> <span data-ttu-id="f7f7e-200">Istotne zmiany są wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-200">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="f7f7e-201">Zmiany kodu są podobne do tworzenia strony z pewnymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-201">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="f7f7e-202">`OnPostAsync` ma opcjonalny `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-202">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="f7f7e-203">Bieżący ucznia jest pobierana z bazy danych, zamiast tworzenia puste dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-203">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="f7f7e-204">`FirstOrDefaultAsync` został zastąpiony [metoda findasync dla](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-204">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="f7f7e-205">`FindAsync` jest to dobry wybór, podczas wybierania jednostki z klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-205">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="f7f7e-206">Zobacz [metoda findasync dla](#FindAsync) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-206">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="f7f7e-207">Testowanie edycji i tworzenie stron</span><span class="sxs-lookup"><span data-stu-id="f7f7e-207">Test the Edit and Create pages</span></span>

<span data-ttu-id="f7f7e-208">Tworzenie i edytowanie kilka jednostek dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-208">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="f7f7e-209">Stany jednostki</span><span class="sxs-lookup"><span data-stu-id="f7f7e-209">Entity States</span></span>

<span data-ttu-id="f7f7e-210">Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednie wiersze w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-210">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="f7f7e-211">Informacje o synchronizacji w kontekście bazy danych określa, co się stanie, gdy [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-211">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="f7f7e-212">Na przykład, gdy nowa jednostka jest przekazywany do [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) metody, która stan obiektu jest ustawiony na [dodano](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span><span class="sxs-lookup"><span data-stu-id="f7f7e-212">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="f7f7e-213">Gdy `SaveChangesAsync` jest wywoływana, bazy danych kontekstu wydaje polecenie SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-213">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="f7f7e-214">Jednostki mogą być w jednym z [następujące stany](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span><span class="sxs-lookup"><span data-stu-id="f7f7e-214">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="f7f7e-215">`Added`: Jednostki jeszcze nie istnieje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-215">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="f7f7e-216">`SaveChanges` Metoda generuje instrukcji INSERT.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-216">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="f7f7e-217">`Unchanged`: Bez zmian konieczne zapisane przy użyciu tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-217">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="f7f7e-218">Określona jednostka ma ten stan, gdy jest do odczytu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-218">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="f7f7e-219">`Modified`: Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-219">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="f7f7e-220">`SaveChanges` Metoda generuje instrukcji UPDATE.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-220">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="f7f7e-221">`Deleted`: Jednostki został oznaczony do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-221">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="f7f7e-222">`SaveChanges` Metoda generuje instrukcję DELETE.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-222">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="f7f7e-223">`Detached`: Jednostki nie jest śledzony przez kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-223">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="f7f7e-224">W przypadku aplikacji klasycznej zmiany stanu są zazwyczaj ustawiane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-224">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="f7f7e-225">Jednostki jest do odczytu, zostaną wprowadzone zmiany i stanu jednostki automatycznie był zmieniany na `Modified`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-225">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="f7f7e-226">Wywoływanie `SaveChanges` generuje instrukcję aktualizacji programu SQL, która aktualizuje tylko zmienionych właściwości.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-226">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="f7f7e-227">W aplikacji sieci web `DbContext` który odczytuje jednostki i wyświetla dane, zostanie usunięty po wyrenderowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-227">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="f7f7e-228">Gdy strona `OnPostAsync` metoda jest wywoływana, nowe żądania sieci web i nowe wystąpienie klasy `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-228">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="f7f7e-229">Ponowne czytanie jednostki, w tym kontekście nowych symuluje pulpitu przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-229">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="f7f7e-230">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="f7f7e-230">Update the Delete page</span></span>

<span data-ttu-id="f7f7e-231">W tej sekcji kodu zostanie dodany do zaimplementowania błąd niestandardowy komunikat, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-231">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="f7f7e-232">Dodaj ciąg ma zawierać komunikatów o błędach:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-232">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="f7f7e-233">Zastąp `OnGetAsync` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-233">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="f7f7e-234">Powyższy kod zawiera opcjonalny parametr `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-234">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="f7f7e-235">`saveChangesError` Wskazuje, czy metoda została wywołana po nieudanej próbie usunięcia obiektu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-235">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="f7f7e-236">Operacja usuwania może zakończyć się niepowodzeniem ze względu na przejściowe problemy z siecią.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-236">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="f7f7e-237">Błędy przejściowe problemy z siecią jest bardziej prawdopodobne w chmurze.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-237">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="f7f7e-238">`saveChangesError`ma wartość FAŁSZ, gdy strona usuwania `OnGetAsync` jest wywoływana z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-238">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="f7f7e-239">Gdy `OnGetAsync` jest wywoływana przez `OnPostAsync` (ponieważ operacja usuwania nie powiodła się), `saveChangesError` parametr ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-239">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="f7f7e-240">Metoda OnPostAsync stron Delete</span><span class="sxs-lookup"><span data-stu-id="f7f7e-240">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="f7f7e-241">Zastąp `OnPostAsync` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-241">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="f7f7e-242">Powyższy kod pobiera wybranej jednostki, następnie wywołuje [Usuń](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) metodę, aby ustawić stan jednostki `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-242">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="f7f7e-243">Gdy `SaveChanges` nosi SQL DELETE polecenia jest generowany.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-243">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="f7f7e-244">Jeśli `Remove` zakończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-244">If `Remove` fails:</span></span>

* <span data-ttu-id="f7f7e-245">Zostaje przechwycony wyjątek bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-245">The DB exception is caught.</span></span>
* <span data-ttu-id="f7f7e-246">Usuwać strony `OnGetAsync` metoda jest wywoływana z `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-246">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="f7f7e-247">Strona Razor usuwania aktualizacji</span><span class="sxs-lookup"><span data-stu-id="f7f7e-247">Update the Delete Razor Page</span></span>

<span data-ttu-id="f7f7e-248">Dodaj następujący wyróżniony komunikat o błędzie do strony Razor usunąć.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-248">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="f7f7e-249">Przetestuj Delete.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-249">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="f7f7e-250">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="f7f7e-250">Common errors</span></span>

<span data-ttu-id="f7f7e-251">Nie działają dla uczniów/Index lub inne łącza:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-251">Student/Index or other links don't work:</span></span>

<span data-ttu-id="f7f7e-252">Sprawdź stronę Razor zawiera poprawny `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-252">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="f7f7e-253">Na przykład strony Razor dla uczniów/indeksu powinna **nie** zawiera szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="f7f7e-253">For example, The Student/Index Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="f7f7e-254">Musi zawierać każdej strony Razor `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="f7f7e-254">Each Razor Page must include the `@page` directive.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="f7f7e-255">[Poprzednie](xref:data/ef-rp/intro)
> [dalej](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="f7f7e-255">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
