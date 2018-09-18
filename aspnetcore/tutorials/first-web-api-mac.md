---
title: Tworzenie internetowego interfejsu API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Tworzenie internetowego interfejsu API platformy ASP.NET Core MVC i programu Visual Studio dla komputerów Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011628"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="5c7e2-103">Tworzenie internetowego interfejsu API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5c7e2-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="5c7e2-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="5c7e2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="5c7e2-105">W tym samouczku Tworzenie internetowego interfejsu API do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="5c7e2-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="5c7e2-106">Interfejs użytkownika nie jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-106">The UI isn't constructed.</span></span>

<span data-ttu-id="5c7e2-107">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="5c7e2-108">System macOS: Web API za pomocą programu Visual Studio dla komputerów Mac (w tym samouczku)</span><span class="sxs-lookup"><span data-stu-id="5c7e2-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="5c7e2-109">Windows: [internetowego interfejsu API z programem Visual Studio for Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="5c7e2-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="5c7e2-110">macOS i Linux, Windows: [interfejsu API sieci Web za pomocą programu Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="5c7e2-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="5c7e2-111">Zobacz [wprowadzenie do ASP.NET Core MVC w systemie macOS lub Linux](xref:tutorials/first-mvc-app-xplat/index) na przykład, który korzysta z trwałego bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c7e2-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5c7e2-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="5c7e2-113">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="5c7e2-113">Create the project</span></span>

<span data-ttu-id="5c7e2-114">W programie Visual Studio, wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="5c7e2-116">Wybierz **aplikacji programu .NET Core** > **interfejsu API sieci Web platformy ASP.NET Core** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)

<span data-ttu-id="5c7e2-118">Wprowadź *TodoApi* dla **Nazwa projektu**, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="5c7e2-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="5c7e2-120">Launch the app</span></span>

<span data-ttu-id="5c7e2-121">W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="5c7e2-122">Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="5c7e2-123">Wystąpi błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="5c7e2-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="5c7e2-124">Zmień adres URL do `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="5c7e2-125">`ValuesController` Dane są wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="5c7e2-126">Dodaj obsługę platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5c7e2-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="5c7e2-127">Zainstaluj [Entity Framework Core InMemory](/ef/core/providers/in-memory/) dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="5c7e2-128">Ten dostawca bazy danych umożliwia platformy Entity Framework Core ma być używany z bazą danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="5c7e2-129">Z **projektu** menu, wybierz opcję **Dodaj pakiety NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="5c7e2-130">Alternatywnie możesz kliknąć prawym przyciskiem myszy **zależności**, a następnie wybierz pozycję **Dodawanie pakietów**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="5c7e2-131">Wprowadź `EntityFrameworkCore.InMemory` w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="5c7e2-132">Wybierz `Microsoft.EntityFrameworkCore.InMemory`, a następnie wybierz pozycję **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="5c7e2-133">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="5c7e2-133">Add a model class</span></span>

<span data-ttu-id="5c7e2-134">Model jest obiekt reprezentujący dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="5c7e2-135">W takim przypadku tylko model jest element do wykonania.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="5c7e2-136">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="5c7e2-137">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="5c7e2-138">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-138">Name the folder *Models*.</span></span>

![Nowy folder](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="5c7e2-140">Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="5c7e2-141">Kliknij prawym przyciskiem myszy *modeli* folder, a następnie wybierz **Dodaj** > **nowy plik** > **ogólne**  >  **Pusta klasa**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="5c7e2-142">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="5c7e2-143">Zastąp wygenerowany kod za pomocą:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="5c7e2-144">Baza danych generuje `Id` podczas `TodoItem` zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="5c7e2-145">Utwórz kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="5c7e2-145">Create the database context</span></span>

<span data-ttu-id="5c7e2-146">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="5c7e2-147">Tworzenie tej klasy, wynikające z `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="5c7e2-148">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="5c7e2-149">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="5c7e2-149">Add a controller</span></span>

<span data-ttu-id="5c7e2-150">W Eksploratorze rozwiązań w *kontrolerów* folderu, dodać klasę `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="5c7e2-151">Zastąp wygenerowany kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="5c7e2-152">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="5c7e2-152">Launch the app</span></span>

<span data-ttu-id="5c7e2-153">W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="5c7e2-154">Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="5c7e2-155">Wystąpi błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="5c7e2-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="5c7e2-156">Zmień adres URL do `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="5c7e2-157">`ValuesController` Dane są wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="5c7e2-158">Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="5c7e2-159">Zwracane są następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="5c7e2-160">Implementowanie inne operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="5c7e2-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="5c7e2-161">Dodamy `Create`, `Update`, i `Delete` metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="5c7e2-162">Te metody są odmiany motywu, więc po prostu będzie I wyświetlić kod i wyróżnić najważniejsze różnice.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="5c7e2-163">Po dodaniu lub zmodyfikowaniu kodu, skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="5c7e2-164">Create</span><span class="sxs-lookup"><span data-stu-id="5c7e2-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5c7e2-165">Powyższa metoda reaguje na metodę POST protokołu HTTP, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5c7e2-166">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybut informuje MVC, aby uzyskać wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5c7e2-167">Powyższa metoda reaguje na metodę POST protokołu HTTP, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5c7e2-168">MVC pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="5c7e2-169">`CreatedAtRoute` Metoda zwraca odpowiedź 201.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="5c7e2-170">Jest to standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="5c7e2-171">`CreatedAtRoute` również dodaje do odpowiedzi nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="5c7e2-172">Nagłówek Location określa identyfikator URI nowo utworzonego zadania do wykonania.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="5c7e2-173">Zobacz [10.2.2 201 utworzone](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5c7e2-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="5c7e2-174">Wyślij żądanie utworzenia przy użyciu narzędzia Postman</span><span class="sxs-lookup"><span data-stu-id="5c7e2-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="5c7e2-175">Uruchom aplikację (**Uruchom** > **rozpocząć debugowanie**).</span><span class="sxs-lookup"><span data-stu-id="5c7e2-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="5c7e2-176">Otwórz narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-176">Open Postman.</span></span>

![Konsola narzędzia postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="5c7e2-178">Zaktualizuj numer portu w adresie URL localhost.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="5c7e2-179">Ustawia metodę HTTP *WPIS*.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="5c7e2-180">Kliknij przycisk **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="5c7e2-181">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="5c7e2-182">Ustaw typ *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="5c7e2-183">Wprowadź treść żądania z elementem zadań do wykonania przypominającą następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="5c7e2-184">Kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="5c7e2-185">Jeśli odpowiedź nie jest wyświetlany po kliknięciu przycisku **wysyłania**, wyłącz **weryfikacji certyfikacji SSL** opcji.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="5c7e2-186">To znajduje się w folderze **pliku** > **ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="5c7e2-187">Kliknij przycisk **wysyłania** przycisk ponownie po wyłączeniu ustawienia.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-187">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="5c7e2-188">Kliknij przycisk **nagłówki** karcie **odpowiedzi** okienka i skopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="5c7e2-190">Dostęp do zasobu, który został utworzony, można użyć w nagłówku Location identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="5c7e2-191">`Create` Metoda zwraca [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="5c7e2-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="5c7e2-192">Pierwszy parametr przekazany do `CreatedAtRoute` reprezentuje trasą mającą nazwę do użycia podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="5c7e2-193">Pamiętamy `GetById` metoda utworzone `"GetTodo"` o nazwie trasy:</span><span class="sxs-lookup"><span data-stu-id="5c7e2-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="5c7e2-194">Aktualizacja</span><span class="sxs-lookup"><span data-stu-id="5c7e2-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="5c7e2-195">`Update` jest podobny do `Create`, ale używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="5c7e2-196">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5c7e2-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5c7e2-197">Według specyfikacji protokołu HTTP żądanie PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko różnice.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="5c7e2-198">Aby obsługiwać aktualizacje częściowe, należy użyć HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="5c7e2-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="5c7e2-200">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="5c7e2-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="5c7e2-201">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5c7e2-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
