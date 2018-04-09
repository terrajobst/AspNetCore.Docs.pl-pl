---
title: Wprowadzenie do platformy ASP.NET Core 1.1
author: rick-anderson
description: Postępuj zgodnie z tego samouczka szybkiego do tworzenia i uruchamiania prostej aplikacji Hello World przy użyciu platformy ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a><span data-ttu-id="115f4-103">Wprowadzenie do platformy ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="115f4-103">Get Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="115f4-104">Te instrukcje dotyczą ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="115f4-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="115f4-105">Szukasz do najnowszej wersji?</span><span class="sxs-lookup"><span data-stu-id="115f4-105">Looking for the latest version?</span></span> <span data-ttu-id="115f4-106">Zobacz [bieżącej wersji tego samouczka](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="115f4-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="115f4-107">Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="115f4-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="115f4-108">Utwórz folder dla nowego projektu platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="115f4-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="115f4-109">W systemach macOS i Linux otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="115f4-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="115f4-110">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="115f4-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="115f4-111">Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* pliku, aby wybrać 1.0.4 zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="115f4-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="115f4-112">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="115f4-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="115f4-113">Przywracanie pakietów.</span><span class="sxs-lookup"><span data-stu-id="115f4-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="115f4-114">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="115f4-114">Run the app.</span></span>

   <span data-ttu-id="115f4-115">[Dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="115f4-115">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="115f4-116">Przejdź do `http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="115f4-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="115f4-117">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="115f4-117">Next steps</span></span>

<span data-ttu-id="115f4-118">Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="115f4-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="115f4-119">Aby zapoznać się z wprowadzeniem do podstawowych pojęć i architektury platformy ASP.NET, zobacz [Wprowadzenie do platformy ASP.NET Core](index.md) i [Podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="115f4-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="115f4-120">Aplikacja ASP.NET Core może używać biblioteki klas bazowych i środowiska uruchomieniowego .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="115f4-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="115f4-121">Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="115f4-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
