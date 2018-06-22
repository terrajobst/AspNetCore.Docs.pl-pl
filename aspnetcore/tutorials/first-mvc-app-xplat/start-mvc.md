---
title: Wprowadzenie do platformy ASP.NET Core MVC macOS, Linux lub Windows
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformy ASP.NET Core MVC i Visual Studio Code na macOS, Linux i Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275278"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="1088e-103">Wprowadzenie do platformy ASP.NET Core MVC macOS, Linux lub Windows</span><span class="sxs-lookup"><span data-stu-id="1088e-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="1088e-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1088e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1088e-105">W tym samouczku udzieli Ci podstawy platformy ASP.NET Core MVC sieci web aplikacji za pomocą [Visual Studio Code](https://code.visualstudio.com) (VS kodu).</span><span class="sxs-lookup"><span data-stu-id="1088e-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="1088e-106">Samouczek jest przeznaczony dla familarity z kodem VS.</span><span class="sxs-lookup"><span data-stu-id="1088e-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="1088e-107">Zobacz [wprowadzenie kodu VS](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1088e-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="1088e-108">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="1088e-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="1088e-109">System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1088e-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="1088e-110">System Windows: [utworzyć platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1088e-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="1088e-111">System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC z kodem Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1088e-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1088e-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1088e-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="1088e-113">Tworzenie aplikacji sieci web w usłudze dotnet</span><span class="sxs-lookup"><span data-stu-id="1088e-113">Create a web app with dotnet</span></span>

<span data-ttu-id="1088e-114">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1088e-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="1088e-115">Otwórz *MvcMovie* folderu w Visual Studio (kod VS) i wybierz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="1088e-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="1088e-116">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"MvcMovie".</span><span class="sxs-lookup"><span data-stu-id="1088e-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="1088e-117">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="1088e-117">Add them?"</span></span>
- <span data-ttu-id="1088e-118">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="1088e-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![Kod w PORÓWNANIU z Ostrzegaj wymagane zasoby do tworzenia i debugowania brakuje "MvcMovie".](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="1088e-122">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="1088e-122">Press **Debug** (F5) to build and run the program.</span></span>

![uruchomionej aplikacji](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="1088e-124">Uruchamia kod VS [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="1088e-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="1088e-125">Należy zauważyć, że na pasku adresu `localhost:5000` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1088e-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="1088e-126">Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1088e-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="1088e-127">Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="1088e-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="1088e-128">Powyższy obraz przeglądarki nie wyświetla tych łączy.</span><span class="sxs-lookup"><span data-stu-id="1088e-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="1088e-129">W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="1088e-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona nawigacji w prawym górnym rogu](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="1088e-131">W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="1088e-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="1088e-132">Visual Studio Code pomocy</span><span class="sxs-lookup"><span data-stu-id="1088e-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="1088e-133">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="1088e-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="1088e-134">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="1088e-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="1088e-135">Zintegrowane terminali</span><span class="sxs-lookup"><span data-stu-id="1088e-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="1088e-136">Skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="1088e-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="1088e-137">System macOS skróty klawiaturowe</span><span class="sxs-lookup"><span data-stu-id="1088e-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="1088e-138">Skróty klawiaturowe systemu Linux</span><span class="sxs-lookup"><span data-stu-id="1088e-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="1088e-139">Skróty klawiaturowe systemu Windows</span><span class="sxs-lookup"><span data-stu-id="1088e-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="1088e-140">Następne — Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="1088e-140">Next - Add a controller</span></span>](adding-controller.md)
