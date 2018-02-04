# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="4bb2b-101">Dodawanie do aplikacji stron Razor wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="4bb2b-101">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="4bb2b-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4bb2b-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4bb2b-103">W tym dokumencie możliwości wyszukiwania jest dodawany do strony indeksu, która umożliwia wyszukiwanie filmów przez *genre* lub *nazwa*.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-103">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="4bb2b-104">Zaktualizuj strony indeksu `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4bb2b-104">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="4bb2b-105">W pierwszym wierszu `OnGetAsync` metoda tworzy [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) zapytanie, aby wybrać filmy:</span><span class="sxs-lookup"><span data-stu-id="4bb2b-105">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="4bb2b-106">Zapytanie jest *tylko* zdefiniowane w tym momencie, ma **nie** zostały uruchomione w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="4bb2b-107">Jeśli `searchString` parametru zawiera ciąg, filmy zapytania są modyfikowane w celu filtrowania ciąg wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="4bb2b-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="4bb2b-108">`s => s.Title.Contains()` Kod [wyrażenia Lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="4bb2b-108">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="4bb2b-109">Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) wysyła zapytanie jako argumenty do metod operator standardowej kwerendy, takich jak [gdzie](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metody lub `Contains` (używane w powyższym kodzie).</span><span class="sxs-lookup"><span data-stu-id="4bb2b-109">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="4bb2b-110">Podczas definiowania lub są one zmienione przez wywołanie metody nie zostaną wykonane zapytań LINQ (takich jak `Where`, `Contains` lub `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="4bb2b-110">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="4bb2b-111">Zamiast wykonywania zapytania została odroczona.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-111">Rather, query execution is deferred.</span></span> <span data-ttu-id="4bb2b-112">Oznacza to, że obliczania wyrażenia jest opóźnione aż do jej wartość rzeczywista jest iterowane za pośrednictwem lub `ToListAsync` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-112">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="4bb2b-113">Zobacz [wykonywania zapytania](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-113">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="4bb2b-114">**Uwaga:** [zawiera](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metody jest uruchamiane na bazie danych, nie znajduje się w kodzie języka C#.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-114">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="4bb2b-115">Uwzględniana wielkość liter w zapytaniu jest zależna od bazy danych i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="4bb2b-116">W programie SQL Server `Contains` mapuje [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-116">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="4bb2b-117">W SQLite z sortowania domyślnego, jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="4bb2b-118">Przejdź do strony filmy i Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL (na przykład `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="4bb2b-118">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="4bb2b-119">Filtrowane filmów są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-119">The filtered movies are displayed.</span></span>

![Widok indeksu](../../tutorials/razor-pages/search/_static/ghost.png)

<span data-ttu-id="4bb2b-121">Jeśli następujący szablon trasy zostanie dodany do strony indeksu, ciąg wyszukiwania mogą być przekazywane jako segment adresu URL (na przykład `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="4bb2b-121">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="4bb2b-122">Poprzednie ograniczenie trasy umożliwia wyszukiwanie tytuł jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-122">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="4bb2b-123">`?` w `"{searchString?}"` oznacza, że jest to parametr opcjonalny trasy.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-123">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Widok indeksu z widma word dodane do adresu Url i filmu zwrócona lista filmów Ghostbusters i Ghostbusters 2](../../tutorials/razor-pages/search/_static/g2.png)

<span data-ttu-id="4bb2b-125">Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, aby wyszukać filmu.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-125">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="4bb2b-126">W tym kroku interfejsu użytkownika jest dodawany do filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-126">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="4bb2b-127">Jeśli dodano ograniczenia trasy `"{searchString?}"`, usuń go.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-127">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="4bb2b-128">Otwórz *Pages/Movies/Index.cshtml* pliku, a następnie dodaj `<form>` wyróżnione następujący kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="4bb2b-128">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="4bb2b-129">Kod HTML `<form>` tagów używa [pomocnika Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="4bb2b-129">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="4bb2b-130">Po przesłaniu formularza ciąg filtru jest wysyłana do *stron/filmów/indeksu* strony.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-130">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="4bb2b-131">Zapisz zmiany i przetestować filtr.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-131">Save the changes and test the filter.</span></span>

![Widok indeksu z widma word wpisane w polu tekstowym filtru tytułu](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="4bb2b-133">Wyszukiwanie według rodzaju</span><span class="sxs-lookup"><span data-stu-id="4bb2b-133">Search by genre</span></span>

<span data-ttu-id="4bb2b-134">Dodaj następujące wyróżnione właściwości *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="4bb2b-134">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

<span data-ttu-id="4bb2b-135">`SelectList Genres` Zawiera listę gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-135">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="4bb2b-136">Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-136">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="4bb2b-137">`MovieGenre` Właściwość zawiera określonego rodzaju wybiera użytkownika (na przykład "zachodni").</span><span class="sxs-lookup"><span data-stu-id="4bb2b-137">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="4bb2b-138">Aktualizacja `OnGetAsync` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4bb2b-138">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="4bb2b-139">Następujący kod jest zapytania LINQ, która pobiera wszystkie genres z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-139">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="4bb2b-140">`SelectList` Gatunkami muzyki jest tworzony przez różne genres projekcji.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-140">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="4bb2b-141">Dodawanie wyszukiwania według rodzaju</span><span class="sxs-lookup"><span data-stu-id="4bb2b-141">Adding search by genre</span></span>

<span data-ttu-id="4bb2b-142">Aktualizacja *Index.cshtml* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4bb2b-142">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="4bb2b-143">Aby przetestować aplikację, wyszukiwanie według rodzaju, tytuł filmu i obu.</span><span class="sxs-lookup"><span data-stu-id="4bb2b-143">Test the app by searching by genre, by movie title, and by both.</span></span>
