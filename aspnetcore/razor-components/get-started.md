---
title: Wprowadzenie do składników Razor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę ze składnikami Razor, tworząc i modyfikując projekt składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068149"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="fc78e-103">Wprowadzenie do składników Razor</span><span class="sxs-lookup"><span data-stu-id="fc78e-103">Get started with Razor Components</span></span>

<span data-ttu-id="fc78e-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fc78e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="fc78e-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fc78e-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fc78e-106">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="fc78e-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="fc78e-107">Aby utworzyć swój pierwszy projekt Razor składników w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="fc78e-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="fc78e-108">Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.</span><span class="sxs-lookup"><span data-stu-id="fc78e-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="fc78e-109">Włącz Visual Studio ma używać zestawów SDK w wersji zapoznawczej:</span><span class="sxs-lookup"><span data-stu-id="fc78e-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="fc78e-110">Otwórz **narzędzia** > **opcje** na pasku menu.</span><span class="sxs-lookup"><span data-stu-id="fc78e-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="fc78e-111">Otwórz **projekty i rozwiązania** węzła.</span><span class="sxs-lookup"><span data-stu-id="fc78e-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="fc78e-112">Otwórz **platformy .NET Core** kartę.</span><span class="sxs-lookup"><span data-stu-id="fc78e-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="fc78e-113">Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="fc78e-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="fc78e-114">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc78e-114">Select **OK**.</span></span>
1. <span data-ttu-id="fc78e-115">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="fc78e-115">Create a new project.</span></span>
1. <span data-ttu-id="fc78e-116">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="fc78e-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="fc78e-117">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="fc78e-117">Select **Next**.</span></span>
1. <span data-ttu-id="fc78e-118">Podaj nazwę w **Nazwa projektu** pola.</span><span class="sxs-lookup"><span data-stu-id="fc78e-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="fc78e-119">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="fc78e-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="fc78e-120">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fc78e-120">Select **Create**.</span></span>
1. <span data-ttu-id="fc78e-121">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="fc78e-121">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="fc78e-122">Wybierz **składniki Razor** szablonu, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fc78e-122">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="fc78e-123">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc78e-123">Press **F5** to run the app.</span></span>

<span data-ttu-id="fc78e-124">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="fc78e-124">Congratulations!</span></span> <span data-ttu-id="fc78e-125">Po prostu uruchomieniu pierwszej aplikacji składniki Razor!</span><span class="sxs-lookup"><span data-stu-id="fc78e-125">You just ran your first Razor Components app!</span></span>

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

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="fc78e-126">.NET core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="fc78e-126">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="fc78e-127">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="fc78e-127">Prerequisites:</span></span>

* [<span data-ttu-id="fc78e-128">.NET core SDK 3.0 w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="fc78e-128">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="fc78e-129">Aby utworzyć swój pierwszy projekt składniki Razor z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="fc78e-129">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="fc78e-130">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="fc78e-130">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="fc78e-131">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="fc78e-131">Congratulations!</span></span> <span data-ttu-id="fc78e-132">Po prostu uruchomieniu pierwszej aplikacji składniki Razor!</span><span class="sxs-lookup"><span data-stu-id="fc78e-132">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="fc78e-133">Razor składników projektów</span><span class="sxs-lookup"><span data-stu-id="fc78e-133">Razor Components project</span></span>

<span data-ttu-id="fc78e-134">Składniki razor są tworzone za pomocą składni Razor, ale są kompilowane w sposób inny niż widoki stron Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="fc78e-134">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="fc78e-135">*.Razor* rozszerzenie pliku jest używany do określenia składnika Razor.</span><span class="sxs-lookup"><span data-stu-id="fc78e-135">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="fc78e-136">Strony razor i MVC widoków w dalszym ciągu używać *.cshtml* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="fc78e-136">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="fc78e-137">Razor składniki mogą być tworzone za pomocą *.cshtml* rozszerzenie pliku, tak długo, jak te pliki są identyfikowane jako pliki składnika Razor przy użyciu `_RazorComponentInclude` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="fc78e-137">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="fc78e-138">Na przykład aplikacja utworzona za pomocą szablonu Razor składnika Określa, że wszystkie *.cshtml* plików w obszarze *składniki* folder powinien być traktowany jako składniki Razor:</span><span class="sxs-lookup"><span data-stu-id="fc78e-138">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="fc78e-139">Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="fc78e-139">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="fc78e-140">Home</span><span class="sxs-lookup"><span data-stu-id="fc78e-140">Home</span></span>
* <span data-ttu-id="fc78e-141">Licznik</span><span class="sxs-lookup"><span data-stu-id="fc78e-141">Counter</span></span>
* <span data-ttu-id="fc78e-142">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="fc78e-142">Fetch data</span></span>

<span data-ttu-id="fc78e-143">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="fc78e-143">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="fc78e-144">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="fc78e-144">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="fc78e-145">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="fc78e-145">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="fc78e-146">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="fc78e-146">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="fc78e-147">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="fc78e-147">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="fc78e-148">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="fc78e-148">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="fc78e-149">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="fc78e-149">The `onclick` event is fired.</span></span>
* <span data-ttu-id="fc78e-150">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="fc78e-150">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="fc78e-151">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="fc78e-151">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="fc78e-152">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="fc78e-152">The component is rendered again.</span></span>

<span data-ttu-id="fc78e-153">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="fc78e-153">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="fc78e-154">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="fc78e-154">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="fc78e-155">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="fc78e-155">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="fc78e-156">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="fc78e-156">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="fc78e-157">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="fc78e-157">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="fc78e-158">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="fc78e-158">Run the app.</span></span> <span data-ttu-id="fc78e-159">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="fc78e-159">The homepage has its own counter.</span></span>

<span data-ttu-id="fc78e-160">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="fc78e-160">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="fc78e-161">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="fc78e-161">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="fc78e-162">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="fc78e-162">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="fc78e-163">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="fc78e-163">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="fc78e-164">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="fc78e-164">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="fc78e-165">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="fc78e-165">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="fc78e-166">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="fc78e-166">Run the app.</span></span> <span data-ttu-id="fc78e-167">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="fc78e-167">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc78e-168">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="fc78e-168">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
