---
title: Wprowadzenie do ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 0c8c59a5c59c8a70985dc8463c80f9569a00621f
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2019
ms.locfileid: "73761238"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="cadb5-103">Wprowadzenie do ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="cadb5-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="cadb5-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cadb5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="cadb5-105">Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cadb5-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="cadb5-106">Aplikacja zarządza bazą danych tytułów filmu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="cadb5-107">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="cadb5-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cadb5-108">Utwórz aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="cadb5-108">Create a web app.</span></span>
> * <span data-ttu-id="cadb5-109">Dodawanie i tworzenie szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="cadb5-110">Pracuj z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="cadb5-110">Work with a database.</span></span>
> * <span data-ttu-id="cadb5-111">Dodaj wyszukiwanie i sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="cadb5-111">Add search and validation.</span></span>

<span data-ttu-id="cadb5-112">Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="cadb5-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cadb5-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cadb5-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cadb5-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cadb5-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cadb5-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cadb5-116">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cadb5-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="cadb5-117">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="cadb5-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cadb5-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cadb5-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cadb5-119">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="cadb5-120">Wybierz **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="cadb5-122">Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="cadb5-123">Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.</span><span class="sxs-lookup"><span data-stu-id="cadb5-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="cadb5-125">Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="cadb5-126">Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="cadb5-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="cadb5-127">Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="cadb5-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="cadb5-128">Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="cadb5-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="cadb5-129">Jest to podstawowy projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="cadb5-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cadb5-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cadb5-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cadb5-131">Samouczek założono familarity z VS Code.</span><span class="sxs-lookup"><span data-stu-id="cadb5-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="cadb5-132">Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="cadb5-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="cadb5-133">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="cadb5-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="cadb5-134">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="cadb5-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="cadb5-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cadb5-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="cadb5-136">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="cadb5-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="cadb5-137">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="cadb5-137">Select **Yes**</span></span>

  * <span data-ttu-id="cadb5-138">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .</span><span class="sxs-lookup"><span data-stu-id="cadb5-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="cadb5-139">`code -r MvcMovie`: ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="cadb5-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cadb5-140">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cadb5-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cadb5-141">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-141">Select **File** > **New Solution**.</span></span>

  ![macOS nowe rozwiązanie](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="cadb5-143">Wybierz pozycję **.NET Core** > **App** > **aplikacja internetowa (Model-View-Controller)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe nowego projektu macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="cadb5-145">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę **docelową** programu **.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="cadb5-146">Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="cadb5-147">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cadb5-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cadb5-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cadb5-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cadb5-149">Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="cadb5-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="cadb5-150">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="cadb5-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="cadb5-151">Zauważ, że na pasku adresu są wyświetlane `localhost:port#`, a nie takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cadb5-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="cadb5-152">Jest to spowodowane tym, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="cadb5-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="cadb5-153">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="cadb5-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="cadb5-154">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="cadb5-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="cadb5-155">Wielu deweloperów preferuje Używanie trybu niedebugowania, aby szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="cadb5-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="cadb5-156">Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :</span><span class="sxs-lookup"><span data-stu-id="cadb5-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="cadb5-158">Możesz debugować aplikację, wybierając przycisk **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cadb5-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="cadb5-160">Na poniższej ilustracji przedstawiono aplikację:</span><span class="sxs-lookup"><span data-stu-id="cadb5-160">The following image shows the app:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cadb5-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cadb5-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cadb5-163">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="cadb5-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="cadb5-164">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="cadb5-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="cadb5-165">Na pasku adresu są wyświetlane `localhost:port:5001`, a nie takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cadb5-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="cadb5-166">Jest to spowodowane tym, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="cadb5-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="cadb5-167">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="cadb5-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="cadb5-168">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="cadb5-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="cadb5-169">Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.</span><span class="sxs-lookup"><span data-stu-id="cadb5-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cadb5-171">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cadb5-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="cadb5-172">Wybierz pozycję **uruchom** > **Rozpocznij bez debugowania** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="cadb5-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="cadb5-173">Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* to losowo wybierany numer portu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="cadb5-174">Na pasku adresu są wyświetlane `localhost:port#`, a nie takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cadb5-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="cadb5-175">Jest to spowodowane tym, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="cadb5-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="cadb5-176">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="cadb5-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="cadb5-177">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="cadb5-178">Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).</span><span class="sxs-lookup"><span data-stu-id="cadb5-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="cadb5-179">Na poniższej ilustracji przedstawiono aplikację:</span><span class="sxs-lookup"><span data-stu-id="cadb5-179">The following image shows the app:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="cadb5-181">W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.</span><span class="sxs-lookup"><span data-stu-id="cadb5-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cadb5-182">Next</span><span class="sxs-lookup"><span data-stu-id="cadb5-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="cadb5-183">Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cadb5-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="cadb5-184">Aplikacja zarządza bazą danych tytułów filmu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="cadb5-185">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="cadb5-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cadb5-186">Utwórz aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="cadb5-186">Create a web app.</span></span>
> * <span data-ttu-id="cadb5-187">Dodawanie i tworzenie szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="cadb5-188">Pracuj z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="cadb5-188">Work with a database.</span></span>
> * <span data-ttu-id="cadb5-189">Dodaj wyszukiwanie i sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="cadb5-189">Add search and validation.</span></span>

<span data-ttu-id="cadb5-190">Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="cadb5-191">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cadb5-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cadb5-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cadb5-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cadb5-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cadb5-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cadb5-194">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cadb5-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="cadb5-195">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="cadb5-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cadb5-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cadb5-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cadb5-197">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="cadb5-198">Wybierz **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-198">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="cadb5-200">Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="cadb5-201">Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.</span><span class="sxs-lookup"><span data-stu-id="cadb5-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="cadb5-203">Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="cadb5-204">Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="cadb5-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="cadb5-205">Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="cadb5-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="cadb5-206">Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="cadb5-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="cadb5-207">Jest to podstawowy projekt początkowy i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="cadb5-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cadb5-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cadb5-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cadb5-209">Samouczek założono familarity z VS Code.</span><span class="sxs-lookup"><span data-stu-id="cadb5-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="cadb5-210">Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="cadb5-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="cadb5-211">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="cadb5-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="cadb5-212">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="cadb5-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="cadb5-213">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cadb5-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="cadb5-214">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="cadb5-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="cadb5-215">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="cadb5-215">Select **Yes**</span></span>

  * <span data-ttu-id="cadb5-216">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .</span><span class="sxs-lookup"><span data-stu-id="cadb5-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="cadb5-217">`code -r MvcMovie`: ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="cadb5-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cadb5-218">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cadb5-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cadb5-219">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-219">Select **File** > **New Solution**.</span></span>

  ![macOS nowe rozwiązanie](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="cadb5-221">Wybierz pozycję **.NET Core** > **App** > **aplikacja internetowa (Model-View-Controller)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe nowego projektu macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="cadb5-223">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** zaakceptuj domyślną **platformę docelową** programu **.NET Core 2,2**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![wybór macOS .NET Core 2,2](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="cadb5-225">Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="cadb5-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="cadb5-226">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cadb5-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cadb5-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cadb5-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cadb5-228">Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="cadb5-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="cadb5-229">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="cadb5-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="cadb5-230">Zauważ, że na pasku adresu są wyświetlane `localhost:port#`, a nie takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cadb5-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="cadb5-231">Jest to spowodowane tym, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="cadb5-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="cadb5-232">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="cadb5-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="cadb5-233">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="cadb5-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="cadb5-234">Wielu deweloperów preferuje Używanie trybu niedebugowania, aby szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="cadb5-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="cadb5-235">Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :</span><span class="sxs-lookup"><span data-stu-id="cadb5-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="cadb5-237">Możesz debugować aplikację, wybierając przycisk **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cadb5-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="cadb5-239">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="cadb5-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="cadb5-240">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="cadb5-240">This app doesn't track personal information.</span></span> <span data-ttu-id="cadb5-241">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="cadb5-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="cadb5-243">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="cadb5-243">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cadb5-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cadb5-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cadb5-246">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="cadb5-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="cadb5-247">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="cadb5-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="cadb5-248">Na pasku adresu są wyświetlane `localhost:port:5001`, a nie takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cadb5-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="cadb5-249">Jest to spowodowane tym, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="cadb5-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="cadb5-250">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="cadb5-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="cadb5-251">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="cadb5-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="cadb5-252">Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.</span><span class="sxs-lookup"><span data-stu-id="cadb5-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="cadb5-253">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="cadb5-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="cadb5-254">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="cadb5-254">This app doesn't track personal information.</span></span> <span data-ttu-id="cadb5-255">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="cadb5-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="cadb5-257">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="cadb5-257">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cadb5-259">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="cadb5-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="cadb5-260">Wybierz pozycję **uruchom** > **Rozpocznij bez debugowania** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="cadb5-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="cadb5-261">Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* to losowo wybierany numer portu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="cadb5-262">Na pasku adresu są wyświetlane `localhost:port#`, a nie takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cadb5-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="cadb5-263">Jest to spowodowane tym, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="cadb5-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="cadb5-264">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="cadb5-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="cadb5-265">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="cadb5-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="cadb5-266">Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).</span><span class="sxs-lookup"><span data-stu-id="cadb5-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="cadb5-267">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="cadb5-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="cadb5-268">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="cadb5-268">This app doesn't track personal information.</span></span> <span data-ttu-id="cadb5-269">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="cadb5-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="cadb5-271">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="cadb5-271">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="cadb5-273">W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.</span><span class="sxs-lookup"><span data-stu-id="cadb5-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cadb5-274">Next</span><span class="sxs-lookup"><span data-stu-id="cadb5-274">Next</span></span>](adding-controller.md)

::: moniker-end
