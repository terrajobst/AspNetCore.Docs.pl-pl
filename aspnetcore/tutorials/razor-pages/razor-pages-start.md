---
title: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core
author: rick-anderson
description: Poznaj podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony. Strony razor jest zalecane dla obciążeń internetowych w programie ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: bc18ec3ad3bb7e3afe38030a34b2e748ce9e341b
ms.sourcegitcommit: 74c09caec8992635825b45b7f065f871d33c077a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42634980"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f14a0-104">Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f14a0-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f14a0-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f14a0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f14a0-106">Zaleca się wykonać wersji platformy ASP.NET Core 2.1 w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="f14a0-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="f14a0-107">Ma ona **znacznie** łatwiejszej do obserwowania i obejmuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="f14a0-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="f14a0-108">Wybierz **platformy ASP.NET Core 2.1** w selektorze wersja.</span><span class="sxs-lookup"><span data-stu-id="f14a0-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Selektor wersji w spisie treści](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="f14a0-110">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="f14a0-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="f14a0-111">Strony razor jest zalecany sposób tworzenia interfejsu użytkownika dla aplikacji sieci web w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f14a0-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="f14a0-112">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="f14a0-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="f14a0-113">Windows: W tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="f14a0-113">Windows: This tutorial</span></span>
* <span data-ttu-id="f14a0-114">System MacOS: [Rozpoczynanie pracy ze stronami Razor z programem Visual Studio dla komputerów Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="f14a0-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="f14a0-115">System macOS, Linux i Windows: [Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="f14a0-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="f14a0-116">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f14a0-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="f14a0-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f14a0-117">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f14a0-118">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="f14a0-118">Create a Razor web app</span></span>

* <span data-ttu-id="f14a0-119">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f14a0-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f14a0-120">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f14a0-120">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f14a0-121">Nadaj projektowi nazwę **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f14a0-121">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f14a0-122">Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne, jeśli kopiujesz/wklejasz kod.</span><span class="sxs-lookup"><span data-stu-id="f14a0-122">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="f14a0-123">![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="f14a0-123">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="f14a0-124">Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="f14a0-124">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="f14a0-126">Szablon programu Visual Studio tworzy projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="f14a0-126">The Visual Studio template creates a starter project:</span></span>

![Eksplorator rozwiązań](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="f14a0-128">Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** do uruchomienia bez dołączania debugera.</span><span class="sxs-lookup"><span data-stu-id="f14a0-128">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="f14a0-129">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="f14a0-129">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="f14a0-130">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="f14a0-130">This app doesn't track personal information.</span></span> <span data-ttu-id="f14a0-131">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="f14a0-131">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Strona główna lub indeks](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="f14a0-133">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="f14a0-133">The following image shows the app after accepting tracking:</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="f14a0-135">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f14a0-135">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="f14a0-136">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f14a0-136">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f14a0-137">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="f14a0-137">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f14a0-138">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="f14a0-138">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f14a0-139">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="f14a0-139">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f14a0-140">Na wcześniejszej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="f14a0-140">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="f14a0-141">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="f14a0-141">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f14a0-142">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="f14a0-142">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f14a0-143">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="f14a0-143">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="f14a0-144">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f14a0-144">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f14a0-145">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="f14a0-145">Create a Razor web app</span></span>

* <span data-ttu-id="f14a0-146">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f14a0-146">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f14a0-147">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f14a0-147">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f14a0-148">Nadaj projektowi nazwę **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f14a0-148">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f14a0-149">Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne, jeśli kopiujesz/wklejasz kod.</span><span class="sxs-lookup"><span data-stu-id="f14a0-149">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="f14a0-150">![Nowa aplikacja internetowa ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="f14a0-150">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="f14a0-151">Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="f14a0-151">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="f14a0-152">Szablon programu Visual Studio tworzy projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="f14a0-152">The Visual Studio template creates a starter project:</span></span>

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

<span data-ttu-id="f14a0-154">Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** do uruchomienia bez dołączania debugera</span><span class="sxs-lookup"><span data-stu-id="f14a0-154">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* <span data-ttu-id="f14a0-156">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="f14a0-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="f14a0-157">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f14a0-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f14a0-158">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="f14a0-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f14a0-159">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="f14a0-159">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f14a0-160">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="f14a0-160">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f14a0-161">Na wcześniejszej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="f14a0-161">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="f14a0-162">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="f14a0-162">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f14a0-163">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="f14a0-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f14a0-164">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="f14a0-164">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="f14a0-165">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="f14a0-165">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
