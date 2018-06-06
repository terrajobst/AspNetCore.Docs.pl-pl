---
title: Rozpoczynanie pracy z Razor strony platformy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Stron razor jest zalecane w przypadku obciążeń sieci web w ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d7cdf7c8fac3b2ac1e526c6eeee8205068964ec9
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582820"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="e6b3b-104">Rozpoczynanie pracy z Razor strony platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6b3b-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e6b3b-105">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e6b3b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e6b3b-106">Zaleca się wykonanie wersji platformy ASP.NET Core 2.1 tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="e6b3b-107">Ma ona **znacznie** łatwiej wykonaj i obejmuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="e6b3b-108">Wybierz **platformy ASP.NET Core 2.1** w selektorze wersji.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Selektor wersji w spisie treści](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="e6b3b-110">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="e6b3b-111">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="e6b3b-112">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="e6b3b-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="e6b3b-113">Systemu Windows: W tym samouczku</span><span class="sxs-lookup"><span data-stu-id="e6b3b-113">Windows: This tutorial</span></span>
* <span data-ttu-id="e6b3b-114">System MacOS: [wprowadzenie stron Razor przy użyciu programu Visual Studio dla komputerów Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="e6b3b-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="e6b3b-115">System macOS, Linux i Windows: [wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="e6b3b-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="e6b3b-116">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e6b3b-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="e6b3b-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e6b3b-117">Prerequisites</span></span>

<span data-ttu-id="e6b3b-118">[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="e6b3b-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e6b3b-119">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="e6b3b-119">Create a Razor web app</span></span>

* <span data-ttu-id="e6b3b-120">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e6b3b-121">Utwórz nową aplikację sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e6b3b-122">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e6b3b-123">Ważne jest, aby nazwa projektu *RazorPagesMovie* , przestrzenie nazw będzie dopasowania, gdy użytkownik kopiowanie/wklejanie kodu.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="e6b3b-124">![nową aplikację sieci Web Core ASP.NET](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="e6b3b-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="e6b3b-125">Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![nową aplikację sieci Web Core ASP.NET](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="e6b3b-127">Szablon programu Visual Studio tworzy projekt starter:</span><span class="sxs-lookup"><span data-stu-id="e6b3b-127">The Visual Studio template creates a starter project:</span></span>

![Eksplorator rozwiązań](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="e6b3b-129">Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="e6b3b-130">Wybierz **Akceptuj** o zgodę do śledzenia.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e6b3b-131">Ta aplikacja nie śledzi informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-131">This app doesn't track personal information.</span></span> <span data-ttu-id="e6b3b-132">Kod szablonu wygenerowanego zawiera zasoby, które ułatwiają korzystanie z [rozporządzenia ogólne ochrony danych (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e6b3b-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Strona główna lub indeks](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="e6b3b-134">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="e6b3b-134">The following image shows the app after accepting tracking:</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="e6b3b-136">Uruchamia program Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e6b3b-137">Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e6b3b-138">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e6b3b-139">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e6b3b-140">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e6b3b-141">Na poprzedniej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="e6b3b-142">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e6b3b-143">Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e6b3b-144">Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="e6b3b-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e6b3b-145">Prerequisites</span></span>

<span data-ttu-id="e6b3b-146">[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="e6b3b-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e6b3b-147">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="e6b3b-147">Create a Razor web app</span></span>

* <span data-ttu-id="e6b3b-148">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e6b3b-149">Utwórz nową aplikację sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e6b3b-150">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e6b3b-151">Ważne jest, aby nazwa projektu *RazorPagesMovie* , przestrzenie nazw będzie dopasowania, gdy użytkownik kopiowanie/wklejanie kodu.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="e6b3b-152">![nową aplikację sieci Web Core ASP.NET](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="e6b3b-152">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="e6b3b-153">Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="e6b3b-154">Szablon programu Visual Studio tworzy projekt starter:</span><span class="sxs-lookup"><span data-stu-id="e6b3b-154">The Visual Studio template creates a starter project:</span></span>

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

<span data-ttu-id="e6b3b-156">Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera</span><span class="sxs-lookup"><span data-stu-id="e6b3b-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* <span data-ttu-id="e6b3b-158">Uruchamia program Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e6b3b-159">Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e6b3b-160">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e6b3b-161">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e6b3b-162">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e6b3b-163">Na poprzedniej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="e6b3b-164">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e6b3b-165">Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e6b3b-166">Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="e6b3b-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="e6b3b-167">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="e6b3b-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
