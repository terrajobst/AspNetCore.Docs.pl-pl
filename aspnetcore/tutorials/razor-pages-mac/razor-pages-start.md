---
title: "Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core dla komputerów Mac"
author: rick-anderson
description: "Wprowadzenie do korzystania z Razor strony platformy ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: d4d8c7c543273aa38d1599b51d00a348df7182de
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/26/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="6019f-103">Wprowadzenie do korzystania z Razor strony platformy ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="6019f-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="6019f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6019f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6019f-105">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="6019f-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="6019f-106">Firma Microsoft zaleca przejrzenie [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="6019f-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="6019f-107">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6019f-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6019f-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6019f-108">Prerequisites</span></span>

<span data-ttu-id="6019f-109">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="6019f-109">Install the following:</span></span>

* <span data-ttu-id="6019f-110">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6019f-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="6019f-111">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6019f-111">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="6019f-112">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="6019f-112">Create a Razor web app</span></span>

<span data-ttu-id="6019f-113">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6019f-113">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="6019f-114">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="6019f-114">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="6019f-115">Otwórz w przeglądarce http://localhost: 5000, aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6019f-115">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="6019f-117">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="6019f-117">Open the project</span></span>

<span data-ttu-id="6019f-118">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="6019f-118">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="6019f-119">W programie Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="6019f-119">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="6019f-120">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="6019f-120">Launch the app</span></span>

<span data-ttu-id="6019f-121">W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6019f-121">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="6019f-122">Uruchamia program Visual Studio [usług IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6019f-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="6019f-123">W następnym samouczku modelu dodania do projektu.</span><span class="sxs-lookup"><span data-stu-id="6019f-123">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6019f-124">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="6019f-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
