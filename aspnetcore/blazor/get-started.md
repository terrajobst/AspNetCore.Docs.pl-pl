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
ms.openlocfilehash: 2135c2a090d60ec7a46fa4f899f0f14987b6b4e0
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/14/2020
ms.locfileid: "75951724"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="d7436-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="d7436-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="d7436-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d7436-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="d7436-105">Rozpocznij pracę z Blazor:</span><span class="sxs-lookup"><span data-stu-id="d7436-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="d7436-106">Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d7436-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="d7436-107">Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="d7436-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="d7436-108">Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d7436-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="d7436-109">Uruchom następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="d7436-109">Run the following command in a command shell.</span></span> <span data-ttu-id="d7436-110">[Microsoft.AspNetCore.Blazor.Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="d7436-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="d7436-111">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="d7436-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7436-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7436-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d7436-113">1\.</span><span class="sxs-lookup"><span data-stu-id="d7436-113">1\.</span></span> <span data-ttu-id="d7436-114">Zainstaluj [program Visual Studio 16,4 lub nowszy](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="d7436-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d7436-115">2\.</span><span class="sxs-lookup"><span data-stu-id="d7436-115">2\.</span></span> <span data-ttu-id="d7436-116">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="d7436-116">Create a new project.</span></span>

   <span data-ttu-id="d7436-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-117">3\.</span></span> <span data-ttu-id="d7436-118">Wybierz pozycję **Blazor aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="d7436-118">Select **Blazor App**.</span></span> <span data-ttu-id="d7436-119">Wybierz pozycję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="d7436-119">Select **Next**.</span></span>

   <span data-ttu-id="d7436-120">4\.</span><span class="sxs-lookup"><span data-stu-id="d7436-120">4\.</span></span> <span data-ttu-id="d7436-121">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="d7436-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d7436-122">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="d7436-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d7436-123">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d7436-123">Select **Create**.</span></span>

   <span data-ttu-id="d7436-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-124">5\.</span></span> <span data-ttu-id="d7436-125">W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="d7436-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d7436-126">W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="d7436-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d7436-127">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d7436-127">Select **Create**.</span></span> <span data-ttu-id="d7436-128">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d7436-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d7436-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-129">6\.</span></span> <span data-ttu-id="d7436-130">Naciśnij klawisz **Ctrl** ,+**F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d7436-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d7436-131">Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="d7436-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d7436-132">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d7436-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7436-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7436-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d7436-134">1\.</span><span class="sxs-lookup"><span data-stu-id="d7436-134">1\.</span></span> <span data-ttu-id="d7436-135">Zainstalowanie programu [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d7436-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d7436-136">2\.</span><span class="sxs-lookup"><span data-stu-id="d7436-136">2\.</span></span> <span data-ttu-id="d7436-137">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="d7436-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d7436-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-138">3\.</span></span> <span data-ttu-id="d7436-139">W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d7436-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d7436-140">W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d7436-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d7436-141">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d7436-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d7436-142">4\.</span><span class="sxs-lookup"><span data-stu-id="d7436-142">4\.</span></span> <span data-ttu-id="d7436-143">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7436-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d7436-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-144">5\.</span></span> <span data-ttu-id="d7436-145">Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="d7436-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d7436-146">Wybierz pozycję **Yes**.</span><span class="sxs-lookup"><span data-stu-id="d7436-146">Select **Yes**.</span></span>

   <span data-ttu-id="d7436-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-147">6\.</span></span> <span data-ttu-id="d7436-148">Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7436-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d7436-149">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d7436-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d7436-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-150">7\.</span></span> <span data-ttu-id="d7436-151">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d7436-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7436-152">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d7436-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="d7436-153">1\.</span><span class="sxs-lookup"><span data-stu-id="d7436-153">1\.</span></span> <span data-ttu-id="d7436-154">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="d7436-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="d7436-155">2\.</span><span class="sxs-lookup"><span data-stu-id="d7436-155">2\.</span></span> <span data-ttu-id="d7436-156">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="d7436-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="d7436-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-157">3\.</span></span> <span data-ttu-id="d7436-158">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="d7436-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="d7436-159">4\.</span><span class="sxs-lookup"><span data-stu-id="d7436-159">4\.</span></span> <span data-ttu-id="d7436-160">Wybierz szablon **aplikacjiBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="d7436-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="d7436-161">W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d7436-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="d7436-162">W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d7436-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="d7436-163">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d7436-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="d7436-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-164">5\.</span></span> <span data-ttu-id="d7436-165">Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d7436-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="d7436-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-166">6\.</span></span> <span data-ttu-id="d7436-167">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="d7436-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="d7436-168">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d7436-168">Select **Create**.</span></span>

   <span data-ttu-id="d7436-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-169">7\.</span></span> <span data-ttu-id="d7436-170">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="d7436-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="d7436-171">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="d7436-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="d7436-172">Jeśli zostanie wyświetlony monit o zaufać certyfikatowi Deweloperskiemu, zaufaj certyfikatowi i Kontynuuj.</span><span class="sxs-lookup"><span data-stu-id="d7436-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d7436-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d7436-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d7436-174">W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d7436-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d7436-175">W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d7436-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d7436-176">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d7436-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d7436-177">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d7436-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="d7436-178">Zainstaluj najnowszą wersję [zestawu SDK platformy .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="d7436-178">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="d7436-179">Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="d7436-179">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="d7436-180">Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d7436-180">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="d7436-181">Uruchom następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="d7436-181">Run the following command in a command shell.</span></span> <span data-ttu-id="d7436-182">[Microsoft.AspNetCore.Blazor.Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="d7436-182">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="d7436-183">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="d7436-183">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7436-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7436-184">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d7436-185">1\.</span><span class="sxs-lookup"><span data-stu-id="d7436-185">1\.</span></span> <span data-ttu-id="d7436-186">Zainstaluj najnowszą wersję [programu Visual Studio](https://visualstudio.com/vs/) , korzystając z obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="d7436-186">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d7436-187">2\.</span><span class="sxs-lookup"><span data-stu-id="d7436-187">2\.</span></span> <span data-ttu-id="d7436-188">Opcjonalnie zainstaluj [program Visual Studio 16,4 w wersji zapoznawczej 2 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** Blazor dla tworzenia aplikacji webassembly.</span><span class="sxs-lookup"><span data-stu-id="d7436-188">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="d7436-189">3 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-189">3\.</span></span> <span data-ttu-id="d7436-190">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="d7436-190">Create a new project.</span></span>

   <span data-ttu-id="d7436-191">4\.</span><span class="sxs-lookup"><span data-stu-id="d7436-191">4\.</span></span> <span data-ttu-id="d7436-192">Wybierz pozycję **Blazor aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="d7436-192">Select **Blazor App**.</span></span> <span data-ttu-id="d7436-193">Wybierz pozycję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="d7436-193">Select **Next**.</span></span>

   <span data-ttu-id="d7436-194">5 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-194">5\.</span></span> <span data-ttu-id="d7436-195">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="d7436-195">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d7436-196">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="d7436-196">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d7436-197">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d7436-197">Select **Create**.</span></span>

   <span data-ttu-id="d7436-198">6 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-198">6\.</span></span> <span data-ttu-id="d7436-199">W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="d7436-199">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d7436-200">W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="d7436-200">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d7436-201">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d7436-201">Select **Create**.</span></span> <span data-ttu-id="d7436-202">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d7436-202">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d7436-203">7 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-203">7\.</span></span> <span data-ttu-id="d7436-204">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d7436-204">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d7436-205">Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="d7436-205">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d7436-206">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d7436-206">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7436-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7436-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d7436-208">1\.</span><span class="sxs-lookup"><span data-stu-id="d7436-208">1\.</span></span> <span data-ttu-id="d7436-209">Zainstalowanie programu [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d7436-209">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d7436-210">2\.</span><span class="sxs-lookup"><span data-stu-id="d7436-210">2\.</span></span> <span data-ttu-id="d7436-211">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="d7436-211">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d7436-212">3 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-212">3\.</span></span> <span data-ttu-id="d7436-213">W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d7436-213">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d7436-214">W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d7436-214">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d7436-215">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d7436-215">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d7436-216">4\.</span><span class="sxs-lookup"><span data-stu-id="d7436-216">4\.</span></span> <span data-ttu-id="d7436-217">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7436-217">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d7436-218">5 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-218">5\.</span></span> <span data-ttu-id="d7436-219">Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="d7436-219">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d7436-220">Wybierz pozycję **Yes**.</span><span class="sxs-lookup"><span data-stu-id="d7436-220">Select **Yes**.</span></span>

   <span data-ttu-id="d7436-221">6 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-221">6\.</span></span> <span data-ttu-id="d7436-222">Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7436-222">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d7436-223">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d7436-223">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d7436-224">7 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-224">7\.</span></span> <span data-ttu-id="d7436-225">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d7436-225">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7436-226">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d7436-226">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="d7436-227">1\.</span><span class="sxs-lookup"><span data-stu-id="d7436-227">1\.</span></span> <span data-ttu-id="d7436-228">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="d7436-228">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="d7436-229">Przełącz [kanał aktualizacji na wersję zapoznawczą](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="d7436-229">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="d7436-230">2\.</span><span class="sxs-lookup"><span data-stu-id="d7436-230">2\.</span></span> <span data-ttu-id="d7436-231">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="d7436-231">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="d7436-232">3 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-232">3\.</span></span> <span data-ttu-id="d7436-233">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="d7436-233">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="d7436-234">4\.</span><span class="sxs-lookup"><span data-stu-id="d7436-234">4\.</span></span> <span data-ttu-id="d7436-235">Wybierz szablon **aplikacjiBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="d7436-235">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="d7436-236">W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d7436-236">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="d7436-237">W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d7436-237">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="d7436-238">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d7436-238">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="d7436-239">5 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-239">5\.</span></span> <span data-ttu-id="d7436-240">Ustaw platformę **docelową** na **platformę .NET Core 3,0** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d7436-240">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="d7436-241">6 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-241">6\.</span></span> <span data-ttu-id="d7436-242">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="d7436-242">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="d7436-243">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d7436-243">Select **Create**.</span></span>

   <span data-ttu-id="d7436-244">7 \.</span><span class="sxs-lookup"><span data-stu-id="d7436-244">7\.</span></span> <span data-ttu-id="d7436-245">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="d7436-245">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="d7436-246">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="d7436-246">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d7436-247">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d7436-247">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d7436-248">W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d7436-248">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d7436-249">W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d7436-249">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d7436-250">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d7436-250">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d7436-251">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d7436-251">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="d7436-252">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="d7436-252">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="d7436-253">Strona główna programu</span><span class="sxs-lookup"><span data-stu-id="d7436-253">Home</span></span>
* <span data-ttu-id="d7436-254">Licznik</span><span class="sxs-lookup"><span data-stu-id="d7436-254">Counter</span></span>
* <span data-ttu-id="d7436-255">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="d7436-255">Fetch data</span></span>

<span data-ttu-id="d7436-256">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="d7436-256">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d7436-257">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale z Blazor można użyć C#.</span><span class="sxs-lookup"><span data-stu-id="d7436-257">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="d7436-258">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d7436-258">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="d7436-259">Żądanie `/counter` w przeglądarce, zgodnie z dyrektywą `@page` w górnej części, powoduje, że składnik `Counter` renderuje jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="d7436-259">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="d7436-260">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="d7436-260">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="d7436-261">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="d7436-261">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="d7436-262">Zdarzenie `onclick` jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="d7436-262">The `onclick` event is fired.</span></span>
* <span data-ttu-id="d7436-263">Metoda `IncrementCount` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="d7436-263">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="d7436-264">`currentCount` jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="d7436-264">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="d7436-265">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="d7436-265">The component is rendered again.</span></span>

<span data-ttu-id="d7436-266">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="d7436-266">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="d7436-267">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="d7436-267">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="d7436-268">Na przykład Dodaj składnik `Counter` do strony głównej aplikacji przez dodanie elementu `<Counter />` do składnika `Index`.</span><span class="sxs-lookup"><span data-stu-id="d7436-268">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="d7436-269">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d7436-269">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="d7436-270">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d7436-270">Run the app.</span></span> <span data-ttu-id="d7436-271">Strona główna ma swój własny licznik dostarczony przez składnik `Counter`.</span><span class="sxs-lookup"><span data-stu-id="d7436-271">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="d7436-272">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d7436-272">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="d7436-273">Aby dodać parametr do składnika `Counter`, zaktualizuj blok `@code` składnika:</span><span class="sxs-lookup"><span data-stu-id="d7436-273">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="d7436-274">Dodaj właściwość publiczną dla `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d7436-274">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="d7436-275">Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d7436-275">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="d7436-276">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d7436-276">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="d7436-277">Określ `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d7436-277">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="d7436-278">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d7436-278">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="d7436-279">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d7436-279">Run the app.</span></span> <span data-ttu-id="d7436-280">Składnik `Index` ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="d7436-280">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d7436-281">Składnik `Counter` (*Counter. Razor*) w `/counter` nadal zwiększa się o jeden.</span><span class="sxs-lookup"><span data-stu-id="d7436-281">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7436-282">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d7436-282">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="d7436-283">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d7436-283">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
