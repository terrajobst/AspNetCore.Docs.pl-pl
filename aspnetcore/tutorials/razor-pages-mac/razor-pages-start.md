---
title: "Rozpoczynanie pracy z Razor strony platformy ASP.NET Core dla komputerów Mac"
author: rick-anderson
description: "Dowiedzieć się, jak rozpocząć pracę z Razor strony platformy ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac."
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: e56799d321cc84c60188a72def9448a0b339d568
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="99813-103">Rozpoczynanie pracy z Razor strony platformy ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="99813-103">Get started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="99813-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="99813-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="99813-105">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="99813-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="99813-106">Firma Microsoft zaleca przejrzenie [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="99813-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="99813-107">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99813-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99813-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="99813-108">Prerequisites</span></span>

<span data-ttu-id="99813-109">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="99813-109">Install the following:</span></span>

* <span data-ttu-id="99813-110">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="99813-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="99813-111">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="99813-111">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="99813-112">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="99813-112">Create a Razor web app</span></span>

<span data-ttu-id="99813-113">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="99813-113">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="99813-114">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="99813-114">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="99813-115">Otwórz w przeglądarce http://localhost: 5000, aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="99813-115">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="99813-117">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="99813-117">Open the project</span></span>

<span data-ttu-id="99813-118">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="99813-118">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="99813-119">W programie Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="99813-119">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="99813-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="99813-120">Launch the app</span></span>

<span data-ttu-id="99813-121">W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="99813-121">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="99813-122">Uruchamia program Visual Studio [Kestrel](xref:fundamentals/servers/kestrel)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="99813-122">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="99813-123">W następnym samouczku modelu dodania do projektu.</span><span class="sxs-lookup"><span data-stu-id="99813-123">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="99813-124">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="99813-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
