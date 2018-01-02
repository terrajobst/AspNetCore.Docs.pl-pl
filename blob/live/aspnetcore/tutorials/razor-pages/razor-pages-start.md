---
title: Wprowadzenie do platformy ASP.NET Core stron Razor
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core stron Razor
keywords: Platformy ASP.NET Core stron Razor, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 12685d6a391c127d78399408291c26caa0192ee2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="3ad47-104">Rozpoczynanie pracy z Razor strony platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ad47-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3ad47-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3ad47-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3ad47-106">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="3ad47-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="3ad47-107">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ad47-107">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="3ad47-108">Istnieją trzy wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="3ad47-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="3ad47-109">Systemu Windows: W tym samouczku</span><span class="sxs-lookup"><span data-stu-id="3ad47-109">Windows: This tutorial</span></span>
* <span data-ttu-id="3ad47-110">System MacOS: [wprowadzenie stron Razor przy użyciu programu Visual Studio dla komputerów Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="3ad47-110">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="3ad47-111">System macOS, Linux i Windows: [wprowadzenie Razor strony platformy ASP.NET Core z kodem Visual Studio](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="3ad47-111">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="3ad47-112">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3ad47-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ad47-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3ad47-113">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="3ad47-114">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="3ad47-114">Create a Razor web app</span></span>

* <span data-ttu-id="3ad47-115">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="3ad47-116">Utwórz nową aplikację sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ad47-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="3ad47-117">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="3ad47-118">Ważne jest, aby nazwa projektu *RazorPagesMovie* , przestrzenie nazw będzie dopasowania, gdy użytkownik kopiowanie/wklejanie kodu.</span><span class="sxs-lookup"><span data-stu-id="3ad47-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="3ad47-119">![nową aplikację sieci Web Core ASP.NET](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="3ad47-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="3ad47-120">Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

> [!NOTE]
> <span data-ttu-id="3ad47-121">Jeśli chcesz użyć platformy ASP.NET Core w programie .NET Framework, musisz najpierw wybrać **.NET Framework** z pierwszej listy rozwijanej w oknie dialogowym, można wybrać odpowiednią wersję platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ad47-121">If you want to use ASP.NET Core on the .NET Framework, you must first select **.NET Framework** from the leftmost dropdown in the dialog, then you can select the desired ASP.NET Core version.</span></span>

  ![Aplikacja sieci Web (Razor strony)](razor-pages-start/_static/np2.png)

<span data-ttu-id="3ad47-123">Szablon programu Visual Studio tworzy projekt starter:</span><span class="sxs-lookup"><span data-stu-id="3ad47-123">The Visual Studio template creates a starter project:</span></span>

![Eksplorator rozwiązań](razor-pages-start/_static/se.png)

<span data-ttu-id="3ad47-125">Naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** działać bez dołączanie debugera</span><span class="sxs-lookup"><span data-stu-id="3ad47-125">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home.png)

* <span data-ttu-id="3ad47-127">Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="3ad47-127">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="3ad47-128">Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="3ad47-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="3ad47-129">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="3ad47-129">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="3ad47-130">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="3ad47-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="3ad47-131">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="3ad47-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="3ad47-132">Na poprzedniej ilustracji numer portu to 5000.</span><span class="sxs-lookup"><span data-stu-id="3ad47-132">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="3ad47-133">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="3ad47-133">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="3ad47-134">Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="3ad47-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="3ad47-135">Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="3ad47-135">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="3ad47-136">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="3ad47-136">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
