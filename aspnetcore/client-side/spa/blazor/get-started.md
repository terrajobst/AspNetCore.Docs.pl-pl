---
title: Rozpoczynanie pracy z usługą Blazor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z Blazor, tworząc i modyfikując projekt Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 667c57d536450fa2f8ae1cabc7c5a76a16d38a55
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665590"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="c7165-103">Rozpoczynanie pracy z usługą Blazor</span><span class="sxs-lookup"><span data-stu-id="c7165-103">Get started with Blazor</span></span>

<span data-ttu-id="c7165-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c7165-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c7165-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7165-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c7165-106">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="c7165-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="c7165-107">Aby utworzyć swój pierwszy projekt Blazor w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c7165-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="c7165-108">Zainstaluj najnowszą wersję [rozszerzenia usług językowych Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7165-108">Install the latest [Blazor Language Services extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="c7165-109">Ten krok udostępnia Blazor szablony programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7165-109">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="c7165-110">Udostępnia szablony Blazor do użytku z programem .NET Core interfejsu wiersza polecenia, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="c7165-110">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="c7165-111">Wybierz **pliku** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c7165-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="c7165-112">Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="c7165-112">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="c7165-113">Wybierz **Blazor** szablonu, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7165-113">Choose the **Blazor** template and select **OK**.</span></span>
1. <span data-ttu-id="c7165-114">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7165-114">Press **F5** to run the app.</span></span>

<span data-ttu-id="c7165-115">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="c7165-115">Congratulations!</span></span> <span data-ttu-id="c7165-116">Po prostu uruchomiono swoją pierwszą aplikację Blazor!</span><span class="sxs-lookup"><span data-stu-id="c7165-116">You just ran your first Blazor app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c7165-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c7165-117">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="c7165-118">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="c7165-118">Prerequisites:</span></span>

* [<span data-ttu-id="c7165-119">.NET core SDK 3.0 w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="c7165-119">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="c7165-120">Dodaj do niego szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="c7165-120">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="c7165-121">Utwórz swój pierwszy projekt Blazor w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="c7165-121">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="c7165-122">W przeglądarce przejdź do `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c7165-122">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="c7165-123">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="c7165-123">Congratulations!</span></span> <span data-ttu-id="c7165-124">Po prostu uruchomiono swoją pierwszą aplikację Blazor!</span><span class="sxs-lookup"><span data-stu-id="c7165-124">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="c7165-125">Blazor projektu</span><span class="sxs-lookup"><span data-stu-id="c7165-125">Blazor project</span></span>

<span data-ttu-id="c7165-126">Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:</span><span class="sxs-lookup"><span data-stu-id="c7165-126">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="c7165-127">Home</span><span class="sxs-lookup"><span data-stu-id="c7165-127">Home</span></span>
* <span data-ttu-id="c7165-128">Licznik</span><span class="sxs-lookup"><span data-stu-id="c7165-128">Counter</span></span>
* <span data-ttu-id="c7165-129">Pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="c7165-129">Fetch data</span></span>

<span data-ttu-id="c7165-130">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="c7165-130">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="c7165-131">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Blazor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="c7165-131">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="c7165-132">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7165-132">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="c7165-133">Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="c7165-133">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="c7165-134">Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="c7165-134">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="c7165-135">Każdorazowo **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="c7165-135">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="c7165-136">`onclick` Jest wyzwalane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="c7165-136">The `onclick` event is fired.</span></span>
* <span data-ttu-id="c7165-137">`IncrementCount` Metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c7165-137">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="c7165-138">`currentCount` Jest zwiększany.</span><span class="sxs-lookup"><span data-stu-id="c7165-138">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="c7165-139">Składnik jest renderowany ponownie.</span><span class="sxs-lookup"><span data-stu-id="c7165-139">The component is rendered again.</span></span>

<span data-ttu-id="c7165-140">Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="c7165-140">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="c7165-141">Dodaj składnik do innego składnika przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="c7165-141">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="c7165-142">Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="c7165-142">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="c7165-143">Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="c7165-143">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="c7165-144">W *Pages/Index.cshtml*, Zastąp ankiety monitu składnika ze składnikiem licznika:</span><span class="sxs-lookup"><span data-stu-id="c7165-144">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="c7165-145">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="c7165-145">Run the app.</span></span> <span data-ttu-id="c7165-146">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="c7165-146">The homepage has its own counter.</span></span>

<span data-ttu-id="c7165-147">Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:</span><span class="sxs-lookup"><span data-stu-id="c7165-147">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="c7165-148">Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c7165-148">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="c7165-149">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="c7165-149">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="c7165-150">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7165-150">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="c7165-151">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c7165-151">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="c7165-152">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7165-152">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="c7165-153">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="c7165-153">Run the app.</span></span> <span data-ttu-id="c7165-154">Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="c7165-154">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7165-155">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c7165-155">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
