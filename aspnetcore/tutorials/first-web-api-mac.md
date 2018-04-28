---
title: Tworzenie składnika Web API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Tworzenie składnika Web API platformy ASP.NET Core MVC i programu Visual Studio dla komputerów Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 9fca43e809720c22d87b963925bc009dc6c51831
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="b03ee-103">Tworzenie składnika Web API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="b03ee-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="b03ee-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="b03ee-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="b03ee-105">W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="b03ee-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="b03ee-106">Nie jest zbudowana w Interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b03ee-106">The UI isn't constructed.</span></span>

<span data-ttu-id="b03ee-107">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="b03ee-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="b03ee-108">System macOS: składnika Web API z programem Visual Studio dla komputerów Mac (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="b03ee-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="b03ee-109">System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="b03ee-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="b03ee-110">System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="b03ee-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="b03ee-111">Zobacz [wprowadzenie do platformy ASP.NET MVC rdzeni na macOS lub Linux](xref:tutorials/first-mvc-app-xplat/index) przykład korzystającego z trwałe bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b03ee-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b03ee-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b03ee-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="b03ee-113">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="b03ee-113">Create the project</span></span>

<span data-ttu-id="b03ee-114">W programie Visual Studio, wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![System macOS nowego rozwiązania](first-web-api-mac/_static/sln.png)

<span data-ttu-id="b03ee-116">Wybierz **aplikacji .NET Core** > **interfejsu API sieci Web platformy ASP.NET Core** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![okno dialogowe macOS nowego projektu](first-web-api-mac/_static/1.png)

<span data-ttu-id="b03ee-118">Wprowadź *TodoApi* dla **Nazwa projektu**, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="b03ee-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="b03ee-120">Launch the app</span></span>

<span data-ttu-id="b03ee-121">W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b03ee-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="b03ee-122">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b03ee-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="b03ee-123">Błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="b03ee-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="b03ee-124">Zmień adres URL do `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="b03ee-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="b03ee-125">`ValuesController` Dane są wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="b03ee-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="b03ee-126">Dodaj obsługę Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b03ee-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="b03ee-127">Zainstaluj [Entity Framework Core InMemory](/ef/core/providers/in-memory/) dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b03ee-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="b03ee-128">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b03ee-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="b03ee-129">Z **projektu** menu, wybierz opcję **Dodawanie pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="b03ee-130">Alternatywnie możesz kliknąć prawym przyciskiem myszy **zależności**, a następnie wybierz **Dodawanie pakietów**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="b03ee-131">Wprowadź `EntityFrameworkCore.InMemory` w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="b03ee-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="b03ee-132">Wybierz `Microsoft.EntityFrameworkCore.InMemory`, a następnie wybierz **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="b03ee-133">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="b03ee-133">Add a model class</span></span>

<span data-ttu-id="b03ee-134">Model jest obiekt reprezentujący dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b03ee-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="b03ee-135">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b03ee-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="b03ee-136">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="b03ee-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="b03ee-137">Wybierz **dodać** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="b03ee-138">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="b03ee-138">Name the folder *Models*.</span></span>

![Nowy folder](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="b03ee-140">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="b03ee-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="b03ee-141">Kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj** > **nowy plik** > **ogólne**  >  **Pusta klasa**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="b03ee-142">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **nowy**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="b03ee-143">Zamień z wygenerowanego kodu:</span><span class="sxs-lookup"><span data-stu-id="b03ee-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="b03ee-144">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="b03ee-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="b03ee-145">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="b03ee-145">Create the database context</span></span>

<span data-ttu-id="b03ee-146">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="b03ee-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="b03ee-147">Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="b03ee-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="b03ee-148">Dodaj `TodoContext` klasy do *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="b03ee-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="b03ee-149">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="b03ee-149">Add a controller</span></span>

<span data-ttu-id="b03ee-150">W Eksploratorze rozwiązań w *kontrolerów* folderu, Dodaj klasę `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="b03ee-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="b03ee-151">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="b03ee-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="b03ee-152">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="b03ee-152">Launch the app</span></span>

<span data-ttu-id="b03ee-153">W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b03ee-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="b03ee-154">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:<port>`, gdzie `<port>` jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="b03ee-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="b03ee-155">Błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="b03ee-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="b03ee-156">Zmień adres URL do `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="b03ee-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="b03ee-157">`ValuesController` Dane są wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="b03ee-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="b03ee-158">Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="b03ee-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="b03ee-159">Implementowanie inne operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="b03ee-159">Implement the other CRUD operations</span></span>

<span data-ttu-id="b03ee-160">Dodamy `Create`, `Update`, i `Delete` metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b03ee-160">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="b03ee-161">Te metody są odmiany motywu, więc będzie tylko wyświetlić kod i zaznacz główne różnice.</span><span class="sxs-lookup"><span data-stu-id="b03ee-161">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="b03ee-162">Po dodaniu lub zmiana kodu, skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="b03ee-162">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="b03ee-163">Create</span><span class="sxs-lookup"><span data-stu-id="b03ee-163">Create</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="b03ee-164">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="b03ee-164">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="b03ee-165">Powyższa metoda odpowiada HTTP POST wskazywany przez [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b03ee-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b03ee-166">[[FromBody]](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b03ee-166">The [[FromBody]](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="b03ee-167">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="b03ee-167">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="b03ee-168">Powyższa metoda odpowiada HTTP POST wskazywany przez [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b03ee-168">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b03ee-169">MVC pobiera wartość elementu zadań do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b03ee-169">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="b03ee-170">`CreatedAtRoute` Metoda zwraca odpowiedź 201.</span><span class="sxs-lookup"><span data-stu-id="b03ee-170">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="b03ee-171">Jest standardowe odpowiedzi dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b03ee-171">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="b03ee-172">`CreatedAtRoute` również dodaje do odpowiedzi nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b03ee-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="b03ee-173">Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b03ee-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="b03ee-174">Zobacz [10.2.2 201 utworzony](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="b03ee-174">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="b03ee-175">Umożliwia wysłanie żądanie utworzenia Postman</span><span class="sxs-lookup"><span data-stu-id="b03ee-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="b03ee-176">Uruchom aplikację (**Uruchom** > **rozpoczynać debugowania**).</span><span class="sxs-lookup"><span data-stu-id="b03ee-176">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="b03ee-177">Otwórz Postman.</span><span class="sxs-lookup"><span data-stu-id="b03ee-177">Open Postman.</span></span>

![Konsola postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="b03ee-179">Zaktualizuj numer portu w adresie URL localhost.</span><span class="sxs-lookup"><span data-stu-id="b03ee-179">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="b03ee-180">Ustawia metodę HTTP *POST*.</span><span class="sxs-lookup"><span data-stu-id="b03ee-180">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="b03ee-181">Kliknij przycisk **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="b03ee-181">Click the **Body** tab.</span></span>
* <span data-ttu-id="b03ee-182">Wybierz **raw** przycisk radiowy.</span><span class="sxs-lookup"><span data-stu-id="b03ee-182">Select the **raw** radio button.</span></span>
* <span data-ttu-id="b03ee-183">Ustaw typ *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="b03ee-183">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="b03ee-184">Wprowadź treść żądania z zadanie do wykonania podobne do następujących JSON:</span><span class="sxs-lookup"><span data-stu-id="b03ee-184">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="b03ee-185">Kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b03ee-185">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="b03ee-186">Jeśli odpowiedź nie są wyświetlane po kliknięciu przycisku **wysyłania**, wyłącz **weryfikacji certyfikacji SSL** opcji.</span><span class="sxs-lookup"><span data-stu-id="b03ee-186">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="b03ee-187">To znajduje się w **pliku** > **ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="b03ee-187">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="b03ee-188">Kliknij przycisk **wysyłania** przycisk ponownie po wyłączeniu ustawienia.</span><span class="sxs-lookup"><span data-stu-id="b03ee-188">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="b03ee-189">Kliknij przycisk **nagłówki** karcie **odpowiedzi** okienko i skopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="b03ee-189">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Karta nagłówki konsoli Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="b03ee-191">Nagłówek lokalizacji URI umożliwia dostęp do zasobu, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="b03ee-191">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="b03ee-192">`Create` Metoda zwraca [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="b03ee-192">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="b03ee-193">Pierwszy parametr przekazany do `CreatedAtRoute` reprezentuje nazwanej trasy do użycia podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="b03ee-193">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="b03ee-194">Odwołania, który `GetById` metody utworzony `"GetTodo"` o nazwie trasy:</span><span class="sxs-lookup"><span data-stu-id="b03ee-194">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="b03ee-195">Aktualizacja</span><span class="sxs-lookup"><span data-stu-id="b03ee-195">Update</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="b03ee-196">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="b03ee-196">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="b03ee-197">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="b03ee-197">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="b03ee-198">`Update` przypomina `Create`, ale używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="b03ee-198">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="b03ee-199">Odpowiedź jest [204 (bez zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b03ee-199">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="b03ee-200">Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice.</span><span class="sxs-lookup"><span data-stu-id="b03ee-200">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="b03ee-201">Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="b03ee-201">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="b03ee-203">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="b03ee-203">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="b03ee-204">Odpowiedź jest [204 (bez zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b03ee-204">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
