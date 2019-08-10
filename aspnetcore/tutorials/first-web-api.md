---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 5190fa30373c75e6358d9643cfe0042c7fc53557
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68915115"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="045cd-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="045cd-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="045cd-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="045cd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="045cd-105">W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="045cd-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="045cd-106">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="045cd-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="045cd-107">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="045cd-107">Create a web API project.</span></span>
> * <span data-ttu-id="045cd-108">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="045cd-109">Tworzy szkielet kontrolera z metodami CRUD.</span><span class="sxs-lookup"><span data-stu-id="045cd-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="045cd-110">Skonfiguruj Routing, ścieżki URL i wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="045cd-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="045cd-111">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="045cd-111">Call the web API with Postman.</span></span>

<span data-ttu-id="045cd-112">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="045cd-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="045cd-113">Overview</span></span>

<span data-ttu-id="045cd-114">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="045cd-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="045cd-115">interfejs API</span><span class="sxs-lookup"><span data-stu-id="045cd-115">API</span></span> | <span data-ttu-id="045cd-116">Opis</span><span class="sxs-lookup"><span data-stu-id="045cd-116">Description</span></span> | <span data-ttu-id="045cd-117">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="045cd-117">Request body</span></span> | <span data-ttu-id="045cd-118">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="045cd-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="045cd-119">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="045cd-119">GET /api/TodoItems</span></span> | <span data-ttu-id="045cd-120">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-120">Get all to-do items</span></span> | <span data-ttu-id="045cd-121">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-121">None</span></span> | <span data-ttu-id="045cd-122">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-122">Array of to-do items</span></span>|
|<span data-ttu-id="045cd-123">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="045cd-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="045cd-124">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="045cd-124">Get an item by ID</span></span> | <span data-ttu-id="045cd-125">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-125">None</span></span> | <span data-ttu-id="045cd-126">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-126">To-do item</span></span>|
|<span data-ttu-id="045cd-127">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="045cd-127">POST /api/TodoItems</span></span> | <span data-ttu-id="045cd-128">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="045cd-128">Add a new item</span></span> | <span data-ttu-id="045cd-129">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-129">To-do item</span></span> | <span data-ttu-id="045cd-130">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-130">To-do item</span></span> |
|<span data-ttu-id="045cd-131">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="045cd-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="045cd-132">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="045cd-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="045cd-133">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-133">To-do item</span></span> | <span data-ttu-id="045cd-134">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-134">None</span></span> |
|<span data-ttu-id="045cd-135">Usuń/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="045cd-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="045cd-136">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="045cd-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="045cd-137">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-137">None</span></span> | <span data-ttu-id="045cd-138">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-138">None</span></span>|

<span data-ttu-id="045cd-139">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="045cd-139">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="045cd-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="045cd-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045cd-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045cd-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="045cd-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="045cd-149">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="045cd-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="045cd-151">Z menu **plik** wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="045cd-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="045cd-152">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="045cd-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="045cd-153">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="045cd-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="045cd-154">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="045cd-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="045cd-155">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="045cd-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="045cd-156">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="045cd-156">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045cd-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045cd-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="045cd-159">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="045cd-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="045cd-160">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="045cd-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="045cd-161">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="045cd-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="045cd-162">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="045cd-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="045cd-163">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="045cd-163">The preceding commands:</span></span>

  * <span data-ttu-id="045cd-164">Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="045cd-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="045cd-165">Dodaje pakiety NuGet, które są wymagane w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="045cd-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="045cd-166">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="045cd-167">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="045cd-167">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="045cd-169">Wybierzpozycję **interfejs API** > > aplikacji.NET> Core.</span><span class="sxs-lookup"><span data-stu-id="045cd-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="045cd-171">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję docelowa **platforma** \* *.NET Core 3,0*.</span><span class="sxs-lookup"><span data-stu-id="045cd-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="045cd-172">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="045cd-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="045cd-174">Otwórz Terminal poleceń w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="045cd-174">Open a command terminal in the project folder and run the following commands:</span></span>

   ```console
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="045cd-175">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="045cd-175">Test the API</span></span>

<span data-ttu-id="045cd-176">Szablon projektu umożliwia utworzenie `WeatherForecast` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-176">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="045cd-177">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="045cd-177">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-178">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="045cd-179">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="045cd-179">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="045cd-180">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="045cd-180">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="045cd-181">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="045cd-181">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="045cd-182">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="045cd-182">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045cd-183">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045cd-183">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="045cd-184">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="045cd-184">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="045cd-185">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="045cd-185">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="045cd-186">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-186">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="045cd-187">Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="045cd-187">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="045cd-188">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="045cd-188">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="045cd-189">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="045cd-189">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="045cd-190">Dołącz `/WeatherForecast` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="045cd-190">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="045cd-191">Zwracany jest kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="045cd-191">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="045cd-192">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="045cd-192">Add a model class</span></span>

<span data-ttu-id="045cd-193">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="045cd-193">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="045cd-194">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="045cd-194">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="045cd-196">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="045cd-196">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="045cd-197">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="045cd-197">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="045cd-198">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="045cd-198">Name the folder *Models*.</span></span>

* <span data-ttu-id="045cd-199">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="045cd-199">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="045cd-200">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="045cd-200">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="045cd-201">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-201">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045cd-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045cd-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="045cd-203">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="045cd-203">Add a folder named *Models*.</span></span>

* <span data-ttu-id="045cd-204">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-204">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="045cd-205">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="045cd-206">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="045cd-206">Right-click the project.</span></span> <span data-ttu-id="045cd-207">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="045cd-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="045cd-208">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="045cd-208">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="045cd-210">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.</span><span class="sxs-lookup"><span data-stu-id="045cd-210">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="045cd-211">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="045cd-211">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="045cd-212">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-212">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="045cd-213">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-213">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="045cd-214">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="045cd-214">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="045cd-215">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="045cd-215">Add a database context</span></span>

<span data-ttu-id="045cd-216">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="045cd-216">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="045cd-217">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="045cd-217">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-218">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="045cd-219">Dodaj Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="045cd-219">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="045cd-220">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="045cd-220">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="045cd-221">Zaznacz pole wyboru **Uwzględnij wersję wstępną** .</span><span class="sxs-lookup"><span data-stu-id="045cd-221">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="045cd-222">Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="045cd-222">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="045cd-223">W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer v 3.0.0 — wersja** zapoznawcza.</span><span class="sxs-lookup"><span data-stu-id="045cd-223">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="045cd-224">Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="045cd-224">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="045cd-225">Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="045cd-225">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="045cd-227">Dodawanie kontekstu bazy danych TodoContext</span><span class="sxs-lookup"><span data-stu-id="045cd-227">Add the TodoContext database context</span></span>

* <span data-ttu-id="045cd-228">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="045cd-228">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="045cd-229">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="045cd-229">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="045cd-230">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-230">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="045cd-231">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="045cd-231">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="045cd-232">Wprowadź następujący kod:</span><span class="sxs-lookup"><span data-stu-id="045cd-232">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="045cd-233">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="045cd-233">Register the database context</span></span>

<span data-ttu-id="045cd-234">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="045cd-234">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="045cd-235">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="045cd-235">The container provides the service to controllers.</span></span>

<span data-ttu-id="045cd-236">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="045cd-236">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="045cd-237">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="045cd-237">The preceding code:</span></span>

* <span data-ttu-id="045cd-238">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="045cd-238">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="045cd-239">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-239">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="045cd-240">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="045cd-240">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="045cd-241">Tworzenie szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="045cd-241">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-242">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="045cd-243">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="045cd-243">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="045cd-244">Wybierz pozycję **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="045cd-244">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="045cd-245">Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="045cd-245">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="045cd-246">Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:</span><span class="sxs-lookup"><span data-stu-id="045cd-246">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="045cd-247">Wybierz pozycję **TodoItem (TodoAPI. models)** w **klasie model**.</span><span class="sxs-lookup"><span data-stu-id="045cd-247">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="045cd-248">W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoAPI. models)** .</span><span class="sxs-lookup"><span data-stu-id="045cd-248">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="045cd-249">Wybierz pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="045cd-249">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="045cd-250">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-250">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="045cd-251">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="045cd-251">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="045cd-252">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="045cd-252">The preceding commands:</span></span>

* <span data-ttu-id="045cd-253">Dodaj pakiety NuGet wymagane do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="045cd-253">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="045cd-254">Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="045cd-254">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="045cd-255">Szkielety `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="045cd-255">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="045cd-256">Wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="045cd-256">The generated code:</span></span>

* <span data-ttu-id="045cd-257">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="045cd-257">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="045cd-258">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="045cd-258">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="045cd-259">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-259">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="045cd-260">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="045cd-260">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="045cd-261">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="045cd-261">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="045cd-262">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="045cd-262">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="045cd-263">Badanie metody PostTodoItem Create</span><span class="sxs-lookup"><span data-stu-id="045cd-263">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="045cd-264">Zastąp instrukcję return w, `PostTodoItem` aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="045cd-264">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="045cd-265">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="045cd-265">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="045cd-266">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="045cd-266">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="045cd-267"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Metody:</span><span class="sxs-lookup"><span data-stu-id="045cd-267">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="045cd-268">W razie powodzenia zwraca kod stanu HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="045cd-268">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="045cd-269">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="045cd-269">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="045cd-270">Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="045cd-270">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="045cd-271">Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania. [](https://developer.mozilla.org/docs/Glossary/URI) `Location`</span><span class="sxs-lookup"><span data-stu-id="045cd-271">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="045cd-272">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="045cd-272">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="045cd-273">Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka.</span><span class="sxs-lookup"><span data-stu-id="045cd-273">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="045cd-274">Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="045cd-274">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="045cd-275">Zainstaluj program Poster</span><span class="sxs-lookup"><span data-stu-id="045cd-275">Install Postman</span></span>

<span data-ttu-id="045cd-276">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-276">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="045cd-277">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="045cd-277">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="045cd-278">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="045cd-278">Start the web app.</span></span>
* <span data-ttu-id="045cd-279">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="045cd-279">Start Postman.</span></span>
* <span data-ttu-id="045cd-280">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="045cd-280">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="045cd-281">Z **Plik > Ustawienia** (\**ogólne* karty), wyłącz **weryfikacji certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="045cd-281">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="045cd-282">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="045cd-282">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="045cd-283">Test PostTodoItem za pomocą programu Poster</span><span class="sxs-lookup"><span data-stu-id="045cd-283">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="045cd-284">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="045cd-284">Create a new request.</span></span>
* <span data-ttu-id="045cd-285">Ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="045cd-285">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="045cd-286">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="045cd-286">Select the **Body** tab.</span></span>
* <span data-ttu-id="045cd-287">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="045cd-287">Select the **raw** radio button.</span></span>
* <span data-ttu-id="045cd-288">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="045cd-288">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="045cd-289">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="045cd-289">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="045cd-290">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="045cd-290">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="045cd-292">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="045cd-292">Test the location header URI</span></span>

* <span data-ttu-id="045cd-293">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="045cd-293">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="045cd-294">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="045cd-294">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="045cd-296">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="045cd-296">Set the method to GET.</span></span>
* <span data-ttu-id="045cd-297">Wklej identyfikator URI (na przykład `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="045cd-297">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="045cd-298">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="045cd-298">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="045cd-299">Badanie metod GET</span><span class="sxs-lookup"><span data-stu-id="045cd-299">Examine the GET methods</span></span>

<span data-ttu-id="045cd-300">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="045cd-300">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="045cd-301">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu.</span><span class="sxs-lookup"><span data-stu-id="045cd-301">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="045cd-302">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="045cd-302">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="045cd-303">Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="045cd-303">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="045cd-304">Test get przy użyciu programu Poster</span><span class="sxs-lookup"><span data-stu-id="045cd-304">Test Get with Postman</span></span>

* <span data-ttu-id="045cd-305">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="045cd-305">Create a new request.</span></span>
* <span data-ttu-id="045cd-306">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="045cd-306">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="045cd-307">Ustaw adres URL żądania `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="045cd-307">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="045cd-308">Na przykład `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="045cd-308">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="045cd-309">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="045cd-309">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="045cd-310">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="045cd-310">Select **Send**.</span></span>

<span data-ttu-id="045cd-311">Ta aplikacja używa bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="045cd-311">This app uses an in-memory database.</span></span> <span data-ttu-id="045cd-312">Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-312">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="045cd-313">Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="045cd-313">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="045cd-314">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="045cd-314">Routing and URL paths</span></span>

<span data-ttu-id="045cd-315">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="045cd-315">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="045cd-316">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="045cd-316">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="045cd-317">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="045cd-317">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="045cd-318">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="045cd-318">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="045cd-319">Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="045cd-319">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="045cd-320">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="045cd-320">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="045cd-321">Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="045cd-321">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="045cd-322">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="045cd-322">This sample doesn't use a template.</span></span> <span data-ttu-id="045cd-323">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="045cd-323">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="045cd-324">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="045cd-324">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="045cd-325">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="045cd-325">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="045cd-326">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="045cd-326">Return values</span></span>

<span data-ttu-id="045cd-327">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="045cd-327">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="045cd-328">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="045cd-328">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="045cd-329">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="045cd-329">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="045cd-330">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="045cd-330">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="045cd-331">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="045cd-331">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="045cd-332">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="045cd-332">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="045cd-333">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="045cd-333">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="045cd-334">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="045cd-334">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="045cd-335">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="045cd-335">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="045cd-336">Metoda PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="045cd-336">The PutTodoItem method</span></span>

<span data-ttu-id="045cd-337">`PutTodoItem` Badanie metody:</span><span class="sxs-lookup"><span data-stu-id="045cd-337">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="045cd-338">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="045cd-338">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="045cd-339">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="045cd-339">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="045cd-340">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="045cd-340">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="045cd-341">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="045cd-341">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="045cd-342">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="045cd-342">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="045cd-343">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="045cd-343">Test the PutTodoItem method</span></span>

<span data-ttu-id="045cd-344">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="045cd-344">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="045cd-345">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-345">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="045cd-346">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="045cd-346">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="045cd-347">Zaktualizuj element do wykonania o IDENTYFIKATORze 1 i ustaw jego nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="045cd-347">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="045cd-348">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="045cd-348">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="045cd-350">Metoda DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="045cd-350">The DeleteTodoItem method</span></span>

<span data-ttu-id="045cd-351">`DeleteTodoItem` Badanie metody:</span><span class="sxs-lookup"><span data-stu-id="045cd-351">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="045cd-352">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="045cd-352">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="045cd-353">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="045cd-353">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="045cd-354">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="045cd-354">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="045cd-355">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="045cd-355">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="045cd-356">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="045cd-356">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="045cd-357">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="045cd-357">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="045cd-358">Wywoływanie interfejsu API z platformy jQuery</span><span class="sxs-lookup"><span data-stu-id="045cd-358">Call the API from jQuery</span></span>

<span data-ttu-id="045cd-359">Zobacz [samouczek: Wywołaj ASP.NET Core Web API z jQuery](xref:tutorials/web-api-jquery).</span><span class="sxs-lookup"><span data-stu-id="045cd-359">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="045cd-360">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="045cd-360">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="045cd-361">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="045cd-361">Create a web API project.</span></span>
> * <span data-ttu-id="045cd-362">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-362">Add a model class and a database context.</span></span>
> * <span data-ttu-id="045cd-363">Dodawanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="045cd-363">Add a controller.</span></span>
> * <span data-ttu-id="045cd-364">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="045cd-364">Add CRUD methods.</span></span>
> * <span data-ttu-id="045cd-365">Konfigurowanie routingu i ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="045cd-365">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="045cd-366">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="045cd-366">Specify return values.</span></span>
> * <span data-ttu-id="045cd-367">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="045cd-367">Call the web API with Postman.</span></span>
> * <span data-ttu-id="045cd-368">Wywołaj interfejs API sieci Web za pomocą platformy jQuery.</span><span class="sxs-lookup"><span data-stu-id="045cd-368">Call the web API with jQuery.</span></span>

<span data-ttu-id="045cd-369">Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-369">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="045cd-370">Omówienie</span><span class="sxs-lookup"><span data-stu-id="045cd-370">Overview</span></span>

<span data-ttu-id="045cd-371">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="045cd-371">This tutorial creates the following API:</span></span>

|<span data-ttu-id="045cd-372">interfejs API</span><span class="sxs-lookup"><span data-stu-id="045cd-372">API</span></span> | <span data-ttu-id="045cd-373">Opis</span><span class="sxs-lookup"><span data-stu-id="045cd-373">Description</span></span> | <span data-ttu-id="045cd-374">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="045cd-374">Request body</span></span> | <span data-ttu-id="045cd-375">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="045cd-375">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="045cd-376">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="045cd-376">GET /api/TodoItems</span></span> | <span data-ttu-id="045cd-377">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-377">Get all to-do items</span></span> | <span data-ttu-id="045cd-378">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-378">None</span></span> | <span data-ttu-id="045cd-379">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-379">Array of to-do items</span></span>|
|<span data-ttu-id="045cd-380">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="045cd-380">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="045cd-381">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="045cd-381">Get an item by ID</span></span> | <span data-ttu-id="045cd-382">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-382">None</span></span> | <span data-ttu-id="045cd-383">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-383">To-do item</span></span>|
|<span data-ttu-id="045cd-384">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="045cd-384">POST /api/TodoItems</span></span> | <span data-ttu-id="045cd-385">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="045cd-385">Add a new item</span></span> | <span data-ttu-id="045cd-386">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-386">To-do item</span></span> | <span data-ttu-id="045cd-387">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-387">To-do item</span></span> |
|<span data-ttu-id="045cd-388">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="045cd-388">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="045cd-389">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="045cd-389">Update an existing item &nbsp;</span></span> | <span data-ttu-id="045cd-390">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-390">To-do item</span></span> | <span data-ttu-id="045cd-391">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-391">None</span></span> |
|<span data-ttu-id="045cd-392">Usuń/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="045cd-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="045cd-393">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="045cd-393">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="045cd-394">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-394">None</span></span> | <span data-ttu-id="045cd-395">Brak</span><span class="sxs-lookup"><span data-stu-id="045cd-395">None</span></span>|

<span data-ttu-id="045cd-396">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="045cd-396">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="045cd-402">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="045cd-402">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-403">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045cd-404">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045cd-404">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="045cd-405">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-405">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="045cd-406">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="045cd-406">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-407">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-407">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="045cd-408">Z menu **plik** wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="045cd-408">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="045cd-409">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="045cd-409">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="045cd-410">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="045cd-410">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="045cd-411">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** .</span><span class="sxs-lookup"><span data-stu-id="045cd-411">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="045cd-412">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="045cd-412">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="045cd-413">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="045cd-413">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045cd-415">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045cd-415">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="045cd-416">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="045cd-416">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="045cd-417">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="045cd-417">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="045cd-418">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="045cd-418">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="045cd-419">Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="045cd-419">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="045cd-420">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="045cd-420">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="045cd-421">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-421">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="045cd-422">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="045cd-422">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="045cd-424">Wybierzpozycję **interfejs API** > > aplikacji.NET> Core.</span><span class="sxs-lookup"><span data-stu-id="045cd-424">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="045cd-426">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="045cd-426">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="045cd-427">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="045cd-427">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="045cd-429">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="045cd-429">Test the API</span></span>

<span data-ttu-id="045cd-430">Szablon projektu umożliwia utworzenie `values` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-430">The project template creates a `values` API.</span></span> <span data-ttu-id="045cd-431">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="045cd-431">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-432">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-432">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="045cd-433">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="045cd-433">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="045cd-434">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="045cd-434">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="045cd-435">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="045cd-435">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="045cd-436">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="045cd-436">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045cd-437">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045cd-437">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="045cd-438">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="045cd-438">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="045cd-439">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="045cd-439">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="045cd-440">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="045cd-441">Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="045cd-441">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="045cd-442">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="045cd-442">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="045cd-443">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="045cd-443">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="045cd-444">Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="045cd-444">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="045cd-445">Zwracane są następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="045cd-445">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="045cd-446">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="045cd-446">Add a model class</span></span>

<span data-ttu-id="045cd-447">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="045cd-447">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="045cd-448">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="045cd-448">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-449">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-449">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="045cd-450">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="045cd-450">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="045cd-451">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="045cd-451">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="045cd-452">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="045cd-452">Name the folder *Models*.</span></span>

* <span data-ttu-id="045cd-453">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="045cd-453">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="045cd-454">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="045cd-454">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="045cd-455">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-455">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045cd-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045cd-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="045cd-457">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="045cd-457">Add a folder named *Models*.</span></span>

* <span data-ttu-id="045cd-458">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-458">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="045cd-459">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="045cd-460">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="045cd-460">Right-click the project.</span></span> <span data-ttu-id="045cd-461">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="045cd-461">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="045cd-462">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="045cd-462">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="045cd-464">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.</span><span class="sxs-lookup"><span data-stu-id="045cd-464">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="045cd-465">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="045cd-465">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="045cd-466">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-466">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="045cd-467">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-467">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="045cd-468">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="045cd-468">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="045cd-469">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="045cd-469">Add a database context</span></span>

<span data-ttu-id="045cd-470">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="045cd-470">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="045cd-471">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="045cd-471">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-472">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-472">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="045cd-473">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="045cd-473">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="045cd-474">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="045cd-474">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="045cd-475">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-475">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="045cd-476">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="045cd-476">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="045cd-477">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-477">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="045cd-478">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="045cd-478">Register the database context</span></span>

<span data-ttu-id="045cd-479">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="045cd-479">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="045cd-480">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="045cd-480">The container provides the service to controllers.</span></span>

<span data-ttu-id="045cd-481">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="045cd-481">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="045cd-482">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="045cd-482">The preceding code:</span></span>

* <span data-ttu-id="045cd-483">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="045cd-483">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="045cd-484">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-484">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="045cd-485">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="045cd-485">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="045cd-486">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="045cd-486">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-487">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-487">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="045cd-488">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="045cd-488">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="045cd-489">Wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="045cd-489">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="045cd-490">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="045cd-490">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="045cd-491">Nazwa klasy *TodoController*i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="045cd-491">Name the class *TodoController*, and select **Add**.</span></span>

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="045cd-493">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-493">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="045cd-494">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="045cd-494">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="045cd-495">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-495">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="045cd-496">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="045cd-496">The preceding code:</span></span>

* <span data-ttu-id="045cd-497">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="045cd-497">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="045cd-498">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="045cd-498">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="045cd-499">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-499">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="045cd-500">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="045cd-500">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="045cd-501">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="045cd-501">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="045cd-502">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="045cd-502">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="045cd-503">Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta.</span><span class="sxs-lookup"><span data-stu-id="045cd-503">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="045cd-504">Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="045cd-504">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="045cd-505">Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-505">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="045cd-506">Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.</span><span class="sxs-lookup"><span data-stu-id="045cd-506">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="045cd-507">Dodaj metody Get</span><span class="sxs-lookup"><span data-stu-id="045cd-507">Add Get methods</span></span>

<span data-ttu-id="045cd-508">Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="045cd-508">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="045cd-509">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="045cd-509">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="045cd-510">Zatrzymaj aplikację, jeśli jest nadal uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="045cd-510">Stop the app if it's still running.</span></span> <span data-ttu-id="045cd-511">Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.</span><span class="sxs-lookup"><span data-stu-id="045cd-511">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="045cd-512">Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="045cd-512">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="045cd-513">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="045cd-513">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="045cd-514">Następującą odpowiedź HTTP jest tworzony przez wywołanie metody `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="045cd-514">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="045cd-515">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="045cd-515">Routing and URL paths</span></span>

<span data-ttu-id="045cd-516">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="045cd-516">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="045cd-517">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="045cd-517">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="045cd-518">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="045cd-518">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="045cd-519">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="045cd-519">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="045cd-520">W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera, więc nazwa kontrolera jest "todo".</span><span class="sxs-lookup"><span data-stu-id="045cd-520">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="045cd-521">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="045cd-521">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="045cd-522">Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="045cd-522">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="045cd-523">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="045cd-523">This sample doesn't use a template.</span></span> <span data-ttu-id="045cd-524">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="045cd-524">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="045cd-525">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="045cd-525">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="045cd-526">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="045cd-526">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="045cd-527">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="045cd-527">Return values</span></span>

<span data-ttu-id="045cd-528">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="045cd-528">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="045cd-529">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="045cd-529">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="045cd-530">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="045cd-530">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="045cd-531">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="045cd-531">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="045cd-532">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="045cd-532">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="045cd-533">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="045cd-533">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="045cd-534">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="045cd-534">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="045cd-535">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="045cd-535">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="045cd-536">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="045cd-536">Returning `item` results in an HTTP 200 response.</span></span>


## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="045cd-537">Metoda GetTodoItems testu</span><span class="sxs-lookup"><span data-stu-id="045cd-537">Test the GetTodoItems method</span></span>

<span data-ttu-id="045cd-538">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-538">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="045cd-539">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="045cd-539">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="045cd-540">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="045cd-540">Start the web app.</span></span>
* <span data-ttu-id="045cd-541">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="045cd-541">Start Postman.</span></span>
* <span data-ttu-id="045cd-542">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="045cd-542">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045cd-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045cd-543">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="045cd-544">W obszarze **Ustawienia** **pliku** > (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="045cd-544">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="045cd-545">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="045cd-545">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="045cd-546">Z > poziomu**preferencji** programu Poster (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="045cd-546">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="045cd-547">Alternatywnie wybierz klucz i wybierz pozycję **Ustawienia**, a następnie wyłącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="045cd-547">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="045cd-548">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="045cd-548">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="045cd-549">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="045cd-549">Create a new request.</span></span>
  * <span data-ttu-id="045cd-550">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="045cd-550">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="045cd-551">Ustaw adres URL żądania `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="045cd-551">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="045cd-552">Na przykład `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="045cd-552">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="045cd-553">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="045cd-553">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="045cd-554">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="045cd-554">Select **Send**.</span></span>

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="045cd-556">Dodawanie metody Create</span><span class="sxs-lookup"><span data-stu-id="045cd-556">Add a Create method</span></span>

<span data-ttu-id="045cd-557">Dodaj następującą `PostTodoItem` metodę w obszarze *controllers/TodoController. cs*:</span><span class="sxs-lookup"><span data-stu-id="045cd-557">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="045cd-558">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="045cd-558">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="045cd-559">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="045cd-559">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="045cd-560">`CreatedAtAction` Metody:</span><span class="sxs-lookup"><span data-stu-id="045cd-560">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="045cd-561">Zwraca kod stanu HTTP 201, jeśli powodzenie.</span><span class="sxs-lookup"><span data-stu-id="045cd-561">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="045cd-562">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="045cd-562">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="045cd-563">`Location` Dodaje nagłówek do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="045cd-563">Adds a `Location` header to the response.</span></span> <span data-ttu-id="045cd-564">`Location` Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="045cd-564">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="045cd-565">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="045cd-565">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="045cd-566">Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka.</span><span class="sxs-lookup"><span data-stu-id="045cd-566">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="045cd-567">Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="045cd-567">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="045cd-568">Metoda PostTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="045cd-568">Test the PostTodoItem method</span></span>

* <span data-ttu-id="045cd-569">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="045cd-569">Build the project.</span></span>
* <span data-ttu-id="045cd-570">W narzędziu Postman, Ustawia metodę HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="045cd-570">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="045cd-571">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="045cd-571">Select the **Body** tab.</span></span>
* <span data-ttu-id="045cd-572">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="045cd-572">Select the **raw** radio button.</span></span>
* <span data-ttu-id="045cd-573">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="045cd-573">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="045cd-574">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="045cd-574">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="045cd-575">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="045cd-575">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  <span data-ttu-id="045cd-577">Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu `PostTodoItem` metody.</span><span class="sxs-lookup"><span data-stu-id="045cd-577">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="045cd-578">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="045cd-578">Test the location header URI</span></span>

* <span data-ttu-id="045cd-579">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="045cd-579">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="045cd-580">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="045cd-580">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="045cd-582">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="045cd-582">Set the method to GET.</span></span>
* <span data-ttu-id="045cd-583">Wklej identyfikator URI (na przykład `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="045cd-583">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="045cd-584">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="045cd-584">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="045cd-585">Dodaj metodę PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="045cd-585">Add a PutTodoItem method</span></span>

<span data-ttu-id="045cd-586">Dodaj następujący kod `PutTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="045cd-586">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="045cd-587">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="045cd-587">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="045cd-588">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="045cd-588">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="045cd-589">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="045cd-589">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="045cd-590">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="045cd-590">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="045cd-591">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="045cd-591">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="045cd-592">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="045cd-592">Test the PutTodoItem method</span></span>

<span data-ttu-id="045cd-593">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="045cd-593">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="045cd-594">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="045cd-594">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="045cd-595">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="045cd-595">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="045cd-596">Zaktualizuj element zadania do wykonania, który ma identyfikator = 1 i ustaw jego nazwę na "feed ryb":</span><span class="sxs-lookup"><span data-stu-id="045cd-596">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="045cd-597">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="045cd-597">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="045cd-599">Dodaj metodę DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="045cd-599">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="045cd-600">Dodaj następujący kod `DeleteTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="045cd-600">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="045cd-601">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="045cd-601">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="045cd-602">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="045cd-602">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="045cd-603">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="045cd-603">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="045cd-604">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="045cd-604">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="045cd-605">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="045cd-605">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="045cd-606">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="045cd-606">Select **Send**</span></span>

<span data-ttu-id="045cd-607">Przykładowa aplikacja umożliwia usunięcie wszystkich elementów.</span><span class="sxs-lookup"><span data-stu-id="045cd-607">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="045cd-608">Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-608">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="045cd-609">Wywoływanie interfejsu API przy użyciu jQuery</span><span class="sxs-lookup"><span data-stu-id="045cd-609">Call the API with jQuery</span></span>

<span data-ttu-id="045cd-610">W tej sekcji strony HTML jest dodawany, który używa technologii jQuery do wywołania sieci web interfejsu api.</span><span class="sxs-lookup"><span data-stu-id="045cd-610">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="045cd-611">jQuery inicjuje żądanie i aktualizowanie strony ze szczegółami z odpowiedzi interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-611">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="045cd-612">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-612">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="045cd-613">Tworzenie *wwwroot* folder w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="045cd-613">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="045cd-614">Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="045cd-614">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="045cd-615">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-615">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="045cd-616">Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="045cd-616">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="045cd-617">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="045cd-617">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="045cd-618">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="045cd-618">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="045cd-619">Otwórz *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="045cd-619">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="045cd-620">Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.</span><span class="sxs-lookup"><span data-stu-id="045cd-620">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="045cd-621">Istnieje kilka sposobów uzyskania biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="045cd-621">There are several ways to get jQuery.</span></span> <span data-ttu-id="045cd-622">W poprzednim fragmencie kodu biblioteki jest ładowany z usługi CDN.</span><span class="sxs-lookup"><span data-stu-id="045cd-622">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="045cd-623">Ten przykład wywołuje wszystkie metody CRUD interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-623">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="045cd-624">Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="045cd-624">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="045cd-625">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-625">Get a list of to-do items</span></span>

<span data-ttu-id="045cd-626">JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `GET` żądanie do interfejsu API, która zwraca wartość JSON reprezentująca tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="045cd-626">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="045cd-627">`success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="045cd-627">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="045cd-628">Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="045cd-628">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="045cd-629">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-629">Add a to-do item</span></span>

<span data-ttu-id="045cd-630">[Ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `POST` żądania z elementem zadań do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="045cd-630">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="045cd-631">`accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych.</span><span class="sxs-lookup"><span data-stu-id="045cd-631">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="045cd-632">Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="045cd-632">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="045cd-633">Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="045cd-633">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="045cd-634">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-634">Update a to-do item</span></span>

<span data-ttu-id="045cd-635">Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="045cd-635">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="045cd-636">`url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="045cd-636">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="045cd-637">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="045cd-637">Delete a to-do item</span></span>

<span data-ttu-id="045cd-638">Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="045cd-638">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="045cd-639">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="045cd-639">Additional resources</span></span>

<span data-ttu-id="045cd-640">[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="045cd-640">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="045cd-641">Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="045cd-641">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="045cd-642">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="045cd-642">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="045cd-643">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="045cd-643">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
