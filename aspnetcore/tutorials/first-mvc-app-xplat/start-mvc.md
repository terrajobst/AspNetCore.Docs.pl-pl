---
title: Wprowadzenie do platformy ASP.NET Core MVC Mac, Linux lub Windows
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio Code, Mac, Linux i Windows
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4771555b66f328a819f17a32eb3959f9ecf33d44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="0ba22-103">Wprowadzenie do platformy ASP.NET MVC rdzeni na Mac, Linux lub Windows</span><span class="sxs-lookup"><span data-stu-id="0ba22-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="0ba22-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ba22-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0ba22-105">W tym samouczku udzieli Ci podstawy platformy ASP.NET Core MVC sieci web aplikacji za pomocą [Visual Studio Code](https://code.visualstudio.com) (VS kodu).</span><span class="sxs-lookup"><span data-stu-id="0ba22-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="0ba22-106">Samouczek jest przeznaczony dla familarity z kodem VS.</span><span class="sxs-lookup"><span data-stu-id="0ba22-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0ba22-107">Zobacz [wprowadzenie kodu VS](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="0ba22-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="0ba22-108">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="0ba22-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="0ba22-109">System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="0ba22-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="0ba22-110">System Windows: [utworzyć platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="0ba22-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="0ba22-111">System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC z kodem Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="0ba22-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="0ba22-112">Zainstaluj kodzie VS i .NET Core</span><span class="sxs-lookup"><span data-stu-id="0ba22-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="0ba22-113">Ten samouczek wymaga [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="0ba22-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="0ba22-114">Zobacz [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) dla wersji platformy ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="0ba22-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="0ba22-115">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0ba22-115">Install the following:</span></span>

* <span data-ttu-id="0ba22-116">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="0ba22-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="0ba22-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ba22-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="0ba22-118">Kod VS [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="0ba22-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="0ba22-119">Tworzenie aplikacji sieci web w usłudze dotnet</span><span class="sxs-lookup"><span data-stu-id="0ba22-119">Create a web app with dotnet</span></span>

<span data-ttu-id="0ba22-120">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="0ba22-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="0ba22-121">Otwórz *MvcMovie* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="0ba22-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="0ba22-122">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"MvcMovie".</span><span class="sxs-lookup"><span data-stu-id="0ba22-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="0ba22-123">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="0ba22-123">Add them?"</span></span>
- <span data-ttu-id="0ba22-124">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="0ba22-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "MvcMovie".](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="0ba22-128">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="0ba22-128">Press **Debug** (F5) to build and run the program.</span></span>

![uruchomionej aplikacji](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="0ba22-130">Uruchamia kod VS [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="0ba22-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="0ba22-131">Należy zauważyć, że na pasku adresu `localhost:5000` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0ba22-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="0ba22-132">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="0ba22-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="0ba22-133">Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="0ba22-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="0ba22-134">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="0ba22-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="0ba22-135">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="0ba22-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona nawigacji w prawym górnym rogu](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="0ba22-137">W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="0ba22-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="0ba22-138">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="0ba22-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="0ba22-139">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="0ba22-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="0ba22-140">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0ba22-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="0ba22-141">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="0ba22-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="0ba22-142">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="0ba22-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="0ba22-143">Skróty klawiaturowe Mac</span><span class="sxs-lookup"><span data-stu-id="0ba22-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="0ba22-144">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="0ba22-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="0ba22-145">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="0ba22-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="0ba22-146">Następne — Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="0ba22-146">Next - Add a controller</span></span>](adding-controller.md)
