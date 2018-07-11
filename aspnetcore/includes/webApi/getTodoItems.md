::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="5e111-101">Powyższy kod definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="5e111-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="5e111-102">W kolejnych sekcjach metody są dodawane do implementacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5e111-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="5e111-103">Powyższy kod definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="5e111-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="5e111-104">W kolejnych sekcjach metody są dodawane do implementacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5e111-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="5e111-105">Klasa jest oznaczona za pomocą `[ApiController]` atrybutu, aby włączyć funkcje wygodne.</span><span class="sxs-lookup"><span data-stu-id="5e111-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="5e111-106">Aby uzyskać informacji na temat funkcji włączone za pomocą atrybutu, zobacz [dodawać adnotacje do klasy za pomocą ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="5e111-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="5e111-107">Używa kontrolera Konstruktor [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5e111-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="5e111-108">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="5e111-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="5e111-109">Konstruktor dodaje element do bazy danych w pamięci, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="5e111-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="5e111-110">Pobierz elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="5e111-110">Get to-do items</span></span>

<span data-ttu-id="5e111-111">Aby uzyskać elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="5e111-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="5e111-112">Te metody zaimplementować te dwie metody GET.</span><span class="sxs-lookup"><span data-stu-id="5e111-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="5e111-113">Poniżej przedstawiono przykład odpowiedzi HTTP dla `GetAll` metody:</span><span class="sxs-lookup"><span data-stu-id="5e111-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="5e111-114">W dalszej części tego samouczka I pokazano, jak mogą być wyświetlane odpowiedzi HTTP z [Postman](https://www.getpostman.com/) lub [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="5e111-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="5e111-115">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="5e111-115">Routing and URL paths</span></span>

<span data-ttu-id="5e111-116">`[HttpGet]` Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="5e111-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="5e111-117">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5e111-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="5e111-118">Wykonaj ciąg szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="5e111-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="5e111-119">Zastąp `[controller]` nazwę kontrolera, który jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="5e111-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="5e111-120">W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera i nazwę katalogu głównego jest "todo".</span><span class="sxs-lookup"><span data-stu-id="5e111-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="5e111-121">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5e111-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="5e111-122">Jeśli `[HttpGet]` atrybut ma szablon trasy (takie jak `[HttpGet("/products")]`, dołączania, do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="5e111-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="5e111-123">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="5e111-123">This sample doesn't use a template.</span></span> <span data-ttu-id="5e111-124">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="5e111-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="5e111-125">W następującym `GetById` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="5e111-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="5e111-126">Gdy `GetById` jest wywołana, przypisuje wartość `"{id}"` w adresie URL do metody `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="5e111-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="5e111-127">`Name = "GetTodo"` tworzy trasę o nazwie.</span><span class="sxs-lookup"><span data-stu-id="5e111-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="5e111-128">Nazwane trasy:</span><span class="sxs-lookup"><span data-stu-id="5e111-128">Named routes:</span></span>

* <span data-ttu-id="5e111-129">Korzystanie z aplikacji utworzyć link HTTP przy użyciu nazwy trasy.</span><span class="sxs-lookup"><span data-stu-id="5e111-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="5e111-130">Zostały omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5e111-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="5e111-131">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="5e111-131">Return values</span></span>

<span data-ttu-id="5e111-132">`GetAll` Metoda zwraca kolekcję `TodoItem` obiektów.</span><span class="sxs-lookup"><span data-stu-id="5e111-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="5e111-133">MVC automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5e111-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="5e111-134">Kod odpowiedzi dla tej metody jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="5e111-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="5e111-135">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="5e111-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="5e111-136">Z kolei `GetById` metoda zwraca bardziej ogólnej [typu IActionResult](xref:web-api/action-return-types#iactionresult-type), która reprezentuje szeroką gamę zwracanych typów.</span><span class="sxs-lookup"><span data-stu-id="5e111-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="5e111-137">`GetById` ma dwa różne typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="5e111-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="5e111-138">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="5e111-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="5e111-139">Zwracanie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zwraca komunikat odpowiedzi HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="5e111-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="5e111-140">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="5e111-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="5e111-141">Zwracanie [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="5e111-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="5e111-142">Z kolei `GetById` metoda zwraca [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type), która reprezentuje szeroką gamę zwracanych typów.</span><span class="sxs-lookup"><span data-stu-id="5e111-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="5e111-143">`GetById` ma dwa różne typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="5e111-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="5e111-144">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="5e111-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="5e111-145">Zwracanie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zwraca komunikat odpowiedzi HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="5e111-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="5e111-146">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="5e111-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="5e111-147">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="5e111-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
