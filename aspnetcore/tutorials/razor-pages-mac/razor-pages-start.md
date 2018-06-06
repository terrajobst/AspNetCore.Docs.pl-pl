---
title: Rozpoczynanie pracy z Razor strony platformy ASP.NET Core na macOS z programem Visual Studio dla komputerów Mac
author: rick-anderson
description: Dowiedzieć się, jak rozpocząć pracę z Razor strony platformy ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8da34f0a59976032747edcaf482f75c087ca8d8d
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688273"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="ab9c7-103">Rozpoczynanie pracy z Razor strony platformy ASP.NET Core na macOS z programem Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ab9c7-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="ab9c7-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab9c7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ab9c7-105">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ab9c7-106">Firma Microsoft zaleca przejrzenie [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="ab9c7-107">Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab9c7-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ab9c7-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ab9c7-109">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="ab9c7-109">Create a Razor web app</span></span>

<span data-ttu-id="ab9c7-110">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ab9c7-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="ab9c7-111">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="ab9c7-112">Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="ab9c7-114">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="ab9c7-114">Open the project</span></span>

<span data-ttu-id="ab9c7-115">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="ab9c7-116">W programie Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-116">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="ab9c7-117">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="ab9c7-117">Launch the app</span></span>

<span data-ttu-id="ab9c7-118">W programie Visual Studio, wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-118">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="ab9c7-119">Uruchamia program Visual Studio [Kestrel](xref:fundamentals/servers/kestrel)spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-119">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="ab9c7-120">W następnym samouczku modelu dodania do projektu.</span><span class="sxs-lookup"><span data-stu-id="ab9c7-120">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ab9c7-121">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="ab9c7-121">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
