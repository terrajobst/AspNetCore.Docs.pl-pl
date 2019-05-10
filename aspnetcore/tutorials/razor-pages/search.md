---
title: Dodawanie wyszukiwania do stron Razor programu ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać wyszukiwanie do stron Razor programu ASP.NET Core
ms.author: riande
ms.date: 12/3/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: d0ce13e87c3f5e66008f308f6258403ea37b8847
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899515"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="5e909-103">Dodawanie wyszukiwania do stron Razor programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e909-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="5e909-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5e909-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="5e909-105">W poniższych sekcjach wyszukiwanie filmów przez *gatunku* lub *nazwa* zostanie dodany.</span><span class="sxs-lookup"><span data-stu-id="5e909-105">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="5e909-106">Dodaj następujące właściwości wyróżniony, aby *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5e909-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="5e909-107">`SearchString`: zawiera tekst, użytkownicy wprowadzają w polu tekstowym wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="5e909-107">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="5e909-108">`SearchString` zostanie nadany [ `[BindProperty]` ](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5e909-108">`SearchString` is decorated with the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="5e909-109">`[BindProperty]` wiąże wartości formularza i ciągi zapytania z taką samą nazwę jak właściwość.</span><span class="sxs-lookup"><span data-stu-id="5e909-109">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="5e909-110">`(SupportsGet = true)` jest wymagany dla wiązania w odpowiedzi na żądania GET.</span><span class="sxs-lookup"><span data-stu-id="5e909-110">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="5e909-111">`Genres`: zawiera listę gatunki.</span><span class="sxs-lookup"><span data-stu-id="5e909-111">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="5e909-112">`Genres` Zezwala użytkownikowi na wybranie określonego rodzaju z listy.</span><span class="sxs-lookup"><span data-stu-id="5e909-112">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="5e909-113">`SelectList` wymaga `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="5e909-113">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="5e909-114">`MovieGenre`: zawiera gatunku określonych przez użytkownika (na przykład "zachodni").</span><span class="sxs-lookup"><span data-stu-id="5e909-114">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="5e909-115">`Genres` i `MovieGenre` są używane w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5e909-115">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="5e909-116">Aktualizuj stronę indeksu `OnGetAsync` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5e909-116">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="5e909-117">W pierwszym wierszu `OnGetAsync` metoda tworzy [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) zapytanie, aby wybrać filmów:</span><span class="sxs-lookup"><span data-stu-id="5e909-117">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="5e909-118">Zapytanie jest *tylko* zdefiniowane w tym momencie, ma ona **nie** zostały uruchomione dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e909-118">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="5e909-119">Jeśli `SearchString` właściwość nie jest zerowa lub pusta, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania ciąg wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="5e909-119">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="5e909-120">`s => s.Title.Contains()` Kod jest [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="5e909-120">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="5e909-121">Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metody lub `Contains` (używane w poprzednim kodzie).</span><span class="sxs-lookup"><span data-stu-id="5e909-121">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="5e909-122">Zapytania LINQ nie są wykonywane, gdy są one definiowane lub ich modyfikacji przez wywołanie metody (takie jak `Where`, `Contains` lub `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="5e909-122">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="5e909-123">Przeciwnie wykonanie zapytania jest odroczone.</span><span class="sxs-lookup"><span data-stu-id="5e909-123">Rather, query execution is deferred.</span></span> <span data-ttu-id="5e909-124">Oznacza to, że obliczania wyrażenia została opóźniona do momentu jego rzeczywista wartość jest powtarzana lub `ToListAsync` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="5e909-124">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="5e909-125">Zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="5e909-125">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="5e909-126">**Uwaga:** [Zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda jest uruchomiona w bazie danych, nie w C# kodu.</span><span class="sxs-lookup"><span data-stu-id="5e909-126">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="5e909-127">Rozróżnianie wielkości liter w zapytaniu, zależy od bazy danych i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="5e909-127">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="5e909-128">W programie SQL Server `Contains` mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5e909-128">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="5e909-129">W SQLite przy użyciu sortowania domyślnego, jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5e909-129">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="5e909-130">Przejdź do strony filmy i Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL (na przykład `https://localhost:5001/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="5e909-130">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="5e909-131">Wyświetlane są filtrowane filmów.</span><span class="sxs-lookup"><span data-stu-id="5e909-131">The filtered movies are displayed.</span></span>

![Widok indeksu](search/_static/ghost.png)

<span data-ttu-id="5e909-133">Jeśli następujący szablon trasy zostanie dodany do strony indeksu, ciąg wyszukiwania mogą być przekazywane jako segment adresu URL (na przykład `https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="5e909-133">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="5e909-134">Poprzedni ograniczenia trasy umożliwia wyszukiwanie tytuł, np. dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5e909-134">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="5e909-135">`?` w `"{searchString?}"` oznacza, że jest to parametr opcjonalny trasy.</span><span class="sxs-lookup"><span data-stu-id="5e909-135">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Widok indeksu z ghost word dodawany do adresu Url i zwrócony filmu listę filmów Ghostbusters i Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="5e909-137">Środowisko uruchomieniowe programu ASP.NET Core używa [wiązanie modelu](xref:mvc/models/model-binding) można ustawić wartość `SearchString` właściwości z ciągu zapytania (`?searchString=Ghost`) lub kierować dane (`https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="5e909-137">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="5e909-138">Powiązanie modelu nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5e909-138">Model binding is not case sensitive.</span></span>

<span data-ttu-id="5e909-139">Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, aby wyszukać film.</span><span class="sxs-lookup"><span data-stu-id="5e909-139">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="5e909-140">W tym kroku interfejs użytkownika jest dodawany do filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="5e909-140">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="5e909-141">Jeśli dodano ograniczenia trasy `"{searchString?}"`, usuń go.</span><span class="sxs-lookup"><span data-stu-id="5e909-141">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="5e909-142">Otwórz *Pages/Movies/Index.cshtml* pliku i Dodaj `<form>` znaczników wyróżnione w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="5e909-142">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="5e909-143">Kod HTML `<form>` tag korzysta z następujących [pomocników tagów](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="5e909-143">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="5e909-144">[Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="5e909-144">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="5e909-145">Po przesłaniu formularza ciąg filtru są wysyłane do *stron/filmy/Index* strony za pomocą ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5e909-145">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="5e909-146">Pomocnik tagu wejściowego</span><span class="sxs-lookup"><span data-stu-id="5e909-146">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="5e909-147">Zapisz zmiany i przetestować filtr.</span><span class="sxs-lookup"><span data-stu-id="5e909-147">Save the changes and test the filter.</span></span>

![Widok indeksu z ghost słowa wpisane w polu tekstowym filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="5e909-149">Wyszukaj według gatunku</span><span class="sxs-lookup"><span data-stu-id="5e909-149">Search by genre</span></span>

<span data-ttu-id="5e909-150">Aktualizacja `OnGetAsync` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5e909-150">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="5e909-151">Poniższy kod jest zapytanie LINQ, która pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e909-151">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="5e909-152">`SelectList` Gatunków jest tworzona przy wyświetlaniu gatunki distinct.</span><span class="sxs-lookup"><span data-stu-id="5e909-152">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="5e909-153">Dodaj wyszukiwanie według gatunku do strony Razor</span><span class="sxs-lookup"><span data-stu-id="5e909-153">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="5e909-154">Aktualizacja *Index.cshtml* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5e909-154">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="5e909-155">Testowanie aplikacji przez wyszukiwanie według gatunku, tytuł filmu i obu.</span><span class="sxs-lookup"><span data-stu-id="5e909-155">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e909-156">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5e909-156">Additional resources</span></span>

* [<span data-ttu-id="5e909-157">Wersja usługi YouTube w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="5e909-157">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="5e909-158">[Poprzednie: Aktualizowanie stron](xref:tutorials/razor-pages/da1)
> [dalej: Dodawanie nowego pola](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="5e909-158">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
