---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z Blazor, tworząc aplikację Blazor za pomocą wybranego przez siebie narzędzia.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083238"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="227ab-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="227ab-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="227ab-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="227ab-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="227ab-105">Rozpocznij pracę z usługą Blazor:</span><span class="sxs-lookup"><span data-stu-id="227ab-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="227ab-106">Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="227ab-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="227ab-107">Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="227ab-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="227ab-108">Zainstaluj [zestaw SDK platformy .NET Core 3.1.102 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="227ab-108">Install the [.NET Core 3.1.102 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="227ab-109">Uruchom następujące polecenie w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="227ab-109">Run the following command in a command shell.</span></span> <span data-ttu-id="227ab-110">Pakiet [Microsoft. AspNetCore. Components. webassembly. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="227ab-110">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > <span data-ttu-id="227ab-111">Aby można było użyć szablonu zestawu webassembly 3,2 w wersji 2 Blazor, **wymagany** jest zestaw .NET Core SDK wersja 3.1.102 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="227ab-111">.NET Core SDK version 3.1.102 or later is **required** to use the 3.2 Preview 2 Blazor WebAssembly template.</span></span> <span data-ttu-id="227ab-112">Potwierdź zainstalowaną zestaw .NET Core SDK wersję, uruchamiając `dotnet --version` w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="227ab-112">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="227ab-113">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="227ab-113">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studio"></a>[<span data-ttu-id="227ab-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="227ab-114">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="227ab-115">1\.</span><span class="sxs-lookup"><span data-stu-id="227ab-115">1\.</span></span> <span data-ttu-id="227ab-116">Zainstaluj [program Visual Studio 2019 w wersji 16,4 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="227ab-116">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="227ab-117">2\.</span><span class="sxs-lookup"><span data-stu-id="227ab-117">2\.</span></span> <span data-ttu-id="227ab-118">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="227ab-118">Create a new project.</span></span>

   <span data-ttu-id="227ab-119">3 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-119">3\.</span></span> <span data-ttu-id="227ab-120">Wybierz pozycję **aplikacja Blazor**.</span><span class="sxs-lookup"><span data-stu-id="227ab-120">Select **Blazor App**.</span></span> <span data-ttu-id="227ab-121">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="227ab-121">Select **Next**.</span></span>

   <span data-ttu-id="227ab-122">4\.</span><span class="sxs-lookup"><span data-stu-id="227ab-122">4\.</span></span> <span data-ttu-id="227ab-123">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="227ab-123">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="227ab-124">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="227ab-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="227ab-125">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="227ab-125">Select **Create**.</span></span>

   <span data-ttu-id="227ab-126">5 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-126">5\.</span></span> <span data-ttu-id="227ab-127">Aby zapoznać się z Blazor webassembly, wybierz szablon **aplikacji Blazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="227ab-127">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="227ab-128">Dla środowiska serwera Blazor wybierz szablon **aplikacji Blazor Server** .</span><span class="sxs-lookup"><span data-stu-id="227ab-128">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="227ab-129">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="227ab-129">Select **Create**.</span></span> <span data-ttu-id="227ab-130">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="227ab-130">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span> <span data-ttu-id="227ab-131">Jeśli szablon Blazor webassembly nie istnieje, Wróć do poprzedniego kroku i ponownie zainstaluj szablon.</span><span class="sxs-lookup"><span data-stu-id="227ab-131">If the Blazor WebAssembly template isn't present, return to the previous step and reinstall the template.</span></span>

   <span data-ttu-id="227ab-132">6 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-132">6\.</span></span> <span data-ttu-id="227ab-133">Naciśnij klawisz **Ctrl** ,+**F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="227ab-133">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="227ab-134">Jeśli zainstalowano rozszerzenie Blazor programu Visual Studio dla starszej wersji zapoznawczej programu ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="227ab-134">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="227ab-135">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="227ab-135">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-code"></a>[<span data-ttu-id="227ab-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="227ab-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="227ab-137">1\.</span><span class="sxs-lookup"><span data-stu-id="227ab-137">1\.</span></span> <span data-ttu-id="227ab-138">Zainstaluj narzędzie [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="227ab-138">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="227ab-139">2\.</span><span class="sxs-lookup"><span data-stu-id="227ab-139">2\.</span></span> <span data-ttu-id="227ab-140">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span><span class="sxs-lookup"><span data-stu-id="227ab-140">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

   <span data-ttu-id="227ab-141">3 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-141">3\.</span></span> <span data-ttu-id="227ab-142">W przypadku środowiska webassembly Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="227ab-142">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="227ab-143">W przypadku środowiska serwera Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="227ab-143">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="227ab-144">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="227ab-144">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="227ab-145">4\.</span><span class="sxs-lookup"><span data-stu-id="227ab-145">4\.</span></span> <span data-ttu-id="227ab-146">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="227ab-146">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="227ab-147">5 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-147">5\.</span></span> <span data-ttu-id="227ab-148">W przypadku projektu serwera Blazor, IDE żąda dodania zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="227ab-148">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="227ab-149">Wybierz pozycję **Tak**.</span><span class="sxs-lookup"><span data-stu-id="227ab-149">Select **Yes**.</span></span>

   <span data-ttu-id="227ab-150">6 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-150">6\.</span></span> <span data-ttu-id="227ab-151">W przypadku korzystania z aplikacji serwera Blazor należy uruchomić aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="227ab-151">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="227ab-152">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="227ab-152">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="227ab-153">7 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-153">7\.</span></span> <span data-ttu-id="227ab-154">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="227ab-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mac"></a>[<span data-ttu-id="227ab-155">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="227ab-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="227ab-156">1\.</span><span class="sxs-lookup"><span data-stu-id="227ab-156">1\.</span></span> <span data-ttu-id="227ab-157">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="227ab-157">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="227ab-158">2\.</span><span class="sxs-lookup"><span data-stu-id="227ab-158">2\.</span></span> <span data-ttu-id="227ab-159">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="227ab-159">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="227ab-160">3 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-160">3\.</span></span> <span data-ttu-id="227ab-161">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="227ab-161">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="227ab-162">4\.</span><span class="sxs-lookup"><span data-stu-id="227ab-162">4\.</span></span> <span data-ttu-id="227ab-163">Wybierz szablon **aplikacji Blazor Server** .</span><span class="sxs-lookup"><span data-stu-id="227ab-163">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="227ab-164">W Visual Studio dla komputerów Mac jest dostępny tylko szablon serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="227ab-164">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="227ab-165">W przypadku środowiska webassembly Blazor postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="227ab-165">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="227ab-166">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="227ab-166">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="227ab-167">5 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-167">5\.</span></span> <span data-ttu-id="227ab-168">Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="227ab-168">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="227ab-169">6 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-169">6\.</span></span> <span data-ttu-id="227ab-170">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="227ab-170">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="227ab-171">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="227ab-171">Select **Create**.</span></span>

   <span data-ttu-id="227ab-172">7 \.</span><span class="sxs-lookup"><span data-stu-id="227ab-172">7\.</span></span> <span data-ttu-id="227ab-173">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="227ab-173">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="227ab-174">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="227ab-174">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="227ab-175">Jeśli zostanie wyświetlony monit o zaufać certyfikatowi Deweloperskiemu, zaufaj certyfikatowi i Kontynuuj.</span><span class="sxs-lookup"><span data-stu-id="227ab-175">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-cli"></a>[<span data-ttu-id="227ab-176">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="227ab-176">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="227ab-177">W przypadku środowiska webassembly Blazor wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="227ab-177">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="227ab-178">W przypadku środowiska serwera Blazor należy wykonać następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="227ab-178">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="227ab-179">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="227ab-179">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="227ab-180">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="227ab-180">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="227ab-181">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="227ab-181">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="227ab-182">Home</span><span class="sxs-lookup"><span data-stu-id="227ab-182">Home</span></span>
* <span data-ttu-id="227ab-183">Licznik</span><span class="sxs-lookup"><span data-stu-id="227ab-183">Counter</span></span>
* <span data-ttu-id="227ab-184">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="227ab-184">Fetch data</span></span>

<span data-ttu-id="227ab-185">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="227ab-185">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="227ab-186">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale z Blazor można użyć C#.</span><span class="sxs-lookup"><span data-stu-id="227ab-186">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="227ab-187">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="227ab-187">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="227ab-188">Żądanie `/counter` w przeglądarce, zgodnie z dyrektywą `@page` w górnej części, powoduje, że składnik `Counter` renderuje jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="227ab-188">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="227ab-189">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="227ab-189">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="227ab-190">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="227ab-190">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="227ab-191">Zdarzenie `onclick` jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="227ab-191">The `onclick` event is fired.</span></span>
* <span data-ttu-id="227ab-192">Metoda `IncrementCount` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="227ab-192">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="227ab-193">`currentCount` jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="227ab-193">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="227ab-194">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="227ab-194">The component is rendered again.</span></span>

<span data-ttu-id="227ab-195">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="227ab-195">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="227ab-196">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="227ab-196">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="227ab-197">Na przykład Dodaj składnik `Counter` do strony głównej aplikacji przez dodanie elementu `<Counter />` do składnika `Index`.</span><span class="sxs-lookup"><span data-stu-id="227ab-197">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="227ab-198">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="227ab-198">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="227ab-199">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="227ab-199">Run the app.</span></span> <span data-ttu-id="227ab-200">Strona główna ma swój własny licznik dostarczony przez składnik `Counter`.</span><span class="sxs-lookup"><span data-stu-id="227ab-200">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="227ab-201">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="227ab-201">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="227ab-202">Aby dodać parametr do składnika `Counter`, zaktualizuj blok `@code` składnika:</span><span class="sxs-lookup"><span data-stu-id="227ab-202">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="227ab-203">Dodaj właściwość publiczną dla `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="227ab-203">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="227ab-204">Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="227ab-204">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="227ab-205">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="227ab-205">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="227ab-206">Określ `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="227ab-206">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="227ab-207">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="227ab-207">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="227ab-208">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="227ab-208">Run the app.</span></span> <span data-ttu-id="227ab-209">Składnik `Index` ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="227ab-209">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="227ab-210">Składnik `Counter` (*Counter. Razor*) w `/counter` nadal zwiększa się o jeden.</span><span class="sxs-lookup"><span data-stu-id="227ab-210">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="227ab-211">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="227ab-211">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="227ab-212">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="227ab-212">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
