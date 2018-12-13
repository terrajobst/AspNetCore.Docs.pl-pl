---
title: Wprowadzenie do ASP.NET Core MVC i programu Visual Studio
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dfc9a864cf10db5fc44d5631dcbb2f73325d9e14
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335328"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="053bc-103">Wprowadzenie do ASP.NET Core MVC i programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="053bc-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="053bc-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="053bc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="053bc-105">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="053bc-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="053bc-106">System macOS: [Tworzenie aplikacji MVC platformy ASP.NET Core z programem Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="053bc-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="053bc-107">W systemie Windows: [Tworzenie aplikacji MVC platformy ASP.NET Core z programem Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="053bc-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="053bc-108">z systemem macOS, Linux i Windows: [Tworzenie aplikacji MVC platformy ASP.NET Core przy użyciu programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="053bc-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="053bc-109">Instalowanie programu Visual Studio i .NET Core</span><span class="sxs-lookup"><span data-stu-id="053bc-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="053bc-110">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="053bc-110">Create a web app</span></span>

<span data-ttu-id="053bc-111">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="053bc-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="053bc-113">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="053bc-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="053bc-114">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="053bc-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="053bc-115">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="053bc-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="053bc-116">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="053bc-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="053bc-117">Naciśnij pozycję **OK**</span><span class="sxs-lookup"><span data-stu-id="053bc-117">Tap **OK**</span></span>

![<span data-ttu-id="053bc-118">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="053bc-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="053bc-119">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="053bc-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="053bc-120">W polu listy rozwijanej selektora wersji wybierz **platformy ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="053bc-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="053bc-121">Wybierz **(Model-View-Controller) aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="053bc-121">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="053bc-122">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="053bc-122">Tap **OK**.</span></span>

![<span data-ttu-id="053bc-123">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="053bc-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="053bc-124">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="053bc-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="053bc-125">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="053bc-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="053bc-126">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="053bc-126">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="053bc-127">Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="053bc-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="053bc-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="053bc-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="053bc-129">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="053bc-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="053bc-130">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="053bc-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="053bc-131">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="053bc-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="053bc-132">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="053bc-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="053bc-133">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="053bc-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="053bc-134">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="053bc-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="053bc-135">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="053bc-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="053bc-136">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="053bc-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="053bc-137">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="053bc-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="053bc-138">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="053bc-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="053bc-140">Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="053bc-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="053bc-142">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="053bc-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="053bc-143">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="053bc-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="053bc-144">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="053bc-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="053bc-146">Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="053bc-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="053bc-147">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="053bc-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="053bc-148">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="053bc-148">Create a web app</span></span>

<span data-ttu-id="053bc-149">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="053bc-149">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="053bc-151">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="053bc-151">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="053bc-152">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="053bc-152">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="053bc-153">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="053bc-153">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="053bc-154">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="053bc-154">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="053bc-155">Naciśnij pozycję **OK**</span><span class="sxs-lookup"><span data-stu-id="053bc-155">Tap **OK**</span></span>

![<span data-ttu-id="053bc-156">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="053bc-156">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="053bc-157">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="053bc-157">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="053bc-158">W polu listy rozwijanej selektora wersji wybierz **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="053bc-158">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="053bc-159">Wybierz **Application(Model-View-Controller) w sieci Web**</span><span class="sxs-lookup"><span data-stu-id="053bc-159">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="053bc-160">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="053bc-160">Tap **OK**.</span></span>

![<span data-ttu-id="053bc-161">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="053bc-161">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="053bc-162">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="053bc-162">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="053bc-163">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="053bc-163">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="053bc-164">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia,</span><span class="sxs-lookup"><span data-stu-id="053bc-164">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="053bc-165">Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="053bc-165">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="053bc-166"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="053bc-166"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="053bc-167">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="053bc-167">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="053bc-168">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="053bc-168">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="053bc-169">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="053bc-169">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="053bc-170">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="053bc-170">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="053bc-171">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="053bc-171">In the image above, the port number is 5000.</span></span> <span data-ttu-id="053bc-172">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="053bc-172">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="053bc-173">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="053bc-173">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="053bc-174">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="053bc-174">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="053bc-175">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="053bc-175">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="053bc-176">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="053bc-176">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="053bc-178">Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="053bc-178">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="053bc-180">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="053bc-180">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="053bc-181">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="053bc-181">The browser image above doesn't show these links.</span></span> <span data-ttu-id="053bc-182">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="053bc-182">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="053bc-184">Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="053bc-184">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="053bc-185">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="053bc-185">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="053bc-186">Next</span><span class="sxs-lookup"><span data-stu-id="053bc-186">Next</span></span>](adding-controller.md)  
