---
title: Wprowadzenie do składników Razor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę ze składnikami Razor, tworząc i modyfikując projekt składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: razor-components/get-started
ms.openlocfilehash: 86427f9d8a6bc70a65f58ff1b9f8f37c536a97a6
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978334"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="63a86-103">Wprowadzenie do składników Razor</span><span class="sxs-lookup"><span data-stu-id="63a86-103">Get started with Razor Components</span></span>

<span data-ttu-id="63a86-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="63a86-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63a86-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63a86-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="63a86-106">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="63a86-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="63a86-107">Aby utworzyć swój pierwszy projekt Razor składników w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="63a86-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="63a86-108">Wybierz **pliku** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="63a86-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="63a86-109">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="63a86-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="63a86-110">Wybierz **składniki Razor** szablonu, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="63a86-110">Choose the **Razor Components** template and select **OK**.</span></span>
1. <span data-ttu-id="63a86-111">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="63a86-111">Press **F5** to run the app.</span></span>

<span data-ttu-id="63a86-112">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="63a86-112">Congratulations!</span></span> <span data-ttu-id="63a86-113">Po prostu uruchomieniu pierwszej aplikacji składniki Razor!</span><span class="sxs-lookup"><span data-stu-id="63a86-113">You just ran your first Razor Components app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="63a86-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="63a86-114">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="63a86-115">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="63a86-115">Prerequisites:</span></span>

* [<span data-ttu-id="63a86-116">.NET core SDK 3.0 w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="63a86-116">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="63a86-117">Aby utworzyć swój pierwszy projekt składniki Razor z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="63a86-117">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="63a86-118">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="63a86-118">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="63a86-119">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="63a86-119">Congratulations!</span></span> <span data-ttu-id="63a86-120">Po prostu uruchomieniu pierwszej aplikacji składniki Razor!</span><span class="sxs-lookup"><span data-stu-id="63a86-120">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="63a86-121">Razor składników projektów</span><span class="sxs-lookup"><span data-stu-id="63a86-121">Razor Components project</span></span>

<span data-ttu-id="63a86-122">Składniki razor są tworzone za pomocą składni Razor, ale są kompilowane w sposób inny niż widoki stron Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="63a86-122">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="63a86-123">*.Razor* rozszerzenie pliku jest używany do określenia składnika Razor.</span><span class="sxs-lookup"><span data-stu-id="63a86-123">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="63a86-124">Strony razor i MVC widoków w dalszym ciągu używać *.cshtml* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="63a86-124">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="63a86-125">Razor składniki mogą być tworzone za pomocą *.cshtml* rozszerzenie pliku, tak długo, jak te pliki są identyfikowane jako pliki składnika Razor przy użyciu `_RazorComponentInclude` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="63a86-125">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="63a86-126">Na przykład aplikacja utworzona za pomocą szablonu Razor składnika Określa, że wszystkie *.cshtml* plików w obszarze *składniki* folder powinien być traktowany jako składniki Razor:</span><span class="sxs-lookup"><span data-stu-id="63a86-126">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="63a86-127">Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="63a86-127">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="63a86-128">Home</span><span class="sxs-lookup"><span data-stu-id="63a86-128">Home</span></span>
* <span data-ttu-id="63a86-129">Licznik</span><span class="sxs-lookup"><span data-stu-id="63a86-129">Counter</span></span>
* <span data-ttu-id="63a86-130">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="63a86-130">Fetch data</span></span>

<span data-ttu-id="63a86-131">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="63a86-131">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="63a86-132">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="63a86-132">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="63a86-133">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="63a86-133">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="63a86-134">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="63a86-134">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="63a86-135">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="63a86-135">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="63a86-136">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="63a86-136">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="63a86-137">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="63a86-137">The `onclick` event is fired.</span></span>
* <span data-ttu-id="63a86-138">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="63a86-138">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="63a86-139">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="63a86-139">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="63a86-140">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="63a86-140">The component is rendered again.</span></span>

<span data-ttu-id="63a86-141">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="63a86-141">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="63a86-142">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="63a86-142">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="63a86-143">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="63a86-143">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="63a86-144">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="63a86-144">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="63a86-145">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="63a86-145">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="63a86-146">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="63a86-146">Run the app.</span></span> <span data-ttu-id="63a86-147">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="63a86-147">The homepage has its own counter.</span></span>

<span data-ttu-id="63a86-148">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="63a86-148">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="63a86-149">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="63a86-149">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="63a86-150">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="63a86-150">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="63a86-151">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="63a86-151">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="63a86-152">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="63a86-152">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="63a86-153">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="63a86-153">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="63a86-154">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="63a86-154">Run the app.</span></span> <span data-ttu-id="63a86-155">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="63a86-155">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63a86-156">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="63a86-156">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
