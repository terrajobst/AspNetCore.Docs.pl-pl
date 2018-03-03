---
title: "Tworzenie składnika Web API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac"
author: rick-anderson
description: "Tworzenie składnika Web API platformy ASP.NET Core MVC i programu Visual Studio dla komputerów Mac"
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: c7825530146e5e4a879bf44db5a92bc7700de73b
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="2d154-103">Tworzenie składnika Web API platformy ASP.NET Core MVC i programu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2d154-103">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="2d154-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="2d154-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="2d154-105">W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="2d154-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="2d154-106">Nie jest zbudowana w Interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2d154-106">The UI isn't constructed.</span></span>

<span data-ttu-id="2d154-107">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="2d154-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2d154-108">System macOS: składnika Web API z programem Visual Studio dla komputerów Mac (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="2d154-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="2d154-109">System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="2d154-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="2d154-110">System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="2d154-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="2d154-111">Zobacz [wprowadzenie do platformy ASP.NET MVC rdzeni na Mac lub Linux](xref:tutorials/first-mvc-app-xplat/index) przykład korzystającego z trwałe bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d154-111">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d154-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2d154-112">Prerequisites</span></span>

<span data-ttu-id="2d154-113">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2d154-113">Install the following:</span></span>

- <span data-ttu-id="2d154-114">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="2d154-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="2d154-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2d154-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="2d154-116">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="2d154-116">Create the project</span></span>

<span data-ttu-id="2d154-117">W programie Visual Studio, wybierz **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="2d154-117">From Visual Studio, select **File > New Solution**.</span></span>

![System macOS nowego rozwiązania](first-web-api-mac/_static/sln.png)

<span data-ttu-id="2d154-119">Wybierz **aplikacji .NET Core > interfejsu API sieci Web platformy ASP.NET Core > Dalej**.</span><span class="sxs-lookup"><span data-stu-id="2d154-119">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![okno dialogowe macOS nowego projektu](first-web-api-mac/_static/1.png)

<span data-ttu-id="2d154-121">Wprowadź **TodoApi** dla **Nazwa projektu**, a następnie wybierz przycisk Utwórz.</span><span class="sxs-lookup"><span data-stu-id="2d154-121">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="2d154-123">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="2d154-123">Launch the app</span></span>

<span data-ttu-id="2d154-124">W programie Visual Studio, wybierz **Uruchom > Rozpocznij z debugowanie** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2d154-124">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="2d154-125">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2d154-125">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="2d154-126">Błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="2d154-126">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="2d154-127">Zmień adres URL do `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="2d154-127">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="2d154-128">`ValuesController` Zostaną wyświetlone dane:</span><span class="sxs-lookup"><span data-stu-id="2d154-128">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="2d154-129">Dodaj obsługę Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2d154-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="2d154-130">Zainstaluj [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d154-130">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="2d154-131">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="2d154-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="2d154-132">Z **projektu** menu, wybierz opcję **Dodawanie pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2d154-132">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="2d154-133">Alternatywnie możesz kliknąć prawym przyciskiem myszy **zależności**, a następnie wybierz **Dodawanie pakietów**.</span><span class="sxs-lookup"><span data-stu-id="2d154-133">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="2d154-134">Wprowadź `EntityFrameworkCore.InMemory` w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="2d154-134">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="2d154-135">Wybierz `Microsoft.EntityFrameworkCore.InMemory`, a następnie wybierz **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="2d154-135">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="2d154-136">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="2d154-136">Add a model class</span></span>

<span data-ttu-id="2d154-137">Model jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2d154-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="2d154-138">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="2d154-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="2d154-139">Dodaj folder o nazwie *modele*.</span><span class="sxs-lookup"><span data-stu-id="2d154-139">Add a folder named *Models*.</span></span> <span data-ttu-id="2d154-140">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="2d154-140">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="2d154-141">Wybierz **dodać** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="2d154-141">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="2d154-142">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="2d154-142">Name the folder *Models*.</span></span>

![Nowy folder](first-web-api-mac/_static/folder.png)

<span data-ttu-id="2d154-144">Uwaga: Możesz też zaznaczyć klasy modelu dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="2d154-144">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="2d154-145">Dodaj `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="2d154-145">Add a `TodoItem` class.</span></span> <span data-ttu-id="2d154-146">Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj > Nowy plik > Ogólne > pustą klasę**.</span><span class="sxs-lookup"><span data-stu-id="2d154-146">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="2d154-147">Nazwa klasy `TodoItem`, a następnie wybierz **nowy**.</span><span class="sxs-lookup"><span data-stu-id="2d154-147">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="2d154-148">Zamień z wygenerowanego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d154-148">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="2d154-149">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="2d154-149">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="2d154-150">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d154-150">Create the database context</span></span>

<span data-ttu-id="2d154-151">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="2d154-151">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="2d154-152">Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="2d154-152">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="2d154-153">Dodaj `TodoContext` klasy do *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="2d154-153">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="2d154-154">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="2d154-154">Add a controller</span></span>

<span data-ttu-id="2d154-155">W Eksploratorze rozwiązań w *kontrolerów* folderu, Dodaj klasę `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="2d154-155">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="2d154-156">Zamień na wygenerowany kod poniżej (i dodać klamrowe nawiasy zamykające):</span><span class="sxs-lookup"><span data-stu-id="2d154-156">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="2d154-157">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="2d154-157">Launch the app</span></span>

<span data-ttu-id="2d154-158">W programie Visual Studio, wybierz **Uruchom > Rozpocznij z debugowanie** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2d154-158">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="2d154-159">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="2d154-159">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="2d154-160">Błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="2d154-160">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="2d154-161">Zmień adres URL do `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="2d154-161">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="2d154-162">`ValuesController` Zostaną wyświetlone dane:</span><span class="sxs-lookup"><span data-stu-id="2d154-162">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="2d154-163">Przejdź do `Todo` kontroler na`http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="2d154-163">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="2d154-164">Implementowanie inne operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="2d154-164">Implement the other CRUD operations</span></span>

<span data-ttu-id="2d154-165">Dodamy `Create`, `Update`, i `Delete` metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="2d154-165">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="2d154-166">Odmiany motywu, są tak właśnie będzie I wyświetlić kod i zaznacz główne różnice.</span><span class="sxs-lookup"><span data-stu-id="2d154-166">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="2d154-167">Po dodaniu lub zmiana kodu, skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2d154-167">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="2d154-168">Create</span><span class="sxs-lookup"><span data-stu-id="2d154-168">Create</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="2d154-169">Jest to metoda HTTP POST, wskazane przez [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2d154-169">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="2d154-170">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="2d154-170">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="2d154-171">`CreatedAtRoute` Metoda zwraca odpowiedź 201 standardowa odpowiedź metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2d154-171">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="2d154-172">`CreatedAtRoute` również dodaje do odpowiedzi nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="2d154-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="2d154-173">Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="2d154-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="2d154-174">Zobacz [10.2.2 201 utworzony](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="2d154-174">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="2d154-175">Umożliwia wysłanie żądanie utworzenia Postman</span><span class="sxs-lookup"><span data-stu-id="2d154-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="2d154-176">Uruchom aplikację (**Uruchom > Start z debugowaniem**).</span><span class="sxs-lookup"><span data-stu-id="2d154-176">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="2d154-177">Uruchom Postman.</span><span class="sxs-lookup"><span data-stu-id="2d154-177">Start Postman.</span></span>

![Konsola postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="2d154-179">Ustawia metodę HTTP `POST`</span><span class="sxs-lookup"><span data-stu-id="2d154-179">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="2d154-180">Wybierz **treści** przycisku radiowego</span><span class="sxs-lookup"><span data-stu-id="2d154-180">Select the **Body** radio button</span></span>
* <span data-ttu-id="2d154-181">Wybierz **raw** przycisku radiowego</span><span class="sxs-lookup"><span data-stu-id="2d154-181">Select the **raw** radio button</span></span>
* <span data-ttu-id="2d154-182">Ustaw typ do ciągu JSON</span><span class="sxs-lookup"><span data-stu-id="2d154-182">Set the type to JSON</span></span>
* <span data-ttu-id="2d154-183">W edytorze klucz wartość wprowadzać pozycji zadania, takie jak</span><span class="sxs-lookup"><span data-stu-id="2d154-183">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="2d154-184">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="2d154-184">Select **Send**</span></span>

* <span data-ttu-id="2d154-185">Wybierz kartę nagłówków w dolnym okienku i skopiuj **lokalizacji** nagłówka:</span><span class="sxs-lookup"><span data-stu-id="2d154-185">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Karta nagłówki konsoli Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="2d154-187">Nagłówek lokalizacji URI umożliwia dostęp do utworzonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="2d154-187">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="2d154-188">Odwołaj `GetById` metody utworzony `"GetTodo"` o nazwie trasy:</span><span class="sxs-lookup"><span data-stu-id="2d154-188">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="2d154-189">Aktualizacja</span><span class="sxs-lookup"><span data-stu-id="2d154-189">Update</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="2d154-190">`Update` przypomina `Create`, ale używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="2d154-190">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="2d154-191">Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="2d154-191">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="2d154-192">Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice.</span><span class="sxs-lookup"><span data-stu-id="2d154-192">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="2d154-193">Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="2d154-193">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="2d154-195">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="2d154-195">Delete</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="2d154-196">Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="2d154-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="2d154-198">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2d154-198">Next steps</span></span>

* [<span data-ttu-id="2d154-199">Routing do akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="2d154-199">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="2d154-200">Aby uzyskać informacje dotyczące wdrażania interfejsu API, zobacz [hosta i wdrażanie](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="2d154-200">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="2d154-201">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2d154-201">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="2d154-202">Postman</span><span class="sxs-lookup"><span data-stu-id="2d154-202">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="2d154-203">Fiddler</span><span class="sxs-lookup"><span data-stu-id="2d154-203">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
