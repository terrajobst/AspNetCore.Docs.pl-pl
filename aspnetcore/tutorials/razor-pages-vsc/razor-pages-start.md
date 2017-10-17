---
title: Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core z kodem Visual Studio
author: rick-anderson
description: "Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core za pomocą programu Visual Studio Code"
keywords: Platformy ASP.NET Core, stron Razor tworzenia szkieletu, Entity Framework Core, EF, EF Core, bazy danych, mac, macOS, Visual Studio Code, kod
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 1b9dff14fa98314601fa44aa229aef6b73bb79d0
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="251e6-104">Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core z kodem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="251e6-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="251e6-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="251e6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="251e6-106">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="251e6-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="251e6-107">Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="251e6-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="251e6-108">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="251e6-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="251e6-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="251e6-109">Prerequisites</span></span>

<span data-ttu-id="251e6-110">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="251e6-110">Install the following:</span></span>

* <span data-ttu-id="251e6-111">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="251e6-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="251e6-112">Kod Visual Studio</span><span class="sxs-lookup"><span data-stu-id="251e6-112">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="251e6-113">Kod VS [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="251e6-113">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="251e6-114">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="251e6-114">Create a Razor web app</span></span>

<span data-ttu-id="251e6-115">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="251e6-115">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="251e6-116">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="251e6-116">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="251e6-117">Otwórz w przeglądarce http://localhost: 5000, aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="251e6-117">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="251e6-119">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="251e6-119">Open the project</span></span>

<span data-ttu-id="251e6-120">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="251e6-120">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="251e6-121">Z programu Visual Studio (kod VS), wybierz **Plik > Otwórz Folder**, a następnie wybierz *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="251e6-121">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="251e6-122">Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"RazorPagesMovie".</span><span class="sxs-lookup"><span data-stu-id="251e6-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="251e6-123">Dodaj je?"</span><span class="sxs-lookup"><span data-stu-id="251e6-123">Add them?"</span></span>
- <span data-ttu-id="251e6-124">Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="251e6-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="251e6-125">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="251e6-125">Launch the app</span></span>

<span data-ttu-id="251e6-126">Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="251e6-126">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="251e6-127">Alternatywnie z **debugowania** menu, wybierz opcję **uruchomić bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="251e6-127">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="251e6-128">W następnym samouczku modelu dodania do projektu.</span><span class="sxs-lookup"><span data-stu-id="251e6-128">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="251e6-129">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="251e6-129">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
