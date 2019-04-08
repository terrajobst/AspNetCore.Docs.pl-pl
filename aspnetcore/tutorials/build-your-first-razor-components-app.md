---
title: Tworzenie pierwszej aplikacji składniki Razor
author: guardrex
description: Tworzenie składników Razor aplikacji krok po kroku i pojęcia dotyczące podstawowych składników Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 40bf5728193c854589e072730554b4f54086fb6a
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068131"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="ce4ee-103">Tworzenie pierwszej aplikacji składniki Razor</span><span class="sxs-lookup"><span data-stu-id="ce4ee-103">Build your first Razor Components app</span></span>

<span data-ttu-id="ce4ee-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ce4ee-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="ce4ee-105">W tym samouczku pokazano, jak utworzyć aplikację ze składnikami Razor i pokazuje podstawowe pojęcia składniki Razor.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="ce4ee-106">Można korzystać z tego samouczka przy użyciu obu składników Razor na podstawie projektu (obsługiwane w programie .NET Core 3.0 lub nowszej) lub projektu na podstawie Blazor (obsługiwane w przyszłej wersji programu .NET Core).</span><span class="sxs-lookup"><span data-stu-id="ce4ee-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="ce4ee-107">Środowisko za pomocą platformy ASP.NET Core Razor składników (*zalecane*):</span><span class="sxs-lookup"><span data-stu-id="ce4ee-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="ce4ee-108">Postępuj zgodnie ze wskazówkami w <xref:razor-components/get-started> do tworzenia projektu na podstawie składników Razor.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="ce4ee-109">Nadaj projektowi nazwę `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="ce4ee-110">Aby uzyskać doświadczenie w korzystaniu z Blazor:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="ce4ee-111">Postępuj zgodnie ze wskazówkami w <xref:spa/blazor/get-started> do tworzenia projektu na podstawie Blazor.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="ce4ee-112">Nadaj projektowi nazwę `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="ce4ee-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ce4ee-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="ce4ee-114">Zobacz następujące tematy dotyczące wymagań wstępnych:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="ce4ee-115">Tworzenie składników</span><span class="sxs-lookup"><span data-stu-id="ce4ee-115">Build components</span></span>

1. <span data-ttu-id="ce4ee-116">Przejdź do każdego z trzech stron aplikacji w *składniki/strony* folder (*stron* w Blazor): Strona główna licznik i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="ce4ee-117">Te strony są implementowane przez pliki Razor składników: *Index.razor*, *Counter.razor*, i *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="ce4ee-118">(Blazor w dalszym ciągu używa *.cshtml* rozszerzenie pliku: *Index.cshtml*, *Counter.cshtml*, i *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="ce4ee-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="ce4ee-119">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="ce4ee-120">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="ce4ee-121">Zapoznania się z implementacją składnikiem licznika w *Counter.razor* pliku.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="ce4ee-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="ce4ee-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="ce4ee-123">Interfejs użytkownika składnika licznika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="ce4ee-124">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="ce4ee-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="ce4ee-125">Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="ce4ee-126">Nazwa wygenerowanej klasy .NET zgodny z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="ce4ee-127">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="ce4ee-128">W `@functions` blokować stan składnika (właściwości, pola) i metod są określone dla obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="ce4ee-129">Te elementy członkowskie są następnie używane w ramach składnika renderowania logiki oraz obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="ce4ee-130">Gdy **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="ce4ee-131">Zarejestrowane składnik licznika `onclick` program obsługi jest wywoływany ( `IncrementCount` metody).</span><span class="sxs-lookup"><span data-stu-id="ce4ee-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="ce4ee-132">Składnik licznika generuje drzewo jego renderowania.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="ce4ee-133">Nowe drzewo renderowania w porównaniu do poprzedniego.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="ce4ee-134">Modyfikacje do modelu DOM (Document Object) są stosowane.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="ce4ee-135">Liczba wyświetlanych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="ce4ee-136">Modyfikowanie C# logiki licznika składnika zwiększenie liczby dwa, a nie jeden.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="ce4ee-137">Ponownie skompiluj i uruchom aplikację, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="ce4ee-138">Wybierz **kliknij mnie** przycisk i zwiększa licznik przez dwa.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="ce4ee-139">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="ce4ee-139">Use components</span></span>

<span data-ttu-id="ce4ee-140">Uwzględnij składnika w innym składniku przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="ce4ee-141">Dodaj składnik licznika do składnik indeksu (dom) aplikacji przez dodanie `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-141">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="ce4ee-142">Jeśli używasz Blazor dla tego środowiska, składnik ankiety monitu (`<SurveyPrompt>` elementu) znajduje się w składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="ce4ee-143">Zastąp `<SurveyPrompt>` element z `<Counter>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="ce4ee-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="ce4ee-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="ce4ee-145">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-145">Rebuild and run the app.</span></span> <span data-ttu-id="ce4ee-146">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-146">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="ce4ee-147">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="ce4ee-147">Component parameters</span></span>

<span data-ttu-id="ce4ee-148">Składniki mogą także mieć parametrów.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-148">Components can also have parameters.</span></span> <span data-ttu-id="ce4ee-149">Składnik parametry są definiowane za pomocą właściwości niepubliczne klasy składnika ozdobione `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="ce4ee-150">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="ce4ee-151">Aktualizowanie składnika `@functions` C# kodu:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="ce4ee-152">Dodaj `IncrementAmount` ozdobione właściwość `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="ce4ee-153">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="ce4ee-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="ce4ee-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="ce4ee-155">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="ce4ee-156">Ustaw wartość do zwiększenia licznika dziesięciu.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="ce4ee-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="ce4ee-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="ce4ee-158">Załaduj ponownie stronę główną.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-158">Reload the Home page.</span></span> <span data-ttu-id="ce4ee-159">Licznik rośnie przez dziesięć za każdym razem **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-159">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="ce4ee-160">Licznik w przyrostach strony liczników za pomocą jednej.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-160">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="ce4ee-161">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="ce4ee-161">Route to components</span></span>

<span data-ttu-id="ce4ee-162">`@page` Dyrektywę w górnej części *Counter.razor* pliku Określa, że ten składnik jest routingu punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="ce4ee-163">Składnik licznika obsługuje żądania wysyłane do `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="ce4ee-164">Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal można używać składnika przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="ce4ee-165">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="ce4ee-165">Dependency injection</span></span>

<span data-ttu-id="ce4ee-166">Zarejestrowane w kontenerze usługi app Services są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ce4ee-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ce4ee-167">Wstrzyknięcie usług do składnika za pomocą `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="ce4ee-168">Sprawdź dyrektywy składnika FetchData w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-168">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="ce4ee-169">Przykładowa aplikacja składniki Razor `WeatherForecastService` usługi jest zarejestrowana jako [pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-169">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="ce4ee-170">`@inject` Dyrektywy służy do dodania wystąpienia programu `WeatherForecastService` usługi do składnika.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-170">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="ce4ee-171">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-171">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="ce4ee-172">Składnik FetchData używa usługi wprowadzonego jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="ce4ee-173">W przykładowej aplikacji w wersji Blazor `HttpClient` są wstrzykiwane można uzyskać prognozę pogody dane z *weather.json* w pliku *wwwroot/przykładowe data* folderu:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-173">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="ce4ee-174">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-174">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="ce4ee-175">W obu przykładowe aplikacje [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli danych o pogodzie:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-175">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="ce4ee-176">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="ce4ee-176">Build a todo list</span></span>

<span data-ttu-id="ce4ee-177">Dodawanie nowego składnika do aplikacji, która implementuje listy zadań do wykonania prostego.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-177">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="ce4ee-178">Dodaj pusty plik do przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-178">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="ce4ee-179">Dla składników Razor środowiska, Dodaj *Todo.razor* plik *składniki/strony* folderu.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-179">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="ce4ee-180">Środowisko pracy Blazor, Dodaj *Todo.cshtml* plik *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-180">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="ce4ee-181">Podaj początkowe znaczniki dla składnika:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-181">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="ce4ee-182">Dodaj składnik zadań do wykonania na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-182">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="ce4ee-183">Składnik NavMenu (*Components/Shared/NavMenu.razor* lub *Shared/NavMenu.cshtml* w Blazor) jest używana w układzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-183">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="ce4ee-184">Układy są składniki, które pozwalają uniknąć duplikowania zawartości w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-184">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="ce4ee-185">Aby uzyskać więcej informacji, zobacz <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-185">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="ce4ee-186">Dodaj `<NavLink>` składnika Todo, dodając następujące znaczniki elementu listy poniżej istniejące elementy listy w *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* w Blazor) pliku:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-186">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="ce4ee-187">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-187">Rebuild and run the app.</span></span> <span data-ttu-id="ce4ee-188">Odwiedź nową stronę Todo, aby upewnić się, czy działa link do składnika zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-188">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="ce4ee-189">Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasa, która reprezentuje element todo.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-189">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="ce4ee-190">Należy użyć następującego C# kod `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-190">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="ce4ee-191">Wróć do części Todo (*Components/Pages/Todo.razor* lub *Pages/Todo.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="ce4ee-191">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="ce4ee-192">Dodaj pole do zadań do wykonania w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-192">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="ce4ee-193">Składnik Todo to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-193">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="ce4ee-194">Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-194">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="ce4ee-195">Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadań do wykonania do listy.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-195">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="ce4ee-196">Dodaj przycisk poniżej listy i tekstowych danych wejściowych:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-196">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="ce4ee-197">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-197">Rebuild and run the app.</span></span> <span data-ttu-id="ce4ee-198">Gdy **dodawania zadań do wykonania** przycisk jest zaznaczony, nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest powiązaną przycisku.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-198">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="ce4ee-199">Dodaj `AddTodo` metodę, aby składnik zadań do wykonania i zarejestruj go dla przycisku kliknie, za pomocą `onclick` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-199">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="ce4ee-200">`AddTodo` C# Metoda jest wywoływana po wybraniu przycisku.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-200">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="ce4ee-201">Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` ciąg pola i powiązać wartość wprowadzania przy użyciu tekstu `bind` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-201">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="ce4ee-202">Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-202">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="ce4ee-203">Wyczyść wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-203">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="ce4ee-204">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-204">Rebuild and run the app.</span></span> <span data-ttu-id="ce4ee-205">Niektórych zadań do wykonania należy dodać do listy rzeczy do zrobienia, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-205">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="ce4ee-206">Tekst tytułu dla każdego elementu todo zyski, można edytować i pola wyboru mogą pomóc użytkownikowi informacje o ukończonych elementów.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-206">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="ce4ee-207">Dodaj dane wejściowe pola wyboru dla każdego elementu todo i powiązać jej wartość na `IsDone` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-207">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="ce4ee-208">Zmiana `@todo.Title` do `<input>` powiązany element `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="ce4ee-208">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="ce4ee-209">Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `<h1>` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie są kompletne (`IsDone` jest `false`).</span><span class="sxs-lookup"><span data-stu-id="ce4ee-209">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="ce4ee-210">Ukończone składnika Todo (*Components/Pages/Todo.razor* lub *Pages/Todo.cshtml* w Blazor):</span><span class="sxs-lookup"><span data-stu-id="ce4ee-210">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="ce4ee-211">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-211">Rebuild and run the app.</span></span> <span data-ttu-id="ce4ee-212">Dodaj do testowania nowego kodu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-212">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="ce4ee-213">Publikowanie i wdrażanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ce4ee-213">Publish and deploy the app</span></span>

<span data-ttu-id="ce4ee-214">Aby opublikować aplikację, zobacz <xref:host-and-deploy/razor-components-blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="ce4ee-214">To publish the app, see <xref:host-and-deploy/razor-components-blazor/index>.</span></span>
