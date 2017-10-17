<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="33edd-101">Teraz podczas przesyłania wyszukiwania adres URL zawiera ciąg zapytania wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="33edd-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="33edd-102">Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.</span><span class="sxs-lookup"><span data-stu-id="33edd-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Okno przeglądarki, przedstawiające parametru Wyszukiwany_ciąg = widma w adres Url i zwracany, filmy Ghostbusters i Ghostbusters 2 zawierają widma programu word](../../tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="33edd-104">Następujący kod przedstawia zmiany `form` tagu:</span><span class="sxs-lookup"><span data-stu-id="33edd-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="33edd-105">Dodawanie wyszukiwania według rodzaju</span><span class="sxs-lookup"><span data-stu-id="33edd-105">Adding Search by genre</span></span>

<span data-ttu-id="33edd-106">Dodaj następujące `MovieGenreViewModel` klasy do *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="33edd-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

<span data-ttu-id="33edd-107">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="33edd-107">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]</span></span>

<span data-ttu-id="33edd-108">Model widoku genre film będzie zawierać:</span><span class="sxs-lookup"><span data-stu-id="33edd-108">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="33edd-109">Lista filmów.</span><span class="sxs-lookup"><span data-stu-id="33edd-109">A list of movies.</span></span>
   * <span data-ttu-id="33edd-110">A `SelectList` zawierający listę gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="33edd-110">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="33edd-111">Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.</span><span class="sxs-lookup"><span data-stu-id="33edd-111">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="33edd-112">`movieGenre`, zawierającą wybranego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="33edd-112">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="33edd-113">Zastąp `Index` metoda `MoviesController.cs` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="33edd-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

<span data-ttu-id="33edd-114">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]</span><span class="sxs-lookup"><span data-stu-id="33edd-114">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]</span></span>

<span data-ttu-id="33edd-115">Następujący kod jest `LINQ` kwerendę, która pobiera wszystkie genres z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="33edd-115">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

<span data-ttu-id="33edd-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]</span><span class="sxs-lookup"><span data-stu-id="33edd-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]</span></span>

<span data-ttu-id="33edd-117">`SelectList` Gatunkami muzyki jest tworzony przez projekcji różne gatunki (nie chcemy naszej listy wyboru mają zduplikowane genres).</span><span class="sxs-lookup"><span data-stu-id="33edd-117">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="33edd-118">Dodawanie wyszukiwania według rodzaju do widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="33edd-118">Adding search by genre to the Index view</span></span>

<span data-ttu-id="33edd-119">Aktualizacja `Index.cshtml` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="33edd-119">Update `Index.cshtml` as follows:</span></span>

<span data-ttu-id="33edd-120">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]</span><span class="sxs-lookup"><span data-stu-id="33edd-120">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]</span></span>

<span data-ttu-id="33edd-121">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="33edd-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="33edd-122">W powyższym kodzie `DisplayNameFor` przeprowadzający pomocnika kodu HTML `Title` właściwości, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="33edd-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="33edd-123">Ponieważ wyrażenie lambda jest sprawdzana zamiast obliczone, nie otrzyma naruszenia zasad dostępu podczas `model`, `model.movies`, lub `model.movies[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="33edd-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="33edd-124">Podczas oceny wyrażenia lambda (na przykład `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="33edd-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="33edd-125">Aby przetestować aplikację, wyszukiwanie według rodzaju, tytuł filmu i obu.</span><span class="sxs-lookup"><span data-stu-id="33edd-125">Test the app by searching by genre, by movie title, and by both.</span></span>
