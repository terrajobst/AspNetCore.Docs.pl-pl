---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 855d05fa2b9c1a7572212c40adbe61bb396f4bac
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819835"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="fd85f-103">Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd85f-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="fd85f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="fd85f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="fd85f-105">W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd85f-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fd85f-106">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fd85f-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fd85f-107">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fd85f-107">Create a web API project.</span></span>
> * <span data-ttu-id="fd85f-108">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="fd85f-109">Tworzy szkielet kontrolera z metodami CRUD.</span><span class="sxs-lookup"><span data-stu-id="fd85f-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="fd85f-110">Skonfiguruj Routing, ścieżki URL i wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="fd85f-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="fd85f-111">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="fd85f-111">Call the web API with Postman.</span></span>

<span data-ttu-id="fd85f-112">Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="fd85f-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="fd85f-113">Overview</span></span>

<span data-ttu-id="fd85f-114">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="fd85f-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="fd85f-115">interfejs API</span><span class="sxs-lookup"><span data-stu-id="fd85f-115">API</span></span> | <span data-ttu-id="fd85f-116">Opis</span><span class="sxs-lookup"><span data-stu-id="fd85f-116">Description</span></span> | <span data-ttu-id="fd85f-117">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="fd85f-117">Request body</span></span> | <span data-ttu-id="fd85f-118">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="fd85f-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="fd85f-119">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="fd85f-119">GET /api/TodoItems</span></span> | <span data-ttu-id="fd85f-120">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-120">Get all to-do items</span></span> | <span data-ttu-id="fd85f-121">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-121">None</span></span> | <span data-ttu-id="fd85f-122">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-122">Array of to-do items</span></span>|
|<span data-ttu-id="fd85f-123">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="fd85f-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="fd85f-124">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="fd85f-124">Get an item by ID</span></span> | <span data-ttu-id="fd85f-125">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-125">None</span></span> | <span data-ttu-id="fd85f-126">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-126">To-do item</span></span>|
|<span data-ttu-id="fd85f-127">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="fd85f-127">POST /api/TodoItems</span></span> | <span data-ttu-id="fd85f-128">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="fd85f-128">Add a new item</span></span> | <span data-ttu-id="fd85f-129">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-129">To-do item</span></span> | <span data-ttu-id="fd85f-130">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-130">To-do item</span></span> |
|<span data-ttu-id="fd85f-131">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="fd85f-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="fd85f-132">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="fd85f-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="fd85f-133">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-133">To-do item</span></span> | <span data-ttu-id="fd85f-134">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-134">None</span></span> |
|<span data-ttu-id="fd85f-135">Usuń/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="fd85f-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="fd85f-136">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="fd85f-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="fd85f-137">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-137">None</span></span> | <span data-ttu-id="fd85f-138">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-138">None</span></span>|

<span data-ttu-id="fd85f-139">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-139">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="fd85f-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fd85f-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fd85f-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd85f-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fd85f-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="fd85f-149">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="fd85f-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd85f-151">Z menu **plik** wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="fd85f-152">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="fd85f-153">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="fd85f-154">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="fd85f-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="fd85f-155">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="fd85f-156">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-156">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fd85f-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd85f-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fd85f-159">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="fd85f-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="fd85f-160">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="fd85f-161">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fd85f-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="fd85f-162">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="fd85f-163">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="fd85f-163">The preceding commands:</span></span>

  * <span data-ttu-id="fd85f-164">Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fd85f-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="fd85f-165">Dodaje pakiety NuGet, które są wymagane w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fd85f-166">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fd85f-167">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-167">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="fd85f-169">Wybierzpozycję **interfejs API** > > aplikacji.NET> Core.</span><span class="sxs-lookup"><span data-stu-id="fd85f-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="fd85f-171">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję docelowa **platforma** \* *.NET Core 3,0*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="fd85f-172">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="fd85f-174">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="fd85f-174">Test the API</span></span>

<span data-ttu-id="fd85f-175">Szablon projektu umożliwia utworzenie `WeatherForecast` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="fd85f-176">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="fd85f-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fd85f-178">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fd85f-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="fd85f-179">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="fd85f-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="fd85f-180">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="fd85f-181">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fd85f-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd85f-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fd85f-183">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fd85f-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="fd85f-184">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="fd85f-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fd85f-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fd85f-186">Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fd85f-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="fd85f-187">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="fd85f-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="fd85f-188">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="fd85f-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="fd85f-189">Dołącz `/WeatherForecast` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="fd85f-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="fd85f-190">Zwracany jest kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="fd85f-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="fd85f-191">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="fd85f-191">Add a model class</span></span>

<span data-ttu-id="fd85f-192">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="fd85f-193">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="fd85f-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd85f-195">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="fd85f-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="fd85f-196">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="fd85f-197">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="fd85f-198">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="fd85f-199">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="fd85f-200">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fd85f-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd85f-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fd85f-202">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="fd85f-203">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fd85f-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fd85f-205">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="fd85f-205">Right-click the project.</span></span> <span data-ttu-id="fd85f-206">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="fd85f-207">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-207">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="fd85f-209">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="fd85f-210">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="fd85f-211">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="fd85f-212">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="fd85f-213">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="fd85f-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="fd85f-214">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="fd85f-214">Add a database context</span></span>

<span data-ttu-id="fd85f-215">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fd85f-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="fd85f-216">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="fd85f-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="fd85f-218">Dodaj Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="fd85f-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="fd85f-219">W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="fd85f-220">Zaznacz pole wyboru **Uwzględnij wersję wstępną** .</span><span class="sxs-lookup"><span data-stu-id="fd85f-220">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="fd85f-221">Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="fd85f-221">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="fd85f-222">W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer v 3.0.0 — wersja** zapoznawcza.</span><span class="sxs-lookup"><span data-stu-id="fd85f-222">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="fd85f-223">Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-223">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="fd85f-224">Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-224">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="fd85f-226">Dodawanie kontekstu bazy danych TodoContext</span><span class="sxs-lookup"><span data-stu-id="fd85f-226">Add the TodoContext database context</span></span>

* <span data-ttu-id="fd85f-227">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-227">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="fd85f-228">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-228">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="fd85f-229">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="fd85f-230">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-230">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="fd85f-231">Wprowadź następujący kod:</span><span class="sxs-lookup"><span data-stu-id="fd85f-231">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="fd85f-232">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="fd85f-232">Register the database context</span></span>

<span data-ttu-id="fd85f-233">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="fd85f-233">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="fd85f-234">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="fd85f-234">The container provides the service to controllers.</span></span>

<span data-ttu-id="fd85f-235">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="fd85f-235">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="fd85f-236">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="fd85f-236">The preceding code:</span></span>

* <span data-ttu-id="fd85f-237">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-237">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="fd85f-238">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-238">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="fd85f-239">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="fd85f-239">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="fd85f-240">Tworzenie szkieletu kontrolera</span><span class="sxs-lookup"><span data-stu-id="fd85f-240">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-241">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-241">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd85f-242">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-242">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="fd85f-243">Wybierz pozycję **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-243">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="fd85f-244">Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-244">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="fd85f-245">Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:</span><span class="sxs-lookup"><span data-stu-id="fd85f-245">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="fd85f-246">Wybierz pozycję **TodoItem (TodoAPI. models)** w **klasie model**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-246">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="fd85f-247">W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoAPI. models)** .</span><span class="sxs-lookup"><span data-stu-id="fd85f-247">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="fd85f-248">Wybierz pozycję **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="fd85f-248">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="fd85f-249">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="fd85f-250">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fd85f-250">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="fd85f-251">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="fd85f-251">The preceding commands:</span></span>

* <span data-ttu-id="fd85f-252">Dodaj pakiety NuGet wymagane do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="fd85f-252">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="fd85f-253">Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="fd85f-253">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="fd85f-254">Szkielety `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-254">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="fd85f-255">Wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="fd85f-255">The generated code:</span></span>

* <span data-ttu-id="fd85f-256">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="fd85f-256">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="fd85f-257">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="fd85f-257">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="fd85f-258">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-258">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="fd85f-259">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="fd85f-259">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="fd85f-260">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fd85f-260">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="fd85f-261">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="fd85f-261">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="fd85f-262">Badanie metody PostTodoItem Create</span><span class="sxs-lookup"><span data-stu-id="fd85f-262">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="fd85f-263">Zastąp instrukcję return w, `PostTodoItem` aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="fd85f-263">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="fd85f-264">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-264">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="fd85f-265">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd85f-265">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="fd85f-266"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Metody:</span><span class="sxs-lookup"><span data-stu-id="fd85f-266">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="fd85f-267">W razie powodzenia zwraca kod stanu HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="fd85f-267">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="fd85f-268">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="fd85f-268">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="fd85f-269">Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fd85f-269">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="fd85f-270">Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania. [](https://developer.mozilla.org/docs/Glossary/URI) `Location`</span><span class="sxs-lookup"><span data-stu-id="fd85f-270">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="fd85f-271">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="fd85f-271">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="fd85f-272">Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka.</span><span class="sxs-lookup"><span data-stu-id="fd85f-272">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="fd85f-273">Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-273">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="fd85f-274">Zainstaluj program Poster</span><span class="sxs-lookup"><span data-stu-id="fd85f-274">Install Postman</span></span>

<span data-ttu-id="fd85f-275">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-275">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="fd85f-276">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="fd85f-276">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="fd85f-277">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="fd85f-277">Start the web app.</span></span>
* <span data-ttu-id="fd85f-278">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="fd85f-278">Start Postman.</span></span>
* <span data-ttu-id="fd85f-279">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="fd85f-279">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="fd85f-280">Z **Plik > Ustawienia** (\**ogólne* karty), wyłącz **weryfikacji certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-280">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="fd85f-281">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fd85f-281">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="fd85f-282">Test PostTodoItem za pomocą programu Poster</span><span class="sxs-lookup"><span data-stu-id="fd85f-282">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="fd85f-283">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="fd85f-283">Create a new request.</span></span>
* <span data-ttu-id="fd85f-284">Ustaw metodę HTTP na `POST`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-284">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="fd85f-285">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="fd85f-285">Select the **Body** tab.</span></span>
* <span data-ttu-id="fd85f-286">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="fd85f-286">Select the **raw** radio button.</span></span>
* <span data-ttu-id="fd85f-287">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="fd85f-287">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="fd85f-288">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="fd85f-288">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="fd85f-289">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-289">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="fd85f-291">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="fd85f-291">Test the location header URI</span></span>

* <span data-ttu-id="fd85f-292">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="fd85f-292">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="fd85f-293">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="fd85f-293">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="fd85f-295">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="fd85f-295">Set the method to GET.</span></span>
* <span data-ttu-id="fd85f-296">Wklej identyfikator URI (na przykład `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="fd85f-296">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="fd85f-297">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-297">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="fd85f-298">Badanie metod GET</span><span class="sxs-lookup"><span data-stu-id="fd85f-298">Examine the GET methods</span></span>

<span data-ttu-id="fd85f-299">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="fd85f-299">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="fd85f-300">Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-300">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="fd85f-301">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fd85f-301">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="fd85f-302">Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="fd85f-302">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="fd85f-303">Test get przy użyciu programu Poster</span><span class="sxs-lookup"><span data-stu-id="fd85f-303">Test Get with Postman</span></span>

* <span data-ttu-id="fd85f-304">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="fd85f-304">Create a new request.</span></span>
* <span data-ttu-id="fd85f-305">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-305">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="fd85f-306">Ustaw adres URL żądania `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-306">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="fd85f-307">Na przykład `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-307">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="fd85f-308">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="fd85f-308">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="fd85f-309">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-309">Select **Send**.</span></span>

<span data-ttu-id="fd85f-310">Ta aplikacja używa bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="fd85f-310">This app uses an in-memory database.</span></span> <span data-ttu-id="fd85f-311">Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-311">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="fd85f-312">Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-312">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="fd85f-313">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="fd85f-313">Routing and URL paths</span></span>

<span data-ttu-id="fd85f-314">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="fd85f-314">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="fd85f-315">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fd85f-315">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="fd85f-316">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="fd85f-316">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="fd85f-317">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="fd85f-317">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="fd85f-318">Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="fd85f-318">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="fd85f-319">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="fd85f-319">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="fd85f-320">Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="fd85f-320">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="fd85f-321">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-321">This sample doesn't use a template.</span></span> <span data-ttu-id="fd85f-322">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="fd85f-322">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="fd85f-323">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="fd85f-323">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="fd85f-324">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="fd85f-324">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="fd85f-325">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="fd85f-325">Return values</span></span>

<span data-ttu-id="fd85f-326">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="fd85f-326">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="fd85f-327">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fd85f-327">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="fd85f-328">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="fd85f-328">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="fd85f-329">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="fd85f-329">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="fd85f-330">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd85f-330">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="fd85f-331">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="fd85f-331">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="fd85f-332">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-332">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="fd85f-333">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="fd85f-333">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="fd85f-334">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="fd85f-334">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="fd85f-335">Metoda PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="fd85f-335">The PutTodoItem method</span></span>

<span data-ttu-id="fd85f-336">`PutTodoItem` Badanie metody:</span><span class="sxs-lookup"><span data-stu-id="fd85f-336">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="fd85f-337">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="fd85f-337">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="fd85f-338">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="fd85f-338">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="fd85f-339">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="fd85f-339">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="fd85f-340">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="fd85f-340">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="fd85f-341">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="fd85f-341">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="fd85f-342">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="fd85f-342">Test the PutTodoItem method</span></span>

<span data-ttu-id="fd85f-343">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="fd85f-343">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="fd85f-344">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-344">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="fd85f-345">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="fd85f-345">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="fd85f-346">Zaktualizuj element do wykonania o IDENTYFIKATORze 1 i ustaw jego nazwę na "Źródło danych":</span><span class="sxs-lookup"><span data-stu-id="fd85f-346">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="fd85f-347">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="fd85f-347">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="fd85f-349">Metoda DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="fd85f-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="fd85f-350">`DeleteTodoItem` Badanie metody:</span><span class="sxs-lookup"><span data-stu-id="fd85f-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="fd85f-351">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="fd85f-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="fd85f-352">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="fd85f-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="fd85f-353">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="fd85f-353">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="fd85f-354">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-354">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="fd85f-355">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="fd85f-355">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="fd85f-356">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="fd85f-356">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="fd85f-357">Wywoływanie interfejsu API z platformy jQuery</span><span class="sxs-lookup"><span data-stu-id="fd85f-357">Call the API from jQuery</span></span>

<span data-ttu-id="fd85f-358">Zobacz [samouczek: Wywołaj ASP.NET Core Web API z jQuery](xref:tutorials/web-api-jquery).</span><span class="sxs-lookup"><span data-stu-id="fd85f-358">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fd85f-359">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fd85f-359">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fd85f-360">Utwórz projekt interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fd85f-360">Create a web API project.</span></span>
> * <span data-ttu-id="fd85f-361">Dodaj klasę modelu i kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-361">Add a model class and a database context.</span></span>
> * <span data-ttu-id="fd85f-362">Dodawanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fd85f-362">Add a controller.</span></span>
> * <span data-ttu-id="fd85f-363">Dodaj metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="fd85f-363">Add CRUD methods.</span></span>
> * <span data-ttu-id="fd85f-364">Konfigurowanie routingu i ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="fd85f-364">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="fd85f-365">Określ wartości zwracane.</span><span class="sxs-lookup"><span data-stu-id="fd85f-365">Specify return values.</span></span>
> * <span data-ttu-id="fd85f-366">Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.</span><span class="sxs-lookup"><span data-stu-id="fd85f-366">Call the web API with Postman.</span></span>
> * <span data-ttu-id="fd85f-367">Wywołaj interfejs API sieci Web za pomocą platformy jQuery.</span><span class="sxs-lookup"><span data-stu-id="fd85f-367">Call the web API with jQuery.</span></span>

<span data-ttu-id="fd85f-368">Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-368">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="fd85f-369">Omówienie</span><span class="sxs-lookup"><span data-stu-id="fd85f-369">Overview</span></span>

<span data-ttu-id="fd85f-370">Ten samouczek tworzy następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="fd85f-370">This tutorial creates the following API:</span></span>

|<span data-ttu-id="fd85f-371">interfejs API</span><span class="sxs-lookup"><span data-stu-id="fd85f-371">API</span></span> | <span data-ttu-id="fd85f-372">Opis</span><span class="sxs-lookup"><span data-stu-id="fd85f-372">Description</span></span> | <span data-ttu-id="fd85f-373">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="fd85f-373">Request body</span></span> | <span data-ttu-id="fd85f-374">Treść odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="fd85f-374">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="fd85f-375">Pobierz/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="fd85f-375">GET /api/TodoItems</span></span> | <span data-ttu-id="fd85f-376">Pobierz wszystkie elementy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-376">Get all to-do items</span></span> | <span data-ttu-id="fd85f-377">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-377">None</span></span> | <span data-ttu-id="fd85f-378">Tablica elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-378">Array of to-do items</span></span>|
|<span data-ttu-id="fd85f-379">Pobierz/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="fd85f-379">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="fd85f-380">Umieść element według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="fd85f-380">Get an item by ID</span></span> | <span data-ttu-id="fd85f-381">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-381">None</span></span> | <span data-ttu-id="fd85f-382">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-382">To-do item</span></span>|
|<span data-ttu-id="fd85f-383">Opublikuj/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="fd85f-383">POST /api/TodoItems</span></span> | <span data-ttu-id="fd85f-384">Dodaj nowy element</span><span class="sxs-lookup"><span data-stu-id="fd85f-384">Add a new item</span></span> | <span data-ttu-id="fd85f-385">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-385">To-do item</span></span> | <span data-ttu-id="fd85f-386">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-386">To-do item</span></span> |
|<span data-ttu-id="fd85f-387">Umieść/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="fd85f-387">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="fd85f-388">Zaktualizuj istniejący element &nbsp;</span><span class="sxs-lookup"><span data-stu-id="fd85f-388">Update an existing item &nbsp;</span></span> | <span data-ttu-id="fd85f-389">Zadania do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-389">To-do item</span></span> | <span data-ttu-id="fd85f-390">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-390">None</span></span> |
|<span data-ttu-id="fd85f-391">Usuń/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="fd85f-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="fd85f-392">Usuwanie elementu &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="fd85f-392">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="fd85f-393">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-393">None</span></span> | <span data-ttu-id="fd85f-394">Brak</span><span class="sxs-lookup"><span data-stu-id="fd85f-394">None</span></span>|

<span data-ttu-id="fd85f-395">Na poniższym diagramie przedstawiono projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-395">The following diagram shows the design of the app.</span></span>

![Klient jest reprezentowany przez pole po lewej stronie.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="fd85f-401">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fd85f-401">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-402">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-402">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fd85f-403">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd85f-403">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fd85f-404">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-404">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="fd85f-405">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="fd85f-405">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd85f-407">Z menu **plik** wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-407">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="fd85f-408">Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-408">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="fd85f-409">Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-409">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="fd85f-410">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** .</span><span class="sxs-lookup"><span data-stu-id="fd85f-410">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="fd85f-411">Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-411">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="fd85f-412">**Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-412">**Don't** select **Enable Docker Support**.</span></span>

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fd85f-414">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd85f-414">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fd85f-415">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="fd85f-415">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="fd85f-416">Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-416">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="fd85f-417">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fd85f-417">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="fd85f-418">Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-418">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="fd85f-419">Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-419">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fd85f-420">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-420">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fd85f-421">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-421">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="fd85f-423">Wybierzpozycję **interfejs API** > > aplikacji.NET> Core.</span><span class="sxs-lookup"><span data-stu-id="fd85f-423">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="fd85f-425">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-425">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="fd85f-426">Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-426">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="fd85f-428">Testowanie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="fd85f-428">Test the API</span></span>

<span data-ttu-id="fd85f-429">Szablon projektu umożliwia utworzenie `values` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-429">The project template creates a `values` API.</span></span> <span data-ttu-id="fd85f-430">Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="fd85f-430">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-431">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-431">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fd85f-432">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fd85f-432">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="fd85f-433">Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="fd85f-433">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="fd85f-434">Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-434">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="fd85f-435">W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-435">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fd85f-436">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd85f-436">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fd85f-437">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fd85f-437">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="fd85f-438">W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="fd85f-438">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fd85f-439">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fd85f-440">Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fd85f-440">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="fd85f-441">Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="fd85f-441">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="fd85f-442">Jest zwracany błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="fd85f-442">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="fd85f-443">Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="fd85f-443">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="fd85f-444">Zwracane są następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="fd85f-444">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="fd85f-445">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="fd85f-445">Add a model class</span></span>

<span data-ttu-id="fd85f-446">A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-446">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="fd85f-447">Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="fd85f-447">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-448">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-448">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd85f-449">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="fd85f-449">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="fd85f-450">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-450">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="fd85f-451">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-451">Name the folder *Models*.</span></span>

* <span data-ttu-id="fd85f-452">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-452">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="fd85f-453">Nazwa klasy *TodoItem* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-453">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="fd85f-454">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-454">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fd85f-455">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd85f-455">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fd85f-456">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-456">Add a folder named *Models*.</span></span>

* <span data-ttu-id="fd85f-457">Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-457">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fd85f-458">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-458">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fd85f-459">Kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="fd85f-459">Right-click the project.</span></span> <span data-ttu-id="fd85f-460">Wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-460">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="fd85f-461">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-461">Name the folder *Models*.</span></span>

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="fd85f-463">Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-463">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="fd85f-464">Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-464">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="fd85f-465">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-465">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="fd85f-466">`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-466">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="fd85f-467">Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="fd85f-467">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="fd85f-468">Dodawanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="fd85f-468">Add a database context</span></span>

<span data-ttu-id="fd85f-469">*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fd85f-469">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="fd85f-470">Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="fd85f-470">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-471">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-471">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd85f-472">Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="fd85f-473">Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-473">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="fd85f-474">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-474">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="fd85f-475">Dodaj `TodoContext` klasy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-475">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="fd85f-476">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-476">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="fd85f-477">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="fd85f-477">Register the database context</span></span>

<span data-ttu-id="fd85f-478">W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="fd85f-478">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="fd85f-479">Kontener zawiera usługę do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="fd85f-479">The container provides the service to controllers.</span></span>

<span data-ttu-id="fd85f-480">Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="fd85f-480">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="fd85f-481">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="fd85f-481">The preceding code:</span></span>

* <span data-ttu-id="fd85f-482">Usuwa nieużywane `using` deklaracji.</span><span class="sxs-lookup"><span data-stu-id="fd85f-482">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="fd85f-483">Dodaje kontener DI kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-483">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="fd85f-484">Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="fd85f-484">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="fd85f-485">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="fd85f-485">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-486">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-486">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd85f-487">Kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-487">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="fd85f-488">Wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-488">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="fd85f-489">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-489">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="fd85f-490">Nazwa klasy *TodoController*i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-490">Name the class *TodoController*, and select **Add**.</span></span>

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="fd85f-492">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-492">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="fd85f-493">W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-493">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="fd85f-494">Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-494">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="fd85f-495">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="fd85f-495">The preceding code:</span></span>

* <span data-ttu-id="fd85f-496">Definiuje klasę kontrolera interfejsu API bez metody.</span><span class="sxs-lookup"><span data-stu-id="fd85f-496">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="fd85f-497">Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="fd85f-497">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="fd85f-498">Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-498">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="fd85f-499">Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="fd85f-499">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="fd85f-500">Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fd85f-500">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="fd85f-501">Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="fd85f-501">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="fd85f-502">Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta.</span><span class="sxs-lookup"><span data-stu-id="fd85f-502">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="fd85f-503">Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd85f-503">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="fd85f-504">Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-504">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="fd85f-505">Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.</span><span class="sxs-lookup"><span data-stu-id="fd85f-505">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="fd85f-506">Dodaj metody Get</span><span class="sxs-lookup"><span data-stu-id="fd85f-506">Add Get methods</span></span>

<span data-ttu-id="fd85f-507">Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:</span><span class="sxs-lookup"><span data-stu-id="fd85f-507">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="fd85f-508">Te metody zaimplementować dwa GET punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="fd85f-508">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="fd85f-509">Zatrzymaj aplikację, jeśli jest nadal uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="fd85f-509">Stop the app if it's still running.</span></span> <span data-ttu-id="fd85f-510">Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.</span><span class="sxs-lookup"><span data-stu-id="fd85f-510">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="fd85f-511">Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="fd85f-511">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="fd85f-512">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fd85f-512">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="fd85f-513">Następującą odpowiedź HTTP jest tworzony przez wywołanie metody `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="fd85f-513">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="fd85f-514">Ścieżki routingu i adres URL</span><span class="sxs-lookup"><span data-stu-id="fd85f-514">Routing and URL paths</span></span>

<span data-ttu-id="fd85f-515">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="fd85f-515">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="fd85f-516">Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fd85f-516">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="fd85f-517">Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="fd85f-517">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="fd85f-518">Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller".</span><span class="sxs-lookup"><span data-stu-id="fd85f-518">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="fd85f-519">W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera, więc nazwa kontrolera jest "todo".</span><span class="sxs-lookup"><span data-stu-id="fd85f-519">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="fd85f-520">Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="fd85f-520">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="fd85f-521">Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="fd85f-521">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="fd85f-522">W tym przykładzie nie używa szablonu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-522">This sample doesn't use a template.</span></span> <span data-ttu-id="fd85f-523">Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="fd85f-523">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="fd85f-524">W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="fd85f-524">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="fd85f-525">Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.</span><span class="sxs-lookup"><span data-stu-id="fd85f-525">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="fd85f-526">Zwracane wartości</span><span class="sxs-lookup"><span data-stu-id="fd85f-526">Return values</span></span>

<span data-ttu-id="fd85f-527">Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="fd85f-527">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="fd85f-528">Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fd85f-528">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="fd85f-529">Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="fd85f-529">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="fd85f-530">Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.</span><span class="sxs-lookup"><span data-stu-id="fd85f-530">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="fd85f-531">`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd85f-531">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="fd85f-532">Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:</span><span class="sxs-lookup"><span data-stu-id="fd85f-532">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="fd85f-533">Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-533">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="fd85f-534">W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON.</span><span class="sxs-lookup"><span data-stu-id="fd85f-534">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="fd85f-535">Zwracanie `item` skutkuje odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="fd85f-535">Returning `item` results in an HTTP 200 response.</span></span>


## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="fd85f-536">Metoda GetTodoItems testu</span><span class="sxs-lookup"><span data-stu-id="fd85f-536">Test the GetTodoItems method</span></span>

<span data-ttu-id="fd85f-537">Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-537">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="fd85f-538">Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="fd85f-538">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="fd85f-539">Uruchamiają aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="fd85f-539">Start the web app.</span></span>
* <span data-ttu-id="fd85f-540">Uruchom narzędzie Postman.</span><span class="sxs-lookup"><span data-stu-id="fd85f-540">Start Postman.</span></span>
* <span data-ttu-id="fd85f-541">Wyłącz **weryfikacji certyfikatu SSL**</span><span class="sxs-lookup"><span data-stu-id="fd85f-541">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fd85f-542">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd85f-542">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fd85f-543">W obszarze **Ustawienia** **pliku** > (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-543">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="fd85f-544">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="fd85f-544">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="fd85f-545">Z > poziomu**preferencji** programu Poster (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-545">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="fd85f-546">Alternatywnie wybierz klucz i wybierz pozycję **Ustawienia**, a następnie wyłącz weryfikację certyfikatu SSL.</span><span class="sxs-lookup"><span data-stu-id="fd85f-546">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="fd85f-547">Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fd85f-547">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="fd85f-548">Utwórz nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="fd85f-548">Create a new request.</span></span>
  * <span data-ttu-id="fd85f-549">Ustawia metodę HTTP **UZYSKAĆ**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-549">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="fd85f-550">Ustaw adres URL żądania `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-550">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="fd85f-551">Na przykład `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-551">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="fd85f-552">Ustaw **widoku dwa okienka** w narzędziu Postman.</span><span class="sxs-lookup"><span data-stu-id="fd85f-552">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="fd85f-553">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-553">Select **Send**.</span></span>

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="fd85f-555">Dodawanie metody Create</span><span class="sxs-lookup"><span data-stu-id="fd85f-555">Add a Create method</span></span>

<span data-ttu-id="fd85f-556">Dodaj następującą `PostTodoItem` metodę w obszarze *controllers/TodoController. cs*:</span><span class="sxs-lookup"><span data-stu-id="fd85f-556">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="fd85f-557">Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-557">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="fd85f-558">Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd85f-558">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="fd85f-559">`CreatedAtAction` Metody:</span><span class="sxs-lookup"><span data-stu-id="fd85f-559">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="fd85f-560">Zwraca kod stanu HTTP 201, jeśli powodzenie.</span><span class="sxs-lookup"><span data-stu-id="fd85f-560">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="fd85f-561">Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="fd85f-561">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="fd85f-562">`Location` Dodaje nagłówek do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fd85f-562">Adds a `Location` header to the response.</span></span> <span data-ttu-id="fd85f-563">`Location` Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="fd85f-563">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="fd85f-564">Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="fd85f-564">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="fd85f-565">Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka.</span><span class="sxs-lookup"><span data-stu-id="fd85f-565">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="fd85f-566">Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-566">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="fd85f-567">Metoda PostTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="fd85f-567">Test the PostTodoItem method</span></span>

* <span data-ttu-id="fd85f-568">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="fd85f-568">Build the project.</span></span>
* <span data-ttu-id="fd85f-569">W narzędziu Postman, Ustawia metodę HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-569">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="fd85f-570">Wybierz **treści** kartę.</span><span class="sxs-lookup"><span data-stu-id="fd85f-570">Select the **Body** tab.</span></span>
* <span data-ttu-id="fd85f-571">Wybierz **pierwotne** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="fd85f-571">Select the **raw** radio button.</span></span>
* <span data-ttu-id="fd85f-572">Ustaw typ **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="fd85f-572">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="fd85f-573">W treści żądania wprowadź JSON element do wykonania:</span><span class="sxs-lookup"><span data-stu-id="fd85f-573">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="fd85f-574">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-574">Select **Send**.</span></span>

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  <span data-ttu-id="fd85f-576">Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu `PostTodoItem` metody.</span><span class="sxs-lookup"><span data-stu-id="fd85f-576">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="fd85f-577">Testowanie nagłówek location identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="fd85f-577">Test the location header URI</span></span>

* <span data-ttu-id="fd85f-578">Wybierz **nagłówki** karcie **odpowiedzi** okienka.</span><span class="sxs-lookup"><span data-stu-id="fd85f-578">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="fd85f-579">Kopiuj **lokalizacji** wartość nagłówka:</span><span class="sxs-lookup"><span data-stu-id="fd85f-579">Copy the **Location** header value:</span></span>

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="fd85f-581">Ustaw metodę GET.</span><span class="sxs-lookup"><span data-stu-id="fd85f-581">Set the method to GET.</span></span>
* <span data-ttu-id="fd85f-582">Wklej identyfikator URI (na przykład `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="fd85f-582">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="fd85f-583">Wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="fd85f-583">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="fd85f-584">Dodaj metodę PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="fd85f-584">Add a PutTodoItem method</span></span>

<span data-ttu-id="fd85f-585">Dodaj następujący kod `PutTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="fd85f-585">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="fd85f-586">`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="fd85f-586">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="fd85f-587">Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="fd85f-587">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="fd85f-588">Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="fd85f-588">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="fd85f-589">Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="fd85f-589">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="fd85f-590">Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.</span><span class="sxs-lookup"><span data-stu-id="fd85f-590">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="fd85f-591">Metoda PutTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="fd85f-591">Test the PutTodoItem method</span></span>

<span data-ttu-id="fd85f-592">Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="fd85f-592">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="fd85f-593">Przed wykonaniem wywołania PUT musi istnieć element w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-593">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="fd85f-594">Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.</span><span class="sxs-lookup"><span data-stu-id="fd85f-594">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="fd85f-595">Zaktualizuj element zadania do wykonania, który ma identyfikator = 1 i ustaw jego nazwę na "feed ryb":</span><span class="sxs-lookup"><span data-stu-id="fd85f-595">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="fd85f-596">Na poniższej ilustracji przedstawiono aktualizacji Postman:</span><span class="sxs-lookup"><span data-stu-id="fd85f-596">The following image shows the Postman update:</span></span>

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="fd85f-598">Dodaj metodę DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="fd85f-598">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="fd85f-599">Dodaj następujący kod `DeleteTodoItem` metody:</span><span class="sxs-lookup"><span data-stu-id="fd85f-599">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="fd85f-600">`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="fd85f-600">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="fd85f-601">Metoda DeleteTodoItem testu</span><span class="sxs-lookup"><span data-stu-id="fd85f-601">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="fd85f-602">Użyj narzędzia Postman, aby usunąć zadanie do wykonania:</span><span class="sxs-lookup"><span data-stu-id="fd85f-602">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="fd85f-603">Ustawia metodę `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-603">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="fd85f-604">Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="fd85f-604">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="fd85f-605">Wybierz **wysyłania**</span><span class="sxs-lookup"><span data-stu-id="fd85f-605">Select **Send**</span></span>

<span data-ttu-id="fd85f-606">Przykładowa aplikacja umożliwia usunięcie wszystkich elementów.</span><span class="sxs-lookup"><span data-stu-id="fd85f-606">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="fd85f-607">Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-607">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="fd85f-608">Wywoływanie interfejsu API przy użyciu jQuery</span><span class="sxs-lookup"><span data-stu-id="fd85f-608">Call the API with jQuery</span></span>

<span data-ttu-id="fd85f-609">W tej sekcji strony HTML jest dodawany, który używa technologii jQuery do wywołania sieci web interfejsu api.</span><span class="sxs-lookup"><span data-stu-id="fd85f-609">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="fd85f-610">jQuery inicjuje żądanie i aktualizowanie strony ze szczegółami z odpowiedzi interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-610">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="fd85f-611">Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="fd85f-612">Tworzenie *wwwroot* folder w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="fd85f-613">Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="fd85f-614">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="fd85f-615">Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="fd85f-616">Zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fd85f-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="fd85f-617">Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:</span><span class="sxs-lookup"><span data-stu-id="fd85f-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="fd85f-618">Otwórz *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fd85f-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="fd85f-619">Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.</span><span class="sxs-lookup"><span data-stu-id="fd85f-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="fd85f-620">Istnieje kilka sposobów uzyskania biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="fd85f-620">There are several ways to get jQuery.</span></span> <span data-ttu-id="fd85f-621">W poprzednim fragmencie kodu biblioteki jest ładowany z usługi CDN.</span><span class="sxs-lookup"><span data-stu-id="fd85f-621">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="fd85f-622">Ten przykład wywołuje wszystkie metody CRUD interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-622">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="fd85f-623">Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fd85f-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="fd85f-624">Pobierz listę elementów do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-624">Get a list of to-do items</span></span>

<span data-ttu-id="fd85f-625">JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `GET` żądanie do interfejsu API, która zwraca wartość JSON reprezentująca tablicę elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="fd85f-625">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="fd85f-626">`success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="fd85f-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="fd85f-627">Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="fd85f-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="fd85f-628">Dodaj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-628">Add a to-do item</span></span>

<span data-ttu-id="fd85f-629">[Ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `POST` żądania z elementem zadań do wykonania w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="fd85f-629">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="fd85f-630">`accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych.</span><span class="sxs-lookup"><span data-stu-id="fd85f-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="fd85f-631">Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="fd85f-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="fd85f-632">Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="fd85f-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="fd85f-633">Zaktualizuj element do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-633">Update a to-do item</span></span>

<span data-ttu-id="fd85f-634">Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego.</span><span class="sxs-lookup"><span data-stu-id="fd85f-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="fd85f-635">`url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.</span><span class="sxs-lookup"><span data-stu-id="fd85f-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="fd85f-636">Usuń element do wykonania</span><span class="sxs-lookup"><span data-stu-id="fd85f-636">Delete a to-do item</span></span>

<span data-ttu-id="fd85f-637">Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="fd85f-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fd85f-638">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fd85f-638">Additional resources</span></span>

<span data-ttu-id="fd85f-639">[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="fd85f-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="fd85f-640">Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="fd85f-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="fd85f-641">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="fd85f-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="fd85f-642">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="fd85f-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
