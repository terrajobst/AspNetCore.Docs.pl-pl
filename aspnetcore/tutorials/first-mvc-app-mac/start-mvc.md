---
title: "Wprowadzenie do korzystania z platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac"
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio
keywords: "Visual Studio platformy ASP.NET Core MVC, dla komputerów Mac programu Entity Framework"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 028d6d3246908a9cd44a6834449d2fdbc9cae0b8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="a2691-104">Wprowadzenie do korzystania z platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a2691-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="a2691-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a2691-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a2691-106">W tym samouczku uczy podstaw konstruowania platformy ASP.NET Core MVC aplikację sieciową przy użyciu [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="a2691-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="a2691-107">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="a2691-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a2691-108">System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a2691-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="a2691-109">System Windows: [kompilacji platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a2691-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="a2691-110">System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a2691-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a2691-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a2691-111">Prerequisites</span></span>

<span data-ttu-id="a2691-112">Ten samouczek wymaga [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="a2691-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="a2691-113">Zobacz [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) dla wersji platformy ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="a2691-113">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="a2691-114">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="a2691-114">Install the following:</span></span>

- <span data-ttu-id="a2691-115">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a2691-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="a2691-116">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a2691-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="a2691-117">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="a2691-117">Create a web app</span></span>

<span data-ttu-id="a2691-118">W programie Visual Studio, wybierz **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="a2691-118">From Visual Studio, select **File > New Solution**.</span></span>

![System macOS nowego rozwiązania](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="a2691-120">Wybierz **aplikacji .NET Core > platformy ASP.NET Core > sieci Web aplikacji > Dalej**.</span><span class="sxs-lookup"><span data-stu-id="a2691-120">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![okno dialogowe macOS nowego projektu](start-mvc/1.png)

<span data-ttu-id="a2691-122">Nazwij projekt **MvcMovie**, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a2691-122">Name the project **MvcMovie**, and then select **Create**.</span></span>

![okno dialogowe macOS nowego projektu](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="a2691-124">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="a2691-124">Launch the app</span></span>

<span data-ttu-id="a2691-125">W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a2691-125">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="a2691-126">Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="a2691-126">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Przeglądarki za pomocą nowego projektu](start-mvc/b1.png)

* <span data-ttu-id="a2691-128">Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a2691-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a2691-129">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="a2691-129">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a2691-130">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="a2691-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a2691-131">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="a2691-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a2691-132">Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **Uruchom** menu.</span><span class="sxs-lookup"><span data-stu-id="a2691-132">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="a2691-133">Domyślny szablon umożliwia **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="a2691-133">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="a2691-134">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="a2691-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="a2691-135">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="a2691-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Przeglądarki za pomocą nowego projektu](start-mvc/b2.png)

<span data-ttu-id="a2691-137">W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="a2691-137">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="a2691-138">Dalej</span><span class="sxs-lookup"><span data-stu-id="a2691-138">Next</span></span>](adding-controller.md)  
