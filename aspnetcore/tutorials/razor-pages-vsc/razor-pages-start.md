---
title: Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code
author: rick-anderson
description: Poznaj podstawy tworzenia aplikacji sieci web stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244856"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="e6456-103">Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e6456-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="e6456-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e6456-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e6456-105">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="e6456-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="e6456-106">Firma Microsoft zaleca, po ukończeniu [wprowadzenie do stron Razor](xref:razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e6456-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="e6456-107">Strony razor jest zalecany sposób tworzenia interfejsu użytkownika dla aplikacji sieci web w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6456-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6456-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e6456-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e6456-109">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="e6456-109">Create a Razor web app</span></span>

<span data-ttu-id="e6456-110">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e6456-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="e6456-111">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektów stron Razor.</span><span class="sxs-lookup"><span data-stu-id="e6456-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="e6456-112">Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="e6456-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="e6456-114">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="e6456-114">Open the project</span></span>

<span data-ttu-id="e6456-115">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="e6456-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="e6456-116">W programie Visual Studio Code (VS Code) wybierz **Plik > Otwórz Folder**, a następnie wybierz pozycję *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="e6456-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="e6456-117">Wybierz **tak** do **Ostrzegaj** komunikat "wymagane zasoby do tworzenia i debugowania brakuje"RazorPagesMovie".</span><span class="sxs-lookup"><span data-stu-id="e6456-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="e6456-118">Dodane?"</span><span class="sxs-lookup"><span data-stu-id="e6456-118">Add them?"</span></span>
- <span data-ttu-id="e6456-119">Wybierz **przywrócić** do **informacje** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="e6456-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="e6456-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="e6456-120">Launch the app</span></span>

<span data-ttu-id="e6456-121">Z **debugowania** menu, wybierz opcję **Rozpocznij bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="e6456-121">From the **Debug** menu, select **Start Without Debugging**.</span></span> <span data-ttu-id="e6456-122">Alternatywnie możesz nacisnąć skrót klawiaturowy wyświetlane obok opcji menu.</span><span class="sxs-lookup"><span data-stu-id="e6456-122">Alternatively, you can press the keyboard shortcut displayed next to the menu option.</span></span> <span data-ttu-id="e6456-123">Ten skrót różni się w zależności od używanego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="e6456-123">This shortcut varies depending on your operating system.</span></span>

<span data-ttu-id="e6456-124">W następnym samouczku dodamy modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="e6456-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="e6456-125">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="e6456-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
