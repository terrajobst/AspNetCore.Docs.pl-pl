---
title: Dodawanie do platformy ASP.NET Core Razor strony wyszukiwania
author: rick-anderson
description: "Przedstawiono sposób dodawania wyszukiwania do platformy ASP.NET Core Razor stron"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/search
ms.openlocfilehash: f66d83a94505a74f7e0f6b7ae74ff2d408e36478
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="0f03e-103">Dodawanie do aplikacji stron Razor wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="0f03e-103">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="0f03e-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0f03e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0f03e-105">W tym dokumencie możliwości wyszukiwania jest dodawany do strony indeksu, która umożliwia wyszukiwanie filmów przez *genre* lub *nazwa*.</span><span class="sxs-lookup"><span data-stu-id="0f03e-105">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="0f03e-106">Zaktualizuj strony indeksu `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0f03e-106">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="0f03e-107">W pierwszym wierszu `OnGetAsync` metoda tworzy [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) zapytanie, aby wybrać filmy:</span><span class="sxs-lookup"><span data-stu-id="0f03e-107">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="0f03e-108">Zapytanie jest *tylko* zdefiniowane w tym momencie, ma **nie** zostały uruchomione w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f03e-108">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="0f03e-109">Jeśli `searchString` parametru zawiera ciąg, filmy zapytania są modyfikowane w celu filtrowania ciąg wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="0f03e-109">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="0f03e-110">`s => s.Title.Contains()` Kod [wyrażenia Lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="0f03e-110">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="0f03e-111">Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) wysyła zapytanie jako argumenty do metod operator standardowej kwerendy, takich jak [gdzie](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metody lub `Contains` (używane w powyższym kodzie).</span><span class="sxs-lookup"><span data-stu-id="0f03e-111">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="0f03e-112">Podczas definiowania lub są one zmienione przez wywołanie metody nie zostaną wykonane zapytań LINQ (takich jak `Where`, `Contains` lub `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="0f03e-112">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="0f03e-113">Zamiast wykonywania zapytania została odroczona.</span><span class="sxs-lookup"><span data-stu-id="0f03e-113">Rather, query execution is deferred.</span></span> <span data-ttu-id="0f03e-114">Oznacza to, że obliczania wyrażenia jest opóźnione aż do jej wartość rzeczywista jest iterowane za pośrednictwem lub `ToListAsync` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0f03e-114">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="0f03e-115">Zobacz [wykonywania zapytania](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="0f03e-115">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="0f03e-116">**Uwaga:** [zawiera](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metody jest uruchamiane na bazie danych, nie znajduje się w kodzie języka C#.</span><span class="sxs-lookup"><span data-stu-id="0f03e-116">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="0f03e-117">Uwzględniana wielkość liter w zapytaniu jest zależna od bazy danych i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="0f03e-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="0f03e-118">W programie SQL Server `Contains` mapuje [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="0f03e-118">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="0f03e-119">W SQLite z sortowania domyślnego, jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="0f03e-119">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="0f03e-120">Przejdź do strony filmy i Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL (na przykład `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="0f03e-120">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="0f03e-121">Filtrowane filmów są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="0f03e-121">The filtered movies are displayed.</span></span>

![Widok indeksu](search/_static/ghost.png)

<span data-ttu-id="0f03e-123">Jeśli następujący szablon trasy zostanie dodany do strony indeksu, ciąg wyszukiwania mogą być przekazywane jako segment adresu URL (na przykład `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="0f03e-123">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="0f03e-124">Poprzednie ograniczenie trasy umożliwia wyszukiwanie tytuł jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="0f03e-124">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="0f03e-125">`?` w `"{searchString?}"` oznacza, że jest to parametr opcjonalny trasy.</span><span class="sxs-lookup"><span data-stu-id="0f03e-125">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Widok indeksu z widma word dodane do adresu Url i filmu zwrócona lista filmów Ghostbusters i Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="0f03e-127">Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, aby wyszukać filmu.</span><span class="sxs-lookup"><span data-stu-id="0f03e-127">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="0f03e-128">W tym kroku interfejsu użytkownika jest dodawany do filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="0f03e-128">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="0f03e-129">Jeśli dodano ograniczenia trasy `"{searchString?}"`, usuń go.</span><span class="sxs-lookup"><span data-stu-id="0f03e-129">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="0f03e-130">Otwórz *Pages/Movies/Index.cshtml* pliku, a następnie dodaj `<form>` wyróżnione następujący kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="0f03e-130">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="0f03e-131">Kod HTML `<form>` tagów używa [pomocnika Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0f03e-131">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="0f03e-132">Po przesłaniu formularza ciąg filtru jest wysyłana do *stron/filmów/indeksu* strony.</span><span class="sxs-lookup"><span data-stu-id="0f03e-132">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="0f03e-133">Zapisz zmiany i przetestować filtr.</span><span class="sxs-lookup"><span data-stu-id="0f03e-133">Save the changes and test the filter.</span></span>

![Widok indeksu z widma word wpisane w polu tekstowym filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="0f03e-135">Wyszukiwanie według rodzaju</span><span class="sxs-lookup"><span data-stu-id="0f03e-135">Search by genre</span></span>

<span data-ttu-id="0f03e-136">Dodaj następujące właściwości wyróżnione *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0f03e-136">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

<span data-ttu-id="0f03e-137">`SelectList Genres` Zawiera listę gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="0f03e-137">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="0f03e-138">Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.</span><span class="sxs-lookup"><span data-stu-id="0f03e-138">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="0f03e-139">`MovieGenre` Właściwość zawiera określonego rodzaju wybiera użytkownika (na przykład "zachodni").</span><span class="sxs-lookup"><span data-stu-id="0f03e-139">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="0f03e-140">Aktualizacja `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0f03e-140">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="0f03e-141">Następujący kod jest zapytania LINQ, która pobiera wszystkie genres z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0f03e-141">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="0f03e-142">`SelectList` Gatunkami muzyki jest tworzony przez różne genres projekcji.</span><span class="sxs-lookup"><span data-stu-id="0f03e-142">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="0f03e-143">Dodawanie wyszukiwania według rodzaju</span><span class="sxs-lookup"><span data-stu-id="0f03e-143">Adding search by genre</span></span>

<span data-ttu-id="0f03e-144">Aktualizacja *Index.cshtml* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0f03e-144">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="0f03e-145">Aby przetestować aplikację, wyszukiwanie według rodzaju, tytuł filmu i obu.</span><span class="sxs-lookup"><span data-stu-id="0f03e-145">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0f03e-146">[Poprzedni: Aktualizowanie stron](xref:tutorials/razor-pages/da1)
[dalej: dodanie nowego pola](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="0f03e-146">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
