---
title: Wprowadzenie do platformy ASP.NET Core MVC Mac, Linux lub Windows
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio Code, Mac, Linux i Windows
keywords: Platformy ASP.NET Core MVC, VS, kod programu Visual Studio, Mac, Linux, w systemie Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="586e7-104">Wprowadzenie do platformy ASP.NET MVC rdzeni na Mac, Linux lub Windows</span><span class="sxs-lookup"><span data-stu-id="586e7-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="586e7-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="586e7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="586e7-106">W tym samouczku udzieli Ci podstawy platformy ASP.NET Core MVC sieci web aplikacji za pomocą [Visual Studio Code](https://code.visualstudio.com) (VS kodu).</span><span class="sxs-lookup"><span data-stu-id="586e7-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="586e7-107">Samouczek jest przeznaczony dla familarity z kodem VS.</span><span class="sxs-lookup"><span data-stu-id="586e7-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="586e7-108">Zobacz [wprowadzenie kodu VS](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="586e7-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="586e7-109">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="586e7-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="586e7-110">System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="586e7-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="586e7-111">System Windows: [utworzyć platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="586e7-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="586e7-112">System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC z kodem Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="586e7-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="586e7-113">Zainstaluj kodzie VS i .NET Core</span><span class="sxs-lookup"><span data-stu-id="586e7-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="586e7-114">Ten samouczek wymaga [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="586e7-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="586e7-115">Zobacz [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) dla wersji platformy ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="586e7-115">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="586e7-116">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="586e7-116">Install the following:</span></span>

* <span data-ttu-id="586e7-117">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="586e7-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="586e7-118">Kod Visual Studio</span><span class="sxs-lookup"><span data-stu-id="586e7-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="586e7-119">Kod VS [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="586e7-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="586e7-120">Tworzenie aplikacji sieci web w usłudze dotnet</span><span class="sxs-lookup"><span data-stu-id="586e7-120">Create a web app with dotnet</span></span>

<span data-ttu-id="586e7-121">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="586e7-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="586e7-122">Otwórz *MvcMovie* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="586e7-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="586e7-123">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"MvcMovie".</span><span class="sxs-lookup"><span data-stu-id="586e7-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="586e7-124">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="586e7-124">Add them?"</span></span>
- <span data-ttu-id="586e7-125">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="586e7-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "MvcMovie".](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="586e7-129">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="586e7-129">Press **Debug** (F5) to build and run the program.</span></span>

![uruchomionej aplikacji](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="586e7-131">Uruchamia kod VS [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="586e7-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="586e7-132">Należy zauważyć, że na pasku adresu `localhost:5000` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="586e7-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="586e7-133">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="586e7-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="586e7-134">Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="586e7-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="586e7-135">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="586e7-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="586e7-136">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="586e7-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona nawigacji w prawym górnym rogu](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="586e7-138">W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="586e7-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="586e7-139">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="586e7-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="586e7-140">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="586e7-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="586e7-141">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="586e7-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="586e7-142">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="586e7-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="586e7-143">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="586e7-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="586e7-144">Skróty klawiaturowe Mac</span><span class="sxs-lookup"><span data-stu-id="586e7-144">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="586e7-145">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="586e7-145">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="586e7-146">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="586e7-146">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="586e7-147">Następne — Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="586e7-147">Next - Add a controller</span></span>](adding-controller.md)
