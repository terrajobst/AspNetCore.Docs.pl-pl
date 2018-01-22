---
title: Wprowadzenie do platformy ASP.NET Core 1.1
author: rick-anderson
description: "Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core 1.1."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: 7f178618508e1a1e9c49d8ace619b9f942998dae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="ecca7-103">Wprowadzenie do platformy ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="ecca7-103">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="ecca7-104">Te instrukcje dotyczą ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="ecca7-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="ecca7-105">Szukasz do najnowszej wersji?</span><span class="sxs-lookup"><span data-stu-id="ecca7-105">Looking for the latest version?</span></span> <span data-ttu-id="ecca7-106">Zobacz [bieżącej wersji tego samouczka](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="ecca7-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="ecca7-107">Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core 1.0.5 & 1.1.2 strona pliki do pobrania zestawu SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="ecca7-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="ecca7-108">Utwórz folder dla nowego projektu platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecca7-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="ecca7-109">System macOS i Linux Otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="ecca7-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="ecca7-110">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="ecca7-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="ecca7-111">Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* pliku, aby wybrać 1.0.4 zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="ecca7-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="ecca7-112">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecca7-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="ecca7-113">Przywracanie pakietów.</span><span class="sxs-lookup"><span data-stu-id="ecca7-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="ecca7-114">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ecca7-114">Run the app.</span></span>

   <span data-ttu-id="ecca7-115">`dotnet run` Polecenie tworzy najpierw aplikacji, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="ecca7-115">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="ecca7-116">Przejdź do`http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="ecca7-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="ecca7-117">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ecca7-117">Next steps</span></span>

<span data-ttu-id="ecca7-118">Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="ecca7-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="ecca7-119">Aby obejrzeć wprowadzenie do platformy ASP.NET podstawowych pojęć i architektury, zobacz [platformy ASP.NET Core wprowadzenie](index.md) i [podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="ecca7-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="ecca7-120">Użyć aplikacji platformy ASP.NET Core .NET Core lub biblioteka klas programu .NET Framework podstawowej i środowiska wykonawczego.</span><span class="sxs-lookup"><span data-stu-id="ecca7-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="ecca7-121">Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="ecca7-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>