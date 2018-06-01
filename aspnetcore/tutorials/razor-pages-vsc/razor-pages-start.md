---
title: Wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code
author: rick-anderson
description: Poznaj podstawy tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony z kodem Visual Studio.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 3dda0f20dfbb7066dfeb3360361435ef65caaca4
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34688390"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="fb1a6-103">Wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fb1a6-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="fb1a6-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb1a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fb1a6-105">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="fb1a6-106">Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="fb1a6-107">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb1a6-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fb1a6-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="fb1a6-109">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="fb1a6-109">Create a Razor web app</span></span>

<span data-ttu-id="fb1a6-110">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fb1a6-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="fb1a6-111">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="fb1a6-112">Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="fb1a6-114">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="fb1a6-114">Open the project</span></span>

<span data-ttu-id="fb1a6-115">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="fb1a6-116">Z programu Visual Studio (kod VS), wybierz **Plik > Otwórz Folder**, a następnie wybierz *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="fb1a6-117">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"RazorPagesMovie".</span><span class="sxs-lookup"><span data-stu-id="fb1a6-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="fb1a6-118">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="fb1a6-118">Add them?"</span></span>
- <span data-ttu-id="fb1a6-119">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="fb1a6-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="fb1a6-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="fb1a6-120">Launch the app</span></span>

<span data-ttu-id="fb1a6-121">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-121">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="fb1a6-122">Alternatywnie z **debugowania** menu, wybierz opcję **uruchomić bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-122">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="fb1a6-123">W następnym samouczku modelu dodania do projektu.</span><span class="sxs-lookup"><span data-stu-id="fb1a6-123">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="fb1a6-124">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="fb1a6-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
