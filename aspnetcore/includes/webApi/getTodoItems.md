[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="b2663-101">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="b2663-101">The preceding code:</span></span>

* <span data-ttu-id="b2663-102">Definiuje klasę pusty kontroler.</span><span class="sxs-lookup"><span data-stu-id="b2663-102">Defines an empty controller class.</span></span> <span data-ttu-id="b2663-103">W kolejnych sekcjach metody są dodawane do zaimplementowania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b2663-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="b2663-104">Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`TodoContext `) z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="b2663-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="b2663-105">Kontekst bazy danych jest używany w każdym [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metod w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="b2663-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="b2663-106">Konstruktor dodaje element do bazy danych w pamięci, jeśli jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="b2663-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="b2663-107">Pobieranie elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="b2663-107">Getting to-do items</span></span>

<span data-ttu-id="b2663-108">Aby uzyskać elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy.</span><span class="sxs-lookup"><span data-stu-id="b2663-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="b2663-109">Te metody zaimplementować te dwie metody GET.</span><span class="sxs-lookup"><span data-stu-id="b2663-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="b2663-110">Oto przykład HTTP odpowiedzi dla `GetAll` metody:</span><span class="sxs-lookup"><span data-stu-id="b2663-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="b2663-111">W dalszej części tego samouczka I opisano sposób można wyświetlić odpowiedzi HTTP z [Postman](https://www.getpostman.com/) lub [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="b2663-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="b2663-112">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="b2663-112">Routing and URL paths</span></span>

<span data-ttu-id="b2663-113">`[HttpGet]` Atrybut określa metody HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="b2663-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="b2663-114">Ścieżka adresu URL dla każdej metody jest tworzony w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b2663-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="b2663-115">Wykonaj ciąg szablonu w kontrolera `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="b2663-115">Take the template string in the controller's `Route` attribute:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="b2663-116">Zastępuje `[controller]` nazwę kontrolera, który jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="b2663-116">Replaces `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="b2663-117">Ten przykład jest nazwa klasy kontrolera **Todo**kontrolera, a nazwa katalogu głównego to "todo".</span><span class="sxs-lookup"><span data-stu-id="b2663-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="b2663-118">Platformy ASP.NET Core [routingu](xref:mvc/controllers/routing) nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b2663-118">ASP.NET Core [routing](xref:mvc/controllers/routing) isn't case sensitive.</span></span>
* <span data-ttu-id="b2663-119">Jeśli `[HttpGet]` atrybut ma szablon trasy (takich jak `[HttpGet("/products")]`, które dołącza do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="b2663-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="b2663-120">W tym przykładzie nie używać szablonu.</span><span class="sxs-lookup"><span data-stu-id="b2663-120">This sample doesn't use a template.</span></span> <span data-ttu-id="b2663-121">Zobacz [atrybutu routingu z atrybutami Http [zlecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b2663-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="b2663-122">W `GetById` metody:</span><span class="sxs-lookup"><span data-stu-id="b2663-122">In the `GetById` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="b2663-123">`"{id}"` to symbol zastępczy zmienna identyfikatora `todo` elementu.</span><span class="sxs-lookup"><span data-stu-id="b2663-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="b2663-124">Gdy `GetById` jest wywołane, przypisuje wartość "{id}" w adresie URL do metody `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="b2663-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="b2663-125">`Name = "GetTodo"` Tworzy nazwanej trasy.</span><span class="sxs-lookup"><span data-stu-id="b2663-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="b2663-126">Trasy o nazwie:</span><span class="sxs-lookup"><span data-stu-id="b2663-126">Named routes:</span></span>

* <span data-ttu-id="b2663-127">Włączanie aplikacji do utworzenia łącza HTTP, używając nazwy trasy.</span><span class="sxs-lookup"><span data-stu-id="b2663-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="b2663-128">Opisano szczegółowo w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="b2663-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="b2663-129">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="b2663-129">Return values</span></span>

<span data-ttu-id="b2663-130">`GetAll` Metoda zwraca `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="b2663-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="b2663-131">MVC automatycznie serializuje obiekt do [JSON](http://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b2663-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="b2663-132">Kod odpowiedzi dla tej metody, 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="b2663-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="b2663-133">(Nieobsługiwane wyjątki są przekształcane na błędy 5xx).</span><span class="sxs-lookup"><span data-stu-id="b2663-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="b2663-134">Z kolei `GetById` metoda zwraca więcej ogólnych `IActionResult` typu, który przedstawia szeroką gamę zwracanych typów.</span><span class="sxs-lookup"><span data-stu-id="b2663-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="b2663-135">`GetById` ma dwa różne typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="b2663-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="b2663-136">Jeśli element nie jest zgodny z Identyfikatorem żądanego, metoda zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="b2663-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="b2663-137">Zwracanie `NotFound` zwraca odpowiedź HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="b2663-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="b2663-138">W przeciwnym razie metoda zwraca 200 z treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="b2663-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="b2663-139">Zwracanie `ObjectResult` zwraca odpowiedź 200 protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b2663-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
