---
title: Wprowadzenie do ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC.
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 36f4811f876a6e35440445103a1f86ae06b31b6a
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022524"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="7e8d4-103">Wprowadzenie do ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="7e8d4-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="7e8d4-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e8d4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="7e8d4-105">Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="7e8d4-106">Aplikacja zarządza bazą danych tytułów filmu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="7e8d4-107">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7e8d4-108">Utwórz aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-108">Create a web app.</span></span>
> * <span data-ttu-id="7e8d4-109">Dodawanie i tworzenie szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="7e8d4-110">Pracuj z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-110">Work with a database.</span></span>
> * <span data-ttu-id="7e8d4-111">Dodaj wyszukiwanie i sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-111">Add search and validation.</span></span>

<span data-ttu-id="7e8d4-112">Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="7e8d4-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7e8d4-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7e8d4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e8d4-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7e8d4-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7e8d4-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7e8d4-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7e8d4-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="7e8d4-117">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="7e8d4-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7e8d4-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e8d4-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7e8d4-119">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="7e8d4-120">Wybierz **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="7e8d4-122">Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="7e8d4-123">Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="7e8d4-125">Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="7e8d4-126">Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="7e8d4-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="7e8d4-127">Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="7e8d4-128">Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7e8d4-129">Jest to podstawowy projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7e8d4-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7e8d4-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7e8d4-131">Samouczek założono familarity z VS Code.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="7e8d4-132">Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="7e8d4-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="7e8d4-133">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="7e8d4-134">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="7e8d4-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="7e8d4-136">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="7e8d4-137">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-137">Select **Yes**</span></span>

  * <span data-ttu-id="7e8d4-138">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .</span><span class="sxs-lookup"><span data-stu-id="7e8d4-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="7e8d4-139">`code -r MvcMovie`: Ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7e8d4-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7e8d4-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7e8d4-141">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-141">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="7e8d4-143">Wybierz pozycję aplikacja internetowa **aplikacji** > **.NET Core** > **(Model-View-Controller)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="7e8d4-145">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę docelową programu **.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="7e8d4-146">Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="7e8d4-147">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7e8d4-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7e8d4-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e8d4-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7e8d4-149">Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="7e8d4-150">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="7e8d4-151">Zauważ, że pasek adresu pokazuje `localhost:port#` , a nie `example.com`coś innego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7e8d4-152">To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7e8d4-153">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="7e8d4-154">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7e8d4-155">Wielu deweloperów preferuje Używanie trybu niedebugowania, aby szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7e8d4-156">Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :</span><span class="sxs-lookup"><span data-stu-id="7e8d4-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7e8d4-158">Możesz debugować aplikację, wybierając przycisk **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="7e8d4-160">Na poniższej ilustracji przedstawiono aplikację:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-160">The following image shows the app:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7e8d4-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7e8d4-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7e8d4-163">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="7e8d4-164">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="7e8d4-165">Na pasku adresu są `localhost:port:5001` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="7e8d4-166">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="7e8d4-167">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="7e8d4-168">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7e8d4-169">Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7e8d4-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7e8d4-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7e8d4-172">Wybierz pozycję **Uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="7e8d4-173">Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* jest losowo wybranym numerem portu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="7e8d4-174">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7e8d4-175">To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7e8d4-176">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7e8d4-177">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7e8d4-178">Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="7e8d4-179">Na poniższej ilustracji przedstawiono aplikację:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-179">The following image shows the app:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="7e8d4-181">W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7e8d4-182">Next</span><span class="sxs-lookup"><span data-stu-id="7e8d4-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="7e8d4-183">Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="7e8d4-184">Aplikacja zarządza bazą danych tytułów filmu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="7e8d4-185">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7e8d4-186">Utwórz aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-186">Create a web app.</span></span>
> * <span data-ttu-id="7e8d4-187">Dodawanie i tworzenie szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="7e8d4-188">Pracuj z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-188">Work with a database.</span></span>
> * <span data-ttu-id="7e8d4-189">Dodaj wyszukiwanie i sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-189">Add search and validation.</span></span>

<span data-ttu-id="7e8d4-190">Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="7e8d4-191">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7e8d4-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7e8d4-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e8d4-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7e8d4-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7e8d4-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7e8d4-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7e8d4-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="7e8d4-195">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="7e8d4-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7e8d4-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e8d4-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7e8d4-197">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="7e8d4-198">Selecct **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-198">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="7e8d4-200">Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="7e8d4-201">Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="7e8d4-203">Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="7e8d4-204">Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="7e8d4-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="7e8d4-205">Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="7e8d4-206">Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7e8d4-207">Jest to podstawowy projekt początkowy i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7e8d4-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7e8d4-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7e8d4-209">Samouczek założono familarity z VS Code.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="7e8d4-210">Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="7e8d4-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="7e8d4-211">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="7e8d4-212">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="7e8d4-213">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-213">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="7e8d4-214">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="7e8d4-215">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-215">Select **Yes**</span></span>

  * <span data-ttu-id="7e8d4-216">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .</span><span class="sxs-lookup"><span data-stu-id="7e8d4-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="7e8d4-217">`code -r MvcMovie`: Ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7e8d4-218">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7e8d4-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7e8d4-219">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-219">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="7e8d4-221">Wybierz pozycję aplikacja internetowa **aplikacji** > **.NET Core** > **(Model-View-Controller)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="7e8d4-223">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** zaakceptuj domyślną **platformę** docelową programu **.NET Core 2,2**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![wybór macOS .NET Core 2,2](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="7e8d4-225">Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="7e8d4-226">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7e8d4-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7e8d4-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e8d4-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7e8d4-228">Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="7e8d4-229">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="7e8d4-230">Zauważ, że pasek adresu pokazuje `localhost:port#` , a nie `example.com`coś innego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7e8d4-231">To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7e8d4-232">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="7e8d4-233">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7e8d4-234">Wielu deweloperów preferuje Używanie trybu niedebugowania, aby szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7e8d4-235">Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :</span><span class="sxs-lookup"><span data-stu-id="7e8d4-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7e8d4-237">Możesz debugować aplikację, wybierając przycisk **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="7e8d4-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="7e8d4-239">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="7e8d4-240">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-240">This app doesn't track personal information.</span></span> <span data-ttu-id="7e8d4-241">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="7e8d4-243">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-243">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7e8d4-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7e8d4-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7e8d4-246">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="7e8d4-247">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="7e8d4-248">Na pasku adresu są `localhost:port:5001` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="7e8d4-249">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="7e8d4-250">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="7e8d4-251">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7e8d4-252">Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="7e8d4-253">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="7e8d4-254">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-254">This app doesn't track personal information.</span></span> <span data-ttu-id="7e8d4-255">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="7e8d4-257">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-257">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7e8d4-259">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7e8d4-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7e8d4-260">Wybierz pozycję **Uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="7e8d4-261">Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* jest losowo wybranym numerem portu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="7e8d4-262">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7e8d4-263">To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7e8d4-264">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7e8d4-265">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7e8d4-266">Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="7e8d4-267">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="7e8d4-268">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-268">This app doesn't track personal information.</span></span> <span data-ttu-id="7e8d4-269">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7e8d4-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="7e8d4-271">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="7e8d4-271">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="7e8d4-273">W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.</span><span class="sxs-lookup"><span data-stu-id="7e8d4-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7e8d4-274">Next</span><span class="sxs-lookup"><span data-stu-id="7e8d4-274">Next</span></span>](adding-controller.md)

::: moniker-end
