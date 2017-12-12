---
title: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio
keywords: Platformy ASP.NET Core MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 4b86eb22e367d2305b7995421aec6f37008c5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="09439-104">Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09439-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="09439-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="09439-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="09439-106">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="09439-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="09439-107">System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="09439-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="09439-108">System Windows: [utworzyć platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="09439-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="09439-109">System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC z kodem Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="09439-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="09439-110">Zainstaluj oprogramowanie .NET Core i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09439-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="09439-111">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="09439-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="09439-112">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="09439-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="09439-113">Zainstaluj program Visual Studio Community 2017 r.</span><span class="sxs-lookup"><span data-stu-id="09439-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="09439-114">Wybierz pobieranie społeczności.</span><span class="sxs-lookup"><span data-stu-id="09439-114">Select the Community download.</span></span> <span data-ttu-id="09439-115">Jeśli masz program Visual Studio 2017 r zainstalowany, Pomiń ten krok.</span><span class="sxs-lookup"><span data-stu-id="09439-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="09439-116">Visual Studio 2017 głównej strony Instalatora</span><span class="sxs-lookup"><span data-stu-id="09439-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="09439-117">Uruchom Instalatora, a następnie wybierz następujące obciążenia:</span><span class="sxs-lookup"><span data-stu-id="09439-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="09439-118">**ASP.NET i sieć web development** (w obszarze **sieci Web & chmurze**)</span><span class="sxs-lookup"><span data-stu-id="09439-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="09439-119">**Programowanie wieloplatformowych .NET core** (w obszarze **inne procesami**)</span><span class="sxs-lookup"><span data-stu-id="09439-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET i sieci web development ** (w obszarze ** & sieci Web chmury **)](start-mvc/_static/web_workload.png)

![* *.NET core cross-cross-platfrom Programowanie ** (w obszarze ** inne procesami **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="09439-122">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="09439-122">Create a web app</span></span>

<span data-ttu-id="09439-123">W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="09439-123">From Visual Studio, select  **File > New > Project**.</span></span>

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="09439-125">Zakończenie **nowy projekt** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="09439-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="09439-126">W okienku po lewej stronie wybierz **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="09439-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="09439-127">W środkowym okienku wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="09439-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="09439-128">Nazwa projektu "MvcMovie" (należy nazwa projektu "MvcMovie", po skopiowaniu kodu przestrzeni nazw będą zgodne.)</span><span class="sxs-lookup"><span data-stu-id="09439-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="09439-129">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="09439-129">Tap **OK**</span></span>

![<span data-ttu-id="09439-130">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09439-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="09439-131">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="09439-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="09439-132">Zakończenie **nowej platformy ASP.NET Core aplikacji sieci Web (.NET Core) — MvcMovie** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="09439-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="09439-133">W polu listy rozwijanej selektora wersja wybierz **platformy ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="09439-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="09439-134">Wybierz **Application(Model-View-Controller) w sieci Web**</span><span class="sxs-lookup"><span data-stu-id="09439-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="09439-135">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="09439-135">Tap **OK**.</span></span>

![<span data-ttu-id="09439-136">Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09439-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="09439-137">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="09439-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="09439-138">Zakończenie **nowej platformy ASP.NET Core aplikacji sieci Web (.NET Core) — MvcMovie** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="09439-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="09439-139">W naciśnij pola rozwijanego selektora wersji **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="09439-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="09439-140">Wybierz **aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="09439-140">Tap **Web Application**</span></span>
* <span data-ttu-id="09439-141">Zachowaj ustawienie domyślne **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="09439-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="09439-142">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="09439-142">Tap **OK**.</span></span>

![Nowa aplikacja sieci web platformy ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="09439-144">Visual Studio używany domyślny szablon projektu MVC, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="09439-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="09439-145">Masz aplikację pracy z teraz wpisując nazwę projektu i wybierając kilka opcji.</span><span class="sxs-lookup"><span data-stu-id="09439-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="09439-146">Jest to projekt starter proste i jest dobrym miejscem do rozpoczęcia,</span><span class="sxs-lookup"><span data-stu-id="09439-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="09439-147">Wybierz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="09439-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchomionej aplikacji](start-mvc/_static/1.png)

* <span data-ttu-id="09439-149">Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="09439-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="09439-150">Należy zauważyć, że na pasku adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="09439-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="09439-151">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="09439-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="09439-152">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="09439-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="09439-153">Powyższy obraz numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="09439-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="09439-154">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="09439-154">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="09439-155">Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="09439-155">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="09439-156">Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="09439-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="09439-157">Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **debugowania** element menu:</span><span class="sxs-lookup"><span data-stu-id="09439-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="09439-159">Można debugować aplikacji, naciskając pozycję **usług IIS Express** przycisku</span><span class="sxs-lookup"><span data-stu-id="09439-159">You can debug the app by tapping the **IIS Express** button</span></span>

![Usługi IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="09439-161">Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="09439-161">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="09439-162">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="09439-162">The browser image above doesn't show these links.</span></span> <span data-ttu-id="09439-163">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="09439-163">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona nawigacji w prawym górnym rogu](start-mvc/_static/2.png)

<span data-ttu-id="09439-165">Podczas uruchamiania w trybie debugowania, naciśnij przycisk **Shift-F5** można zatrzymać debugowania.</span><span class="sxs-lookup"><span data-stu-id="09439-165">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="09439-166">W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="09439-166">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="09439-167">Dalej</span><span class="sxs-lookup"><span data-stu-id="09439-167">Next</span></span>](adding-controller.md)  
