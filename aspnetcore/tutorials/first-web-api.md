---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 3bf930d19684e84365f0ff0255fccd2939fb3f39
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354920"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="1121f-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1121f-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="1121f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="1121f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="1121f-105">W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1121f-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1121f-106">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1121f-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1121f-107">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1121f-107">Create a web API project.</span></span>
> * <span data-ttu-id="1121f-108">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="1121f-109">Tworzy szkielet kontrolera z metodami CRUD.</span><span class="sxs-lookup"><span data-stu-id="1121f-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="1121f-110">Skonfiguruj Routing, ścieżki URL i wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="1121f-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="1121f-111">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="1121f-111">Call the web API with Postman.</span></span>

<span data-ttu-id="1121f-112">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="1121f-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="1121f-113">Overview</span></span>

<span data-ttu-id="1121f-114">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="1121f-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="1121f-115">interfejs API</span><span class="sxs-lookup"><span data-stu-id="1121f-115">API</span></span> | <span data-ttu-id="1121f-116">Opis</span><span class="sxs-lookup"><span data-stu-id="1121f-116">Description</span></span> | <span data-ttu-id="1121f-117">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="1121f-117">Request body</span></span> | <span data-ttu-id="1121f-118">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="1121f-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="1121f-119">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="1121f-119">GET /api/TodoItems</span></span> | <span data-ttu-id="1121f-120">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-120">Get all to-do items</span></span> | <span data-ttu-id="1121f-121">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-121">None</span></span> | <span data-ttu-id="1121f-122">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-122">Array of to-do items</span></span>|
|<span data-ttu-id="1121f-123">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="1121f-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="1121f-124">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="1121f-124">Get an item by ID</span></span> | <span data-ttu-id="1121f-125">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-125">None</span></span> | <span data-ttu-id="1121f-126">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-126">To-do item</span></span>|
|<span data-ttu-id="1121f-127">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="1121f-127">POST /api/TodoItems</span></span> | <span data-ttu-id="1121f-128">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="1121f-128">Add a new item</span></span> | <span data-ttu-id="1121f-129">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-129">To-do item</span></span> | <span data-ttu-id="1121f-130">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-130">To-do item</span></span> |
|<span data-ttu-id="1121f-131">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="1121f-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="1121f-132">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="1121f-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="1121f-133">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-133">To-do item</span></span> | <span data-ttu-id="1121f-134">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-134">None</span></span> |
|<span data-ttu-id="1121f-135">Usuń/api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="1121f-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="1121f-136">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="1121f-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="1121f-137">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-137">None</span></span> | <span data-ttu-id="1121f-138">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-138">None</span></span>|

<span data-ttu-id="1121f-139">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1121f-139">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="1121f-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1121f-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1121f-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1121f-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1121f-148">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="1121f-149">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="1121f-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1121f-151">Z menu **plik** wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="1121f-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1121f-152">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="1121f-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="1121f-153">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1121f-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="1121f-154">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,1** .</span><span class="sxs-lookup"><span data-stu-id="1121f-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="1121f-155">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1121f-155">Select the **API** template and click **Create**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1121f-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1121f-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1121f-158">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="1121f-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="1121f-159">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="1121f-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="1121f-160">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1121f-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="1121f-161">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="1121f-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="1121f-162">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="1121f-162">The preceding commands:</span></span>

  * <span data-ttu-id="1121f-163">Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1121f-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="1121f-164">Dodaje pakiety NuGet, które są wymagane w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1121f-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1121f-165">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1121f-166">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="1121f-166">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="1121f-168">Wybierz pozycję **.NET Core** > **App** > **API** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="1121f-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="1121f-170">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję **docelowa platforma** \* *.NET Core 3,1*.</span><span class="sxs-lookup"><span data-stu-id="1121f-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="1121f-171">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1121f-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="1121f-173">Otwórz Terminal poleceń w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1121f-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="1121f-174">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="1121f-174">Test the API</span></span>

<span data-ttu-id="1121f-175">Szablon projektu umożliwia utworzenie `WeatherForecast` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="1121f-176">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121f-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1121f-178">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121f-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="1121f-179">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="1121f-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="1121f-180">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="1121f-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="1121f-181">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="1121f-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1121f-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1121f-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1121f-183">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121f-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="1121f-184">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="1121f-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1121f-185">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1121f-186">Wybierz pozycję **uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121f-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="1121f-187">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="1121f-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="1121f-188">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="1121f-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="1121f-189">Dołącz `/WeatherForecast` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="1121f-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="1121f-190">Zwracany jest kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="1121f-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="1121f-191">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="1121f-191">Add a model class</span></span>

<span data-ttu-id="1121f-192">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1121f-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="1121f-193">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="1121f-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1121f-195">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="1121f-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="1121f-196">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="1121f-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="1121f-197">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="1121f-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="1121f-198">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="1121f-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1121f-199">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1121f-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="1121f-200">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1121f-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1121f-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1121f-202">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="1121f-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="1121f-203">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1121f-204">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1121f-205">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="1121f-205">Right-click the project.</span></span> <span data-ttu-id="1121f-206">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="1121f-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="1121f-207">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="1121f-207">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="1121f-209">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** > **nowy plik** > **Ogólne** > **pustej klasy**.</span><span class="sxs-lookup"><span data-stu-id="1121f-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="1121f-210">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="1121f-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="1121f-211">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="1121f-212">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="1121f-213">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="1121f-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="1121f-214">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="1121f-214">Add a database context</span></span>

<span data-ttu-id="1121f-215">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1121f-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="1121f-216">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="1121f-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="1121f-218">Dodaj Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="1121f-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="1121f-219">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="1121f-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="1121f-220">Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="1121f-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="1121f-221">W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="1121f-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="1121f-222">Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="1121f-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="1121f-223">Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="1121f-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="1121f-225">Dodawanie kontekstu bazy danych TodoContext</span><span class="sxs-lookup"><span data-stu-id="1121f-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="1121f-226">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="1121f-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1121f-227">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1121f-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1121f-228">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="1121f-229">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="1121f-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="1121f-230">Wprowadź następujący kod:</span><span class="sxs-lookup"><span data-stu-id="1121f-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="1121f-231">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="1121f-231">Register the database context</span></span>

<span data-ttu-id="1121f-232">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="1121f-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1121f-233">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="1121f-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="1121f-234">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="1121f-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="1121f-235">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1121f-235">The preceding code:</span></span>

* <span data-ttu-id="1121f-236">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="1121f-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="1121f-237">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="1121f-238">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="1121f-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="1121f-239">Tworzenie szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="1121f-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1121f-241">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="1121f-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="1121f-242">Wybierz pozycję **dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="1121f-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="1121f-243">Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1121f-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="1121f-244">Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:</span><span class="sxs-lookup"><span data-stu-id="1121f-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="1121f-245">Wybierz pozycję **TodoItem (TodoApi. models)** w **klasie model**.</span><span class="sxs-lookup"><span data-stu-id="1121f-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="1121f-246">W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoApi. models)** .</span><span class="sxs-lookup"><span data-stu-id="1121f-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="1121f-247">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1121f-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1121f-248">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="1121f-249">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1121f-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="1121f-250">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="1121f-250">The preceding commands:</span></span>

* <span data-ttu-id="1121f-251">Dodaj pakiety NuGet wymagane do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="1121f-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="1121f-252">Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="1121f-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="1121f-253">Szkieletuje `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="1121f-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="1121f-254">Wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="1121f-254">The generated code:</span></span>

* <span data-ttu-id="1121f-255">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="1121f-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="1121f-256">Oznacza klasę atrybutem [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="1121f-256">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="1121f-257">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="1121f-258">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="1121f-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="1121f-259">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1121f-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="1121f-260">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="1121f-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="1121f-261">Badanie metody PostTodoItem Create</span><span class="sxs-lookup"><span data-stu-id="1121f-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="1121f-262">Zastąp instrukcję return w `PostTodoItem`, aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="1121f-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="1121f-263">Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="1121f-263">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="1121f-264">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="1121f-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="1121f-265"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Metody:</span><span class="sxs-lookup"><span data-stu-id="1121f-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="1121f-266">W razie powodzenia zwraca kod stanu HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="1121f-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="1121f-267">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="1121f-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="1121f-268">Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1121f-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="1121f-269">Nagłówek `Location` określa [Identyfikator URI](https://developer.mozilla.org/docs/Glossary/URI) nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1121f-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="1121f-270">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="1121f-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="1121f-271">Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`.</span><span class="sxs-lookup"><span data-stu-id="1121f-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="1121f-272">Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="1121f-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="1121f-273">Zainstaluj program Poster</span><span class="sxs-lookup"><span data-stu-id="1121f-273">Install Postman</span></span>

<span data-ttu-id="1121f-274">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="1121f-275">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="1121f-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="1121f-276">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="1121f-276">Start the web app.</span></span>
* <span data-ttu-id="1121f-277">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="1121f-277">Start Postman.</span></span>
* <span data-ttu-id="1121f-278">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="1121f-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="1121f-279">Z **ustawień** > **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="1121f-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="1121f-280">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1121f-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="1121f-281">Test PostTodoItem za pomocą programu Poster</span><span class="sxs-lookup"><span data-stu-id="1121f-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="1121f-282">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="1121f-282">Create a new request.</span></span>
* <span data-ttu-id="1121f-283">Ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="1121f-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="1121f-284">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="1121f-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="1121f-285">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="1121f-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="1121f-286">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="1121f-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="1121f-287">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="1121f-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="1121f-288">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="1121f-288">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="1121f-290">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="1121f-290">Test the location header URI</span></span>

* <span data-ttu-id="1121f-291">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="1121f-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="1121f-292">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="1121f-292">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="1121f-294">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="1121f-294">Set the method to GET.</span></span>
* <span data-ttu-id="1121f-295">Wklej URI (na przykład `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="1121f-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="1121f-296">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="1121f-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="1121f-297">Badanie metod GET</span><span class="sxs-lookup"><span data-stu-id="1121f-297">Examine the GET methods</span></span>

<span data-ttu-id="1121f-298">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="1121f-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="1121f-299">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu.</span><span class="sxs-lookup"><span data-stu-id="1121f-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="1121f-300">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1121f-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="1121f-301">Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="1121f-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="1121f-302">Test get przy użyciu programu Poster</span><span class="sxs-lookup"><span data-stu-id="1121f-302">Test Get with Postman</span></span>

* <span data-ttu-id="1121f-303">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="1121f-303">Create a new request.</span></span>
* <span data-ttu-id="1121f-304">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="1121f-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="1121f-305">Ustaw adres URL żądania `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="1121f-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="1121f-306">Na przykład `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="1121f-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="1121f-307">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="1121f-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="1121f-308">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="1121f-308">Select **Send**.</span></span>

<span data-ttu-id="1121f-309">Ta aplikacja używa bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="1121f-309">This app uses an in-memory database.</span></span> <span data-ttu-id="1121f-310">Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="1121f-311">Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1121f-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="1121f-312">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="1121f-312">Routing and URL paths</span></span>

<span data-ttu-id="1121f-313">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="1121f-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="1121f-314">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1121f-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="1121f-315">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="1121f-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="1121f-316">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="1121f-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="1121f-317">Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="1121f-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="1121f-318">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="1121f-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="1121f-319">Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="1121f-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="1121f-320">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="1121f-320">This sample doesn't use a template.</span></span> <span data-ttu-id="1121f-321">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="1121f-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="1121f-322">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1121f-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="1121f-323">Po wywołaniu `GetTodoItem` wartość `"{id}"` w adresie URL jest podawana do metody w `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="1121f-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="1121f-324">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="1121f-324">Return values</span></span>

<span data-ttu-id="1121f-325">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="1121f-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="1121f-326">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1121f-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="1121f-327">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="1121f-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="1121f-328">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="1121f-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="1121f-329">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1121f-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="1121f-330">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="1121f-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="1121f-331">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="1121f-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="1121f-332">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="1121f-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="1121f-333">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="1121f-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="1121f-334">Metoda PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="1121f-334">The PutTodoItem method</span></span>

<span data-ttu-id="1121f-335">Przeanalizuj metodę `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="1121f-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="1121f-336">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="1121f-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="1121f-337">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="1121f-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="1121f-338">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="1121f-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="1121f-339">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="1121f-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="1121f-340">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="1121f-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="1121f-341">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="1121f-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="1121f-342">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="1121f-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="1121f-343">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="1121f-344">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="1121f-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="1121f-345">Zaktualizuj element do wykonania o IDENTYFIKATORze 1 i ustaw jego nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="1121f-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="1121f-346">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="1121f-346">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="1121f-348">Metoda DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="1121f-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="1121f-349">Przeanalizuj metodę `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="1121f-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="1121f-350">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="1121f-350">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="1121f-351">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="1121f-351">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="1121f-352">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="1121f-352">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="1121f-353">Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="1121f-353">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="1121f-354">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="1121f-354">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="1121f-355">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="1121f-355">Call the web API with JavaScript</span></span>

<span data-ttu-id="1121f-356">Zobacz [Samouczek: wywoływanie interfejsu API sieci web ASP.NET Core przy użyciu języka JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="1121f-356">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1121f-357">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1121f-357">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1121f-358">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1121f-358">Create a web API project.</span></span>
> * <span data-ttu-id="1121f-359">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-359">Add a model class and a database context.</span></span>
> * <span data-ttu-id="1121f-360">Dodawanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1121f-360">Add a controller.</span></span>
> * <span data-ttu-id="1121f-361">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="1121f-361">Add CRUD methods.</span></span>
> * <span data-ttu-id="1121f-362">Konfigurowanie routingu i ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="1121f-362">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="1121f-363">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="1121f-363">Specify return values.</span></span>
> * <span data-ttu-id="1121f-364">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="1121f-364">Call the web API with Postman.</span></span>
> * <span data-ttu-id="1121f-365">Wywołaj interfejs API sieci Web za pomocą języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1121f-365">Call the web API with JavaScript.</span></span>

<span data-ttu-id="1121f-366">Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-366">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="1121f-367">Omówienie</span><span class="sxs-lookup"><span data-stu-id="1121f-367">Overview</span></span>

<span data-ttu-id="1121f-368">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="1121f-368">This tutorial creates the following API:</span></span>

|<span data-ttu-id="1121f-369">interfejs API</span><span class="sxs-lookup"><span data-stu-id="1121f-369">API</span></span> | <span data-ttu-id="1121f-370">Opis</span><span class="sxs-lookup"><span data-stu-id="1121f-370">Description</span></span> | <span data-ttu-id="1121f-371">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="1121f-371">Request body</span></span> | <span data-ttu-id="1121f-372">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="1121f-372">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="1121f-373">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="1121f-373">GET /api/TodoItems</span></span> | <span data-ttu-id="1121f-374">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-374">Get all to-do items</span></span> | <span data-ttu-id="1121f-375">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-375">None</span></span> | <span data-ttu-id="1121f-376">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-376">Array of to-do items</span></span>|
|<span data-ttu-id="1121f-377">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="1121f-377">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="1121f-378">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="1121f-378">Get an item by ID</span></span> | <span data-ttu-id="1121f-379">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-379">None</span></span> | <span data-ttu-id="1121f-380">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-380">To-do item</span></span>|
|<span data-ttu-id="1121f-381">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="1121f-381">POST /api/TodoItems</span></span> | <span data-ttu-id="1121f-382">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="1121f-382">Add a new item</span></span> | <span data-ttu-id="1121f-383">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-383">To-do item</span></span> | <span data-ttu-id="1121f-384">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-384">To-do item</span></span> |
|<span data-ttu-id="1121f-385">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="1121f-385">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="1121f-386">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="1121f-386">Update an existing item &nbsp;</span></span> | <span data-ttu-id="1121f-387">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-387">To-do item</span></span> | <span data-ttu-id="1121f-388">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-388">None</span></span> |
|<span data-ttu-id="1121f-389">Usuń/api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="1121f-389">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="1121f-390">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="1121f-390">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="1121f-391">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-391">None</span></span> | <span data-ttu-id="1121f-392">Brak</span><span class="sxs-lookup"><span data-stu-id="1121f-392">None</span></span>|

<span data-ttu-id="1121f-393">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1121f-393">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="1121f-399">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1121f-399">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-400">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-400">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1121f-401">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1121f-401">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1121f-402">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-402">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="1121f-403">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="1121f-403">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-404">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-404">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1121f-405">Z menu **plik** wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="1121f-405">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1121f-406">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="1121f-406">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="1121f-407">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1121f-407">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="1121f-408">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** .</span><span class="sxs-lookup"><span data-stu-id="1121f-408">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="1121f-409">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1121f-409">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="1121f-410">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="1121f-410">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1121f-412">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1121f-412">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1121f-413">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="1121f-413">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="1121f-414">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="1121f-414">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="1121f-415">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1121f-415">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="1121f-416">Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="1121f-416">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="1121f-417">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="1121f-417">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1121f-418">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-418">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1121f-419">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="1121f-419">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="1121f-421">Wybierz pozycję **.NET Core** > **App** > **API** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="1121f-421">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="1121f-423">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="1121f-423">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="1121f-424">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1121f-424">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="1121f-426">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="1121f-426">Test the API</span></span>

<span data-ttu-id="1121f-427">Szablon projektu umożliwia utworzenie `values` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-427">The project template creates a `values` API.</span></span> <span data-ttu-id="1121f-428">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121f-428">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-429">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-429">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1121f-430">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121f-430">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="1121f-431">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="1121f-431">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="1121f-432">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="1121f-432">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="1121f-433">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="1121f-433">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1121f-434">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1121f-434">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1121f-435">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121f-435">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="1121f-436">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="1121f-436">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1121f-437">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-437">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1121f-438">Wybierz pozycję **uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121f-438">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="1121f-439">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="1121f-439">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="1121f-440">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="1121f-440">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="1121f-441">Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="1121f-441">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="1121f-442">Zwracane są następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="1121f-442">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="1121f-443">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="1121f-443">Add a model class</span></span>

<span data-ttu-id="1121f-444">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1121f-444">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="1121f-445">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="1121f-445">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-446">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-446">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1121f-447">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="1121f-447">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="1121f-448">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="1121f-448">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="1121f-449">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="1121f-449">Name the folder *Models*.</span></span>

* <span data-ttu-id="1121f-450">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="1121f-450">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1121f-451">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1121f-451">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="1121f-452">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-452">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1121f-453">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1121f-453">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1121f-454">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="1121f-454">Add a folder named *Models*.</span></span>

* <span data-ttu-id="1121f-455">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-455">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1121f-456">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-456">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1121f-457">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="1121f-457">Right-click the project.</span></span> <span data-ttu-id="1121f-458">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="1121f-458">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="1121f-459">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="1121f-459">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="1121f-461">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** > **nowy plik** > **Ogólne** > **pustej klasy**.</span><span class="sxs-lookup"><span data-stu-id="1121f-461">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="1121f-462">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="1121f-462">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="1121f-463">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-463">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="1121f-464">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-464">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="1121f-465">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="1121f-465">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="1121f-466">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="1121f-466">Add a database context</span></span>

<span data-ttu-id="1121f-467">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1121f-467">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="1121f-468">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="1121f-468">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-469">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-469">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1121f-470">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="1121f-470">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1121f-471">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1121f-471">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1121f-472">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-472">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="1121f-473">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="1121f-473">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="1121f-474">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-474">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="1121f-475">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="1121f-475">Register the database context</span></span>

<span data-ttu-id="1121f-476">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="1121f-476">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1121f-477">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="1121f-477">The container provides the service to controllers.</span></span>

<span data-ttu-id="1121f-478">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="1121f-478">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="1121f-479">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1121f-479">The preceding code:</span></span>

* <span data-ttu-id="1121f-480">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="1121f-480">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="1121f-481">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-481">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="1121f-482">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="1121f-482">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="1121f-483">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="1121f-483">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-484">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-484">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1121f-485">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="1121f-485">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="1121f-486">Wybierz pozycję **dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="1121f-486">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="1121f-487">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="1121f-487">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="1121f-488">Nazwa klasy *TodoController*i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1121f-488">Name the class *TodoController*, and select **Add**.</span></span>

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1121f-490">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-490">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="1121f-491">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="1121f-491">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="1121f-492">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-492">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="1121f-493">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1121f-493">The preceding code:</span></span>

* <span data-ttu-id="1121f-494">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="1121f-494">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="1121f-495">Oznacza klasę atrybutem [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="1121f-495">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="1121f-496">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-496">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="1121f-497">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="1121f-497">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="1121f-498">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1121f-498">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="1121f-499">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="1121f-499">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="1121f-500">Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta.</span><span class="sxs-lookup"><span data-stu-id="1121f-500">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="1121f-501">Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="1121f-501">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="1121f-502">Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-502">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="1121f-503">Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.</span><span class="sxs-lookup"><span data-stu-id="1121f-503">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="1121f-504">Dodaj metody Get</span><span class="sxs-lookup"><span data-stu-id="1121f-504">Add Get methods</span></span>

<span data-ttu-id="1121f-505">Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="1121f-505">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="1121f-506">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="1121f-506">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="1121f-507">Zatrzymaj aplikację, jeśli jest nadal uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="1121f-507">Stop the app if it's still running.</span></span> <span data-ttu-id="1121f-508">Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.</span><span class="sxs-lookup"><span data-stu-id="1121f-508">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="1121f-509">Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1121f-509">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="1121f-510">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1121f-510">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="1121f-511">Następującą odpowiedź HTTP jest tworzony przez wywołanie metody `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="1121f-511">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="1121f-512">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="1121f-512">Routing and URL paths</span></span>

<span data-ttu-id="1121f-513">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="1121f-513">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="1121f-514">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1121f-514">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="1121f-515">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="1121f-515">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="1121f-516">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="1121f-516">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="1121f-517">W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera, więc nazwa kontrolera jest "todo".</span><span class="sxs-lookup"><span data-stu-id="1121f-517">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="1121f-518">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="1121f-518">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="1121f-519">Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="1121f-519">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="1121f-520">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="1121f-520">This sample doesn't use a template.</span></span> <span data-ttu-id="1121f-521">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="1121f-521">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="1121f-522">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1121f-522">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="1121f-523">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="1121f-523">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="1121f-524">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="1121f-524">Return values</span></span>

<span data-ttu-id="1121f-525">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="1121f-525">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="1121f-526">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1121f-526">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="1121f-527">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="1121f-527">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="1121f-528">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="1121f-528">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="1121f-529">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1121f-529">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="1121f-530">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="1121f-530">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="1121f-531">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="1121f-531">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="1121f-532">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="1121f-532">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="1121f-533">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="1121f-533">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="1121f-534">Metoda GetTodoItems testu</span><span class="sxs-lookup"><span data-stu-id="1121f-534">Test the GetTodoItems method</span></span>

<span data-ttu-id="1121f-535">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-535">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="1121f-536">Zainstaluj program [Poster](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="1121f-536">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="1121f-537">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="1121f-537">Start the web app.</span></span>
* <span data-ttu-id="1121f-538">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="1121f-538">Start Postman.</span></span>
* <span data-ttu-id="1121f-539">Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="1121f-539">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1121f-540">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1121f-540">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1121f-541">Z **ustawień** > **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="1121f-541">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1121f-542">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1121f-542">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="1121f-543">Z poziomu **preferencji** > **Poster** (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="1121f-543">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="1121f-544">Alternatywnie wybierz klucz i wybierz pozycję **Ustawienia**, a następnie wyłącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="1121f-544">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="1121f-545">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1121f-545">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="1121f-546">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="1121f-546">Create a new request.</span></span>
  * <span data-ttu-id="1121f-547">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="1121f-547">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="1121f-548">Ustaw adres URL żądania `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="1121f-548">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="1121f-549">Na przykład `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="1121f-549">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="1121f-550">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="1121f-550">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="1121f-551">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="1121f-551">Select **Send**.</span></span>

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="1121f-553">Dodawanie metody Create</span><span class="sxs-lookup"><span data-stu-id="1121f-553">Add a Create method</span></span>

<span data-ttu-id="1121f-554">Dodaj następującą metodę `PostTodoItem` wewnątrz *kontrolera/TodoController. cs*:</span><span class="sxs-lookup"><span data-stu-id="1121f-554">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="1121f-555">Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="1121f-555">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="1121f-556">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="1121f-556">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="1121f-557">`CreatedAtAction` Metody:</span><span class="sxs-lookup"><span data-stu-id="1121f-557">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="1121f-558">Zwraca kod stanu HTTP 201, jeśli powodzenie.</span><span class="sxs-lookup"><span data-stu-id="1121f-558">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="1121f-559">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="1121f-559">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="1121f-560">Dodaje nagłówek `Location` do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1121f-560">Adds a `Location` header to the response.</span></span> <span data-ttu-id="1121f-561">Nagłówek `Location` określa identyfikator URI nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1121f-561">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="1121f-562">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="1121f-562">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="1121f-563">Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`.</span><span class="sxs-lookup"><span data-stu-id="1121f-563">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="1121f-564">Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="1121f-564">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="1121f-565">Metoda PostTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="1121f-565">Test the PostTodoItem method</span></span>

* <span data-ttu-id="1121f-566">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="1121f-566">Build the project.</span></span>
* <span data-ttu-id="1121f-567">W narzędziu Postman, Ustawia metodę HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="1121f-567">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="1121f-568">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="1121f-568">Select the **Body** tab.</span></span>
* <span data-ttu-id="1121f-569">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="1121f-569">Select the **raw** radio button.</span></span>
* <span data-ttu-id="1121f-570">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="1121f-570">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="1121f-571">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="1121f-571">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="1121f-572">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="1121f-572">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  <span data-ttu-id="1121f-574">Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu metody `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="1121f-574">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="1121f-575">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="1121f-575">Test the location header URI</span></span>

* <span data-ttu-id="1121f-576">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="1121f-576">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="1121f-577">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="1121f-577">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="1121f-579">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="1121f-579">Set the method to GET.</span></span>
* <span data-ttu-id="1121f-580">Wklej URI (na przykład `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="1121f-580">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="1121f-581">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="1121f-581">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="1121f-582">Dodaj metodę PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="1121f-582">Add a PutTodoItem method</span></span>

<span data-ttu-id="1121f-583">Dodaj następujący kod `PutTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="1121f-583">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="1121f-584">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="1121f-584">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="1121f-585">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="1121f-585">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="1121f-586">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="1121f-586">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="1121f-587">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="1121f-587">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="1121f-588">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="1121f-588">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="1121f-589">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="1121f-589">Test the PutTodoItem method</span></span>

<span data-ttu-id="1121f-590">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="1121f-590">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="1121f-591">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1121f-591">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="1121f-592">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="1121f-592">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="1121f-593">Zaktualizuj element zadania do wykonania, który ma identyfikator = 1 i ustaw jego nazwę na "feed ryb":</span><span class="sxs-lookup"><span data-stu-id="1121f-593">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="1121f-594">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="1121f-594">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="1121f-596">Dodaj metodę DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="1121f-596">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="1121f-597">Dodaj następujący kod `DeleteTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="1121f-597">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="1121f-598">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="1121f-598">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="1121f-599">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="1121f-599">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="1121f-600">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="1121f-600">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="1121f-601">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="1121f-601">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="1121f-602">Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="1121f-602">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="1121f-603">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="1121f-603">Select **Send**.</span></span>

<span data-ttu-id="1121f-604">Przykładowa aplikacja umożliwia usunięcie wszystkich elementów.</span><span class="sxs-lookup"><span data-stu-id="1121f-604">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="1121f-605">Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-605">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="1121f-606">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="1121f-606">Call the web API with JavaScript</span></span>

<span data-ttu-id="1121f-607">W tej sekcji zostanie dodana strona HTML, która używa języka JavaScript do wywoływania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-607">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="1121f-608">jQuery inicjuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="1121f-608">jQuery initiates the request.</span></span> <span data-ttu-id="1121f-609">Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-609">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="1121f-610">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-610">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="1121f-611">Tworzenie *wwwroot* folder w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="1121f-611">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="1121f-612">Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="1121f-612">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="1121f-613">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-613">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="1121f-614">Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="1121f-614">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="1121f-615">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1121f-615">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="1121f-616">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="1121f-616">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="1121f-617">Otwórz *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1121f-617">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="1121f-618">Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.</span><span class="sxs-lookup"><span data-stu-id="1121f-618">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="1121f-619">Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-619">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="1121f-620">Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1121f-620">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="1121f-621">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-621">Get a list of to-do items</span></span>

<span data-ttu-id="1121f-622">jQuery wysyła żądanie HTTP GET do internetowego interfejsu API, który zwraca kod JSON reprezentujący tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1121f-622">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="1121f-623">`success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1121f-623">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="1121f-624">Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1121f-624">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="1121f-625">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-625">Add a to-do item</span></span>

<span data-ttu-id="1121f-626">jQuery wysyła żądanie HTTP POST z elementem do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="1121f-626">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="1121f-627">`accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych.</span><span class="sxs-lookup"><span data-stu-id="1121f-627">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="1121f-628">Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="1121f-628">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="1121f-629">Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="1121f-629">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="1121f-630">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-630">Update a to-do item</span></span>

<span data-ttu-id="1121f-631">Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="1121f-631">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="1121f-632">`url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="1121f-632">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="1121f-633">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="1121f-633">Delete a to-do item</span></span>

<span data-ttu-id="1121f-634">Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="1121f-634">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="1121f-635">Dodawanie obsługi uwierzytelniania do internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="1121f-635">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="1121f-636">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1121f-636">Additional resources</span></span>

<span data-ttu-id="1121f-637">[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="1121f-637">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="1121f-638">Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="1121f-638">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="1121f-639">Więcej informacji można znaleźć w następujących zasobach:</span><span class="sxs-lookup"><span data-stu-id="1121f-639">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="1121f-640">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="1121f-640">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
