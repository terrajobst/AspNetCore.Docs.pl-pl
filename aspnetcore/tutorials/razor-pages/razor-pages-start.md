---
title: Wprowadzenie do platformy ASP.NET Core stron Razor
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core stron Razor
keywords: Platformy ASP.NET Core stron Razor, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 5c58b5156f62572687755c9c0878db10c3c14eb1
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="3aa44-104">Wprowadzenie do platformy ASP.NET Core stron Razor</span><span class="sxs-lookup"><span data-stu-id="3aa44-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3aa44-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3aa44-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3aa44-106">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="3aa44-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="3aa44-107">Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="3aa44-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="3aa44-108">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3aa44-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

<span data-ttu-id="3aa44-109">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="3aa44-109">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="3aa44-110">Systemu Windows: W tym samouczku</span><span class="sxs-lookup"><span data-stu-id="3aa44-110">Windows: This tutorial</span></span>
* <span data-ttu-id="3aa44-111">System MacOS: [wprowadzenie stron Razor przy użyciu programu Visual Studio dla komputerów Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="3aa44-111">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="3aa44-112">System macOS, Linux i Windows: [wprowadzenie Razor strony platformy ASP.NET Core z kodem Visual Studio](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="3aa44-112">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3aa44-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3aa44-113">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="3aa44-114">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="3aa44-114">Create a Razor web app</span></span>

* <span data-ttu-id="3aa44-115">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="3aa44-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="3aa44-116">Utwórz nową aplikację sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3aa44-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="3aa44-117">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="3aa44-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="3aa44-118">Ważne jest, aby nazwa projektu *RazorPagesMovie* , przestrzenie nazw będzie dopasowania, gdy użytkownik kopiowanie/wklejanie kodu.</span><span class="sxs-lookup"><span data-stu-id="3aa44-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="3aa44-119">![nową aplikację sieci Web Core ASP.NET](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="3aa44-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="3aa44-120">Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="3aa44-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
  <span data-ttu-id="3aa44-121">![Aplikacja sieci Web (Razor strony)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="3aa44-121">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="3aa44-122">Szablon programu Visual Studio tworzy projekt starter:</span><span class="sxs-lookup"><span data-stu-id="3aa44-122">The Visual Studio template creates a starter project:</span></span>

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

<span data-ttu-id="3aa44-124">Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera</span><span class="sxs-lookup"><span data-stu-id="3aa44-124">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* <span data-ttu-id="3aa44-126">Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="3aa44-126">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="3aa44-127">Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="3aa44-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="3aa44-128">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="3aa44-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="3aa44-129">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="3aa44-129">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="3aa44-130">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="3aa44-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="3aa44-131">Na poprzedniej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="3aa44-131">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="3aa44-132">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="3aa44-132">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="3aa44-133">Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="3aa44-133">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="3aa44-134">Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="3aa44-134">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="3aa44-135">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="3aa44-135">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
