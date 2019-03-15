---
title: Tworzenie pierwszej aplikacji składniki Razor
author: guardrex
description: Tworzenie składników Razor aplikacji krok po kroku i pojęcia dotyczące podstawowych składników Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: c0f7b27fdfc770f8001625ecb3bf8d50af517b99
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978426"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="6b55d-103">Tworzenie pierwszej aplikacji składniki Razor</span><span class="sxs-lookup"><span data-stu-id="6b55d-103">Build your first Razor Components app</span></span>

<span data-ttu-id="6b55d-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6b55d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="6b55d-105">W tym samouczku pokazano, jak utworzyć aplikację ze składnikami Razor i pokazuje podstawowe pojęcia składniki Razor.</span><span class="sxs-lookup"><span data-stu-id="6b55d-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="6b55d-106">Można korzystać z tego samouczka przy użyciu obu składników Razor na podstawie projektu (obsługiwane w programie .NET Core 3.0 lub nowszej) lub projektu na podstawie Blazor (obsługiwane w przyszłej wersji programu .NET Core).</span><span class="sxs-lookup"><span data-stu-id="6b55d-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="6b55d-107">Środowisko za pomocą platformy ASP.NET Core Razor składników (*zalecane*):</span><span class="sxs-lookup"><span data-stu-id="6b55d-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="6b55d-108">Postępuj zgodnie ze wskazówkami w <xref:razor-components/get-started> do tworzenia projektu na podstawie składników Razor.</span><span class="sxs-lookup"><span data-stu-id="6b55d-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="6b55d-109">Nadaj projektowi nazwę `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="6b55d-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="6b55d-110">Aby uzyskać doświadczenie w korzystaniu z Blazor:</span><span class="sxs-lookup"><span data-stu-id="6b55d-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="6b55d-111">Postępuj zgodnie ze wskazówkami w <xref:spa/blazor/get-started> do tworzenia projektu na podstawie Blazor.</span><span class="sxs-lookup"><span data-stu-id="6b55d-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="6b55d-112">Nadaj projektowi nazwę `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="6b55d-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="6b55d-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6b55d-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="6b55d-114">Zobacz następujące tematy dotyczące wymagań wstępnych:</span><span class="sxs-lookup"><span data-stu-id="6b55d-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="6b55d-115">Tworzenie składników</span><span class="sxs-lookup"><span data-stu-id="6b55d-115">Build components</span></span>

1. <span data-ttu-id="6b55d-116">Przejdź do każdego z trzech stron aplikacji w *składniki/strony* folder (*stron* w Blazor): Strona główna licznik i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="6b55d-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="6b55d-117">Te strony są implementowane przez pliki Razor składników: *Index.razor*, *Counter.razor*, i *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="6b55d-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="6b55d-118">(Blazor w dalszym ciągu używa *.cshtml* rozszerzenie pliku: *Index.cshtml*, *Counter.cshtml*, i *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="6b55d-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="6b55d-119">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="6b55d-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="6b55d-120">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="6b55d-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="6b55d-121">Zapoznania się z implementacją składnikiem licznika w *Counter.razor* pliku.</span><span class="sxs-lookup"><span data-stu-id="6b55d-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="6b55d-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="6b55d-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="6b55d-123">Interfejs użytkownika składnika licznika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="6b55d-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="6b55d-124">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="6b55d-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="6b55d-125">Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6b55d-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="6b55d-126">Nazwa wygenerowanej klasy .NET zgodny z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="6b55d-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="6b55d-127">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="6b55d-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="6b55d-128">W `@functions` blokować stan składnika (właściwości, pola) i metod są określone dla obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="6b55d-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="6b55d-129">Te elementy członkowskie są następnie używane w ramach składnika renderowania logiki oraz obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="6b55d-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="6b55d-130">Gdy **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="6b55d-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="6b55d-131">Zarejestrowane składnik licznika `onclick` program obsługi jest wywoływany ( `IncrementCount` metody).</span><span class="sxs-lookup"><span data-stu-id="6b55d-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="6b55d-132">Składnik licznika generuje drzewo jego renderowania.</span><span class="sxs-lookup"><span data-stu-id="6b55d-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="6b55d-133">Nowe drzewo renderowania w porównaniu do poprzedniego.</span><span class="sxs-lookup"><span data-stu-id="6b55d-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="6b55d-134">Modyfikacje do modelu DOM (Document Object) są stosowane.</span><span class="sxs-lookup"><span data-stu-id="6b55d-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="6b55d-135">Liczba wyświetlanych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="6b55d-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="6b55d-136">Modyfikowanie C# logiki licznika składnika zwiększenie liczby dwa, a nie jeden.</span><span class="sxs-lookup"><span data-stu-id="6b55d-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="6b55d-137">Ponownie skompiluj i uruchom aplikację, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="6b55d-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="6b55d-138">Wybierz **kliknij mnie** przycisk i zwiększa licznik przez dwa.</span><span class="sxs-lookup"><span data-stu-id="6b55d-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="6b55d-139">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="6b55d-139">Use components</span></span>

<span data-ttu-id="6b55d-140">Uwzględnij składnika w innym składniku przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="6b55d-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="6b55d-141">Dodaj składnik licznika do aplikacji, składnik indeksu (strona główna), dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="6b55d-141">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="6b55d-142">Jeśli używasz Blazor dla tego środowiska, składnik ankiety monitu (`<SurveyPrompt>` elementu) znajduje się w składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="6b55d-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="6b55d-143">Zastąp `<SurveyPrompt>` element z `<Counter>` elementu.</span><span class="sxs-lookup"><span data-stu-id="6b55d-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="6b55d-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="6b55d-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="6b55d-145">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b55d-145">Rebuild and run the app.</span></span> <span data-ttu-id="6b55d-146">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="6b55d-146">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="6b55d-147">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="6b55d-147">Component parameters</span></span>

<span data-ttu-id="6b55d-148">Składniki mogą także mieć parametrów.</span><span class="sxs-lookup"><span data-stu-id="6b55d-148">Components can also have parameters.</span></span> <span data-ttu-id="6b55d-149">Składnik parametry są definiowane za pomocą właściwości niepubliczne klasy składnika ozdobione `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="6b55d-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="6b55d-150">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="6b55d-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="6b55d-151">Aktualizowanie składnika `@functions` C# kodu:</span><span class="sxs-lookup"><span data-stu-id="6b55d-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="6b55d-152">Dodaj `IncrementAmount` ozdobione właściwość `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6b55d-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="6b55d-153">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="6b55d-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="6b55d-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="6b55d-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="6b55d-155">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6b55d-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="6b55d-156">Ustaw wartość do zwiększenia licznika dziesięciu.</span><span class="sxs-lookup"><span data-stu-id="6b55d-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="6b55d-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="6b55d-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="6b55d-158">Ponownie załaduj stronę.</span><span class="sxs-lookup"><span data-stu-id="6b55d-158">Reload the page.</span></span> <span data-ttu-id="6b55d-159">Licznik strony głównej rośnie przez dziesięć za każdym razem **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="6b55d-159">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="6b55d-160">Licznik na *licznika* stronie zwiększa o jeden.</span><span class="sxs-lookup"><span data-stu-id="6b55d-160">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="6b55d-161">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="6b55d-161">Route to components</span></span>

<span data-ttu-id="6b55d-162">`@page` Dyrektywę w górnej części *Counter.razor* pliku Określa, że ten składnik jest routingu punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="6b55d-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="6b55d-163">Składnik licznika obsługuje żądania wysyłane do `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="6b55d-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="6b55d-164">Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal można używać składnika przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="6b55d-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="6b55d-165">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="6b55d-165">Dependency injection</span></span>

<span data-ttu-id="6b55d-166">Zarejestrowane w kontenerze usługi app Services są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6b55d-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6b55d-167">Wstrzyknięcie usług do składnika za pomocą `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="6b55d-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="6b55d-168">Sprawdź dyrektywy składnika FetchData.</span><span class="sxs-lookup"><span data-stu-id="6b55d-168">Examine the directives of the FetchData component.</span></span> <span data-ttu-id="6b55d-169">`@inject` Dyrektywy służy do dodania wystąpienia programu `WeatherForecastService` usługi do składnika:</span><span class="sxs-lookup"><span data-stu-id="6b55d-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

<span data-ttu-id="6b55d-170">*Components/Pages/FetchData.razor* (*Pages/FetchData.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="6b55d-170">*Components/Pages/FetchData.razor* (*Pages/FetchData.cshtml* in Blazor):</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="6b55d-171">`WeatherForecastService` Usługi jest zarejestrowana jako [pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b55d-171">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="6b55d-172">Składnik FetchData używa usługi wprowadzonego jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:</span><span class="sxs-lookup"><span data-stu-id="6b55d-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="6b55d-173">A [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli danych o pogodzie:</span><span class="sxs-lookup"><span data-stu-id="6b55d-173">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="6b55d-174">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="6b55d-174">Build a todo list</span></span>

<span data-ttu-id="6b55d-175">Dodaj nową stronę do aplikacji, która implementuje listy zadań do wykonania prostego.</span><span class="sxs-lookup"><span data-stu-id="6b55d-175">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="6b55d-176">Dodaj pusty plik do *składniki/strony* folder (*stron* folderu w Blazor) o nazwie *Todo.razor*.</span><span class="sxs-lookup"><span data-stu-id="6b55d-176">Add an empty file to the *Components/Pages* folder (*Pages* folder in Blazor) named *Todo.razor*.</span></span>

1. <span data-ttu-id="6b55d-177">Podaj początkowe znaczniki dla strony:</span><span class="sxs-lookup"><span data-stu-id="6b55d-177">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="6b55d-178">Na stronie zadań do wykonania należy dodać do paska nawigacyjnego.</span><span class="sxs-lookup"><span data-stu-id="6b55d-178">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="6b55d-179">Składnik NavMenu (*Components/Shared/NavMenu.razor* lub *Shared/NavMenu.cshtml* w Blazor) jest używana w układzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b55d-179">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="6b55d-180">Układy są składniki, które pozwalają uniknąć duplikowania zawartości w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b55d-180">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="6b55d-181">Aby uzyskać więcej informacji, zobacz <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="6b55d-181">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="6b55d-182">Dodaj `<NavLink>` strony Todo, dodając następujące znaczniki elementu listy poniżej istniejące elementy listy w *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* w Blazor) pliku:</span><span class="sxs-lookup"><span data-stu-id="6b55d-182">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="6b55d-183">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b55d-183">Rebuild and run the app.</span></span> <span data-ttu-id="6b55d-184">Odwiedź nową stronę Todo, aby upewnić się, czy działa link do strony zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6b55d-184">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="6b55d-185">Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasa, która reprezentuje element todo.</span><span class="sxs-lookup"><span data-stu-id="6b55d-185">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="6b55d-186">Należy użyć następującego C# kod `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="6b55d-186">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="6b55d-187">Wróć do części Todo (*Components/Pages/Todo.razor* lub *Pages/Todo.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="6b55d-187">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="6b55d-188">Dodaj pole do zadań do wykonania w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="6b55d-188">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="6b55d-189">Składnik Todo to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="6b55d-189">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="6b55d-190">Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy.</span><span class="sxs-lookup"><span data-stu-id="6b55d-190">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="6b55d-191">Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadań do wykonania do listy.</span><span class="sxs-lookup"><span data-stu-id="6b55d-191">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="6b55d-192">Dodaj przycisk poniżej listy i tekstowych danych wejściowych:</span><span class="sxs-lookup"><span data-stu-id="6b55d-192">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="6b55d-193">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b55d-193">Rebuild and run the app.</span></span> <span data-ttu-id="6b55d-194">Gdy **dodawania zadań do wykonania** przycisk jest zaznaczony, nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest powiązaną przycisku.</span><span class="sxs-lookup"><span data-stu-id="6b55d-194">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="6b55d-195">Dodaj `AddTodo` metodę, aby składnik zadań do wykonania i zarejestruj go dla przycisku kliknie, za pomocą `onclick` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="6b55d-195">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="6b55d-196">`AddTodo` C# Metoda jest wywoływana po wybraniu przycisku.</span><span class="sxs-lookup"><span data-stu-id="6b55d-196">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="6b55d-197">Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` ciąg pola i powiązać wartość wprowadzania przy użyciu tekstu `bind` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="6b55d-197">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="6b55d-198">Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy.</span><span class="sxs-lookup"><span data-stu-id="6b55d-198">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="6b55d-199">Wyczyść wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="6b55d-199">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="6b55d-200">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b55d-200">Rebuild and run the app.</span></span> <span data-ttu-id="6b55d-201">Niektórych zadań do wykonania należy dodać do listy rzeczy do zrobienia, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="6b55d-201">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="6b55d-202">Tekst tytułu dla każdego elementu todo zyski, można edytować i pola wyboru mogą pomóc użytkownikowi informacje o ukończonych elementów.</span><span class="sxs-lookup"><span data-stu-id="6b55d-202">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="6b55d-203">Dodaj dane wejściowe pola wyboru dla każdego elementu todo i powiązać jej wartość na `IsDone` właściwości.</span><span class="sxs-lookup"><span data-stu-id="6b55d-203">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="6b55d-204">Zmiana `@todo.Title` do `<input>` powiązany element `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="6b55d-204">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="6b55d-205">Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `<h1>` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie są kompletne (`IsDone` jest `false`).</span><span class="sxs-lookup"><span data-stu-id="6b55d-205">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="6b55d-206">Ukończone składnika Todo (*Components/Pages/Todo.razor* lub *Pages/Todo.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="6b55d-206">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="6b55d-207">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b55d-207">Rebuild and run the app.</span></span> <span data-ttu-id="6b55d-208">Dodaj do testowania nowego kodu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6b55d-208">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="6b55d-209">Publikowanie i wdrażanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6b55d-209">Publish and deploy the app</span></span>

<span data-ttu-id="6b55d-210">Aby opublikować aplikację, zobacz <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="6b55d-210">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
