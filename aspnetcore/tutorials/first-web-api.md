---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core MVC'
author: rick-anderson
description: Tworzenie internetowego interfejsu API platformy ASP.NET Core MVC
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: tutorials/first-web-api
ms.openlocfilehash: a27031550dd6fe810a250b369f83b7fe322aad1f
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329175"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="b1934-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b1934-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="b1934-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="b1934-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="b1934-105">W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1934-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="b1934-106">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b1934-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b1934-107">Utwórz projekt interfejsu api sieci web.</span><span class="sxs-lookup"><span data-stu-id="b1934-107">Create a web api project.</span></span>
> * <span data-ttu-id="b1934-108">Dodaj klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="b1934-108">Add a model class.</span></span>
> * <span data-ttu-id="b1934-109">Utwórz kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b1934-109">Create the database context.</span></span>
> * <span data-ttu-id="b1934-110">Zarejestruj kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b1934-110">Register the database context.</span></span>
> * <span data-ttu-id="b1934-111">Dodawanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b1934-111">Add a controller.</span></span>
> * <span data-ttu-id="b1934-112">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="b1934-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="b1934-113">Konfigurowanie routingu i ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="b1934-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="b1934-114">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="b1934-114">Specify return values.</span></span>
> * <span data-ttu-id="b1934-115">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="b1934-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="b1934-116">Wywoływanie internetowego interfejsu api przy użyciu jQuery.</span><span class="sxs-lookup"><span data-stu-id="b1934-116">Call the web api with jQuery.</span></span>

<span data-ttu-id="b1934-117">Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b1934-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="b1934-118">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b1934-118">Overview</span></span>

<span data-ttu-id="b1934-119">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="b1934-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="b1934-120">interfejs API</span><span class="sxs-lookup"><span data-stu-id="b1934-120">API</span></span> | <span data-ttu-id="b1934-121">Opis</span><span class="sxs-lookup"><span data-stu-id="b1934-121">Description</span></span> | <span data-ttu-id="b1934-122">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="b1934-122">Request body</span></span> | <span data-ttu-id="b1934-123">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="b1934-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="b1934-124">Pobierz /api/todo</span><span class="sxs-lookup"><span data-stu-id="b1934-124">GET /api/todo</span></span> | <span data-ttu-id="b1934-125">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-125">Get all to-do items</span></span> | <span data-ttu-id="b1934-126">Brak</span><span class="sxs-lookup"><span data-stu-id="b1934-126">None</span></span> | <span data-ttu-id="b1934-127">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-127">Array of to-do items</span></span>|
|<span data-ttu-id="b1934-128">Pobierz/interfejs API/zadania / {id}</span><span class="sxs-lookup"><span data-stu-id="b1934-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="b1934-129">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="b1934-129">Get an item by ID</span></span> | <span data-ttu-id="b1934-130">Brak</span><span class="sxs-lookup"><span data-stu-id="b1934-130">None</span></span> | <span data-ttu-id="b1934-131">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-131">To-do item</span></span>|
|<span data-ttu-id="b1934-132">OPUBLIKUJ/api/zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-132">POST /api/todo</span></span> | <span data-ttu-id="b1934-133">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="b1934-133">Add a new item</span></span> | <span data-ttu-id="b1934-134">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-134">To-do item</span></span> | <span data-ttu-id="b1934-135">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-135">To-do item</span></span> |
|<span data-ttu-id="b1934-136">PUT/interfejs API/zadania / {id}</span><span class="sxs-lookup"><span data-stu-id="b1934-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="b1934-137">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="b1934-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="b1934-138">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-138">To-do item</span></span> | <span data-ttu-id="b1934-139">Brak</span><span class="sxs-lookup"><span data-stu-id="b1934-139">None</span></span> |
|<span data-ttu-id="b1934-140">Usuń/interfejs API/zadania / {id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="b1934-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="b1934-141">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="b1934-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="b1934-142">Brak</span><span class="sxs-lookup"><span data-stu-id="b1934-142">None</span></span> | <span data-ttu-id="b1934-143">Brak</span><span class="sxs-lookup"><span data-stu-id="b1934-143">None</span></span>|

<span data-ttu-id="b1934-144">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1934-144">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie i przesyła żądanie i odbiera odpowiedź od aplikacji rysowania po prawej stronie pola.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="b1934-149">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="b1934-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1934-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1934-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1934-151">Z **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b1934-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b1934-152">Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="b1934-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="b1934-153">Nadaj projektowi nazwę *TodoApi* i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1934-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="b1934-154">W **nowej podstawowej aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz wersję platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1934-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="b1934-155">Wybierz **API** szablon i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1934-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="b1934-156">Czy **nie** wybierz **włączyć obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="b1934-156">Do **not** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1934-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1934-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b1934-159">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b1934-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b1934-160">Zmień katalog (`cd`) do folderu, który będzie zawierać folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="b1934-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="b1934-161">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b1934-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="b1934-162">Te polecenia Utwórz nowy projekt interfejsu API sieci web i Otwórz nowe wystąpienie programu Visual Studio Code w nowym folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="b1934-162">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="b1934-163">Gdy okno dialogowe z pytaniem, jeśli chcesz dodać wymagane zasoby do projektu, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="b1934-163">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b1934-164">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1934-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b1934-165">Wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="b1934-165">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="b1934-167">Wybierz **aplikacji programu .NET Core** > **interfejsu API sieci Web platformy ASP.NET Core** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b1934-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="b1934-169">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="b1934-169">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="b1934-170">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="b1934-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="b1934-172">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="b1934-172">Test the API</span></span>

<span data-ttu-id="b1934-173">Szablon projektu umożliwia utworzenie `values` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b1934-173">The project template creates a `values` API.</span></span> <span data-ttu-id="b1934-174">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="b1934-174">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1934-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1934-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b1934-176">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="b1934-176">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="b1934-177">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="b1934-177">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="b1934-178">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="b1934-178">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="b1934-179">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="b1934-179">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1934-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1934-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b1934-181">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="b1934-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="b1934-182">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="b1934-182">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b1934-183">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1934-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b1934-184">Wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1934-184">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="b1934-185">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="b1934-185">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="b1934-186">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="b1934-186">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="b1934-187">Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="b1934-187">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="b1934-188">Zwracane są następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="b1934-188">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="b1934-189">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="b1934-189">Add a model class</span></span>

<span data-ttu-id="b1934-190">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1934-190">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="b1934-191">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="b1934-191">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1934-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1934-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1934-193">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="b1934-193">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="b1934-194">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="b1934-194">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="b1934-195">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="b1934-195">Name the folder *Models*.</span></span>

* <span data-ttu-id="b1934-196">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="b1934-196">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="b1934-197">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b1934-197">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="b1934-198">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="b1934-198">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1934-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1934-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b1934-200">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="b1934-200">Add a folder named *Models*.</span></span>

* <span data-ttu-id="b1934-201">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b1934-201">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b1934-202">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1934-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b1934-203">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="b1934-203">Right-click the project.</span></span> <span data-ttu-id="b1934-204">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="b1934-204">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="b1934-205">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="b1934-205">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="b1934-207">Kliknij prawym przyciskiem myszy *modeli* folder, a następnie wybierz **Dodaj** > **nowy plik** > **ogólne**  >  **Pusta klasa**.</span><span class="sxs-lookup"><span data-stu-id="b1934-207">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="b1934-208">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="b1934-208">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="b1934-209">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="b1934-209">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="b1934-210">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b1934-210">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="b1934-211">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="b1934-211">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="b1934-212">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="b1934-212">Add a database context</span></span>

<span data-ttu-id="b1934-213">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1934-213">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="b1934-214">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="b1934-214">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1934-215">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1934-215">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1934-216">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="b1934-216">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="b1934-217">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b1934-217">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1934-218">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1934-218">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b1934-219">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="b1934-219">Add a `TodoContext` class to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b1934-220">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1934-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b1934-221">Dodaj `TodoContext` klasy w *modeli* folderu:</span><span class="sxs-lookup"><span data-stu-id="b1934-221">Add a `TodoContext` class in the *Models* folder:</span></span>

---

* <span data-ttu-id="b1934-222">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="b1934-222">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="b1934-223">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="b1934-223">Register the database context</span></span>

<span data-ttu-id="b1934-224">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="b1934-224">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b1934-225">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="b1934-225">The container provides the service to controllers.</span></span>

<span data-ttu-id="b1934-226">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="b1934-226">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="b1934-227">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="b1934-227">The preceding code:</span></span>

* <span data-ttu-id="b1934-228">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="b1934-228">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="b1934-229">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b1934-229">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="b1934-230">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b1934-230">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="b1934-231">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="b1934-231">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1934-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1934-232">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1934-233">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="b1934-233">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="b1934-234">Wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="b1934-234">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="b1934-235">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="b1934-235">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="b1934-236">Nazwa klasy *TodoController*i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b1934-236">Name the class *TodoController*, and select **Add**.</span></span>

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1934-238">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1934-238">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b1934-239">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="b1934-239">In the *Controllers* folder, create a class named `TodoController`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b1934-240">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1934-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b1934-241">W *kontrolerów* folderu, dodać klasę `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="b1934-241">In the *Controllers* folder, add the class `TodoController`.</span></span>

---

* <span data-ttu-id="b1934-242">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="b1934-242">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="b1934-243">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="b1934-243">The preceding code:</span></span>

* <span data-ttu-id="b1934-244">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="b1934-244">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="b1934-245">Rozszerza klasę za pomocą [ `[ApiController]` ](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b1934-245">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="b1934-246">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b1934-246">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="b1934-247">Aby uzyskać informacji na temat określonych zachowań, które umożliwia atrybutu, zobacz [adnotacji z atrybutem klasy ApiController](xref:web-api/index#annotation-with-apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="b1934-247">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="b1934-248">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b1934-248">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="b1934-249">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="b1934-249">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="b1934-250">Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta.</span><span class="sxs-lookup"><span data-stu-id="b1934-250">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="b1934-251">Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="b1934-251">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="b1934-252">Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b1934-252">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="b1934-253">Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.</span><span class="sxs-lookup"><span data-stu-id="b1934-253">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="b1934-254">Dodaj metody Get</span><span class="sxs-lookup"><span data-stu-id="b1934-254">Add Get methods</span></span>

<span data-ttu-id="b1934-255">Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="b1934-255">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="b1934-256">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="b1934-256">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="b1934-257">Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b1934-257">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="b1934-258">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b1934-258">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="b1934-259">Następującą odpowiedź HTTP jest tworzony przez wywołanie metody `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="b1934-259">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="b1934-260">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="b1934-260">Routing and URL paths</span></span>

<span data-ttu-id="b1934-261">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="b1934-261">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="b1934-262">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b1934-262">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="b1934-263">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="b1934-263">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="b1934-264">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="b1934-264">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="b1934-265">W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera, więc nazwa kontrolera jest "todo".</span><span class="sxs-lookup"><span data-stu-id="b1934-265">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="b1934-266">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b1934-266">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="b1934-267">Jeśli `[HttpGet]` atrybut ma szablon trasy (na przykład `[HttpGet("/products")]`, dołączania, do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="b1934-267">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="b1934-268">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="b1934-268">This sample doesn't use a template.</span></span> <span data-ttu-id="b1934-269">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="b1934-269">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="b1934-270">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b1934-270">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="b1934-271">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="b1934-271">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="b1934-272">`Name = "GetTodo"` Parametr tworzy trasą mającą nazwę.</span><span class="sxs-lookup"><span data-stu-id="b1934-272">The `Name = "GetTodo"` parameter creates a named route.</span></span> <span data-ttu-id="b1934-273">Zostanie wyświetlony, pokażemy, jak aplikacja może używać nazwy do utworzenia łącza HTTP, używając nazwy trasy.</span><span class="sxs-lookup"><span data-stu-id="b1934-273">You'll see later how the app can use the name to create an HTTP link using the route name.</span></span>

## <a name="return-values"></a><span data-ttu-id="b1934-274">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="b1934-274">Return values</span></span>

<span data-ttu-id="b1934-275">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="b1934-275">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="b1934-276">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b1934-276">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="b1934-277">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="b1934-277">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="b1934-278">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="b1934-278">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="b1934-279">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b1934-279">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="b1934-280">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="b1934-280">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="b1934-281">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="b1934-281">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="b1934-282">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="b1934-282">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="b1934-283">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="b1934-283">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="b1934-284">Metoda GetTodoItems testu</span><span class="sxs-lookup"><span data-stu-id="b1934-284">Test the GetTodoItems method</span></span>

<span data-ttu-id="b1934-285">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b1934-285">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="b1934-286">Zainstaluj [narzędzia Postman](https://www.getpostman.com/apps)</span><span class="sxs-lookup"><span data-stu-id="b1934-286">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="b1934-287">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="b1934-287">Start the web app.</span></span>
* <span data-ttu-id="b1934-288">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="b1934-288">Start Postman.</span></span>
* <span data-ttu-id="b1934-289">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="b1934-289">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="b1934-290">Z **Plik > Ustawienia** (\**ogólne* karty), wyłącz **weryfikacji certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="b1934-290">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="b1934-291">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b1934-291">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="b1934-292">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="b1934-292">Create a new request.</span></span>
  * <span data-ttu-id="b1934-293">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="b1934-293">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="b1934-294">Ustaw adres URL żądania `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="b1934-294">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="b1934-295">Na przykład `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="b1934-295">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="b1934-296">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="b1934-296">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="b1934-297">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="b1934-297">Select **Send**.</span></span>

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="b1934-299">Dodawanie metody Create</span><span class="sxs-lookup"><span data-stu-id="b1934-299">Add a Create method</span></span>

<span data-ttu-id="b1934-300">Dodaj następujący kod `PostTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="b1934-300">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="b1934-301">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b1934-301">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b1934-302">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b1934-302">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="b1934-303">`CreatedAtRoute` Metody:</span><span class="sxs-lookup"><span data-stu-id="b1934-303">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="b1934-304">Zwraca odpowiedź 201.</span><span class="sxs-lookup"><span data-stu-id="b1934-304">Returns a 201 response.</span></span> <span data-ttu-id="b1934-305">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b1934-305">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="b1934-306">Dodaje do odpowiedzi nagłówek lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b1934-306">Adds a Location header to the response.</span></span> <span data-ttu-id="b1934-307">Nagłówek Location określa identyfikator URI nowo utworzonego zadania do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b1934-307">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="b1934-308">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="b1934-308">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="b1934-309">Używa "GetTodo" o nazwie tras w celu utworzenia adresu URL.</span><span class="sxs-lookup"><span data-stu-id="b1934-309">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="b1934-310">"GetTodo" o nazwie trasa jest zdefiniowana w `GetTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="b1934-310">The "GetTodo" named route is defined in `GetTodoItem`:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="b1934-311">Metoda PostTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="b1934-311">Test the PostTodoItem method</span></span>

* <span data-ttu-id="b1934-312">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="b1934-312">Build the project.</span></span>
* <span data-ttu-id="b1934-313">W narzędziu Postman, Ustawia metodę HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="b1934-313">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="b1934-314">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="b1934-314">Select the **Body** tab.</span></span>
* <span data-ttu-id="b1934-315">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="b1934-315">Select the **raw** radio button.</span></span>
* <span data-ttu-id="b1934-316">Ustaw typ **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="b1934-316">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="b1934-317">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="b1934-317">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="b1934-318">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="b1934-318">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  <span data-ttu-id="b1934-320">Jeśli 405 błąd niedozwolona metoda, prawdopodobnie wynik nie Kompilowanie projektu po dodaniu, po dodaniu `PostTodoItem` metody.</span><span class="sxs-lookup"><span data-stu-id="b1934-320">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="b1934-321">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="b1934-321">Test the location header URI</span></span>

* <span data-ttu-id="b1934-322">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="b1934-322">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="b1934-323">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="b1934-323">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="b1934-325">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="b1934-325">Set the method to GET.</span></span>
* <span data-ttu-id="b1934-326">Wklej identyfikator URI (na przykład `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="b1934-326">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="b1934-327">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="b1934-327">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="b1934-328">Dodaj metodę PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="b1934-328">Add a PutTodoItem method</span></span>

<span data-ttu-id="b1934-329">Dodaj następujący kod `PutTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="b1934-329">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="b1934-330">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="b1934-330">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="b1934-331">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b1934-331">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="b1934-332">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="b1934-332">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="b1934-333">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="b1934-333">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="b1934-334">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="b1934-334">Test the PutTodoItem method</span></span>

<span data-ttu-id="b1934-335">Zaktualizuj element zadania do wykonania, który ma identyfikator = 1 i ustaw jego nazwę na "feed ryb":</span><span class="sxs-lookup"><span data-stu-id="b1934-335">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="b1934-336">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="b1934-336">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="b1934-338">Dodaj metodę DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="b1934-338">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="b1934-339">Dodaj następujący kod `DeleteTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="b1934-339">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="b1934-340">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b1934-340">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="b1934-341">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="b1934-341">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="b1934-342">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="b1934-342">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="b1934-343">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="b1934-343">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="b1934-344">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="b1934-344">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="b1934-345">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="b1934-345">Select **Send**</span></span>

<span data-ttu-id="b1934-346">Przykładowa aplikacja umożliwia usunięcie wszystkich elementów, ale po usunięciu ostatniego elementu jest tworzony nowy przez konstruktora klasy modelu podczas następnego wywołania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b1934-346">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="b1934-347">Wywoływanie interfejsu API przy użyciu jQuery</span><span class="sxs-lookup"><span data-stu-id="b1934-347">Call the API with jQuery</span></span>

<span data-ttu-id="b1934-348">W tej sekcji strony HTML jest dodawany, który używa technologii jQuery do wywołania sieci web interfejsu api.</span><span class="sxs-lookup"><span data-stu-id="b1934-348">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="b1934-349">jQuery inicjuje żądanie i aktualizowanie strony ze szczegółami z odpowiedzi interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b1934-349">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="b1934-350">Konfigurowanie aplikacji w celu [Obsługa plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [włączyć domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span><span class="sxs-lookup"><span data-stu-id="b1934-350">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="b1934-351">Tworzenie *wwwroot* folder w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="b1934-351">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="b1934-352">Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="b1934-352">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="b1934-353">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b1934-353">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="b1934-354">Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="b1934-354">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="b1934-355">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b1934-355">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="b1934-356">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="b1934-356">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="b1934-357">Otwórz *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b1934-357">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="b1934-358">Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.</span><span class="sxs-lookup"><span data-stu-id="b1934-358">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="b1934-359">Istnieje kilka sposobów uzyskania biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="b1934-359">There are several ways to get jQuery.</span></span> <span data-ttu-id="b1934-360">W poprzednim fragmencie kodu biblioteki jest ładowany z usługi CDN.</span><span class="sxs-lookup"><span data-stu-id="b1934-360">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="b1934-361">Ten przykład wywołuje wszystkie metody CRUD interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b1934-361">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="b1934-362">Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b1934-362">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="b1934-363">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-363">Get a list of to-do items</span></span>

<span data-ttu-id="b1934-364">JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `GET` żądanie do interfejsu API, która zwraca wartość JSON reprezentująca tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b1934-364">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="b1934-365">`success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b1934-365">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="b1934-366">Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b1934-366">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="b1934-367">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-367">Add a to-do item</span></span>

<span data-ttu-id="b1934-368">[Ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `POST` żądania z elementem zadań do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="b1934-368">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="b1934-369">`accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych.</span><span class="sxs-lookup"><span data-stu-id="b1934-369">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="b1934-370">Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="b1934-370">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="b1934-371">Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="b1934-371">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="b1934-372">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-372">Update a to-do item</span></span>

<span data-ttu-id="b1934-373">Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="b1934-373">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="b1934-374">`url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="b1934-374">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="b1934-375">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="b1934-375">Delete a to-do item</span></span>

<span data-ttu-id="b1934-376">Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="b1934-376">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="b1934-377">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b1934-377">Additional resources</span></span>

<span data-ttu-id="b1934-378">[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="b1934-378">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="b1934-379">Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="b1934-379">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="b1934-380">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="b1934-380">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="b1934-381">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b1934-381">Next steps</span></span>

<span data-ttu-id="b1934-382">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b1934-382">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b1934-383">Utwórz projekt interfejsu api sieci web.</span><span class="sxs-lookup"><span data-stu-id="b1934-383">Create a web api project.</span></span>
> * <span data-ttu-id="b1934-384">Dodaj klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="b1934-384">Add a model class.</span></span>
> * <span data-ttu-id="b1934-385">Utwórz kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b1934-385">Create the database context.</span></span>
> * <span data-ttu-id="b1934-386">Zarejestruj kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b1934-386">Register the database context.</span></span>
> * <span data-ttu-id="b1934-387">Dodawanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b1934-387">Add a controller.</span></span>
> * <span data-ttu-id="b1934-388">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="b1934-388">Add CRUD methods.</span></span>
> * <span data-ttu-id="b1934-389">Konfigurowanie routingu i ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="b1934-389">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="b1934-390">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="b1934-390">Specify return values.</span></span>
> * <span data-ttu-id="b1934-391">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="b1934-391">Call the web API with Postman.</span></span>
> * <span data-ttu-id="b1934-392">Wywoływanie internetowego interfejsu api przy użyciu jQuery.</span><span class="sxs-lookup"><span data-stu-id="b1934-392">Call the web api with jQuery.</span></span>

<span data-ttu-id="b1934-393">Przejdź do następnego samouczka, aby dowiedzieć się, jak można wygenerować stron pomocy interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="b1934-393">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
