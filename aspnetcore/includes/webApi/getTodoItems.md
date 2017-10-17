<span data-ttu-id="71623-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="71623-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="71623-102">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="71623-102">The preceding code:</span></span>

* <span data-ttu-id="71623-103">Definiuje klasę pusty kontroler.</span><span class="sxs-lookup"><span data-stu-id="71623-103">Defines an empty controller class.</span></span> <span data-ttu-id="71623-104">W kolejnych sekcjach dodamy metody służące do implementacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="71623-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="71623-105">Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`TodoContext `) z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="71623-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="71623-106">Kontekst bazy danych jest używany w każdym [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metod w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="71623-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="71623-107">Konstruktor dodaje element do bazy danych w pamięci, jeśli jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="71623-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="71623-108">Pobieranie elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="71623-108">Getting to-do items</span></span>

<span data-ttu-id="71623-109">Aby uzyskać elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy.</span><span class="sxs-lookup"><span data-stu-id="71623-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="71623-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="71623-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="71623-111">Te metody zaimplementować te dwie metody GET.</span><span class="sxs-lookup"><span data-stu-id="71623-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="71623-112">Oto przykład HTTP odpowiedzi dla `GetAll` metody:</span><span class="sxs-lookup"><span data-stu-id="71623-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="71623-113">W dalszej części tego samouczka I opisano sposób można wyświetlić przy użyciu odpowiedzi HTTP [Postman](https://www.getpostman.com/) lub [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="71623-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="71623-114">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="71623-114">Routing and URL paths</span></span>

<span data-ttu-id="71623-115">`[HttpGet]` Atrybut określa metody HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="71623-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="71623-116">Ścieżka adresu URL dla każdej metody jest tworzony w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="71623-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="71623-117">Wykonaj ciąg szablonu w atrybucie trasy kontrolera:</span><span class="sxs-lookup"><span data-stu-id="71623-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="71623-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="71623-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="71623-119">Zamień na nazwę kontrolera, który jest nazwa klasy kontrolera minus sufiks "Controller" "[kontrolera]".</span><span class="sxs-lookup"><span data-stu-id="71623-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="71623-120">Ten przykład jest nazwa klasy kontrolera **Todo**kontrolera, a nazwa katalogu głównego to "todo".</span><span class="sxs-lookup"><span data-stu-id="71623-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="71623-121">Platformy ASP.NET Core [routingu](xref:mvc/controllers/routing) nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="71623-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="71623-122">Jeśli `[HttpGet]` atrybut ma szablon trasy (takich jak `[HttpGet("/products")]`, które dołącza do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="71623-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="71623-123">W tym przykładzie nie używać szablonu.</span><span class="sxs-lookup"><span data-stu-id="71623-123">This sample doesn't use a template.</span></span> <span data-ttu-id="71623-124">Zobacz [atrybutu routingu z atrybutami Http [zlecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="71623-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="71623-125">W `GetById` metody:</span><span class="sxs-lookup"><span data-stu-id="71623-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="71623-126">`"{id}"`to symbol zastępczy zmienna identyfikatora `todo` elementu.</span><span class="sxs-lookup"><span data-stu-id="71623-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="71623-127">Gdy `GetById` jest wywołane, przypisuje wartość "{id}" w adresie URL do metody `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="71623-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="71623-128">`Name = "GetTodo"`tworzy trasę nazwane i umożliwia łącze do tej trasy w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="71623-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="71623-129">Będzie opisano go z przykładem później.</span><span class="sxs-lookup"><span data-stu-id="71623-129">I'll explain it with an example later.</span></span> <span data-ttu-id="71623-130">Zobacz [routingu do akcji kontrolera](xref:mvc/controllers/routing) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="71623-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="71623-131">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="71623-131">Return values</span></span>

<span data-ttu-id="71623-132">`GetAll` Metoda zwraca `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="71623-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="71623-133">MVC automatycznie serializuje obiekt do [JSON](http://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="71623-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="71623-134">Kod odpowiedzi dla tej metody, 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="71623-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="71623-135">(Nieobsługiwane wyjątki są przekształcane na błędy 5xx).</span><span class="sxs-lookup"><span data-stu-id="71623-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="71623-136">Z kolei `GetById` metoda zwraca więcej ogólnych `IActionResult` typu, który przedstawia szeroką gamę zwracanych typów.</span><span class="sxs-lookup"><span data-stu-id="71623-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="71623-137">`GetById`ma dwa różne typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="71623-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="71623-138">Jeśli element nie jest zgodny z Identyfikatorem żądanego, metoda zwraca błąd 404.</span><span class="sxs-lookup"><span data-stu-id="71623-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="71623-139">Polega to na zwracanie `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="71623-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="71623-140">W przeciwnym razie metoda zwraca 200 z treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="71623-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="71623-141">Polega to na zwracanie`ObjectResult`</span><span class="sxs-lookup"><span data-stu-id="71623-141">This is done by returning an `ObjectResult`</span></span>
