---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z Blazor, tworząc aplikację Blazor za pomocą wybranego przez siebie narzędzia.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: abecb640930c1e5770c0fad45a1e9a6df31a20f4
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306458"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="925cc-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="925cc-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="925cc-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="925cc-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="925cc-105">Aby rozpocząć pracę z usługą Blazor, postępuj zgodnie ze wskazówkami dotyczącymi wybranych narzędzi:</span><span class="sxs-lookup"><span data-stu-id="925cc-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="925cc-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="925cc-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="925cc-107">Aby utworzyć aplikacje serwera Blazor, zainstaluj [program Visual Studio 2019 w wersji 16,4 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążenia **ASP.NET i sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="925cc-107">To create Blazor Server apps, install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="925cc-108">Aby utworzyć aplikacje Blazor Server i Blazor webassembly, zainstaluj program Visual Studio 2019 16,6 Preview 2 lub nowszy przy użyciu obciążeń programu **ASP.NET i sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="925cc-108">To create Blazor Server and Blazor WebAssembly apps, install Visual Studio 2019 16.6 Preview 2 or later with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="925cc-109">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor webassembly* i *Blazor Server*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="925cc-109">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="925cc-110">Tworzenie nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="925cc-110">Create a new project.</span></span>

1. <span data-ttu-id="925cc-111">Wybierz pozycję **aplikacja Blazor**.</span><span class="sxs-lookup"><span data-stu-id="925cc-111">Select **Blazor App**.</span></span> <span data-ttu-id="925cc-112">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="925cc-112">Select **Next**.</span></span>

1. <span data-ttu-id="925cc-113">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="925cc-113">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="925cc-114">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="925cc-114">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="925cc-115">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="925cc-115">Select **Create**.</span></span>

1. <span data-ttu-id="925cc-116">W przypadku środowiska webassembly Blazor (Visual Studio 16,6 Preview 2 lub nowszego) wybierz szablon **aplikacji Blazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="925cc-116">For a Blazor WebAssembly experience (Visual Studio 16.6 Preview 2 or later), choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="925cc-117">W przypadku środowiska serwera Blazor (Visual Studio 16,4 lub nowszego) wybierz szablon **aplikacji Blazor Server** .</span><span class="sxs-lookup"><span data-stu-id="925cc-117">For a Blazor Server experience (Visual Studio 16.4 or later), choose the **Blazor Server App** template.</span></span> <span data-ttu-id="925cc-118">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="925cc-118">Select **Create**.</span></span>

1. <span data-ttu-id="925cc-119">Naciśnij klawisz <kbd>Ctrl</kbd> ,+<kbd>F5</kbd> , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="925cc-119">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="925cc-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="925cc-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="925cc-121">Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="925cc-121">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="925cc-122">Opcjonalnie można zainstalować szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) Preview, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="925cc-122">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="925cc-123">[Zestaw .NET Core SDK w wersji 3.1.201 lub nowszej](https://dotnet.microsoft.com/download/dotnet-core/3.1) jest **wymagana** , aby można było użyć szablonu zestawu webassembly 3,2 (wersja zapoznawcza 3 Blazor).</span><span class="sxs-lookup"><span data-stu-id="925cc-123">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="925cc-124">Potwierdź zainstalowaną zestaw .NET Core SDK wersję, uruchamiając `dotnet --version` w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="925cc-124">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="925cc-125">Zainstaluj narzędzie [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="925cc-125">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="925cc-126">Zainstaluj najnowsze [ C# rozszerzenie programu dla Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) i rozszerzenie [JavaScript Debugger (nocne)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) z `debug.javascript.usePreview` ustawionym na `true`.</span><span class="sxs-lookup"><span data-stu-id="925cc-126">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

1. <span data-ttu-id="925cc-127">W przypadku środowiska serwera Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="925cc-127">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="925cc-128">W przypadku środowiska webassembly Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="925cc-128">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="925cc-129">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="925cc-129">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="925cc-130">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="925cc-130">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="925cc-131">Żądania IDE służące do dodawania zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="925cc-131">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="925cc-132">Wybierz pozycję **Tak**.</span><span class="sxs-lookup"><span data-stu-id="925cc-132">Select **Yes**.</span></span>

1. <span data-ttu-id="925cc-133">TUN aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="925cc-133">Tun the app using the Visual Studio Code debugger.</span></span>

1. <span data-ttu-id="925cc-134">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="925cc-134">In a browser, navigate to `https://localhost:5001`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="925cc-135">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="925cc-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="925cc-136">Serwer Blazor jest obsługiwany w Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="925cc-136">Blazor Server is supported in Visual Studio for Mac.</span></span> <span data-ttu-id="925cc-137">Zestaw webassembly Blazor nie jest obecnie obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="925cc-137">Blazor WebAssembly isn't supported at this time.</span></span> <span data-ttu-id="925cc-138">Aby kompilować Blazor aplikacje webassembly na macOS, postępuj zgodnie ze wskazówkami na karcie **interfejs wiersza polecenia platformy .NET Core** .</span><span class="sxs-lookup"><span data-stu-id="925cc-138">To build Blazor WebAssembly apps on macOS, follow the guidance on the **.NET Core CLI** tab.</span></span>

1. <span data-ttu-id="925cc-139">Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="925cc-139">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="925cc-140">Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="925cc-140">Select **File** > **New Solution** or create a **New Project**.</span></span>

1. <span data-ttu-id="925cc-141">Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="925cc-141">In the sidebar, select **.NET Core** > **App**.</span></span>

1. <span data-ttu-id="925cc-142">Wybierz szablon **aplikacji Blazor Server** .</span><span class="sxs-lookup"><span data-stu-id="925cc-142">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="925cc-143">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="925cc-143">Select **Create**.</span></span>

   <span data-ttu-id="925cc-144">Aby uzyskać informacje na temat modelu hostingu serwera Blazor, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="925cc-144">For information on the Blazor Server hosting model, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="925cc-145">Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="925cc-145">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

1. <span data-ttu-id="925cc-146">W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="925cc-146">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="925cc-147">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="925cc-147">Select **Create**.</span></span>

1. <span data-ttu-id="925cc-148">Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="925cc-148">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="925cc-149">Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.</span><span class="sxs-lookup"><span data-stu-id="925cc-149">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

<span data-ttu-id="925cc-150">Jeśli zostanie wyświetlony monit o zaufać certyfikatowi Deweloperskiemu, zaufaj certyfikatowi i Kontynuuj.</span><span class="sxs-lookup"><span data-stu-id="925cc-150">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="925cc-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="925cc-151">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="925cc-152">Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="925cc-152">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="925cc-153">Opcjonalnie można zainstalować szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) Preview, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="925cc-153">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="925cc-154">[Zestaw .NET Core SDK w wersji 3.1.201 lub nowszej](https://dotnet.microsoft.com/download/dotnet-core/3.1) jest **wymagana** , aby można było użyć szablonu zestawu webassembly 3,2 (wersja zapoznawcza 3 Blazor).</span><span class="sxs-lookup"><span data-stu-id="925cc-154">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="925cc-155">Potwierdź zainstalowaną zestaw .NET Core SDK wersję, uruchamiając `dotnet --version` w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="925cc-155">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="925cc-156">W przypadku środowiska serwera Blazor należy wykonać następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="925cc-156">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="925cc-157">W przypadku środowiska webassembly Blazor wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="925cc-157">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="925cc-158">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="925cc-158">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="925cc-159">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="925cc-159">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="925cc-160">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="925cc-160">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="925cc-161">Domowy</span><span class="sxs-lookup"><span data-stu-id="925cc-161">Home</span></span>
* <span data-ttu-id="925cc-162">Licznik</span><span class="sxs-lookup"><span data-stu-id="925cc-162">Counter</span></span>
* <span data-ttu-id="925cc-163">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="925cc-163">Fetch data</span></span>

<span data-ttu-id="925cc-164">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="925cc-164">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="925cc-165">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale z Blazor można użyć C#.</span><span class="sxs-lookup"><span data-stu-id="925cc-165">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="925cc-166">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="925cc-166">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="925cc-167">Żądanie `/counter` w przeglądarce, zgodnie z dyrektywą `@page` w górnej części, powoduje, że składnik `Counter` renderuje jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="925cc-167">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="925cc-168">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="925cc-168">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="925cc-169">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="925cc-169">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="925cc-170">Zdarzenie `onclick` jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="925cc-170">The `onclick` event is fired.</span></span>
* <span data-ttu-id="925cc-171">Metoda `IncrementCount` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="925cc-171">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="925cc-172">`currentCount` jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="925cc-172">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="925cc-173">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="925cc-173">The component is rendered again.</span></span>

<span data-ttu-id="925cc-174">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="925cc-174">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="925cc-175">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="925cc-175">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="925cc-176">Na przykład Dodaj składnik `Counter` do strony głównej aplikacji przez dodanie elementu `<Counter />` do składnika `Index`.</span><span class="sxs-lookup"><span data-stu-id="925cc-176">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="925cc-177">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="925cc-177">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="925cc-178">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="925cc-178">Run the app.</span></span> <span data-ttu-id="925cc-179">Strona główna ma swój własny licznik dostarczony przez składnik `Counter`.</span><span class="sxs-lookup"><span data-stu-id="925cc-179">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="925cc-180">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="925cc-180">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="925cc-181">Aby dodać parametr do składnika `Counter`, zaktualizuj blok `@code` składnika:</span><span class="sxs-lookup"><span data-stu-id="925cc-181">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="925cc-182">Dodaj właściwość publiczną dla `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="925cc-182">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="925cc-183">Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="925cc-183">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="925cc-184">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="925cc-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="925cc-185">Określ `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="925cc-185">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="925cc-186">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="925cc-186">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="925cc-187">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="925cc-187">Run the app.</span></span> <span data-ttu-id="925cc-188">Składnik `Index` ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="925cc-188">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="925cc-189">Składnik `Counter` (*Counter. Razor*) w `/counter` nadal zwiększa się o jeden.</span><span class="sxs-lookup"><span data-stu-id="925cc-189">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="925cc-190">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="925cc-190">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="925cc-191">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="925cc-191">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
