# <a name="add-search-to-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="75dcf-101">Dodawanie wyszukiwania do aplikacji stron Razor programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75dcf-101">Add search to an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="75dcf-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="75dcf-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="75dcf-103">W tym dokumencie możliwości wyszukiwania jest dodawany do strony indeksu, który umożliwia wyszukiwanie filmów przez *gatunku* lub *nazwa*.</span><span class="sxs-lookup"><span data-stu-id="75dcf-103">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="75dcf-104">Aktualizuj stronę indeksu `OnGetAsync` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="75dcf-104">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="75dcf-105">W pierwszym wierszu `OnGetAsync` metoda tworzy [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) zapytanie, aby wybrać filmów:</span><span class="sxs-lookup"><span data-stu-id="75dcf-105">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="75dcf-106">Zapytanie jest *tylko* zdefiniowane w tym momencie, ma ona **nie** zostały uruchomione dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="75dcf-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="75dcf-107">Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania ciąg wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="75dcf-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="75dcf-108">`s => s.Title.Contains()` Kod jest [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="75dcf-108">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="75dcf-109">Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metody lub `Contains` (używane w poprzednim kodzie).</span><span class="sxs-lookup"><span data-stu-id="75dcf-109">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="75dcf-110">Zapytania LINQ nie są wykonywane, gdy są one definiowane lub ich modyfikacji przez wywołanie metody (takie jak `Where`, `Contains` lub `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="75dcf-110">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="75dcf-111">Przeciwnie wykonanie zapytania jest odroczone.</span><span class="sxs-lookup"><span data-stu-id="75dcf-111">Rather, query execution is deferred.</span></span> <span data-ttu-id="75dcf-112">Oznacza to, że obliczania wyrażenia została opóźniona do momentu jego rzeczywista wartość jest powtarzana lub `ToListAsync` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="75dcf-112">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="75dcf-113">Zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="75dcf-113">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="75dcf-114">**Uwaga:** [Zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda jest uruchomiona w bazie danych, nie w C# kodu.</span><span class="sxs-lookup"><span data-stu-id="75dcf-114">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="75dcf-115">Rozróżnianie wielkości liter w zapytaniu, zależy od bazy danych i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="75dcf-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="75dcf-116">W programie SQL Server `Contains` mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="75dcf-116">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="75dcf-117">W SQLite przy użyciu sortowania domyślnego, jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="75dcf-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="75dcf-118">Przejdź do strony filmy i Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL (na przykład `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="75dcf-118">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="75dcf-119">Wyświetlane są filtrowane filmów.</span><span class="sxs-lookup"><span data-stu-id="75dcf-119">The filtered movies are displayed.</span></span>

![Widok indeksu](../../tutorials/razor-pages/search/_static/ghost.png)

<span data-ttu-id="75dcf-121">Jeśli następujący szablon trasy zostanie dodany do strony indeksu, ciąg wyszukiwania mogą być przekazywane jako segment adresu URL (na przykład `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="75dcf-121">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="75dcf-122">Poprzedni ograniczenia trasy umożliwia wyszukiwanie tytuł, np. dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="75dcf-122">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="75dcf-123">`?` w `"{searchString?}"` oznacza, że jest to parametr opcjonalny trasy.</span><span class="sxs-lookup"><span data-stu-id="75dcf-123">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Widok indeksu z ghost word dodawany do adresu Url i zwrócony filmu listę filmów Ghostbusters i Ghostbusters 2](../../tutorials/razor-pages/search/_static/g2.png)

<span data-ttu-id="75dcf-125">Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, aby wyszukać film.</span><span class="sxs-lookup"><span data-stu-id="75dcf-125">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="75dcf-126">W tym kroku interfejs użytkownika jest dodawany do filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="75dcf-126">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="75dcf-127">Jeśli dodano ograniczenia trasy `"{searchString?}"`, usuń go.</span><span class="sxs-lookup"><span data-stu-id="75dcf-127">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="75dcf-128">Otwórz *Pages/Movies/Index.cshtml* pliku i Dodaj `<form>` znaczników wyróżnione w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="75dcf-128">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="75dcf-129">Kod HTML `<form>` tagów używa [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="75dcf-129">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="75dcf-130">Po przesłaniu formularza ciąg filtru są wysyłane do *stron/filmy/Index* strony.</span><span class="sxs-lookup"><span data-stu-id="75dcf-130">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="75dcf-131">Zapisz zmiany i przetestować filtr.</span><span class="sxs-lookup"><span data-stu-id="75dcf-131">Save the changes and test the filter.</span></span>

![Widok indeksu z ghost słowa wpisane w polu tekstowym filtru tytułu](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="75dcf-133">Wyszukaj według gatunku</span><span class="sxs-lookup"><span data-stu-id="75dcf-133">Search by genre</span></span>

<span data-ttu-id="75dcf-134">Dodaj następujące właściwości wyróżniony, aby *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="75dcf-134">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

<span data-ttu-id="75dcf-135">`SelectList Genres` Zawiera listę gatunki.</span><span class="sxs-lookup"><span data-stu-id="75dcf-135">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="75dcf-136">Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.</span><span class="sxs-lookup"><span data-stu-id="75dcf-136">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="75dcf-137">`MovieGenre` Właściwość zawiera gatunku określonych przez użytkownika (na przykład "zachodni").</span><span class="sxs-lookup"><span data-stu-id="75dcf-137">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="75dcf-138">Aktualizacja `OnGetAsync` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="75dcf-138">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="75dcf-139">Poniższy kod jest zapytanie LINQ, która pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="75dcf-139">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="75dcf-140">`SelectList` Gatunków jest tworzona przy wyświetlaniu gatunki distinct.</span><span class="sxs-lookup"><span data-stu-id="75dcf-140">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="75dcf-141">Dodawanie wyszukiwania według gatunku</span><span class="sxs-lookup"><span data-stu-id="75dcf-141">Adding search by genre</span></span>

<span data-ttu-id="75dcf-142">Aktualizacja *Index.cshtml* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="75dcf-142">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="75dcf-143">Testowanie aplikacji przez wyszukiwanie według gatunku, tytuł filmu i obu.</span><span class="sxs-lookup"><span data-stu-id="75dcf-143">Test the app by searching by genre, by movie title, and by both.</span></span>
