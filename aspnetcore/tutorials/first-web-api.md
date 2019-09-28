---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 366323416061bf729c092419f2f6a5912884252b
ms.sourcegitcommit: 5d25a7f22c50ca6fdd0f8ecd8e525822e1b35b7a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2019
ms.locfileid: "71551734"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="5c407-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c407-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="5c407-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="5c407-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="5c407-105">W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c407-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5c407-106">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5c407-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5c407-107">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5c407-107">Create a web API project.</span></span>
> * <span data-ttu-id="5c407-108">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="5c407-109">Tworzy szkielet kontrolera z metodami CRUD.</span><span class="sxs-lookup"><span data-stu-id="5c407-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="5c407-110">Skonfiguruj Routing, ścieżki URL i wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="5c407-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="5c407-111">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="5c407-111">Call the web API with Postman.</span></span>

<span data-ttu-id="5c407-112">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="5c407-113">Przegląd</span><span class="sxs-lookup"><span data-stu-id="5c407-113">Overview</span></span>

<span data-ttu-id="5c407-114">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="5c407-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="5c407-115">interfejs API</span><span class="sxs-lookup"><span data-stu-id="5c407-115">API</span></span> | <span data-ttu-id="5c407-116">Opis</span><span class="sxs-lookup"><span data-stu-id="5c407-116">Description</span></span> | <span data-ttu-id="5c407-117">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="5c407-117">Request body</span></span> | <span data-ttu-id="5c407-118">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="5c407-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="5c407-119">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="5c407-119">GET /api/TodoItems</span></span> | <span data-ttu-id="5c407-120">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-120">Get all to-do items</span></span> | <span data-ttu-id="5c407-121">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-121">None</span></span> | <span data-ttu-id="5c407-122">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-122">Array of to-do items</span></span>|
|<span data-ttu-id="5c407-123">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="5c407-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="5c407-124">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="5c407-124">Get an item by ID</span></span> | <span data-ttu-id="5c407-125">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-125">None</span></span> | <span data-ttu-id="5c407-126">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-126">To-do item</span></span>|
|<span data-ttu-id="5c407-127">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="5c407-127">POST /api/TodoItems</span></span> | <span data-ttu-id="5c407-128">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="5c407-128">Add a new item</span></span> | <span data-ttu-id="5c407-129">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-129">To-do item</span></span> | <span data-ttu-id="5c407-130">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-130">To-do item</span></span> |
|<span data-ttu-id="5c407-131">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="5c407-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="5c407-132">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="5c407-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="5c407-133">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-133">To-do item</span></span> | <span data-ttu-id="5c407-134">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-134">None</span></span> |
|<span data-ttu-id="5c407-135">Usuń/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="5c407-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="5c407-136">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="5c407-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="5c407-137">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-137">None</span></span> | <span data-ttu-id="5c407-138">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-138">None</span></span>|

<span data-ttu-id="5c407-139">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c407-139">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="5c407-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5c407-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c407-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c407-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5c407-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="5c407-149">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="5c407-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5c407-151">Z menu **plik** wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="5c407-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5c407-152">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="5c407-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="5c407-153">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5c407-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="5c407-154">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="5c407-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="5c407-155">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5c407-155">Select the **API** template and click **Create**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c407-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c407-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5c407-158">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="5c407-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="5c407-159">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="5c407-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="5c407-160">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c407-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="5c407-161">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="5c407-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="5c407-162">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c407-162">The preceding commands:</span></span>

  * <span data-ttu-id="5c407-163">Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5c407-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="5c407-164">Dodaje pakiety NuGet, które są wymagane w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="5c407-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5c407-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5c407-166">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5c407-166">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="5c407-168">Wybierz pozycję **interfejs API** > > aplikacji> .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c407-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="5c407-170">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję **docelowa platforma** \* *.NET Core 3,0*.</span><span class="sxs-lookup"><span data-stu-id="5c407-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="5c407-171">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5c407-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="5c407-173">Otwórz Terminal poleceń w folderze projektu i uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c407-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="5c407-174">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="5c407-174">Test the API</span></span>

<span data-ttu-id="5c407-175">Szablon projektu umożliwia utworzenie `WeatherForecast` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="5c407-176">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="5c407-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5c407-178">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="5c407-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="5c407-179">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="5c407-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="5c407-180">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="5c407-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="5c407-181">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="5c407-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c407-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c407-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5c407-183">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="5c407-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="5c407-184">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="5c407-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5c407-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5c407-186">Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="5c407-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="5c407-187">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="5c407-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="5c407-188">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="5c407-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="5c407-189">Dołącz `/WeatherForecast` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="5c407-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="5c407-190">Zwracany jest kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="5c407-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="5c407-191">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="5c407-191">Add a model class</span></span>

<span data-ttu-id="5c407-192">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c407-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="5c407-193">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="5c407-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5c407-195">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="5c407-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="5c407-196">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5c407-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="5c407-197">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5c407-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="5c407-198">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="5c407-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="5c407-199">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5c407-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="5c407-200">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c407-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c407-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5c407-202">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5c407-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="5c407-203">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5c407-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5c407-205">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="5c407-205">Right-click the project.</span></span> <span data-ttu-id="5c407-206">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5c407-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="5c407-207">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5c407-207">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="5c407-209">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.</span><span class="sxs-lookup"><span data-stu-id="5c407-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="5c407-210">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="5c407-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="5c407-211">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="5c407-212">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="5c407-213">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="5c407-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="5c407-214">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="5c407-214">Add a database context</span></span>

<span data-ttu-id="5c407-215">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c407-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="5c407-216">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="5c407-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="5c407-218">Dodaj Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="5c407-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="5c407-219">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="5c407-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="5c407-220">Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="5c407-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="5c407-221">W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="5c407-221">Select  **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="5c407-222">Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="5c407-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="5c407-223">Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="5c407-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="5c407-225">Dodawanie kontekstu bazy danych TodoContext</span><span class="sxs-lookup"><span data-stu-id="5c407-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="5c407-226">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="5c407-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="5c407-227">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5c407-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5c407-228">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="5c407-229">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="5c407-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="5c407-230">Wprowadź następujący kod:</span><span class="sxs-lookup"><span data-stu-id="5c407-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="5c407-231">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="5c407-231">Register the database context</span></span>

<span data-ttu-id="5c407-232">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="5c407-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="5c407-233">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="5c407-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="5c407-234">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="5c407-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="5c407-235">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="5c407-235">The preceding code:</span></span>

* <span data-ttu-id="5c407-236">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="5c407-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="5c407-237">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="5c407-238">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="5c407-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="5c407-239">Tworzenie szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="5c407-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5c407-241">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="5c407-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="5c407-242">Wybierz pozycję **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="5c407-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="5c407-243">Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5c407-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="5c407-244">Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:</span><span class="sxs-lookup"><span data-stu-id="5c407-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="5c407-245">Wybierz pozycję **TodoItem (TodoApi. models)** w **klasie model**.</span><span class="sxs-lookup"><span data-stu-id="5c407-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="5c407-246">W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoApi. models)** .</span><span class="sxs-lookup"><span data-stu-id="5c407-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="5c407-247">Wybierz pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="5c407-247">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5c407-248">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="5c407-249">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c407-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="5c407-250">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c407-250">The preceding commands:</span></span>

* <span data-ttu-id="5c407-251">Dodaj pakiety NuGet wymagane do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="5c407-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="5c407-252">Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="5c407-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="5c407-253">Szkielety `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="5c407-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="5c407-254">Wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="5c407-254">The generated code:</span></span>

* <span data-ttu-id="5c407-255">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="5c407-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="5c407-256">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="5c407-256">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="5c407-257">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="5c407-258">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="5c407-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="5c407-259">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5c407-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="5c407-260">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="5c407-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="5c407-261">Badanie metody PostTodoItem Create</span><span class="sxs-lookup"><span data-stu-id="5c407-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="5c407-262">Zastąp instrukcję return w, `PostTodoItem` aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="5c407-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="5c407-263">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5c407-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5c407-264">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c407-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="5c407-265"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Metody:</span><span class="sxs-lookup"><span data-stu-id="5c407-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="5c407-266">W razie powodzenia zwraca kod stanu HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="5c407-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="5c407-267">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="5c407-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="5c407-268">Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5c407-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="5c407-269">Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania. [](https://developer.mozilla.org/docs/Glossary/URI) `Location`</span><span class="sxs-lookup"><span data-stu-id="5c407-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="5c407-270">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5c407-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="5c407-271">Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka.</span><span class="sxs-lookup"><span data-stu-id="5c407-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="5c407-272">Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="5c407-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="5c407-273">Zainstaluj program Poster</span><span class="sxs-lookup"><span data-stu-id="5c407-273">Install Postman</span></span>

<span data-ttu-id="5c407-274">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="5c407-275">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="5c407-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="5c407-276">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="5c407-276">Start the web app.</span></span>
* <span data-ttu-id="5c407-277">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="5c407-277">Start Postman.</span></span>
* <span data-ttu-id="5c407-278">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="5c407-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="5c407-279">Z **Plik > Ustawienia** (\**ogólne* karty), wyłącz **weryfikacji certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="5c407-279">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="5c407-280">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5c407-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="5c407-281">Test PostTodoItem za pomocą programu Poster</span><span class="sxs-lookup"><span data-stu-id="5c407-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="5c407-282">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="5c407-282">Create a new request.</span></span>
* <span data-ttu-id="5c407-283">Ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="5c407-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="5c407-284">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="5c407-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="5c407-285">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="5c407-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="5c407-286">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="5c407-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="5c407-287">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="5c407-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="5c407-288">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="5c407-288">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="5c407-290">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="5c407-290">Test the location header URI</span></span>

* <span data-ttu-id="5c407-291">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="5c407-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="5c407-292">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="5c407-292">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="5c407-294">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="5c407-294">Set the method to GET.</span></span>
* <span data-ttu-id="5c407-295">Wklej identyfikator URI (na przykład `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="5c407-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="5c407-296">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="5c407-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="5c407-297">Badanie metod GET</span><span class="sxs-lookup"><span data-stu-id="5c407-297">Examine the GET methods</span></span>

<span data-ttu-id="5c407-298">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="5c407-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="5c407-299">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu.</span><span class="sxs-lookup"><span data-stu-id="5c407-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="5c407-300">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5c407-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="5c407-301">Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="5c407-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="5c407-302">Test get przy użyciu programu Poster</span><span class="sxs-lookup"><span data-stu-id="5c407-302">Test Get with Postman</span></span>

* <span data-ttu-id="5c407-303">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="5c407-303">Create a new request.</span></span>
* <span data-ttu-id="5c407-304">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="5c407-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="5c407-305">Ustaw adres URL żądania `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="5c407-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="5c407-306">Na przykład `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="5c407-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="5c407-307">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="5c407-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="5c407-308">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="5c407-308">Select **Send**.</span></span>

<span data-ttu-id="5c407-309">Ta aplikacja używa bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="5c407-309">This app uses an in-memory database.</span></span> <span data-ttu-id="5c407-310">Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="5c407-311">Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c407-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="5c407-312">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="5c407-312">Routing and URL paths</span></span>

<span data-ttu-id="5c407-313">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="5c407-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="5c407-314">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5c407-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="5c407-315">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="5c407-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="5c407-316">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="5c407-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="5c407-317">Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="5c407-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="5c407-318">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5c407-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="5c407-319">Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="5c407-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="5c407-320">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="5c407-320">This sample doesn't use a template.</span></span> <span data-ttu-id="5c407-321">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="5c407-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="5c407-322">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="5c407-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="5c407-323">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="5c407-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="5c407-324">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="5c407-324">Return values</span></span>

<span data-ttu-id="5c407-325">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="5c407-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="5c407-326">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5c407-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="5c407-327">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="5c407-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="5c407-328">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="5c407-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="5c407-329">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c407-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="5c407-330">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="5c407-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="5c407-331">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="5c407-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="5c407-332">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="5c407-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="5c407-333">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="5c407-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="5c407-334">Metoda PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="5c407-334">The PutTodoItem method</span></span>

<span data-ttu-id="5c407-335">`PutTodoItem` Badanie metody:</span><span class="sxs-lookup"><span data-stu-id="5c407-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="5c407-336">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="5c407-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="5c407-337">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5c407-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5c407-338">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="5c407-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="5c407-339">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="5c407-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="5c407-340">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="5c407-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="5c407-341">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="5c407-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="5c407-342">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="5c407-342">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="5c407-343">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="5c407-344">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="5c407-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="5c407-345">Zaktualizuj element do wykonania o IDENTYFIKATORze 1 i ustaw jego nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="5c407-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="5c407-346">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="5c407-346">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="5c407-348">Metoda DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="5c407-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="5c407-349">`DeleteTodoItem` Badanie metody:</span><span class="sxs-lookup"><span data-stu-id="5c407-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="5c407-350">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5c407-350">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="5c407-351">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="5c407-351">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="5c407-352">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="5c407-352">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="5c407-353">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="5c407-353">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="5c407-354">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="5c407-354">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="5c407-355">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="5c407-355">Select **Send**</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="5c407-356">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="5c407-356">Call the web API with JavaScript</span></span>

<span data-ttu-id="5c407-357">Zobacz [samouczek: Wywołaj ASP.NET Core interfejs API sieci Web](xref:tutorials/web-api-javascript)przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5c407-357">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5c407-358">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5c407-358">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5c407-359">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5c407-359">Create a web API project.</span></span>
> * <span data-ttu-id="5c407-360">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-360">Add a model class and a database context.</span></span>
> * <span data-ttu-id="5c407-361">Dodawanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5c407-361">Add a controller.</span></span>
> * <span data-ttu-id="5c407-362">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="5c407-362">Add CRUD methods.</span></span>
> * <span data-ttu-id="5c407-363">Konfigurowanie routingu i ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="5c407-363">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="5c407-364">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="5c407-364">Specify return values.</span></span>
> * <span data-ttu-id="5c407-365">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="5c407-365">Call the web API with Postman.</span></span>
> * <span data-ttu-id="5c407-366">Wywołaj interfejs API sieci Web za pomocą języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5c407-366">Call the web API with JavaScript.</span></span>

<span data-ttu-id="5c407-367">Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-367">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="5c407-368">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5c407-368">Overview</span></span>

<span data-ttu-id="5c407-369">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="5c407-369">This tutorial creates the following API:</span></span>

|<span data-ttu-id="5c407-370">interfejs API</span><span class="sxs-lookup"><span data-stu-id="5c407-370">API</span></span> | <span data-ttu-id="5c407-371">Opis</span><span class="sxs-lookup"><span data-stu-id="5c407-371">Description</span></span> | <span data-ttu-id="5c407-372">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="5c407-372">Request body</span></span> | <span data-ttu-id="5c407-373">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="5c407-373">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="5c407-374">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="5c407-374">GET /api/TodoItems</span></span> | <span data-ttu-id="5c407-375">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-375">Get all to-do items</span></span> | <span data-ttu-id="5c407-376">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-376">None</span></span> | <span data-ttu-id="5c407-377">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-377">Array of to-do items</span></span>|
|<span data-ttu-id="5c407-378">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="5c407-378">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="5c407-379">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="5c407-379">Get an item by ID</span></span> | <span data-ttu-id="5c407-380">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-380">None</span></span> | <span data-ttu-id="5c407-381">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-381">To-do item</span></span>|
|<span data-ttu-id="5c407-382">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="5c407-382">POST /api/TodoItems</span></span> | <span data-ttu-id="5c407-383">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="5c407-383">Add a new item</span></span> | <span data-ttu-id="5c407-384">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-384">To-do item</span></span> | <span data-ttu-id="5c407-385">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-385">To-do item</span></span> |
|<span data-ttu-id="5c407-386">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="5c407-386">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="5c407-387">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="5c407-387">Update an existing item &nbsp;</span></span> | <span data-ttu-id="5c407-388">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-388">To-do item</span></span> | <span data-ttu-id="5c407-389">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-389">None</span></span> |
|<span data-ttu-id="5c407-390">Usuń/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="5c407-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="5c407-391">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="5c407-391">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="5c407-392">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-392">None</span></span> | <span data-ttu-id="5c407-393">Brak</span><span class="sxs-lookup"><span data-stu-id="5c407-393">None</span></span>|

<span data-ttu-id="5c407-394">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c407-394">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="5c407-400">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5c407-400">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-401">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-401">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c407-402">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c407-402">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5c407-403">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-403">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="5c407-404">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="5c407-404">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-405">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-405">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5c407-406">Z menu **plik** wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="5c407-406">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5c407-407">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="5c407-407">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="5c407-408">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5c407-408">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="5c407-409">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** .</span><span class="sxs-lookup"><span data-stu-id="5c407-409">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="5c407-410">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5c407-410">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="5c407-411">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="5c407-411">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c407-413">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c407-413">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5c407-414">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="5c407-414">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="5c407-415">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="5c407-415">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="5c407-416">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c407-416">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="5c407-417">Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="5c407-417">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="5c407-418">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="5c407-418">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5c407-419">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-419">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5c407-420">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5c407-420">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="5c407-422">Wybierz pozycję **interfejs API** > > aplikacji> .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c407-422">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="5c407-424">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="5c407-424">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="5c407-425">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5c407-425">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="5c407-427">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="5c407-427">Test the API</span></span>

<span data-ttu-id="5c407-428">Szablon projektu umożliwia utworzenie `values` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-428">The project template creates a `values` API.</span></span> <span data-ttu-id="5c407-429">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="5c407-429">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-430">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-430">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5c407-431">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="5c407-431">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="5c407-432">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="5c407-432">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="5c407-433">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="5c407-433">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="5c407-434">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="5c407-434">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c407-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c407-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5c407-436">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="5c407-436">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="5c407-437">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="5c407-437">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5c407-438">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-438">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5c407-439">Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="5c407-439">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="5c407-440">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="5c407-440">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="5c407-441">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="5c407-441">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="5c407-442">Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="5c407-442">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="5c407-443">Zwracane są następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="5c407-443">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="5c407-444">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="5c407-444">Add a model class</span></span>

<span data-ttu-id="5c407-445">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c407-445">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="5c407-446">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="5c407-446">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-447">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-447">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5c407-448">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="5c407-448">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="5c407-449">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5c407-449">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="5c407-450">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5c407-450">Name the folder *Models*.</span></span>

* <span data-ttu-id="5c407-451">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="5c407-451">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="5c407-452">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5c407-452">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="5c407-453">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-453">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c407-454">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c407-454">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5c407-455">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5c407-455">Add a folder named *Models*.</span></span>

* <span data-ttu-id="5c407-456">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-456">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5c407-457">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-457">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5c407-458">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="5c407-458">Right-click the project.</span></span> <span data-ttu-id="5c407-459">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="5c407-459">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="5c407-460">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="5c407-460">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="5c407-462">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.</span><span class="sxs-lookup"><span data-stu-id="5c407-462">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="5c407-463">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="5c407-463">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="5c407-464">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-464">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="5c407-465">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-465">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="5c407-466">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="5c407-466">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="5c407-467">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="5c407-467">Add a database context</span></span>

<span data-ttu-id="5c407-468">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c407-468">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="5c407-469">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="5c407-469">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-470">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-470">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5c407-471">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="5c407-471">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="5c407-472">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5c407-472">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5c407-473">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-473">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="5c407-474">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="5c407-474">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="5c407-475">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-475">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="5c407-476">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="5c407-476">Register the database context</span></span>

<span data-ttu-id="5c407-477">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="5c407-477">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="5c407-478">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="5c407-478">The container provides the service to controllers.</span></span>

<span data-ttu-id="5c407-479">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="5c407-479">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="5c407-480">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="5c407-480">The preceding code:</span></span>

* <span data-ttu-id="5c407-481">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="5c407-481">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="5c407-482">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-482">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="5c407-483">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="5c407-483">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="5c407-484">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="5c407-484">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-485">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-485">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5c407-486">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="5c407-486">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="5c407-487">Wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="5c407-487">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="5c407-488">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="5c407-488">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="5c407-489">Nazwa klasy *TodoController*i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5c407-489">Name the class *TodoController*, and select **Add**.</span></span>

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5c407-491">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-491">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="5c407-492">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="5c407-492">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="5c407-493">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-493">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="5c407-494">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="5c407-494">The preceding code:</span></span>

* <span data-ttu-id="5c407-495">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="5c407-495">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="5c407-496">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="5c407-496">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="5c407-497">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-497">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="5c407-498">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="5c407-498">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="5c407-499">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5c407-499">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="5c407-500">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="5c407-500">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="5c407-501">Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta.</span><span class="sxs-lookup"><span data-stu-id="5c407-501">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="5c407-502">Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c407-502">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="5c407-503">Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-503">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="5c407-504">Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.</span><span class="sxs-lookup"><span data-stu-id="5c407-504">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="5c407-505">Dodaj metody Get</span><span class="sxs-lookup"><span data-stu-id="5c407-505">Add Get methods</span></span>

<span data-ttu-id="5c407-506">Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="5c407-506">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="5c407-507">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="5c407-507">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="5c407-508">Zatrzymaj aplikację, jeśli jest nadal uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="5c407-508">Stop the app if it's still running.</span></span> <span data-ttu-id="5c407-509">Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.</span><span class="sxs-lookup"><span data-stu-id="5c407-509">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="5c407-510">Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="5c407-510">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="5c407-511">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5c407-511">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="5c407-512">Następującą odpowiedź HTTP jest tworzony przez wywołanie metody `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="5c407-512">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="5c407-513">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="5c407-513">Routing and URL paths</span></span>

<span data-ttu-id="5c407-514">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="5c407-514">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="5c407-515">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5c407-515">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="5c407-516">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="5c407-516">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="5c407-517">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="5c407-517">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="5c407-518">W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera, więc nazwa kontrolera jest "todo".</span><span class="sxs-lookup"><span data-stu-id="5c407-518">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="5c407-519">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5c407-519">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="5c407-520">Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="5c407-520">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="5c407-521">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="5c407-521">This sample doesn't use a template.</span></span> <span data-ttu-id="5c407-522">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="5c407-522">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="5c407-523">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="5c407-523">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="5c407-524">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="5c407-524">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="5c407-525">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="5c407-525">Return values</span></span>

<span data-ttu-id="5c407-526">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="5c407-526">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="5c407-527">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5c407-527">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="5c407-528">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="5c407-528">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="5c407-529">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="5c407-529">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="5c407-530">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c407-530">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="5c407-531">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="5c407-531">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="5c407-532">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="5c407-532">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="5c407-533">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="5c407-533">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="5c407-534">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="5c407-534">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="5c407-535">Metoda GetTodoItems testu</span><span class="sxs-lookup"><span data-stu-id="5c407-535">Test the GetTodoItems method</span></span>

<span data-ttu-id="5c407-536">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-536">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="5c407-537">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="5c407-537">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="5c407-538">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="5c407-538">Start the web app.</span></span>
* <span data-ttu-id="5c407-539">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="5c407-539">Start Postman.</span></span>
* <span data-ttu-id="5c407-540">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="5c407-540">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c407-541">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c407-541">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5c407-542">W obszarze **Ustawienia** **pliku** > (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="5c407-542">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5c407-543">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5c407-543">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="5c407-544">Z poziomu**preferencji** programu **Poster** > (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="5c407-544">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="5c407-545">Alternatywnie wybierz klucz i wybierz pozycję **Ustawienia**, a następnie wyłącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="5c407-545">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="5c407-546">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5c407-546">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="5c407-547">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="5c407-547">Create a new request.</span></span>
  * <span data-ttu-id="5c407-548">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="5c407-548">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="5c407-549">Ustaw adres URL żądania `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="5c407-549">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="5c407-550">Na przykład `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="5c407-550">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="5c407-551">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="5c407-551">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="5c407-552">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="5c407-552">Select **Send**.</span></span>

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="5c407-554">Dodawanie metody Create</span><span class="sxs-lookup"><span data-stu-id="5c407-554">Add a Create method</span></span>

<span data-ttu-id="5c407-555">Dodaj następującą `PostTodoItem` metodę w obszarze *controllers/TodoController. cs*:</span><span class="sxs-lookup"><span data-stu-id="5c407-555">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5c407-556">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5c407-556">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5c407-557">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c407-557">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="5c407-558">`CreatedAtAction` Metody:</span><span class="sxs-lookup"><span data-stu-id="5c407-558">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="5c407-559">Zwraca kod stanu HTTP 201, jeśli powodzenie.</span><span class="sxs-lookup"><span data-stu-id="5c407-559">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="5c407-560">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="5c407-560">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="5c407-561">`Location` Dodaje nagłówek do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5c407-561">Adds a `Location` header to the response.</span></span> <span data-ttu-id="5c407-562">`Location` Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="5c407-562">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="5c407-563">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5c407-563">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="5c407-564">Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka.</span><span class="sxs-lookup"><span data-stu-id="5c407-564">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="5c407-565">Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="5c407-565">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="5c407-566">Metoda PostTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="5c407-566">Test the PostTodoItem method</span></span>

* <span data-ttu-id="5c407-567">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="5c407-567">Build the project.</span></span>
* <span data-ttu-id="5c407-568">W narzędziu Postman, Ustawia metodę HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="5c407-568">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="5c407-569">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="5c407-569">Select the **Body** tab.</span></span>
* <span data-ttu-id="5c407-570">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="5c407-570">Select the **raw** radio button.</span></span>
* <span data-ttu-id="5c407-571">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="5c407-571">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="5c407-572">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="5c407-572">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="5c407-573">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="5c407-573">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  <span data-ttu-id="5c407-575">Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu `PostTodoItem` metody.</span><span class="sxs-lookup"><span data-stu-id="5c407-575">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="5c407-576">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="5c407-576">Test the location header URI</span></span>

* <span data-ttu-id="5c407-577">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="5c407-577">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="5c407-578">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="5c407-578">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="5c407-580">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="5c407-580">Set the method to GET.</span></span>
* <span data-ttu-id="5c407-581">Wklej identyfikator URI (na przykład `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="5c407-581">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="5c407-582">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="5c407-582">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="5c407-583">Dodaj metodę PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="5c407-583">Add a PutTodoItem method</span></span>

<span data-ttu-id="5c407-584">Dodaj następujący kod `PutTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="5c407-584">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="5c407-585">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="5c407-585">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="5c407-586">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5c407-586">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5c407-587">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="5c407-587">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="5c407-588">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="5c407-588">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="5c407-589">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="5c407-589">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="5c407-590">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="5c407-590">Test the PutTodoItem method</span></span>

<span data-ttu-id="5c407-591">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="5c407-591">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="5c407-592">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5c407-592">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="5c407-593">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="5c407-593">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="5c407-594">Zaktualizuj element zadania do wykonania, który ma identyfikator = 1 i ustaw jego nazwę na "feed ryb":</span><span class="sxs-lookup"><span data-stu-id="5c407-594">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="5c407-595">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="5c407-595">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="5c407-597">Dodaj metodę DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="5c407-597">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="5c407-598">Dodaj następujący kod `DeleteTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="5c407-598">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="5c407-599">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5c407-599">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="5c407-600">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="5c407-600">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="5c407-601">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="5c407-601">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="5c407-602">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="5c407-602">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="5c407-603">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="5c407-603">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="5c407-604">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="5c407-604">Select **Send**</span></span>

<span data-ttu-id="5c407-605">Przykładowa aplikacja umożliwia usunięcie wszystkich elementów.</span><span class="sxs-lookup"><span data-stu-id="5c407-605">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="5c407-606">Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-606">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="5c407-607">Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="5c407-607">Call the web API with JavaScript</span></span>

<span data-ttu-id="5c407-608">W tej sekcji zostanie dodana strona HTML, która używa języka JavaScript do wywoływania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-608">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="5c407-609">Interfejs API pobierania inicjuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="5c407-609">The Fetch API initiates the request.</span></span> <span data-ttu-id="5c407-610">Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-610">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="5c407-611">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="5c407-612">Tworzenie *wwwroot* folder w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="5c407-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="5c407-613">Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="5c407-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="5c407-614">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="5c407-615">Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="5c407-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="5c407-616">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c407-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="5c407-617">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="5c407-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="5c407-618">Otwórz *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5c407-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="5c407-619">Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.</span><span class="sxs-lookup"><span data-stu-id="5c407-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="5c407-620">Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-620">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="5c407-621">Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c407-621">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="5c407-622">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-622">Get a list of to-do items</span></span>

<span data-ttu-id="5c407-623">Polecenie pobrania wysyła żądanie HTTP GET do internetowego interfejsu API, który zwraca kod JSON reprezentujący tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="5c407-623">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="5c407-624">`success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="5c407-624">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="5c407-625">Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="5c407-625">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="5c407-626">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-626">Add a to-do item</span></span>

<span data-ttu-id="5c407-627">Polecenie pobrania wysyła żądanie HTTP POST z elementem do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="5c407-627">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="5c407-628">`accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych.</span><span class="sxs-lookup"><span data-stu-id="5c407-628">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="5c407-629">Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="5c407-629">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="5c407-630">Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="5c407-630">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="5c407-631">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-631">Update a to-do item</span></span>

<span data-ttu-id="5c407-632">Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="5c407-632">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="5c407-633">`url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="5c407-633">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="5c407-634">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="5c407-634">Delete a to-do item</span></span>

<span data-ttu-id="5c407-635">Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="5c407-635">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="5c407-636">Dodawanie obsługi uwierzytelniania do internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="5c407-636">Add authentication support to a web API</span></span>

<span data-ttu-id="5c407-637">Zobacz samouczek [usługi identityserver4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .</span><span class="sxs-lookup"><span data-stu-id="5c407-637">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c407-638">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5c407-638">Additional resources</span></span>

<span data-ttu-id="5c407-639">[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="5c407-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="5c407-640">Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="5c407-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="5c407-641">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="5c407-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="5c407-642">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="5c407-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
