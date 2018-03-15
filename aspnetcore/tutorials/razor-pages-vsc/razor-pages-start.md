---
title: Wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code
author: rick-anderson
description: Poznaj podstawy tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony z kodem Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: ab14937508ad68f3ffc9c9ba797119b3c83fd743
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="3d4b0-103">Wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d4b0-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="3d4b0-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3d4b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3d4b0-105">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="3d4b0-106">Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="3d4b0-107">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d4b0-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3d4b0-108">Prerequisites</span></span>

<span data-ttu-id="3d4b0-109">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="3d4b0-109">Install the following:</span></span>

* <span data-ttu-id="3d4b0-110">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="3d4b0-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="3d4b0-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d4b0-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="3d4b0-112">Kod VS [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="3d4b0-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="3d4b0-113">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="3d4b0-113">Create a Razor web app</span></span>

<span data-ttu-id="3d4b0-114">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="3d4b0-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="3d4b0-115">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="3d4b0-116">Otwórz w przeglądarce http://localhost: 5000, aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="3d4b0-118">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="3d4b0-118">Open the project</span></span>

<span data-ttu-id="3d4b0-119">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="3d4b0-120">Z programu Visual Studio (kod VS), wybierz **Plik > Otwórz Folder**, a następnie wybierz *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="3d4b0-121">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"RazorPagesMovie".</span><span class="sxs-lookup"><span data-stu-id="3d4b0-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="3d4b0-122">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="3d4b0-122">Add them?"</span></span>
- <span data-ttu-id="3d4b0-123">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="3d4b0-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="3d4b0-124">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="3d4b0-124">Launch the app</span></span>

<span data-ttu-id="3d4b0-125">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="3d4b0-126">Alternatywnie z **debugowania** menu, wybierz opcję **uruchomić bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="3d4b0-127">W następnym samouczku modelu dodania do projektu.</span><span class="sxs-lookup"><span data-stu-id="3d4b0-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="3d4b0-128">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="3d4b0-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
