---
title: Rozpoczynanie pracy z platformą ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformą ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1bc5da7dcab5a5070260ca464a82b650cc4838bf
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978461"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="0958b-103">Rozpoczynanie pracy z platformą ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="0958b-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="0958b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0958b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="0958b-105">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="0958b-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="0958b-106">Aplikacja zarządza bazę tytułów filmów.</span><span class="sxs-lookup"><span data-stu-id="0958b-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="0958b-107">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="0958b-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0958b-108">Tworzenie aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="0958b-108">Create a web app.</span></span>
> * <span data-ttu-id="0958b-109">Dodaj i tworzenia szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="0958b-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="0958b-110">Praca z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="0958b-110">Work with a database.</span></span>
> * <span data-ttu-id="0958b-111">Dodaj wyszukiwanie i sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="0958b-111">Add search and validation.</span></span>

<span data-ttu-id="0958b-112">Na koniec masz aplikację, która może zarządzać i wyświetlić dane dotyczące filmu.</span><span class="sxs-lookup"><span data-stu-id="0958b-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="0958b-113">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="0958b-113">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0958b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0958b-114">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0958b-115">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="0958b-115">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="0958b-117">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="0958b-117">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="0958b-118">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="0958b-118">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="0958b-119">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="0958b-119">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="0958b-120">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="0958b-120">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="0958b-121">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="0958b-121">select **OK**</span></span>

![<span data-ttu-id="0958b-122">Okno dialogowe nowego projektu, .NET Core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0958b-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="0958b-123">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="0958b-123">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="0958b-124">W polu listy rozwijanej selektora wersji wybierz **platformy ASP.NET Core 2.2**</span><span class="sxs-lookup"><span data-stu-id="0958b-124">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="0958b-125">Wybierz **(Model-View-Controller) aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="0958b-125">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="0958b-126">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="0958b-126">select **OK**.</span></span>

![<span data-ttu-id="0958b-127">Okno dialogowe nowego projektu, .NET Core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0958b-127">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="0958b-128">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="0958b-128">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="0958b-129">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="0958b-129">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="0958b-130">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="0958b-130">This is a basic starter project, and it's a good place to start.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0958b-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0958b-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0958b-132">W samouczku założono familarity z programem VS Code.</span><span class="sxs-lookup"><span data-stu-id="0958b-132">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0958b-133">Zobacz [wprowadzenie do programu VS Code](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="0958b-133">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="0958b-134">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0958b-134">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0958b-135">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="0958b-135">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0958b-136">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0958b-136">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="0958b-137">Zostanie wyświetlone okno dialogowe z **"MvcMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="0958b-137">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="0958b-138">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="0958b-138">Select **Yes**</span></span>

  * <span data-ttu-id="0958b-139">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w *MvcMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="0958b-139">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="0958b-140">`code -r MvcMovie`: Ładunki *MvcMovie.csproj* plik projektu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0958b-140">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0958b-141">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0958b-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0958b-142">Wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="0958b-142">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="0958b-144">Wybierz **aplikacji programu .NET Core** > **platformy ASP.NET Core** > **aplikacji internetowej ASP.NET Core (MVC)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="0958b-144">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="0958b-146">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z \**platformy .NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="0958b-146">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="0958b-147">Nadaj projektowi nazwę **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="0958b-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---  
<!-- End of VS tabs -->

### <a name="run-the-app"></a><span data-ttu-id="0958b-148">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0958b-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0958b-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0958b-149">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0958b-150">Wybierz **Ctrl-F5** do uruchomienia aplikacji w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="0958b-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="0958b-151">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0958b-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0958b-152">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0958b-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0958b-153">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="0958b-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0958b-154">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="0958b-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="0958b-155">Uruchamianie aplikacji za pomocą klawiszy Ctrl + F5 (bez debugowania w trybie) umożliwia wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="0958b-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0958b-156">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="0958b-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="0958b-157">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="0958b-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="0958b-159">Można debugować aplikację, wybierając **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="0958b-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0958b-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0958b-161">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="0958b-162">Naciśnij klawisze Ctrl + F5, aby uruchomić bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="0958b-162">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="0958b-163">Visual Studio Code rozpoczyna się rozpoczyna się [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0958b-163">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="0958b-164">Przedstawia pasek adresu `localhost:port:5001` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0958b-164">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="0958b-165">To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="0958b-165">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0958b-166">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="0958b-166">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="0958b-167">Uruchamianie aplikacji za pomocą klawiszy Ctrl + F5 (bez debugowania w trybie) umożliwia wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="0958b-167">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0958b-168">Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="0958b-168">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0958b-169">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0958b-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0958b-170">Wybierz **Uruchom** > **Rozpocznij bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0958b-170">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="0958b-171">Program Visual Studio for Mac rozpoczyna [Kestrel](xref:fundamentals/servers/index#kestrel) serwera spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="0958b-171">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0958b-172">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0958b-172">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0958b-173">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="0958b-173">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0958b-174">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="0958b-174">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0958b-175">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="0958b-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0958b-176">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **Uruchom** menu.</span><span class="sxs-lookup"><span data-stu-id="0958b-176">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

------

* <span data-ttu-id="0958b-177">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="0958b-177">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0958b-178">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="0958b-178">This app doesn't track personal information.</span></span> <span data-ttu-id="0958b-179">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0958b-179">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="0958b-181">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="0958b-181">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="0958b-183">W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="0958b-183">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0958b-184">Next</span><span class="sxs-lookup"><span data-stu-id="0958b-184">Next</span></span>](adding-controller.md)  
