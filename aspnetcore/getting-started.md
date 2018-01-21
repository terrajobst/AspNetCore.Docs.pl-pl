---
title: Wprowadzenie do platformy ASP.NET Core 2.0
author: rick-anderson
description: "Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="66e6f-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66e6f-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="66e6f-104">Te instrukcje dotyczą najnowszą wersję platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66e6f-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="66e6f-105">Wyszukiwanie, aby rozpocząć korzystanie z wcześniejszej wersji?</span><span class="sxs-lookup"><span data-stu-id="66e6f-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="66e6f-106">Zobacz [w wersji 1.1 tego samouczka](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="66e6f-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="66e6f-107">Zainstaluj [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="66e6f-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="66e6f-108">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="66e6f-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="66e6f-109">System macOS i Linux Otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="66e6f-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="66e6f-110">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="66e6f-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="66e6f-111">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="66e6f-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="66e6f-112">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="66e6f-112">Run the app.</span></span>

    <span data-ttu-id="66e6f-113">Użyj następujących poleceń, aby uruchomić aplikację:</span><span class="sxs-lookup"><span data-stu-id="66e6f-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="66e6f-114">Przejdź do [http://localhost: 5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="66e6f-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="66e6f-115">Otwórz *Pages/About.cshtml* i modyfikować strony, aby wyświetlić komunikat "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="66e6f-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="66e6f-116">Czas na serwerze jest @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="66e6f-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="66e6f-117">Przejdź do [http://localhost: 5000/o](http://localhost:5000/About) i zweryfikować zmiany.</span><span class="sxs-lookup"><span data-stu-id="66e6f-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="66e6f-118">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="66e6f-118">Next steps</span></span>

<span data-ttu-id="66e6f-119">Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="66e6f-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="66e6f-120">Aby obejrzeć wprowadzenie do platformy ASP.NET podstawowych pojęć i architektury, zobacz [platformy ASP.NET Core wprowadzenie](index.md) i [podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="66e6f-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="66e6f-121">Użyć aplikacji platformy ASP.NET Core .NET Core lub biblioteka klas programu .NET Framework podstawowej i środowiska wykonawczego.</span><span class="sxs-lookup"><span data-stu-id="66e6f-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="66e6f-122">Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="66e6f-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
