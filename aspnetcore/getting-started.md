---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 1b4d765c08c67a65a8752e7f650d4e61ec0d2fbf
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687460"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="0a09c-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a09c-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="0a09c-104">Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="0a09c-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="0a09c-105">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a09c-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="0a09c-106">W systemach macOS i Linux otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="0a09c-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0a09c-107">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="0a09c-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="0a09c-108">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0a09c-108">Enter the following command:</span></span>

    ```terminal
    dotnet new webapp -o aspnetcoreapp
    ```

3. <span data-ttu-id="0a09c-109">Uruchom aplikację za pomocą następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="0a09c-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="0a09c-110">Przejdź do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="0a09c-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="0a09c-111">Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world!</span><span class="sxs-lookup"><span data-stu-id="0a09c-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="0a09c-112">Czas na serwerze jest @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="0a09c-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="0a09c-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="0a09c-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="0a09c-114">Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) i zweryfikować zmiany.</span><span class="sxs-lookup"><span data-stu-id="0a09c-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="0a09c-115">Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="0a09c-115">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="0a09c-116">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a09c-116">Create a new .NET Core project.</span></span>

   <span data-ttu-id="0a09c-117">W systemach macOS i Linux otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="0a09c-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0a09c-118">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="0a09c-118">On Windows, open a command prompt.</span></span> <span data-ttu-id="0a09c-119">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0a09c-119">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="0a09c-120">Uruchom aplikację za pomocą następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="0a09c-120">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="0a09c-121">Przejdź do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="0a09c-121">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="0a09c-122">Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world!</span><span class="sxs-lookup"><span data-stu-id="0a09c-122">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="0a09c-123">Czas na serwerze jest @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="0a09c-123">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="0a09c-124">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="0a09c-124">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="0a09c-125">Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) i zweryfikować zmiany.</span><span class="sxs-lookup"><span data-stu-id="0a09c-125">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="0a09c-126">Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="0a09c-126">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="0a09c-127">Utwórz folder dla nowego projektu platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a09c-127">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="0a09c-128">W systemach macOS i Linux otwórz okno terminala.</span><span class="sxs-lookup"><span data-stu-id="0a09c-128">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0a09c-129">W systemie Windows otwórz wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="0a09c-129">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="0a09c-130">Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* pliku, aby wybrać 1.0.4 zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="0a09c-130">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="0a09c-131">Utwórz nowy projekt platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a09c-131">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="0a09c-132">Przywracanie pakietów.</span><span class="sxs-lookup"><span data-stu-id="0a09c-132">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="0a09c-133">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="0a09c-133">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="0a09c-134">[Dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="0a09c-134">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="0a09c-135">Przejdź do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0a09c-135">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end