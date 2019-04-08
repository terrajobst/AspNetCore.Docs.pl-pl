---
title: Rozpoczynanie pracy z usługą Blazor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z Blazor, tworząc i modyfikując projekt Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: spa/blazor/get-started
ms.openlocfilehash: b3928c2812be6f34cdf2f17295a1251106f651e5
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068238"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="f3328-103">Rozpoczynanie pracy z usługą Blazor</span><span class="sxs-lookup"><span data-stu-id="f3328-103">Get started with Blazor</span></span>

<span data-ttu-id="f3328-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f3328-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# [<a name="visual-studio"></a><span data-ttu-id="f3328-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3328-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f3328-106">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="f3328-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="f3328-107">Aby utworzyć swój pierwszy projekt Blazor w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f3328-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="f3328-108">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.</span><span class="sxs-lookup"><span data-stu-id="f3328-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="f3328-109">Włącz Visual Studio ma używać zestawów SDK w wersji zapoznawczej:</span><span class="sxs-lookup"><span data-stu-id="f3328-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="f3328-110">Otwórz **narzędzia** > **opcje** na pasku menu.</span><span class="sxs-lookup"><span data-stu-id="f3328-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="f3328-111">Otwórz **projekty i rozwiązania** węzła.</span><span class="sxs-lookup"><span data-stu-id="f3328-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="f3328-112">Otwórz **platformy .NET Core** kartę.</span><span class="sxs-lookup"><span data-stu-id="f3328-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="f3328-113">Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="f3328-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="f3328-114">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f3328-114">Select **OK**.</span></span>
1. <span data-ttu-id="f3328-115">Zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f3328-115">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="f3328-116">Ten krok udostępnia Blazor szablony programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f3328-116">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="f3328-117">Udostępnia szablony Blazor do użytku z programem .NET Core interfejsu wiersza polecenia, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="f3328-117">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```
1. <span data-ttu-id="f3328-118">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="f3328-118">Create a new project.</span></span>
1. <span data-ttu-id="f3328-119">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f3328-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f3328-120">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="f3328-120">Select **Next**.</span></span>
1. <span data-ttu-id="f3328-121">Podaj nazwę w **Nazwa projektu** pola.</span><span class="sxs-lookup"><span data-stu-id="f3328-121">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="f3328-122">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="f3328-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="f3328-123">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f3328-123">Select **Create**.</span></span>
1. <span data-ttu-id="f3328-124">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="f3328-124">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="f3328-125">Wybierz **Blazor** szablonu, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f3328-125">Select the **Blazor** template and select **Create**.</span></span>
1. <span data-ttu-id="f3328-126">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f3328-126">Press **F5** to run the app.</span></span>

<span data-ttu-id="f3328-127">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="f3328-127">Congratulations!</span></span> <span data-ttu-id="f3328-128">Po prostu uruchomiono swoją pierwszą aplikację Blazor!</span><span class="sxs-lookup"><span data-stu-id="f3328-128">You just ran your first Blazor app!</span></span>

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

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

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

# [<a name="net-core-cli"></a><span data-ttu-id="f3328-129">.NET core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="f3328-129">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="f3328-130">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="f3328-130">Prerequisites:</span></span>

* [<span data-ttu-id="f3328-131">.NET core SDK 3.0 w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="f3328-131">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="f3328-132">Dodaj do niego szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="f3328-132">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="f3328-133">Utwórz swój pierwszy projekt Blazor w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="f3328-133">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="f3328-134">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f3328-134">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="f3328-135">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="f3328-135">Congratulations!</span></span> <span data-ttu-id="f3328-136">Po prostu uruchomiono swoją pierwszą aplikację Blazor!</span><span class="sxs-lookup"><span data-stu-id="f3328-136">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="f3328-137">Blazor projektu</span><span class="sxs-lookup"><span data-stu-id="f3328-137">Blazor project</span></span>

<span data-ttu-id="f3328-138">Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="f3328-138">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="f3328-139">Home</span><span class="sxs-lookup"><span data-stu-id="f3328-139">Home</span></span>
* <span data-ttu-id="f3328-140">Licznik</span><span class="sxs-lookup"><span data-stu-id="f3328-140">Counter</span></span>
* <span data-ttu-id="f3328-141">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="f3328-141">Fetch data</span></span>

<span data-ttu-id="f3328-142">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="f3328-142">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="f3328-143">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Blazor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="f3328-143">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="f3328-144">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f3328-144">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="f3328-145">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="f3328-145">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="f3328-146">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="f3328-146">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="f3328-147">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="f3328-147">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="f3328-148">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="f3328-148">The `onclick` event is fired.</span></span>
* <span data-ttu-id="f3328-149">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f3328-149">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="f3328-150">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="f3328-150">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="f3328-151">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="f3328-151">The component is rendered again.</span></span>

<span data-ttu-id="f3328-152">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="f3328-152">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="f3328-153">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="f3328-153">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="f3328-154">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="f3328-154">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="f3328-155">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="f3328-155">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="f3328-156">W *Pages/Index.cshtml*, Zastąp ankiety monitu składnika ze składnikiem licznika:</span><span class="sxs-lookup"><span data-stu-id="f3328-156">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="f3328-157">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="f3328-157">Run the app.</span></span> <span data-ttu-id="f3328-158">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="f3328-158">The homepage has its own counter.</span></span>

<span data-ttu-id="f3328-159">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="f3328-159">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="f3328-160">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f3328-160">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="f3328-161">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="f3328-161">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="f3328-162">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f3328-162">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="f3328-163">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f3328-163">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="f3328-164">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f3328-164">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="f3328-165">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="f3328-165">Run the app.</span></span> <span data-ttu-id="f3328-166">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="f3328-166">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3328-167">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="f3328-167">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
