## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="b3da5-101">Implementowanie inne operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="b3da5-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="b3da5-102">Dodamy `Create`, `Update`, i `Delete` metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b3da5-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="b3da5-103">Odmiany motywu, są tak właśnie będzie I wyświetlić kod i zaznacz główne różnice.</span><span class="sxs-lookup"><span data-stu-id="b3da5-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="b3da5-104">Po dodaniu lub zmiana kodu, skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="b3da5-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="b3da5-105">Create</span><span class="sxs-lookup"><span data-stu-id="b3da5-105">Create</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="b3da5-106">Jest to metoda HTTP POST, wskazane przez [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b3da5-106">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b3da5-107">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3da5-107">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="b3da5-108">`CreatedAtRoute` Metoda zwraca odpowiedź 201 standardowa odpowiedź metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b3da5-108">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="b3da5-109">`CreatedAtRoute`również dodaje do odpowiedzi nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b3da5-109">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="b3da5-110">Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b3da5-110">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="b3da5-111">Zobacz [10.2.2 201 utworzony](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="b3da5-111">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="b3da5-112">Umożliwia wysłanie żądanie utworzenia Postman</span><span class="sxs-lookup"><span data-stu-id="b3da5-112">Use Postman to send a Create request</span></span>

![Konsola postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="b3da5-114">Ustawia metodę HTTP`POST`</span><span class="sxs-lookup"><span data-stu-id="b3da5-114">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="b3da5-115">Wybierz **treści** przycisku radiowego</span><span class="sxs-lookup"><span data-stu-id="b3da5-115">Select the **Body** radio button</span></span>
* <span data-ttu-id="b3da5-116">Wybierz **raw** przycisku radiowego</span><span class="sxs-lookup"><span data-stu-id="b3da5-116">Select the **raw** radio button</span></span>
* <span data-ttu-id="b3da5-117">Ustaw typ do ciągu JSON</span><span class="sxs-lookup"><span data-stu-id="b3da5-117">Set the type to JSON</span></span>
* <span data-ttu-id="b3da5-118">W edytorze klucz wartość wprowadzać pozycji zadania, takie jak</span><span class="sxs-lookup"><span data-stu-id="b3da5-118">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="b3da5-119">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="b3da5-119">Select **Send**</span></span>

* <span data-ttu-id="b3da5-120">Wybierz kartę nagłówków w dolnym okienku i skopiuj **lokalizacji** nagłówka:</span><span class="sxs-lookup"><span data-stu-id="b3da5-120">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Karta nagłówki konsoli Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="b3da5-122">Nagłówek lokalizacji URI umożliwia dostęp do utworzonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="b3da5-122">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="b3da5-123">Odwołaj `GetById` metody utworzony `"GetTodo"` o nazwie trasy:</span><span class="sxs-lookup"><span data-stu-id="b3da5-123">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="b3da5-124">Aktualizacja</span><span class="sxs-lookup"><span data-stu-id="b3da5-124">Update</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="b3da5-125">`Update`przypomina `Create`, ale używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="b3da5-125">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="b3da5-126">Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b3da5-126">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="b3da5-127">Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice.</span><span class="sxs-lookup"><span data-stu-id="b3da5-127">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="b3da5-128">Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="b3da5-128">To support partial updates, use HTTP PATCH.</span></span>

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="b3da5-130">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="b3da5-130">Delete</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="b3da5-131">Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b3da5-131">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmd.png)
