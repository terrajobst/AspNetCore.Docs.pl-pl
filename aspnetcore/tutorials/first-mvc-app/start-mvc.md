---
title: Rozpoczynanie pracy z platformą ASP.NET Core MVC
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Dowiedz się, jak rozpocząć pracę z platformą ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: cfce3b5792a5d0673bae5ddbba9e2d4d515a6279
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381806"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="39838-103">Rozpoczynanie pracy z platformą ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="39838-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="39838-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="39838-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

https://docs.microsoft.com/en-us/visualstudio/ide/visual-studio-ide?view=vs-2017

<span data-ttu-id="39838-105">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="39838-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="39838-106">Aplikacja zarządza bazę tytułów filmów.</span><span class="sxs-lookup"><span data-stu-id="39838-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="39838-107">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="39838-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="39838-108">Tworzenie aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="39838-108">Create a web app.</span></span>
> * <span data-ttu-id="39838-109">Dodaj i tworzenia szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="39838-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="39838-110">Praca z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="39838-110">Work with a database.</span></span>
> * <span data-ttu-id="39838-111">Dodaj wyszukiwanie i sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="39838-111">Add search and validation.</span></span>

<span data-ttu-id="39838-112">Na koniec masz aplikację, która może zarządzać i wyświetlić dane dotyczące filmu.</span><span class="sxs-lookup"><span data-stu-id="39838-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

> [!NOTE]
> <span data-ttu-id="39838-113">W tej chwili testujemy użyteczność nowo zaproponowanej struktury spisu treści dla dokumentacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39838-113">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="39838-114">Jeśli możesz poświęcić chwilę na wykonanie ćwiczenia polegającego na znalezieniu 7 różnych tematów w aktualnym lub zaproponowanym spisie treści, [kliknij tutaj i weź udział w badaniu](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="39838-114">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="39838-115">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="39838-115">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39838-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39838-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39838-117">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="39838-117">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="39838-119">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="39838-119">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="39838-120">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="39838-120">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="39838-121">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="39838-121">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="39838-122">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="39838-122">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="39838-123">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="39838-123">select **OK**</span></span>

![<span data-ttu-id="39838-124">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39838-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="39838-125">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="39838-125">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="39838-126">W polu listy rozwijanej selektora wersji wybierz **platformy ASP.NET Core 2.2**</span><span class="sxs-lookup"><span data-stu-id="39838-126">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="39838-127">Wybierz **(Model-View-Controller) aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="39838-127">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="39838-128">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="39838-128">select **OK**.</span></span>

![<span data-ttu-id="39838-129">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39838-129">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="39838-130">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="39838-130">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="39838-131">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="39838-131">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="39838-132">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="39838-132">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="39838-133">Wybierz **Ctrl-F5** do uruchomienia aplikacji w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="39838-133">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

* <span data-ttu-id="39838-134">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="39838-134">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="39838-135">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="39838-135">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="39838-136">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="39838-136">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="39838-137">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="39838-137">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="39838-138">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="39838-138">In the image above, the port number is 5000.</span></span> <span data-ttu-id="39838-139">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="39838-139">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="39838-140">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="39838-140">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="39838-141">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="39838-141">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="39838-142">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="39838-142">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="39838-143">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="39838-143">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="39838-145">Można debugować aplikację, wybierając **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="39838-145">You can debug the app by selecting the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="39838-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39838-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="39838-148">W samouczku założono familarity z programem VS Code.</span><span class="sxs-lookup"><span data-stu-id="39838-148">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="39838-149">Zobacz [wprowadzenie do programu VS Code](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="39838-149">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="39838-150">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="39838-150">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="39838-151">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="39838-151">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="39838-152">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="39838-152">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="39838-153">Zostanie wyświetlone okno dialogowe z **"MvcMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="39838-153">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="39838-154">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="39838-154">Select **Yes**</span></span>

  * <span data-ttu-id="39838-155">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w *MvcMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="39838-155">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="39838-156">`code -r MvcMovie`: Ładunki *MvcMovie.csproj* plik projektu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="39838-156">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="39838-157">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="39838-157">Launch the app</span></span>

* <span data-ttu-id="39838-158">Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.</span><span class="sxs-lookup"><span data-stu-id="39838-158">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="39838-159">Visual Studio Code rozpoczyna się rozpoczyna się [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="39838-159">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="39838-160">Przedstawia pasek adresu `localhost:port:5001` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="39838-160">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="39838-161">To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="39838-161">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="39838-162">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="39838-162">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="39838-163">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="39838-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="39838-164">Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="39838-164">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="39838-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="39838-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="39838-166">Wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="39838-166">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="39838-168">Wybierz **aplikacji programu .NET Core** > **platformy ASP.NET Core** > **aplikacji internetowej ASP.NET Core (MVC)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="39838-168">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="39838-170">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="39838-170">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="39838-171">Nadaj projektowi nazwę **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="39838-171">Name the project **MvcMovie**, and then select **Create**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="39838-172">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="39838-172">Launch the app</span></span>

<span data-ttu-id="39838-173">Wybierz **Uruchom** > **Rozpocznij bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39838-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="39838-174">Program Visual Studio for Mac rozpoczyna [Kestrel](xref:fundamentals/servers/index#kestrel) serwera spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="39838-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

* <span data-ttu-id="39838-175">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="39838-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="39838-176">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="39838-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="39838-177">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="39838-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="39838-178">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="39838-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="39838-179">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **Uruchom** menu.</span><span class="sxs-lookup"><span data-stu-id="39838-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

---  
<!-- End of VS tabs -->

* <span data-ttu-id="39838-180">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="39838-180">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="39838-181">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="39838-181">This app doesn't track personal information.</span></span> <span data-ttu-id="39838-182">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="39838-182">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="39838-184">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="39838-184">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="39838-186">W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="39838-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="39838-187">Next</span><span class="sxs-lookup"><span data-stu-id="39838-187">Next</span></span>](adding-controller.md)  
