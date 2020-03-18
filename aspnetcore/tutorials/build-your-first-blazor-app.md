---
title: Tworzenie pierwszej aplikacji Blazor
author: guardrex
description: Kompiluj aplikację Blazor krok po kroku.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2020
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 8b3802a6ffe3613e5d4ca65c57fafc3f404c8329
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434502"
---
# <a name="build-your-first-opno-locblazor-app"></a><span data-ttu-id="74886-103">Tworzenie pierwszej aplikacji Blazor</span><span class="sxs-lookup"><span data-stu-id="74886-103">Build your first Blazor app</span></span>

<span data-ttu-id="74886-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="74886-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="74886-105">W tym samouczku pokazano, jak skompilować i zmodyfikować aplikację Blazor.</span><span class="sxs-lookup"><span data-stu-id="74886-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

## <a name="build-components"></a><span data-ttu-id="74886-106">Składniki kompilacji</span><span class="sxs-lookup"><span data-stu-id="74886-106">Build components</span></span>

1. <span data-ttu-id="74886-107">Postępuj zgodnie ze wskazówkami w artykule <xref:blazor/get-started>, aby utworzyć projekt Blazor dla tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="74886-107">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="74886-108">Nazwij projekt *todolist*.</span><span class="sxs-lookup"><span data-stu-id="74886-108">Name the project *ToDoList*.</span></span>

1. <span data-ttu-id="74886-109">Przejdź do każdej z trzech stron aplikacji w folderze *Pages* : Home, Counter i Fetch Data.</span><span class="sxs-lookup"><span data-stu-id="74886-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="74886-110">Te strony są implementowane przez pliki składników Razor *index. Razor*, *Counter. Razor*i *FetchData. Razor*.</span><span class="sxs-lookup"><span data-stu-id="74886-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="74886-111">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="74886-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="74886-112">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="74886-112">Incrementing a counter in a webpage normally requires writing JavaScript.</span></span> <span data-ttu-id="74886-113">W przypadku Blazormożna pisać C# zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="74886-113">With Blazor, you can write C# instead.</span></span>

1. <span data-ttu-id="74886-114">Sprawdzanie implementacji składnika `Counter` w pliku *Counter. Razor* .</span><span class="sxs-lookup"><span data-stu-id="74886-114">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="74886-115">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="74886-115">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="74886-116">Interfejs użytkownika składnika `Counter` jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="74886-116">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="74886-117">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="74886-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="74886-118">Logika znaczników HTML C# i renderowania jest konwertowana na klasę składników w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="74886-118">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="74886-119">Nazwa wygenerowanej klasy .NET jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="74886-119">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="74886-120">Elementy członkowskie klasy składnika są zdefiniowane w bloku `@code`.</span><span class="sxs-lookup"><span data-stu-id="74886-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="74886-121">W bloku `@code`, stan składnika (właściwości, pola) i metody są określone dla obsługi zdarzeń lub do definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="74886-121">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="74886-122">Te elementy członkowskie są następnie używane jako część logiki renderowania składnika i dla zdarzeń obsługi.</span><span class="sxs-lookup"><span data-stu-id="74886-122">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="74886-123">Po wybraniu przycisku **kliknij do mnie** :</span><span class="sxs-lookup"><span data-stu-id="74886-123">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="74886-124">Zarejestrowana procedura obsługi `onclick` składnika `Counter` jest wywoływana (Metoda `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="74886-124">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="74886-125">Składnik `Counter` ponownie generuje drzewo renderowania.</span><span class="sxs-lookup"><span data-stu-id="74886-125">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="74886-126">Nowe drzewo renderowania jest porównywane z poprzednią.</span><span class="sxs-lookup"><span data-stu-id="74886-126">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="74886-127">Stosowane są tylko modyfikacje Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="74886-127">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="74886-128">Wyświetlana liczba jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="74886-128">The displayed count is updated.</span></span>

1. <span data-ttu-id="74886-129">Zmodyfikuj C# logikę składnika `Counter`, aby zwiększyć liczbę o dwa zamiast jednego.</span><span class="sxs-lookup"><span data-stu-id="74886-129">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="74886-130">Skompiluj ponownie i uruchom aplikację, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="74886-130">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="74886-131">Wybierz przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="74886-131">Select the **Click me** button.</span></span> <span data-ttu-id="74886-132">Licznik jest zwiększany o dwa.</span><span class="sxs-lookup"><span data-stu-id="74886-132">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="74886-133">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="74886-133">Use components</span></span>

<span data-ttu-id="74886-134">Uwzględnij składnik w innym składniku przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="74886-134">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="74886-135">Dodaj składnik `Counter` do składnika `Index` aplikacji, dodając element `<Counter />` do składnika `Index` (*index. Razor*).</span><span class="sxs-lookup"><span data-stu-id="74886-135">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="74886-136">Jeśli używasz Blazor webassembly dla tego środowiska, składnik `SurveyPrompt` jest używany przez składnik `Index`.</span><span class="sxs-lookup"><span data-stu-id="74886-136">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="74886-137">Zastąp element `<SurveyPrompt>` elementem `<Counter />`.</span><span class="sxs-lookup"><span data-stu-id="74886-137">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="74886-138">Jeśli używasz aplikacji serwera Blazor dla tego środowiska, Dodaj element `<Counter />` do składnika `Index`:</span><span class="sxs-lookup"><span data-stu-id="74886-138">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="74886-139">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="74886-139">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="74886-140">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="74886-140">Rebuild and run the app.</span></span> <span data-ttu-id="74886-141">Składnik `Index` ma swój własny licznik.</span><span class="sxs-lookup"><span data-stu-id="74886-141">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="74886-142">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="74886-142">Component parameters</span></span>

<span data-ttu-id="74886-143">Składniki mogą także mieć parametry.</span><span class="sxs-lookup"><span data-stu-id="74886-143">Components can also have parameters.</span></span> <span data-ttu-id="74886-144">Parametry składnika są definiowane przy użyciu właściwości publicznych w klasie składnika z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="74886-144">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="74886-145">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="74886-145">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="74886-146">Zaktualizuj kod `@code` C# składnika w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="74886-146">Update the component's `@code` C# code as follows:</span></span>

   * <span data-ttu-id="74886-147">Dodaj publiczną właściwość `IncrementAmount` z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="74886-147">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="74886-148">Zmień metodę `IncrementCount`, aby użyć właściwości `IncrementAmount` podczas zwiększania wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="74886-148">Change the `IncrementCount` method to use the `IncrementAmount` property when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="74886-149">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="74886-149">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. <span data-ttu-id="74886-150">Określ parametr `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="74886-150">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="74886-151">Ustaw wartość, aby zwiększyć licznik o dziesięć.</span><span class="sxs-lookup"><span data-stu-id="74886-151">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="74886-152">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="74886-152">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="74886-153">Załaduj ponownie składnik `Index`.</span><span class="sxs-lookup"><span data-stu-id="74886-153">Reload the `Index` component.</span></span> <span data-ttu-id="74886-154">Licznik jest zwiększany o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="74886-154">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="74886-155">Licznik w składniku `Counter` w dalszym ciągu zwiększa się o jeden.</span><span class="sxs-lookup"><span data-stu-id="74886-155">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="74886-156">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="74886-156">Route to components</span></span>

<span data-ttu-id="74886-157">Dyrektywa `@page` w górnej części pliku *Counter. Razor* określa, że składnik `Counter` jest punktem końcowym routingu.</span><span class="sxs-lookup"><span data-stu-id="74886-157">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="74886-158">Składnik `Counter` obsługuje żądania wysyłane do `/counter`.</span><span class="sxs-lookup"><span data-stu-id="74886-158">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="74886-159">Bez dyrektywy `@page` składnik nie obsługuje rozesłanych żądań, ale składnik nadal może być używany przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="74886-159">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="74886-160">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="74886-160">Dependency injection</span></span>

### <a name="opno-locblazor-server-experience"></a><span data-ttu-id="74886-161">środowisko serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="74886-161">Blazor Server experience</span></span>

<span data-ttu-id="74886-162">W przypadku korzystania z aplikacji Blazor Server usługa `WeatherForecastService` zostanie zarejestrowana jako [Pojedyncza](xref:fundamentals/dependency-injection#service-lifetimes) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="74886-162">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="74886-163">Wystąpienie usługi jest dostępne w całej aplikacji za pośrednictwem [iniekcji zależności (di)](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="74886-163">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="74886-164">Dyrektywa `@inject` służy do wstrzykiwania wystąpienia usługi `WeatherForecastService` do składnika `FetchData`.</span><span class="sxs-lookup"><span data-stu-id="74886-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="74886-165">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="74886-165">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="74886-166">Składnik `FetchData` używa wstrzykniętej usługi jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:</span><span class="sxs-lookup"><span data-stu-id="74886-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a>Blazor<span data-ttu-id="74886-167"> środowisko webassembly</span><span class="sxs-lookup"><span data-stu-id="74886-167"> WebAssembly experience</span></span>

<span data-ttu-id="74886-168">W przypadku pracy z aplikacją Blazor webassembly `HttpClient` jest wstrzykiwana w celu uzyskania danych prognozy pogody z pliku *Pogoda. JSON* w folderze *wwwroot/Sample-Data* .</span><span class="sxs-lookup"><span data-stu-id="74886-168">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="74886-169">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="74886-169">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="74886-170">Pętla [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) jest używana do renderowania każdego wystąpienia prognozy jako wiersza w tabeli danych o pogodzie:</span><span class="sxs-lookup"><span data-stu-id="74886-170">An [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="74886-171">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="74886-171">Build a todo list</span></span>

<span data-ttu-id="74886-172">Dodaj do aplikacji nowy składnik implementujący prostą listę zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="74886-172">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="74886-173">Dodaj nowy składnik `Todo` Razor do aplikacji w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="74886-173">Add a new `Todo` Razor component to the app in the *Pages* folder.</span></span> <span data-ttu-id="74886-174">W programie Visual Studio kliknij prawym przyciskiem myszy folder **strony** i wybierz polecenie **Dodaj** > **nowy element** > **składnik Razor**.</span><span class="sxs-lookup"><span data-stu-id="74886-174">In Visual Studio, right-click the **Pages** folder and select **Add** > **New Item** > **Razor Component**.</span></span> <span data-ttu-id="74886-175">Nadaj nazwę plikowi do *zrobienia. Razor*.</span><span class="sxs-lookup"><span data-stu-id="74886-175">Name the component's file *Todo.razor*.</span></span> <span data-ttu-id="74886-176">W innych środowiskach programistycznych Dodaj pusty plik do folderu **Pages** o nazwie do *zrobienia. Razor*.</span><span class="sxs-lookup"><span data-stu-id="74886-176">In other development environments, add a blank file to the **Pages** folder named *Todo.razor*.</span></span>

1. <span data-ttu-id="74886-177">Podaj początkowe znaczniki dla składnika:</span><span class="sxs-lookup"><span data-stu-id="74886-177">Provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. <span data-ttu-id="74886-178">Dodaj składnik `Todo` na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="74886-178">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="74886-179">Składnik `NavMenu` (*Shared/NavMenu. Razor*) jest używany w układzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="74886-179">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="74886-180">Układy są składnikami, które umożliwiają uniknięcie duplikowania zawartości w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="74886-180">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="74886-181">Dodaj element `<NavLink>` dla składnika `Todo`, dodając następujący znacznik elementu listy poniżej istniejących elementów listy w pliku *Shared/NavMenu. Razor* :</span><span class="sxs-lookup"><span data-stu-id="74886-181">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="74886-182">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="74886-182">Rebuild and run the app.</span></span> <span data-ttu-id="74886-183">Odwiedź stronę Nowa czynność, aby upewnić się, że łącze do składnika `Todo` działa.</span><span class="sxs-lookup"><span data-stu-id="74886-183">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="74886-184">Dodaj plik *TodoItem.cs* do katalogu głównego projektu, aby pomieścić klasę reprezentującą element do wykonania.</span><span class="sxs-lookup"><span data-stu-id="74886-184">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="74886-185">Użyj następującego C# kodu dla klasy `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="74886-185">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="74886-186">Wróć do składnika `Todo` (*strony/do zrobienia. Razor*):</span><span class="sxs-lookup"><span data-stu-id="74886-186">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="74886-187">Dodaj pole dla elementów do wykonania w bloku `@code`.</span><span class="sxs-lookup"><span data-stu-id="74886-187">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="74886-188">Składnik `Todo` używa tego pola do obsługi stanu listy zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="74886-188">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="74886-189">Dodaj znaczniki listy nieuporządkowane i pętlę `foreach`, aby renderować każdy element do wykonania jako element listy (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="74886-189">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="74886-190">Aplikacja wymaga elementów interfejsu użytkownika do dodawania do listy elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="74886-190">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="74886-191">Dodaj tekst wejściowy (`<input>`) i przycisk (`<button>`) poniżej listy nieuporządkowanej (`<ul>...</ul>`):</span><span class="sxs-lookup"><span data-stu-id="74886-191">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="74886-192">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="74886-192">Rebuild and run the app.</span></span> <span data-ttu-id="74886-193">Po wybraniu przycisku **Dodaj do zrobienia** nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest w trybie przewodowym.</span><span class="sxs-lookup"><span data-stu-id="74886-193">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="74886-194">Dodaj metodę `AddTodo` do składnika `Todo` i zarejestruj ją w celu wybrania opcji przy użyciu atrybutu `@onclick`.</span><span class="sxs-lookup"><span data-stu-id="74886-194">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="74886-195">Metoda `AddTodo` C# jest wywoływana, gdy przycisk jest zaznaczony:</span><span class="sxs-lookup"><span data-stu-id="74886-195">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="74886-196">Aby uzyskać tytuł nowego elementu do wykonania, należy dodać pole ciągu `newTodo` w górnej części bloku `@code` i powiązać je z wartością wejściową tekstu przy użyciu atrybutu `bind` w elemencie `<input>`:</span><span class="sxs-lookup"><span data-stu-id="74886-196">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="74886-197">Zaktualizuj metodę `AddTodo`, aby dodać do listy `TodoItem` z określonym tytułem.</span><span class="sxs-lookup"><span data-stu-id="74886-197">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="74886-198">Wyczyść wartość pola wejściowego tekstu przez ustawienie `newTodo` do pustego ciągu:</span><span class="sxs-lookup"><span data-stu-id="74886-198">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="74886-199">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="74886-199">Rebuild and run the app.</span></span> <span data-ttu-id="74886-200">Dodaj do listy zadań do zrobienia elementy do wykonania, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="74886-200">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="74886-201">Tekst tytułu dla każdego elementu do wykonania można edytować, a pole wyboru może pomóc użytkownikowi śledzić elementy ukończone.</span><span class="sxs-lookup"><span data-stu-id="74886-201">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="74886-202">Dodaj pole wyboru dla każdego elementu do wykonania i powiąż jego wartość z właściwością `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="74886-202">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="74886-203">Zmień `@todo.Title` na element `<input>` powiązany z `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="74886-203">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="74886-204">Aby sprawdzić, czy te wartości są powiązane, zaktualizuj nagłówek `<h1>`, aby pokazać liczbę elementów do wykonania, które nie zostały ukończone (`IsDone` jest `false`).</span><span class="sxs-lookup"><span data-stu-id="74886-204">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="74886-205">Ukończony składnik `Todo` (*strony/do zrobienia. Razor*):</span><span class="sxs-lookup"><span data-stu-id="74886-205">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="74886-206">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="74886-206">Rebuild and run the app.</span></span> <span data-ttu-id="74886-207">Dodaj elementy do wykonania, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="74886-207">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
