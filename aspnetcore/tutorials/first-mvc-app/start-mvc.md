---
title: Rozpoczynanie pracy z platformą ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformą ASP.NET Core MVC.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 91564bac02df77632a3b56b6dbddeb3b622f6438
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555850"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="4c8b7-103">Rozpoczynanie pracy z platformą ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="4c8b7-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="4c8b7-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4c8b7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="4c8b7-105">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="4c8b7-106">Aplikacja zarządza bazę tytułów filmów.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="4c8b7-107">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="4c8b7-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4c8b7-108">Tworzenie aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-108">Create a web app.</span></span>
> * <span data-ttu-id="4c8b7-109">Dodaj i tworzenia szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="4c8b7-110">Praca z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-110">Work with a database.</span></span>
> * <span data-ttu-id="4c8b7-111">Dodaj wyszukiwanie i sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-111">Add search and validation.</span></span>

<span data-ttu-id="4c8b7-112">Na koniec masz aplikację, która może zarządzać i wyświetlić dane dotyczące filmu.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="4c8b7-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4c8b7-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c8b7-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c8b7-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4c8b7-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4c8b7-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4c8b7-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4c8b7-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="4c8b7-117">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="4c8b7-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c8b7-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c8b7-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4c8b7-119">Z programu Visual Studio wybierz opcję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="4c8b7-120">Selecct **aplikacji sieci Web programu ASP.NET Core** , a następnie wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-120">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja internetowa ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="4c8b7-122">Nadaj projektowi nazwę **MvcMovie** i wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="4c8b7-123">Ważne jest, aby nadaj projektowi nazwę **MvcMovie** tak podczas kopiowania kodu przestrzeni nazw będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="4c8b7-125">Wybierz **Web Application(Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="4c8b7-126">Okno dialogowe nowego projektu, .NET Core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c8b7-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="4c8b7-127">Program Visual Studio używany domyślny szablon projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="4c8b7-128">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="4c8b7-129">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-129">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4c8b7-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4c8b7-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4c8b7-131">W samouczku założono familarity z programem VS Code.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="4c8b7-132">Zobacz [wprowadzenie do programu VS Code](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="4c8b7-133">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4c8b7-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4c8b7-134">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4c8b7-135">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4c8b7-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="4c8b7-136">Zostanie wyświetlone okno dialogowe z **"MvcMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="4c8b7-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="4c8b7-137">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="4c8b7-137">Select **Yes**</span></span>

  * <span data-ttu-id="4c8b7-138">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w *MvcMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="4c8b7-139">`code -r MvcMovie`: Ładunki *MvcMovie.csproj* plik projektu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4c8b7-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4c8b7-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4c8b7-141">Wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-141">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="4c8b7-143">Wybierz **platformy .NET Core** > **aplikacji** >  **(Model-View-Controller) aplikacji sieci Web** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="4c8b7-145">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z **platformy .NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-145">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![System macOS wybór platformy .NET Core 2.2](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="4c8b7-147">Nadaj projektowi nazwę **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="4c8b7-148">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4c8b7-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c8b7-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c8b7-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4c8b7-150">Wybierz **Ctrl-F5** do uruchomienia aplikacji w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="4c8b7-151">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="4c8b7-152">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4c8b7-153">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4c8b7-154">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="4c8b7-155">Uruchamianie aplikacji za pomocą klawiszy Ctrl + F5 (bez debugowania w trybie) umożliwia wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4c8b7-156">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="4c8b7-157">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="4c8b7-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="4c8b7-159">Można debugować aplikację, wybierając **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="4c8b7-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="4c8b7-161">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-161">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4c8b7-162">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-162">This app doesn't track personal information.</span></span> <span data-ttu-id="4c8b7-163">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="4c8b7-163">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="4c8b7-165">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="4c8b7-165">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4c8b7-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4c8b7-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4c8b7-168">Naciśnij klawisze Ctrl + F5, aby uruchomić bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-168">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="4c8b7-169">Uruchamia programu Visual Studio Code [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-169">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="4c8b7-170">Przedstawia pasek adresu `localhost:port:5001` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-170">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="4c8b7-171">To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-171">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="4c8b7-172">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-172">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="4c8b7-173">Uruchamianie aplikacji za pomocą klawiszy Ctrl + F5 (bez debugowania w trybie) umożliwia wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-173">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4c8b7-174">Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-174">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="4c8b7-175">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-175">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4c8b7-176">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-176">This app doesn't track personal information.</span></span> <span data-ttu-id="4c8b7-177">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="4c8b7-177">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="4c8b7-179">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="4c8b7-179">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4c8b7-181">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4c8b7-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4c8b7-182">Wybierz **Uruchom** > **Rozpocznij bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-182">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="4c8b7-183">Program Visual Studio for Mac rozpoczyna [Kestrel](xref:fundamentals/servers/index#kestrel) serwera spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-183">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="4c8b7-184">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-184">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4c8b7-185">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-185">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4c8b7-186">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-186">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4c8b7-187">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-187">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="4c8b7-188">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **Uruchom** menu.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-188">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="4c8b7-189">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-189">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4c8b7-190">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-190">This app doesn't track personal information.</span></span> <span data-ttu-id="4c8b7-191">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="4c8b7-191">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="4c8b7-193">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="4c8b7-193">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="4c8b7-195">W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="4c8b7-195">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4c8b7-196">Next</span><span class="sxs-lookup"><span data-stu-id="4c8b7-196">Next</span></span>](adding-controller.md)
