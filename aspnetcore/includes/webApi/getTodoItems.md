::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="25396-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="25396-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="25396-102">Poprzedni kod definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="25396-102">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="25396-103">W kolejnych sekcjach metody są dodawane do zaimplementowania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="25396-103">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="25396-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="25396-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="25396-105">Poprzedni kod definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="25396-105">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="25396-106">W kolejnych sekcjach metody są dodawane do zaimplementowania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="25396-106">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="25396-107">Klasa jest oznaczony za pomocą `[ApiController]` atrybutu, aby włączyć funkcje wygodne.</span><span class="sxs-lookup"><span data-stu-id="25396-107">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="25396-108">Informacji o funkcjach, włączone przez atrybut, zobacz [adnotacji klasy z ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="25396-108">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="25396-109">Używa kontrolera konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`TodoContext`) z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="25396-109">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="25396-110">Kontekst bazy danych jest używany w każdym [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metod w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="25396-110">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="25396-111">Konstruktor dodaje element do bazy danych w pamięci, jeśli jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="25396-111">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="25396-112">Pobierz elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="25396-112">Get to-do items</span></span>

<span data-ttu-id="25396-113">Aby uzyskać elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="25396-113">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="25396-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="25396-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="25396-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="25396-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end

<span data-ttu-id="25396-116">Te metody zaimplementować te dwie metody GET.</span><span class="sxs-lookup"><span data-stu-id="25396-116">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="25396-117">Oto przykładowe odpowiedzi HTTP dla `GetAll` metody:</span><span class="sxs-lookup"><span data-stu-id="25396-117">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="25396-118">W dalszej części samouczka I opisano sposób można wyświetlić odpowiedzi HTTP z [Postman](https://www.getpostman.com/) lub [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="25396-118">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="25396-119">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="25396-119">Routing and URL paths</span></span>

<span data-ttu-id="25396-120">`[HttpGet]` Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="25396-120">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="25396-121">Ścieżka adresu URL dla każdej metody jest tworzony w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="25396-121">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="25396-122">Wykonaj ciąg szablonu w kontrolera `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="25396-122">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="25396-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="25396-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="25396-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="25396-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end

* <span data-ttu-id="25396-125">Zastąp `[controller]` nazwę kontrolera, który jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="25396-125">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="25396-126">Ten przykład jest nazwa klasy kontrolera **Todo**kontrolera, a nazwa katalogu głównego to "todo".</span><span class="sxs-lookup"><span data-stu-id="25396-126">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="25396-127">Platformy ASP.NET Core [routingu](xref:mvc/controllers/routing) nie uwzględnia wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="25396-127">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="25396-128">Jeśli `[HttpGet]` atrybut ma szablon trasy (takich jak `[HttpGet("/products")]`, które dołącza do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="25396-128">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="25396-129">W tym przykładzie nie używać szablonu.</span><span class="sxs-lookup"><span data-stu-id="25396-129">This sample doesn't use a template.</span></span> <span data-ttu-id="25396-130">Aby uzyskać więcej informacji, zobacz [atrybutu routingu z atrybutami Http [zlecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="25396-130">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="25396-131">W następujących `GetById` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="25396-131">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="25396-132">Gdy `GetById` jest wywołane, przypisuje wartość `"{id}"` w adresie URL do metody `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="25396-132">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="25396-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="25396-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="25396-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="25396-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

<span data-ttu-id="25396-135">`Name = "GetTodo"` Tworzy nazwanej trasy.</span><span class="sxs-lookup"><span data-stu-id="25396-135">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="25396-136">Trasy o nazwie:</span><span class="sxs-lookup"><span data-stu-id="25396-136">Named routes:</span></span>

* <span data-ttu-id="25396-137">Włączanie aplikacji do utworzenia łącza HTTP, używając nazwy trasy.</span><span class="sxs-lookup"><span data-stu-id="25396-137">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="25396-138">Opisano szczegółowo w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="25396-138">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="25396-139">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="25396-139">Return values</span></span>

<span data-ttu-id="25396-140">`GetAll` Metoda zwraca kolekcję `TodoItem` obiektów.</span><span class="sxs-lookup"><span data-stu-id="25396-140">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="25396-141">MVC automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="25396-141">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="25396-142">Kod odpowiedzi dla tej metody, 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="25396-142">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="25396-143">Nieobsługiwane wyjątki są przetłumaczyć 5xx błędy.</span><span class="sxs-lookup"><span data-stu-id="25396-143">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="25396-144">Z kolei `GetById` metoda zwraca więcej ogólnych [typu IActionResult](xref:web-api/action-return-types#iactionresult-type), który przedstawia szeroką gamę zwracane typy.</span><span class="sxs-lookup"><span data-stu-id="25396-144">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="25396-145">`GetById` ma dwa różne typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="25396-145">`GetById` has two different return types:</span></span>

* <span data-ttu-id="25396-146">Jeśli element nie jest zgodny z Identyfikatorem żądanego, metoda zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="25396-146">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="25396-147">Zwracanie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zwraca odpowiedź HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="25396-147">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="25396-148">W przeciwnym razie metoda zwraca 200 z treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="25396-148">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="25396-149">Zwracanie [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) powoduje odpowiedź 200 protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="25396-149">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="25396-150">Z kolei `GetById` metoda zwraca [ActionResult\<T > typ](xref:web-api/action-return-types#actionresultt-type), który przedstawia szeroką gamę zwracane typy.</span><span class="sxs-lookup"><span data-stu-id="25396-150">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="25396-151">`GetById` ma dwa różne typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="25396-151">`GetById` has two different return types:</span></span>

* <span data-ttu-id="25396-152">Jeśli element nie jest zgodny z Identyfikatorem żądanego, metoda zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="25396-152">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="25396-153">Zwracanie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zwraca odpowiedź HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="25396-153">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="25396-154">W przeciwnym razie metoda zwraca 200 z treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="25396-154">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="25396-155">Zwracanie `item` powoduje odpowiedź 200 protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="25396-155">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end