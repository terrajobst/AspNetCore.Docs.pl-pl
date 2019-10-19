---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/29/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 6f2d62600da828261ecfc3a1df688ce914eccf33
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2019
ms.locfileid: "72590012"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="6a2d5-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a2d5-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="6a2d5-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Jan Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="6a2d5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="6a2d5-105">Ten samouczek uczy się podstaw tworzenia interfejsu API sieci Web za pomocą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6a2d5-106">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6a2d5-107">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-107">Create a web API project.</span></span>
> * <span data-ttu-id="6a2d5-108">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="6a2d5-109">Tworzy szkielet kontrolera z metodami CRUD.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="6a2d5-110">Skonfiguruj Routing, ścieżki URL i wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="6a2d5-111">Wywołaj interfejs API sieci Web za pomocą programu Poster.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-111">Call the web API with Postman.</span></span>

<span data-ttu-id="6a2d5-112">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="6a2d5-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6a2d5-113">Overview</span></span>

<span data-ttu-id="6a2d5-114">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="6a2d5-115">interfejs API</span><span class="sxs-lookup"><span data-stu-id="6a2d5-115">API</span></span> | <span data-ttu-id="6a2d5-116">Opis</span><span class="sxs-lookup"><span data-stu-id="6a2d5-116">Description</span></span> | <span data-ttu-id="6a2d5-117">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-117">Request body</span></span> | <span data-ttu-id="6a2d5-118">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="6a2d5-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="6a2d5-119">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="6a2d5-119">GET /api/TodoItems</span></span> | <span data-ttu-id="6a2d5-120">Pobierz wszystkie elementy do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-120">Get all to-do items</span></span> | <span data-ttu-id="6a2d5-121">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-121">None</span></span> | <span data-ttu-id="6a2d5-122">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-122">Array of to-do items</span></span>|
|<span data-ttu-id="6a2d5-123">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="6a2d5-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="6a2d5-124">Pobieranie elementu według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="6a2d5-124">Get an item by ID</span></span> | <span data-ttu-id="6a2d5-125">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-125">None</span></span> | <span data-ttu-id="6a2d5-126">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-126">To-do item</span></span>|
|<span data-ttu-id="6a2d5-127">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="6a2d5-127">POST /api/TodoItems</span></span> | <span data-ttu-id="6a2d5-128">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="6a2d5-128">Add a new item</span></span> | <span data-ttu-id="6a2d5-129">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-129">To-do item</span></span> | <span data-ttu-id="6a2d5-130">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-130">To-do item</span></span> |
|<span data-ttu-id="6a2d5-131">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="6a2d5-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="6a2d5-132">Aktualizowanie istniejącego elementu &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6a2d5-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="6a2d5-133">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-133">To-do item</span></span> | <span data-ttu-id="6a2d5-134">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-134">None</span></span> |
|<span data-ttu-id="6a2d5-135">Usuń/api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6a2d5-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="6a2d5-136">Usuń element &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6a2d5-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="6a2d5-137">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-137">None</span></span> | <span data-ttu-id="6a2d5-138">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-138">None</span></span>|

<span data-ttu-id="6a2d5-139">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-139">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="6a2d5-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6a2d5-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a2d5-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a2d5-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a2d5-148">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="6a2d5-149">Tworzenie projektu sieci Web</span><span class="sxs-lookup"><span data-stu-id="6a2d5-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a2d5-151">Z menu **plik** wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="6a2d5-152">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="6a2d5-153">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="6a2d5-154">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="6a2d5-155">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-155">Select the **API** template and click **Create**.</span></span>

![Okno dialogowe programu VS New Project](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a2d5-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a2d5-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6a2d5-158">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6a2d5-159">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="6a2d5-160">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="6a2d5-161">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="6a2d5-162">Poprzednie polecenia:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-162">The preceding commands:</span></span>

  * <span data-ttu-id="6a2d5-163">Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="6a2d5-164">Dodaje pakiety NuGet, które są wymagane w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a2d5-165">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6a2d5-166">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-166">Select **File** > **New Solution**.</span></span>

  ![macOS nowe rozwiązanie](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="6a2d5-168">Wybierz pozycję **.NET Core** > **App** > **API** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe nowego projektu macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="6a2d5-170">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję **docelowa platforma** \* *.NET Core 3,0*.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="6a2d5-171">Wprowadź *TodoApi* jako **nazwę projektu** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="6a2d5-173">Otwórz Terminal poleceń w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="6a2d5-174">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="6a2d5-174">Test the API</span></span>

<span data-ttu-id="6a2d5-175">Szablon projektu tworzy interfejs API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="6a2d5-176">Wywołaj metodę `Get` z poziomu przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6a2d5-178">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6a2d5-179">Program Visual Studio uruchamia przeglądarkę i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` to losowo wybierany numer portu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="6a2d5-180">Jeśli zostanie wyświetlone okno dialogowe z pytaniem, czy należy zaufać certyfikatowi IIS Express, wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="6a2d5-181">W wyświetlonym oknie dialogowym **ostrzeżenia o zabezpieczeniach** wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a2d5-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a2d5-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6a2d5-183">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6a2d5-184">W przeglądarce przejdź do następującego adresu URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a2d5-185">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6a2d5-186">Wybierz pozycję **uruchom**  > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="6a2d5-187">Visual Studio dla komputerów Mac uruchamia przeglądarkę i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest losowo wybranym numerem portu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="6a2d5-188">Zwracany jest błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="6a2d5-189">Dołącz `/WeatherForecast` do adresu URL (Zmień adres URL na `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="6a2d5-190">Zwracany jest kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="6a2d5-191">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="6a2d5-191">Add a model class</span></span>

<span data-ttu-id="6a2d5-192">*Model* to zestaw klas, które reprezentują dane zarządzane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="6a2d5-193">Model tej aplikacji jest pojedynczym `TodoItem` klasą.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a2d5-195">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="6a2d5-196">Wybierz pozycję **dodaj**  > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6a2d5-197">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="6a2d5-198">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6a2d5-199">Nadaj klasie nazwę *TodoItem* i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="6a2d5-200">Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a2d5-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a2d5-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6a2d5-202">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="6a2d5-203">Dodaj klasę `TodoItem` do folderu *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a2d5-204">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6a2d5-205">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-205">Right-click the project.</span></span> <span data-ttu-id="6a2d5-206">Wybierz pozycję **dodaj**  > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6a2d5-207">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-207">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="6a2d5-209">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** > **nowy plik** > **Ogólne** > **pustej klasy**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="6a2d5-210">Nazwij klasę *TodoItem*, a następnie kliknij pozycję **New (nowy**).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="6a2d5-211">Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="6a2d5-212">Właściwość `Id` działa jako unikatowy klucz w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="6a2d5-213">Klasy modelu mogą przejść do dowolnego miejsca w projekcie, ale folder *modele* jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="6a2d5-214">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="6a2d5-214">Add a database context</span></span>

<span data-ttu-id="6a2d5-215">*Kontekst bazy danych* jest główną klasą, która koordynuje Entity Framework funkcji dla modelu danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="6a2d5-216">Ta klasa jest tworzona przez wyprowadzanie z klasy `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="6a2d5-218">Dodaj Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="6a2d5-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="6a2d5-219">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="6a2d5-220">Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="6a2d5-221">W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="6a2d5-222">Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="6a2d5-223">Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="6a2d5-225">Dodawanie kontekstu bazy danych TodoContext</span><span class="sxs-lookup"><span data-stu-id="6a2d5-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="6a2d5-226">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6a2d5-227">Nadaj klasie nazwę *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a2d5-228">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6a2d5-229">Dodaj klasę `TodoContext` do folderu *models* .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="6a2d5-230">Wprowadź następujący kod:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="6a2d5-231">Rejestrowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="6a2d5-231">Register the database context</span></span>

<span data-ttu-id="6a2d5-232">W ASP.NET Core usługi, takie jak kontekst bazy danych, muszą być zarejestrowane z kontenerem [iniekcji zależności (di)](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6a2d5-233">Kontener udostępnia usługę kontrolerom.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="6a2d5-234">Zaktualizuj *Startup.cs* o następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="6a2d5-235">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-235">The preceding code:</span></span>

* <span data-ttu-id="6a2d5-236">Usuwa nieużywane deklaracje `using`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="6a2d5-237">Dodaje kontekst bazy danych do kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="6a2d5-238">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="6a2d5-239">Tworzenie szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="6a2d5-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a2d5-241">Kliknij prawym przyciskiem myszy folder *controllers* .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="6a2d5-242">Wybierz pozycję **dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="6a2d5-243">Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="6a2d5-244">Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="6a2d5-245">Wybierz pozycję **TodoItem (TodoApi. models)** w **klasie model**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="6a2d5-246">W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoApi. models)** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="6a2d5-247">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a2d5-248">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6a2d5-249">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="6a2d5-250">Poprzednie polecenia:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-250">The preceding commands:</span></span>

* <span data-ttu-id="6a2d5-251">Dodaj pakiety NuGet wymagane do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="6a2d5-252">Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="6a2d5-253">Szkieletuje `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="6a2d5-254">Wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-254">The generated code:</span></span>

* <span data-ttu-id="6a2d5-255">Definiuje klasę kontrolera interfejsu API bez metod.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="6a2d5-256">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-256">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="6a2d5-257">Ten atrybut wskazuje, że kontroler odpowiada na żądania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="6a2d5-258">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="6a2d5-259">Używa funkcji DI do iniekcji kontekstu bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="6a2d5-260">Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="6a2d5-261">Badanie metody PostTodoItem Create</span><span class="sxs-lookup"><span data-stu-id="6a2d5-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="6a2d5-262">Zastąp instrukcję return w `PostTodoItem`, aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="6a2d5-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="6a2d5-263">Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [[HTTPPOST]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="6a2d5-264">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="6a2d5-265">Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="6a2d5-266">W razie powodzenia zwraca kod stanu HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="6a2d5-267">HTTP 201 to standardowa odpowiedź dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="6a2d5-268">Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="6a2d5-269">Nagłówek `Location` określa [Identyfikator URI](https://developer.mozilla.org/docs/Glossary/URI) nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="6a2d5-270">Aby uzyskać więcej informacji, zobacz [10.2.2 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="6a2d5-271">Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="6a2d5-272">Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="6a2d5-273">Zainstaluj program Poster</span><span class="sxs-lookup"><span data-stu-id="6a2d5-273">Install Postman</span></span>

<span data-ttu-id="6a2d5-274">Ten samouczek używa programu do testowania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="6a2d5-275">Zainstaluj program [Poster](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="6a2d5-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="6a2d5-276">Uruchom aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-276">Start the web app.</span></span>
* <span data-ttu-id="6a2d5-277">Uruchom wpis.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-277">Start Postman.</span></span>
* <span data-ttu-id="6a2d5-278">Wyłącz **weryfikację certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="6a2d5-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="6a2d5-279">Z **ustawień** > **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="6a2d5-280">Po przetestowaniu kontrolera ponownie Włącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="6a2d5-281">Test PostTodoItem za pomocą programu Poster</span><span class="sxs-lookup"><span data-stu-id="6a2d5-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="6a2d5-282">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-282">Create a new request.</span></span>
* <span data-ttu-id="6a2d5-283">Ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="6a2d5-284">Wybierz kartę **treść** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="6a2d5-285">Wybierz przycisk radiowy **RAW** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="6a2d5-286">Ustaw typ na **JSON (Application/JSON)** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="6a2d5-287">W treści żądania wprowadź kod JSON dla elementu do wykonania:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="6a2d5-288">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-288">Select **Send**.</span></span>

  ![Ogłoś przy użyciu żądania Create](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="6a2d5-290">Testowanie identyfikatora URI nagłówka lokalizacji</span><span class="sxs-lookup"><span data-stu-id="6a2d5-290">Test the location header URI</span></span>

* <span data-ttu-id="6a2d5-291">Wybierz kartę **nagłówki** w okienku **odpowiedź** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="6a2d5-292">Skopiuj wartość nagłówka **lokalizacji** :</span><span class="sxs-lookup"><span data-stu-id="6a2d5-292">Copy the **Location** header value:</span></span>

  ![Karta nagłówki w konsoli programu Poster](first-web-api/_static/3/create.png)

* <span data-ttu-id="6a2d5-294">Ustaw metodę, aby uzyskać.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-294">Set the method to GET.</span></span>
* <span data-ttu-id="6a2d5-295">Wklej URI (na przykład `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="6a2d5-296">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="6a2d5-297">Badanie metod GET</span><span class="sxs-lookup"><span data-stu-id="6a2d5-297">Examine the GET methods</span></span>

<span data-ttu-id="6a2d5-298">Te metody implementują dwa punkty końcowe GET:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="6a2d5-299">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="6a2d5-300">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="6a2d5-301">Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="6a2d5-302">Test get przy użyciu programu Poster</span><span class="sxs-lookup"><span data-stu-id="6a2d5-302">Test Get with Postman</span></span>

* <span data-ttu-id="6a2d5-303">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-303">Create a new request.</span></span>
* <span data-ttu-id="6a2d5-304">Ustaw metodę HTTP, aby **uzyskać**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="6a2d5-305">Ustaw adres URL żądania na `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="6a2d5-306">Na przykład `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="6a2d5-307">Ustaw **dwa widoki okienka** w programie Poster.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="6a2d5-308">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-308">Select **Send**.</span></span>

<span data-ttu-id="6a2d5-309">Ta aplikacja używa bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-309">This app uses an in-memory database.</span></span> <span data-ttu-id="6a2d5-310">Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="6a2d5-311">Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="6a2d5-312">Ścieżki routingu i adresów URL</span><span class="sxs-lookup"><span data-stu-id="6a2d5-312">Routing and URL paths</span></span>

<span data-ttu-id="6a2d5-313">Atrybut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) oznacza metodę, która reaguje na żądanie HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="6a2d5-314">Ścieżka adresu URL dla każdej metody jest zbudowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="6a2d5-315">Rozpocznij od ciągu szablonu w atrybucie `Route` kontrolera:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="6a2d5-316">Zastąp `[controller]` nazwą kontrolera, którą Konwencją jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="6a2d5-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="6a2d5-317">Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="6a2d5-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="6a2d5-318">W ASP.NET Core [routingu](xref:mvc/controllers/routing) jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="6a2d5-319">Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="6a2d5-320">Ten przykład nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-320">This sample doesn't use a template.</span></span> <span data-ttu-id="6a2d5-321">Aby uzyskać więcej informacji, zobacz temat [Routing atrybutów z atrybutami http [Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="6a2d5-322">W poniższej metodzie `GetTodoItem` `"{id}"` jest zmienną zastępczą dla unikatowego identyfikatora elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="6a2d5-323">Po wywołaniu `GetTodoItem` wartość `"{id}"` w adresie URL jest podawana do metody w `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="6a2d5-324">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="6a2d5-324">Return values</span></span>

<span data-ttu-id="6a2d5-325">Typem zwracanym `GetTodoItems` i `GetTodoItem` Metoda jest [ActionResult \<T > typ](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="6a2d5-326">ASP.NET Core automatycznie serializować obiektu do [formatu JSON](https://www.json.org/) i zapisuje kod JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="6a2d5-327">Kod odpowiedzi dla tego typu zwracanego to 200, przy założeniu, że nie istnieją Nieobsłużone wyjątki.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="6a2d5-328">Nieobsłużone wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="6a2d5-329">`ActionResult` zwracane typy mogą reprezentować szeroką gamę kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="6a2d5-330">Na przykład `GetTodoItem` mogą zwracać dwie różne wartości stanu:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="6a2d5-331">Jeśli żaden element nie jest zgodny z żądanym IDENTYFIKATORem, metoda zwraca 404 kod błędu [NOTFOUND](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="6a2d5-332">W przeciwnym razie metoda zwraca 200 z treścią odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="6a2d5-333">Zwracanie `item` wyników w odpowiedzi HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="6a2d5-334">Metoda PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-334">The PutTodoItem method</span></span>

<span data-ttu-id="6a2d5-335">Przeanalizuj metodę `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="6a2d5-336">`PutTodoItem` jest podobna do `PostTodoItem`, z tą różnicą, że używa protokołu HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="6a2d5-337">Odpowiedź to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="6a2d5-338">Zgodnie ze specyfikacją protokołu HTTP żądanie PUT wymaga, aby klient wysłał całą zaktualizowaną jednostkę, a nie tylko te zmiany.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="6a2d5-339">Aby zapewnić obsługę częściowych aktualizacji, użyj [poprawki http](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="6a2d5-340">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="6a2d5-341">Testowanie metody PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="6a2d5-342">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="6a2d5-343">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="6a2d5-344">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="6a2d5-345">Zaktualizuj element do wykonania o IDENTYFIKATORze 1 i ustaw jego nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="6a2d5-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="6a2d5-346">Na poniższej ilustracji przedstawiono aktualizację programu Poster:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-346">The following image shows the Postman update:</span></span>

![Konsola programu Poster pokazująca odpowiedź 204 (brak zawartości)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="6a2d5-348">Metoda DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="6a2d5-349">Przeanalizuj metodę `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="6a2d5-350">Odpowiedź `DeleteTodoItem` to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-350">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="6a2d5-351">Testowanie metody DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-351">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="6a2d5-352">Użyj programu Poster, aby usunąć element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-352">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="6a2d5-353">Ustaw metodę na `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-353">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="6a2d5-354">Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-354">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="6a2d5-355">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-355">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="6a2d5-356">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a2d5-356">Call the web API with JavaScript</span></span>

<span data-ttu-id="6a2d5-357">Zobacz [Samouczek: wywoływanie interfejsu API sieci web ASP.NET Core przy użyciu języka JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-357">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6a2d5-358">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-358">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6a2d5-359">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-359">Create a web API project.</span></span>
> * <span data-ttu-id="6a2d5-360">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-360">Add a model class and a database context.</span></span>
> * <span data-ttu-id="6a2d5-361">Dodaj kontroler.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-361">Add a controller.</span></span>
> * <span data-ttu-id="6a2d5-362">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-362">Add CRUD methods.</span></span>
> * <span data-ttu-id="6a2d5-363">Skonfiguruj ścieżki routingu i adresów URL.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-363">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="6a2d5-364">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-364">Specify return values.</span></span>
> * <span data-ttu-id="6a2d5-365">Wywołaj interfejs API sieci Web za pomocą programu Poster.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-365">Call the web API with Postman.</span></span>
> * <span data-ttu-id="6a2d5-366">Wywołaj interfejs API sieci Web za pomocą języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-366">Call the web API with JavaScript.</span></span>

<span data-ttu-id="6a2d5-367">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-367">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="6a2d5-368">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6a2d5-368">Overview</span></span>

<span data-ttu-id="6a2d5-369">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-369">This tutorial creates the following API:</span></span>

|<span data-ttu-id="6a2d5-370">interfejs API</span><span class="sxs-lookup"><span data-stu-id="6a2d5-370">API</span></span> | <span data-ttu-id="6a2d5-371">Opis</span><span class="sxs-lookup"><span data-stu-id="6a2d5-371">Description</span></span> | <span data-ttu-id="6a2d5-372">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-372">Request body</span></span> | <span data-ttu-id="6a2d5-373">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="6a2d5-373">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="6a2d5-374">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="6a2d5-374">GET /api/TodoItems</span></span> | <span data-ttu-id="6a2d5-375">Pobierz wszystkie elementy do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-375">Get all to-do items</span></span> | <span data-ttu-id="6a2d5-376">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-376">None</span></span> | <span data-ttu-id="6a2d5-377">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-377">Array of to-do items</span></span>|
|<span data-ttu-id="6a2d5-378">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="6a2d5-378">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="6a2d5-379">Pobieranie elementu według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="6a2d5-379">Get an item by ID</span></span> | <span data-ttu-id="6a2d5-380">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-380">None</span></span> | <span data-ttu-id="6a2d5-381">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-381">To-do item</span></span>|
|<span data-ttu-id="6a2d5-382">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="6a2d5-382">POST /api/TodoItems</span></span> | <span data-ttu-id="6a2d5-383">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="6a2d5-383">Add a new item</span></span> | <span data-ttu-id="6a2d5-384">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-384">To-do item</span></span> | <span data-ttu-id="6a2d5-385">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-385">To-do item</span></span> |
|<span data-ttu-id="6a2d5-386">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="6a2d5-386">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="6a2d5-387">Aktualizowanie istniejącego elementu &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6a2d5-387">Update an existing item &nbsp;</span></span> | <span data-ttu-id="6a2d5-388">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-388">To-do item</span></span> | <span data-ttu-id="6a2d5-389">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-389">None</span></span> |
|<span data-ttu-id="6a2d5-390">Usuń/api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6a2d5-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="6a2d5-391">Usuń element &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6a2d5-391">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="6a2d5-392">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-392">None</span></span> | <span data-ttu-id="6a2d5-393">Brak</span><span class="sxs-lookup"><span data-stu-id="6a2d5-393">None</span></span>|

<span data-ttu-id="6a2d5-394">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-394">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="6a2d5-400">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6a2d5-400">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-401">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-401">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a2d5-402">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a2d5-402">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a2d5-403">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-403">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="6a2d5-404">Tworzenie projektu sieci Web</span><span class="sxs-lookup"><span data-stu-id="6a2d5-404">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-405">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-405">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a2d5-406">Z menu **plik** wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-406">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="6a2d5-407">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-407">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="6a2d5-408">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-408">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="6a2d5-409">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-409">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="6a2d5-410">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-410">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="6a2d5-411">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-411">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS New Project](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a2d5-413">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a2d5-413">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6a2d5-414">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-414">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6a2d5-415">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-415">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="6a2d5-416">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-416">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="6a2d5-417">Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-417">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="6a2d5-418">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-418">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a2d5-419">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-419">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6a2d5-420">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-420">Select **File** > **New Solution**.</span></span>

  ![macOS nowe rozwiązanie](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="6a2d5-422">Wybierz pozycję **.NET Core** > **App** > **API** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-422">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe nowego projektu macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="6a2d5-424">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** zaakceptuj domyślną **platformę docelową** programu \* *.NET Core 2,2*.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-424">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="6a2d5-425">Wprowadź *TodoApi* jako **nazwę projektu** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-425">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="6a2d5-427">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="6a2d5-427">Test the API</span></span>

<span data-ttu-id="6a2d5-428">Szablon projektu tworzy interfejs API `values`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-428">The project template creates a `values` API.</span></span> <span data-ttu-id="6a2d5-429">Wywołaj metodę `Get` z poziomu przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-429">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-430">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-430">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6a2d5-431">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-431">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6a2d5-432">Program Visual Studio uruchamia przeglądarkę i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` to losowo wybierany numer portu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-432">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="6a2d5-433">Jeśli zostanie wyświetlone okno dialogowe z pytaniem, czy należy zaufać certyfikatowi IIS Express, wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-433">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="6a2d5-434">W wyświetlonym oknie dialogowym **ostrzeżenia o zabezpieczeniach** wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-434">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a2d5-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a2d5-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6a2d5-436">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-436">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6a2d5-437">W przeglądarce przejdź do następującego adresu URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-437">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a2d5-438">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-438">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6a2d5-439">Wybierz pozycję **uruchom**  > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-439">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="6a2d5-440">Visual Studio dla komputerów Mac uruchamia przeglądarkę i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest losowo wybranym numerem portu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-440">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="6a2d5-441">Zwracany jest błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-441">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="6a2d5-442">Dołącz `/api/values` do adresu URL (Zmień adres URL na `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-442">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="6a2d5-443">Zwracany jest następujący kod JSON:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-443">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="6a2d5-444">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="6a2d5-444">Add a model class</span></span>

<span data-ttu-id="6a2d5-445">*Model* to zestaw klas, które reprezentują dane zarządzane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-445">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="6a2d5-446">Model tej aplikacji jest pojedynczym `TodoItem` klasą.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-446">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-447">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-447">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a2d5-448">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-448">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="6a2d5-449">Wybierz pozycję **dodaj**  > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-449">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6a2d5-450">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-450">Name the folder *Models*.</span></span>

* <span data-ttu-id="6a2d5-451">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-451">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6a2d5-452">Nadaj klasie nazwę *TodoItem* i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-452">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="6a2d5-453">Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-453">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a2d5-454">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a2d5-454">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6a2d5-455">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-455">Add a folder named *Models*.</span></span>

* <span data-ttu-id="6a2d5-456">Dodaj klasę `TodoItem` do folderu *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-456">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a2d5-457">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-457">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6a2d5-458">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-458">Right-click the project.</span></span> <span data-ttu-id="6a2d5-459">Wybierz pozycję **dodaj**  > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-459">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6a2d5-460">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-460">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="6a2d5-462">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** > **nowy plik** > **Ogólne** > **pustej klasy**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-462">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="6a2d5-463">Nazwij klasę *TodoItem*, a następnie kliknij pozycję **New (nowy**).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-463">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="6a2d5-464">Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-464">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="6a2d5-465">Właściwość `Id` działa jako unikatowy klucz w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-465">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="6a2d5-466">Klasy modelu mogą przejść do dowolnego miejsca w projekcie, ale folder *modele* jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-466">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="6a2d5-467">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="6a2d5-467">Add a database context</span></span>

<span data-ttu-id="6a2d5-468">*Kontekst bazy danych* jest główną klasą, która koordynuje Entity Framework funkcji dla modelu danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-468">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="6a2d5-469">Ta klasa jest tworzona przez wyprowadzanie z klasy `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-469">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-470">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-470">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a2d5-471">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-471">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6a2d5-472">Nadaj klasie nazwę *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-472">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a2d5-473">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-473">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6a2d5-474">Dodaj klasę `TodoContext` do folderu *models* .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-474">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="6a2d5-475">Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-475">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="6a2d5-476">Rejestrowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="6a2d5-476">Register the database context</span></span>

<span data-ttu-id="6a2d5-477">W ASP.NET Core usługi, takie jak kontekst bazy danych, muszą być zarejestrowane z kontenerem [iniekcji zależności (di)](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-477">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6a2d5-478">Kontener udostępnia usługę kontrolerom.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-478">The container provides the service to controllers.</span></span>

<span data-ttu-id="6a2d5-479">Zaktualizuj *Startup.cs* o następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-479">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="6a2d5-480">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-480">The preceding code:</span></span>

* <span data-ttu-id="6a2d5-481">Usuwa nieużywane deklaracje `using`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-481">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="6a2d5-482">Dodaje kontekst bazy danych do kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-482">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="6a2d5-483">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-483">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="6a2d5-484">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="6a2d5-484">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-485">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-485">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a2d5-486">Kliknij prawym przyciskiem myszy folder *controllers* .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-486">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="6a2d5-487">Wybierz pozycję **dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-487">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="6a2d5-488">W oknie dialogowym **Dodaj nowy element** wybierz szablon **Klasa kontrolera interfejsu API** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-488">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="6a2d5-489">Nadaj klasie nazwę *TodoController*i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-489">Name the class *TodoController*, and select **Add**.</span></span>

  ![Okno dialogowe Dodawanie nowego elementu z kontrolerem w polu wyszukiwania i wybranym kontrolerem interfejsu API sieci Web](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a2d5-491">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-491">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6a2d5-492">W folderze *controllers* Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-492">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="6a2d5-493">Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-493">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="6a2d5-494">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-494">The preceding code:</span></span>

* <span data-ttu-id="6a2d5-495">Definiuje klasę kontrolera interfejsu API bez metod.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-495">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="6a2d5-496">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-496">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="6a2d5-497">Ten atrybut wskazuje, że kontroler odpowiada na żądania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-497">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="6a2d5-498">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-498">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="6a2d5-499">Używa funkcji DI do iniekcji kontekstu bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-499">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="6a2d5-500">Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-500">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="6a2d5-501">Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-501">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="6a2d5-502">Ten kod znajduje się w konstruktorze, więc jest uruchamiany za każdym razem, gdy istnieje nowe żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-502">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="6a2d5-503">Jeśli usuniesz wszystkie elementy, Konstruktor utworzy `Item1` ponownie przy następnym wywołaniu metody interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-503">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="6a2d5-504">Dlatego może wyglądać podobnie jak usunięcie nie zadziałało, gdy rzeczywiście zadziałało.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-504">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="6a2d5-505">Dodawanie metod get</span><span class="sxs-lookup"><span data-stu-id="6a2d5-505">Add Get methods</span></span>

<span data-ttu-id="6a2d5-506">Aby udostępnić interfejs API, który pobiera elementy do wykonania, Dodaj następujące metody do klasy `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-506">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="6a2d5-507">Te metody implementują dwa punkty końcowe GET:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-507">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="6a2d5-508">Zatrzymaj aplikację, jeśli jest nadal uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-508">Stop the app if it's still running.</span></span> <span data-ttu-id="6a2d5-509">Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-509">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="6a2d5-510">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-510">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="6a2d5-511">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-511">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="6a2d5-512">Wywołanie do `GetTodoItems` jest generowane w następującej odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-512">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="6a2d5-513">Ścieżki routingu i adresów URL</span><span class="sxs-lookup"><span data-stu-id="6a2d5-513">Routing and URL paths</span></span>

<span data-ttu-id="6a2d5-514">Atrybut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) oznacza metodę, która reaguje na żądanie HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-514">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="6a2d5-515">Ścieżka adresu URL dla każdej metody jest zbudowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-515">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="6a2d5-516">Rozpocznij od ciągu szablonu w atrybucie `Route` kontrolera:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-516">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="6a2d5-517">Zastąp `[controller]` nazwą kontrolera, którą Konwencją jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="6a2d5-517">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="6a2d5-518">W przypadku tego przykładu nazwa klasy kontrolera to kontroler do **zrobienia**, więc nazwa kontrolera to "do zrobienia".</span><span class="sxs-lookup"><span data-stu-id="6a2d5-518">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="6a2d5-519">W ASP.NET Core [routingu](xref:mvc/controllers/routing) jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-519">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="6a2d5-520">Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-520">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="6a2d5-521">Ten przykład nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-521">This sample doesn't use a template.</span></span> <span data-ttu-id="6a2d5-522">Aby uzyskać więcej informacji, zobacz temat [Routing atrybutów z atrybutami http [Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-522">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="6a2d5-523">W poniższej metodzie `GetTodoItem` `"{id}"` jest zmienną zastępczą dla unikatowego identyfikatora elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-523">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="6a2d5-524">Po wywołaniu `GetTodoItem` wartość `"{id}"` w adresie URL jest podawana do metody w `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-524">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="6a2d5-525">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="6a2d5-525">Return values</span></span>

<span data-ttu-id="6a2d5-526">Typem zwracanym `GetTodoItems` i `GetTodoItem` Metoda jest [ActionResult \<T > typ](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-526">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="6a2d5-527">ASP.NET Core automatycznie serializować obiektu do [formatu JSON](https://www.json.org/) i zapisuje kod JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-527">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="6a2d5-528">Kod odpowiedzi dla tego typu zwracanego to 200, przy założeniu, że nie istnieją Nieobsłużone wyjątki.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-528">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="6a2d5-529">Nieobsłużone wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-529">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="6a2d5-530">`ActionResult` zwracane typy mogą reprezentować szeroką gamę kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-530">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="6a2d5-531">Na przykład `GetTodoItem` mogą zwracać dwie różne wartości stanu:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-531">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="6a2d5-532">Jeśli żaden element nie jest zgodny z żądanym IDENTYFIKATORem, metoda zwraca 404 kod błędu [NOTFOUND](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-532">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="6a2d5-533">W przeciwnym razie metoda zwraca 200 z treścią odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-533">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="6a2d5-534">Zwracanie `item` wyników w odpowiedzi HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-534">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="6a2d5-535">Testowanie metody GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="6a2d5-535">Test the GetTodoItems method</span></span>

<span data-ttu-id="6a2d5-536">Ten samouczek używa programu do testowania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-536">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="6a2d5-537">Zainstaluj program [Poster](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-537">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="6a2d5-538">Uruchom aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-538">Start the web app.</span></span>
* <span data-ttu-id="6a2d5-539">Uruchom wpis.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-539">Start Postman.</span></span>
* <span data-ttu-id="6a2d5-540">Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-540">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a2d5-541">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a2d5-541">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a2d5-542">Z **ustawień** > **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-542">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a2d5-543">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6a2d5-543">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6a2d5-544">Z poziomu**preferencji**  >  **Poster** (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-544">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="6a2d5-545">Alternatywnie wybierz klucz i wybierz pozycję **Ustawienia**, a następnie wyłącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-545">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="6a2d5-546">Po przetestowaniu kontrolera ponownie Włącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-546">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="6a2d5-547">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-547">Create a new request.</span></span>
  * <span data-ttu-id="6a2d5-548">Ustaw metodę HTTP, aby **uzyskać**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-548">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="6a2d5-549">Ustaw adres URL żądania na `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-549">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="6a2d5-550">Na przykład `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-550">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="6a2d5-551">Ustaw **dwa widoki okienka** w programie Poster.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-551">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="6a2d5-552">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-552">Select **Send**.</span></span>

![Ogłoś przy użyciu żądania GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="6a2d5-554">Dodawanie metody Create</span><span class="sxs-lookup"><span data-stu-id="6a2d5-554">Add a Create method</span></span>

<span data-ttu-id="6a2d5-555">Dodaj następującą metodę `PostTodoItem` wewnątrz *kontrolera/TodoController. cs*:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-555">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="6a2d5-556">Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [[HTTPPOST]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-556">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="6a2d5-557">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-557">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="6a2d5-558">Metoda `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-558">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="6a2d5-559">Zwraca kod stanu HTTP 201, jeśli powodzenie.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-559">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="6a2d5-560">HTTP 201 to standardowa odpowiedź dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-560">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="6a2d5-561">Dodaje nagłówek `Location` do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-561">Adds a `Location` header to the response.</span></span> <span data-ttu-id="6a2d5-562">Nagłówek `Location` określa identyfikator URI nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-562">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="6a2d5-563">Aby uzyskać więcej informacji, zobacz [10.2.2 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-563">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="6a2d5-564">Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-564">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="6a2d5-565">Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-565">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="6a2d5-566">Testowanie metody PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-566">Test the PostTodoItem method</span></span>

* <span data-ttu-id="6a2d5-567">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-567">Build the project.</span></span>
* <span data-ttu-id="6a2d5-568">W polu Poster ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-568">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="6a2d5-569">Wybierz kartę **treść** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-569">Select the **Body** tab.</span></span>
* <span data-ttu-id="6a2d5-570">Wybierz przycisk radiowy **RAW** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-570">Select the **raw** radio button.</span></span>
* <span data-ttu-id="6a2d5-571">Ustaw typ na **JSON (Application/JSON)** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-571">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="6a2d5-572">W treści żądania wprowadź kod JSON dla elementu do wykonania:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-572">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="6a2d5-573">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-573">Select **Send**.</span></span>

  ![Ogłoś przy użyciu żądania Create](first-web-api/_static/create.png)

  <span data-ttu-id="6a2d5-575">Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu metody `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-575">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="6a2d5-576">Testowanie identyfikatora URI nagłówka lokalizacji</span><span class="sxs-lookup"><span data-stu-id="6a2d5-576">Test the location header URI</span></span>

* <span data-ttu-id="6a2d5-577">Wybierz kartę **nagłówki** w okienku **odpowiedź** .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-577">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="6a2d5-578">Skopiuj wartość nagłówka **lokalizacji** :</span><span class="sxs-lookup"><span data-stu-id="6a2d5-578">Copy the **Location** header value:</span></span>

  ![Karta nagłówki w konsoli programu Poster](first-web-api/_static/pmc2.png)

* <span data-ttu-id="6a2d5-580">Ustaw metodę, aby uzyskać.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-580">Set the method to GET.</span></span>
* <span data-ttu-id="6a2d5-581">Wklej URI (na przykład `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-581">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="6a2d5-582">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-582">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="6a2d5-583">Dodawanie metody PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-583">Add a PutTodoItem method</span></span>

<span data-ttu-id="6a2d5-584">Dodaj następującą metodę `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-584">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="6a2d5-585">`PutTodoItem` jest podobna do `PostTodoItem`, z tą różnicą, że używa protokołu HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-585">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="6a2d5-586">Odpowiedź to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-586">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="6a2d5-587">Zgodnie ze specyfikacją protokołu HTTP żądanie PUT wymaga, aby klient wysłał całą zaktualizowaną jednostkę, a nie tylko te zmiany.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-587">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="6a2d5-588">Aby zapewnić obsługę częściowych aktualizacji, użyj [poprawki http](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-588">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="6a2d5-589">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-589">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="6a2d5-590">Testowanie metody PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-590">Test the PutTodoItem method</span></span>

<span data-ttu-id="6a2d5-591">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-591">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="6a2d5-592">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-592">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="6a2d5-593">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-593">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="6a2d5-594">Zaktualizuj element do wykonania o identyfikatorze 1 i ustaw jego nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="6a2d5-594">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="6a2d5-595">Na poniższej ilustracji przedstawiono aktualizację programu Poster:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-595">The following image shows the Postman update:</span></span>

![Konsola programu Poster pokazująca odpowiedź 204 (brak zawartości)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="6a2d5-597">Dodawanie metody DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-597">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="6a2d5-598">Dodaj następującą metodę `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-598">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="6a2d5-599">Odpowiedź `DeleteTodoItem` to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-599">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="6a2d5-600">Testowanie metody DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6a2d5-600">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="6a2d5-601">Użyj programu Poster, aby usunąć element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-601">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="6a2d5-602">Ustaw metodę na `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-602">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="6a2d5-603">Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-603">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="6a2d5-604">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-604">Select **Send**.</span></span>

<span data-ttu-id="6a2d5-605">Przykładowa aplikacja umożliwia usunięcie wszystkich elementów.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-605">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="6a2d5-606">Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-606">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="6a2d5-607">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a2d5-607">Call the web API with JavaScript</span></span>

<span data-ttu-id="6a2d5-608">W tej sekcji zostanie dodana strona HTML, która używa języka JavaScript do wywoływania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-608">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="6a2d5-609">jQuery inicjuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-609">jQuery initiates the request.</span></span> <span data-ttu-id="6a2d5-610">Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-610">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="6a2d5-611">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="6a2d5-612">Utwórz folder *wwwroot* w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="6a2d5-613">Dodaj plik HTML o nazwie *index. html* do katalogu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="6a2d5-614">Zastąp jego zawartość następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="6a2d5-615">Dodaj plik języka JavaScript o nazwie *site. js* do katalogu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="6a2d5-616">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="6a2d5-617">Zmiana ustawień uruchamiania projektu ASP.NET Core może być wymagana do lokalnego przetestowania strony HTML:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="6a2d5-618">Otwórz *Properties\launchSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="6a2d5-619">Usuń właściwość `launchUrl`, aby wymusić, że aplikacja zostanie otwarta w pliku *index. html* &mdash;the domyślny plik projektu.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="6a2d5-620">Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-620">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="6a2d5-621">Poniżej znajdują się wyjaśnienia wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-621">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="6a2d5-622">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-622">Get a list of to-do items</span></span>

<span data-ttu-id="6a2d5-623">jQuery wysyła żądanie HTTP GET do internetowego interfejsu API, który zwraca kod JSON reprezentujący tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-623">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="6a2d5-624">Funkcja wywołania zwrotnego `success` jest wywoływana, jeśli żądanie zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-624">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="6a2d5-625">W wywołaniu zwrotnym model DOM jest aktualizowany przy użyciu informacji o tym do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-625">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="6a2d5-626">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-626">Add a to-do item</span></span>

<span data-ttu-id="6a2d5-627">jQuery wysyła żądanie HTTP POST z elementem do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-627">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="6a2d5-628">Opcje `accepts` i `contentType` są ustawione na `application/json`, aby określić typ nośnika, który odbiera i wysyła.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-628">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="6a2d5-629">Element do wykonania jest konwertowany na format JSON przy użyciu [formatu JSON. stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-629">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="6a2d5-630">Gdy interfejs API zwraca kod stanu pomyślnego, funkcja `getData` jest wywoływana w celu zaktualizowania tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-630">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="6a2d5-631">Aktualizowanie elementu do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-631">Update a to-do item</span></span>

<span data-ttu-id="6a2d5-632">Aktualizowanie elementu do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-632">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="6a2d5-633">@No__t_0 zmieni się, aby dodać unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-633">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="6a2d5-634">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="6a2d5-634">Delete a to-do item</span></span>

<span data-ttu-id="6a2d5-635">Usuwanie elementu do wykonania jest realizowane przez ustawienie `type` w wywołaniu AJAX, aby `DELETE` i określić unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="6a2d5-635">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="6a2d5-636">Dodawanie obsługi uwierzytelniania do internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="6a2d5-636">Add authentication support to a web API</span></span>

<span data-ttu-id="6a2d5-637">Zobacz samouczek [usługi identityserver4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .</span><span class="sxs-lookup"><span data-stu-id="6a2d5-637">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a2d5-638">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6a2d5-638">Additional resources</span></span>

<span data-ttu-id="6a2d5-639">[Wyświetl lub Pobierz przykładowy kod dla tego samouczka](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="6a2d5-640">Zobacz artykuł [jak pobrać](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="6a2d5-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="6a2d5-641">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="6a2d5-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="6a2d5-642">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="6a2d5-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
