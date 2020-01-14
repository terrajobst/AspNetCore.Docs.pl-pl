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
ms.openlocfilehash: 554f4daff92a0839ee7679287a4618e9b51e0fe5
ms.sourcegitcommit: 925cdbd94613243f33bc7613a62ea34006219931
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921306"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="34e11-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="34e11-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="34e11-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="34e11-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="34e11-105">Rozpocznij pracę z Blazor:</span><span class="sxs-lookup"><span data-stu-id="34e11-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="34e11-106">Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="34e11-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="34e11-107">Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="34e11-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="34e11-108">Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="34e11-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="34e11-109">Uruchom następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="34e11-109">Run the following command in a command shell.</span></span> <span data-ttu-id="34e11-110">[Microsoft.AspNetCore.Blazor.Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.
</span><span class="sxs-lookup"><span data-stu-id="34e11-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="34e11-111">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="34e11-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e11-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e11-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="34e11-113">1\.</span><span class="sxs-lookup"><span data-stu-id="34e11-113">1\.</span></span> <span data-ttu-id="34e11-114">Zainstaluj [program Visual Studio 16,4 lub nowszy](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="34e11-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="34e11-115">2\.</span><span class="sxs-lookup"><span data-stu-id="34e11-115">2\.</span></span> <span data-ttu-id="34e11-116">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="34e11-116">Create a new project.</span></span>

   <span data-ttu-id="34e11-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-117">3\.</span></span> <span data-ttu-id="34e11-118">Wybierz pozycję **Blazor aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="34e11-118">Select **Blazor App**.</span></span> <span data-ttu-id="34e11-119">Wybierz pozycję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="34e11-119">Select **Next**.</span></span>

   <span data-ttu-id="34e11-120">4\.</span><span class="sxs-lookup"><span data-stu-id="34e11-120">4\.</span></span> <span data-ttu-id="34e11-121">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="34e11-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="34e11-122">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="34e11-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="34e11-123">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="34e11-123">Select **Create**.</span></span>

   <span data-ttu-id="34e11-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-124">5\.</span></span> <span data-ttu-id="34e11-125">W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="34e11-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="34e11-126">W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="34e11-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="34e11-127">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="34e11-127">Select **Create**.</span></span> <span data-ttu-id="34e11-128">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="34e11-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="34e11-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-129">6\.</span></span> <span data-ttu-id="34e11-130">Naciśnij klawisz **Ctrl** ,+**F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="34e11-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="34e11-131">Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="34e11-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="34e11-132">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34e11-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e11-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e11-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="34e11-134">1\.</span><span class="sxs-lookup"><span data-stu-id="34e11-134">1\.</span></span> <span data-ttu-id="34e11-135">Zainstalowanie programu [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="34e11-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="34e11-136">2\.</span><span class="sxs-lookup"><span data-stu-id="34e11-136">2\.</span></span> <span data-ttu-id="34e11-137">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="34e11-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="34e11-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-138">3\.</span></span> <span data-ttu-id="34e11-139">W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="34e11-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="34e11-140">W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="34e11-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="34e11-141">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="34e11-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="34e11-142">4\.</span><span class="sxs-lookup"><span data-stu-id="34e11-142">4\.</span></span> <span data-ttu-id="34e11-143">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="34e11-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="34e11-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-144">5\.</span></span> <span data-ttu-id="34e11-145">Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="34e11-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="34e11-146">Wybierz pozycję **Yes**.</span><span class="sxs-lookup"><span data-stu-id="34e11-146">Select **Yes**.</span></span>

   <span data-ttu-id="34e11-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-147">6\.</span></span> <span data-ttu-id="34e11-148">Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="34e11-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="34e11-149">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="34e11-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="34e11-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-150">7\.</span></span> <span data-ttu-id="34e11-151">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="34e11-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="34e11-152">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="34e11-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="34e11-153">1\.</span><span class="sxs-lookup"><span data-stu-id="34e11-153">1\.</span></span> <span data-ttu-id="34e11-154">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="34e11-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="34e11-155">2\.</span><span class="sxs-lookup"><span data-stu-id="34e11-155">2\.</span></span> <span data-ttu-id="34e11-156">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="34e11-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="34e11-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-157">3\.</span></span> <span data-ttu-id="34e11-158">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="34e11-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="34e11-159">4\.</span><span class="sxs-lookup"><span data-stu-id="34e11-159">4\.</span></span> <span data-ttu-id="34e11-160">Wybierz szablon **aplikacjiBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="34e11-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="34e11-161">W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="34e11-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="34e11-162">W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="34e11-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="34e11-163">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="34e11-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="34e11-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-164">5\.</span></span> <span data-ttu-id="34e11-165">Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="34e11-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="34e11-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-166">6\.</span></span> <span data-ttu-id="34e11-167">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="34e11-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="34e11-168">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="34e11-168">Select **Create**.</span></span>

   <span data-ttu-id="34e11-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-169">7\.</span></span> <span data-ttu-id="34e11-170">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="34e11-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="34e11-171">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="34e11-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="34e11-172">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="34e11-172">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="34e11-173">W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="34e11-173">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="34e11-174">W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="34e11-174">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="34e11-175">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="34e11-175">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="34e11-176">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="34e11-176">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="34e11-177">Zainstaluj najnowszą wersję [zestawu SDK platformy .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="34e11-177">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="34e11-178">Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="34e11-178">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="34e11-179">Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="34e11-179">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="34e11-180">Uruchom następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="34e11-180">Run the following command in a command shell.</span></span> <span data-ttu-id="34e11-181">[Microsoft.AspNetCore.Blazor.Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="34e11-181">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="34e11-182">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="34e11-182">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e11-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e11-183">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="34e11-184">1\.</span><span class="sxs-lookup"><span data-stu-id="34e11-184">1\.</span></span> <span data-ttu-id="34e11-185">Zainstaluj najnowszą wersję [programu Visual Studio](https://visualstudio.com/vs/) , korzystając z obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="34e11-185">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="34e11-186">2\.</span><span class="sxs-lookup"><span data-stu-id="34e11-186">2\.</span></span> <span data-ttu-id="34e11-187">Opcjonalnie zainstaluj [program Visual Studio 16,4 w wersji zapoznawczej 2 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** Blazor dla tworzenia aplikacji webassembly.</span><span class="sxs-lookup"><span data-stu-id="34e11-187">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="34e11-188">3 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-188">3\.</span></span> <span data-ttu-id="34e11-189">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="34e11-189">Create a new project.</span></span>

   <span data-ttu-id="34e11-190">4\.</span><span class="sxs-lookup"><span data-stu-id="34e11-190">4\.</span></span> <span data-ttu-id="34e11-191">Wybierz pozycję **Blazor aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="34e11-191">Select **Blazor App**.</span></span> <span data-ttu-id="34e11-192">Wybierz pozycję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="34e11-192">Select **Next**.</span></span>

   <span data-ttu-id="34e11-193">5 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-193">5\.</span></span> <span data-ttu-id="34e11-194">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="34e11-194">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="34e11-195">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="34e11-195">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="34e11-196">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="34e11-196">Select **Create**.</span></span>

   <span data-ttu-id="34e11-197">6 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-197">6\.</span></span> <span data-ttu-id="34e11-198">W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="34e11-198">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="34e11-199">W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="34e11-199">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="34e11-200">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="34e11-200">Select **Create**.</span></span> <span data-ttu-id="34e11-201">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="34e11-201">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="34e11-202">7 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-202">7\.</span></span> <span data-ttu-id="34e11-203">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="34e11-203">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="34e11-204">Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="34e11-204">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="34e11-205">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34e11-205">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e11-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e11-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="34e11-207">1\.</span><span class="sxs-lookup"><span data-stu-id="34e11-207">1\.</span></span> <span data-ttu-id="34e11-208">Zainstalowanie programu [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="34e11-208">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="34e11-209">2\.</span><span class="sxs-lookup"><span data-stu-id="34e11-209">2\.</span></span> <span data-ttu-id="34e11-210">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="34e11-210">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="34e11-211">3 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-211">3\.</span></span> <span data-ttu-id="34e11-212">W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="34e11-212">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="34e11-213">W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="34e11-213">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="34e11-214">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="34e11-214">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="34e11-215">4\.</span><span class="sxs-lookup"><span data-stu-id="34e11-215">4\.</span></span> <span data-ttu-id="34e11-216">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="34e11-216">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="34e11-217">5 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-217">5\.</span></span> <span data-ttu-id="34e11-218">Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="34e11-218">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="34e11-219">Wybierz pozycję **Yes**.</span><span class="sxs-lookup"><span data-stu-id="34e11-219">Select **Yes**.</span></span>

   <span data-ttu-id="34e11-220">6 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-220">6\.</span></span> <span data-ttu-id="34e11-221">Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="34e11-221">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="34e11-222">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="34e11-222">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="34e11-223">7 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-223">7\.</span></span> <span data-ttu-id="34e11-224">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="34e11-224">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="34e11-225">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="34e11-225">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="34e11-226">1\.</span><span class="sxs-lookup"><span data-stu-id="34e11-226">1\.</span></span> <span data-ttu-id="34e11-227">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="34e11-227">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="34e11-228">Przełącz [kanał aktualizacji na wersję zapoznawczą](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="34e11-228">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="34e11-229">2\.</span><span class="sxs-lookup"><span data-stu-id="34e11-229">2\.</span></span> <span data-ttu-id="34e11-230">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="34e11-230">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="34e11-231">3 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-231">3\.</span></span> <span data-ttu-id="34e11-232">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="34e11-232">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="34e11-233">4\.</span><span class="sxs-lookup"><span data-stu-id="34e11-233">4\.</span></span> <span data-ttu-id="34e11-234">Wybierz szablon **aplikacjiBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="34e11-234">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="34e11-235">W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="34e11-235">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="34e11-236">W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="34e11-236">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="34e11-237">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="34e11-237">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="34e11-238">5 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-238">5\.</span></span> <span data-ttu-id="34e11-239">Ustaw platformę **docelową** na **platformę .NET Core 3,0** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="34e11-239">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="34e11-240">6 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-240">6\.</span></span> <span data-ttu-id="34e11-241">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="34e11-241">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="34e11-242">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="34e11-242">Select **Create**.</span></span>

   <span data-ttu-id="34e11-243">7 \.</span><span class="sxs-lookup"><span data-stu-id="34e11-243">7\.</span></span> <span data-ttu-id="34e11-244">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="34e11-244">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="34e11-245">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="34e11-245">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="34e11-246">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="34e11-246">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="34e11-247">W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="34e11-247">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="34e11-248">W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="34e11-248">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="34e11-249">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="34e11-249">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="34e11-250">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="34e11-250">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="34e11-251">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="34e11-251">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="34e11-252">Strona główna programu</span><span class="sxs-lookup"><span data-stu-id="34e11-252">Home</span></span>
* <span data-ttu-id="34e11-253">Licznik</span><span class="sxs-lookup"><span data-stu-id="34e11-253">Counter</span></span>
* <span data-ttu-id="34e11-254">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="34e11-254">Fetch data</span></span>

<span data-ttu-id="34e11-255">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="34e11-255">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="34e11-256">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale z Blazor można użyć C#.</span><span class="sxs-lookup"><span data-stu-id="34e11-256">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="34e11-257">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="34e11-257">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="34e11-258">Żądanie `/counter` w przeglądarce, zgodnie z dyrektywą `@page` w górnej części, powoduje, że składnik `Counter` renderuje jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="34e11-258">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="34e11-259">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="34e11-259">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="34e11-260">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="34e11-260">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="34e11-261">Zdarzenie `onclick` jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="34e11-261">The `onclick` event is fired.</span></span>
* <span data-ttu-id="34e11-262">Metoda `IncrementCount` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="34e11-262">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="34e11-263">`currentCount` jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="34e11-263">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="34e11-264">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="34e11-264">The component is rendered again.</span></span>

<span data-ttu-id="34e11-265">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="34e11-265">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="34e11-266">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="34e11-266">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="34e11-267">Na przykład Dodaj składnik `Counter` do strony głównej aplikacji przez dodanie elementu `<Counter />` do składnika `Index`.</span><span class="sxs-lookup"><span data-stu-id="34e11-267">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="34e11-268">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="34e11-268">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="34e11-269">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="34e11-269">Run the app.</span></span> <span data-ttu-id="34e11-270">Strona główna ma swój własny licznik dostarczony przez składnik `Counter`.</span><span class="sxs-lookup"><span data-stu-id="34e11-270">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="34e11-271">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="34e11-271">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="34e11-272">Aby dodać parametr do składnika `Counter`, zaktualizuj blok `@code` składnika:</span><span class="sxs-lookup"><span data-stu-id="34e11-272">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="34e11-273">Dodaj właściwość publiczną dla `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="34e11-273">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="34e11-274">Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="34e11-274">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="34e11-275">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="34e11-275">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="34e11-276">Określ `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="34e11-276">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="34e11-277">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="34e11-277">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="34e11-278">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="34e11-278">Run the app.</span></span> <span data-ttu-id="34e11-279">Składnik `Index` ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="34e11-279">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="34e11-280">Składnik `Counter` (*Counter. Razor*) w `/counter` nadal zwiększa się o jeden.</span><span class="sxs-lookup"><span data-stu-id="34e11-280">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34e11-281">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="34e11-281">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="34e11-282">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="34e11-282">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
