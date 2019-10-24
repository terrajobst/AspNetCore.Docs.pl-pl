---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z usługą Blazor, tworząc aplikację Blazor przy użyciu wybranych przez siebie narzędzi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/21/2019
uid: blazor/get-started
ms.openlocfilehash: 80ff7b42a44e722dd27bc4fde53a066863448e10
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779126"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="6bcda-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="6bcda-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="6bcda-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6bcda-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="6bcda-105">Rozpocznij pracę z usługą Blazor:</span><span class="sxs-lookup"><span data-stu-id="6bcda-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="6bcda-106">Zainstaluj [zestaw SDK programu .NET Core 3,1 (wersja zapoznawcza](https://dotnet.microsoft.com/download/dotnet-core/3.1)).</span><span class="sxs-lookup"><span data-stu-id="6bcda-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="6bcda-107">Zainstaluj szablon [Webassembly Blazor](xref:blazor/hosting-models#blazor-webassembly) , uruchamiając następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="6bcda-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="6bcda-108">Pakiet [Microsoft. AspNetCore. Blazor. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, podczas gdy Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="6bcda-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="6bcda-109">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="6bcda-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6bcda-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6bcda-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="6bcda-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-111">1\.</span></span> <span data-ttu-id="6bcda-112">Zainstaluj [program Visual Studio 16,4 w wersji zapoznawczej 2 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="6bcda-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="6bcda-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-113">2\.</span></span> <span data-ttu-id="6bcda-114">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="6bcda-114">Create a new project.</span></span>

   <span data-ttu-id="6bcda-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-115">3\.</span></span> <span data-ttu-id="6bcda-116">Wybierz pozycję **aplikacja Blazor**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-116">Select **Blazor App**.</span></span> <span data-ttu-id="6bcda-117">Wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-117">Select **Next**.</span></span>

   <span data-ttu-id="6bcda-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-118">4\.</span></span> <span data-ttu-id="6bcda-119">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="6bcda-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="6bcda-120">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="6bcda-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="6bcda-121">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-121">Select **Create**.</span></span>

   <span data-ttu-id="6bcda-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-122">5\.</span></span> <span data-ttu-id="6bcda-123">Aby zapoznać się z Blazor webassembly, wybierz szablon **aplikacji Blazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="6bcda-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="6bcda-124">Dla środowiska serwera Blazor wybierz szablon **aplikacji Blazor Server** .</span><span class="sxs-lookup"><span data-stu-id="6bcda-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="6bcda-125">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-125">Select **Create**.</span></span> <span data-ttu-id="6bcda-126">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bcda-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bcda-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-127">6\.</span></span> <span data-ttu-id="6bcda-128">Naciśnij klawisz **Ctrl** , +**F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6bcda-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6bcda-129">Jeśli zainstalowano rozszerzenie Blazor programu Visual Studio dla starszej wersji zapoznawczej programu ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="6bcda-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="6bcda-130">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6bcda-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6bcda-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6bcda-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="6bcda-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-132">1\.</span></span> <span data-ttu-id="6bcda-133">Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="6bcda-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="6bcda-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-134">2\.</span></span> <span data-ttu-id="6bcda-135">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="6bcda-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="6bcda-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-136">3\.</span></span> <span data-ttu-id="6bcda-137">W przypadku środowiska webassembly Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="6bcda-138">W przypadku środowiska serwera Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="6bcda-139">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bcda-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bcda-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-140">4\.</span></span> <span data-ttu-id="6bcda-141">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6bcda-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="6bcda-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-142">5\.</span></span> <span data-ttu-id="6bcda-143">W przypadku projektu serwera Blazor, IDE żąda dodania zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="6bcda-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="6bcda-144">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-144">Select **Yes**.</span></span>

   <span data-ttu-id="6bcda-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-145">6\.</span></span> <span data-ttu-id="6bcda-146">W przypadku korzystania z aplikacji serwera Blazor należy uruchomić aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6bcda-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="6bcda-147">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bcda-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="6bcda-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-148">7\.</span></span> <span data-ttu-id="6bcda-149">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6bcda-149">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6bcda-150">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6bcda-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="6bcda-151">W przypadku środowiska webassembly Blazor wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6bcda-152">W przypadku środowiska serwera Blazor należy wykonać następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6bcda-153">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bcda-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bcda-154">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6bcda-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="6bcda-155">Zainstaluj najnowszą wersję [zestawu SDK platformy .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="6bcda-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="6bcda-156">Opcjonalnie można zainstalować szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) , instalując [zestaw .NET Core 3,1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) , a następnie uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="6bcda-157">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="6bcda-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6bcda-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6bcda-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="6bcda-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-159">1\.</span></span> <span data-ttu-id="6bcda-160">Zainstaluj najnowszą wersję [programu Visual Studio](https://visualstudio.com/vs/) , korzystając z obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="6bcda-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="6bcda-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-161">2\.</span></span> <span data-ttu-id="6bcda-162">Opcjonalnie zainstaluj [program Visual Studio 16,4 w wersji zapoznawczej 2 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** dla tworzenia aplikacji Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="6bcda-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="6bcda-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-163">3\.</span></span> <span data-ttu-id="6bcda-164">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="6bcda-164">Create a new project.</span></span>

   <span data-ttu-id="6bcda-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-165">4\.</span></span> <span data-ttu-id="6bcda-166">Wybierz pozycję **aplikacja Blazor**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-166">Select **Blazor App**.</span></span> <span data-ttu-id="6bcda-167">Wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-167">Select **Next**.</span></span>

   <span data-ttu-id="6bcda-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-168">5\.</span></span> <span data-ttu-id="6bcda-169">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="6bcda-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="6bcda-170">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="6bcda-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="6bcda-171">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-171">Select **Create**.</span></span>

   <span data-ttu-id="6bcda-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-172">6\.</span></span> <span data-ttu-id="6bcda-173">Aby zapoznać się z Blazor webassembly, wybierz szablon **aplikacji Blazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="6bcda-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="6bcda-174">Dla środowiska serwera Blazor wybierz szablon **aplikacji Blazor Server** .</span><span class="sxs-lookup"><span data-stu-id="6bcda-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="6bcda-175">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-175">Select **Create**.</span></span> <span data-ttu-id="6bcda-176">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bcda-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bcda-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-177">7\.</span></span> <span data-ttu-id="6bcda-178">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6bcda-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6bcda-179">Jeśli zainstalowano rozszerzenie Blazor programu Visual Studio dla starszej wersji zapoznawczej programu ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="6bcda-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="6bcda-180">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6bcda-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6bcda-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6bcda-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="6bcda-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-182">1\.</span></span> <span data-ttu-id="6bcda-183">Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="6bcda-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="6bcda-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-184">2\.</span></span> <span data-ttu-id="6bcda-185">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="6bcda-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="6bcda-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-186">3\.</span></span> <span data-ttu-id="6bcda-187">W przypadku środowiska webassembly Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="6bcda-188">W przypadku środowiska serwera Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="6bcda-189">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bcda-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bcda-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-190">4\.</span></span> <span data-ttu-id="6bcda-191">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6bcda-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="6bcda-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-192">5\.</span></span> <span data-ttu-id="6bcda-193">W przypadku projektu serwera Blazor, IDE żąda dodania zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="6bcda-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="6bcda-194">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="6bcda-194">Select **Yes**.</span></span>

   <span data-ttu-id="6bcda-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-195">6\.</span></span> <span data-ttu-id="6bcda-196">W przypadku korzystania z aplikacji serwera Blazor należy uruchomić aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6bcda-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="6bcda-197">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bcda-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="6bcda-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="6bcda-198">7\.</span></span> <span data-ttu-id="6bcda-199">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6bcda-199">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6bcda-200">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6bcda-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="6bcda-201">W przypadku środowiska webassembly Blazor wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6bcda-202">W przypadku środowiska serwera Blazor należy wykonać następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6bcda-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6bcda-203">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bcda-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bcda-204">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6bcda-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="6bcda-205">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="6bcda-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="6bcda-206">Home</span><span class="sxs-lookup"><span data-stu-id="6bcda-206">Home</span></span>
* <span data-ttu-id="6bcda-207">Licznik</span><span class="sxs-lookup"><span data-stu-id="6bcda-207">Counter</span></span>
* <span data-ttu-id="6bcda-208">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="6bcda-208">Fetch data</span></span>

<span data-ttu-id="6bcda-209">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="6bcda-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="6bcda-210">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale z Blazor można użyć C#.</span><span class="sxs-lookup"><span data-stu-id="6bcda-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="6bcda-211">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="6bcda-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="6bcda-212">Żądanie `/counter` w przeglądarce, zgodnie z definicją w dyrektywie `@page` u góry, powoduje, że składnik `Counter` będzie renderować jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="6bcda-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="6bcda-213">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="6bcda-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="6bcda-214">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="6bcda-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="6bcda-215">Zdarzenie `onclick` jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="6bcda-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="6bcda-216">Metoda `IncrementCount` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="6bcda-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="6bcda-217">@No__t_0 jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="6bcda-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="6bcda-218">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="6bcda-218">The component is rendered again.</span></span>

<span data-ttu-id="6bcda-219">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="6bcda-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="6bcda-220">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="6bcda-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="6bcda-221">Na przykład Dodaj składnik `Counter` do strony głównej aplikacji, dodając element `<Counter />` do składnika `Index`.</span><span class="sxs-lookup"><span data-stu-id="6bcda-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="6bcda-222">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="6bcda-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="6bcda-223">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6bcda-223">Run the app.</span></span> <span data-ttu-id="6bcda-224">Strona główna ma swój własny licznik dostarczony przez składnik `Counter`.</span><span class="sxs-lookup"><span data-stu-id="6bcda-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="6bcda-225">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="6bcda-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="6bcda-226">Aby dodać parametr do składnika `Counter`, zaktualizuj blok `@code` składnika:</span><span class="sxs-lookup"><span data-stu-id="6bcda-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="6bcda-227">Dodaj właściwość publiczną dla `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="6bcda-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="6bcda-228">Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="6bcda-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="6bcda-229">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="6bcda-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="6bcda-230">Określ `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6bcda-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="6bcda-231">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="6bcda-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="6bcda-232">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6bcda-232">Run the app.</span></span> <span data-ttu-id="6bcda-233">Składnik `Index` ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="6bcda-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="6bcda-234">Składnik `Counter` (*Counter. Razor*) w `/counter` kontynuuje zwiększanie o jeden.</span><span class="sxs-lookup"><span data-stu-id="6bcda-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6bcda-235">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="6bcda-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="6bcda-236">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6bcda-236">Additional resources</span></span>

* <xref:signalr/introduction>
