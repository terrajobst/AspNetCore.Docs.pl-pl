---
title: Rozpoczynanie pracy z platformą ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformą ASP.NET Core MVC.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dc3499c89860190b76d6be7b8abeeaef827880d6
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491250"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="62312-103">Rozpoczynanie pracy z platformą ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="62312-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="62312-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="62312-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="62312-105">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="62312-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="62312-106">Aplikacja zarządza bazę tytułów filmów.</span><span class="sxs-lookup"><span data-stu-id="62312-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="62312-107">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="62312-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="62312-108">Tworzenie aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="62312-108">Create a web app.</span></span>
> * <span data-ttu-id="62312-109">Dodaj i tworzenia szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="62312-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="62312-110">Praca z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="62312-110">Work with a database.</span></span>
> * <span data-ttu-id="62312-111">Dodaj wyszukiwanie i sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="62312-111">Add search and validation.</span></span>

<span data-ttu-id="62312-112">Na koniec masz aplikację, która może zarządzać i wyświetlić dane dotyczące filmu.</span><span class="sxs-lookup"><span data-stu-id="62312-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="62312-113">tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="62312-113">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="62312-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62312-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="62312-115">Z programu Visual Studio wybierz opcję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="62312-115">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="62312-116">Selecct **aplikacji sieci Web programu ASP.NET Core** , a następnie wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="62312-116">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nowa aplikacja internetowa ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="62312-118">Nadaj projektowi nazwę **MvcMovie** i wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="62312-118">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="62312-119">Ważne jest, aby nadaj projektowi nazwę **MvcMovie** tak podczas kopiowania kodu przestrzeni nazw będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="62312-119">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="62312-121">Wybierz **Web Application(Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="62312-121">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="62312-122">Okno dialogowe nowego projektu, .NET Core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62312-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="62312-123">Program Visual Studio używany domyślny szablon projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="62312-123">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="62312-124">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="62312-124">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="62312-125">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="62312-125">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="62312-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62312-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="62312-127">W samouczku założono familarity z programem VS Code.</span><span class="sxs-lookup"><span data-stu-id="62312-127">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="62312-128">Zobacz [wprowadzenie do programu VS Code](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="62312-128">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="62312-129">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="62312-129">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="62312-130">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="62312-130">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="62312-131">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="62312-131">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="62312-132">Zostanie wyświetlone okno dialogowe z **"MvcMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="62312-132">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="62312-133">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="62312-133">Select **Yes**</span></span>

  * <span data-ttu-id="62312-134">`dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w *MvcMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="62312-134">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="62312-135">`code -r MvcMovie`: Ładunki *MvcMovie.csproj* plik projektu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="62312-135">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="62312-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="62312-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="62312-137">Wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="62312-137">Select **File** > **New Solution**.</span></span>

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="62312-139">Wybierz **platformy .NET Core** > **aplikacji** >  **(Model-View-Controller) aplikacji sieci Web** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="62312-139">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="62312-141">W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z **platformy .NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="62312-141">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![System macOS wybór platformy .NET Core 2.2](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="62312-143">Nadaj projektowi nazwę **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="62312-143">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="62312-144">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="62312-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="62312-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62312-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="62312-146">Wybierz **Ctrl-F5** do uruchomienia aplikacji w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="62312-146">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="62312-147">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="62312-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="62312-148">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="62312-148">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="62312-149">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="62312-149">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="62312-150">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="62312-150">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="62312-151">Uruchamianie aplikacji za pomocą klawiszy Ctrl + F5 (bez debugowania w trybie) umożliwia wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="62312-151">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="62312-152">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="62312-152">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="62312-153">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="62312-153">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="62312-155">Można debugować aplikację, wybierając **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="62312-155">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="62312-157">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="62312-157">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="62312-158">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="62312-158">This app doesn't track personal information.</span></span> <span data-ttu-id="62312-159">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="62312-159">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="62312-161">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="62312-161">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="62312-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62312-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="62312-164">Naciśnij klawisze Ctrl + F5, aby uruchomić bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="62312-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="62312-165">Uruchamia programu Visual Studio Code [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="62312-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="62312-166">Przedstawia pasek adresu `localhost:port:5001` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="62312-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="62312-167">To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="62312-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="62312-168">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="62312-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="62312-169">Uruchamianie aplikacji za pomocą klawiszy Ctrl + F5 (bez debugowania w trybie) umożliwia wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="62312-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="62312-170">Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="62312-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="62312-171">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="62312-171">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="62312-172">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="62312-172">This app doesn't track personal information.</span></span> <span data-ttu-id="62312-173">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="62312-173">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  <span data-ttu-id="62312-175">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="62312-175">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="62312-177">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="62312-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="62312-178">Wybierz **Uruchom** > **Rozpocznij bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="62312-178">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="62312-179">Program Visual Studio for Mac rozpoczyna [Kestrel](xref:fundamentals/servers/index#kestrel) serwera spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="62312-179">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="62312-180">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="62312-180">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="62312-181">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="62312-181">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="62312-182">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="62312-182">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="62312-183">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="62312-183">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="62312-184">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **Uruchom** menu.</span><span class="sxs-lookup"><span data-stu-id="62312-184">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="62312-185">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="62312-185">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="62312-186">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="62312-186">This app doesn't track personal information.</span></span> <span data-ttu-id="62312-187">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="62312-187">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="62312-189">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="62312-189">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="62312-191">W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="62312-191">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="62312-192">Next</span><span class="sxs-lookup"><span data-stu-id="62312-192">Next</span></span>](adding-controller.md)
