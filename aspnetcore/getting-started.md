---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: f772bd922d9e474d5ad99c08af19c90fe06027af
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="24ce6-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24ce6-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="24ce6-104">Te instrukcje dotyczą najnowszej wersjiplatformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="24ce6-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="24ce6-105">Chcesz rozpocząć korzystanie z wcześniejszej wersji?</span><span class="sxs-lookup"><span data-stu-id="24ce6-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="24ce6-106">Zobacz [wersja 1.1 tego samouczka](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="24ce6-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="24ce6-107">Zainstaluj [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="24ce6-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="24ce6-108">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="24ce6-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="24ce6-109">W systemach macOS i Linux otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="24ce6-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="24ce6-110">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="24ce6-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="24ce6-111">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="24ce6-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="24ce6-112">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="24ce6-112">Run the app.</span></span>

    <span data-ttu-id="24ce6-113">Użyj następujących poleceń, aby uruchomić aplikację:</span><span class="sxs-lookup"><span data-stu-id="24ce6-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="24ce6-114">Przejdź do [http://localhost: 5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="24ce6-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="24ce6-115">Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world!</span><span class="sxs-lookup"><span data-stu-id="24ce6-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="24ce6-116">The time on the server is @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="24ce6-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="24ce6-117">Przejdź do [http://localhost: 5000/o](http://localhost:5000/About) i zweryfikuj zmiany.</span><span class="sxs-lookup"><span data-stu-id="24ce6-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="24ce6-118">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="24ce6-118">Next steps</span></span>

<span data-ttu-id="24ce6-119">Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="24ce6-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="24ce6-120">Aby zapoznać się z wprowadzeniem do podstawowych pojęć i architektury platformy ASP.NET, zobacz [Wprowadzenie do platformy ASP.NET Core](index.md) i [Podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="24ce6-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="24ce6-121">Aplikacja ASP.NET Core może używać biblioteki klas bazowych i środowiska uruchomieniowego .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="24ce6-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="24ce6-122">Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="24ce6-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
