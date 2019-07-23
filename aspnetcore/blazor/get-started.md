---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z usługą Blazor, tworząc aplikację Blazor przy użyciu wybranych przez siebie narzędzi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/19/2019
uid: blazor/get-started
ms.openlocfilehash: 7cc302216d14a6f1791ac3c0892d03ddb8abc974
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371791"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="680f3-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="680f3-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="680f3-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="680f3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="680f3-105">Rozpocznij pracę z usługą Blazor:</span><span class="sxs-lookup"><span data-stu-id="680f3-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="680f3-106">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="680f3-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="680f3-107">Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="680f3-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview6.19307.2
   ```

1. <span data-ttu-id="680f3-108">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="680f3-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="680f3-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="680f3-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="680f3-110">1\.</span><span class="sxs-lookup"><span data-stu-id="680f3-110">1\.</span></span> <span data-ttu-id="680f3-111">Zainstaluj najnowszą wersję [zapoznawczą programu Visual Studio](https://visualstudio.com/vs/preview) , korzystając z obciążeń **ASP.NET i Web Development** .</span><span class="sxs-lookup"><span data-stu-id="680f3-111">Install the latest [Visual Studio preview](https://visualstudio.com/vs/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="680f3-112">2\.</span><span class="sxs-lookup"><span data-stu-id="680f3-112">2\.</span></span> <span data-ttu-id="680f3-113">Zainstaluj najnowsze [rozszerzenie Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="680f3-113">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="680f3-114">Ten krok sprawia, że szablony Blazor są dostępne dla programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="680f3-114">This step makes Blazor templates available to Visual Studio.</span></span>

   <span data-ttu-id="680f3-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-115">3\.</span></span> <span data-ttu-id="680f3-116">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="680f3-116">Create a new project.</span></span>

   <span data-ttu-id="680f3-117">4\.</span><span class="sxs-lookup"><span data-stu-id="680f3-117">4\.</span></span> <span data-ttu-id="680f3-118">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="680f3-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="680f3-119">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="680f3-119">Select **Next**.</span></span>

   <span data-ttu-id="680f3-120">5 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-120">5\.</span></span> <span data-ttu-id="680f3-121">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="680f3-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="680f3-122">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="680f3-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="680f3-123">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="680f3-123">Select **Create**.</span></span>

   <span data-ttu-id="680f3-124">6 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-124">6\.</span></span> <span data-ttu-id="680f3-125">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="680f3-125">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>

   <span data-ttu-id="680f3-126">7 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-126">7\.</span></span> <span data-ttu-id="680f3-127">W przypadku środowiska po stronie klienta Blazor wybierz szablon **aplikacji Blazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="680f3-127">For a Blazor client-side experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="680f3-128">W przypadku środowiska po stronie serwera Blazor wybierz szablon **aplikacji Blazor Server** .</span><span class="sxs-lookup"><span data-stu-id="680f3-128">For a Blazor server-side experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="680f3-129">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="680f3-129">Select **Create**.</span></span> <span data-ttu-id="680f3-130">Aby uzyskać informacje na temat dwóch Blazorch modeli hostingu, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="680f3-130">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="680f3-131">8 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-131">8\.</span></span> <span data-ttu-id="680f3-132">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="680f3-132">Press **F5** to run the app.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="680f3-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="680f3-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="680f3-134">1\.</span><span class="sxs-lookup"><span data-stu-id="680f3-134">1\.</span></span> <span data-ttu-id="680f3-135">Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="680f3-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="680f3-136">2\.</span><span class="sxs-lookup"><span data-stu-id="680f3-136">2\.</span></span> <span data-ttu-id="680f3-137">Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="680f3-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="680f3-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-138">3\.</span></span> <span data-ttu-id="680f3-139">W przypadku środowiska po stronie klienta Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="680f3-139">For a Blazor client-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="680f3-140">W przypadku środowiska po stronie serwera Blazor wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="680f3-140">For a Blazor server-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      <span data-ttu-id="680f3-141">Aby uzyskać informacje na temat dwóch Blazorch modeli hostingu, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="680f3-141">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="680f3-142">4\.</span><span class="sxs-lookup"><span data-stu-id="680f3-142">4\.</span></span> <span data-ttu-id="680f3-143">Otwórz folder *WebApplication1* w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="680f3-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="680f3-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-144">5\.</span></span> <span data-ttu-id="680f3-145">W przypadku projektu po stronie serwera Blazor IDE żąda dodania zasobów do kompilowania i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="680f3-145">For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="680f3-146">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="680f3-146">Select **Yes**.</span></span>

   <span data-ttu-id="680f3-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-147">6\.</span></span> <span data-ttu-id="680f3-148">Jeśli używasz aplikacji po stronie serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="680f3-148">If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="680f3-149">Jeśli używasz aplikacji po stronie klienta Blazor, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="680f3-149">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="680f3-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="680f3-150">7\.</span></span> <span data-ttu-id="680f3-151">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="680f3-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **ASP.NET Core Blazor Server App** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="680f3-152">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="680f3-152">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="680f3-153">W przypadku środowiska po stronie klienta Blazor wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="680f3-153">For a Blazor client-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="680f3-154">W przypadku środowiska Blazor po stronie serwera wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="680f3-154">For a Blazor server-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="680f3-155">Aby uzyskać informacje na temat dwóch Blazorch modeli hostingu, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="680f3-155">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="680f3-156">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="680f3-156">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="680f3-157">Na pasku bocznym są dostępne wiele stron:</span><span class="sxs-lookup"><span data-stu-id="680f3-157">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="680f3-158">Home</span><span class="sxs-lookup"><span data-stu-id="680f3-158">Home</span></span>
* <span data-ttu-id="680f3-159">Licznik</span><span class="sxs-lookup"><span data-stu-id="680f3-159">Counter</span></span>
* <span data-ttu-id="680f3-160">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="680f3-160">Fetch data</span></span>

<span data-ttu-id="680f3-161">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="680f3-161">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="680f3-162">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale składniki Razor zapewniają lepsze podejście przy użyciu C#.</span><span class="sxs-lookup"><span data-stu-id="680f3-162">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="680f3-163">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="680f3-163">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="680f3-164">Żądanie `/counter` w przeglądarce, zgodnie z definicją `@page` w dyrektywie u góry, powoduje, że `Counter` składnik renderuje jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="680f3-164">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="680f3-165">Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="680f3-165">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="680f3-166">Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :</span><span class="sxs-lookup"><span data-stu-id="680f3-166">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="680f3-167">`onclick` Zdarzenie jest wyzwalane.</span><span class="sxs-lookup"><span data-stu-id="680f3-167">The `onclick` event is fired.</span></span>
* <span data-ttu-id="680f3-168">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="680f3-168">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="680f3-169">Wartość `currentCount` jest zwiększana.</span><span class="sxs-lookup"><span data-stu-id="680f3-169">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="680f3-170">Składnik jest ponownie renderowany.</span><span class="sxs-lookup"><span data-stu-id="680f3-170">The component is rendered again.</span></span>

<span data-ttu-id="680f3-171">Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="680f3-171">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="680f3-172">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="680f3-172">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="680f3-173">Na przykład Dodaj `Counter` składnik do strony głównej aplikacji przez `<Counter />` dodanie elementu do `Index` składnika.</span><span class="sxs-lookup"><span data-stu-id="680f3-173">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="680f3-174">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="680f3-174">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="680f3-175">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="680f3-175">Run the app.</span></span> <span data-ttu-id="680f3-176">Strona główna ma swój własny licznik dostarczony przez `Counter` składnik.</span><span class="sxs-lookup"><span data-stu-id="680f3-176">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="680f3-177">Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="680f3-177">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="680f3-178">Aby dodać parametr do `Counter` składnika, zaktualizuj `@code` blok składnika:</span><span class="sxs-lookup"><span data-stu-id="680f3-178">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="680f3-179">Dodaj właściwość dla `IncrementAmount` `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="680f3-179">Add a property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="680f3-180">Zmień metodę, aby `IncrementAmount` użyć `currentCount`podczas zwiększania wartości. `IncrementCount`</span><span class="sxs-lookup"><span data-stu-id="680f3-180">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="680f3-181">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="680f3-181">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="680f3-182">`<Counter>` Określ element `IncrementAmount` w elemencie `Index` składnika przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="680f3-182">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="680f3-183">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="680f3-183">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="680f3-184">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="680f3-184">Run the app.</span></span> <span data-ttu-id="680f3-185">Składnik ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie.** `Index`</span><span class="sxs-lookup"><span data-stu-id="680f3-185">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="680f3-186">Składnik (*Counter. Razor*) w `/counter` dalszym ciągu zwiększa się o jeden. `Counter`</span><span class="sxs-lookup"><span data-stu-id="680f3-186">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="680f3-187">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="680f3-187">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="680f3-188">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="680f3-188">Additional resources</span></span>

* <xref:signalr/introduction>
