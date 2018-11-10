---
title: Wprowadzenie do ASP.NET Core MVC i programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio
ms.author: riande
ms.date: 8/23/2017
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 059ac1f7fa94d97adc958be3c0b936cdfa7f6d3e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225476"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="4138a-103">Wprowadzenie do ASP.NET Core MVC i programu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="4138a-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="4138a-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4138a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4138a-105">W tym samouczku pokazano podstawy tworzenia, ASP.NET Core MVC sieci web aplikacji za pomocą [programu Visual Studio dla komputerów Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="4138a-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="4138a-106">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="4138a-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="4138a-107">System macOS: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4138a-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="4138a-108">Windows: [kompilacji platformy ASP.NET Core MVC aplikacji za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4138a-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="4138a-109">Linux, macOS i Windows: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4138a-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4138a-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4138a-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="4138a-111">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="4138a-111">Create a web app</span></span>

<span data-ttu-id="4138a-112">W programie Visual Studio, wybierz **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="4138a-112">From Visual Studio, select **File > New Solution**.</span></span>

![Nowe rozwiązanie w systemie macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="4138a-114">Wybierz **aplikacji programu .NET Core > platformy ASP.NET Core > Aplikacja sieci Web platformy ASP.NET Core (MVC) > Dalej**.</span><span class="sxs-lookup"><span data-stu-id="4138a-114">Select **.NET Core App >  ASP.NET Core > ASP.NET Core Web App (MVC) > Next**.</span></span>

![okno dialogowe z systemem macOS nowego projektu](start-mvc/1.png)

<span data-ttu-id="4138a-116">Nadaj projektowi nazwę **MvcMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4138a-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![okno dialogowe z systemem macOS nowego projektu](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="4138a-118">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="4138a-118">Launch the app</span></span>

<span data-ttu-id="4138a-119">W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4138a-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="4138a-120">Uruchomieniu programu Visual Studio [Kestrel](xref:fundamentals/servers/index#kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:port`, gdzie *portu* jest numer portu wybranego losowo.</span><span class="sxs-lookup"><span data-stu-id="4138a-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Przeglądarki za pomocą nowego projektu](start-mvc/b1.png)

* <span data-ttu-id="4138a-122">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4138a-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4138a-123">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="4138a-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4138a-124">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="4138a-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4138a-125">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="4138a-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="4138a-126">Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **Uruchom** menu.</span><span class="sxs-lookup"><span data-stu-id="4138a-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="4138a-127">Domyślny szablon umożliwia **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="4138a-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="4138a-128">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="4138a-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="4138a-129">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="4138a-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Przeglądarki za pomocą nowego projektu](start-mvc/b2.png)

<span data-ttu-id="4138a-131">W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="4138a-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4138a-132">Next</span><span class="sxs-lookup"><span data-stu-id="4138a-132">Next</span></span>](adding-controller.md)  
