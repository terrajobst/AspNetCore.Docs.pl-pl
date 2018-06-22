## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="a0f5f-101">Implementowanie inne operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="a0f5f-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="a0f5f-102">W poniższych sekcjach `Create`, `Update`, i `Delete` metody są dodawane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="a0f5f-103">Create</span><span class="sxs-lookup"><span data-stu-id="a0f5f-103">Create</span></span>

<span data-ttu-id="a0f5f-104">Dodaj następujące `Create` metody:</span><span class="sxs-lookup"><span data-stu-id="a0f5f-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a0f5f-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="a0f5f-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="a0f5f-106">Poprzedni kod jest metodą HTTP POST, wskazywany przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a0f5f-107">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-107">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a0f5f-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="a0f5f-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="a0f5f-109">Poprzedni kod jest metodą HTTP POST, wskazywany przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a0f5f-110">MVC pobiera wartość elementu zadań do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="a0f5f-111">`CreatedAtRoute` Metody:</span><span class="sxs-lookup"><span data-stu-id="a0f5f-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="a0f5f-112">Zwraca odpowiedź 201.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-112">Returns a 201 response.</span></span> <span data-ttu-id="a0f5f-113">201 protokołu HTTP jest standardowe odpowiedzi dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="a0f5f-114">Dodaje do odpowiedzi nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-114">Adds a Location header to the response.</span></span> <span data-ttu-id="a0f5f-115">Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="a0f5f-116">Zobacz [10.2.2 201 utworzony](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="a0f5f-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="a0f5f-117">Używa "GetTodo" o nazwie trasy w celu utworzenia adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="a0f5f-118">"GetTodo" o nazwie trasa jest zdefiniowana w `GetById`:</span><span class="sxs-lookup"><span data-stu-id="a0f5f-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a0f5f-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="a0f5f-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a0f5f-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="a0f5f-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="a0f5f-121">Umożliwia wysłanie żądanie utworzenia Postman</span><span class="sxs-lookup"><span data-stu-id="a0f5f-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="a0f5f-122">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-122">Start the app.</span></span>
* <span data-ttu-id="a0f5f-123">Otwórz Postman.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-123">Open Postman.</span></span>

![Konsola postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="a0f5f-125">Zaktualizuj numer portu w adresie URL localhost.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="a0f5f-126">Ustawia metodę HTTP *POST*.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="a0f5f-127">Kliknij przycisk **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="a0f5f-128">Wybierz **raw** przycisk radiowy.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="a0f5f-129">Ustaw typ *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="a0f5f-130">Wprowadź treść żądania z zadanie do wykonania podobne do następujących JSON:</span><span class="sxs-lookup"><span data-stu-id="a0f5f-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="a0f5f-131">Kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="a0f5f-132">Jeśli odpowiedź nie są wyświetlane po kliknięciu przycisku **wysyłania**, wyłącz **weryfikacji certyfikacji SSL** opcji.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="a0f5f-133">To znajduje się w **pliku** > **ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="a0f5f-134">Kliknij przycisk **wysyłania** przycisk ponownie po wyłączeniu ustawienia.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="a0f5f-135">Kliknij przycisk **nagłówki** karcie **odpowiedzi** okienko i skopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="a0f5f-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Karta nagłówki konsoli Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="a0f5f-137">Nagłówek lokalizacji URI może służyć do dostępu do nowego elementu.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="a0f5f-138">Aktualizacja</span><span class="sxs-lookup"><span data-stu-id="a0f5f-138">Update</span></span>

<span data-ttu-id="a0f5f-139">Dodaj następujące `Update` metody:</span><span class="sxs-lookup"><span data-stu-id="a0f5f-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a0f5f-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="a0f5f-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a0f5f-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="a0f5f-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="a0f5f-142">`Update` przypomina `Create`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="a0f5f-143">Odpowiedź jest [204 (bez zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a0f5f-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="a0f5f-144">Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="a0f5f-145">Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="a0f5f-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="a0f5f-146">Użyj Postman, aby zaktualizować element zadania do wykonania nazwa przeprowadzenie "kot":</span><span class="sxs-lookup"><span data-stu-id="a0f5f-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="a0f5f-148">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="a0f5f-148">Delete</span></span>

<span data-ttu-id="a0f5f-149">Dodaj następujące `Delete` metody:</span><span class="sxs-lookup"><span data-stu-id="a0f5f-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="a0f5f-150">`Delete` Odpowiedź jest [204 (bez zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a0f5f-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="a0f5f-151">Użyj Postman, aby usunąć element zadania do wykonania:</span><span class="sxs-lookup"><span data-stu-id="a0f5f-151">Use Postman to delete the to-do item:</span></span>

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmd.png)
