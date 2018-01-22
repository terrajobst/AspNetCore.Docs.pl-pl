---
title: Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core z kodem Visual Studio
author: rick-anderson
description: "Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core za pomocą programu Visual Studio Code"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 727c3d5c8bed0aef3094816b7e5f0a940aea654c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="79cdc-103">Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core z kodem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79cdc-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="79cdc-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="79cdc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="79cdc-105">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="79cdc-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="79cdc-106">Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="79cdc-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="79cdc-107">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79cdc-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79cdc-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="79cdc-108">Prerequisites</span></span>

<span data-ttu-id="79cdc-109">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="79cdc-109">Install the following:</span></span>

* <span data-ttu-id="79cdc-110">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="79cdc-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="79cdc-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79cdc-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="79cdc-112">Kod VS [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="79cdc-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="79cdc-113">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="79cdc-113">Create a Razor web app</span></span>

<span data-ttu-id="79cdc-114">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="79cdc-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="79cdc-115">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="79cdc-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="79cdc-116">Otwórz w przeglądarce http://localhost: 5000, aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="79cdc-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="79cdc-118">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="79cdc-118">Open the project</span></span>

<span data-ttu-id="79cdc-119">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="79cdc-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="79cdc-120">Z programu Visual Studio (kod VS), wybierz **Plik > Otwórz Folder**, a następnie wybierz *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="79cdc-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="79cdc-121">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"RazorPagesMovie".</span><span class="sxs-lookup"><span data-stu-id="79cdc-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="79cdc-122">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="79cdc-122">Add them?"</span></span>
- <span data-ttu-id="79cdc-123">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="79cdc-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="79cdc-124">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="79cdc-124">Launch the app</span></span>

<span data-ttu-id="79cdc-125">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="79cdc-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="79cdc-126">Alternatywnie z **debugowania** menu, wybierz opcję **uruchomić bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="79cdc-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="79cdc-127">W następnym samouczku modelu dodania do projektu.</span><span class="sxs-lookup"><span data-stu-id="79cdc-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="79cdc-128">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="79cdc-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
