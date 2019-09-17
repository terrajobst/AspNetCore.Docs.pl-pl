---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z usługą Blazor, tworząc aplikację Blazor przy użyciu wybranych przez siebie narzędzi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/get-started
ms.openlocfilehash: cce91b6332295f77c639f881fe342b625fee7fca
ms.sourcegitcommit: 92c901c7f32ee9efb335d99ec4c3add2cc9f3142
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "71025521"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="4e156-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="4e156-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="4e156-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4e156-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4e156-105">Rozpocznij pracę z usługą Blazor:</span><span class="sxs-lookup"><span data-stu-id="4e156-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="4e156-106">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="4e156-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="4e156-107">Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="4e156-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19457.4
   ```

1. <span data-ttu-id="4e156-108">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="4e156-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e156-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e156-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="4e156-110">1\.</span><span class="sxs-lookup"><span data-stu-id="4e156-110">1\.</span></span> <span data-ttu-id="4e156-111">Zainstaluj najnowszą wersję [zapoznawczą programu Visual Studio](https://visualstudio.com/vs/preview) , korzystając z obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="4e156-111">Install the latest [Visual Studio preview](https://visualstudio.com/vs/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="4e156-112">2\.</span><span class="sxs-lookup"><span data-stu-id="4e156-112">2\.</span></span> <span data-ttu-id="4e156-113">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="4e156-113">Create a new project.</span></span>

   <span data-ttu-id="4e156-114">3 \.</span><span class="sxs-lookup"><span data-stu-id="4e156-114">3\.</span></span> <span data-ttu-id="4e156-115">Wybierz pozycję **aplikacja Blazor**.</span><span class="sxs-lookup"><span data-stu-id="4e156-115">Select **Blazor App**.</span></span> <span data-ttu-id="4e156-116">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="4e156-116">Select **Next**.</span></span>

   <span data-ttu-id="4e156-117">4\.</span><span class="sxs-lookup"><span data-stu-id="4e156-117">4\.</span></span> <span data-ttu-id="4e156-118">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="4e156-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="4e156-119">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="4e156-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="4e156-120">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4e156-120">Select **Create**.</span></span>

   <span data-ttu-id="4e156-121">5 \.</span><span class="sxs-lookup"><span data-stu-id="4e156-121">5\.</span></span> <span data-ttu-id="4e156-122">Aby zapoznać się z Blazor webassembly, wybierz szablon **aplikacji Blazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="4e156-122">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="4e156-123">Dla środowiska serwera Blazor wybierz szablon **aplikacji Blazor Server** .</span><span class="sxs-lookup"><span data-stu-id="4e156-123">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="4e156-124">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4e156-124">Select **Create**.</span></span> <span data-ttu-id="4e156-125">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, <xref:blazor/hosting-models>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="4e156-125">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="4e156-126">6 \.</span><span class="sxs-lookup"><span data-stu-id="4e156-126">6\.</span></span> <span data-ttu-id="4e156-127">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="4e156-127">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4e156-128">Jeśli zainstalowano rozszerzenie Blazor programu Visual Studio dla starszej wersji zapoznawczej programu ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="4e156-128">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="4e156-129">Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e156-129">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4e156-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4e156-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="4e156-131">1\.</span><span class="sxs-lookup"><span data-stu-id="4e156-131">1\.</span></span> <span data-ttu-id="4e156-132">Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="4e156-132">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="4e156-133">2\.</span><span class="sxs-lookup"><span data-stu-id="4e156-133">2\.</span></span> <span data-ttu-id="4e156-134">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="4e156-134">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="4e156-135">3 \.</span><span class="sxs-lookup"><span data-stu-id="4e156-135">3\.</span></span> <span data-ttu-id="4e156-136">W przypadku środowiska webassembly Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="4e156-136">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="4e156-137">W przypadku środowiska serwera Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="4e156-137">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="4e156-138">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, <xref:blazor/hosting-models>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="4e156-138">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="4e156-139">4\.</span><span class="sxs-lookup"><span data-stu-id="4e156-139">4\.</span></span> <span data-ttu-id="4e156-140">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4e156-140">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="4e156-141">5 \.</span><span class="sxs-lookup"><span data-stu-id="4e156-141">5\.</span></span> <span data-ttu-id="4e156-142">W przypadku projektu serwera Blazor, IDE żąda dodania zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="4e156-142">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="4e156-143">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="4e156-143">Select **Yes**.</span></span>

   <span data-ttu-id="4e156-144">6 \.</span><span class="sxs-lookup"><span data-stu-id="4e156-144">6\.</span></span> <span data-ttu-id="4e156-145">W przypadku korzystania z aplikacji serwera Blazor należy uruchomić aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4e156-145">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="4e156-146">Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4e156-146">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="4e156-147">7 \.</span><span class="sxs-lookup"><span data-stu-id="4e156-147">7\.</span></span> <span data-ttu-id="4e156-148">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4e156-148">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4e156-149">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4e156-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="4e156-150">W przypadku środowiska webassembly Blazor wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="4e156-150">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="4e156-151">W przypadku środowiska serwera Blazor należy wykonać następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="4e156-151">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="4e156-152">Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, <xref:blazor/hosting-models>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="4e156-152">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="4e156-153">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4e156-153">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="4e156-154">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="4e156-154">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="4e156-155">Home</span><span class="sxs-lookup"><span data-stu-id="4e156-155">Home</span></span>
* <span data-ttu-id="4e156-156">Licznik</span><span class="sxs-lookup"><span data-stu-id="4e156-156">Counter</span></span>
* <span data-ttu-id="4e156-157">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="4e156-157">Fetch data</span></span>

<span data-ttu-id="4e156-158">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="4e156-158">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="4e156-159">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale składniki Razor zapewniają lepsze podejście przy użyciu C#.</span><span class="sxs-lookup"><span data-stu-id="4e156-159">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="4e156-160">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4e156-160">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="4e156-161">Żądanie `/counter` w przeglądarce, zgodnie z definicją `@page` w dyrektywie u góry, powoduje, że `Counter` składnik renderuje jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="4e156-161">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="4e156-162">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="4e156-162">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="4e156-163">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="4e156-163">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="4e156-164">`onclick` Zdarzenie jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="4e156-164">The `onclick` event is fired.</span></span>
* <span data-ttu-id="4e156-165">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="4e156-165">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="4e156-166">Wartość `currentCount` jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="4e156-166">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="4e156-167">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="4e156-167">The component is rendered again.</span></span>

<span data-ttu-id="4e156-168">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="4e156-168">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="4e156-169">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="4e156-169">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="4e156-170">Na przykład Dodaj `Counter` składnik do strony głównej aplikacji przez `<Counter />` dodanie elementu do `Index` składnika.</span><span class="sxs-lookup"><span data-stu-id="4e156-170">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="4e156-171">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4e156-171">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="4e156-172">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="4e156-172">Run the app.</span></span> <span data-ttu-id="4e156-173">Strona główna ma swój własny licznik dostarczony przez `Counter` składnik.</span><span class="sxs-lookup"><span data-stu-id="4e156-173">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="4e156-174">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4e156-174">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="4e156-175">Aby dodać parametr do `Counter` składnika, zaktualizuj `@code` blok składnika:</span><span class="sxs-lookup"><span data-stu-id="4e156-175">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="4e156-176">Dodaj właściwość publiczną dla `IncrementAmount` `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4e156-176">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="4e156-177">Zmień metodę, aby `IncrementAmount` użyć `currentCount`podczas zwiększania wartości. `IncrementCount`</span><span class="sxs-lookup"><span data-stu-id="4e156-177">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="4e156-178">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4e156-178">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="4e156-179">`<Counter>` Określ element `IncrementAmount` w elemencie `Index` składnika przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4e156-179">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="4e156-180">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4e156-180">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="4e156-181">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="4e156-181">Run the app.</span></span> <span data-ttu-id="4e156-182">Składnik ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie.** `Index`</span><span class="sxs-lookup"><span data-stu-id="4e156-182">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="4e156-183">Składnik (*Counter. Razor*) w `/counter` dalszym ciągu zwiększa się o jeden. `Counter`</span><span class="sxs-lookup"><span data-stu-id="4e156-183">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e156-184">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4e156-184">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="4e156-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4e156-185">Additional resources</span></span>

* <xref:signalr/introduction>
