---
title: Rozpoczynanie pracy z usługą Blazor
author: guardrex
description: Rozpoczynanie pracy z usługą Blazor, tworząc aplikację Blazor, za pomocą narzędzi dostępnych w wybranym.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/06/2019
uid: blazor/get-started
ms.openlocfilehash: 09613f5d8a4d130f7dca53f31bdd33de527fc776
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969865"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="6ea72-103">Rozpoczynanie pracy z usługą Blazor</span><span class="sxs-lookup"><span data-stu-id="6ea72-103">Get started with Blazor</span></span>

<span data-ttu-id="6ea72-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6ea72-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6ea72-105">Rozpoczynanie pracy z usługą Blazor:</span><span class="sxs-lookup"><span data-stu-id="6ea72-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="6ea72-106">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.</span><span class="sxs-lookup"><span data-stu-id="6ea72-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="6ea72-107">Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6ea72-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview5-19227-01
   ```

1. <span data-ttu-id="6ea72-108">Zgodnie z wytycznymi z dowolnie wybranych narzędzi:</span><span class="sxs-lookup"><span data-stu-id="6ea72-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6ea72-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ea72-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="6ea72-110">1.&nbsp;zainstaluj najnowszą wersję [programu Visual Studio w wersji zapoznawczej](https://visualstudio.com/preview) za pomocą **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="6ea72-110">1.&nbsp;Install the latest [Visual Studio preview](https://visualstudio.com/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="6ea72-111">2.&nbsp;zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ea72-111">2.&nbsp;Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="6ea72-112">Ten krok udostępnia Blazor szablony programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ea72-112">This step makes Blazor templates available to Visual Studio.</span></span>

   <span data-ttu-id="6ea72-113">3.&nbsp;używania stabilne najnowszą wersję programu Visual Studio (nie wersja zapoznawcza), włączyć Visual Studio ma używać zestawów SDK w wersji zapoznawczej: Otwórz **narzędzia** > **opcje** na pasku menu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-113">3.&nbsp;If using the latest stable release of Visual Studio (not a preview release), enable Visual Studio to use preview SDKs: Open **Tools** > **Options** in the menu bar.</span></span> <span data-ttu-id="6ea72-114">Otwórz **projekty i rozwiązania** węzła.</span><span class="sxs-lookup"><span data-stu-id="6ea72-114">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="6ea72-115">Otwórz **platformy .NET Core** kartę. Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="6ea72-115">Open the **.NET Core** tab. Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="6ea72-116">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ea72-116">Select **OK**.</span></span>

   <span data-ttu-id="6ea72-117">4.&nbsp;Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="6ea72-117">4.&nbsp;Create a new project.</span></span>

   <span data-ttu-id="6ea72-118">5.&nbsp;wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6ea72-118">5.&nbsp;Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6ea72-119">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="6ea72-119">Select **Next**.</span></span>

   <span data-ttu-id="6ea72-120">6.&nbsp;Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-120">6.&nbsp;Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="6ea72-121">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-121">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="6ea72-122">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6ea72-122">Select **Create**.</span></span>

   <span data-ttu-id="6ea72-123">7.&nbsp;w **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="6ea72-123">7.&nbsp;In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>

   <span data-ttu-id="6ea72-124">8.&nbsp;Blazor środowisko pracy klienta, wybierz **Blazor (po stronie klienta)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-124">8.&nbsp;For a Blazor client-side experience, choose the **Blazor (client-side)** template.</span></span> <span data-ttu-id="6ea72-125">Środowisko pracy Blazor po stronie serwera, wybierz **Blazor (po stronie serwera)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-125">For a Blazor server-side experience, choose the **Blazor (server-side)** template.</span></span> <span data-ttu-id="6ea72-126">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6ea72-126">Select **Create**.</span></span> <span data-ttu-id="6ea72-127">Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6ea72-127">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6ea72-128">9.&nbsp;naciśnij **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6ea72-128">9.&nbsp;Press **F5** to run the app.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6ea72-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6ea72-129">Visual Studio Code</span></span>](#tab/visual-studio-code)
   
   <span data-ttu-id="6ea72-130">1.&nbsp;zainstalować [programu Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="6ea72-130">1.&nbsp;Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="6ea72-131">2.&nbsp;zainstaluj najnowszą wersję [ C# rozszerzenia programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="6ea72-131">2.&nbsp;Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="6ea72-132">3.&nbsp;Blazor środowisko pracy klienta, wykonaj następujące polecenie z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="6ea72-132">3.&nbsp;For a Blazor client-side experience, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="6ea72-133">Środowisko pracy Blazor po stronie serwera wykonaj następujące polecenie z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="6ea72-133">For a Blazor server-side experience, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      <span data-ttu-id="6ea72-134">Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6ea72-134">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6ea72-135">4.&nbsp;Otwórz *WebApplication1* folderu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6ea72-135">4.&nbsp;Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="6ea72-136">5.&nbsp;projektu po stronie serwera dla Blazor, IDE żądań, dodać zasoby do tworzenia i debugowania projektu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-136">5.&nbsp;For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="6ea72-137">Wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="6ea72-137">Select **Yes**.</span></span>

   <span data-ttu-id="6ea72-138">6.&nbsp;Jeśli za pomocą Blazor aplikacji po stronie serwera, uruchom aplikację za pomocą debugera programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6ea72-138">6.&nbsp;If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="6ea72-139">Jeśli używasz aplikacji po stronie klienta Blazor, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6ea72-139">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1.&nbsp;Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2.&nbsp;Select **File** > **New Solution** or **New Project**.

   3.&nbsp;In the sidebar, select **.NET Core** > **App**.

   4.&nbsp;For a Blazor server-side experience, select the **ASP.NET Core Blazor (server-side)** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5.&nbsp;The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6.&nbsp;In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7.&nbsp;Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6ea72-140">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6ea72-140">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="6ea72-141">Środowisko pracy klienta Blazor wykonaj następujące polecenia z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="6ea72-141">For a Blazor client-side experience, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6ea72-142">Środowisko pracy Blazor po stronie serwera wykonaj następujące polecenia z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="6ea72-142">For a Blazor server-side experience, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6ea72-143">Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6ea72-143">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   ---

<span data-ttu-id="6ea72-144">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6ea72-144">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="6ea72-145">Wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="6ea72-145">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="6ea72-146">Home</span><span class="sxs-lookup"><span data-stu-id="6ea72-146">Home</span></span>
* <span data-ttu-id="6ea72-147">Licznik</span><span class="sxs-lookup"><span data-stu-id="6ea72-147">Counter</span></span>
* <span data-ttu-id="6ea72-148">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="6ea72-148">Fetch data</span></span>

<span data-ttu-id="6ea72-149">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="6ea72-149">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="6ea72-150">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Razor składniki zapewniają lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="6ea72-150">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="6ea72-151">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="6ea72-151">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="6ea72-152">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="6ea72-152">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="6ea72-153">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="6ea72-153">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="6ea72-154">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="6ea72-154">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="6ea72-155">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="6ea72-155">The `onclick` event is fired.</span></span>
* <span data-ttu-id="6ea72-156">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="6ea72-156">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="6ea72-157">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="6ea72-157">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="6ea72-158">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="6ea72-158">The component is rendered again.</span></span>

<span data-ttu-id="6ea72-159">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="6ea72-159">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="6ea72-160">Dodaj składnik do innego składnika przy użyciu składni HTML.</span><span class="sxs-lookup"><span data-stu-id="6ea72-160">Add a component to another component using an HTML syntax.</span></span> <span data-ttu-id="6ea72-161">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="6ea72-161">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="6ea72-162">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-162">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="6ea72-163">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="6ea72-163">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="6ea72-164">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6ea72-164">Run the app.</span></span> <span data-ttu-id="6ea72-165">Strona główna ma swój własny licznika, dostarczone przez składnik licznika.</span><span class="sxs-lookup"><span data-stu-id="6ea72-165">The homepage has its own counter provided by the Counter component.</span></span>

<span data-ttu-id="6ea72-166">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="6ea72-166">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="6ea72-167">Dodaj właściwość `IncrementAmount` z `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-167">Add a property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="6ea72-168">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="6ea72-168">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="6ea72-169">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="6ea72-169">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="6ea72-170">Określ `IncrementAmount` w składnik indeksu `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6ea72-170">Specify the `IncrementAmount` in the Index component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="6ea72-171">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="6ea72-171">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="6ea72-172">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6ea72-172">Run the app.</span></span> <span data-ttu-id="6ea72-173">Składnik indeksu ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="6ea72-173">The Index component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="6ea72-174">Składnik Counter (*Counter.razor*) na `/counter` w dalszym ciągu o jeden.</span><span class="sxs-lookup"><span data-stu-id="6ea72-174">The Counter component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ea72-175">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="6ea72-175">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="6ea72-176">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6ea72-176">Additional resources</span></span>

* <xref:signalr/introduction>
