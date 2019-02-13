---
title: Wprowadzenie do składników Razor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę ze składnikami Razor, tworząc i modyfikując projekt składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159322"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="8a6f2-103">Wprowadzenie do składników Razor</span><span class="sxs-lookup"><span data-stu-id="8a6f2-103">Get started with Razor Components</span></span>

<span data-ttu-id="8a6f2-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8a6f2-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8a6f2-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a6f2-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8a6f2-106">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="8a6f2-107">Aby utworzyć swój pierwszy projekt Razor składników w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="8a6f2-108">Wybierz **pliku** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="8a6f2-109">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="8a6f2-110">Wybierz **składniki Razor** szablonu, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Nowe okno dialogowe aplikacji](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="8a6f2-112">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="8a6f2-113">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="8a6f2-113">Congratulations!</span></span> <span data-ttu-id="8a6f2-114">Po prostu uruchomieniu pierwszej aplikacji składniki Razor!</span><span class="sxs-lookup"><span data-stu-id="8a6f2-114">You just ran your first Razor Components app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8a6f2-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8a6f2-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="8a6f2-116">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-116">Prerequisites:</span></span>

* [<span data-ttu-id="8a6f2-117">.NET core SDK 3.0 w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="8a6f2-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="8a6f2-118">Aby utworzyć swój pierwszy projekt składniki Razor z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-118">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="8a6f2-119">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-119">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="8a6f2-120">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="8a6f2-120">Congratulations!</span></span> <span data-ttu-id="8a6f2-121">Po prostu uruchomieniu pierwszej aplikacji składniki Razor!</span><span class="sxs-lookup"><span data-stu-id="8a6f2-121">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="8a6f2-122">Razor składników projektów</span><span class="sxs-lookup"><span data-stu-id="8a6f2-122">Razor Components project</span></span>

<span data-ttu-id="8a6f2-123">Rozwiązanie utworzone przez szablon Razor składników zawiera dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-123">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="8a6f2-124">*WebApplication1.Server* &ndash; Projekt serwera jest projektem platformy ASP.NET Core, ustawić, aby hostować aplikację składniki Razor.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-124">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="8a6f2-125">*WebApplication1.App* &ndash; projektu interfejsu użytkownika sieci web po stronie klienta, który używa składników Razor.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-125">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="8a6f2-126">Logika interfejsu użytkownika w *WebApplication1.App* projektu jest oddzielony od pozostałej części aplikacji ze względu na ograniczenia techniczne w ASP.NET Core 3.0 w wersji zapoznawczej 2.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-126">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="8a6f2-127">Rozszerzenie pliku Razor (*.cshtml*) używany dla składników Razor jest również używany widoków stron Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-127">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="8a6f2-128">Obecnie składniki Razor i stron Razor/MVC mają modeli różnych kompilacji, dlatego plikach Razor składniki Razor są oddzielone.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-128">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="8a6f2-129">W przyszłej wersji zapoznawczej, planujemy wprowadzenie nowe rozszerzenie pliku dla składników Razor (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="8a6f2-129">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="8a6f2-130">Składniki, strony i widoki będzie obsługiwana *w tym samym projekcie*.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-130">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="8a6f2-131">Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-131">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="8a6f2-132">Home</span><span class="sxs-lookup"><span data-stu-id="8a6f2-132">Home</span></span>
* <span data-ttu-id="8a6f2-133">Licznik</span><span class="sxs-lookup"><span data-stu-id="8a6f2-133">Counter</span></span>
* <span data-ttu-id="8a6f2-134">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="8a6f2-134">Fetch data</span></span>

<span data-ttu-id="8a6f2-135">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-135">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="8a6f2-136">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-136">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="8a6f2-137">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-137">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="8a6f2-138">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-138">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="8a6f2-139">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-139">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="8a6f2-140">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-140">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="8a6f2-141">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-141">The `onclick` event is fired.</span></span>
* <span data-ttu-id="8a6f2-142">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-142">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="8a6f2-143">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-143">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="8a6f2-144">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-144">The component is rendered again.</span></span>

<span data-ttu-id="8a6f2-145">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="8a6f2-145">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="8a6f2-146">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-146">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="8a6f2-147">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-147">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="8a6f2-148">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-148">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="8a6f2-149">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-149">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="8a6f2-150">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-150">Run the app.</span></span> <span data-ttu-id="8a6f2-151">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-151">The homepage has its own counter.</span></span>

<span data-ttu-id="8a6f2-152">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-152">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="8a6f2-153">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-153">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="8a6f2-154">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-154">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="8a6f2-155">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-155">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="8a6f2-156">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-156">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="8a6f2-157">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8a6f2-157">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="8a6f2-158">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-158">Run the app.</span></span> <span data-ttu-id="8a6f2-159">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="8a6f2-159">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a6f2-160">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="8a6f2-160">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
