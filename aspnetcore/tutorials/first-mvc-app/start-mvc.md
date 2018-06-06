---
title: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformy ASP.NET Core MVC i Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 10/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 3272700c7739778a6a341ae8ee424fd69605ca53
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729720"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="f004b-103">Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f004b-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="f004b-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f004b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="f004b-105">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="f004b-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f004b-106">System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f004b-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="f004b-107">System Windows: [utworzyć platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f004b-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="f004b-108">System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC z kodem Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f004b-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="f004b-109">Zainstaluj oprogramowanie .NET Core i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f004b-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f004b-110">[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="f004b-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="f004b-111">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="f004b-111">Create a web app</span></span>

<span data-ttu-id="f004b-112">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f004b-112">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="f004b-114">Zakończenie **nowy projekt** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="f004b-114">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="f004b-115">W okienku po lewej stronie wybierz **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="f004b-115">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="f004b-116">W środkowym okienku wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="f004b-116">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="f004b-117">Nazwa projektu "MvcMovie" (należy nazwa projektu "MvcMovie", po skopiowaniu kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="f004b-117">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="f004b-118">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="f004b-118">Tap **OK**</span></span>

![<span data-ttu-id="f004b-119">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f004b-119">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="f004b-120">Zakończenie **nowej platformy ASP.NET Core aplikacji sieci Web (.NET Core) — MvcMovie** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="f004b-120">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="f004b-121">W polu listy rozwijanej selektora wersja wybierz **platformy ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="f004b-121">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="f004b-122">Wybierz **Application(Model-View-Controller) w sieci Web**</span><span class="sxs-lookup"><span data-stu-id="f004b-122">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="f004b-123">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="f004b-123">Tap **OK**.</span></span>

![<span data-ttu-id="f004b-124">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f004b-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="f004b-125">Visual Studio używany domyślny szablon projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f004b-125">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="f004b-126">Masz aplikację pracy z teraz wpisując nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="f004b-126">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="f004b-127">Jest to projekt starter podstawowe i jest dobrym miejscem do rozpoczęcia,</span><span class="sxs-lookup"><span data-stu-id="f004b-127">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="f004b-128">Wybierz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="f004b-128">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchomionej aplikacji](start-mvc/_static/1.png)

* <span data-ttu-id="f004b-130">Uruchamia program Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="f004b-130">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="f004b-131">Należy zauważyć, że na pasku adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f004b-131">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f004b-132">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="f004b-132">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f004b-133">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="f004b-133">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f004b-134">Powyższy obraz numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="f004b-134">In the image above, the port number is 5000.</span></span> <span data-ttu-id="f004b-135">Adres URL w przeglądarce przedstawiono `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f004b-135">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="f004b-136">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="f004b-136">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f004b-137">Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="f004b-137">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f004b-138">Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="f004b-138">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="f004b-139">Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="f004b-139">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="f004b-141">Można debugować aplikacji, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="f004b-141">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="f004b-143">Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="f004b-143">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="f004b-144">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="f004b-144">The browser image above doesn't show these links.</span></span> <span data-ttu-id="f004b-145">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="f004b-145">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona nawigacji w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="f004b-147">Podczas uruchamiania w trybie debugowania, naciśnij przycisk **Shift-F5** można zatrzymać debugowania.</span><span class="sxs-lookup"><span data-stu-id="f004b-147">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="f004b-148">W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="f004b-148">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f004b-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f004b-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f004b-150">[! OBEJMUJĄ [] (~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="f004b-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f004b-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f004b-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f004b-152">Install Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="f004b-152">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="f004b-153">Wybierz pobieranie społeczności.</span><span class="sxs-lookup"><span data-stu-id="f004b-153">Select the Community download.</span></span> <span data-ttu-id="f004b-154">Jeśli masz program Visual Studio 2017 r zainstalowany, Pomiń ten krok.</span><span class="sxs-lookup"><span data-stu-id="f004b-154">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="f004b-155">Visual Studio 2017 głównej strony Instalatora</span><span class="sxs-lookup"><span data-stu-id="f004b-155">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="f004b-156">Uruchom Instalatora, a następnie wybierz następujące obciążenia:</span><span class="sxs-lookup"><span data-stu-id="f004b-156">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="f004b-157">**ASP.NET i sieć web development** (w obszarze **sieci Web & chmurze**)</span><span class="sxs-lookup"><span data-stu-id="f004b-157">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="f004b-158">**Programowanie wieloplatformowych .NET core** (w obszarze **inne procesami**)</span><span class="sxs-lookup"><span data-stu-id="f004b-158">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET i sieci web development ** (w obszarze ** & sieci Web chmury **)](start-mvc/_static/web_workload.png)

![* *.NET core cross-cross-platfrom Programowanie ** (w obszarze ** inne procesami **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="f004b-161">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="f004b-161">Create a web app</span></span>

<span data-ttu-id="f004b-162">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f004b-162">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="f004b-164">Zakończenie **nowy projekt** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="f004b-164">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="f004b-165">W okienku po lewej stronie wybierz **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="f004b-165">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="f004b-166">W środkowym okienku wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="f004b-166">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="f004b-167">Nazwa projektu "MvcMovie" (należy nazwa projektu "MvcMovie", po skopiowaniu kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="f004b-167">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="f004b-168">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="f004b-168">Tap **OK**</span></span>

![<span data-ttu-id="f004b-169">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f004b-169">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f004b-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f004b-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f004b-171">Zakończenie **nowej platformy ASP.NET Core aplikacji sieci Web (.NET Core) — MvcMovie** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="f004b-171">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="f004b-172">W polu listy rozwijanej selektora wersja wybierz **platformy ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="f004b-172">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="f004b-173">Wybierz **Application(Model-View-Controller) w sieci Web**</span><span class="sxs-lookup"><span data-stu-id="f004b-173">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="f004b-174">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="f004b-174">Tap **OK**.</span></span>

![<span data-ttu-id="f004b-175">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f004b-175">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f004b-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f004b-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f004b-177">Zakończenie **nowej platformy ASP.NET Core aplikacji sieci Web (.NET Core) — MvcMovie** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="f004b-177">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="f004b-178">W naciśnij pola rozwijanego selektora wersji **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="f004b-178">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="f004b-179">Wybierz **aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="f004b-179">Tap **Web Application**</span></span>
* <span data-ttu-id="f004b-180">Zachowaj ustawienie domyślne **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="f004b-180">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="f004b-181">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="f004b-181">Tap **OK**.</span></span>

![Nowa aplikacja sieci web platformy ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="f004b-183">Visual Studio używany domyślny szablon projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f004b-183">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="f004b-184">Masz aplikację pracy z teraz wpisując nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="f004b-184">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="f004b-185">Jest to projekt starter podstawowe i jest dobrym miejscem do rozpoczęcia,</span><span class="sxs-lookup"><span data-stu-id="f004b-185">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="f004b-186">Wybierz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="f004b-186">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchomionej aplikacji](start-mvc/_static/1.png)

* <span data-ttu-id="f004b-188">Uruchamia program Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="f004b-188">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="f004b-189">Należy zauważyć, że na pasku adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f004b-189">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f004b-190">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="f004b-190">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f004b-191">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="f004b-191">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f004b-192">Powyższy obraz numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="f004b-192">In the image above, the port number is 5000.</span></span> <span data-ttu-id="f004b-193">Adres URL w przeglądarce przedstawiono `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f004b-193">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="f004b-194">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="f004b-194">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f004b-195">Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="f004b-195">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f004b-196">Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="f004b-196">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="f004b-197">Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="f004b-197">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="f004b-199">Można debugować aplikacji, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="f004b-199">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="f004b-201">Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="f004b-201">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="f004b-202">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="f004b-202">The browser image above doesn't show these links.</span></span> <span data-ttu-id="f004b-203">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="f004b-203">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona nawigacji w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="f004b-205">Podczas uruchamiania w trybie debugowania, naciśnij przycisk **Shift-F5** można zatrzymać debugowania.</span><span class="sxs-lookup"><span data-stu-id="f004b-205">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="f004b-206">W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="f004b-206">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end
> [!div class="step-by-step"]
> [<span data-ttu-id="f004b-207">Next</span><span class="sxs-lookup"><span data-stu-id="f004b-207">Next</span></span>](adding-controller.md)  
