---
title: Wprowadzenie do ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 901257efdfbc7b36249233745175f5ed253da2c7
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722892"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="dfc8f-103">Wprowadzenie do ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="dfc8f-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="dfc8f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dfc8f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="dfc8f-105">Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="dfc8f-106">Aplikacja zarządza bazą danych tytułów filmu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="dfc8f-107">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfc8f-108">Utwórz aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-108">Create a web app.</span></span>
> * <span data-ttu-id="dfc8f-109">Dodawanie i tworzenie szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="dfc8f-110">Pracuj z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-110">Work with a database.</span></span>
> * <span data-ttu-id="dfc8f-111">Dodaj wyszukiwanie i sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-111">Add search and validation.</span></span>

<span data-ttu-id="dfc8f-112">Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="dfc8f-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="dfc8f-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc8f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc8f-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc8f-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfc8f-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc8f-116">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="dfc8f-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="dfc8f-117">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="dfc8f-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc8f-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc8f-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dfc8f-119">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="dfc8f-120">Wybierz **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="dfc8f-122">Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="dfc8f-123">Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="dfc8f-125">Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="dfc8f-126">Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="dfc8f-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="dfc8f-127">Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="dfc8f-128">Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="dfc8f-129">Jest to podstawowy projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc8f-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfc8f-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dfc8f-131">Samouczek założono familarity z VS Code.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="dfc8f-132">Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="dfc8f-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="dfc8f-133">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dfc8f-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="dfc8f-134">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="dfc8f-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="dfc8f-136">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="dfc8f-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="dfc8f-137">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="dfc8f-137">Select **Yes**</span></span>

  * <span data-ttu-id="dfc8f-138">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .</span><span class="sxs-lookup"><span data-stu-id="dfc8f-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="dfc8f-139">`code -r MvcMovie`: ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc8f-140">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="dfc8f-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dfc8f-141">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-141">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="dfc8f-143">Wybierz pozycję **.NET Core** > **App** > **aplikacji sieci Web (Model-View-Controller)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="dfc8f-145">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę **docelową** programu **.NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

  ![wybór macOS .NET Core 3,1](./start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="dfc8f-147">Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="dfc8f-148">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="dfc8f-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc8f-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc8f-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dfc8f-150">Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="dfc8f-151">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="dfc8f-152">Zauważ, że na pasku adresu są wyświetlane `localhost:port#` a nie `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dfc8f-153">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="dfc8f-154">Podczas tworzenia projektu internetowego w programie Visual Studio dla serwera internetowego jest używany losowy port.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="dfc8f-155">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="dfc8f-156">Wielu deweloperów woli używać trybu bez debugowania, aby móc szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="dfc8f-157">Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :</span><span class="sxs-lookup"><span data-stu-id="dfc8f-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="dfc8f-159">Możesz debugować aplikację, wybierając przycisk **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="dfc8f-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![Usługi IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="dfc8f-161">Na poniższej ilustracji przedstawiono aplikację:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-161">The following image shows the app:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc8f-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfc8f-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dfc8f-164">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="dfc8f-165">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="dfc8f-166">Na pasku adresu są wyświetlane `localhost:port:5001` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="dfc8f-167">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="dfc8f-168">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="dfc8f-169">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="dfc8f-170">Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc8f-172">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="dfc8f-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dfc8f-173">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="dfc8f-174">Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* to losowo wybierany numer portu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="dfc8f-175">Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dfc8f-176">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="dfc8f-177">Podczas tworzenia projektu internetowego w programie Visual Studio dla serwera internetowego jest używany losowy port.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="dfc8f-178">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="dfc8f-179">Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).</span><span class="sxs-lookup"><span data-stu-id="dfc8f-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="dfc8f-180">Na poniższej ilustracji przedstawiono aplikację:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-180">The following image shows the app:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="dfc8f-182">W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-182">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dfc8f-183">Next</span><span class="sxs-lookup"><span data-stu-id="dfc8f-183">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="dfc8f-184">Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-184">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="dfc8f-185">Aplikacja zarządza bazą danych tytułów filmu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-185">The app manages a database of movie titles.</span></span> <span data-ttu-id="dfc8f-186">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-186">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfc8f-187">Utwórz aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-187">Create a web app.</span></span>
> * <span data-ttu-id="dfc8f-188">Dodawanie i tworzenie szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-188">Add and scaffold a model.</span></span>
> * <span data-ttu-id="dfc8f-189">Pracuj z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-189">Work with a database.</span></span>
> * <span data-ttu-id="dfc8f-190">Dodaj wyszukiwanie i sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-190">Add search and validation.</span></span>

<span data-ttu-id="dfc8f-191">Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-191">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="dfc8f-192">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="dfc8f-192">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc8f-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc8f-193">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc8f-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfc8f-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc8f-195">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="dfc8f-195">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="dfc8f-196">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="dfc8f-196">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc8f-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc8f-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dfc8f-198">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-198">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="dfc8f-199">Wybierz **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-199">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="dfc8f-201">Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-201">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="dfc8f-202">Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-202">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="dfc8f-204">Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-204">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="dfc8f-205">Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="dfc8f-205">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="dfc8f-206">Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-206">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="dfc8f-207">Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-207">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="dfc8f-208">Jest to podstawowy projekt początkowy i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-208">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc8f-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfc8f-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dfc8f-210">Samouczek założono familarity z VS Code.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-210">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="dfc8f-211">Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="dfc8f-211">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="dfc8f-212">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dfc8f-212">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="dfc8f-213">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-213">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="dfc8f-214">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-214">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="dfc8f-215">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="dfc8f-215">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="dfc8f-216">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="dfc8f-216">Select **Yes**</span></span>

  * <span data-ttu-id="dfc8f-217">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .</span><span class="sxs-lookup"><span data-stu-id="dfc8f-217">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="dfc8f-218">`code -r MvcMovie`: ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-218">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc8f-219">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="dfc8f-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dfc8f-220">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-220">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="dfc8f-222">Wybierz pozycję **.NET Core** > **App** > **aplikacji sieci Web (Model-View-Controller)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-222">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="dfc8f-224">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** zaakceptuj domyślną **platformę docelową** programu **.NET Core 2,2**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-224">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![wybór macOS .NET Core 2,2](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="dfc8f-226">Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-226">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="dfc8f-227">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="dfc8f-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc8f-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc8f-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dfc8f-229">Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-229">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="dfc8f-230">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="dfc8f-231">Zauważ, że na pasku adresu są wyświetlane `localhost:port#` a nie `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-231">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dfc8f-232">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-232">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="dfc8f-233">Podczas tworzenia projektu internetowego w programie Visual Studio dla serwera internetowego jest używany losowy port.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="dfc8f-234">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-234">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="dfc8f-235">Wielu deweloperów woli używać trybu bez debugowania, aby móc szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-235">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="dfc8f-236">Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :</span><span class="sxs-lookup"><span data-stu-id="dfc8f-236">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="dfc8f-238">Możesz debugować aplikację, wybierając przycisk **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="dfc8f-238">You can debug the app by selecting the **IIS Express** button</span></span>

  ![Usługi IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="dfc8f-240">Wybierz pozycję **Zaakceptuj**, aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-240">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="dfc8f-241">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-241">This app doesn't track personal information.</span></span> <span data-ttu-id="dfc8f-242">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dfc8f-242">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="dfc8f-244">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-244">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc8f-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfc8f-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dfc8f-247">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-247">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="dfc8f-248">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="dfc8f-249">Na pasku adresu są wyświetlane `localhost:port:5001` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-249">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="dfc8f-250">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="dfc8f-251">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-251">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="dfc8f-252">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-252">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="dfc8f-253">Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-253">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="dfc8f-254">Wybierz pozycję **Zaakceptuj**, aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-254">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="dfc8f-255">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-255">This app doesn't track personal information.</span></span> <span data-ttu-id="dfc8f-256">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dfc8f-256">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="dfc8f-258">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-258">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc8f-260">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="dfc8f-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dfc8f-261">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-261">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="dfc8f-262">Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* to losowo wybierany numer portu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-262">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="dfc8f-263">Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-263">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dfc8f-264">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-264">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="dfc8f-265">Podczas tworzenia projektu internetowego w programie Visual Studio dla serwera internetowego jest używany losowy port.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-265">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="dfc8f-266">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-266">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="dfc8f-267">Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).</span><span class="sxs-lookup"><span data-stu-id="dfc8f-267">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="dfc8f-268">Wybierz pozycję **Zaakceptuj**, aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-268">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="dfc8f-269">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-269">This app doesn't track personal information.</span></span> <span data-ttu-id="dfc8f-270">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dfc8f-270">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="dfc8f-272">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="dfc8f-272">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="dfc8f-274">W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.</span><span class="sxs-lookup"><span data-stu-id="dfc8f-274">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dfc8f-275">Next</span><span class="sxs-lookup"><span data-stu-id="dfc8f-275">Next</span></span>](adding-controller.md)

::: moniker-end
