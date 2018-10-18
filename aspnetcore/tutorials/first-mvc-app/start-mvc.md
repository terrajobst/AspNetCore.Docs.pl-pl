---
title: Wprowadzenie do ASP.NET Core MVC i programu Visual Studio
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: fe555e4cfcaec5d4bb8ccee00b06d1bbcaae9dcd
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391209"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="1ed2f-103">Wprowadzenie do ASP.NET Core MVC i programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ed2f-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="1ed2f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="1ed2f-105">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="1ed2f-106">System macOS: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="1ed2f-107">Windows: [tworzenie aplikacji MVC platformy ASP.NET Core z programem Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="1ed2f-108">System macOS, Linux i Windows: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="1ed2f-109">Instalowanie programu Visual Studio i .NET Core</span><span class="sxs-lookup"><span data-stu-id="1ed2f-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="1ed2f-110">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="1ed2f-110">Create a web app</span></span>

<span data-ttu-id="1ed2f-111">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="1ed2f-113">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="1ed2f-114">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="1ed2f-115">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="1ed2f-116">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="1ed2f-117">Naciśnij pozycję **OK**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-117">Tap **OK**</span></span>

![<span data-ttu-id="1ed2f-118">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ed2f-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="1ed2f-119">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="1ed2f-120">W polu listy rozwijanej selektora wersji wybierz **platformy ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="1ed2f-121">Wybierz **(Model-View-Controller) aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-121">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="1ed2f-122">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-122">Tap **OK**.</span></span>

![<span data-ttu-id="1ed2f-123">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ed2f-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="1ed2f-124">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="1ed2f-125">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="1ed2f-126">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-126">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="1ed2f-127">Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="1ed2f-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="1ed2f-129">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="1ed2f-130">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1ed2f-131">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="1ed2f-132">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="1ed2f-133">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="1ed2f-134">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="1ed2f-135">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="1ed2f-136">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="1ed2f-137">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="1ed2f-138">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="1ed2f-140">Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="1ed2f-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="1ed2f-142">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="1ed2f-143">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="1ed2f-144">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="1ed2f-146">Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="1ed2f-147">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ed2f-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1ed2f-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ed2f-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1ed2f-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="1ed2f-150">Install Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="1ed2f-151">Wybierz pakiet do pobrania społeczności.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-151">Select the Community download.</span></span> <span data-ttu-id="1ed2f-152">Pomiń ten krok, jeśli masz program Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="1ed2f-153">Instalator programu Visual stronę główną programu Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1ed2f-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="1ed2f-154">Uruchom Instalatora i wybierz następujące obciążenia:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="1ed2f-155">**ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="1ed2f-156">**Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![\*\*ASP.NET i web development \*\* (w obszarze \*\* sieć Web i chmura \*\*)](start-mvc/_static/web_workload.png)

![\* \*.NET core cross-cross-platfrom rozwoju \*\* (w obszarze \*\* inne zestawy narzędzi \*\*)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="1ed2f-159">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="1ed2f-159">Create a web app</span></span>

<span data-ttu-id="1ed2f-160">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-160">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="1ed2f-162">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="1ed2f-163">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="1ed2f-164">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="1ed2f-165">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="1ed2f-166">Naciśnij pozycję **OK**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-166">Tap **OK**</span></span>

![<span data-ttu-id="1ed2f-167">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ed2f-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ed2f-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1ed2f-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1ed2f-169">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="1ed2f-170">W polu listy rozwijanej selektora wersji wybierz **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="1ed2f-171">Wybierz **Application(Model-View-Controller) w sieci Web**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="1ed2f-172">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-172">Tap **OK**.</span></span>

![<span data-ttu-id="1ed2f-173">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ed2f-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ed2f-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1ed2f-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ed2f-175">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="1ed2f-176">We wzorcu tap pole listy rozwijanej selektora wersji **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="1ed2f-177">Naciśnij pozycję **aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-177">Tap **Web Application**</span></span>
* <span data-ttu-id="1ed2f-178">Zachowaj wartość domyślną **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="1ed2f-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="1ed2f-179">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-179">Tap **OK**.</span></span>

![Nowa aplikacja sieci web platformy ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="1ed2f-181">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="1ed2f-182">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="1ed2f-183">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia,</span><span class="sxs-lookup"><span data-stu-id="1ed2f-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="1ed2f-184">Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="1ed2f-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="1ed2f-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="1ed2f-186">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="1ed2f-187">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1ed2f-188">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="1ed2f-189">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="1ed2f-190">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="1ed2f-191">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="1ed2f-192">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="1ed2f-193">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="1ed2f-194">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="1ed2f-195">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="1ed2f-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="1ed2f-197">Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="1ed2f-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="1ed2f-199">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="1ed2f-200">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="1ed2f-201">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="1ed2f-203">Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="1ed2f-204">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="1ed2f-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="1ed2f-205">Next</span><span class="sxs-lookup"><span data-stu-id="1ed2f-205">Next</span></span>](adding-controller.md)  
