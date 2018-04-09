---
title: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformy ASP.NET Core MVC i Visual Studio
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: ffa620f07251c52c785672d8fbeefacac31ed4c1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="27db6-103">Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="27db6-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="27db6-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="27db6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="27db6-105">W tym samouczku uczy podstaw konstruowania platformy ASP.NET Core MVC aplikację sieciową przy użyciu [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="27db6-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="27db6-106">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="27db6-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="27db6-107">System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="27db6-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="27db6-108">System Windows: [kompilacji platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="27db6-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="27db6-109">System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="27db6-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27db6-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="27db6-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="27db6-111">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="27db6-111">Create a web app</span></span>

<span data-ttu-id="27db6-112">W programie Visual Studio, wybierz **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="27db6-112">From Visual Studio, select **File > New Solution**.</span></span>

![System macOS nowego rozwiązania](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="27db6-114">Wybierz **aplikacji .NET Core > platformy ASP.NET Core > sieci Web aplikacji > Dalej**.</span><span class="sxs-lookup"><span data-stu-id="27db6-114">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![okno dialogowe macOS nowego projektu](start-mvc/1.png)

<span data-ttu-id="27db6-116">Nazwij projekt **MvcMovie**, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="27db6-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![okno dialogowe macOS nowego projektu](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="27db6-118">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="27db6-118">Launch the app</span></span>

<span data-ttu-id="27db6-119">W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27db6-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="27db6-120">Uruchamia program Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="27db6-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Przeglądarki za pomocą nowego projektu](start-mvc/b1.png)

* <span data-ttu-id="27db6-122">Pokazuje pasek adresu `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="27db6-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="27db6-123">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="27db6-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="27db6-124">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="27db6-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="27db6-125">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="27db6-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="27db6-126">Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **Uruchom** menu.</span><span class="sxs-lookup"><span data-stu-id="27db6-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="27db6-127">Domyślny szablon umożliwia **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="27db6-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="27db6-128">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="27db6-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="27db6-129">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="27db6-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Przeglądarki za pomocą nowego projektu](start-mvc/b2.png)

<span data-ttu-id="27db6-131">W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="27db6-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="27db6-132">Next</span><span class="sxs-lookup"><span data-stu-id="27db6-132">Next</span></span>](adding-controller.md)  
