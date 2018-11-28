---
title: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać stron Razor w programie ASP.NET Core. Dowiedz się, jak utworzyć model, generowanie kodu dla stron Razor, platformy Entity Framework Core i SQL Server na użytek dostęp do danych, dodać funkcje wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracje do aktualizacji modelu.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: ca5b134aaa0a9218bc3b0466c3448bd41e8cef8b
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450739"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="e0a57-104">Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0a57-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e0a57-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e0a57-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e0a57-106">Zaleca się wykonać wersji platformy ASP.NET Core 2.1 w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="e0a57-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="e0a57-107">Ma ona **znacznie** łatwiejszej do obserwowania i obejmuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="e0a57-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="e0a57-108">Wybierz **platformy ASP.NET Core 2.1** w selektorze wersja.</span><span class="sxs-lookup"><span data-stu-id="e0a57-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Selektor wersji w spisie treści](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="e0a57-110">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="e0a57-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="e0a57-111">Strony razor jest zalecany sposób tworzenia interfejsu użytkownika dla aplikacji sieci web w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e0a57-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="e0a57-112">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="e0a57-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="e0a57-113">Windows: W tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="e0a57-113">Windows: This tutorial</span></span>
* <span data-ttu-id="e0a57-114">System MacOS: [Rozpoczynanie pracy ze stronami Razor z programem Visual Studio dla komputerów Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="e0a57-114">macOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="e0a57-115">System macOS, Linux i Windows: [Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="e0a57-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="e0a57-116">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e0a57-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

> [!NOTE]
> <span data-ttu-id="e0a57-117">W tej chwili testujemy użyteczność nowo zaproponowanej struktury spisu treści dla dokumentacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e0a57-117">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="e0a57-118">Jeśli możesz poświęcić chwilę na wykonanie ćwiczenia polegającego na znalezieniu 7 różnych tematów w aktualnym lub zaproponowanym spisie treści, [kliknij tutaj i weź udział w badaniu](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="e0a57-118">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="e0a57-119">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e0a57-119">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e0a57-120">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="e0a57-120">Create a Razor web app</span></span>

* <span data-ttu-id="e0a57-121">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e0a57-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e0a57-122">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e0a57-122">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e0a57-123">Nadaj projektowi nazwę **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e0a57-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e0a57-124">Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne, jeśli kopiujesz/wklejasz kod.</span><span class="sxs-lookup"><span data-stu-id="e0a57-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="e0a57-125">![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="e0a57-125">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="e0a57-126">Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e0a57-126">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="e0a57-128">Szablon programu Visual Studio tworzy projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="e0a57-128">The Visual Studio template creates a starter project:</span></span>

![Eksplorator rozwiązań](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="e0a57-130">Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** do uruchomienia bez dołączania debugera.</span><span class="sxs-lookup"><span data-stu-id="e0a57-130">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="e0a57-131">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="e0a57-131">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e0a57-132">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="e0a57-132">This app doesn't track personal information.</span></span> <span data-ttu-id="e0a57-133">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e0a57-133">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Strona główna lub indeks](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="e0a57-135">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="e0a57-135">The following image shows the app after accepting tracking:</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="e0a57-137">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e0a57-137">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e0a57-138">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e0a57-138">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e0a57-139">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e0a57-139">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e0a57-140">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e0a57-140">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e0a57-141">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="e0a57-141">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e0a57-142">Na wcześniejszej ilustracji numer portu to 5001.</span><span class="sxs-lookup"><span data-stu-id="e0a57-142">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="e0a57-143">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="e0a57-143">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e0a57-144">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="e0a57-144">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e0a57-145">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="e0a57-145">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="e0a57-146">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e0a57-146">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e0a57-147">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="e0a57-147">Create a Razor web app</span></span>

* <span data-ttu-id="e0a57-148">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e0a57-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e0a57-149">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e0a57-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e0a57-150">Nadaj projektowi nazwę **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e0a57-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e0a57-151">Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne, jeśli kopiujesz/wklejasz kod.</span><span class="sxs-lookup"><span data-stu-id="e0a57-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="e0a57-152">![Nowa aplikacja internetowa ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="e0a57-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="e0a57-153">Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e0a57-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="e0a57-154">Szablon programu Visual Studio tworzy projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="e0a57-154">The Visual Studio template creates a starter project:</span></span>

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

<span data-ttu-id="e0a57-156">Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** do uruchomienia bez dołączania debugera</span><span class="sxs-lookup"><span data-stu-id="e0a57-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* <span data-ttu-id="e0a57-158">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="e0a57-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e0a57-159">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e0a57-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e0a57-160">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e0a57-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e0a57-161">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e0a57-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e0a57-162">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="e0a57-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e0a57-163">Na wcześniejszej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="e0a57-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="e0a57-164">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="e0a57-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e0a57-165">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="e0a57-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e0a57-166">Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="e0a57-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="e0a57-167">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="e0a57-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
