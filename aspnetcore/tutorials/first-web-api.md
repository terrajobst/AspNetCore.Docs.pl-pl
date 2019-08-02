---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 60235af56077127093ac1d77338bc228a6edf073
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602535"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="55376-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55376-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="55376-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="55376-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="55376-105">W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="55376-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="55376-106">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="55376-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="55376-107">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="55376-107">Create a web API project.</span></span>
> * <span data-ttu-id="55376-108">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="55376-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="55376-109">Tworzy szkielet kontrolera z metodami CRUD.</span><span class="sxs-lookup"><span data-stu-id="55376-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="55376-110">Skonfiguruj Routing, ścieżki URL i wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="55376-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="55376-111">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="55376-111">Call the web API with Postman.</span></span>

<span data-ttu-id="55376-112">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="55376-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="55376-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="55376-113">Overview</span></span>

<span data-ttu-id="55376-114">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="55376-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="55376-115">interfejs API</span><span class="sxs-lookup"><span data-stu-id="55376-115">API</span></span> | <span data-ttu-id="55376-116">Opis</span><span class="sxs-lookup"><span data-stu-id="55376-116">Description</span></span> | <span data-ttu-id="55376-117">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="55376-117">Request body</span></span> | <span data-ttu-id="55376-118">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="55376-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="55376-119">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="55376-119">GET /api/TodoItems</span></span> | <span data-ttu-id="55376-120">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-120">Get all to-do items</span></span> | <span data-ttu-id="55376-121">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-121">None</span></span> | <span data-ttu-id="55376-122">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-122">Array of to-do items</span></span>|
|<span data-ttu-id="55376-123">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="55376-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="55376-124">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="55376-124">Get an item by ID</span></span> | <span data-ttu-id="55376-125">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-125">None</span></span> | <span data-ttu-id="55376-126">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-126">To-do item</span></span>|
|<span data-ttu-id="55376-127">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="55376-127">POST /api/TodoItems</span></span> | <span data-ttu-id="55376-128">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="55376-128">Add a new item</span></span> | <span data-ttu-id="55376-129">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-129">To-do item</span></span> | <span data-ttu-id="55376-130">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-130">To-do item</span></span> |
|<span data-ttu-id="55376-131">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="55376-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="55376-132">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="55376-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="55376-133">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-133">To-do item</span></span> | <span data-ttu-id="55376-134">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-134">None</span></span> |
|<span data-ttu-id="55376-135">Usuń/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="55376-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="55376-136">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="55376-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="55376-137">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-137">None</span></span> | <span data-ttu-id="55376-138">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-138">None</span></span>|

<span data-ttu-id="55376-139">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55376-139">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="55376-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="55376-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55376-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55376-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55376-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55376-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="55376-149">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="55376-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="55376-151">Z menu **plik** wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="55376-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="55376-152">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="55376-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="55376-153">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="55376-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="55376-154">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="55376-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="55376-155">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="55376-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="55376-156">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="55376-156">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55376-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55376-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="55376-159">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="55376-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="55376-160">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="55376-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="55376-161">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="55376-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="55376-162">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="55376-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="55376-163">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="55376-163">The preceding commands:</span></span>

  * <span data-ttu-id="55376-164">Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="55376-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="55376-165">Dodaje pakiety NuGet, które są wymagane w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="55376-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55376-166">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55376-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="55376-167">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="55376-167">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="55376-169">Wybierz pozycję **interfejs API** > > aplikacji> .NET Core.</span><span class="sxs-lookup"><span data-stu-id="55376-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="55376-171">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję docelowa **platforma** \* *.NET Core 3,0*.</span><span class="sxs-lookup"><span data-stu-id="55376-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="55376-172">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="55376-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="55376-174">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="55376-174">Test the API</span></span>

<span data-ttu-id="55376-175">Szablon projektu umożliwia utworzenie `WeatherForecast` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="55376-176">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="55376-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="55376-178">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="55376-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="55376-179">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="55376-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="55376-180">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="55376-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="55376-181">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="55376-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55376-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55376-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="55376-183">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="55376-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="55376-184">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="55376-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55376-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55376-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="55376-186">Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="55376-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="55376-187">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="55376-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="55376-188">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="55376-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="55376-189">Dołącz `/WeatherForecast` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="55376-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="55376-190">Zwracany jest kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="55376-190">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="55376-191">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="55376-191">Add a model class</span></span>

<span data-ttu-id="55376-192">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55376-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="55376-193">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="55376-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="55376-195">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="55376-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="55376-196">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="55376-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="55376-197">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="55376-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="55376-198">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="55376-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="55376-199">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="55376-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="55376-200">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55376-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55376-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="55376-202">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="55376-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="55376-203">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55376-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55376-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="55376-205">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="55376-205">Right-click the project.</span></span> <span data-ttu-id="55376-206">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="55376-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="55376-207">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="55376-207">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="55376-209">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.</span><span class="sxs-lookup"><span data-stu-id="55376-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="55376-210">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="55376-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="55376-211">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="55376-212">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="55376-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="55376-213">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="55376-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="55376-214">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="55376-214">Add a database context</span></span>

<span data-ttu-id="55376-215">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="55376-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="55376-216">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="55376-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="55376-218">Dodaj Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="55376-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="55376-219">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="55376-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="55376-220">Zaznacz pole wyboru **Uwzględnij wersję wstępną** .</span><span class="sxs-lookup"><span data-stu-id="55376-220">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="55376-221">Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="55376-221">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="55376-222">W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer v 3.0.0 — wersja** zapoznawcza.</span><span class="sxs-lookup"><span data-stu-id="55376-222">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="55376-223">Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="55376-223">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="55376-224">Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="55376-224">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="55376-226">Dodawanie kontekstu bazy danych TodoContext</span><span class="sxs-lookup"><span data-stu-id="55376-226">Add the TodoContext database context</span></span>

* <span data-ttu-id="55376-227">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="55376-227">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="55376-228">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="55376-228">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="55376-229">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="55376-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="55376-230">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="55376-230">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="55376-231">Wprowadź następujący kod:</span><span class="sxs-lookup"><span data-stu-id="55376-231">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="55376-232">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="55376-232">Register the database context</span></span>

<span data-ttu-id="55376-233">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="55376-233">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="55376-234">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="55376-234">The container provides the service to controllers.</span></span>

<span data-ttu-id="55376-235">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="55376-235">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="55376-236">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="55376-236">The preceding code:</span></span>

* <span data-ttu-id="55376-237">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="55376-237">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="55376-238">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="55376-238">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="55376-239">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="55376-239">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="55376-240">Tworzenie szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="55376-240">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-241">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-241">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="55376-242">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="55376-242">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="55376-243">Wybierz pozycję **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="55376-243">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="55376-244">Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="55376-244">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="55376-245">Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:</span><span class="sxs-lookup"><span data-stu-id="55376-245">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="55376-246">Wybierz pozycję **TodoItem (TodoAPI. models)** w **klasie model**.</span><span class="sxs-lookup"><span data-stu-id="55376-246">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="55376-247">W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoAPI. models)** .</span><span class="sxs-lookup"><span data-stu-id="55376-247">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="55376-248">Wybierz pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="55376-248">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="55376-249">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="55376-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="55376-250">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="55376-250">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="55376-251">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="55376-251">The preceding commands:</span></span>

* <span data-ttu-id="55376-252">Dodaj pakiety NuGet wymagane do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="55376-252">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="55376-253">Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="55376-253">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="55376-254">Szkielety `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="55376-254">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="55376-255">Wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="55376-255">The generated code:</span></span>

* <span data-ttu-id="55376-256">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="55376-256">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="55376-257">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="55376-257">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="55376-258">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-258">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="55376-259">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="55376-259">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="55376-260">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="55376-260">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="55376-261">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="55376-261">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="55376-262">Badanie metody PostTodoItem Create</span><span class="sxs-lookup"><span data-stu-id="55376-262">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="55376-263">Zastąp instrukcję return w, `PostTodoItem` aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="55376-263">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="55376-264">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="55376-264">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="55376-265">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="55376-265">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="55376-266"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Metody:</span><span class="sxs-lookup"><span data-stu-id="55376-266">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="55376-267">W razie powodzenia zwraca kod stanu HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="55376-267">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="55376-268">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="55376-268">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="55376-269">Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="55376-269">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="55376-270">Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania. [](https://developer.mozilla.org/docs/Glossary/URI) `Location`</span><span class="sxs-lookup"><span data-stu-id="55376-270">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="55376-271">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="55376-271">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="55376-272">Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka.</span><span class="sxs-lookup"><span data-stu-id="55376-272">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="55376-273">Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="55376-273">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="55376-274">Zainstaluj program Poster</span><span class="sxs-lookup"><span data-stu-id="55376-274">Install Postman</span></span>

<span data-ttu-id="55376-275">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-275">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="55376-276">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="55376-276">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="55376-277">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="55376-277">Start the web app.</span></span>
* <span data-ttu-id="55376-278">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="55376-278">Start Postman.</span></span>
* <span data-ttu-id="55376-279">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="55376-279">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="55376-280">Z **Plik > Ustawienia** (\**ogólne* karty), wyłącz **weryfikacji certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="55376-280">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="55376-281">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="55376-281">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="55376-282">Test PostTodoItem za pomocą programu Poster</span><span class="sxs-lookup"><span data-stu-id="55376-282">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="55376-283">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="55376-283">Create a new request.</span></span>
* <span data-ttu-id="55376-284">Ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="55376-284">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="55376-285">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="55376-285">Select the **Body** tab.</span></span>
* <span data-ttu-id="55376-286">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="55376-286">Select the **raw** radio button.</span></span>
* <span data-ttu-id="55376-287">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="55376-287">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="55376-288">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="55376-288">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="55376-289">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="55376-289">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="55376-291">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="55376-291">Test the location header URI</span></span>

* <span data-ttu-id="55376-292">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="55376-292">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="55376-293">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="55376-293">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="55376-295">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="55376-295">Set the method to GET.</span></span>
* <span data-ttu-id="55376-296">Wklej identyfikator URI (na przykład `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="55376-296">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="55376-297">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="55376-297">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="55376-298">Badanie metod GET</span><span class="sxs-lookup"><span data-stu-id="55376-298">Examine the GET methods</span></span>

<span data-ttu-id="55376-299">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="55376-299">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="55376-300">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu.</span><span class="sxs-lookup"><span data-stu-id="55376-300">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="55376-301">Przykład:</span><span class="sxs-lookup"><span data-stu-id="55376-301">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="55376-302">Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="55376-302">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="55376-303">Test get przy użyciu programu Poster</span><span class="sxs-lookup"><span data-stu-id="55376-303">Test Get with Postman</span></span>

* <span data-ttu-id="55376-304">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="55376-304">Create a new request.</span></span>
* <span data-ttu-id="55376-305">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="55376-305">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="55376-306">Ustaw adres URL żądania `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="55376-306">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="55376-307">Na przykład `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="55376-307">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="55376-308">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="55376-308">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="55376-309">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="55376-309">Select **Send**.</span></span>

<span data-ttu-id="55376-310">Ta aplikacja używa bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="55376-310">This app uses an in-memory database.</span></span> <span data-ttu-id="55376-311">Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="55376-311">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="55376-312">Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55376-312">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="55376-313">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="55376-313">Routing and URL paths</span></span>

<span data-ttu-id="55376-314">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="55376-314">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="55376-315">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="55376-315">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="55376-316">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="55376-316">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="55376-317">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="55376-317">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="55376-318">Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="55376-318">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="55376-319">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="55376-319">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="55376-320">Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="55376-320">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="55376-321">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="55376-321">This sample doesn't use a template.</span></span> <span data-ttu-id="55376-322">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="55376-322">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="55376-323">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="55376-323">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="55376-324">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="55376-324">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="55376-325">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="55376-325">Return values</span></span>

<span data-ttu-id="55376-326">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="55376-326">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="55376-327">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="55376-327">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="55376-328">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="55376-328">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="55376-329">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="55376-329">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="55376-330">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="55376-330">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="55376-331">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="55376-331">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="55376-332">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="55376-332">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="55376-333">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="55376-333">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="55376-334">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="55376-334">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="55376-335">Metoda PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="55376-335">The PutTodoItem method</span></span>

<span data-ttu-id="55376-336">`PutTodoItem` Badanie metody:</span><span class="sxs-lookup"><span data-stu-id="55376-336">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="55376-337">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="55376-337">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="55376-338">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="55376-338">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="55376-339">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="55376-339">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="55376-340">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="55376-340">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="55376-341">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="55376-341">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="55376-342">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="55376-342">Test the PutTodoItem method</span></span>

<span data-ttu-id="55376-343">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="55376-343">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="55376-344">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="55376-344">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="55376-345">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="55376-345">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="55376-346">Zaktualizuj element do wykonania o IDENTYFIKATORze 1 i ustaw jego nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="55376-346">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="55376-347">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="55376-347">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="55376-349">Metoda DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="55376-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="55376-350">`DeleteTodoItem` Badanie metody:</span><span class="sxs-lookup"><span data-stu-id="55376-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="55376-351">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="55376-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="55376-352">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="55376-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="55376-353">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="55376-353">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="55376-354">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="55376-354">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="55376-355">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="55376-355">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="55376-356">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="55376-356">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="55376-357">Wywoływanie interfejsu API z platformy jQuery</span><span class="sxs-lookup"><span data-stu-id="55376-357">Call the API from jQuery</span></span>

<span data-ttu-id="55376-358">Zobacz [samouczek: Wywołaj ASP.NET Core Web API z jQuery](xref:tutorials/web-api-jquery).</span><span class="sxs-lookup"><span data-stu-id="55376-358">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="55376-359">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="55376-359">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="55376-360">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="55376-360">Create a web API project.</span></span>
> * <span data-ttu-id="55376-361">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="55376-361">Add a model class and a database context.</span></span>
> * <span data-ttu-id="55376-362">Dodawanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="55376-362">Add a controller.</span></span>
> * <span data-ttu-id="55376-363">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="55376-363">Add CRUD methods.</span></span>
> * <span data-ttu-id="55376-364">Konfigurowanie routingu i ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="55376-364">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="55376-365">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="55376-365">Specify return values.</span></span>
> * <span data-ttu-id="55376-366">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="55376-366">Call the web API with Postman.</span></span>
> * <span data-ttu-id="55376-367">Wywołaj interfejs API sieci Web za pomocą platformy jQuery.</span><span class="sxs-lookup"><span data-stu-id="55376-367">Call the web API with jQuery.</span></span>

<span data-ttu-id="55376-368">Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="55376-368">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="55376-369">Omówienie</span><span class="sxs-lookup"><span data-stu-id="55376-369">Overview</span></span>

<span data-ttu-id="55376-370">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="55376-370">This tutorial creates the following API:</span></span>

|<span data-ttu-id="55376-371">interfejs API</span><span class="sxs-lookup"><span data-stu-id="55376-371">API</span></span> | <span data-ttu-id="55376-372">Opis</span><span class="sxs-lookup"><span data-stu-id="55376-372">Description</span></span> | <span data-ttu-id="55376-373">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="55376-373">Request body</span></span> | <span data-ttu-id="55376-374">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="55376-374">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="55376-375">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="55376-375">GET /api/TodoItems</span></span> | <span data-ttu-id="55376-376">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-376">Get all to-do items</span></span> | <span data-ttu-id="55376-377">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-377">None</span></span> | <span data-ttu-id="55376-378">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-378">Array of to-do items</span></span>|
|<span data-ttu-id="55376-379">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="55376-379">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="55376-380">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="55376-380">Get an item by ID</span></span> | <span data-ttu-id="55376-381">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-381">None</span></span> | <span data-ttu-id="55376-382">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-382">To-do item</span></span>|
|<span data-ttu-id="55376-383">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="55376-383">POST /api/TodoItems</span></span> | <span data-ttu-id="55376-384">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="55376-384">Add a new item</span></span> | <span data-ttu-id="55376-385">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-385">To-do item</span></span> | <span data-ttu-id="55376-386">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-386">To-do item</span></span> |
|<span data-ttu-id="55376-387">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="55376-387">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="55376-388">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="55376-388">Update an existing item &nbsp;</span></span> | <span data-ttu-id="55376-389">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-389">To-do item</span></span> | <span data-ttu-id="55376-390">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-390">None</span></span> |
|<span data-ttu-id="55376-391">Usuń/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="55376-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="55376-392">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="55376-392">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="55376-393">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-393">None</span></span> | <span data-ttu-id="55376-394">Brak</span><span class="sxs-lookup"><span data-stu-id="55376-394">None</span></span>|

<span data-ttu-id="55376-395">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55376-395">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="55376-401">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="55376-401">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-402">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-402">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55376-403">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55376-403">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55376-404">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55376-404">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="55376-405">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="55376-405">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="55376-407">Z menu **plik** wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="55376-407">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="55376-408">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="55376-408">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="55376-409">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="55376-409">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="55376-410">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** .</span><span class="sxs-lookup"><span data-stu-id="55376-410">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="55376-411">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="55376-411">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="55376-412">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="55376-412">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55376-414">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55376-414">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="55376-415">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="55376-415">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="55376-416">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="55376-416">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="55376-417">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="55376-417">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="55376-418">Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="55376-418">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="55376-419">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="55376-419">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55376-420">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55376-420">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="55376-421">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="55376-421">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="55376-423">Wybierz pozycję **interfejs API** > > aplikacji> .NET Core.</span><span class="sxs-lookup"><span data-stu-id="55376-423">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="55376-425">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="55376-425">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="55376-426">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="55376-426">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="55376-428">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="55376-428">Test the API</span></span>

<span data-ttu-id="55376-429">Szablon projektu umożliwia utworzenie `values` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-429">The project template creates a `values` API.</span></span> <span data-ttu-id="55376-430">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="55376-430">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-431">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-431">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="55376-432">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="55376-432">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="55376-433">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="55376-433">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="55376-434">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="55376-434">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="55376-435">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="55376-435">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55376-436">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55376-436">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="55376-437">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="55376-437">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="55376-438">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="55376-438">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55376-439">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55376-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="55376-440">Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="55376-440">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="55376-441">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="55376-441">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="55376-442">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="55376-442">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="55376-443">Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="55376-443">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="55376-444">Zwracane są następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="55376-444">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="55376-445">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="55376-445">Add a model class</span></span>

<span data-ttu-id="55376-446">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55376-446">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="55376-447">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="55376-447">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-448">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-448">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="55376-449">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="55376-449">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="55376-450">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="55376-450">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="55376-451">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="55376-451">Name the folder *Models*.</span></span>

* <span data-ttu-id="55376-452">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="55376-452">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="55376-453">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="55376-453">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="55376-454">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-454">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55376-455">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55376-455">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="55376-456">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="55376-456">Add a folder named *Models*.</span></span>

* <span data-ttu-id="55376-457">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-457">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55376-458">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55376-458">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="55376-459">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="55376-459">Right-click the project.</span></span> <span data-ttu-id="55376-460">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="55376-460">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="55376-461">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="55376-461">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="55376-463">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.</span><span class="sxs-lookup"><span data-stu-id="55376-463">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="55376-464">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="55376-464">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="55376-465">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-465">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="55376-466">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="55376-466">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="55376-467">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="55376-467">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="55376-468">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="55376-468">Add a database context</span></span>

<span data-ttu-id="55376-469">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="55376-469">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="55376-470">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="55376-470">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-471">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-471">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="55376-472">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="55376-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="55376-473">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="55376-473">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="55376-474">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="55376-474">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="55376-475">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="55376-475">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="55376-476">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-476">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="55376-477">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="55376-477">Register the database context</span></span>

<span data-ttu-id="55376-478">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="55376-478">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="55376-479">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="55376-479">The container provides the service to controllers.</span></span>

<span data-ttu-id="55376-480">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="55376-480">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="55376-481">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="55376-481">The preceding code:</span></span>

* <span data-ttu-id="55376-482">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="55376-482">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="55376-483">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="55376-483">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="55376-484">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="55376-484">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="55376-485">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="55376-485">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55376-486">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55376-486">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="55376-487">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="55376-487">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="55376-488">Wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="55376-488">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="55376-489">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="55376-489">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="55376-490">Nazwa klasy *TodoController*i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="55376-490">Name the class *TodoController*, and select **Add**.</span></span>

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="55376-492">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="55376-492">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="55376-493">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="55376-493">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="55376-494">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-494">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="55376-495">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="55376-495">The preceding code:</span></span>

* <span data-ttu-id="55376-496">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="55376-496">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="55376-497">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="55376-497">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="55376-498">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-498">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="55376-499">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="55376-499">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="55376-500">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="55376-500">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="55376-501">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="55376-501">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="55376-502">Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta.</span><span class="sxs-lookup"><span data-stu-id="55376-502">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="55376-503">Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="55376-503">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="55376-504">Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-504">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="55376-505">Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.</span><span class="sxs-lookup"><span data-stu-id="55376-505">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="55376-506">Dodaj metody Get</span><span class="sxs-lookup"><span data-stu-id="55376-506">Add Get methods</span></span>

<span data-ttu-id="55376-507">Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="55376-507">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="55376-508">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="55376-508">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="55376-509">Zatrzymaj aplikację, jeśli jest nadal uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="55376-509">Stop the app if it's still running.</span></span> <span data-ttu-id="55376-510">Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.</span><span class="sxs-lookup"><span data-stu-id="55376-510">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="55376-511">Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="55376-511">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="55376-512">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="55376-512">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="55376-513">Następującą odpowiedź HTTP jest tworzony przez wywołanie metody `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="55376-513">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="55376-514">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="55376-514">Routing and URL paths</span></span>

<span data-ttu-id="55376-515">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="55376-515">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="55376-516">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="55376-516">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="55376-517">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="55376-517">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="55376-518">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="55376-518">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="55376-519">W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera, więc nazwa kontrolera jest "todo".</span><span class="sxs-lookup"><span data-stu-id="55376-519">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="55376-520">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="55376-520">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="55376-521">Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="55376-521">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="55376-522">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="55376-522">This sample doesn't use a template.</span></span> <span data-ttu-id="55376-523">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="55376-523">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="55376-524">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="55376-524">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="55376-525">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="55376-525">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="55376-526">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="55376-526">Return values</span></span>

<span data-ttu-id="55376-527">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="55376-527">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="55376-528">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="55376-528">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="55376-529">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="55376-529">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="55376-530">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="55376-530">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="55376-531">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="55376-531">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="55376-532">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="55376-532">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="55376-533">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="55376-533">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="55376-534">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="55376-534">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="55376-535">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="55376-535">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="55376-536">Metoda GetTodoItems testu</span><span class="sxs-lookup"><span data-stu-id="55376-536">Test the GetTodoItems method</span></span>

<span data-ttu-id="55376-537">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-537">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="55376-538">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="55376-538">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="55376-539">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="55376-539">Start the web app.</span></span>
* <span data-ttu-id="55376-540">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="55376-540">Start Postman.</span></span>
* <span data-ttu-id="55376-541">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="55376-541">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="55376-542">Z **Plik > Ustawienia** (\**ogólne* karty), wyłącz **weryfikacji certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="55376-542">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="55376-543">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="55376-543">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="55376-544">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="55376-544">Create a new request.</span></span>
  * <span data-ttu-id="55376-545">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="55376-545">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="55376-546">Ustaw adres URL żądania `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="55376-546">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="55376-547">Na przykład `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="55376-547">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="55376-548">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="55376-548">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="55376-549">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="55376-549">Select **Send**.</span></span>

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="55376-551">Dodawanie metody Create</span><span class="sxs-lookup"><span data-stu-id="55376-551">Add a Create method</span></span>

<span data-ttu-id="55376-552">Dodaj następujący kod `PostTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="55376-552">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="55376-553">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="55376-553">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="55376-554">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="55376-554">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="55376-555">`CreatedAtAction` Metody:</span><span class="sxs-lookup"><span data-stu-id="55376-555">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="55376-556">Zwraca kod stanu HTTP 201, jeśli powodzenie.</span><span class="sxs-lookup"><span data-stu-id="55376-556">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="55376-557">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="55376-557">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="55376-558">`Location` Dodaje nagłówek do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="55376-558">Adds a `Location` header to the response.</span></span> <span data-ttu-id="55376-559">`Location` Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="55376-559">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="55376-560">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="55376-560">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="55376-561">Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka.</span><span class="sxs-lookup"><span data-stu-id="55376-561">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="55376-562">Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="55376-562">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="55376-563">Metoda PostTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="55376-563">Test the PostTodoItem method</span></span>

* <span data-ttu-id="55376-564">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="55376-564">Build the project.</span></span>
* <span data-ttu-id="55376-565">W narzędziu Postman, Ustawia metodę HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="55376-565">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="55376-566">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="55376-566">Select the **Body** tab.</span></span>
* <span data-ttu-id="55376-567">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="55376-567">Select the **raw** radio button.</span></span>
* <span data-ttu-id="55376-568">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="55376-568">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="55376-569">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="55376-569">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="55376-570">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="55376-570">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  <span data-ttu-id="55376-572">Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu `PostTodoItem` metody.</span><span class="sxs-lookup"><span data-stu-id="55376-572">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="55376-573">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="55376-573">Test the location header URI</span></span>

* <span data-ttu-id="55376-574">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="55376-574">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="55376-575">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="55376-575">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="55376-577">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="55376-577">Set the method to GET.</span></span>
* <span data-ttu-id="55376-578">Wklej identyfikator URI (na przykład `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="55376-578">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="55376-579">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="55376-579">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="55376-580">Dodaj metodę PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="55376-580">Add a PutTodoItem method</span></span>

<span data-ttu-id="55376-581">Dodaj następujący kod `PutTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="55376-581">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="55376-582">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="55376-582">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="55376-583">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="55376-583">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="55376-584">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="55376-584">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="55376-585">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="55376-585">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="55376-586">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="55376-586">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="55376-587">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="55376-587">Test the PutTodoItem method</span></span>

<span data-ttu-id="55376-588">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="55376-588">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="55376-589">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="55376-589">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="55376-590">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="55376-590">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="55376-591">Zaktualizuj element zadania do wykonania, który ma identyfikator = 1 i ustaw jego nazwę na "feed ryb":</span><span class="sxs-lookup"><span data-stu-id="55376-591">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="55376-592">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="55376-592">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="55376-594">Dodaj metodę DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="55376-594">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="55376-595">Dodaj następujący kod `DeleteTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="55376-595">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="55376-596">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="55376-596">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="55376-597">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="55376-597">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="55376-598">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="55376-598">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="55376-599">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="55376-599">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="55376-600">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="55376-600">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="55376-601">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="55376-601">Select **Send**</span></span>

<span data-ttu-id="55376-602">Przykładowa aplikacja umożliwia usunięcie wszystkich elementów.</span><span class="sxs-lookup"><span data-stu-id="55376-602">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="55376-603">Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-603">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="55376-604">Wywoływanie interfejsu API przy użyciu jQuery</span><span class="sxs-lookup"><span data-stu-id="55376-604">Call the API with jQuery</span></span>

<span data-ttu-id="55376-605">W tej sekcji strony HTML jest dodawany, który używa technologii jQuery do wywołania sieci web interfejsu api.</span><span class="sxs-lookup"><span data-stu-id="55376-605">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="55376-606">jQuery inicjuje żądanie i aktualizowanie strony ze szczegółami z odpowiedzi interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-606">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="55376-607">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-607">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="55376-608">Tworzenie *wwwroot* folder w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="55376-608">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="55376-609">Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="55376-609">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="55376-610">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-610">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="55376-611">Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="55376-611">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="55376-612">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="55376-612">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="55376-613">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="55376-613">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="55376-614">Otwórz *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="55376-614">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="55376-615">Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.</span><span class="sxs-lookup"><span data-stu-id="55376-615">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="55376-616">Istnieje kilka sposobów uzyskania biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="55376-616">There are several ways to get jQuery.</span></span> <span data-ttu-id="55376-617">W poprzednim fragmencie kodu biblioteki jest ładowany z usługi CDN.</span><span class="sxs-lookup"><span data-stu-id="55376-617">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="55376-618">Ten przykład wywołuje wszystkie metody CRUD interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-618">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="55376-619">Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="55376-619">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="55376-620">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-620">Get a list of to-do items</span></span>

<span data-ttu-id="55376-621">JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `GET` żądanie do interfejsu API, która zwraca wartość JSON reprezentująca tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="55376-621">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="55376-622">`success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="55376-622">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="55376-623">Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="55376-623">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="55376-624">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-624">Add a to-do item</span></span>

<span data-ttu-id="55376-625">[Ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `POST` żądania z elementem zadań do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="55376-625">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="55376-626">`accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych.</span><span class="sxs-lookup"><span data-stu-id="55376-626">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="55376-627">Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="55376-627">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="55376-628">Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="55376-628">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="55376-629">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-629">Update a to-do item</span></span>

<span data-ttu-id="55376-630">Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="55376-630">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="55376-631">`url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="55376-631">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="55376-632">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="55376-632">Delete a to-do item</span></span>

<span data-ttu-id="55376-633">Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="55376-633">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="55376-634">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="55376-634">Additional resources</span></span>

<span data-ttu-id="55376-635">[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="55376-635">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="55376-636">Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="55376-636">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="55376-637">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="55376-637">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="55376-638">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="55376-638">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
