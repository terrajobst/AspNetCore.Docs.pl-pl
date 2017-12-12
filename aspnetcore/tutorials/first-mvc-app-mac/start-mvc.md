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
ms.openlocfilehash: 21f115eec924d5e4b21ad78398c8cbd99e02a0a8
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="131a7-104">Wprowadzenie do korzystania z platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="131a7-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="131a7-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="131a7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="131a7-106">W tym samouczku uczy podstaw konstruowania platformy ASP.NET Core MVC aplikację sieciową przy użyciu [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="131a7-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="131a7-107">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="131a7-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="131a7-108">System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="131a7-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="131a7-109">System Windows: [kompilacji platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="131a7-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="131a7-110">System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="131a7-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="131a7-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="131a7-111">Prerequisites</span></span>

<span data-ttu-id="131a7-112">Ten samouczek wymaga [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="131a7-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="131a7-113">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="131a7-113">Install the following:</span></span>

- <span data-ttu-id="131a7-114">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="131a7-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="131a7-115">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="131a7-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="131a7-116">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="131a7-116">Create a web app</span></span>

<span data-ttu-id="131a7-117">W programie Visual Studio, wybierz **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="131a7-117">From Visual Studio, select **File > New Solution**.</span></span>

![System macOS nowego rozwiązania](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="131a7-119">Wybierz **aplikacji .NET Core > platformy ASP.NET Core > sieci Web aplikacji > Dalej**.</span><span class="sxs-lookup"><span data-stu-id="131a7-119">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![okno dialogowe macOS nowego projektu](start-mvc/1.png)

<span data-ttu-id="131a7-121">Nazwij projekt **MvcMovie**, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="131a7-121">Name the project **MvcMovie**, and then select **Create**.</span></span>

![okno dialogowe macOS nowego projektu](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="131a7-123">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="131a7-123">Launch the app</span></span>

<span data-ttu-id="131a7-124">W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="131a7-124">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="131a7-125">Uruchamia program Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="131a7-125">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Przeglądarki za pomocą nowego projektu](start-mvc/b1.png)

* <span data-ttu-id="131a7-127">Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="131a7-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="131a7-128">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="131a7-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="131a7-129">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="131a7-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="131a7-130">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="131a7-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="131a7-131">Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **Uruchom** menu.</span><span class="sxs-lookup"><span data-stu-id="131a7-131">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="131a7-132">Domyślny szablon umożliwia **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="131a7-132">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="131a7-133">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="131a7-133">The browser image above doesn't show these links.</span></span> <span data-ttu-id="131a7-134">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="131a7-134">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Przeglądarki za pomocą nowego projektu](start-mvc/b2.png)

<span data-ttu-id="131a7-136">W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="131a7-136">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="131a7-137">Dalej</span><span class="sxs-lookup"><span data-stu-id="131a7-137">Next</span></span>](adding-controller.md)  
