<span data-ttu-id="45241-101">Zaznaczony kod powyżej przedstawiono filmu kontekst bazy danych dodawane do [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera (w *Startup.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="45241-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container (In the *Startup.cs* file).</span></span> <span data-ttu-id="45241-102">`services.AddDbContext<MvcMovieContext>(options =>`Określa bazę danych i parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="45241-102">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span> <span data-ttu-id="45241-103">`=>`jest [operatora lambda](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="45241-103">`=>` is a [lambda operator](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="45241-104">Otwórz *Controllers/MoviesController.cs* pliku i sprawdź, czy konstruktora:</span><span class="sxs-lookup"><span data-stu-id="45241-104">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

<span data-ttu-id="45241-105">Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`MvcMovieContext `) z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="45241-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="45241-106">Kontekst bazy danych jest używany w każdym [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metod w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="45241-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="45241-107">Silnie typizowane modeli i @model — słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="45241-107">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="45241-108">Wcześniej w tym samouczku widać, jak kontroler może przekazać dane i obiekty do widoku przy użyciu `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="45241-108">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="45241-109">`ViewData` Słownik jest obiekt dynamiczny, które oferują wygodny sposób późnym wiązaniem do przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="45241-109">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="45241-110">MVC oferuje także możliwość przekazywania silnie typizowanych obiektów modelu do widoku.</span><span class="sxs-lookup"><span data-stu-id="45241-110">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="45241-111">To silnie typizowane podejście umożliwia lepsze kompilacji Sprawdzanie kodu.</span><span class="sxs-lookup"><span data-stu-id="45241-111">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="45241-112">Mechanizm szkieletów użyć tej metody, (tj. przekazywanie jednoznacznie modelu) z `MoviesController` klasy i widoki tworzona metod i widoków.</span><span class="sxs-lookup"><span data-stu-id="45241-112">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="45241-113">Sprawdź wygenerowany `Details` metody w *Controllers/MoviesController.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="45241-113">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="45241-114">`id` Parametr jest zwykle przekazywany jako dane trasy.</span><span class="sxs-lookup"><span data-stu-id="45241-114">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="45241-115">Na przykład `http://localhost:5000/movies/details/1` ustawia:</span><span class="sxs-lookup"><span data-stu-id="45241-115">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="45241-116">Kontroler do `movies` kontrolera (pierwszy segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="45241-116">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="45241-117">Akcja `details` (drugi segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="45241-117">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="45241-118">Identyfikator na wartość 1 (ostatni segment adresu URL).</span><span class="sxs-lookup"><span data-stu-id="45241-118">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="45241-119">Można również przekazać `id` za pomocą kwerendy ciągu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="45241-119">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="45241-120">`id` Parametr jest zdefiniowany jako [typ dopuszczający wartość null](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie podano wartość Identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="45241-120">The `id` parameter is defined as a [nullable type](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value is not provided.</span></span>

<span data-ttu-id="45241-121">A [wyrażenia lambda](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przekazywany do `SingleOrDefaultAsync` Wybierz film jednostek, które odpowiada wartości ciągu danych lub zapytanie trasy.</span><span class="sxs-lookup"><span data-stu-id="45241-121">A [lambda expression](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `SingleOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

<span data-ttu-id="45241-122">Jeśli zostanie znaleziony film, wystąpienie `Movie` modelu jest przekazywana do `Details` widoku:</span><span class="sxs-lookup"><span data-stu-id="45241-122">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="45241-123">Sprawdź zawartość *Views/Movies/Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="45241-123">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="45241-124">W tym `@model` instrukcji w górnej części pliku widoku, można określić typu obiektu, który oczekuje widoku.</span><span class="sxs-lookup"><span data-stu-id="45241-124">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="45241-125">Podczas tworzenia kontrolera film, Visual Studio automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="45241-125">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="45241-126">To `@model` dyrektywy umożliwia dostęp do filmów, który kontroler przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="45241-126">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="45241-127">Na przykład w *Details.cshtml* widoku kodu przekazuje każde pole film, aby `DisplayNameFor` i `DisplayFor` pomocników HTML z silnie typizowaną `Model` obiektu.</span><span class="sxs-lookup"><span data-stu-id="45241-127">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="45241-128">`Create` i `Edit` metody i widoki również przekazać `Movie` obiekt modelu.</span><span class="sxs-lookup"><span data-stu-id="45241-128">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="45241-129">Sprawdź *Index.cshtml* widoku i `Index` metody w kontrolerze filmów.</span><span class="sxs-lookup"><span data-stu-id="45241-129">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="45241-130">Zwróć uwagę, jak kod tworzy `List` obiektu, gdy wywołuje `View` metody.</span><span class="sxs-lookup"><span data-stu-id="45241-130">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="45241-131">Kod przekazuje to `Movies` z listy `Index` metody akcji w widoku:</span><span class="sxs-lookup"><span data-stu-id="45241-131">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="45241-132">Podczas tworzenia kontrolera filmów, tworzenia szkieletu automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="45241-132">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="45241-133">`@model` Dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="45241-133">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="45241-134">Na przykład w *Index.cshtml* wyświetlić kod pętlę filmów z `foreach` instrukcji na silnie typizowaną `Model` obiektu:</span><span class="sxs-lookup"><span data-stu-id="45241-134">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="45241-135">Ponieważ `Model` obiektu jest silnie typizowane (jako `IEnumerable<Movie>` obiektu), jest typu każdego elementu w pętli `Movie`.</span><span class="sxs-lookup"><span data-stu-id="45241-135">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="45241-136">Wśród innych korzyści, oznacza to, czy możesz uzyskać kompilacji Sprawdzanie kodu:</span><span class="sxs-lookup"><span data-stu-id="45241-136">Among other benefits, this means that you get compile-time checking of the code:</span></span>
