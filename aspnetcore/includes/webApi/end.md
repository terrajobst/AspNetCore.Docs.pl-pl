## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="33d58-101">Implementowanie inne operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="33d58-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="33d58-102">W poniższych sekcjach `Create`, `Update`, i `Delete` metody są dodawane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33d58-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="33d58-103">Create</span><span class="sxs-lookup"><span data-stu-id="33d58-103">Create</span></span>

<span data-ttu-id="33d58-104">Dodaj następujący kod `Create` metody:</span><span class="sxs-lookup"><span data-stu-id="33d58-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="33d58-105">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="33d58-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="33d58-106">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybut informuje MVC, aby uzyskać wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="33d58-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="33d58-107">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="33d58-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="33d58-108">MVC pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="33d58-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="33d58-109">`CreatedAtRoute` Metody:</span><span class="sxs-lookup"><span data-stu-id="33d58-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="33d58-110">Zwraca odpowiedź 201.</span><span class="sxs-lookup"><span data-stu-id="33d58-110">Returns a 201 response.</span></span> <span data-ttu-id="33d58-111">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="33d58-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="33d58-112">Dodaje do odpowiedzi nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="33d58-112">Adds a Location header to the response.</span></span> <span data-ttu-id="33d58-113">Nagłówek Location określa identyfikator URI nowo utworzonego zadania do wykonania.</span><span class="sxs-lookup"><span data-stu-id="33d58-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="33d58-114">Zobacz [10.2.2 201 utworzone](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="33d58-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="33d58-115">Używa "GetTodo" o nazwie tras w celu utworzenia adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33d58-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="33d58-116">"GetTodo" o nazwie trasa jest zdefiniowana w `GetById`:</span><span class="sxs-lookup"><span data-stu-id="33d58-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="33d58-117">Wyślij żądanie utworzenia przy użyciu narzędzia Postman</span><span class="sxs-lookup"><span data-stu-id="33d58-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="33d58-118">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="33d58-118">Start the app.</span></span>
* <span data-ttu-id="33d58-119">Otwórz narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="33d58-119">Open Postman.</span></span>

![Konsola narzędzia postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="33d58-121">Zaktualizuj numer portu w adresie URL localhost.</span><span class="sxs-lookup"><span data-stu-id="33d58-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="33d58-122">Ustawia metodę HTTP *WPIS*.</span><span class="sxs-lookup"><span data-stu-id="33d58-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="33d58-123">Kliknij przycisk **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="33d58-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="33d58-124">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="33d58-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="33d58-125">Ustaw typ *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="33d58-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="33d58-126">Wprowadź treść żądania z elementem zadań do wykonania przypominającą następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="33d58-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="33d58-127">Kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="33d58-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="33d58-128">Jeśli odpowiedź nie jest wyświetlany po kliknięciu przycisku **wysyłania**, wyłącz **weryfikacji certyfikacji SSL** opcji.</span><span class="sxs-lookup"><span data-stu-id="33d58-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="33d58-129">To znajduje się w folderze **pliku** > **ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="33d58-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="33d58-130">Kliknij przycisk **wysyłania** przycisk ponownie po wyłączeniu ustawienia.</span><span class="sxs-lookup"><span data-stu-id="33d58-130">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="33d58-131">Kliknij przycisk **nagłówki** karcie **odpowiedzi** okienka i skopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="33d58-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Karta nagłówki konsoli narzędzia Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="33d58-133">Nagłówek Location identyfikator URI może służyć do uzyskania dostępu do nowego elementu.</span><span class="sxs-lookup"><span data-stu-id="33d58-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="33d58-134">Aktualizacja</span><span class="sxs-lookup"><span data-stu-id="33d58-134">Update</span></span>

<span data-ttu-id="33d58-135">Dodaj następujący kod `Update` metody:</span><span class="sxs-lookup"><span data-stu-id="33d58-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="33d58-136">`Update` jest podobny do `Create`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="33d58-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="33d58-137">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="33d58-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="33d58-138">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko różnice.</span><span class="sxs-lookup"><span data-stu-id="33d58-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="33d58-139">Aby obsługiwać aktualizacje częściowe, należy użyć HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="33d58-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="33d58-140">Zaktualizuj nazwę elementu do wykonania "przeprowadzi cat" przy użyciu narzędzia Postman:</span><span class="sxs-lookup"><span data-stu-id="33d58-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="33d58-142">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="33d58-142">Delete</span></span>

<span data-ttu-id="33d58-143">Dodaj następujący kod `Delete` metody:</span><span class="sxs-lookup"><span data-stu-id="33d58-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="33d58-144">`Delete` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="33d58-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="33d58-145">Użyj narzędzia Postman, aby usunąć element zadania do wykonania:</span><span class="sxs-lookup"><span data-stu-id="33d58-145">Use Postman to delete the to-do item:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](../../tutorials/first-web-api/_static/pmd.png)
