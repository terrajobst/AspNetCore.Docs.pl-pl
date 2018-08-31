---
title: Wprowadzenie do platformy ASP.NET Core MVC w systemie macOS, Linux lub Windows
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio Code w systemach macOS, Linux i Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "36275278"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="f1de0-103">Wprowadzenie do platformy ASP.NET Core MVC w systemie macOS, Linux lub Windows</span><span class="sxs-lookup"><span data-stu-id="f1de0-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="f1de0-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1de0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1de0-105">Ta seria samouczków obejmuje podstawy tworzenia, ASP.NET Core MVC sieci web aplikacji za pomocą [programu Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="f1de0-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="f1de0-106">W samouczku założono familarity z programem VS Code.</span><span class="sxs-lookup"><span data-stu-id="f1de0-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="f1de0-107">Zobacz [wprowadzenie do programu VS Code](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f1de0-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="f1de0-108">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="f1de0-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f1de0-109">System macOS: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f1de0-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="f1de0-110">Windows: [tworzenie aplikacji MVC platformy ASP.NET Core z programem Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f1de0-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="f1de0-111">System macOS, Linux i Windows: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f1de0-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f1de0-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f1de0-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="f1de0-113">Tworzenie aplikacji sieci web za pomocą polecenia dotnet</span><span class="sxs-lookup"><span data-stu-id="f1de0-113">Create a web app with dotnet</span></span>

<span data-ttu-id="f1de0-114">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f1de0-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="f1de0-115">Otwórz *MvcMovie* folderu w programie Visual Studio Code (VS Code) i wybierz pozycję *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="f1de0-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="f1de0-116">Wybierz **tak** do **Ostrzegaj** komunikat "wymagane zasoby do tworzenia i debugowania brakuje"MvcMovie".</span><span class="sxs-lookup"><span data-stu-id="f1de0-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="f1de0-117">Dodane?"</span><span class="sxs-lookup"><span data-stu-id="f1de0-117">Add them?"</span></span>
- <span data-ttu-id="f1de0-118">Wybierz **przywrócić** do **informacje** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="f1de0-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![Program VS Code z zasobami Ostrzegaj wymagane do tworzenia i debugowania brakuje "MvcMovie".](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="f1de0-122">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="f1de0-122">Press **Debug** (F5) to build and run the program.</span></span>

![uruchamianie aplikacji](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="f1de0-124">Uruchamia program VS Code [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="f1de0-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="f1de0-125">Należy zauważyć, że na pasku adresu `localhost:5000` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f1de0-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="f1de0-126">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="f1de0-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="f1de0-127">Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="f1de0-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="f1de0-128">Na powyższej ilustracji przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="f1de0-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="f1de0-129">W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="f1de0-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona Nawigacja w prawym górnym rogu](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="f1de0-131">W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="f1de0-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="f1de0-132">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="f1de0-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="f1de0-133">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="f1de0-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="f1de0-134">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="f1de0-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="f1de0-135">Zintegrowany terminal</span><span class="sxs-lookup"><span data-stu-id="f1de0-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="f1de0-136">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="f1de0-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="f1de0-137">skróty klawiaturowe dla systemu macOS</span><span class="sxs-lookup"><span data-stu-id="f1de0-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="f1de0-138">Skróty klawiaturowe w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="f1de0-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="f1de0-139">Skróty klawiaturowe Windows</span><span class="sxs-lookup"><span data-stu-id="f1de0-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="f1de0-140">Następne — Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="f1de0-140">Next - Add a controller</span></span>](adding-controller.md)
