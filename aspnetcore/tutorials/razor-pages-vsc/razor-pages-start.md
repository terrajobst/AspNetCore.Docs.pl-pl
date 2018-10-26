---
title: Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code
author: rick-anderson
description: Poznaj podstawy tworzenia aplikacji sieci web stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 9ea66134c524a6a1a670d55bae4e66cf38a45274
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089855"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="58b1c-103">Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="58b1c-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="58b1c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="58b1c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="58b1c-105">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="58b1c-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="58b1c-106">Firma Microsoft zaleca, po ukończeniu [wprowadzenie do stron Razor](xref:razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="58b1c-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="58b1c-107">Strony razor jest zalecany sposób tworzenia interfejsu użytkownika dla aplikacji sieci web w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58b1c-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58b1c-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="58b1c-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="58b1c-109">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="58b1c-109">Create a Razor web app</span></span>

<span data-ttu-id="58b1c-110">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="58b1c-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="58b1c-111">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektów stron Razor.</span><span class="sxs-lookup"><span data-stu-id="58b1c-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="58b1c-112">Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="58b1c-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="58b1c-114">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="58b1c-114">Open the project</span></span>

<span data-ttu-id="58b1c-115">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="58b1c-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="58b1c-116">W programie Visual Studio Code (VS Code) wybierz **Plik > Otwórz Folder**, a następnie wybierz pozycję *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="58b1c-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="58b1c-117">Wybierz **tak** do **Ostrzegaj** komunikat "wymagane zasoby do tworzenia i debugowania brakuje"RazorPagesMovie".</span><span class="sxs-lookup"><span data-stu-id="58b1c-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="58b1c-118">Dodane?"</span><span class="sxs-lookup"><span data-stu-id="58b1c-118">Add them?"</span></span>
- <span data-ttu-id="58b1c-119">Wybierz **przywrócić** do **informacje** komunikatu "Istnieją nierozwiązane zależności".</span><span class="sxs-lookup"><span data-stu-id="58b1c-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="58b1c-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="58b1c-120">Launch the app</span></span>

<span data-ttu-id="58b1c-121">Naciśnij kombinację klawiszy Ctrl + F5, aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="58b1c-121">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="58b1c-122">Alternatywnie z **debugowania** menu, wybierz opcję **Rozpocznij bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="58b1c-122">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="58b1c-123">W następnym samouczku dodamy modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="58b1c-123">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="58b1c-124">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="58b1c-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
