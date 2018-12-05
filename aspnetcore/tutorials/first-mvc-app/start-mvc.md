---
title: Wprowadzenie do ASP.NET Core MVC i programu Visual Studio
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 738c49272c2ae2b075866001f06ad09fe73969f9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862203"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="06289-103">Wprowadzenie do ASP.NET Core MVC i programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06289-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="06289-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="06289-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="06289-105">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="06289-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="06289-106">System macOS: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="06289-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="06289-107">Windows: [tworzenie aplikacji MVC platformy ASP.NET Core z programem Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="06289-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="06289-108">System macOS, Linux i Windows: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="06289-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

> [!NOTE]
> <span data-ttu-id="06289-109">W tej chwili testujemy użyteczność nowo zaproponowanej struktury spisu treści dla dokumentacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06289-109">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="06289-110">Jeśli możesz poświęcić chwilę na wykonanie ćwiczenia polegającego na znalezieniu 7 różnych tematów w aktualnym lub zaproponowanym spisie treści, [kliknij tutaj i weź udział w badaniu](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="06289-110">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="06289-111">Instalowanie programu Visual Studio i .NET Core</span><span class="sxs-lookup"><span data-stu-id="06289-111">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="06289-112">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="06289-112">Create a web app</span></span>

<span data-ttu-id="06289-113">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="06289-113">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="06289-115">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="06289-115">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="06289-116">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="06289-116">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="06289-117">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="06289-117">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="06289-118">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="06289-118">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="06289-119">Naciśnij pozycję **OK**</span><span class="sxs-lookup"><span data-stu-id="06289-119">Tap **OK**</span></span>

![<span data-ttu-id="06289-120">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06289-120">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="06289-121">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="06289-121">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="06289-122">W polu listy rozwijanej selektora wersji wybierz **platformy ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="06289-122">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="06289-123">Wybierz **(Model-View-Controller) aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="06289-123">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="06289-124">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="06289-124">Tap **OK**.</span></span>

![<span data-ttu-id="06289-125">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06289-125">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="06289-126">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="06289-126">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="06289-127">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="06289-127">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="06289-128">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="06289-128">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="06289-129">Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="06289-129">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="06289-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="06289-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="06289-131">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="06289-131">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="06289-132">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="06289-132">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="06289-133">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="06289-133">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="06289-134">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="06289-134">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="06289-135">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="06289-135">In the image above, the port number is 5000.</span></span> <span data-ttu-id="06289-136">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="06289-136">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="06289-137">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="06289-137">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="06289-138">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="06289-138">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="06289-139">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="06289-139">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="06289-140">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="06289-140">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="06289-142">Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="06289-142">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="06289-144">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="06289-144">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="06289-145">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="06289-145">The browser image above doesn't show these links.</span></span> <span data-ttu-id="06289-146">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="06289-146">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="06289-148">Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="06289-148">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="06289-149">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="06289-149">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="06289-150">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="06289-150">Create a web app</span></span>

<span data-ttu-id="06289-151">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="06289-151">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="06289-153">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="06289-153">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="06289-154">W okienku po lewej stronie wybierz **platformy .NET Core**</span><span class="sxs-lookup"><span data-stu-id="06289-154">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="06289-155">W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="06289-155">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="06289-156">Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="06289-156">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="06289-157">Naciśnij pozycję **OK**</span><span class="sxs-lookup"><span data-stu-id="06289-157">Tap **OK**</span></span>

![<span data-ttu-id="06289-158">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06289-158">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="06289-159">Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="06289-159">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="06289-160">W polu listy rozwijanej selektora wersji wybierz **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="06289-160">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="06289-161">Wybierz **Application(Model-View-Controller) w sieci Web**</span><span class="sxs-lookup"><span data-stu-id="06289-161">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="06289-162">Naciśnij pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="06289-162">Tap **OK**.</span></span>

![<span data-ttu-id="06289-163">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06289-163">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="06289-164">Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="06289-164">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="06289-165">Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji.</span><span class="sxs-lookup"><span data-stu-id="06289-165">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="06289-166">Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia,</span><span class="sxs-lookup"><span data-stu-id="06289-166">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="06289-167">Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="06289-167">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="06289-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="06289-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="06289-169">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="06289-169">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="06289-170">Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="06289-170">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="06289-171">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="06289-171">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="06289-172">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="06289-172">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="06289-173">Na powyższej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="06289-173">In the image above, the port number is 5000.</span></span> <span data-ttu-id="06289-174">Adres URL w pokazuje przeglądarki `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="06289-174">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="06289-175">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="06289-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="06289-176">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="06289-176">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="06289-177">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="06289-177">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="06289-178">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="06289-178">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="06289-180">Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="06289-180">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="06289-182">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="06289-182">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="06289-183">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="06289-183">The browser image above doesn't show these links.</span></span> <span data-ttu-id="06289-184">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="06289-184">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="06289-186">Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="06289-186">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="06289-187">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="06289-187">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="06289-188">Next</span><span class="sxs-lookup"><span data-stu-id="06289-188">Next</span></span>](adding-controller.md)  
