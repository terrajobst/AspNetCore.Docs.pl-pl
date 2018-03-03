<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="219bf-101">Teraz podczas przesyłania wyszukiwania adres URL zawiera ciąg zapytania wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="219bf-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="219bf-102">Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.</span><span class="sxs-lookup"><span data-stu-id="219bf-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Okno przeglądarki, przedstawiające parametru Wyszukiwany_ciąg = widma w adres Url i zwracany, filmy Ghostbusters i Ghostbusters 2 zawierają widma programu word](../../tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="219bf-104">Następujący kod przedstawia zmiany `form` tagu:</span><span class="sxs-lookup"><span data-stu-id="219bf-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="219bf-105">Dodawanie wyszukiwania według rodzaju</span><span class="sxs-lookup"><span data-stu-id="219bf-105">Adding Search by genre</span></span>

<span data-ttu-id="219bf-106">Dodaj następujące `MovieGenreViewModel` klasy do *modele* folderu:</span><span class="sxs-lookup"><span data-stu-id="219bf-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="219bf-107">Model widoku genre film będzie zawierać:</span><span class="sxs-lookup"><span data-stu-id="219bf-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="219bf-108">Lista filmów.</span><span class="sxs-lookup"><span data-stu-id="219bf-108">A list of movies.</span></span>
   * <span data-ttu-id="219bf-109">A `SelectList` zawierający listę gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="219bf-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="219bf-110">Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.</span><span class="sxs-lookup"><span data-stu-id="219bf-110">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="219bf-111">`movieGenre`, zawierającą wybranego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="219bf-111">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="219bf-112">Zastąp `Index` metoda `MoviesController.cs` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="219bf-112">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="219bf-113">Następujący kod jest `LINQ` kwerendę, która pobiera wszystkie genres z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="219bf-113">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="219bf-114">`SelectList` Gatunkami muzyki jest tworzony przez projekcji różne gatunki (nie chcemy naszej listy wyboru mają zduplikowane genres).</span><span class="sxs-lookup"><span data-stu-id="219bf-114">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="219bf-115">Dodawanie wyszukiwania według rodzaju do widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="219bf-115">Adding search by genre to the Index view</span></span>

<span data-ttu-id="219bf-116">Aktualizacja `Index.cshtml` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="219bf-116">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="219bf-117">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="219bf-117">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="219bf-118">W powyższym kodzie `DisplayNameFor` przeprowadzający pomocnika kodu HTML `Title` właściwości, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="219bf-118">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="219bf-119">Ponieważ wyrażenie lambda jest sprawdzana zamiast obliczone, nie otrzyma naruszenia zasad dostępu podczas `model`, `model.movies`, lub `model.movies[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="219bf-119">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="219bf-120">Podczas oceny wyrażenia lambda (na przykład `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="219bf-120">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="219bf-121">Aby przetestować aplikację, wyszukiwanie według rodzaju, tytuł filmu i obu.</span><span class="sxs-lookup"><span data-stu-id="219bf-121">Test the app by searching by genre, by movie title, and by both.</span></span>
