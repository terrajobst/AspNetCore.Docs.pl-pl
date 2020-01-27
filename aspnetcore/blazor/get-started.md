---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z Blazor, tworząc aplikację Blazor za pomocą wybranego przez siebie narzędzia.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 642881b5400a70a99f6e7e262d2a2f1038389ce7
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726844"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="2f4df-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="2f4df-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="2f4df-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2f4df-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2f4df-105">Rozpocznij pracę z Blazor:</span><span class="sxs-lookup"><span data-stu-id="2f4df-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="2f4df-106">Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="2f4df-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="2f4df-107">Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="2f4df-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="2f4df-108">Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="2f4df-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="2f4df-109">Uruchom następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="2f4df-109">Run the following command in a command shell.</span></span> <span data-ttu-id="2f4df-110">[Microsoft. AspNetCore.Blazor. Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="2f4df-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="2f4df-111">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="2f4df-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f4df-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f4df-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="2f4df-113">1\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-113">1\.</span></span> <span data-ttu-id="2f4df-114">Zainstaluj [program Visual Studio 2019 w wersji 16,4 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="2f4df-114">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="2f4df-115">2\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-115">2\.</span></span> <span data-ttu-id="2f4df-116">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="2f4df-116">Create a new project.</span></span>

   <span data-ttu-id="2f4df-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-117">3\.</span></span> <span data-ttu-id="2f4df-118">Wybierz pozycję **Blazor aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-118">Select **Blazor App**.</span></span> <span data-ttu-id="2f4df-119">Wybierz pozycję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-119">Select **Next**.</span></span>

   <span data-ttu-id="2f4df-120">4\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-120">4\.</span></span> <span data-ttu-id="2f4df-121">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="2f4df-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="2f4df-122">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="2f4df-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2f4df-123">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-123">Select **Create**.</span></span>

   <span data-ttu-id="2f4df-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-124">5\.</span></span> <span data-ttu-id="2f4df-125">W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="2f4df-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="2f4df-126">W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="2f4df-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="2f4df-127">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-127">Select **Create**.</span></span> <span data-ttu-id="2f4df-128">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2f4df-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2f4df-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-129">6\.</span></span> <span data-ttu-id="2f4df-130">Naciśnij klawisz **Ctrl** ,+**F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="2f4df-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2f4df-131">Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="2f4df-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="2f4df-132">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f4df-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2f4df-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2f4df-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="2f4df-134">1\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-134">1\.</span></span> <span data-ttu-id="2f4df-135">Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="2f4df-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="2f4df-136">2\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-136">2\.</span></span> <span data-ttu-id="2f4df-137">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="2f4df-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="2f4df-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-138">3\.</span></span> <span data-ttu-id="2f4df-139">W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="2f4df-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="2f4df-140">W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="2f4df-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="2f4df-141">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2f4df-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2f4df-142">4\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-142">4\.</span></span> <span data-ttu-id="2f4df-143">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2f4df-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="2f4df-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-144">5\.</span></span> <span data-ttu-id="2f4df-145">Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="2f4df-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="2f4df-146">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-146">Select **Yes**.</span></span>

   <span data-ttu-id="2f4df-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-147">6\.</span></span> <span data-ttu-id="2f4df-148">Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2f4df-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="2f4df-149">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f4df-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="2f4df-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-150">7\.</span></span> <span data-ttu-id="2f4df-151">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2f4df-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2f4df-152">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2f4df-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="2f4df-153">1\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-153">1\.</span></span> <span data-ttu-id="2f4df-154">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="2f4df-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="2f4df-155">2\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-155">2\.</span></span> <span data-ttu-id="2f4df-156">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="2f4df-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-157">3\.</span></span> <span data-ttu-id="2f4df-158">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="2f4df-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="2f4df-159">4\.</span><span class="sxs-lookup"><span data-stu-id="2f4df-159">4\.</span></span> <span data-ttu-id="2f4df-160">Wybierz szablon **aplikacjiBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="2f4df-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="2f4df-161">W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="2f4df-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="2f4df-162">W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="2f4df-163">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2f4df-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="2f4df-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-164">5\.</span></span> <span data-ttu-id="2f4df-165">Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="2f4df-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-166">6\.</span></span> <span data-ttu-id="2f4df-167">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="2f4df-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="2f4df-168">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2f4df-168">Select **Create**.</span></span>

   <span data-ttu-id="2f4df-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="2f4df-169">7\.</span></span> <span data-ttu-id="2f4df-170">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="2f4df-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="2f4df-171">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="2f4df-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="2f4df-172">Jeśli zostanie wyświetlony monit o zaufać certyfikatowi Deweloperskiemu, zaufaj certyfikatowi i Kontynuuj.</span><span class="sxs-lookup"><span data-stu-id="2f4df-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f4df-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2f4df-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="2f4df-174">W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="2f4df-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2f4df-175">W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="2f4df-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2f4df-176">Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2f4df-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2f4df-177">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2f4df-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="2f4df-178">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="2f4df-178">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="2f4df-179">Strona główna programu</span><span class="sxs-lookup"><span data-stu-id="2f4df-179">Home</span></span>
* <span data-ttu-id="2f4df-180">Licznik</span><span class="sxs-lookup"><span data-stu-id="2f4df-180">Counter</span></span>
* <span data-ttu-id="2f4df-181">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="2f4df-181">Fetch data</span></span>

<span data-ttu-id="2f4df-182">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="2f4df-182">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="2f4df-183">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale z Blazor można użyć C#.</span><span class="sxs-lookup"><span data-stu-id="2f4df-183">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="2f4df-184">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2f4df-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="2f4df-185">Żądanie `/counter` w przeglądarce, zgodnie z dyrektywą `@page` w górnej części, powoduje, że składnik `Counter` renderuje jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="2f4df-185">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="2f4df-186">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="2f4df-186">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="2f4df-187">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="2f4df-187">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="2f4df-188">Zdarzenie `onclick` jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="2f4df-188">The `onclick` event is fired.</span></span>
* <span data-ttu-id="2f4df-189">Metoda `IncrementCount` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="2f4df-189">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="2f4df-190">`currentCount` jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="2f4df-190">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="2f4df-191">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="2f4df-191">The component is rendered again.</span></span>

<span data-ttu-id="2f4df-192">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="2f4df-192">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="2f4df-193">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="2f4df-193">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="2f4df-194">Na przykład Dodaj składnik `Counter` do strony głównej aplikacji przez dodanie elementu `<Counter />` do składnika `Index`.</span><span class="sxs-lookup"><span data-stu-id="2f4df-194">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="2f4df-195">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2f4df-195">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="2f4df-196">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2f4df-196">Run the app.</span></span> <span data-ttu-id="2f4df-197">Strona główna ma swój własny licznik dostarczony przez składnik `Counter`.</span><span class="sxs-lookup"><span data-stu-id="2f4df-197">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="2f4df-198">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="2f4df-198">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="2f4df-199">Aby dodać parametr do składnika `Counter`, zaktualizuj blok `@code` składnika:</span><span class="sxs-lookup"><span data-stu-id="2f4df-199">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="2f4df-200">Dodaj właściwość publiczną dla `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="2f4df-200">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="2f4df-201">Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="2f4df-201">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="2f4df-202">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2f4df-202">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="2f4df-203">Określ `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2f4df-203">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="2f4df-204">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2f4df-204">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="2f4df-205">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2f4df-205">Run the app.</span></span> <span data-ttu-id="2f4df-206">Składnik `Index` ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="2f4df-206">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="2f4df-207">Składnik `Counter` (*Counter. Razor*) w `/counter` nadal zwiększa się o jeden.</span><span class="sxs-lookup"><span data-stu-id="2f4df-207">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f4df-208">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2f4df-208">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="2f4df-209">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2f4df-209">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
