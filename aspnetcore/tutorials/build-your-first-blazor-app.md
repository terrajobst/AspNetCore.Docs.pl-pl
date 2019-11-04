---
title: Tworzenie pierwszej aplikacji Blazor
author: guardrex
description: Tworzenie aplikacji Blazor krok po kroku.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: cc7caa1ee01e0282024895ab35c5b9933b1504d0
ms.sourcegitcommit: eb2fe5ad2e82fab86ca952463af8d017ba659b25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2019
ms.locfileid: "73416177"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="1e63d-103">Tworzenie pierwszej aplikacji Blazor</span><span class="sxs-lookup"><span data-stu-id="1e63d-103">Build your first Blazor app</span></span>

<span data-ttu-id="1e63d-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1e63d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="1e63d-105">W tym samouczku pokazano, jak skompilować i zmodyfikować aplikację Blazor.</span><span class="sxs-lookup"><span data-stu-id="1e63d-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="1e63d-106">Postępuj zgodnie ze wskazówkami w artykule <xref:blazor/get-started>, aby utworzyć projekt Blazor dla tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="1e63d-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="1e63d-107">Nazwij projekt *todolist*.</span><span class="sxs-lookup"><span data-stu-id="1e63d-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="1e63d-108">Składniki kompilacji</span><span class="sxs-lookup"><span data-stu-id="1e63d-108">Build components</span></span>

1. <span data-ttu-id="1e63d-109">Przejdź do każdej z trzech stron aplikacji w folderze *Pages* : Home, Counter i Fetch Data.</span><span class="sxs-lookup"><span data-stu-id="1e63d-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="1e63d-110">Te strony są implementowane przez pliki składników Razor *index. Razor*, *Counter. Razor*i *FetchData. Razor*.</span><span class="sxs-lookup"><span data-stu-id="1e63d-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="1e63d-111">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="1e63d-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="1e63d-112">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1e63d-112">Incrementing a counter in a webpage normally requires writing JavaScript.</span></span> <span data-ttu-id="1e63d-113">W przypadku Blazor można pisać C# zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="1e63d-113">With Blazor, you can write C# instead.</span></span>

1. <span data-ttu-id="1e63d-114">Sprawdzanie implementacji składnika `Counter` w pliku *Counter. Razor* .</span><span class="sxs-lookup"><span data-stu-id="1e63d-114">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="1e63d-115">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e63d-115">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="1e63d-116">Interfejs użytkownika składnika `Counter` jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="1e63d-116">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="1e63d-117">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="1e63d-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="1e63d-118">Logika znaczników HTML C# i renderowania jest konwertowana na klasę składników w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1e63d-118">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="1e63d-119">Nazwa wygenerowanej klasy .NET jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="1e63d-119">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="1e63d-120">Elementy członkowskie klasy składnika są zdefiniowane w bloku `@code`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="1e63d-121">W bloku `@code` stan składnika (właściwości, pola) i metody są określone dla obsługi zdarzeń lub do definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="1e63d-121">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="1e63d-122">Te elementy członkowskie są następnie używane jako część logiki renderowania składnika i dla zdarzeń obsługi.</span><span class="sxs-lookup"><span data-stu-id="1e63d-122">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="1e63d-123">Po wybraniu przycisku **kliknij do mnie** :</span><span class="sxs-lookup"><span data-stu-id="1e63d-123">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="1e63d-124">Zarejestrowano procedurę obsługi `onclick` składnika `Counter` (Metoda `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="1e63d-124">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="1e63d-125">Składnik `Counter` generuje ponownie drzewo renderowania.</span><span class="sxs-lookup"><span data-stu-id="1e63d-125">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="1e63d-126">Nowe drzewo renderowania jest porównywane z poprzednią.</span><span class="sxs-lookup"><span data-stu-id="1e63d-126">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="1e63d-127">Stosowane są tylko modyfikacje Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="1e63d-127">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="1e63d-128">Wyświetlana liczba jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="1e63d-128">The displayed count is updated.</span></span>

1. <span data-ttu-id="1e63d-129">Zmodyfikuj C# logikę składnika `Counter`, aby zwiększyć liczbę o dwa zamiast jednego.</span><span class="sxs-lookup"><span data-stu-id="1e63d-129">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="1e63d-130">Skompiluj ponownie i uruchom aplikację, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="1e63d-130">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="1e63d-131">Wybierz przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="1e63d-131">Select the **Click me** button.</span></span> <span data-ttu-id="1e63d-132">Licznik jest zwiększany o dwa.</span><span class="sxs-lookup"><span data-stu-id="1e63d-132">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="1e63d-133">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="1e63d-133">Use components</span></span>

<span data-ttu-id="1e63d-134">Uwzględnij składnik w innym składniku przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="1e63d-134">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="1e63d-135">Dodaj składnik `Counter` do składnika `Index` aplikacji, dodając element `<Counter />` do składnika `Index` (*index. Razor*).</span><span class="sxs-lookup"><span data-stu-id="1e63d-135">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="1e63d-136">Jeśli używasz zestawu webBlazor dla tego środowiska, składnik `SurveyPrompt` jest używany przez składnik `Index`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-136">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="1e63d-137">Zastąp element `<SurveyPrompt>` elementem `<Counter />`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-137">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="1e63d-138">Jeśli używasz aplikacji serwera Blazor dla tego środowiska, Dodaj element `<Counter />` do składnika `Index`:</span><span class="sxs-lookup"><span data-stu-id="1e63d-138">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="1e63d-139">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e63d-139">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="1e63d-140">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1e63d-140">Rebuild and run the app.</span></span> <span data-ttu-id="1e63d-141">Składnik `Index` ma swój własny licznik.</span><span class="sxs-lookup"><span data-stu-id="1e63d-141">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="1e63d-142">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="1e63d-142">Component parameters</span></span>

<span data-ttu-id="1e63d-143">Składniki mogą także mieć parametry.</span><span class="sxs-lookup"><span data-stu-id="1e63d-143">Components can also have parameters.</span></span> <span data-ttu-id="1e63d-144">Parametry składnika są definiowane przy użyciu właściwości publicznych w klasie składnika z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-144">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="1e63d-145">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="1e63d-145">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="1e63d-146">Zaktualizuj kod `@code` C# składnika:</span><span class="sxs-lookup"><span data-stu-id="1e63d-146">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="1e63d-147">Dodaj publiczną właściwość `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-147">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="1e63d-148">Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-148">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="1e63d-149">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e63d-149">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="1e63d-150">Określ parametr `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1e63d-150">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="1e63d-151">Ustaw wartość, aby zwiększyć licznik o dziesięć.</span><span class="sxs-lookup"><span data-stu-id="1e63d-151">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="1e63d-152">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e63d-152">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="1e63d-153">Załaduj ponownie składnik `Index`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-153">Reload the `Index` component.</span></span> <span data-ttu-id="1e63d-154">Licznik jest zwiększany o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="1e63d-154">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="1e63d-155">Licznik w składniku `Counter` kontynuuje zwiększanie o jeden.</span><span class="sxs-lookup"><span data-stu-id="1e63d-155">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="1e63d-156">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="1e63d-156">Route to components</span></span>

<span data-ttu-id="1e63d-157">Dyrektywa `@page` w górnej części pliku *Counter. Razor* określa, że składnik `Counter` jest punktem końcowym routingu.</span><span class="sxs-lookup"><span data-stu-id="1e63d-157">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="1e63d-158">Składnik `Counter` obsługuje żądania wysyłane do `/counter`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-158">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="1e63d-159">Bez dyrektywy `@page` składnik nie obsługuje rozesłanych żądań, ale składnik nadal może być używany przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="1e63d-159">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="1e63d-160">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="1e63d-160">Dependency injection</span></span>

### <a name="blazor-server-experience"></a><span data-ttu-id="1e63d-161">Środowisko serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="1e63d-161">Blazor Server experience</span></span>

<span data-ttu-id="1e63d-162">W przypadku korzystania z aplikacji serwera Blazor usługa `WeatherForecastService` jest zarejestrowana jako [Pojedyncza](xref:fundamentals/dependency-injection#service-lifetimes) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-162">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1e63d-163">Wystąpienie usługi jest dostępne w całej aplikacji za pośrednictwem [iniekcji zależności (di)](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="1e63d-163">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="1e63d-164">Dyrektywa `@inject` służy do wstrzykiwania wystąpienia usługi `WeatherForecastService` do składnika `FetchData`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="1e63d-165">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e63d-165">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="1e63d-166">Składnik `FetchData` używa wstrzykniętej usługi jako `ForecastService`, aby pobrać tablicę obiektów `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="1e63d-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a><span data-ttu-id="1e63d-167">Środowisko webassembly Blazor</span><span class="sxs-lookup"><span data-stu-id="1e63d-167">Blazor WebAssembly experience</span></span>

<span data-ttu-id="1e63d-168">W przypadku korzystania z aplikacji Blazor webassembly `HttpClient` jest wstrzykiwana w celu uzyskania danych prognozy pogody z pliku *Pogoda. JSON* w folderze *wwwroot/Sample-Data* .</span><span class="sxs-lookup"><span data-stu-id="1e63d-168">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="1e63d-169">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e63d-169">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="1e63d-170">Pętla [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) służy do renderowania każdego wystąpienia prognozy jako wiersza w tabeli danych o pogodzie:</span><span class="sxs-lookup"><span data-stu-id="1e63d-170">An [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="1e63d-171">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="1e63d-171">Build a todo list</span></span>

<span data-ttu-id="1e63d-172">Dodaj do aplikacji nowy składnik implementujący prostą listę zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1e63d-172">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="1e63d-173">Dodaj pusty plik o nazwie do *zrobienia. Razor* do aplikacji w folderze *Pages* :</span><span class="sxs-lookup"><span data-stu-id="1e63d-173">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="1e63d-174">Podaj początkowe znaczniki dla składnika:</span><span class="sxs-lookup"><span data-stu-id="1e63d-174">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="1e63d-175">Dodaj składnik `Todo` na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="1e63d-175">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="1e63d-176">Składnik `NavMenu` (*Shared/NavMenu. Razor*) jest używany w układzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1e63d-176">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="1e63d-177">Układy są składnikami, które umożliwiają uniknięcie duplikowania zawartości w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1e63d-177">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="1e63d-178">Dodaj element `<NavLink>` dla składnika `Todo`, dodając następujący znacznik elementu listy poniżej istniejących elementów listy w pliku *Shared/NavMenu. Razor* :</span><span class="sxs-lookup"><span data-stu-id="1e63d-178">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="1e63d-179">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1e63d-179">Rebuild and run the app.</span></span> <span data-ttu-id="1e63d-180">Odwiedź stronę Nowa czynność, aby upewnić się, że łącze do składnika `Todo` działa.</span><span class="sxs-lookup"><span data-stu-id="1e63d-180">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="1e63d-181">Dodaj plik *TodoItem.cs* do katalogu głównego projektu, aby pomieścić klasę reprezentującą element do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1e63d-181">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="1e63d-182">Użyj następującego C# kodu dla klasy `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="1e63d-182">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="1e63d-183">Wróć do składnika `Todo` (*strony/do zrobienia. Razor*):</span><span class="sxs-lookup"><span data-stu-id="1e63d-183">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="1e63d-184">Dodaj pole dla elementów do wykonania w bloku `@code`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-184">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="1e63d-185">Składnik `Todo` używa tego pola do obsługi stanu listy zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1e63d-185">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="1e63d-186">Dodaj znaczniki listy nieuporządkowane i pętlę `foreach`, aby renderować każdy element do wykonania jako element listy (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="1e63d-186">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="1e63d-187">Aplikacja wymaga elementów interfejsu użytkownika do dodawania do listy elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="1e63d-187">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="1e63d-188">Dodaj tekst wejściowy (`<input>`) i przycisk (`<button>`) poniżej listy nieuporządkowanej (`<ul>...</ul>`):</span><span class="sxs-lookup"><span data-stu-id="1e63d-188">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="1e63d-189">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1e63d-189">Rebuild and run the app.</span></span> <span data-ttu-id="1e63d-190">Po wybraniu przycisku **Dodaj do zrobienia** nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest w trybie przewodowym.</span><span class="sxs-lookup"><span data-stu-id="1e63d-190">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="1e63d-191">Dodaj metodę `AddTodo` do składnika `Todo` i zarejestruj ją dla opcji wyboru przy użyciu atrybutu `@onclick`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-191">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="1e63d-192">Metoda `AddTodo` C# jest wywoływana, gdy przycisk jest zaznaczony:</span><span class="sxs-lookup"><span data-stu-id="1e63d-192">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="1e63d-193">Aby uzyskać tytuł nowego elementu do wykonania, należy dodać pole ciągu `newTodo` w górnej części bloku `@code` i powiązać je z wartością wejściową tekstu przy użyciu atrybutu `bind` w elemencie `<input>`:</span><span class="sxs-lookup"><span data-stu-id="1e63d-193">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="1e63d-194">Zaktualizuj metodę `AddTodo`, aby dodać do listy `TodoItem` o określonym tytule.</span><span class="sxs-lookup"><span data-stu-id="1e63d-194">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="1e63d-195">Wyczyść wartość pola wprowadzanie tekstu przez ustawienie `newTodo` do pustego ciągu:</span><span class="sxs-lookup"><span data-stu-id="1e63d-195">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="1e63d-196">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1e63d-196">Rebuild and run the app.</span></span> <span data-ttu-id="1e63d-197">Dodaj do listy zadań do zrobienia elementy do wykonania, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="1e63d-197">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="1e63d-198">Tekst tytułu dla każdego elementu do wykonania można edytować, a pole wyboru może pomóc użytkownikowi śledzić elementy ukończone.</span><span class="sxs-lookup"><span data-stu-id="1e63d-198">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="1e63d-199">Dodaj pole wyboru dla każdego elementu do wykonania i powiąż jego wartość z właściwością `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="1e63d-199">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="1e63d-200">Zmień `@todo.Title` na element `<input>` powiązany z `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="1e63d-200">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="1e63d-201">Aby sprawdzić, czy te wartości są powiązane, zaktualizuj nagłówek `<h1>`, aby pokazać liczbę elementów do wykonania, które nie zostały zakończone (`IsDone` jest `false`).</span><span class="sxs-lookup"><span data-stu-id="1e63d-201">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="1e63d-202">Ukończony składnik `Todo` (*strony/zadanie. Razor*):</span><span class="sxs-lookup"><span data-stu-id="1e63d-202">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="1e63d-203">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1e63d-203">Rebuild and run the app.</span></span> <span data-ttu-id="1e63d-204">Dodaj elementy do wykonania, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="1e63d-204">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
