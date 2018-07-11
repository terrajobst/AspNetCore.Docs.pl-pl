---
title: Wprowadzenie do ASP.NET Core MVC i programu Visual Studio
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1fb3947023843341403f4355c6ae1e61d7e4f6b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38217981"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="64b71-103">Wprowadzenie do ASP.NET Core MVC i programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64b71-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="64b71-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="64b71-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="64b71-105">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="64b71-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="64b71-106">System macOS: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="64b71-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="64b71-107">Windows: [tworzenie aplikacji MVC platformy ASP.NET Core z programem Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="64b71-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="64b71-108">System macOS, Linux i Windows: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="64b71-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="64b71-109">Instalowanie programu Visual Studio i .NET Core</span><span class="sxs-lookup"><span data-stu-id="64b71-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="64b71-110">[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="64b71-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="64b71-111">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="64b71-111">Create a web app</span></span>

<span data-ttu-id="64b71-112">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="64b71-112">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="64b71-114">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="64b71-114">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="64b71-115">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="64b71-115">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="64b71-116">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="64b71-116">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="64b71-117">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="64b71-117">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="64b71-118">Naciśnij pozycję **OK**</span><span class="sxs-lookup"><span data-stu-id="64b71-118">Tap **OK**</span></span>

![<span data-ttu-id="64b71-119">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b71-119">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="64b71-120">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="64b71-120">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="64b71-121">W polu listy rozwijanej selektora wersji wybierz **platformy ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="64b71-121">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="64b71-122">Wybierz **Application(Model-View-Controller) w sieci Web**</span><span class="sxs-lookup"><span data-stu-id="64b71-122">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="64b71-123">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="64b71-123">Tap **OK**.</span></span>

![<span data-ttu-id="64b71-124">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b71-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="64b71-125">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="64b71-125">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="64b71-126">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="64b71-126">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="64b71-127">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia,</span><span class="sxs-lookup"><span data-stu-id="64b71-127">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="64b71-128">Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="64b71-128">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="64b71-129"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="64b71-129"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="64b71-130">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="64b71-130">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="64b71-131">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="64b71-131">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="64b71-132">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="64b71-132">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="64b71-133">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="64b71-133">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="64b71-134">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="64b71-134">In the image above, the port number is 5000.</span></span> <span data-ttu-id="64b71-135">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="64b71-135">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="64b71-136">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="64b71-136">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="64b71-137">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="64b71-137">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="64b71-138">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="64b71-138">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="64b71-139">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="64b71-139">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="64b71-141">Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="64b71-141">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="64b71-143">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="64b71-143">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="64b71-144">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="64b71-144">The browser image above doesn't show these links.</span></span> <span data-ttu-id="64b71-145">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="64b71-145">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="64b71-147">Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="64b71-147">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="64b71-148">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="64b71-148">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="64b71-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="64b71-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="64b71-150">[! OBEJMUJĄ [] (~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="64b71-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="64b71-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="64b71-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="64b71-152">Install Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="64b71-152">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="64b71-153">Wybierz pakiet do pobrania społeczności.</span><span class="sxs-lookup"><span data-stu-id="64b71-153">Select the Community download.</span></span> <span data-ttu-id="64b71-154">Pomiń ten krok, jeśli masz program Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="64b71-154">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="64b71-155">Instalator programu Visual stronę główną programu Studio 2017</span><span class="sxs-lookup"><span data-stu-id="64b71-155">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="64b71-156">Uruchom Instalatora i wybierz następujące obciążenia:</span><span class="sxs-lookup"><span data-stu-id="64b71-156">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="64b71-157">**ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)</span><span class="sxs-lookup"><span data-stu-id="64b71-157">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="64b71-158">**Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)</span><span class="sxs-lookup"><span data-stu-id="64b71-158">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET i web development ** (w obszarze ** sieć Web i chmura **)](start-mvc/_static/web_workload.png)

![* *.NET core cross-cross-platfrom rozwoju ** (w obszarze ** inne zestawy narzędzi **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="64b71-161">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="64b71-161">Create a web app</span></span>

<span data-ttu-id="64b71-162">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="64b71-162">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="64b71-164">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="64b71-164">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="64b71-165">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="64b71-165">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="64b71-166">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="64b71-166">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="64b71-167">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="64b71-167">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="64b71-168">Naciśnij pozycję **OK**</span><span class="sxs-lookup"><span data-stu-id="64b71-168">Tap **OK**</span></span>

![<span data-ttu-id="64b71-169">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b71-169">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="64b71-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="64b71-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="64b71-171">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="64b71-171">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="64b71-172">W polu listy rozwijanej selektora wersji wybierz **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="64b71-172">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="64b71-173">Wybierz **Application(Model-View-Controller) w sieci Web**</span><span class="sxs-lookup"><span data-stu-id="64b71-173">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="64b71-174">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="64b71-174">Tap **OK**.</span></span>

![<span data-ttu-id="64b71-175">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b71-175">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="64b71-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="64b71-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="64b71-177">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="64b71-177">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="64b71-178">We wzorcu tap pole listy rozwijanej selektora wersji **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="64b71-178">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="64b71-179">Naciśnij pozycję **aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="64b71-179">Tap **Web Application**</span></span>
* <span data-ttu-id="64b71-180">Zachowaj wartość domyślną **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="64b71-180">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="64b71-181">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="64b71-181">Tap **OK**.</span></span>

![Nowa aplikacja sieci web platformy ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="64b71-183">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="64b71-183">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="64b71-184">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="64b71-184">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="64b71-185">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia,</span><span class="sxs-lookup"><span data-stu-id="64b71-185">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="64b71-186">Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="64b71-186">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="64b71-187"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="64b71-187"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="64b71-188">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="64b71-188">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="64b71-189">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="64b71-189">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="64b71-190">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="64b71-190">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="64b71-191">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="64b71-191">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="64b71-192">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="64b71-192">In the image above, the port number is 5000.</span></span> <span data-ttu-id="64b71-193">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="64b71-193">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="64b71-194">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="64b71-194">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="64b71-195">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="64b71-195">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="64b71-196">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="64b71-196">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="64b71-197">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="64b71-197">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="64b71-199">Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="64b71-199">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="64b71-201">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="64b71-201">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="64b71-202">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="64b71-202">The browser image above doesn't show these links.</span></span> <span data-ttu-id="64b71-203">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="64b71-203">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="64b71-205">Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="64b71-205">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="64b71-206">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="64b71-206">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end
> [!div class="step-by-step"]
> [<span data-ttu-id="64b71-207">Next</span><span class="sxs-lookup"><span data-stu-id="64b71-207">Next</span></span>](adding-controller.md)  
