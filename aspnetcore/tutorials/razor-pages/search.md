---
title: Dodaj wyszukiwanie do ASP.NET Core Razor Pages
author: rick-anderson
description: Pokazuje, jak dodać wyszukiwanie do ASP.NET Core Razor Pages
ms.author: riande
ms.date: 7/23/2019
uid: tutorials/razor-pages/search
ms.openlocfilehash: d1aa3f914ebcab4d095b6fca1dac3cf44855d516
ms.sourcegitcommit: 16502797ea749e2690feaa5e652a65b89c007c89
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68483287"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="ed57f-103">Dodaj wyszukiwanie do ASP.NET Core Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ed57f-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="ed57f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed57f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="ed57f-105">W poniższych sekcjach są dodawane przeszukiwania filmów według *gatunku* lub *nazwy* .</span><span class="sxs-lookup"><span data-stu-id="ed57f-105">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="ed57f-106">Dodaj następujące wyróżnione właściwości do *stron/filmów/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="ed57f-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="ed57f-107">`SearchString`: zawiera tekst wprowadzany przez użytkowników w polu tekstowym Wyszukaj.</span><span class="sxs-lookup"><span data-stu-id="ed57f-107">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="ed57f-108">`SearchString`[`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) ma atrybut.</span><span class="sxs-lookup"><span data-stu-id="ed57f-108">`SearchString` is decorated with the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="ed57f-109">`[BindProperty]`tworzy powiązanie wartości formularzy i ciągów zapytań o takiej samej nazwie jak właściwość.</span><span class="sxs-lookup"><span data-stu-id="ed57f-109">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="ed57f-110">`(SupportsGet = true)`jest wymagany do tworzenia powiązań w żądaniach GET.</span><span class="sxs-lookup"><span data-stu-id="ed57f-110">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="ed57f-111">`Genres`: zawiera listę gatunku.</span><span class="sxs-lookup"><span data-stu-id="ed57f-111">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="ed57f-112">`Genres`umożliwia użytkownikowi wybranie gatunku z listy.</span><span class="sxs-lookup"><span data-stu-id="ed57f-112">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="ed57f-113">`SelectList`KONIECZN`using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="ed57f-113">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="ed57f-114">`MovieGenre`: zawiera konkretny gatunek wybierany przez użytkownika (na przykład "zachodni").</span><span class="sxs-lookup"><span data-stu-id="ed57f-114">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="ed57f-115">`Genres`i `MovieGenre` są używane w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ed57f-115">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="ed57f-116">Zaktualizuj `OnGetAsync` metodę strony indeksu przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="ed57f-116">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="ed57f-117">Pierwszy wiersz `OnGetAsync` metody tworzy zapytanie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) do wybierania filmów:</span><span class="sxs-lookup"><span data-stu-id="ed57f-117">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="ed57f-118">Zapytanie jest zdefiniowane *tylko* w tym momencie, **nie** zostało uruchomione względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed57f-118">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="ed57f-119">`SearchString` Jeśli właściwość nie ma wartości null lub jest pusta, zapytanie o filmy jest modyfikowane w celu odfiltrowania ciągu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="ed57f-119">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="ed57f-120">Kod jest wyrażeniem [lambda.](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) `s => s.Title.Contains()`</span><span class="sxs-lookup"><span data-stu-id="ed57f-120">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="ed57f-121">Wyrażenia lambda są używane w kwerendach [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) opartych na metodach jako argumenty dla standardowych metod operatora zapytań, takich jak `Contains` Metoda [WHERE](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) lub (używana w poprzednim kodzie).</span><span class="sxs-lookup"><span data-stu-id="ed57f-121">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="ed57f-122">Zapytania LINQ nie są wykonywane, gdy są zdefiniowane lub są modyfikowane przez wywołanie metody (takiej jak `Where` `Contains` lub `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="ed57f-122">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="ed57f-123">Zamiast tego wykonywanie zapytania jest odroczone.</span><span class="sxs-lookup"><span data-stu-id="ed57f-123">Rather, query execution is deferred.</span></span> <span data-ttu-id="ed57f-124">Oznacza to, że Obliczanie wyrażenia jest opóźnione do momentu przekroczenia jego zrealizowanej wartości lub `ToListAsync` wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="ed57f-124">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="ed57f-125">Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań](/dotnet/framework/data/adonet/ef/language-reference/query-execution) .</span><span class="sxs-lookup"><span data-stu-id="ed57f-125">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="ed57f-126">**Uwaga:** Metoda [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) jest uruchamiana w bazie danych, a C# nie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="ed57f-126">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="ed57f-127">Uwzględnianie wielkości liter w zapytaniu zależy od bazy danych i sortowania.</span><span class="sxs-lookup"><span data-stu-id="ed57f-127">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="ed57f-128">Na SQL Server `Contains` mapuje do [programu SQL Server, np](/sql/t-sql/language-elements/like-transact-sql). bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="ed57f-128">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="ed57f-129">W ramach programu SQLite domyślne sortowanie uwzględnia wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="ed57f-129">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="ed57f-130">Przejdź do strony filmy i dołącz ciąg zapytania, taki jak `?searchString=Ghost` adres URL (na `https://localhost:5001/Movies?searchString=Ghost`przykład).</span><span class="sxs-lookup"><span data-stu-id="ed57f-130">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="ed57f-131">Wyświetlane są filtrowane filmy.</span><span class="sxs-lookup"><span data-stu-id="ed57f-131">The filtered movies are displayed.</span></span>

![Widok indeksu](search/_static/ghost.png)

<span data-ttu-id="ed57f-133">Jeśli do strony indeksu zostanie dodany następujący szablon trasy, ciąg wyszukiwania może zostać przekierowany jako segment adresu URL (na przykład `https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="ed57f-133">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="ed57f-134">Powyższe ograniczenie trasy umożliwia przeszukiwanie tytułu jako dane trasy (segment adresu URL), a nie jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ed57f-134">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="ed57f-135">`?` W`"{searchString?}"` tym przypadku jest to opcjonalny parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="ed57f-135">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Widok indeksu z wyrazem Ghost dodany do adresu URL i zwrotną listą filmów dwóch filmów, Ghostbusters i Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="ed57f-137">Środowisko uruchomieniowe ASP.NET Core używa [powiązania modelu](xref:mvc/models/model-binding) , aby ustawić wartość `SearchString` właściwości z ciągu zapytania (`?searchString=Ghost`) lub danych trasy (`https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="ed57f-137">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="ed57f-138">W powiązaniu modelu nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="ed57f-138">Model binding is not case sensitive.</span></span>

<span data-ttu-id="ed57f-139">Nie można jednak oczekiwać, że użytkownicy modyfikują adres URL w celu wyszukania filmu.</span><span class="sxs-lookup"><span data-stu-id="ed57f-139">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="ed57f-140">W tym kroku zostanie dodany interfejs użytkownika do filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="ed57f-140">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="ed57f-141">Jeśli dodano ograniczenie `"{searchString?}"`trasy, usuń je.</span><span class="sxs-lookup"><span data-stu-id="ed57f-141">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="ed57f-142">Otwórz plik *Pages/Films/index. cshtml* i Dodaj `<form>` znaczniki wyróżnione w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="ed57f-142">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="ed57f-143">Tag HTML `<form>` używa następujących pomocników [tagów](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="ed57f-143">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="ed57f-144">[Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ed57f-144">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="ed57f-145">Gdy formularz zostanie przesłany, ciąg filtru jest wysyłany do *stron/filmów/indeksu* za pośrednictwem ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ed57f-145">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="ed57f-146">Pomocnik tagu wejściowego</span><span class="sxs-lookup"><span data-stu-id="ed57f-146">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="ed57f-147">Zapisz zmiany i przetestuj filtr.</span><span class="sxs-lookup"><span data-stu-id="ed57f-147">Save the changes and test the filter.</span></span>

![Widok indeksu z słowem Ghost wpisanych do pola tekstowego filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="ed57f-149">Wyszukaj według gatunku</span><span class="sxs-lookup"><span data-stu-id="ed57f-149">Search by genre</span></span>

<span data-ttu-id="ed57f-150">`OnGetAsync` Zaktualizuj metodę przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="ed57f-150">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="ed57f-151">Poniższy kod jest zapytanie LINQ, które pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed57f-151">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="ed57f-152">`SelectList` Gatunek jest tworzony przez projekcję odrębnych gatuneków.</span><span class="sxs-lookup"><span data-stu-id="ed57f-152">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="ed57f-153">Dodaj wyszukiwanie według gatunku na stronie Razor</span><span class="sxs-lookup"><span data-stu-id="ed57f-153">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="ed57f-154">Zaktualizuj *indeks. cshtml* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ed57f-154">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="ed57f-155">Przetestuj aplikację, wyszukując według gatunku, tytułu filmu i obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="ed57f-155">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed57f-156">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ed57f-156">Additional resources</span></span>

* [<span data-ttu-id="ed57f-157">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="ed57f-157">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="ed57f-158">[Ubiegł Aktualizowanie kolejnych stron](xref:tutorials/razor-pages/da1):
> [ Dodawanie nowego pola](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="ed57f-158">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="ed57f-159">W poniższych sekcjach są dodawane przeszukiwania filmów według *gatunku* lub *nazwy* .</span><span class="sxs-lookup"><span data-stu-id="ed57f-159">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="ed57f-160">Dodaj następujące wyróżnione właściwości do *stron/filmów/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="ed57f-160">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="ed57f-161">`SearchString`: zawiera tekst wprowadzany przez użytkowników w polu tekstowym Wyszukaj.</span><span class="sxs-lookup"><span data-stu-id="ed57f-161">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="ed57f-162">`SearchString`[`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) ma atrybut.</span><span class="sxs-lookup"><span data-stu-id="ed57f-162">`SearchString` is decorated with the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="ed57f-163">`[BindProperty]`tworzy powiązanie wartości formularzy i ciągów zapytań o takiej samej nazwie jak właściwość.</span><span class="sxs-lookup"><span data-stu-id="ed57f-163">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="ed57f-164">`(SupportsGet = true)`jest wymagany do tworzenia powiązań w żądaniach GET.</span><span class="sxs-lookup"><span data-stu-id="ed57f-164">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="ed57f-165">`Genres`: zawiera listę gatunku.</span><span class="sxs-lookup"><span data-stu-id="ed57f-165">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="ed57f-166">`Genres`umożliwia użytkownikowi wybranie gatunku z listy.</span><span class="sxs-lookup"><span data-stu-id="ed57f-166">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="ed57f-167">`SelectList`KONIECZN`using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="ed57f-167">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="ed57f-168">`MovieGenre`: zawiera konkretny gatunek wybierany przez użytkownika (na przykład "zachodni").</span><span class="sxs-lookup"><span data-stu-id="ed57f-168">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="ed57f-169">`Genres`i `MovieGenre` są używane w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ed57f-169">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="ed57f-170">Zaktualizuj `OnGetAsync` metodę strony indeksu przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="ed57f-170">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="ed57f-171">Pierwszy wiersz `OnGetAsync` metody tworzy zapytanie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) do wybierania filmów:</span><span class="sxs-lookup"><span data-stu-id="ed57f-171">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="ed57f-172">Zapytanie jest zdefiniowane *tylko* w tym momencie, **nie** zostało uruchomione względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed57f-172">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="ed57f-173">`SearchString` Jeśli właściwość nie ma wartości null lub jest pusta, zapytanie o filmy jest modyfikowane w celu odfiltrowania ciągu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="ed57f-173">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="ed57f-174">Kod jest wyrażeniem [lambda.](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) `s => s.Title.Contains()`</span><span class="sxs-lookup"><span data-stu-id="ed57f-174">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="ed57f-175">Wyrażenia lambda są używane w kwerendach [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) opartych na metodach jako argumenty dla standardowych metod operatora zapytań, takich jak `Contains` Metoda [WHERE](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) lub (używana w poprzednim kodzie).</span><span class="sxs-lookup"><span data-stu-id="ed57f-175">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="ed57f-176">Zapytania LINQ nie są wykonywane, gdy są zdefiniowane lub są modyfikowane przez wywołanie metody (takiej jak `Where` `Contains` lub `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="ed57f-176">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="ed57f-177">Zamiast tego wykonywanie zapytania jest odroczone.</span><span class="sxs-lookup"><span data-stu-id="ed57f-177">Rather, query execution is deferred.</span></span> <span data-ttu-id="ed57f-178">Oznacza to, że Obliczanie wyrażenia jest opóźnione do momentu przekroczenia jego zrealizowanej wartości lub `ToListAsync` wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="ed57f-178">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="ed57f-179">Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań](/dotnet/framework/data/adonet/ef/language-reference/query-execution) .</span><span class="sxs-lookup"><span data-stu-id="ed57f-179">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="ed57f-180">**Uwaga:** Metoda [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) jest uruchamiana w bazie danych, a C# nie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="ed57f-180">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="ed57f-181">Uwzględnianie wielkości liter w zapytaniu zależy od bazy danych i sortowania.</span><span class="sxs-lookup"><span data-stu-id="ed57f-181">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="ed57f-182">Na SQL Server `Contains` mapuje do [programu SQL Server, np](/sql/t-sql/language-elements/like-transact-sql). bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="ed57f-182">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="ed57f-183">W ramach programu SQLite domyślne sortowanie uwzględnia wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="ed57f-183">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="ed57f-184">Przejdź do strony filmy i dołącz ciąg zapytania, taki jak `?searchString=Ghost` adres URL (na `https://localhost:5001/Movies?searchString=Ghost`przykład).</span><span class="sxs-lookup"><span data-stu-id="ed57f-184">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="ed57f-185">Wyświetlane są filtrowane filmy.</span><span class="sxs-lookup"><span data-stu-id="ed57f-185">The filtered movies are displayed.</span></span>

![Widok indeksu](search/_static/ghost.png)

<span data-ttu-id="ed57f-187">Jeśli do strony indeksu zostanie dodany następujący szablon trasy, ciąg wyszukiwania może zostać przekierowany jako segment adresu URL (na przykład `https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="ed57f-187">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="ed57f-188">Powyższe ograniczenie trasy umożliwia przeszukiwanie tytułu jako dane trasy (segment adresu URL), a nie jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ed57f-188">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="ed57f-189">`?` W`"{searchString?}"` tym przypadku jest to opcjonalny parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="ed57f-189">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Widok indeksu z wyrazem Ghost dodany do adresu URL i zwrotną listą filmów dwóch filmów, Ghostbusters i Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="ed57f-191">Środowisko uruchomieniowe ASP.NET Core używa [powiązania modelu](xref:mvc/models/model-binding) , aby ustawić wartość `SearchString` właściwości z ciągu zapytania (`?searchString=Ghost`) lub danych trasy (`https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="ed57f-191">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="ed57f-192">W powiązaniu modelu nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="ed57f-192">Model binding is not case sensitive.</span></span>

<span data-ttu-id="ed57f-193">Nie można jednak oczekiwać, że użytkownicy modyfikują adres URL w celu wyszukania filmu.</span><span class="sxs-lookup"><span data-stu-id="ed57f-193">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="ed57f-194">W tym kroku zostanie dodany interfejs użytkownika do filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="ed57f-194">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="ed57f-195">Jeśli dodano ograniczenie `"{searchString?}"`trasy, usuń je.</span><span class="sxs-lookup"><span data-stu-id="ed57f-195">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="ed57f-196">Otwórz plik *Pages/Films/index. cshtml* i Dodaj `<form>` znaczniki wyróżnione w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="ed57f-196">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="ed57f-197">Tag HTML `<form>` używa następujących pomocników [tagów](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="ed57f-197">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="ed57f-198">[Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ed57f-198">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="ed57f-199">Gdy formularz zostanie przesłany, ciąg filtru jest wysyłany do *stron/filmów/indeksu* za pośrednictwem ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ed57f-199">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="ed57f-200">Pomocnik tagu wejściowego</span><span class="sxs-lookup"><span data-stu-id="ed57f-200">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="ed57f-201">Zapisz zmiany i przetestuj filtr.</span><span class="sxs-lookup"><span data-stu-id="ed57f-201">Save the changes and test the filter.</span></span>

![Widok indeksu z słowem Ghost wpisanych do pola tekstowego filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="ed57f-203">Wyszukaj według gatunku</span><span class="sxs-lookup"><span data-stu-id="ed57f-203">Search by genre</span></span>

<span data-ttu-id="ed57f-204">`OnGetAsync` Zaktualizuj metodę przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="ed57f-204">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="ed57f-205">Poniższy kod jest zapytanie LINQ, które pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed57f-205">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="ed57f-206">`SelectList` Gatunek jest tworzony przez projekcję odrębnych gatuneków.</span><span class="sxs-lookup"><span data-stu-id="ed57f-206">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="ed57f-207">Dodaj wyszukiwanie według gatunku na stronie Razor</span><span class="sxs-lookup"><span data-stu-id="ed57f-207">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="ed57f-208">Zaktualizuj *indeks. cshtml* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ed57f-208">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="ed57f-209">Przetestuj aplikację, wyszukując według gatunku, tytułu filmu i obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="ed57f-209">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed57f-210">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ed57f-210">Additional resources</span></span>

* [<span data-ttu-id="ed57f-211">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="ed57f-211">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="ed57f-212">[Ubiegł Aktualizowanie kolejnych stron](xref:tutorials/razor-pages/da1):
> [ Dodawanie nowego pola](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="ed57f-212">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end