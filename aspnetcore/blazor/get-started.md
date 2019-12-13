---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z Blazor, tworząc aplikację Blazor za pomocą wybranego przez siebie narzędzia.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/09/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: e368ecaf931d392de7e52ec2d5a2dfd171c2c86f
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943770"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="d4386-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="d4386-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="d4386-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d4386-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="d4386-105">Rozpocznij pracę z Blazor:</span><span class="sxs-lookup"><span data-stu-id="d4386-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="d4386-106">Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d4386-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="d4386-107">Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="d4386-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="d4386-108">Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d4386-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="d4386-109">Uruchom następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="d4386-109">Run the following command in a command shell.</span></span> <span data-ttu-id="d4386-110">[Microsoft. AspNetCore.Blazor. Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="d4386-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="d4386-111">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="d4386-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d4386-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4386-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d4386-113">1\.</span><span class="sxs-lookup"><span data-stu-id="d4386-113">1\.</span></span> <span data-ttu-id="d4386-114">Zainstaluj [program Visual Studio 16,4 lub nowszy](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="d4386-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d4386-115">2\.</span><span class="sxs-lookup"><span data-stu-id="d4386-115">2\.</span></span> <span data-ttu-id="d4386-116">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="d4386-116">Create a new project.</span></span>

   <span data-ttu-id="d4386-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-117">3\.</span></span> <span data-ttu-id="d4386-118">Wybierz pozycję **Blazor aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="d4386-118">Select **Blazor App**.</span></span> <span data-ttu-id="d4386-119">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="d4386-119">Select **Next**.</span></span>

   <span data-ttu-id="d4386-120">4\.</span><span class="sxs-lookup"><span data-stu-id="d4386-120">4\.</span></span> <span data-ttu-id="d4386-121">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="d4386-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d4386-122">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="d4386-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d4386-123">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d4386-123">Select **Create**.</span></span>

   <span data-ttu-id="d4386-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-124">5\.</span></span> <span data-ttu-id="d4386-125">W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="d4386-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d4386-126">W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="d4386-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d4386-127">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d4386-127">Select **Create**.</span></span> <span data-ttu-id="d4386-128">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4386-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d4386-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-129">6\.</span></span> <span data-ttu-id="d4386-130">Naciśnij klawisz **Ctrl** ,+**F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d4386-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d4386-131">Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="d4386-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d4386-132">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4386-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d4386-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4386-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d4386-134">1\.</span><span class="sxs-lookup"><span data-stu-id="d4386-134">1\.</span></span> <span data-ttu-id="d4386-135">Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d4386-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d4386-136">2\.</span><span class="sxs-lookup"><span data-stu-id="d4386-136">2\.</span></span> <span data-ttu-id="d4386-137">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="d4386-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d4386-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-138">3\.</span></span> <span data-ttu-id="d4386-139">W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d4386-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d4386-140">W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d4386-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d4386-141">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4386-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d4386-142">4\.</span><span class="sxs-lookup"><span data-stu-id="d4386-142">4\.</span></span> <span data-ttu-id="d4386-143">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d4386-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d4386-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-144">5\.</span></span> <span data-ttu-id="d4386-145">Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="d4386-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d4386-146">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="d4386-146">Select **Yes**.</span></span>

   <span data-ttu-id="d4386-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-147">6\.</span></span> <span data-ttu-id="d4386-148">Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d4386-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d4386-149">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4386-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d4386-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-150">7\.</span></span> <span data-ttu-id="d4386-151">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d4386-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d4386-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d4386-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="d4386-153">1\.</span><span class="sxs-lookup"><span data-stu-id="d4386-153">1\.</span></span> <span data-ttu-id="d4386-154">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="d4386-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="d4386-155">Przełącz [kanał aktualizacji na wersję zapoznawczą](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="d4386-155">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="d4386-156">2\.</span><span class="sxs-lookup"><span data-stu-id="d4386-156">2\.</span></span> <span data-ttu-id="d4386-157">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="d4386-157">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="d4386-158">3 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-158">3\.</span></span> <span data-ttu-id="d4386-159">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="d4386-159">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="d4386-160">4\.</span><span class="sxs-lookup"><span data-stu-id="d4386-160">4\.</span></span> <span data-ttu-id="d4386-161">Wybierz szablon **aplikacjiBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="d4386-161">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="d4386-162">W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d4386-162">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="d4386-163">W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d4386-163">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="d4386-164">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4386-164">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="d4386-165">5 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-165">5\.</span></span> <span data-ttu-id="d4386-166">Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d4386-166">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="d4386-167">6 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-167">6\.</span></span> <span data-ttu-id="d4386-168">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="d4386-168">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="d4386-169">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d4386-169">Select **Create**.</span></span>

   <span data-ttu-id="d4386-170">7 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-170">7\.</span></span> <span data-ttu-id="d4386-171">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="d4386-171">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="d4386-172">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="d4386-172">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d4386-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d4386-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d4386-174">W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d4386-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d4386-175">W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d4386-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d4386-176">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4386-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d4386-177">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d4386-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="d4386-178">Zainstaluj najnowszą wersję [zestawu SDK platformy .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="d4386-178">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="d4386-179">Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="d4386-179">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="d4386-180">Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d4386-180">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="d4386-181">Uruchom następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="d4386-181">Run the following command in a command shell.</span></span> <span data-ttu-id="d4386-182">[Microsoft. AspNetCore.Blazor. Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="d4386-182">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="d4386-183">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="d4386-183">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d4386-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4386-184">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d4386-185">1\.</span><span class="sxs-lookup"><span data-stu-id="d4386-185">1\.</span></span> <span data-ttu-id="d4386-186">Zainstaluj najnowszą wersję [programu Visual Studio](https://visualstudio.com/vs/) , korzystając z obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="d4386-186">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d4386-187">2\.</span><span class="sxs-lookup"><span data-stu-id="d4386-187">2\.</span></span> <span data-ttu-id="d4386-188">Opcjonalnie zainstaluj [program Visual Studio 16,4 w wersji zapoznawczej 2 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** Blazor dla tworzenia aplikacji webassembly.</span><span class="sxs-lookup"><span data-stu-id="d4386-188">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="d4386-189">3 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-189">3\.</span></span> <span data-ttu-id="d4386-190">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="d4386-190">Create a new project.</span></span>

   <span data-ttu-id="d4386-191">4\.</span><span class="sxs-lookup"><span data-stu-id="d4386-191">4\.</span></span> <span data-ttu-id="d4386-192">Wybierz pozycję **Blazor aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="d4386-192">Select **Blazor App**.</span></span> <span data-ttu-id="d4386-193">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="d4386-193">Select **Next**.</span></span>

   <span data-ttu-id="d4386-194">5 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-194">5\.</span></span> <span data-ttu-id="d4386-195">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="d4386-195">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d4386-196">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="d4386-196">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d4386-197">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d4386-197">Select **Create**.</span></span>

   <span data-ttu-id="d4386-198">6 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-198">6\.</span></span> <span data-ttu-id="d4386-199">W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="d4386-199">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d4386-200">W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="d4386-200">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d4386-201">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d4386-201">Select **Create**.</span></span> <span data-ttu-id="d4386-202">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4386-202">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d4386-203">7 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-203">7\.</span></span> <span data-ttu-id="d4386-204">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d4386-204">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d4386-205">Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="d4386-205">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d4386-206">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4386-206">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d4386-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4386-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d4386-208">1\.</span><span class="sxs-lookup"><span data-stu-id="d4386-208">1\.</span></span> <span data-ttu-id="d4386-209">Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d4386-209">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d4386-210">2\.</span><span class="sxs-lookup"><span data-stu-id="d4386-210">2\.</span></span> <span data-ttu-id="d4386-211">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="d4386-211">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d4386-212">3 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-212">3\.</span></span> <span data-ttu-id="d4386-213">W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d4386-213">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d4386-214">W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d4386-214">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d4386-215">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4386-215">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d4386-216">4\.</span><span class="sxs-lookup"><span data-stu-id="d4386-216">4\.</span></span> <span data-ttu-id="d4386-217">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d4386-217">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d4386-218">5 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-218">5\.</span></span> <span data-ttu-id="d4386-219">Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="d4386-219">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d4386-220">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="d4386-220">Select **Yes**.</span></span>

   <span data-ttu-id="d4386-221">6 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-221">6\.</span></span> <span data-ttu-id="d4386-222">Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d4386-222">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d4386-223">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4386-223">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d4386-224">7 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-224">7\.</span></span> <span data-ttu-id="d4386-225">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d4386-225">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d4386-226">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d4386-226">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="d4386-227">1\.</span><span class="sxs-lookup"><span data-stu-id="d4386-227">1\.</span></span> <span data-ttu-id="d4386-228">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="d4386-228">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="d4386-229">Przełącz [kanał aktualizacji na wersję zapoznawczą](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="d4386-229">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="d4386-230">2\.</span><span class="sxs-lookup"><span data-stu-id="d4386-230">2\.</span></span> <span data-ttu-id="d4386-231">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="d4386-231">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="d4386-232">3 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-232">3\.</span></span> <span data-ttu-id="d4386-233">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="d4386-233">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="d4386-234">4\.</span><span class="sxs-lookup"><span data-stu-id="d4386-234">4\.</span></span> <span data-ttu-id="d4386-235">Wybierz szablon **aplikacjiBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="d4386-235">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="d4386-236">W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d4386-236">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="d4386-237">W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d4386-237">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="d4386-238">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4386-238">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="d4386-239">5 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-239">5\.</span></span> <span data-ttu-id="d4386-240">Ustaw platformę **docelową** na **platformę .NET Core 3,0** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d4386-240">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="d4386-241">6 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-241">6\.</span></span> <span data-ttu-id="d4386-242">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="d4386-242">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="d4386-243">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d4386-243">Select **Create**.</span></span>

   <span data-ttu-id="d4386-244">7 \.</span><span class="sxs-lookup"><span data-stu-id="d4386-244">7\.</span></span> <span data-ttu-id="d4386-245">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="d4386-245">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="d4386-246">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="d4386-246">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d4386-247">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d4386-247">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d4386-248">W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d4386-248">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d4386-249">W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d4386-249">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d4386-250">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4386-250">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d4386-251">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d4386-251">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="d4386-252">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="d4386-252">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="d4386-253">Home</span><span class="sxs-lookup"><span data-stu-id="d4386-253">Home</span></span>
* <span data-ttu-id="d4386-254">Licznik</span><span class="sxs-lookup"><span data-stu-id="d4386-254">Counter</span></span>
* <span data-ttu-id="d4386-255">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="d4386-255">Fetch data</span></span>

<span data-ttu-id="d4386-256">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="d4386-256">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d4386-257">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale z Blazor można użyć C#.</span><span class="sxs-lookup"><span data-stu-id="d4386-257">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="d4386-258">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d4386-258">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="d4386-259">Żądanie `/counter` w przeglądarce, zgodnie z dyrektywą `@page` w górnej części, powoduje, że składnik `Counter` renderuje jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="d4386-259">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="d4386-260">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="d4386-260">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="d4386-261">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="d4386-261">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="d4386-262">Zdarzenie `onclick` jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="d4386-262">The `onclick` event is fired.</span></span>
* <span data-ttu-id="d4386-263">Metoda `IncrementCount` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="d4386-263">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="d4386-264">`currentCount` jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="d4386-264">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="d4386-265">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="d4386-265">The component is rendered again.</span></span>

<span data-ttu-id="d4386-266">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="d4386-266">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="d4386-267">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="d4386-267">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="d4386-268">Na przykład Dodaj składnik `Counter` do strony głównej aplikacji przez dodanie elementu `<Counter />` do składnika `Index`.</span><span class="sxs-lookup"><span data-stu-id="d4386-268">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="d4386-269">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d4386-269">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="d4386-270">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d4386-270">Run the app.</span></span> <span data-ttu-id="d4386-271">Strona główna ma swój własny licznik dostarczony przez składnik `Counter`.</span><span class="sxs-lookup"><span data-stu-id="d4386-271">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="d4386-272">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d4386-272">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="d4386-273">Aby dodać parametr do składnika `Counter`, zaktualizuj blok `@code` składnika:</span><span class="sxs-lookup"><span data-stu-id="d4386-273">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="d4386-274">Dodaj właściwość publiczną dla `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d4386-274">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="d4386-275">Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d4386-275">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="d4386-276">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d4386-276">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="d4386-277">Określ `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d4386-277">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="d4386-278">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d4386-278">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="d4386-279">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d4386-279">Run the app.</span></span> <span data-ttu-id="d4386-280">Składnik `Index` ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="d4386-280">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d4386-281">Składnik `Counter` (*Counter. Razor*) w `/counter` nadal zwiększa się o jeden.</span><span class="sxs-lookup"><span data-stu-id="d4386-281">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4386-282">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d4386-282">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="d4386-283">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d4386-283">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
