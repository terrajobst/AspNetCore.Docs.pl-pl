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
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="005b4-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="005b4-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="005b4-104">Te instrukcje dotyczą najnowszej wersjiplatformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="005b4-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="005b4-105">Zobacz [wprowadzenie do platformy ASP.NET Core 1.1](xref:getting-started-1.1) wersji 1.1 tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="005b4-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="005b4-106">Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="005b4-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="005b4-107">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="005b4-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="005b4-108">W systemach macOS i Linux otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="005b4-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="005b4-109">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="005b4-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="005b4-110">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="005b4-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="005b4-111">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="005b4-111">Run the app.</span></span>

    <span data-ttu-id="005b4-112">Użyj następujących poleceń, aby uruchomić aplikację:</span><span class="sxs-lookup"><span data-stu-id="005b4-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="005b4-113">Przejdź do [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="005b4-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="005b4-114">Otwórz <em>Pages/About.cshtml</em>i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world!</span><span class="sxs-lookup"><span data-stu-id="005b4-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="005b4-115">The time on the server is @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="005b4-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="005b4-116">Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) i zweryfikować zmiany.</span><span class="sxs-lookup"><span data-stu-id="005b4-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="005b4-117">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="005b4-117">Next steps</span></span>

<span data-ttu-id="005b4-118">Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="005b4-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="005b4-119">Aby zapoznać się z wprowadzeniem do podstawowych pojęć i architektury platformy ASP.NET, zobacz [Wprowadzenie do platformy ASP.NET Core](index.md) i [Podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="005b4-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="005b4-120">Aplikacja ASP.NET Core może używać biblioteki klas bazowych i środowiska uruchomieniowego .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="005b4-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="005b4-121">Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="005b4-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
