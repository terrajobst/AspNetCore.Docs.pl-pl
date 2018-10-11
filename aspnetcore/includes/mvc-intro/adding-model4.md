<span data-ttu-id="a5221-101">Wyróżniony kod powyżej przedstawiono kontekst bazy danych filmów, które są dodawane do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera (w *Startup.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="a5221-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container (In the *Startup.cs* file).</span></span> <span data-ttu-id="a5221-102">`services.AddDbContext<MvcMovieContext>(options =>` Określa bazę danych i parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="a5221-102">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span> <span data-ttu-id="a5221-103">`=>` jest [operatora lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="a5221-103">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="a5221-104">Otwórz *Controllers/MoviesController.cs* plików i zbadaj konstruktora:</span><span class="sxs-lookup"><span data-stu-id="a5221-104">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

<span data-ttu-id="a5221-105">Używa konstruktora [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`MvcMovieContext `) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5221-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="a5221-106">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="a5221-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="a5221-107">Silnie typizowane modeli i @model — słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="a5221-107">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="a5221-108">Wcześniej w tym samouczku pokazano, jak kontroler można przekazać dane i obiekty za pomocą widoku `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="a5221-108">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="a5221-109">`ViewData` Słownik jest to obiekt dynamiczny, która zapewnia wygodny sposób z późnym wiązaniem do przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="a5221-109">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="a5221-110">MVC udostępnia również możliwość przekazywania silnie typizowanych obiektów modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="a5221-110">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="a5221-111">Silnie typizowane to podejście umożliwia lepsze kompilacji sprawdzania kodu.</span><span class="sxs-lookup"><span data-stu-id="a5221-111">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="a5221-112">Mechanizm tworzenia szkieletów używane takie podejście (który jest przekazując silnie typizowany model) z `MoviesController` klasy i widoki utworzenia metod i widoków.</span><span class="sxs-lookup"><span data-stu-id="a5221-112">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="a5221-113">Sprawdź wygenerowany `Details` method in Class metoda *Controllers/MoviesController.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="a5221-113">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end


<span data-ttu-id="a5221-114">`id` Parametr ogólnie jest przekazywany jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="a5221-114">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="a5221-115">Na przykład `http://localhost:5000/movies/details/1` ustawia:</span><span class="sxs-lookup"><span data-stu-id="a5221-115">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="a5221-116">Kontroler do `movies` kontrolera (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="a5221-116">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="a5221-117">Działanie `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="a5221-117">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="a5221-118">Identyfikator do 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="a5221-118">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="a5221-119">Możesz również przekazać `id` przy użyciu zapytania następujący ciąg:</span><span class="sxs-lookup"><span data-stu-id="a5221-119">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="a5221-120">`id` Parametr jest zdefiniowany jako [typu dopuszczającego wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie jest podana wartość Identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="a5221-120">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>



::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a5221-121">A [wyrażenia lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przekazywany do `FirstOrDefaultAsync` do wybrania jednostki filmu, które odpowiada wartości ciągu danych lub zapytanie trasy.</span><span class="sxs-lookup"><span data-stu-id="a5221-121">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.ID == id);
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="a5221-122">A [wyrażenia lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przekazywany do `SingleOrDefaultAsync` do wybrania jednostki filmu, które odpowiada wartości ciągu danych lub zapytanie trasy.</span><span class="sxs-lookup"><span data-stu-id="a5221-122">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `SingleOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

::: moniker-end



<span data-ttu-id="a5221-123">Jeśli film zostanie znaleziony, wystąpienie `Movie` modelu jest przekazywany do `Details` widoku:</span><span class="sxs-lookup"><span data-stu-id="a5221-123">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="a5221-124">Sprawdź zawartość *Views/Movies/Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="a5221-124">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="a5221-125">Jeśli dołączysz `@model` instrukcji w górnej części pliku widoku, można określić typu obiektu, który oczekuje, że widok.</span><span class="sxs-lookup"><span data-stu-id="a5221-125">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="a5221-126">Podczas tworzenia kontrolera filmu programu Visual Studio automatycznie uwzględnione następujące `@model` instrukcji na górze *Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="a5221-126">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="a5221-127">To `@model` dyrektywy umożliwia dostęp do filmów, która kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="a5221-127">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="a5221-128">Na przykład w *Details.cshtml* widoku Kod przekazuje każdego pola film, aby `DisplayNameFor` i `DisplayFor` pomocników HTML za pomocą silnie typizowanej `Model` obiektu.</span><span class="sxs-lookup"><span data-stu-id="a5221-128">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="a5221-129">`Create` i `Edit` metody i widoki również przekazać `Movie` obiekt modelu.</span><span class="sxs-lookup"><span data-stu-id="a5221-129">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="a5221-130">Sprawdź *Index.cshtml* widoku i `Index` metody w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="a5221-130">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="a5221-131">Zwróć uwagę, jak kod tworzy `List` obiektu, kiedy wywoływanych przez nią `View` metody.</span><span class="sxs-lookup"><span data-stu-id="a5221-131">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="a5221-132">Kod przekazuje to `Movies` listy z `Index` metody akcji do widoku:</span><span class="sxs-lookup"><span data-stu-id="a5221-132">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="a5221-133">Podczas tworzenia kontrolera filmy tworzenia szkieletów automatycznie uwzględnione następujące `@model` instrukcji na górze *Index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="a5221-133">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="a5221-134">`@model` Dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="a5221-134">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="a5221-135">Na przykład w *Index.cshtml* wyświetlić kod pętlę filmów z `foreach` instrukcji na silnie typizowaną `Model` obiektu:</span><span class="sxs-lookup"><span data-stu-id="a5221-135">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="a5221-136">Ponieważ `Model` obiektu zdecydowanie jest wpisane (jako `IEnumerable<Movie>` obiektu), każdy element w pętli jest wpisana jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a5221-136">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="a5221-137">Wśród innych korzyści, oznacza to, że uzyskujesz kompilacji sprawdzania kodu:</span><span class="sxs-lookup"><span data-stu-id="a5221-137">Among other benefits, this means that you get compile-time checking of the code:</span></span>
