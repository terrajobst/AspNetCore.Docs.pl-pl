---
title: Rozpoczynanie pracy z usługą Blazor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2019
uid: blazor/get-started
ms.openlocfilehash: 45ae0acc6aaee433cce4eddb2fe9c59c306581d7
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982670"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="3dd9e-103">Rozpoczynanie pracy z usługą Blazor</span><span class="sxs-lookup"><span data-stu-id="3dd9e-103">Get started with Blazor</span></span>

<span data-ttu-id="3dd9e-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3dd9e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3dd9e-105">W kilku krokach wprowadzenie Blazor:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-105">In a few steps, get started with Blazor:</span></span>

1. <span data-ttu-id="3dd9e-106">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="3dd9e-107">Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview4-19216-03
   ```

1. <span data-ttu-id="3dd9e-108">Zgodnie z wytycznymi z dowolnie wybranych narzędzi:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dd9e-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dd9e-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="3dd9e-110">1.&nbsp;zainstalowania najnowszej wersji zapoznawczej [Visual Studio 2019](https://visualstudio.com/preview) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-110">1.&nbsp;Install the latest preview of [Visual Studio 2019](https://visualstudio.com/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="3dd9e-111">2.&nbsp;zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-111">2.&nbsp;Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="3dd9e-112">Ten krok udostępnia Blazor szablony programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-112">This step makes Blazor templates available to Visual Studio.</span></span>

   <span data-ttu-id="3dd9e-113">3.&nbsp;Włącz programu Visual Studio, aby użyć zestawów SDK w wersji zapoznawczej: Otwórz **narzędzia** > **opcje** na pasku menu.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-113">3.&nbsp;Enable Visual Studio to use preview SDKs: Open **Tools** > **Options** in the menu bar.</span></span> <span data-ttu-id="3dd9e-114">Otwórz **projekty i rozwiązania** węzła.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-114">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="3dd9e-115">Otwórz **platformy .NET Core** kartę. Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-115">Open the **.NET Core** tab. Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="3dd9e-116">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-116">Select **OK**.</span></span>

   <span data-ttu-id="3dd9e-117">4.&nbsp;Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-117">4.&nbsp;Create a new project.</span></span>

   <span data-ttu-id="3dd9e-118">5.&nbsp;wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-118">5.&nbsp;Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="3dd9e-119">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-119">Select **Next**.</span></span>

   <span data-ttu-id="3dd9e-120">6.&nbsp;Podaj nazwę w **Nazwa projektu** pola.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-120">6.&nbsp;Provide a name in the **Project name** field.</span></span> <span data-ttu-id="3dd9e-121">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-121">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="3dd9e-122">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-122">Select **Create**.</span></span>

   <span data-ttu-id="3dd9e-123">7.&nbsp;upewnij się, **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-123">7.&nbsp;Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>

   <span data-ttu-id="3dd9e-124">8.&nbsp;doświadczenia z Blazor po stronie klienta, można wybrać **Blazor (po stronie klienta)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-124">8.&nbsp;For an experience with Blazor client-side, choose the **Blazor (client-side)** template.</span></span> <span data-ttu-id="3dd9e-125">To środowisko z Blazor po stronie serwera wybierz **Blazor (po stronie serwera)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-125">For an experience with Blazor server-side, choose the **Blazor (server-side)** template.</span></span> <span data-ttu-id="3dd9e-126">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-126">Select **Create**.</span></span>

   <span data-ttu-id="3dd9e-127">9.&nbsp;naciśnij **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-127">9.&nbsp;Press **F5** to run the app.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3dd9e-128">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3dd9e-128">Visual Studio Code</span></span>](#tab/visual-studio-code)
   
   <span data-ttu-id="3dd9e-129">1.&nbsp;zainstalować [programu Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="3dd9e-129">1.&nbsp;Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="3dd9e-130">2.&nbsp;zainstaluj najnowszą wersję [ C# rozszerzenia programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="3dd9e-130">2.&nbsp;Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="3dd9e-131">3.&nbsp;środowisko pracy z Blazor po stronie klienta, wykonaj następujące polecenie z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-131">3.&nbsp;For an experience with Blazor client-side, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="3dd9e-132">Środowisko pracy z Blazor po stronie serwera wykonaj następujące polecenie z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-132">For an experience with Blazor server-side, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      > [!NOTE]
      > <span data-ttu-id="3dd9e-133">Tylko Blazor po stronie klienta jest obsługiwana w systemie macOS w ASP.NET Core 3.0 w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-133">Only Blazor client-side is supported on macOS in ASP.NET Core 3.0 Preview 4.</span></span> <span data-ttu-id="3dd9e-134">Aby uzyskać więcej informacji, zobacz [po stronie serwera Blazor: dotnet, uruchom kończy się niepowodzeniem z InvalidOperationException w systemie MacOS](https://github.com/aspnet/AspNetCore/issues/9402).</span><span class="sxs-lookup"><span data-stu-id="3dd9e-134">For more information, see [Blazor server side: dotnet run fails with InvalidOperationException on MacOS](https://github.com/aspnet/AspNetCore/issues/9402).</span></span>

   <span data-ttu-id="3dd9e-135">4.&nbsp;Otwórz *WebApplication1* folderu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-135">4.&nbsp;Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="3dd9e-136">5.&nbsp;po wyświetleniu monitu przez Visual Studio Code dla projektu Blazor po stronie serwera można dodać zasoby wymagane do kompilowania i debugowania projektu, wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-136">5.&nbsp;When prompted by Visual Studio Code for a Blazor server-side project to add required assets to build and debug the project, select **Yes**.</span></span>

   <span data-ttu-id="3dd9e-137">6.&nbsp;Jeśli za pomocą Blazor aplikacji po stronie serwera, uruchom aplikację za pomocą debugera programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-137">6.&nbsp;If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="3dd9e-138">Jeśli używasz aplikacji po stronie klienta Blazor, wykonaj `dotnet run` z folderu projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-138">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1.&nbsp;Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2.&nbsp;Select **File** > **New Solution** or **New Project**.

   3.&nbsp;In the sidebar, select **.NET Core** > **App**.

   4.&nbsp;For an experience with Blazor server-side, select the **ASP.NET Core Blazor (server-side)** template. For an experience with Blazor server-side, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**.

   5.&nbsp;The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6.&nbsp;In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7.&nbsp;Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dd9e-139">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3dd9e-139">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="3dd9e-140">Środowisko pracy z Blazor po stronie klienta wykonaj następujące polecenia z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-140">For an experience with Blazor client-side, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3dd9e-141">Środowisko pracy z Blazor po stronie serwera wykonaj następujące polecenia z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-141">For an experience with Blazor server-side, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   > [!NOTE]
   > <span data-ttu-id="3dd9e-142">W systemie macOS należy użyć Blazor aplikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-142">On macOS, use a Blazor client-side app.</span></span> <span data-ttu-id="3dd9e-143">Blazor po stronie serwera nie jest obsługiwana dla systemu macOS na platformy ASP.NET Core 3.0 w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-143">Blazor server-side isn't supported for macOS on ASP.NET Core 3.0 Preview 4.</span></span> <span data-ttu-id="3dd9e-144">Aby uzyskać więcej informacji, zobacz [po stronie serwera Blazor: dotnet, uruchom kończy się niepowodzeniem z InvalidOperationException w systemie MacOS](https://github.com/aspnet/AspNetCore/issues/9402).</span><span class="sxs-lookup"><span data-stu-id="3dd9e-144">For more information, see [Blazor server side: dotnet run fails with InvalidOperationException on MacOS](https://github.com/aspnet/AspNetCore/issues/9402).</span></span>

   ---

<span data-ttu-id="3dd9e-145">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-145">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="3dd9e-146">Wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-146">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="3dd9e-147">Home</span><span class="sxs-lookup"><span data-stu-id="3dd9e-147">Home</span></span>
* <span data-ttu-id="3dd9e-148">Licznik</span><span class="sxs-lookup"><span data-stu-id="3dd9e-148">Counter</span></span>
* <span data-ttu-id="3dd9e-149">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="3dd9e-149">Fetch data</span></span>

<span data-ttu-id="3dd9e-150">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-150">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="3dd9e-151">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Razor składniki zapewniają lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-151">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="3dd9e-152">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-152">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="3dd9e-153">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-153">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="3dd9e-154">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-154">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="3dd9e-155">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-155">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="3dd9e-156">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-156">The `onclick` event is fired.</span></span>
* <span data-ttu-id="3dd9e-157">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-157">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="3dd9e-158">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-158">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="3dd9e-159">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-159">The component is rendered again.</span></span>

<span data-ttu-id="3dd9e-160">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="3dd9e-160">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="3dd9e-161">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-161">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="3dd9e-162">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-162">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="3dd9e-163">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-163">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="3dd9e-164">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-164">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="3dd9e-165">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-165">Run the app.</span></span> <span data-ttu-id="3dd9e-166">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-166">The homepage has its own counter.</span></span>

<span data-ttu-id="3dd9e-167">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-167">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="3dd9e-168">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-168">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="3dd9e-169">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-169">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="3dd9e-170">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-170">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4-5,9)]

<span data-ttu-id="3dd9e-171">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-171">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="3dd9e-172">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="3dd9e-172">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="3dd9e-173">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-173">Run the app.</span></span> <span data-ttu-id="3dd9e-174">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="3dd9e-174">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dd9e-175">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="3dd9e-175">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="3dd9e-176">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3dd9e-176">Additional resources</span></span>

* <xref:signalr/introduction>
