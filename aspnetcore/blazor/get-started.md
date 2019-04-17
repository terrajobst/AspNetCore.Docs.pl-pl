---
title: Rozpoczynanie pracy z usługą Blazor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/get-started
ms.openlocfilehash: cc68e1c58af607f840b952f033d3f3b0e54e6cf4
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614876"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="09eac-103">Rozpoczynanie pracy z usługą Blazor</span><span class="sxs-lookup"><span data-stu-id="09eac-103">Get started with Blazor</span></span>

<span data-ttu-id="09eac-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="09eac-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="09eac-105">Rozpoczynanie pracy z usługą Blazor zgodnie z jedną z następujących środowisk:</span><span class="sxs-lookup"><span data-stu-id="09eac-105">Get started with Blazor following either of the following experiences:</span></span>

* [<span data-ttu-id="09eac-106">Składniki po stronie serwera Razor</span><span class="sxs-lookup"><span data-stu-id="09eac-106">Server-side Razor Components</span></span>](#server-side-razor-components-experience)
* [<span data-ttu-id="09eac-107">Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="09eac-107">Client-side Blazor</span></span>](#client-side-blazor-experience)

## <a name="server-side-razor-components-experience"></a><span data-ttu-id="09eac-108">Środowisko Razor składniki po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="09eac-108">Server-side Razor Components experience</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09eac-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09eac-109">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="09eac-110">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="09eac-110">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="09eac-111">Aby utworzyć swój pierwszy projekt Razor składników w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="09eac-111">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="09eac-112">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.</span><span class="sxs-lookup"><span data-stu-id="09eac-112">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="09eac-113">Włącz Visual Studio ma używać zestawów SDK w wersji zapoznawczej:</span><span class="sxs-lookup"><span data-stu-id="09eac-113">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="09eac-114">Otwórz **narzędzia** > **opcje** na pasku menu.</span><span class="sxs-lookup"><span data-stu-id="09eac-114">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="09eac-115">Otwórz **projekty i rozwiązania** węzła.</span><span class="sxs-lookup"><span data-stu-id="09eac-115">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="09eac-116">Otwórz **platformy .NET Core** kartę.</span><span class="sxs-lookup"><span data-stu-id="09eac-116">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="09eac-117">Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="09eac-117">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="09eac-118">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="09eac-118">Select **OK**.</span></span>
1. <span data-ttu-id="09eac-119">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="09eac-119">Create a new project.</span></span>
1. <span data-ttu-id="09eac-120">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="09eac-120">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="09eac-121">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="09eac-121">Select **Next**.</span></span>
1. <span data-ttu-id="09eac-122">Podaj nazwę w **Nazwa projektu** pola.</span><span class="sxs-lookup"><span data-stu-id="09eac-122">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="09eac-123">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="09eac-123">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="09eac-124">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="09eac-124">Select **Create**.</span></span>
1. <span data-ttu-id="09eac-125">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="09eac-125">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="09eac-126">Wybierz **składniki Razor** szablonu, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="09eac-126">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="09eac-127">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09eac-127">Press **F5** to run the app.</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:blazor/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="09eac-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="09eac-128">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="09eac-129">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="09eac-129">Prerequisites:</span></span>

* [<span data-ttu-id="09eac-130">.NET core SDK 3.0 w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="09eac-130">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="09eac-131">Aby utworzyć swój pierwszy projekt składniki Razor z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="09eac-131">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="09eac-132">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="09eac-132">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="09eac-133">Składniki razor są tworzone za pomocą składni Razor, ale są kompilowane w sposób inny niż widoki stron Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="09eac-133">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="09eac-134">*.Razor* rozszerzenie pliku jest używany do określenia składnika Razor.</span><span class="sxs-lookup"><span data-stu-id="09eac-134">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="09eac-135">Strony razor i MVC widoków w dalszym ciągu używać *.cshtml* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="09eac-135">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="09eac-136">Razor składniki mogą być tworzone za pomocą *.cshtml* rozszerzenie pliku, tak długo, jak te pliki są identyfikowane jako pliki składnika Razor przy użyciu `_RazorComponentInclude` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="09eac-136">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="09eac-137">Na przykład aplikacja utworzona za pomocą szablonu Razor składnika Określa, że wszystkie *.cshtml* plików w obszarze *składniki* folder powinien być traktowany jako składniki Razor:</span><span class="sxs-lookup"><span data-stu-id="09eac-137">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="09eac-138">Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="09eac-138">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="09eac-139">Home</span><span class="sxs-lookup"><span data-stu-id="09eac-139">Home</span></span>
* <span data-ttu-id="09eac-140">Licznik</span><span class="sxs-lookup"><span data-stu-id="09eac-140">Counter</span></span>
* <span data-ttu-id="09eac-141">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="09eac-141">Fetch data</span></span>

<span data-ttu-id="09eac-142">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="09eac-142">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="09eac-143">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="09eac-143">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="09eac-144">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="09eac-144">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="09eac-145">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="09eac-145">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="09eac-146">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="09eac-146">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="09eac-147">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="09eac-147">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="09eac-148">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="09eac-148">The `onclick` event is fired.</span></span>
* <span data-ttu-id="09eac-149">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="09eac-149">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="09eac-150">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="09eac-150">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="09eac-151">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="09eac-151">The component is rendered again.</span></span>

<span data-ttu-id="09eac-152">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="09eac-152">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="09eac-153">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="09eac-153">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="09eac-154">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="09eac-154">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="09eac-155">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="09eac-155">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="09eac-156">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="09eac-156">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="09eac-157">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="09eac-157">Run the app.</span></span> <span data-ttu-id="09eac-158">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="09eac-158">The homepage has its own counter.</span></span>

<span data-ttu-id="09eac-159">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="09eac-159">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="09eac-160">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09eac-160">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="09eac-161">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="09eac-161">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="09eac-162">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="09eac-162">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4-5,9)]

<span data-ttu-id="09eac-163">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09eac-163">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="09eac-164">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="09eac-164">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="09eac-165">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="09eac-165">Run the app.</span></span> <span data-ttu-id="09eac-166">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="09eac-166">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="client-side-blazor-experience"></a><span data-ttu-id="09eac-167">Środowisko Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="09eac-167">Client-side Blazor experience</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09eac-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09eac-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="09eac-169">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="09eac-169">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="09eac-170">Aby utworzyć swój pierwszy projekt Blazor w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="09eac-170">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="09eac-171">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.</span><span class="sxs-lookup"><span data-stu-id="09eac-171">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="09eac-172">Włącz Visual Studio ma używać zestawów SDK w wersji zapoznawczej:</span><span class="sxs-lookup"><span data-stu-id="09eac-172">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="09eac-173">Otwórz **narzędzia** > **opcje** na pasku menu.</span><span class="sxs-lookup"><span data-stu-id="09eac-173">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="09eac-174">Otwórz **projekty i rozwiązania** węzła.</span><span class="sxs-lookup"><span data-stu-id="09eac-174">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="09eac-175">Otwórz **platformy .NET Core** kartę.</span><span class="sxs-lookup"><span data-stu-id="09eac-175">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="09eac-176">Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="09eac-176">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="09eac-177">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="09eac-177">Select **OK**.</span></span>
1. <span data-ttu-id="09eac-178">Zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09eac-178">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="09eac-179">Ten krok udostępnia Blazor szablony programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09eac-179">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="09eac-180">Udostępnia szablony Blazor do użytku z programem .NET Core interfejsu wiersza polecenia, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="09eac-180">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="09eac-181">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="09eac-181">Create a new project.</span></span>
1. <span data-ttu-id="09eac-182">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="09eac-182">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="09eac-183">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="09eac-183">Select **Next**.</span></span>
1. <span data-ttu-id="09eac-184">Podaj nazwę w **Nazwa projektu** pola.</span><span class="sxs-lookup"><span data-stu-id="09eac-184">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="09eac-185">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="09eac-185">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="09eac-186">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="09eac-186">Select **Create**.</span></span>
1. <span data-ttu-id="09eac-187">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="09eac-187">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="09eac-188">Wybierz **Blazor** szablonu, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="09eac-188">Select the **Blazor** template and select **Create**.</span></span>
1. <span data-ttu-id="09eac-189">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09eac-189">Press **F5** to run the app.</span></span>

<span data-ttu-id="09eac-190">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="09eac-190">Congratulations!</span></span> <span data-ttu-id="09eac-191">Po prostu uruchomiono swoją pierwszą aplikację Blazor!</span><span class="sxs-lookup"><span data-stu-id="09eac-191">You just ran your first Blazor app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:blazor/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="09eac-192">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="09eac-192">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="09eac-193">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="09eac-193">Prerequisites:</span></span>

* [<span data-ttu-id="09eac-194">.NET core SDK 3.0 w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="09eac-194">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="09eac-195">Dodaj do niego szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="09eac-195">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="09eac-196">Utwórz swój pierwszy projekt Blazor w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="09eac-196">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="09eac-197">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="09eac-197">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="09eac-198">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="09eac-198">Congratulations!</span></span> <span data-ttu-id="09eac-199">Po prostu uruchomiono swoją pierwszą aplikację Blazor!</span><span class="sxs-lookup"><span data-stu-id="09eac-199">You just ran your first Blazor app!</span></span>

---

<span data-ttu-id="09eac-200">Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="09eac-200">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="09eac-201">Home</span><span class="sxs-lookup"><span data-stu-id="09eac-201">Home</span></span>
* <span data-ttu-id="09eac-202">Licznik</span><span class="sxs-lookup"><span data-stu-id="09eac-202">Counter</span></span>
* <span data-ttu-id="09eac-203">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="09eac-203">Fetch data</span></span>

<span data-ttu-id="09eac-204">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="09eac-204">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="09eac-205">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Blazor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="09eac-205">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="09eac-206">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="09eac-206">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="09eac-207">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="09eac-207">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="09eac-208">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="09eac-208">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="09eac-209">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="09eac-209">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="09eac-210">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="09eac-210">The `onclick` event is fired.</span></span>
* <span data-ttu-id="09eac-211">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="09eac-211">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="09eac-212">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="09eac-212">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="09eac-213">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="09eac-213">The component is rendered again.</span></span>

<span data-ttu-id="09eac-214">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="09eac-214">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="09eac-215">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="09eac-215">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="09eac-216">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="09eac-216">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="09eac-217">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="09eac-217">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="09eac-218">W *Pages/Index.cshtml*, Zastąp ankiety monitu składnika ze składnikiem licznika:</span><span class="sxs-lookup"><span data-stu-id="09eac-218">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="09eac-219">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="09eac-219">Run the app.</span></span> <span data-ttu-id="09eac-220">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="09eac-220">The homepage has its own counter.</span></span>

<span data-ttu-id="09eac-221">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="09eac-221">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="09eac-222">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09eac-222">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="09eac-223">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="09eac-223">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="09eac-224">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="09eac-224">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4-5,9)]

<span data-ttu-id="09eac-225">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09eac-225">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="09eac-226">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="09eac-226">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="09eac-227">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="09eac-227">Run the app.</span></span> <span data-ttu-id="09eac-228">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="09eac-228">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09eac-229">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="09eac-229">Next steps</span></span>

<xref:tutorials/first-blazor-app>
