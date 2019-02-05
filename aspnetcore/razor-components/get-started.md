---
title: Wprowadzenie do składników Razor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z architekturą składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: 83f8bbcd415c6776ef14e1ce789f3f0a8cecc464
ms.sourcegitcommit: a91e8dd2f4b788114c8bc834507277f4b5e8d6c5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712229"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="322c5-103">Wprowadzenie do składników Razor</span><span class="sxs-lookup"><span data-stu-id="322c5-103">Get started with Razor Components</span></span>

<span data-ttu-id="322c5-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="322c5-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="322c5-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="322c5-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="322c5-106">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="322c5-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="322c5-107">Aby utworzyć swój pierwszy projekt Razor składników w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="322c5-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="322c5-108">Wybierz **pliku** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="322c5-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="322c5-109">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="322c5-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="322c5-110">Wybierz **składniki Razor** szablonu, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="322c5-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Nowe okno dialogowe aplikacji](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="322c5-112">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="322c5-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="322c5-113">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="322c5-113">Congratulations!</span></span> <span data-ttu-id="322c5-114">Po prostu uruchomieniu pierwszej aplikacji składniki Razor!</span><span class="sxs-lookup"><span data-stu-id="322c5-114">You just ran your first Razor Components app!</span></span>

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

Congrats! You just ran your first Razor Components app!

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="322c5-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="322c5-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="322c5-116">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="322c5-116">Prerequisites:</span></span>

* [<span data-ttu-id="322c5-117">.NET core SDK 3.0 w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="322c5-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

<span data-ttu-id="322c5-118">Aby utworzyć swój pierwszy projekt składniki Razor z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="322c5-118">To create your first Razor Components project from a command shell:</span></span>

```console
dotnet new razorcomponents -o WebApplication1
cd WebApplication1
dotnet run
```

<span data-ttu-id="322c5-119">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="322c5-119">Congratulations!</span></span> <span data-ttu-id="322c5-120">Po prostu uruchomieniu pierwszej aplikacji składniki Razor!</span><span class="sxs-lookup"><span data-stu-id="322c5-120">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="322c5-121">Razor składników projektów</span><span class="sxs-lookup"><span data-stu-id="322c5-121">Razor Components project</span></span>

<span data-ttu-id="322c5-122">Rozwiązanie utworzone przez szablon Razor składników zawiera dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="322c5-122">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="322c5-123">*WebApplication1.Server* &ndash; Projekt serwera jest projektem platformy ASP.NET Core, ustawić, aby hostować aplikację składniki Razor.</span><span class="sxs-lookup"><span data-stu-id="322c5-123">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="322c5-124">*WebApplication1.App* &ndash; projektu interfejsu użytkownika sieci web po stronie klienta, który używa składników Razor.</span><span class="sxs-lookup"><span data-stu-id="322c5-124">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="322c5-125">Logika interfejsu użytkownika w *WebApplication1.App* projektu jest oddzielony od pozostałej części aplikacji ze względu na ograniczenia techniczne w ASP.NET Core 3.0 w wersji zapoznawczej 2.</span><span class="sxs-lookup"><span data-stu-id="322c5-125">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="322c5-126">Rozszerzenie pliku Razor (*.cshtml*) używany dla składników Razor jest również używany widoków stron Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="322c5-126">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="322c5-127">Obecnie składniki Razor i stron Razor/MVC mają modeli różnych kompilacji, dlatego plikach Razor składniki Razor są oddzielone.</span><span class="sxs-lookup"><span data-stu-id="322c5-127">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="322c5-128">W przyszłej wersji zapoznawczej, planujemy wprowadzenie nowe rozszerzenie pliku dla składników Razor (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="322c5-128">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="322c5-129">Składniki, strony i widoki będzie obsługiwana *w tym samym projekcie*.</span><span class="sxs-lookup"><span data-stu-id="322c5-129">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="322c5-130">Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="322c5-130">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="322c5-131">Home</span><span class="sxs-lookup"><span data-stu-id="322c5-131">Home</span></span>
* <span data-ttu-id="322c5-132">Licznik</span><span class="sxs-lookup"><span data-stu-id="322c5-132">Counter</span></span>
* <span data-ttu-id="322c5-133">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="322c5-133">Fetch data</span></span>

<span data-ttu-id="322c5-134">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="322c5-134">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="322c5-135">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="322c5-135">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="322c5-136">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="322c5-136">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="322c5-137">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="322c5-137">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="322c5-138">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="322c5-138">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="322c5-139">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="322c5-139">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="322c5-140">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="322c5-140">The `onclick` event is fired.</span></span>
* <span data-ttu-id="322c5-141">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="322c5-141">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="322c5-142">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="322c5-142">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="322c5-143">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="322c5-143">The component is rendered again.</span></span>

<span data-ttu-id="322c5-144">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="322c5-144">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="322c5-145">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="322c5-145">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="322c5-146">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="322c5-146">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="322c5-147">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="322c5-147">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="322c5-148">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="322c5-148">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="322c5-149">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="322c5-149">Run the app.</span></span> <span data-ttu-id="322c5-150">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="322c5-150">The homepage has its own counter.</span></span>

<span data-ttu-id="322c5-151">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="322c5-151">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="322c5-152">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="322c5-152">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="322c5-153">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="322c5-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="322c5-154">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="322c5-154">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="322c5-155">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="322c5-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="322c5-156">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="322c5-156">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="322c5-157">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="322c5-157">Run the app.</span></span> <span data-ttu-id="322c5-158">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="322c5-158">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="322c5-159">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="322c5-159">Next steps</span></span>

<xref:tutorials/first-blazor-app>
