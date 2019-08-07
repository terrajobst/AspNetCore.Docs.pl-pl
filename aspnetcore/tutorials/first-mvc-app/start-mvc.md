---
title: Wprowadzenie do ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC.
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f6a92423546ebd9d4c8e1a92fb81b6b72f847f61
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820099"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="9e6e8-103">Wprowadzenie do ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="9e6e8-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="9e6e8-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9e6e8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="9e6e8-105">Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="9e6e8-106">Aplikacja zarządza bazą danych tytułów filmu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="9e6e8-107">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9e6e8-108">Utwórz aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-108">Create a web app.</span></span>
> * <span data-ttu-id="9e6e8-109">Dodawanie i tworzenie szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="9e6e8-110">Pracuj z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-110">Work with a database.</span></span>
> * <span data-ttu-id="9e6e8-111">Dodaj wyszukiwanie i sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-111">Add search and validation.</span></span>

<span data-ttu-id="9e6e8-112">Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="9e6e8-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9e6e8-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e6e8-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e6e8-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e6e8-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e6e8-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e6e8-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9e6e8-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="9e6e8-117">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="9e6e8-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e6e8-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e6e8-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9e6e8-119">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="9e6e8-120">Wybierz **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="9e6e8-122">Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="9e6e8-123">Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="9e6e8-125">Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="9e6e8-126">Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="9e6e8-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="9e6e8-127">Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="9e6e8-128">Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="9e6e8-129">Jest to podstawowy projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e6e8-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e6e8-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9e6e8-131">Samouczek założono familarity z VS Code.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="9e6e8-132">Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="9e6e8-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="9e6e8-133">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9e6e8-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="9e6e8-134">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="9e6e8-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="9e6e8-136">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="9e6e8-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="9e6e8-137">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="9e6e8-137">Select **Yes**</span></span>

  * <span data-ttu-id="9e6e8-138">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .</span><span class="sxs-lookup"><span data-stu-id="9e6e8-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="9e6e8-139">`code -r MvcMovie`: Ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e6e8-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9e6e8-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9e6e8-141">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-141">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="9e6e8-143">Wybierz pozycję aplikacja internetowa **aplikacji** > **.NET Core** > **(Model-View-Controller)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="9e6e8-145">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę docelową programu **.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="9e6e8-146">Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="9e6e8-147">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9e6e8-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e6e8-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e6e8-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9e6e8-149">Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="9e6e8-150">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9e6e8-151">Zauważ, że pasek adresu pokazuje `localhost:port#` , a nie `example.com`coś innego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9e6e8-152">To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="9e6e8-153">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="9e6e8-154">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="9e6e8-155">Wielu deweloperów preferuje Używanie trybu niedebugowania, aby szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="9e6e8-156">Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :</span><span class="sxs-lookup"><span data-stu-id="9e6e8-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="9e6e8-158">Możesz debugować aplikację, wybierając przycisk **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="9e6e8-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)


  <span data-ttu-id="9e6e8-160">Na poniższej ilustracji przedstawiono aplikację:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-160">The following image shows the app:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e6e8-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e6e8-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9e6e8-163">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="9e6e8-164">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="9e6e8-165">Na pasku adresu są `localhost:port:5001` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="9e6e8-166">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9e6e8-167">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="9e6e8-168">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="9e6e8-169">Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="9e6e8-170">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-170">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="9e6e8-171">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-171">This app doesn't track personal information.</span></span> <span data-ttu-id="9e6e8-172">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="9e6e8-172">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="9e6e8-174">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-174">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e6e8-176">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9e6e8-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9e6e8-177">Wybierz pozycję **Uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-177">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="9e6e8-178">Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* jest losowo wybranym numerem portu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-178">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9e6e8-179">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-179">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9e6e8-180">To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-180">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="9e6e8-181">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-181">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="9e6e8-182">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-182">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="9e6e8-183">Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).</span><span class="sxs-lookup"><span data-stu-id="9e6e8-183">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="9e6e8-184">Na poniższej ilustracji przedstawiono aplikację:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-184">The following image shows the app:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="9e6e8-186">W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9e6e8-187">Next</span><span class="sxs-lookup"><span data-stu-id="9e6e8-187">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="9e6e8-188">Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-188">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="9e6e8-189">Aplikacja zarządza bazą danych tytułów filmu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-189">The app manages a database of movie titles.</span></span> <span data-ttu-id="9e6e8-190">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-190">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9e6e8-191">Utwórz aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-191">Create a web app.</span></span>
> * <span data-ttu-id="9e6e8-192">Dodawanie i tworzenie szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-192">Add and scaffold a model.</span></span>
> * <span data-ttu-id="9e6e8-193">Pracuj z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-193">Work with a database.</span></span>
> * <span data-ttu-id="9e6e8-194">Dodaj wyszukiwanie i sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-194">Add search and validation.</span></span>

<span data-ttu-id="9e6e8-195">Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-195">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="9e6e8-196">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9e6e8-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e6e8-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e6e8-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e6e8-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e6e8-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e6e8-199">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9e6e8-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="9e6e8-200">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="9e6e8-200">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e6e8-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e6e8-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9e6e8-202">W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-202">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="9e6e8-203">Selecct **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-203">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="9e6e8-205">Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-205">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="9e6e8-206">Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-206">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="9e6e8-208">Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-208">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="9e6e8-209">Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="9e6e8-209">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="9e6e8-210">Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-210">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="9e6e8-211">Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-211">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="9e6e8-212">Jest to podstawowy projekt początkowy i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-212">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e6e8-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e6e8-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9e6e8-214">Samouczek założono familarity z VS Code.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-214">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="9e6e8-215">Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .</span><span class="sxs-lookup"><span data-stu-id="9e6e8-215">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="9e6e8-216">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9e6e8-216">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="9e6e8-217">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-217">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="9e6e8-218">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-218">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="9e6e8-219">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="9e6e8-219">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="9e6e8-220">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="9e6e8-220">Select **Yes**</span></span>

  * <span data-ttu-id="9e6e8-221">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .</span><span class="sxs-lookup"><span data-stu-id="9e6e8-221">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="9e6e8-222">`code -r MvcMovie`: Ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-222">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e6e8-223">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9e6e8-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9e6e8-224">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-224">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="9e6e8-226">Wybierz pozycję aplikacja internetowa **aplikacji** > **.NET Core** > **(Model-View-Controller)** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-226">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="9e6e8-228">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** zaakceptuj domyślną **platformę** docelową programu **.NET Core 2,2**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-228">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![wybór macOS .NET Core 2,2](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="9e6e8-230">Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-230">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="9e6e8-231">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9e6e8-231">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e6e8-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e6e8-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9e6e8-233">Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-233">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="9e6e8-234">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-234">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9e6e8-235">Zauważ, że pasek adresu pokazuje `localhost:port#` , a nie `example.com`coś innego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-235">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9e6e8-236">To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-236">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="9e6e8-237">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-237">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="9e6e8-238">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-238">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="9e6e8-239">Wielu deweloperów preferuje Używanie trybu niedebugowania, aby szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-239">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="9e6e8-240">Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :</span><span class="sxs-lookup"><span data-stu-id="9e6e8-240">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="9e6e8-242">Możesz debugować aplikację, wybierając przycisk **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="9e6e8-242">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="9e6e8-244">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-244">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="9e6e8-245">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-245">This app doesn't track personal information.</span></span> <span data-ttu-id="9e6e8-246">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="9e6e8-246">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="9e6e8-248">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-248">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e6e8-250">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e6e8-250">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9e6e8-251">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-251">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="9e6e8-252">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-252">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="9e6e8-253">Na pasku adresu są `localhost:port:5001` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-253">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="9e6e8-254">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-254">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9e6e8-255">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-255">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="9e6e8-256">Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-256">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="9e6e8-257">Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-257">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="9e6e8-258">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-258">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="9e6e8-259">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-259">This app doesn't track personal information.</span></span> <span data-ttu-id="9e6e8-260">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="9e6e8-260">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="9e6e8-262">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-262">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e6e8-264">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9e6e8-264">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9e6e8-265">Wybierz pozycję **Uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-265">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="9e6e8-266">Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* jest losowo wybranym numerem portu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-266">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9e6e8-267">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-267">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9e6e8-268">To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-268">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="9e6e8-269">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-269">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="9e6e8-270">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-270">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="9e6e8-271">Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).</span><span class="sxs-lookup"><span data-stu-id="9e6e8-271">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="9e6e8-272">Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-272">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="9e6e8-273">Ta aplikacja nie śledzi informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-273">This app doesn't track personal information.</span></span> <span data-ttu-id="9e6e8-274">Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="9e6e8-274">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="9e6e8-276">Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="9e6e8-276">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="9e6e8-278">W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.</span><span class="sxs-lookup"><span data-stu-id="9e6e8-278">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9e6e8-279">Next</span><span class="sxs-lookup"><span data-stu-id="9e6e8-279">Next</span></span>](adding-controller.md)

::: moniker-end
