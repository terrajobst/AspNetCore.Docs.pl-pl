---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541842"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="f3e11-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3e11-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="f3e11-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Jan Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f3e11-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f3e11-105">Ten samouczek uczy się podstaw tworzenia interfejsu API sieci Web za pomocą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3e11-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="f3e11-106">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="f3e11-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f3e11-107">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f3e11-107">Create a web API project.</span></span>
> * <span data-ttu-id="f3e11-108">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f3e11-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="f3e11-109">Tworzy szkielet kontrolera z metodami CRUD.</span><span class="sxs-lookup"><span data-stu-id="f3e11-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="f3e11-110">Skonfiguruj Routing, ścieżki URL i wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="f3e11-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="f3e11-111">Wywołaj interfejs API sieci Web za pomocą programu Poster.</span><span class="sxs-lookup"><span data-stu-id="f3e11-111">Call the web API with Postman.</span></span>

<span data-ttu-id="f3e11-112">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f3e11-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="f3e11-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="f3e11-113">Overview</span></span>

<span data-ttu-id="f3e11-114">Ten samouczek tworzy internetowy interfejs API zawierający następujące punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="f3e11-114">This tutorial creates a web API containing the following endpoints:</span></span>

|<span data-ttu-id="f3e11-115">Punkt końcowy</span><span class="sxs-lookup"><span data-stu-id="f3e11-115">Endpoint</span></span>                  |<span data-ttu-id="f3e11-116">Opis</span><span class="sxs-lookup"><span data-stu-id="f3e11-116">Description</span></span>            |<span data-ttu-id="f3e11-117">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="f3e11-117">Request body</span></span>|<span data-ttu-id="f3e11-118">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="f3e11-118">Response body</span></span>       |
|--------------------------|-----------------------|------------|--------------------|
|<span data-ttu-id="f3e11-119">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f3e11-119">GET /api/TodoItems</span></span>        |<span data-ttu-id="f3e11-120">Pobierz wszystkie elementy do wykonania</span><span class="sxs-lookup"><span data-stu-id="f3e11-120">Get all to-do items</span></span>    |<span data-ttu-id="f3e11-121">Brak</span><span class="sxs-lookup"><span data-stu-id="f3e11-121">None</span></span>        |<span data-ttu-id="f3e11-122">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="f3e11-122">Array of to-do items</span></span>|
|<span data-ttu-id="f3e11-123">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f3e11-123">GET /api/TodoItems/{id}</span></span>   |<span data-ttu-id="f3e11-124">Pobieranie elementu według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="f3e11-124">Get an item by ID</span></span>      |<span data-ttu-id="f3e11-125">Brak</span><span class="sxs-lookup"><span data-stu-id="f3e11-125">None</span></span>        |<span data-ttu-id="f3e11-126">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="f3e11-126">To-do item</span></span>          |
|<span data-ttu-id="f3e11-127">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f3e11-127">POST /api/TodoItems</span></span>       |<span data-ttu-id="f3e11-128">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="f3e11-128">Add a new item</span></span>         |<span data-ttu-id="f3e11-129">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="f3e11-129">To-do item</span></span>  |<span data-ttu-id="f3e11-130">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="f3e11-130">To-do item</span></span>          |
|<span data-ttu-id="f3e11-131">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f3e11-131">PUT /api/TodoItems/{id}</span></span>   |<span data-ttu-id="f3e11-132">Aktualizowanie istniejącego elementu</span><span class="sxs-lookup"><span data-stu-id="f3e11-132">Update an existing item</span></span>|<span data-ttu-id="f3e11-133">Element do wykonania</span><span class="sxs-lookup"><span data-stu-id="f3e11-133">To-do item</span></span>  |<span data-ttu-id="f3e11-134">Brak</span><span class="sxs-lookup"><span data-stu-id="f3e11-134">None</span></span>                |
|<span data-ttu-id="f3e11-135">Usuń/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f3e11-135">DELETE /api/TodoItems/{id}</span></span>|<span data-ttu-id="f3e11-136">Usuń element</span><span class="sxs-lookup"><span data-stu-id="f3e11-136">Delete an item</span></span>         |<span data-ttu-id="f3e11-137">Brak</span><span class="sxs-lookup"><span data-stu-id="f3e11-137">None</span></span>        |<span data-ttu-id="f3e11-138">Brak</span><span class="sxs-lookup"><span data-stu-id="f3e11-138">None</span></span>                |

<span data-ttu-id="f3e11-139">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f3e11-139">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="f3e11-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f3e11-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3e11-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3e11-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f3e11-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3e11-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f3e11-148">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="f3e11-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="f3e11-149">Tworzenie projektu sieci Web</span><span class="sxs-lookup"><span data-stu-id="f3e11-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3e11-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3e11-150">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f3e11-151">Z menu **plik** wybierz pozycję **Nowy** **projekt** > .</span><span class="sxs-lookup"><span data-stu-id="f3e11-151">From the **File** menu, select **New** > **Project**.</span></span>
1. <span data-ttu-id="f3e11-152">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
1. <span data-ttu-id="f3e11-153">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-153">Name the project *TodoApi* and click **Create**.</span></span>
1. <span data-ttu-id="f3e11-154">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="f3e11-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="f3e11-155">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-155">Select the **API** template and click **Create**.</span></span>

![Okno dialogowe programu VS New Project](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f3e11-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3e11-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f3e11-158">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f3e11-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
1. <span data-ttu-id="f3e11-159">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="f3e11-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
1. <span data-ttu-id="f3e11-160">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f3e11-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. <span data-ttu-id="f3e11-161">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="f3e11-162">Poprzednie polecenia:</span><span class="sxs-lookup"><span data-stu-id="f3e11-162">The preceding commands:</span></span>

  * <span data-ttu-id="f3e11-163">Utwórz nowy projekt internetowego interfejsu API i otwórz go w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f3e11-163">Create a new web API project and open it in Visual Studio Code.</span></span>
  * <span data-ttu-id="f3e11-164">Dodaj pakiety NuGet, które są wymagane w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="f3e11-164">Add the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f3e11-165">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="f3e11-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="f3e11-166">Wybierz pozycję **plik**  > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-166">Select **File** > **New Solution**.</span></span>

    ![macOS nowe rozwiązanie](first-web-api-mac/_static/sln.png)

1. <span data-ttu-id="f3e11-168">Wybierz pozycję **.NET Core**  > **App**  > **API**  > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

    ![okno dialogowe nowego projektu macOS](first-web-api-mac/_static/1.png)
  
1. <span data-ttu-id="f3e11-170">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję **docelowa platforma** \* *.NET Core 3,0*.</span><span class="sxs-lookup"><span data-stu-id="f3e11-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>
1. <span data-ttu-id="f3e11-171">Wprowadź *TodoApi* jako **nazwę projektu** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="f3e11-173">Otwórz Terminal poleceń w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f3e11-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="f3e11-174">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="f3e11-174">Test the API</span></span>

<span data-ttu-id="f3e11-175">Szablon projektu tworzy interfejs API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="f3e11-176">Wywołaj metodę `Get` z poziomu przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="f3e11-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3e11-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3e11-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f3e11-178">Naciśnij <kbd>klawisze CTRL + F5</kbd> , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="f3e11-178">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="f3e11-179">Program Visual Studio uruchamia przeglądarkę i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` to losowo wybierany numer portu.</span><span class="sxs-lookup"><span data-stu-id="f3e11-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="f3e11-180">Jeśli zostanie wyświetlone okno dialogowe z pytaniem, czy należy zaufać certyfikatowi IIS Express, wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="f3e11-181">W wyświetlonym oknie dialogowym **ostrzeżenia o zabezpieczeniach** wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f3e11-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3e11-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f3e11-183">Naciśnij <kbd>klawisze CTRL + F5</kbd> , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="f3e11-183">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="f3e11-184">W przeglądarce przejdź do następującego adresu URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="f3e11-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f3e11-185">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="f3e11-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f3e11-186">Wybierz pozycję **uruchom**  > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="f3e11-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="f3e11-187">Visual Studio dla komputerów Mac uruchamia przeglądarkę i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest losowo wybranym numerem portu.</span><span class="sxs-lookup"><span data-stu-id="f3e11-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f3e11-188">Zwracany jest błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="f3e11-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="f3e11-189">Dołącz `/WeatherForecast` do adresu URL (Zmień adres URL na `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="f3e11-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="f3e11-190">Zwracany jest kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="f3e11-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="f3e11-191">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="f3e11-191">Add a model class</span></span>

<span data-ttu-id="f3e11-192">*Model* to zestaw klas, które reprezentują dane zarządzane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="f3e11-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="f3e11-193">Model tej aplikacji jest pojedynczym `TodoItem` klasą.</span><span class="sxs-lookup"><span data-stu-id="f3e11-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3e11-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3e11-194">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f3e11-195">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="f3e11-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="f3e11-196">Wybierz pozycję **dodaj**  > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f3e11-197">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="f3e11-197">Name the folder *Models*.</span></span>
1. <span data-ttu-id="f3e11-198">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="f3e11-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f3e11-199">Nadaj klasie nazwę *TodoItem* i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-199">Name the class *TodoItem* and select **Add**.</span></span>
1. <span data-ttu-id="f3e11-200">Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f3e11-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f3e11-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3e11-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f3e11-202">Dodaj folder o nazwie *models*.</span><span class="sxs-lookup"><span data-stu-id="f3e11-202">Add a folder named *Models*.</span></span>
1. <span data-ttu-id="f3e11-203">Dodaj klasę `TodoItem` do folderu *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="f3e11-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f3e11-204">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="f3e11-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="f3e11-205">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="f3e11-205">Right-click the project.</span></span> <span data-ttu-id="f3e11-206">Wybierz pozycję **dodaj**  > **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f3e11-207">Nazwij *modele*folderów.</span><span class="sxs-lookup"><span data-stu-id="f3e11-207">Name the folder *Models*.</span></span>

    ![Nowy folder](first-web-api-mac/_static/folder.png)

1. <span data-ttu-id="f3e11-209">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj**  > **nowy plik**  > **Ogólne**  > **pustej klasy**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>
1. <span data-ttu-id="f3e11-210">Nazwij klasę *TodoItem*, a następnie kliknij pozycję **New (nowy**).</span><span class="sxs-lookup"><span data-stu-id="f3e11-210">Name the class *TodoItem*, and then click **New**.</span></span>
1. <span data-ttu-id="f3e11-211">Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f3e11-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f3e11-212">Właściwość `Id` działa jako unikatowy klucz w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f3e11-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="f3e11-213">Klasy modelu mogą przejść do dowolnego miejsca w projekcie, ale folder *modele* jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="f3e11-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="f3e11-214">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="f3e11-214">Add a database context</span></span>

<span data-ttu-id="f3e11-215">*Kontekst bazy danych* jest główną klasą, która koordynuje Entity Framework funkcji dla modelu danych.</span><span class="sxs-lookup"><span data-stu-id="f3e11-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="f3e11-216">Ta klasa jest tworzona przez wyprowadzanie z klasy `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3e11-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3e11-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="f3e11-218">Dodaj Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="f3e11-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

1. <span data-ttu-id="f3e11-219">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
1. <span data-ttu-id="f3e11-220">Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="f3e11-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
1. <span data-ttu-id="f3e11-221">W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="f3e11-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
1. <span data-ttu-id="f3e11-222">Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
1. <span data-ttu-id="f3e11-223">Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="f3e11-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Menedżer pakietów NuGet](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="f3e11-225">Dodawanie kontekstu bazy danych TodoContext</span><span class="sxs-lookup"><span data-stu-id="f3e11-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="f3e11-226">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="f3e11-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f3e11-227">Nadaj klasie nazwę *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f3e11-228">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="f3e11-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f3e11-229">Dodaj klasę `TodoContext` do folderu *models* .</span><span class="sxs-lookup"><span data-stu-id="f3e11-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="f3e11-230">Wprowadź następujący kod:</span><span class="sxs-lookup"><span data-stu-id="f3e11-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="f3e11-231">Rejestrowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="f3e11-231">Register the database context</span></span>

<span data-ttu-id="f3e11-232">W ASP.NET Core usługi, takie jak kontekst bazy danych, muszą być zarejestrowane z kontenerem [iniekcji zależności (di)](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="f3e11-232">In ASP.NET Core, services such as the database context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f3e11-233">Kontener udostępnia usługę kontrolerom.</span><span class="sxs-lookup"><span data-stu-id="f3e11-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="f3e11-234">Zaktualizuj *Startup.cs* o następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="f3e11-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="f3e11-235">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="f3e11-235">The preceding code:</span></span>

* <span data-ttu-id="f3e11-236">Usuwa nieużywane deklaracje `using`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="f3e11-237">Dodaje kontekst bazy danych do kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="f3e11-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="f3e11-238">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="f3e11-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="f3e11-239">Tworzenie szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="f3e11-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3e11-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3e11-240">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f3e11-241">Kliknij prawym przyciskiem myszy folder *controllers* .</span><span class="sxs-lookup"><span data-stu-id="f3e11-241">Right-click the *Controllers* folder.</span></span>
1. <span data-ttu-id="f3e11-242">Wybierz pozycję **dodaj**  > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-242">Select **Add** > **New Scaffolded Item**.</span></span>
1. <span data-ttu-id="f3e11-243">Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
1. <span data-ttu-id="f3e11-244">Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:</span><span class="sxs-lookup"><span data-stu-id="f3e11-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>
    * <span data-ttu-id="f3e11-245">Wybierz pozycję **TodoItem (TodoApi. models)** w **klasie model**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
    * <span data-ttu-id="f3e11-246">W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoApi. models)** .</span><span class="sxs-lookup"><span data-stu-id="f3e11-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
    * <span data-ttu-id="f3e11-247">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f3e11-248">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="f3e11-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f3e11-249">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f3e11-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="f3e11-250">Poprzednie polecenia:</span><span class="sxs-lookup"><span data-stu-id="f3e11-250">The preceding commands:</span></span>

* <span data-ttu-id="f3e11-251">Dodaj pakiety NuGet wymagane do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="f3e11-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="f3e11-252">Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="f3e11-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="f3e11-253">Szkieletuje `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="f3e11-254">Wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="f3e11-254">The generated code:</span></span>

* <span data-ttu-id="f3e11-255">Definiuje klasę kontrolera interfejsu API bez metod.</span><span class="sxs-lookup"><span data-stu-id="f3e11-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="f3e11-256">Zdobi klasę z atrybutem [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) .</span><span class="sxs-lookup"><span data-stu-id="f3e11-256">Decorates the class with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="f3e11-257">Ten atrybut wskazuje, że kontroler odpowiada na żądania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f3e11-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="f3e11-258">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="f3e11-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="f3e11-259">Używa funkcji DI do iniekcji kontekstu bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f3e11-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="f3e11-260">Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="f3e11-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="f3e11-261">Badanie metody PostTodoItem Create</span><span class="sxs-lookup"><span data-stu-id="f3e11-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="f3e11-262">Zastąp instrukcję return w `PostTodoItem`, aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="f3e11-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="f3e11-263">Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [[HTTPPOST]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) .</span><span class="sxs-lookup"><span data-stu-id="f3e11-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="f3e11-264">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="f3e11-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f3e11-265">Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="f3e11-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="f3e11-266">W razie powodzenia zwraca kod stanu HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="f3e11-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="f3e11-267">HTTP 201 to standardowa odpowiedź dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f3e11-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f3e11-268">Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3e11-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="f3e11-269">Nagłówek `Location` określa [Identyfikator URI](https://developer.mozilla.org/docs/Glossary/URI) nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="f3e11-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="f3e11-270">Aby uzyskać więcej informacji, zobacz [10.2.2 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f3e11-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f3e11-271">Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="f3e11-272">Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="f3e11-273">Zainstaluj program Poster</span><span class="sxs-lookup"><span data-stu-id="f3e11-273">Install Postman</span></span>

<span data-ttu-id="f3e11-274">Ten samouczek używa programu do testowania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f3e11-274">This tutorial uses Postman to test the web API.</span></span>

1. <span data-ttu-id="f3e11-275">Zainstaluj program [Poster](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="f3e11-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
1. <span data-ttu-id="f3e11-276">Uruchom aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="f3e11-276">Start the web app.</span></span>
1. <span data-ttu-id="f3e11-277">Uruchom wpis.</span><span class="sxs-lookup"><span data-stu-id="f3e11-277">Start Postman.</span></span>
1. <span data-ttu-id="f3e11-278">Wyłącz **weryfikację certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="f3e11-278">Disable **SSL certificate verification**</span></span>
1. <span data-ttu-id="f3e11-279">Z**ustawień**  >  **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="f3e11-280">Po przetestowaniu kontrolera ponownie Włącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="f3e11-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="f3e11-281">Test PostTodoItem za pomocą programu Poster</span><span class="sxs-lookup"><span data-stu-id="f3e11-281">Test PostTodoItem with Postman</span></span>

1. <span data-ttu-id="f3e11-282">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="f3e11-282">Create a new request.</span></span>
1. <span data-ttu-id="f3e11-283">Ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-283">Set the HTTP method to `POST`.</span></span>
1. <span data-ttu-id="f3e11-284">Wybierz kartę **treść** .</span><span class="sxs-lookup"><span data-stu-id="f3e11-284">Select the **Body** tab.</span></span>
1. <span data-ttu-id="f3e11-285">Wybierz przycisk radiowy **RAW** .</span><span class="sxs-lookup"><span data-stu-id="f3e11-285">Select the **raw** radio button.</span></span>
1. <span data-ttu-id="f3e11-286">Ustaw typ na **JSON (Application/JSON)** .</span><span class="sxs-lookup"><span data-stu-id="f3e11-286">Set the type to **JSON (application/json)**.</span></span>
1. <span data-ttu-id="f3e11-287">W treści żądania wprowadź kod JSON dla elementu do wykonania:</span><span class="sxs-lookup"><span data-stu-id="f3e11-287">In the request body, enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. <span data-ttu-id="f3e11-288">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-288">Select **Send**.</span></span>

  ![Ogłoś przy użyciu żądania Create](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="f3e11-290">Testowanie identyfikatora URI nagłówka lokalizacji</span><span class="sxs-lookup"><span data-stu-id="f3e11-290">Test the location header URI</span></span>

1. <span data-ttu-id="f3e11-291">Wybierz kartę **nagłówki** w okienku **odpowiedź** .</span><span class="sxs-lookup"><span data-stu-id="f3e11-291">Select the **Headers** tab in the **Response** pane.</span></span>
1. <span data-ttu-id="f3e11-292">Skopiuj wartość nagłówka **lokalizacji** :</span><span class="sxs-lookup"><span data-stu-id="f3e11-292">Copy the **Location** header value:</span></span>

    ![Karta nagłówki w konsoli programu Poster](first-web-api/_static/create.png)

1. <span data-ttu-id="f3e11-294">Ustaw metodę, aby uzyskać.</span><span class="sxs-lookup"><span data-stu-id="f3e11-294">Set the method to GET.</span></span>
1. <span data-ttu-id="f3e11-295">Wklej URI (na przykład `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="f3e11-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="f3e11-296">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="f3e11-297">Badanie metod GET</span><span class="sxs-lookup"><span data-stu-id="f3e11-297">Examine the GET methods</span></span>

<span data-ttu-id="f3e11-298">Te metody implementują dwa punkty końcowe GET:</span><span class="sxs-lookup"><span data-stu-id="f3e11-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="f3e11-299">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu.</span><span class="sxs-lookup"><span data-stu-id="f3e11-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="f3e11-300">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f3e11-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="f3e11-301">Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="f3e11-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="f3e11-302">Test get przy użyciu programu Poster</span><span class="sxs-lookup"><span data-stu-id="f3e11-302">Test Get with Postman</span></span>

1. <span data-ttu-id="f3e11-303">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="f3e11-303">Create a new request.</span></span>
1. <span data-ttu-id="f3e11-304">Ustaw metodę HTTP, aby **uzyskać**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-304">Set the HTTP method to **GET**.</span></span>
1. <span data-ttu-id="f3e11-305">Ustaw adres URL żądania na `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="f3e11-306">Na przykład `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
1. <span data-ttu-id="f3e11-307">Ustaw **dwa widoki okienka** w programie Poster.</span><span class="sxs-lookup"><span data-stu-id="f3e11-307">Set **Two pane view** in Postman.</span></span>
1. <span data-ttu-id="f3e11-308">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-308">Select **Send**.</span></span>

<span data-ttu-id="f3e11-309">Ta aplikacja używa bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="f3e11-309">This app uses an in-memory database.</span></span> <span data-ttu-id="f3e11-310">Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="f3e11-310">If the app is stopped and started, the preceding GET request won't return any data.</span></span> <span data-ttu-id="f3e11-311">Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f3e11-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="f3e11-312">Ścieżki routingu i adresów URL</span><span class="sxs-lookup"><span data-stu-id="f3e11-312">Routing and URL paths</span></span>

<span data-ttu-id="f3e11-313">Atrybut [[narzędzia HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) oznacza metodę, która reaguje na żądanie HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f3e11-313">The [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="f3e11-314">Ścieżka adresu URL dla każdej metody jest zbudowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f3e11-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f3e11-315">Rozpocznij od ciągu szablonu w atrybucie `Route` kontrolera:</span><span class="sxs-lookup"><span data-stu-id="f3e11-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="f3e11-316">Zastąp `[controller]` nazwą kontrolera, którą Konwencją jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="f3e11-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f3e11-317">Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="f3e11-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="f3e11-318">W ASP.NET Core [routingu](xref:mvc/controllers/routing) jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f3e11-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="f3e11-319">Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="f3e11-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="f3e11-320">Ten przykład nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="f3e11-320">This sample doesn't use a template.</span></span> <span data-ttu-id="f3e11-321">Aby uzyskać więcej informacji, zobacz temat [Routing atrybutów z atrybutami http [Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="f3e11-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="f3e11-322">W poniższej metodzie `GetTodoItem` `"{id}"` jest zmienną zastępczą dla unikatowego identyfikatora elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="f3e11-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="f3e11-323">Po wywołaniu `GetTodoItem` wartość `"{id}"` w adresie URL jest podawana do metody w `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="f3e11-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="f3e11-324">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="f3e11-324">Return values</span></span>

<span data-ttu-id="f3e11-325">Typem zwracanym `GetTodoItems` i `GetTodoItem` Metoda jest [ActionResult \<T > typ](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="f3e11-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="f3e11-326">ASP.NET Core automatycznie serializować obiektu do [formatu JSON](https://www.json.org/) i zapisuje kod JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f3e11-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f3e11-327">Kod odpowiedzi dla tego typu zwracanego to 200, przy założeniu, że nie istnieją Nieobsłużone wyjątki.</span><span class="sxs-lookup"><span data-stu-id="f3e11-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f3e11-328">Nieobsłużone wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="f3e11-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="f3e11-329">`ActionResult` zwracane typy mogą reprezentować szeroką gamę kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f3e11-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="f3e11-330">Na przykład `GetTodoItem` mogą zwracać dwie różne wartości stanu:</span><span class="sxs-lookup"><span data-stu-id="f3e11-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="f3e11-331">Jeśli żaden element nie jest zgodny z żądanym IDENTYFIKATORem, metoda zwróci kod błędu 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound>.</span><span class="sxs-lookup"><span data-stu-id="f3e11-331">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> error code.</span></span>
* <span data-ttu-id="f3e11-332">W przeciwnym razie metoda zwraca 200 z treścią odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="f3e11-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f3e11-333">Zwracanie `item` wyników w odpowiedzi HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="f3e11-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="f3e11-334">Metoda PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f3e11-334">The PutTodoItem method</span></span>

<span data-ttu-id="f3e11-335">Przeanalizuj metodę `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="f3e11-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="f3e11-336">`PutTodoItem` jest podobna do `PostTodoItem`, z tą różnicą, że używa protokołu HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f3e11-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="f3e11-337">Odpowiedź to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f3e11-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f3e11-338">Zgodnie ze specyfikacją protokołu HTTP żądanie PUT wymaga, aby klient wysłał całą zaktualizowaną jednostkę, a nie tylko te zmiany.</span><span class="sxs-lookup"><span data-stu-id="f3e11-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="f3e11-339">Aby zapewnić obsługę częściowych aktualizacji, użyj [poprawki http](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="f3e11-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="f3e11-340">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="f3e11-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="f3e11-341">Testowanie metody PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f3e11-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="f3e11-342">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="f3e11-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="f3e11-343">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f3e11-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="f3e11-344">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="f3e11-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="f3e11-345">Zaktualizuj element do wykonania o IDENTYFIKATORze 1.</span><span class="sxs-lookup"><span data-stu-id="f3e11-345">Update the to-do item that has an ID of 1.</span></span> <span data-ttu-id="f3e11-346">Ustaw jej nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="f3e11-346">Set its name to "feed fish":</span></span>

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

<span data-ttu-id="f3e11-347">Na poniższej ilustracji przedstawiono aktualizację programu Poster:</span><span class="sxs-lookup"><span data-stu-id="f3e11-347">The following image shows the Postman update:</span></span>

![Konsola programu Poster pokazująca odpowiedź 204 (brak zawartości)](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="f3e11-349">Metoda DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f3e11-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="f3e11-350">Przeanalizuj metodę `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="f3e11-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="f3e11-351">Odpowiedź `DeleteTodoItem` to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f3e11-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="f3e11-352">Testowanie metody DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f3e11-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="f3e11-353">Użyj programu Poster, aby usunąć element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="f3e11-353">Use Postman to delete a to-do item:</span></span>

1. <span data-ttu-id="f3e11-354">Ustaw metodę na `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="f3e11-354">Set the method to `DELETE`.</span></span>
1. <span data-ttu-id="f3e11-355">Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="f3e11-355">Set the URI of the object to delete (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="f3e11-356">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="f3e11-356">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="f3e11-357">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="f3e11-357">Call the web API with JavaScript</span></span>

<span data-ttu-id="f3e11-358">Zobacz [Samouczek: wywoływanie interfejsu API sieci web ASP.NET Core przy użyciu języka JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="f3e11-358">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="f3e11-359">Dodawanie obsługi uwierzytelniania do internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="f3e11-359">Add authentication support to a web API</span></span>

<span data-ttu-id="f3e11-360">Zobacz samouczek [usługi identityserver4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .</span><span class="sxs-lookup"><span data-stu-id="f3e11-360">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3e11-361">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f3e11-361">Additional resources</span></span>

<span data-ttu-id="f3e11-362">[Wyświetl lub Pobierz przykładowy kod dla tego samouczka](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="f3e11-362">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="f3e11-363">Zobacz artykuł [jak pobrać](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="f3e11-363">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="f3e11-364">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="f3e11-364">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="f3e11-365">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="f3e11-365">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
