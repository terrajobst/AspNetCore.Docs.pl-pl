---
title: Wprowadzenie do platformy ASP.NET Core Blazor
author: guardrex
description: Rozpoczynanie pracy z usługą Blazor, tworząc aplikację Blazor, za pomocą narzędzi dostępnych w wybranym.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/get-started
ms.openlocfilehash: 11637ef1c4b54961cc3094c263196be222cc2c60
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152698"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="3aeca-103">Wprowadzenie do platformy ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="3aeca-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="3aeca-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3aeca-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3aeca-105">Rozpoczynanie pracy z usługą Blazor:</span><span class="sxs-lookup"><span data-stu-id="3aeca-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="3aeca-106">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.</span><span class="sxs-lookup"><span data-stu-id="3aeca-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="3aeca-107">Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="3aeca-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview6.19307.2
   ```

1. <span data-ttu-id="3aeca-108">Zgodnie z wytycznymi z dowolnie wybranych narzędzi:</span><span class="sxs-lookup"><span data-stu-id="3aeca-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3aeca-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3aeca-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="3aeca-110">1\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-110">1\.</span></span> <span data-ttu-id="3aeca-111">Zainstaluj najnowszą wersję [programu Visual Studio preview](https://visualstudio.com/preview) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="3aeca-111">Install the latest [Visual Studio preview](https://visualstudio.com/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="3aeca-112">2\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-112">2\.</span></span> <span data-ttu-id="3aeca-113">Zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3aeca-113">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="3aeca-114">Ten krok udostępnia Blazor szablony programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3aeca-114">This step makes Blazor templates available to Visual Studio.</span></span>

   <span data-ttu-id="3aeca-115">3\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-115">3\.</span></span> <span data-ttu-id="3aeca-116">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="3aeca-116">Create a new project.</span></span>

   <span data-ttu-id="3aeca-117">4\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-117">4\.</span></span> <span data-ttu-id="3aeca-118">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3aeca-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="3aeca-119">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="3aeca-119">Select **Next**.</span></span>

   <span data-ttu-id="3aeca-120">5\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-120">5\.</span></span> <span data-ttu-id="3aeca-121">Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="3aeca-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="3aeca-122">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="3aeca-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="3aeca-123">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="3aeca-123">Select **Create**.</span></span>

   <span data-ttu-id="3aeca-124">6\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-124">6\.</span></span> <span data-ttu-id="3aeca-125">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="3aeca-125">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>

   <span data-ttu-id="3aeca-126">7\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-126">7\.</span></span> <span data-ttu-id="3aeca-127">Środowisko pracy klienta Blazor, wybierz **Blazor (po stronie klienta)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3aeca-127">For a Blazor client-side experience, choose the **Blazor (client-side)** template.</span></span> <span data-ttu-id="3aeca-128">Środowisko pracy Blazor po stronie serwera, wybierz **Blazor (po stronie serwera)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3aeca-128">For a Blazor server-side experience, choose the **Blazor (server-side)** template.</span></span> <span data-ttu-id="3aeca-129">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="3aeca-129">Select **Create**.</span></span> <span data-ttu-id="3aeca-130">Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3aeca-130">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3aeca-131">8\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-131">8\.</span></span> <span data-ttu-id="3aeca-132">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aeca-132">Press **F5** to run the app.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3aeca-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3aeca-133">Visual Studio Code</span></span>](#tab/visual-studio-code)
   
   <span data-ttu-id="3aeca-134">1\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-134">1\.</span></span> <span data-ttu-id="3aeca-135">Zainstaluj [programu Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="3aeca-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="3aeca-136">2\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-136">2\.</span></span> <span data-ttu-id="3aeca-137">Zainstaluj najnowszą wersję [ C# rozszerzenia programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="3aeca-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="3aeca-138">3\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-138">3\.</span></span> <span data-ttu-id="3aeca-139">Środowisko pracy klienta Blazor wykonaj następujące polecenie z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3aeca-139">For a Blazor client-side experience, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="3aeca-140">Środowisko pracy Blazor po stronie serwera wykonaj następujące polecenie z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3aeca-140">For a Blazor server-side experience, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      <span data-ttu-id="3aeca-141">Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3aeca-141">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3aeca-142">4\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-142">4\.</span></span> <span data-ttu-id="3aeca-143">Otwórz *WebApplication1* folderu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3aeca-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="3aeca-144">5\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-144">5\.</span></span> <span data-ttu-id="3aeca-145">W projekcie po stronie serwera Blazor IDE żądań, dodać zasoby do tworzenia i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="3aeca-145">For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="3aeca-146">Wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="3aeca-146">Select **Yes**.</span></span>

   <span data-ttu-id="3aeca-147">6\.</span><span class="sxs-lookup"><span data-stu-id="3aeca-147">6\.</span></span> <span data-ttu-id="3aeca-148">Jeśli używasz aplikacji po stronie serwera Blazor, uruchom aplikację za pomocą debugera programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3aeca-148">If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="3aeca-149">Jeśli używasz aplikacji po stronie klienta Blazor, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aeca-149">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **ASP.NET Core Blazor (server-side)** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3aeca-150">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3aeca-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="3aeca-151">Środowisko pracy klienta Blazor wykonaj następujące polecenia z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3aeca-151">For a Blazor client-side experience, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3aeca-152">Środowisko pracy Blazor po stronie serwera wykonaj następujące polecenia z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3aeca-152">For a Blazor server-side experience, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3aeca-153">Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3aeca-153">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   ---

<span data-ttu-id="3aeca-154">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="3aeca-154">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="3aeca-155">Wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="3aeca-155">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="3aeca-156">Home</span><span class="sxs-lookup"><span data-stu-id="3aeca-156">Home</span></span>
* <span data-ttu-id="3aeca-157">Licznik</span><span class="sxs-lookup"><span data-stu-id="3aeca-157">Counter</span></span>
* <span data-ttu-id="3aeca-158">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="3aeca-158">Fetch data</span></span>

<span data-ttu-id="3aeca-159">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="3aeca-159">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="3aeca-160">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Razor składniki zapewniają lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="3aeca-160">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="3aeca-161">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="3aeca-161">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="3aeca-162">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="3aeca-162">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="3aeca-163">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="3aeca-163">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="3aeca-164">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="3aeca-164">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="3aeca-165">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="3aeca-165">The `onclick` event is fired.</span></span>
* <span data-ttu-id="3aeca-166">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="3aeca-166">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="3aeca-167">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="3aeca-167">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="3aeca-168">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="3aeca-168">The component is rendered again.</span></span>

<span data-ttu-id="3aeca-169">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="3aeca-169">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="3aeca-170">Dodaj składnik do innego składnika przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="3aeca-170">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="3aeca-171">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="3aeca-171">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="3aeca-172">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="3aeca-172">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="3aeca-173">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="3aeca-173">Run the app.</span></span> <span data-ttu-id="3aeca-174">Strona główna ma swój własny licznika, dostarczone przez składnik licznika.</span><span class="sxs-lookup"><span data-stu-id="3aeca-174">The homepage has its own counter provided by the Counter component.</span></span>

<span data-ttu-id="3aeca-175">Składnik parametry są określane za pomocą atrybutów lub [zawartość elementu podrzędnego](xref:blazor/components#child-content), co pozwala użytkownikowi na ustawianie właściwości w składniku podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="3aeca-175">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="3aeca-176">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@code` bloku:</span><span class="sxs-lookup"><span data-stu-id="3aeca-176">To add a parameter to the Counter component, update the component's `@code` block:</span></span>

* <span data-ttu-id="3aeca-177">Dodaj właściwość `IncrementAmount` z `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3aeca-177">Add a property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="3aeca-178">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="3aeca-178">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="3aeca-179">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="3aeca-179">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="3aeca-180">Określ `IncrementAmount` w składnik indeksu `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3aeca-180">Specify the `IncrementAmount` in the Index component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="3aeca-181">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="3aeca-181">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="3aeca-182">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="3aeca-182">Run the app.</span></span> <span data-ttu-id="3aeca-183">Składnik indeksu ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="3aeca-183">The Index component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="3aeca-184">Składnik Counter (*Counter.razor*) na `/counter` w dalszym ciągu o jeden.</span><span class="sxs-lookup"><span data-stu-id="3aeca-184">The Counter component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3aeca-185">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="3aeca-185">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="3aeca-186">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3aeca-186">Additional resources</span></span>

* <xref:signalr/introduction>
