---
title: Wprowadzenie do platformy ASP.NET Core 1.1
author: rick-anderson
description: "Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core 1.1."
keywords: "Platformy ASP.NET Core, samouczek, rozpoczęcie pracy"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="357a3-104">Wprowadzenie do platformy ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="357a3-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="357a3-105">Te instrukcje dotyczą ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="357a3-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="357a3-106">Szukasz do najnowszej wersji?</span><span class="sxs-lookup"><span data-stu-id="357a3-106">Looking for the latest version?</span></span> <span data-ttu-id="357a3-107">Zobacz [bieżącej wersji tego samouczka](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="357a3-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="357a3-108">Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core 1.0.5 & 1.1.2 strona pliki do pobrania zestawu SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="357a3-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="357a3-109">Utwórz folder dla nowego projektu platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="357a3-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="357a3-110">System macOS i Linux Otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="357a3-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="357a3-111">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="357a3-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="357a3-112">Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* pliku, aby wybrać 1.0.4 zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="357a3-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="357a3-113">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="357a3-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="357a3-114">Przywracanie pakietów.</span><span class="sxs-lookup"><span data-stu-id="357a3-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="357a3-115">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="357a3-115">Run the app.</span></span>

   <span data-ttu-id="357a3-116">`dotnet run` Polecenie tworzy najpierw aplikacji, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="357a3-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="357a3-117">Przejdź do`http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="357a3-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="357a3-118">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="357a3-118">Next steps</span></span>

<span data-ttu-id="357a3-119">Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="357a3-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="357a3-120">Aby obejrzeć wprowadzenie do platformy ASP.NET podstawowych pojęć i architektury, zobacz [platformy ASP.NET Core wprowadzenie](index.md) i [podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="357a3-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="357a3-121">Użyć aplikacji platformy ASP.NET Core .NET Core lub biblioteka klas programu .NET Framework podstawowej i środowiska wykonawczego.</span><span class="sxs-lookup"><span data-stu-id="357a3-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="357a3-122">Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="357a3-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
