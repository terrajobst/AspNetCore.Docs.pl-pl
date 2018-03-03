## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="86128-101">Implementowanie inne operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="86128-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="86128-102">W poniższych sekcjach `Create`, `Update`, i `Delete` metody są dodawane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="86128-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="86128-103">Create</span><span class="sxs-lookup"><span data-stu-id="86128-103">Create</span></span>

<span data-ttu-id="86128-104">Dodaj następujące `Create` metody.</span><span class="sxs-lookup"><span data-stu-id="86128-104">Add the following `Create` method.</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="86128-105">Poprzedni kod jest metodą HTTP POST, wskazane przez [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="86128-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="86128-106">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="86128-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="86128-107">`CreatedAtRoute` Metody:</span><span class="sxs-lookup"><span data-stu-id="86128-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="86128-108">Zwraca odpowiedź 201.</span><span class="sxs-lookup"><span data-stu-id="86128-108">Returns a 201 response.</span></span> <span data-ttu-id="86128-109">201 protokołu HTTP jest standardowe odpowiedzi dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="86128-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="86128-110">Dodaje do odpowiedzi nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="86128-110">Adds a Location header to the response.</span></span> <span data-ttu-id="86128-111">Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="86128-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="86128-112">Zobacz [10.2.2 201 utworzony](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="86128-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="86128-113">Używa "GetTodo" o nazwie trasy w celu utworzenia adresu URL.</span><span class="sxs-lookup"><span data-stu-id="86128-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="86128-114">"GetTodo" o nazwie trasa jest zdefiniowana w `GetById`:</span><span class="sxs-lookup"><span data-stu-id="86128-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="86128-115">Umożliwia wysłanie żądanie utworzenia Postman</span><span class="sxs-lookup"><span data-stu-id="86128-115">Use Postman to send a Create request</span></span>

![Konsola postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="86128-117">Ustawia metodę HTTP `POST`</span><span class="sxs-lookup"><span data-stu-id="86128-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="86128-118">Wybierz **treści** przycisku radiowego</span><span class="sxs-lookup"><span data-stu-id="86128-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="86128-119">Wybierz **raw** przycisku radiowego</span><span class="sxs-lookup"><span data-stu-id="86128-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="86128-120">Ustaw typ do ciągu JSON</span><span class="sxs-lookup"><span data-stu-id="86128-120">Set the type to JSON</span></span>
* <span data-ttu-id="86128-121">W edytorze klucz wartość wprowadzać pozycji zadania, takie jak</span><span class="sxs-lookup"><span data-stu-id="86128-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="86128-122">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="86128-122">Select **Send**</span></span>
* <span data-ttu-id="86128-123">Wybierz kartę nagłówków w dolnym okienku i skopiuj **lokalizacji** nagłówka:</span><span class="sxs-lookup"><span data-stu-id="86128-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Karta nagłówki konsoli Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="86128-125">Nagłówek lokalizacji URI może służyć do dostępu do nowego elementu.</span><span class="sxs-lookup"><span data-stu-id="86128-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="86128-126">Aktualizacja</span><span class="sxs-lookup"><span data-stu-id="86128-126">Update</span></span>

<span data-ttu-id="86128-127">Dodaj następujące `Update` metody:</span><span class="sxs-lookup"><span data-stu-id="86128-127">Add the following `Update` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="86128-128">`Update` przypomina `Create`, ale używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="86128-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="86128-129">Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="86128-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="86128-130">Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice.</span><span class="sxs-lookup"><span data-stu-id="86128-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="86128-131">Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="86128-131">To support partial updates, use HTTP PATCH.</span></span>

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="86128-133">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="86128-133">Delete</span></span>

<span data-ttu-id="86128-134">Dodaj następujące `Delete` metody:</span><span class="sxs-lookup"><span data-stu-id="86128-134">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="86128-135">`Delete` Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="86128-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="86128-136">Test `Delete`:</span><span class="sxs-lookup"><span data-stu-id="86128-136">Test `Delete`:</span></span> 

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmd.png)
