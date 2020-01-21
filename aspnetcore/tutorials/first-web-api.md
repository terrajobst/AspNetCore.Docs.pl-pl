---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 73e547b014d78dcbcbf1c887ebec16e0743d10b9
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294748"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="d2df9-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2df9-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="d2df9-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="d2df9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="d2df9-105">W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2df9-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d2df9-106">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d2df9-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d2df9-107">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d2df9-107">Create a web API project.</span></span>
> * <span data-ttu-id="d2df9-108">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="d2df9-109">Tworzy szkielet kontrolera z metodami CRUD.</span><span class="sxs-lookup"><span data-stu-id="d2df9-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="d2df9-110">Skonfiguruj Routing, ścieżki URL i wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="d2df9-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="d2df9-111">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="d2df9-111">Call the web API with Postman.</span></span>

<span data-ttu-id="d2df9-112">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="d2df9-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d2df9-113">Overview</span></span>

<span data-ttu-id="d2df9-114">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="d2df9-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="d2df9-115">interfejs API</span><span class="sxs-lookup"><span data-stu-id="d2df9-115">API</span></span> | <span data-ttu-id="d2df9-116">Opis</span><span class="sxs-lookup"><span data-stu-id="d2df9-116">Description</span></span> | <span data-ttu-id="d2df9-117">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="d2df9-117">Request body</span></span> | <span data-ttu-id="d2df9-118">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d2df9-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="d2df9-119">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="d2df9-119">GET /api/TodoItems</span></span> | <span data-ttu-id="d2df9-120">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-120">Get all to-do items</span></span> | <span data-ttu-id="d2df9-121">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-121">None</span></span> | <span data-ttu-id="d2df9-122">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-122">Array of to-do items</span></span>|
|<span data-ttu-id="d2df9-123">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="d2df9-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="d2df9-124">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="d2df9-124">Get an item by ID</span></span> | <span data-ttu-id="d2df9-125">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-125">None</span></span> | <span data-ttu-id="d2df9-126">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-126">To-do item</span></span>|
|<span data-ttu-id="d2df9-127">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="d2df9-127">POST /api/TodoItems</span></span> | <span data-ttu-id="d2df9-128">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="d2df9-128">Add a new item</span></span> | <span data-ttu-id="d2df9-129">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-129">To-do item</span></span> | <span data-ttu-id="d2df9-130">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-130">To-do item</span></span> |
|<span data-ttu-id="d2df9-131">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="d2df9-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="d2df9-132">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="d2df9-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="d2df9-133">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-133">To-do item</span></span> | <span data-ttu-id="d2df9-134">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-134">None</span></span> |
|<span data-ttu-id="d2df9-135">Usuń/api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="d2df9-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="d2df9-136">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="d2df9-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="d2df9-137">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-137">None</span></span> | <span data-ttu-id="d2df9-138">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-138">None</span></span>|

<span data-ttu-id="d2df9-139">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-139">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="d2df9-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d2df9-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d2df9-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d2df9-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d2df9-148">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="d2df9-149">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="d2df9-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2df9-151">Z menu **plik** wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="d2df9-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d2df9-152">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="d2df9-153">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="d2df9-154">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,1** .</span><span class="sxs-lookup"><span data-stu-id="d2df9-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="d2df9-155">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-155">Select the **API** template and click **Create**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d2df9-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d2df9-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d2df9-158">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d2df9-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d2df9-159">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="d2df9-160">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d2df9-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="d2df9-161">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="d2df9-162">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d2df9-162">The preceding commands:</span></span>

  * <span data-ttu-id="d2df9-163">Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d2df9-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="d2df9-164">Dodaje pakiety NuGet, które są wymagane w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d2df9-165">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d2df9-166">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-166">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="d2df9-168">Wybierz pozycję **.NET Core** > **App** > **API** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="d2df9-170">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję **docelowa platforma** \* *.NET Core 3,1*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="d2df9-171">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="d2df9-173">Otwórz Terminal poleceń w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d2df9-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="d2df9-174">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="d2df9-174">Test the API</span></span>

<span data-ttu-id="d2df9-175">Szablon projektu umożliwia utworzenie `WeatherForecast` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="d2df9-176">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2df9-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d2df9-178">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2df9-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="d2df9-179">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="d2df9-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="d2df9-180">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="d2df9-181">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d2df9-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d2df9-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d2df9-183">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2df9-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="d2df9-184">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="d2df9-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d2df9-185">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d2df9-186">Wybierz pozycję **uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2df9-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="d2df9-187">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="d2df9-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d2df9-188">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="d2df9-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="d2df9-189">Dołącz `/WeatherForecast` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="d2df9-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="d2df9-190">Zwracany jest kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="d2df9-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="d2df9-191">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="d2df9-191">Add a model class</span></span>

<span data-ttu-id="d2df9-192">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="d2df9-193">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="d2df9-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2df9-195">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="d2df9-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="d2df9-196">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="d2df9-197">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="d2df9-198">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d2df9-199">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="d2df9-200">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d2df9-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d2df9-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d2df9-202">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="d2df9-203">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d2df9-204">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d2df9-205">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="d2df9-205">Right-click the project.</span></span> <span data-ttu-id="d2df9-206">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="d2df9-207">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-207">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="d2df9-209">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** > **nowy plik** > **Ogólne** > **pustej klasy**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="d2df9-210">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="d2df9-211">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="d2df9-212">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="d2df9-213">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="d2df9-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="d2df9-214">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="d2df9-214">Add a database context</span></span>

<span data-ttu-id="d2df9-215">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d2df9-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="d2df9-216">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="d2df9-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="d2df9-218">Dodaj Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="d2df9-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="d2df9-219">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="d2df9-220">Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="d2df9-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="d2df9-221">W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="d2df9-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="d2df9-222">Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="d2df9-223">Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="d2df9-225">Dodawanie kontekstu bazy danych TodoContext</span><span class="sxs-lookup"><span data-stu-id="d2df9-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="d2df9-226">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d2df9-227">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d2df9-228">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="d2df9-229">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="d2df9-230">Wprowadź następujący kod:</span><span class="sxs-lookup"><span data-stu-id="d2df9-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="d2df9-231">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="d2df9-231">Register the database context</span></span>

<span data-ttu-id="d2df9-232">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="d2df9-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d2df9-233">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="d2df9-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="d2df9-234">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="d2df9-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="d2df9-235">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="d2df9-235">The preceding code:</span></span>

* <span data-ttu-id="d2df9-236">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="d2df9-237">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="d2df9-238">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d2df9-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="d2df9-239">Tworzenie szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="d2df9-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2df9-241">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="d2df9-242">Wybierz pozycję **dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d2df9-243">Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="d2df9-244">Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:</span><span class="sxs-lookup"><span data-stu-id="d2df9-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="d2df9-245">Wybierz pozycję **TodoItem (TodoApi. models)** w **klasie model**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="d2df9-246">W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoApi. models)** .</span><span class="sxs-lookup"><span data-stu-id="d2df9-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="d2df9-247">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d2df9-248">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="d2df9-249">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d2df9-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="d2df9-250">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d2df9-250">The preceding commands:</span></span>

* <span data-ttu-id="d2df9-251">Dodaj pakiety NuGet wymagane do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="d2df9-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="d2df9-252">Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="d2df9-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="d2df9-253">Szkieletuje `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="d2df9-254">Wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="d2df9-254">The generated code:</span></span>

* <span data-ttu-id="d2df9-255">Oznacza klasę atrybutem [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="d2df9-255">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="d2df9-256">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-256">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="d2df9-257">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="d2df9-257">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="d2df9-258">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d2df9-258">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="d2df9-259">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="d2df9-259">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="d2df9-260">Badanie metody PostTodoItem Create</span><span class="sxs-lookup"><span data-stu-id="d2df9-260">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="d2df9-261">Zastąp instrukcję return w `PostTodoItem`, aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="d2df9-261">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="d2df9-262">Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="d2df9-262">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="d2df9-263">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2df9-263">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="d2df9-264"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Metody:</span><span class="sxs-lookup"><span data-stu-id="d2df9-264">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="d2df9-265">W razie powodzenia zwraca kod stanu HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="d2df9-265">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="d2df9-266">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d2df9-266">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="d2df9-267">Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2df9-267">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="d2df9-268">Nagłówek `Location` określa [Identyfikator URI](https://developer.mozilla.org/docs/Glossary/URI) nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="d2df9-268">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="d2df9-269">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="d2df9-269">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="d2df9-270">Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-270">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="d2df9-271">Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-271">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="d2df9-272">Zainstaluj program Poster</span><span class="sxs-lookup"><span data-stu-id="d2df9-272">Install Postman</span></span>

<span data-ttu-id="d2df9-273">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-273">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="d2df9-274">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="d2df9-274">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="d2df9-275">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="d2df9-275">Start the web app.</span></span>
* <span data-ttu-id="d2df9-276">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="d2df9-276">Start Postman.</span></span>
* <span data-ttu-id="d2df9-277">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="d2df9-277">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="d2df9-278">Z **ustawień** > **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-278">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="d2df9-279">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d2df9-279">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="d2df9-280">Test PostTodoItem za pomocą programu Poster</span><span class="sxs-lookup"><span data-stu-id="d2df9-280">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="d2df9-281">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="d2df9-281">Create a new request.</span></span>
* <span data-ttu-id="d2df9-282">Ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-282">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="d2df9-283">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="d2df9-283">Select the **Body** tab.</span></span>
* <span data-ttu-id="d2df9-284">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="d2df9-284">Select the **raw** radio button.</span></span>
* <span data-ttu-id="d2df9-285">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="d2df9-285">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="d2df9-286">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="d2df9-286">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="d2df9-287">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-287">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="d2df9-289">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="d2df9-289">Test the location header URI</span></span>

* <span data-ttu-id="d2df9-290">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="d2df9-290">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="d2df9-291">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="d2df9-291">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="d2df9-293">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="d2df9-293">Set the method to GET.</span></span>
* <span data-ttu-id="d2df9-294">Wklej URI (na przykład `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="d2df9-294">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="d2df9-295">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-295">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="d2df9-296">Badanie metod GET</span><span class="sxs-lookup"><span data-stu-id="d2df9-296">Examine the GET methods</span></span>

<span data-ttu-id="d2df9-297">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="d2df9-297">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="d2df9-298">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-298">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="d2df9-299">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d2df9-299">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="d2df9-300">Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="d2df9-300">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="d2df9-301">Test get przy użyciu programu Poster</span><span class="sxs-lookup"><span data-stu-id="d2df9-301">Test Get with Postman</span></span>

* <span data-ttu-id="d2df9-302">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="d2df9-302">Create a new request.</span></span>
* <span data-ttu-id="d2df9-303">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-303">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="d2df9-304">Ustaw adres URL żądania `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-304">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="d2df9-305">Na przykład `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-305">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="d2df9-306">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="d2df9-306">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="d2df9-307">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-307">Select **Send**.</span></span>

<span data-ttu-id="d2df9-308">Ta aplikacja używa bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d2df9-308">This app uses an in-memory database.</span></span> <span data-ttu-id="d2df9-309">Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-309">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="d2df9-310">Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-310">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="d2df9-311">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="d2df9-311">Routing and URL paths</span></span>

<span data-ttu-id="d2df9-312">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d2df9-312">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="d2df9-313">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d2df9-313">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="d2df9-314">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="d2df9-314">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="d2df9-315">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="d2df9-315">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="d2df9-316">Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="d2df9-316">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="d2df9-317">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="d2df9-317">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="d2df9-318">Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="d2df9-318">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="d2df9-319">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-319">This sample doesn't use a template.</span></span> <span data-ttu-id="d2df9-320">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="d2df9-320">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="d2df9-321">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="d2df9-321">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="d2df9-322">Po wywołaniu `GetTodoItem` wartość `"{id}"` w adresie URL jest podawana do metody w `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="d2df9-322">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="d2df9-323">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="d2df9-323">Return values</span></span>

<span data-ttu-id="d2df9-324">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="d2df9-324">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="d2df9-325">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2df9-325">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="d2df9-326">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="d2df9-326">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="d2df9-327">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="d2df9-327">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="d2df9-328">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2df9-328">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="d2df9-329">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="d2df9-329">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="d2df9-330">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-330">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="d2df9-331">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="d2df9-331">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="d2df9-332">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="d2df9-332">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="d2df9-333">Metoda PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="d2df9-333">The PutTodoItem method</span></span>

<span data-ttu-id="d2df9-334">Przeanalizuj metodę `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="d2df9-334">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="d2df9-335">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="d2df9-335">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="d2df9-336">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="d2df9-336">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="d2df9-337">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="d2df9-337">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="d2df9-338">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="d2df9-338">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="d2df9-339">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="d2df9-339">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="d2df9-340">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="d2df9-340">Test the PutTodoItem method</span></span>

<span data-ttu-id="d2df9-341">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="d2df9-341">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="d2df9-342">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-342">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="d2df9-343">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="d2df9-343">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="d2df9-344">Zaktualizuj element do wykonania o IDENTYFIKATORze 1 i ustaw jego nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="d2df9-344">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="d2df9-345">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="d2df9-345">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="d2df9-347">Metoda DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="d2df9-347">The DeleteTodoItem method</span></span>

<span data-ttu-id="d2df9-348">Przeanalizuj metodę `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="d2df9-348">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="d2df9-349">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="d2df9-349">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="d2df9-350">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="d2df9-350">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="d2df9-351">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-351">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="d2df9-352">Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="d2df9-352">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="d2df9-353">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-353">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="d2df9-354">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="d2df9-354">Call the web API with JavaScript</span></span>

<span data-ttu-id="d2df9-355">Zobacz [Samouczek: wywoływanie interfejsu API sieci web ASP.NET Core przy użyciu języka JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="d2df9-355">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d2df9-356">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d2df9-356">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d2df9-357">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d2df9-357">Create a web API project.</span></span>
> * <span data-ttu-id="d2df9-358">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-358">Add a model class and a database context.</span></span>
> * <span data-ttu-id="d2df9-359">Dodawanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d2df9-359">Add a controller.</span></span>
> * <span data-ttu-id="d2df9-360">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="d2df9-360">Add CRUD methods.</span></span>
> * <span data-ttu-id="d2df9-361">Konfigurowanie routingu i ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d2df9-361">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="d2df9-362">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="d2df9-362">Specify return values.</span></span>
> * <span data-ttu-id="d2df9-363">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="d2df9-363">Call the web API with Postman.</span></span>
> * <span data-ttu-id="d2df9-364">Wywołaj interfejs API sieci Web za pomocą języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d2df9-364">Call the web API with JavaScript.</span></span>

<span data-ttu-id="d2df9-365">Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-365">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="d2df9-366">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d2df9-366">Overview</span></span>

<span data-ttu-id="d2df9-367">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="d2df9-367">This tutorial creates the following API:</span></span>

|<span data-ttu-id="d2df9-368">interfejs API</span><span class="sxs-lookup"><span data-stu-id="d2df9-368">API</span></span> | <span data-ttu-id="d2df9-369">Opis</span><span class="sxs-lookup"><span data-stu-id="d2df9-369">Description</span></span> | <span data-ttu-id="d2df9-370">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="d2df9-370">Request body</span></span> | <span data-ttu-id="d2df9-371">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d2df9-371">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="d2df9-372">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="d2df9-372">GET /api/TodoItems</span></span> | <span data-ttu-id="d2df9-373">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-373">Get all to-do items</span></span> | <span data-ttu-id="d2df9-374">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-374">None</span></span> | <span data-ttu-id="d2df9-375">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-375">Array of to-do items</span></span>|
|<span data-ttu-id="d2df9-376">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="d2df9-376">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="d2df9-377">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="d2df9-377">Get an item by ID</span></span> | <span data-ttu-id="d2df9-378">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-378">None</span></span> | <span data-ttu-id="d2df9-379">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-379">To-do item</span></span>|
|<span data-ttu-id="d2df9-380">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="d2df9-380">POST /api/TodoItems</span></span> | <span data-ttu-id="d2df9-381">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="d2df9-381">Add a new item</span></span> | <span data-ttu-id="d2df9-382">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-382">To-do item</span></span> | <span data-ttu-id="d2df9-383">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-383">To-do item</span></span> |
|<span data-ttu-id="d2df9-384">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="d2df9-384">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="d2df9-385">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="d2df9-385">Update an existing item &nbsp;</span></span> | <span data-ttu-id="d2df9-386">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-386">To-do item</span></span> | <span data-ttu-id="d2df9-387">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-387">None</span></span> |
|<span data-ttu-id="d2df9-388">Usuń/api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="d2df9-388">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="d2df9-389">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="d2df9-389">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="d2df9-390">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-390">None</span></span> | <span data-ttu-id="d2df9-391">Brak</span><span class="sxs-lookup"><span data-stu-id="d2df9-391">None</span></span>|

<span data-ttu-id="d2df9-392">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-392">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="d2df9-398">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d2df9-398">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-399">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-399">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d2df9-400">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d2df9-400">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d2df9-401">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-401">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="d2df9-402">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="d2df9-402">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-403">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2df9-404">Z menu **plik** wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="d2df9-404">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d2df9-405">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-405">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="d2df9-406">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-406">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="d2df9-407">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** .</span><span class="sxs-lookup"><span data-stu-id="d2df9-407">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="d2df9-408">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-408">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="d2df9-409">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-409">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d2df9-411">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d2df9-411">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d2df9-412">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d2df9-412">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d2df9-413">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-413">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="d2df9-414">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d2df9-414">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="d2df9-415">Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-415">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="d2df9-416">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-416">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d2df9-417">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-417">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d2df9-418">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-418">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="d2df9-420">Wybierz pozycję **.NET Core** > **App** > **API** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-420">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="d2df9-422">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-422">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="d2df9-423">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-423">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="d2df9-425">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="d2df9-425">Test the API</span></span>

<span data-ttu-id="d2df9-426">Szablon projektu umożliwia utworzenie `values` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-426">The project template creates a `values` API.</span></span> <span data-ttu-id="d2df9-427">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2df9-427">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-428">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-428">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d2df9-429">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2df9-429">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="d2df9-430">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="d2df9-430">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="d2df9-431">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-431">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="d2df9-432">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-432">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d2df9-433">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d2df9-433">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d2df9-434">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2df9-434">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="d2df9-435">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="d2df9-435">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d2df9-436">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-436">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d2df9-437">Wybierz pozycję **uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2df9-437">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="d2df9-438">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="d2df9-438">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d2df9-439">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="d2df9-439">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="d2df9-440">Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="d2df9-440">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="d2df9-441">Zwracane są następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="d2df9-441">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="d2df9-442">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="d2df9-442">Add a model class</span></span>

<span data-ttu-id="d2df9-443">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-443">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="d2df9-444">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="d2df9-444">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-445">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-445">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2df9-446">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="d2df9-446">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="d2df9-447">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-447">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="d2df9-448">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-448">Name the folder *Models*.</span></span>

* <span data-ttu-id="d2df9-449">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-449">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d2df9-450">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-450">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="d2df9-451">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-451">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d2df9-452">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d2df9-452">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d2df9-453">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-453">Add a folder named *Models*.</span></span>

* <span data-ttu-id="d2df9-454">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-454">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d2df9-455">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-455">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d2df9-456">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="d2df9-456">Right-click the project.</span></span> <span data-ttu-id="d2df9-457">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-457">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="d2df9-458">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-458">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="d2df9-460">Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** > **nowy plik** > **Ogólne** > **pustej klasy**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-460">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="d2df9-461">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-461">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="d2df9-462">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-462">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="d2df9-463">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-463">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="d2df9-464">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="d2df9-464">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="d2df9-465">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="d2df9-465">Add a database context</span></span>

<span data-ttu-id="d2df9-466">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d2df9-466">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="d2df9-467">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="d2df9-467">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-468">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-468">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2df9-469">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-469">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d2df9-470">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-470">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d2df9-471">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-471">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="d2df9-472">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-472">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="d2df9-473">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-473">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="d2df9-474">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="d2df9-474">Register the database context</span></span>

<span data-ttu-id="d2df9-475">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="d2df9-475">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d2df9-476">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="d2df9-476">The container provides the service to controllers.</span></span>

<span data-ttu-id="d2df9-477">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="d2df9-477">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="d2df9-478">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="d2df9-478">The preceding code:</span></span>

* <span data-ttu-id="d2df9-479">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="d2df9-479">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="d2df9-480">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-480">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="d2df9-481">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d2df9-481">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="d2df9-482">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="d2df9-482">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-483">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-483">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2df9-484">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-484">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="d2df9-485">Wybierz pozycję **dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-485">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="d2df9-486">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-486">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="d2df9-487">Nazwa klasy *TodoController*i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-487">Name the class *TodoController*, and select **Add**.</span></span>

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d2df9-489">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-489">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="d2df9-490">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-490">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="d2df9-491">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-491">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="d2df9-492">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="d2df9-492">The preceding code:</span></span>

* <span data-ttu-id="d2df9-493">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="d2df9-493">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="d2df9-494">Oznacza klasę atrybutem [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="d2df9-494">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="d2df9-495">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-495">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="d2df9-496">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="d2df9-496">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="d2df9-497">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d2df9-497">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="d2df9-498">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="d2df9-498">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="d2df9-499">Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta.</span><span class="sxs-lookup"><span data-stu-id="d2df9-499">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="d2df9-500">Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2df9-500">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="d2df9-501">Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-501">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="d2df9-502">Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.</span><span class="sxs-lookup"><span data-stu-id="d2df9-502">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="d2df9-503">Dodaj metody Get</span><span class="sxs-lookup"><span data-stu-id="d2df9-503">Add Get methods</span></span>

<span data-ttu-id="d2df9-504">Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="d2df9-504">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="d2df9-505">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="d2df9-505">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="d2df9-506">Zatrzymaj aplikację, jeśli jest nadal uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="d2df9-506">Stop the app if it's still running.</span></span> <span data-ttu-id="d2df9-507">Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.</span><span class="sxs-lookup"><span data-stu-id="d2df9-507">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="d2df9-508">Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d2df9-508">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="d2df9-509">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d2df9-509">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="d2df9-510">Następującą odpowiedź HTTP jest tworzony przez wywołanie metody `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="d2df9-510">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="d2df9-511">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="d2df9-511">Routing and URL paths</span></span>

<span data-ttu-id="d2df9-512">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d2df9-512">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="d2df9-513">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d2df9-513">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="d2df9-514">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="d2df9-514">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="d2df9-515">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="d2df9-515">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="d2df9-516">W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera, więc nazwa kontrolera jest "todo".</span><span class="sxs-lookup"><span data-stu-id="d2df9-516">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="d2df9-517">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="d2df9-517">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="d2df9-518">Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="d2df9-518">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="d2df9-519">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-519">This sample doesn't use a template.</span></span> <span data-ttu-id="d2df9-520">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="d2df9-520">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="d2df9-521">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="d2df9-521">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="d2df9-522">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="d2df9-522">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="d2df9-523">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="d2df9-523">Return values</span></span>

<span data-ttu-id="d2df9-524">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="d2df9-524">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="d2df9-525">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2df9-525">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="d2df9-526">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="d2df9-526">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="d2df9-527">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="d2df9-527">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="d2df9-528">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2df9-528">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="d2df9-529">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="d2df9-529">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="d2df9-530">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-530">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="d2df9-531">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="d2df9-531">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="d2df9-532">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="d2df9-532">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="d2df9-533">Metoda GetTodoItems testu</span><span class="sxs-lookup"><span data-stu-id="d2df9-533">Test the GetTodoItems method</span></span>

<span data-ttu-id="d2df9-534">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-534">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="d2df9-535">Zainstaluj program [Poster](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="d2df9-535">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="d2df9-536">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="d2df9-536">Start the web app.</span></span>
* <span data-ttu-id="d2df9-537">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="d2df9-537">Start Postman.</span></span>
* <span data-ttu-id="d2df9-538">Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-538">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d2df9-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2df9-539">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2df9-540">Z **ustawień** > **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-540">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d2df9-541">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d2df9-541">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="d2df9-542">Z poziomu **preferencji** > **Poster** (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-542">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="d2df9-543">Alternatywnie wybierz klucz i wybierz pozycję **Ustawienia**, a następnie wyłącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="d2df9-543">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="d2df9-544">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d2df9-544">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="d2df9-545">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="d2df9-545">Create a new request.</span></span>
  * <span data-ttu-id="d2df9-546">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-546">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="d2df9-547">Ustaw adres URL żądania `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-547">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="d2df9-548">Na przykład `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-548">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="d2df9-549">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="d2df9-549">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="d2df9-550">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-550">Select **Send**.</span></span>

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="d2df9-552">Dodawanie metody Create</span><span class="sxs-lookup"><span data-stu-id="d2df9-552">Add a Create method</span></span>

<span data-ttu-id="d2df9-553">Dodaj następującą metodę `PostTodoItem` wewnątrz *kontrolera/TodoController. cs*:</span><span class="sxs-lookup"><span data-stu-id="d2df9-553">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="d2df9-554">Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="d2df9-554">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="d2df9-555">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2df9-555">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="d2df9-556">`CreatedAtAction` Metody:</span><span class="sxs-lookup"><span data-stu-id="d2df9-556">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="d2df9-557">Zwraca kod stanu HTTP 201, jeśli powodzenie.</span><span class="sxs-lookup"><span data-stu-id="d2df9-557">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="d2df9-558">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d2df9-558">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="d2df9-559">Dodaje nagłówek `Location` do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2df9-559">Adds a `Location` header to the response.</span></span> <span data-ttu-id="d2df9-560">Nagłówek `Location` określa identyfikator URI nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="d2df9-560">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="d2df9-561">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="d2df9-561">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="d2df9-562">Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-562">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="d2df9-563">Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-563">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="d2df9-564">Metoda PostTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="d2df9-564">Test the PostTodoItem method</span></span>

* <span data-ttu-id="d2df9-565">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="d2df9-565">Build the project.</span></span>
* <span data-ttu-id="d2df9-566">W narzędziu Postman, Ustawia metodę HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-566">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="d2df9-567">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="d2df9-567">Select the **Body** tab.</span></span>
* <span data-ttu-id="d2df9-568">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="d2df9-568">Select the **raw** radio button.</span></span>
* <span data-ttu-id="d2df9-569">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="d2df9-569">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="d2df9-570">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="d2df9-570">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="d2df9-571">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-571">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  <span data-ttu-id="d2df9-573">Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu metody `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-573">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="d2df9-574">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="d2df9-574">Test the location header URI</span></span>

* <span data-ttu-id="d2df9-575">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="d2df9-575">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="d2df9-576">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="d2df9-576">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="d2df9-578">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="d2df9-578">Set the method to GET.</span></span>
* <span data-ttu-id="d2df9-579">Wklej URI (na przykład `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="d2df9-579">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="d2df9-580">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-580">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="d2df9-581">Dodaj metodę PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="d2df9-581">Add a PutTodoItem method</span></span>

<span data-ttu-id="d2df9-582">Dodaj następujący kod `PutTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="d2df9-582">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="d2df9-583">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="d2df9-583">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="d2df9-584">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="d2df9-584">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="d2df9-585">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="d2df9-585">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="d2df9-586">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="d2df9-586">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="d2df9-587">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="d2df9-587">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="d2df9-588">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="d2df9-588">Test the PutTodoItem method</span></span>

<span data-ttu-id="d2df9-589">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="d2df9-589">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="d2df9-590">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-590">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="d2df9-591">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="d2df9-591">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="d2df9-592">Zaktualizuj element zadania do wykonania, który ma identyfikator = 1 i ustaw jego nazwę na "feed ryb":</span><span class="sxs-lookup"><span data-stu-id="d2df9-592">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="d2df9-593">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="d2df9-593">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="d2df9-595">Dodaj metodę DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="d2df9-595">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="d2df9-596">Dodaj następujący kod `DeleteTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="d2df9-596">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="d2df9-597">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="d2df9-597">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="d2df9-598">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="d2df9-598">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="d2df9-599">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="d2df9-599">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="d2df9-600">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-600">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="d2df9-601">Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="d2df9-601">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="d2df9-602">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d2df9-602">Select **Send**.</span></span>

<span data-ttu-id="d2df9-603">Przykładowa aplikacja umożliwia usunięcie wszystkich elementów.</span><span class="sxs-lookup"><span data-stu-id="d2df9-603">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="d2df9-604">Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-604">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="d2df9-605">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="d2df9-605">Call the web API with JavaScript</span></span>

<span data-ttu-id="d2df9-606">W tej sekcji zostanie dodana strona HTML, która używa języka JavaScript do wywoływania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-606">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="d2df9-607">jQuery inicjuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="d2df9-607">jQuery initiates the request.</span></span> <span data-ttu-id="d2df9-608">Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-608">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="d2df9-609">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-609">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="d2df9-610">Tworzenie *wwwroot* folder w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-610">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="d2df9-611">Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-611">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="d2df9-612">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-612">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="d2df9-613">Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-613">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="d2df9-614">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d2df9-614">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="d2df9-615">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="d2df9-615">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="d2df9-616">Otwórz *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d2df9-616">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="d2df9-617">Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.</span><span class="sxs-lookup"><span data-stu-id="d2df9-617">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="d2df9-618">Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-618">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="d2df9-619">Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d2df9-619">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="d2df9-620">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-620">Get a list of to-do items</span></span>

<span data-ttu-id="d2df9-621">jQuery wysyła żądanie HTTP GET do internetowego interfejsu API, który zwraca kod JSON reprezentujący tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="d2df9-621">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="d2df9-622">`success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="d2df9-622">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="d2df9-623">Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="d2df9-623">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="d2df9-624">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-624">Add a to-do item</span></span>

<span data-ttu-id="d2df9-625">jQuery wysyła żądanie HTTP POST z elementem do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="d2df9-625">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="d2df9-626">`accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych.</span><span class="sxs-lookup"><span data-stu-id="d2df9-626">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="d2df9-627">Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="d2df9-627">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="d2df9-628">Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="d2df9-628">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="d2df9-629">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-629">Update a to-do item</span></span>

<span data-ttu-id="d2df9-630">Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="d2df9-630">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="d2df9-631">`url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="d2df9-631">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="d2df9-632">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="d2df9-632">Delete a to-do item</span></span>

<span data-ttu-id="d2df9-633">Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="d2df9-633">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="d2df9-634">Dodawanie obsługi uwierzytelniania do internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="d2df9-634">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="d2df9-635">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d2df9-635">Additional resources</span></span>

<span data-ttu-id="d2df9-636">[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="d2df9-636">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="d2df9-637">Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d2df9-637">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="d2df9-638">Więcej informacji można znaleźć w następujących zasobach:</span><span class="sxs-lookup"><span data-stu-id="d2df9-638">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="d2df9-639">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="d2df9-639">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
