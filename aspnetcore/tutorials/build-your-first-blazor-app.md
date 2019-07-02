---
title: Utwórz swoją pierwszą aplikację Blazor
author: guardrex
description: Tworzenie aplikacji Blazor krok po kroku.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: d592c5bac1eb9822843a1ad1513a15fdfd6b1032
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500313"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="64b07-103">Utwórz swoją pierwszą aplikację Blazor</span><span class="sxs-lookup"><span data-stu-id="64b07-103">Build your first Blazor app</span></span>

<span data-ttu-id="64b07-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="64b07-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="64b07-105">W tym samouczku dowiesz się, jak tworzyć i modyfikować aplikację Blazor.</span><span class="sxs-lookup"><span data-stu-id="64b07-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="64b07-106">Postępuj zgodnie ze wskazówkami w <xref:blazor/get-started> artykuł, aby utworzyć projekt Blazor na potrzeby tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="64b07-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="64b07-107">Nadaj projektowi nazwę *ToDoList*.</span><span class="sxs-lookup"><span data-stu-id="64b07-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="64b07-108">Tworzenie składników</span><span class="sxs-lookup"><span data-stu-id="64b07-108">Build components</span></span>

1. <span data-ttu-id="64b07-109">Przejdź do każdego z trzech stron aplikacji w *stron* folderu: Strona główna licznik i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="64b07-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="64b07-110">Te strony są implementowane przez pliki składników Razor *Index.razor*, *Counter.razor*, i *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="64b07-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="64b07-111">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="64b07-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="64b07-112">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Blazor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="64b07-112">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="64b07-113">Sprawdź wykonania `Counter` składnika w *Counter.razor* pliku.</span><span class="sxs-lookup"><span data-stu-id="64b07-113">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="64b07-114">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="64b07-114">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="64b07-115">Interfejs użytkownika z `Counter` składnika jest definiowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="64b07-115">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="64b07-116">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="64b07-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="64b07-117">Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="64b07-117">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="64b07-118">Nazwa wygenerowanej klasy .NET zgodny z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="64b07-118">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="64b07-119">Elementy członkowskie klasy składników są zdefiniowane w `@code` bloku.</span><span class="sxs-lookup"><span data-stu-id="64b07-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="64b07-120">W `@code` blokować stan składnika (właściwości, pola) i metod są określone dla obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="64b07-120">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="64b07-121">Te elementy członkowskie są następnie używane w ramach składnika renderowania logiki oraz obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="64b07-121">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="64b07-122">Gdy **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="64b07-122">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="64b07-123">`Counter` Zarejestrowane składnika `onclick` program obsługi jest wywoływany ( `IncrementCount` metody).</span><span class="sxs-lookup"><span data-stu-id="64b07-123">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="64b07-124">`Counter` Składnika generuje drzewo jego renderowania.</span><span class="sxs-lookup"><span data-stu-id="64b07-124">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="64b07-125">Nowe drzewo renderowania w porównaniu do poprzedniego.</span><span class="sxs-lookup"><span data-stu-id="64b07-125">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="64b07-126">Modyfikacje do modelu DOM (Document Object) są stosowane.</span><span class="sxs-lookup"><span data-stu-id="64b07-126">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="64b07-127">Liczba wyświetlanych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="64b07-127">The displayed count is updated.</span></span>

1. <span data-ttu-id="64b07-128">Modyfikowanie C# logiki `Counter` składnika zwiększenie liczby dwa, a nie jeden.</span><span class="sxs-lookup"><span data-stu-id="64b07-128">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="64b07-129">Ponownie skompiluj i uruchom aplikację, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="64b07-129">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="64b07-130">Wybierz **kliknij mnie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="64b07-130">Select the **Click me** button.</span></span> <span data-ttu-id="64b07-131">Zwiększa licznik przez dwa.</span><span class="sxs-lookup"><span data-stu-id="64b07-131">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="64b07-132">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="64b07-132">Use components</span></span>

<span data-ttu-id="64b07-133">Obejmują składnika w innym składniku przy użyciu składni HTML.</span><span class="sxs-lookup"><span data-stu-id="64b07-133">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="64b07-134">Dodaj `Counter` składnika w aplikacji `Index` składnika, dodając `<Counter />` elementu `Index` składnika (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="64b07-134">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="64b07-135">Jeśli używasz Blazor po stronie klienta dla tego środowiska `SurveyPrompt` składnik jest używany przez `Index` składnika.</span><span class="sxs-lookup"><span data-stu-id="64b07-135">If you're using Blazor client-side for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="64b07-136">Zastąp `<SurveyPrompt>` element z `<Counter />` elementu.</span><span class="sxs-lookup"><span data-stu-id="64b07-136">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="64b07-137">Jeśli używasz aplikacji po stronie serwera Blazor dla tego środowiska, należy dodać `<Counter />` elementu `Index` składników:</span><span class="sxs-lookup"><span data-stu-id="64b07-137">If you're using a Blazor server-side app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="64b07-138">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="64b07-138">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="64b07-139">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="64b07-139">Rebuild and run the app.</span></span> <span data-ttu-id="64b07-140">`Index` Składnik ma swoje własne licznika.</span><span class="sxs-lookup"><span data-stu-id="64b07-140">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="64b07-141">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="64b07-141">Component parameters</span></span>

<span data-ttu-id="64b07-142">Składniki mogą także mieć parametrów.</span><span class="sxs-lookup"><span data-stu-id="64b07-142">Components can also have parameters.</span></span> <span data-ttu-id="64b07-143">Składnik parametry są definiowane za pomocą właściwości niepubliczne klasy składnika ozdobione `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="64b07-143">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="64b07-144">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="64b07-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="64b07-145">Aktualizowanie składnika `@code` C# kodu:</span><span class="sxs-lookup"><span data-stu-id="64b07-145">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="64b07-146">Dodaj `IncrementAmount` ozdobione właściwość `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="64b07-146">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="64b07-147">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="64b07-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="64b07-148">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="64b07-148">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="64b07-149">Określ `IncrementAmount` parametru w `Index` składnika `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="64b07-149">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="64b07-150">Ustaw wartość do zwiększenia licznika dziesięciu.</span><span class="sxs-lookup"><span data-stu-id="64b07-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="64b07-151">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="64b07-151">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="64b07-152">Załaduj ponownie `Index` składnika.</span><span class="sxs-lookup"><span data-stu-id="64b07-152">Reload the `Index` component.</span></span> <span data-ttu-id="64b07-153">Licznik rośnie przez dziesięć za każdym razem **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="64b07-153">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="64b07-154">Licznik `Counter` składnika w dalszym ciągu o jeden.</span><span class="sxs-lookup"><span data-stu-id="64b07-154">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="64b07-155">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="64b07-155">Route to components</span></span>

<span data-ttu-id="64b07-156">`@page` Dyrektywę w górnej części *Counter.razor* pliku Określa, że `Counter` składnik jest routingu punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="64b07-156">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="64b07-157">`Counter` Składnik obsługi żądań wysyłanych do `/counter`.</span><span class="sxs-lookup"><span data-stu-id="64b07-157">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="64b07-158">Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal można używać składnika przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="64b07-158">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="64b07-159">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="64b07-159">Dependency injection</span></span>

<span data-ttu-id="64b07-160">Zarejestrowane w kontenerze usługi app Services są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="64b07-160">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="64b07-161">Wstrzyknięcie usług do składnika za pomocą `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="64b07-161">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="64b07-162">Sprawdź dyrektyw dotyczących `FetchData` składnika.</span><span class="sxs-lookup"><span data-stu-id="64b07-162">Examine the directives of the `FetchData` component.</span></span>

<span data-ttu-id="64b07-163">Jeśli praca z aplikacją po stronie serwera Blazor `WeatherForecastService` usługi jest zarejestrowana jako [pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64b07-163">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="64b07-164">`@inject` Dyrektywy służy do dodania wystąpienia programu `WeatherForecastService` usługi do składnika.</span><span class="sxs-lookup"><span data-stu-id="64b07-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="64b07-165">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="64b07-165">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="64b07-166">`FetchData` Składnik używa usługi wprowadzonego jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:</span><span class="sxs-lookup"><span data-stu-id="64b07-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="64b07-167">Pracy z aplikacją po stronie klienta Blazor `HttpClient` są wstrzykiwane mają zostać pobrane dane prognozy pogody *weather.json* w pliku *wwwroot/przykładowe data* folderu:</span><span class="sxs-lookup"><span data-stu-id="64b07-167">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="64b07-168">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="64b07-168">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="64b07-169">A [ \@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli danych o pogodzie:</span><span class="sxs-lookup"><span data-stu-id="64b07-169">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="64b07-170">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="64b07-170">Build a todo list</span></span>

<span data-ttu-id="64b07-171">Dodawanie nowego składnika do aplikacji, która implementuje listy zadań do wykonania prostego.</span><span class="sxs-lookup"><span data-stu-id="64b07-171">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="64b07-172">Dodaj pusty plik o nazwie *Todo.razor* do aplikacji w *stron* folderu:</span><span class="sxs-lookup"><span data-stu-id="64b07-172">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="64b07-173">Podaj początkowe znaczniki dla składnika:</span><span class="sxs-lookup"><span data-stu-id="64b07-173">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="64b07-174">Dodaj `Todo` składnika na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="64b07-174">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="64b07-175">`NavMenu` Składnika (*Shared/NavMenu.razor*) jest używana w układzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64b07-175">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="64b07-176">Układy są składniki, które pozwalają uniknąć duplikowania zawartości w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64b07-176">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="64b07-177">Dodaj `<NavLink>` elementu `Todo` składnika, dodając następujące znaczniki elementu listy poniżej istniejącej listy elementów w *Shared/NavMenu.razor* pliku:</span><span class="sxs-lookup"><span data-stu-id="64b07-177">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="64b07-178">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="64b07-178">Rebuild and run the app.</span></span> <span data-ttu-id="64b07-179">Odwiedź nową stronę Todo, aby potwierdzić, że link do `Todo` działania składnika.</span><span class="sxs-lookup"><span data-stu-id="64b07-179">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="64b07-180">Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasa, która reprezentuje element todo.</span><span class="sxs-lookup"><span data-stu-id="64b07-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="64b07-181">Należy użyć następującego C# kod `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="64b07-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="64b07-182">Wróć do `Todo` składnika (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="64b07-182">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="64b07-183">Dodaj pole do wykonania w `@code` bloku.</span><span class="sxs-lookup"><span data-stu-id="64b07-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="64b07-184">`Todo` Składnik to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="64b07-184">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="64b07-185">Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="64b07-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="64b07-186">Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadania do wykonania do listy.</span><span class="sxs-lookup"><span data-stu-id="64b07-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="64b07-187">Dodaj tekstowe dane wejściowe (`<input>`) i przycisk (`<button>`) poniżej nieuporządkowaną listę (`<ul>...</ul>`):</span><span class="sxs-lookup"><span data-stu-id="64b07-187">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="64b07-188">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="64b07-188">Rebuild and run the app.</span></span> <span data-ttu-id="64b07-189">Gdy **dodawania zadań do wykonania** przycisk jest zaznaczony, nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest powiązaną przycisku.</span><span class="sxs-lookup"><span data-stu-id="64b07-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="64b07-190">Dodaj `AddTodo` metody `Todo` składnika i zarejestruj je dla przycisku wyborów przy użyciu `@onclick` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="64b07-190">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="64b07-191">`AddTodo` C# Metoda jest wywoływana po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="64b07-191">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="64b07-192">Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` pola ciągu w górnej części `@code` blokowania i powiązać wartość Tekst wejściowy przy użyciu `bind` atrybutu w `<input>` elementu:</span><span class="sxs-lookup"><span data-stu-id="64b07-192">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="@newTodo" />
   ```

1. <span data-ttu-id="64b07-193">Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy.</span><span class="sxs-lookup"><span data-stu-id="64b07-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="64b07-194">Wyczyść wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="64b07-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="64b07-195">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="64b07-195">Rebuild and run the app.</span></span> <span data-ttu-id="64b07-196">Niektóre zadania do wykonania należy dodać do listy rzeczy do zrobienia, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="64b07-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="64b07-197">Tekst tytułu dla każdego elementu todo zyski, można edytować, a pole wyboru może pomóc użytkownikowi informacje o ukończonych elementów.</span><span class="sxs-lookup"><span data-stu-id="64b07-197">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="64b07-198">Dodaj dane wejściowe pola wyboru dla każdego elementu todo i powiązać jej wartość na `IsDone` właściwości.</span><span class="sxs-lookup"><span data-stu-id="64b07-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="64b07-199">Zmiana `@todo.Title` do `<input>` powiązany element `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="64b07-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="64b07-200">Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `<h1>` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie są kompletne (`IsDone` jest `false`).</span><span class="sxs-lookup"><span data-stu-id="64b07-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="64b07-201">Gotowy `Todo` składnika (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="64b07-201">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="64b07-202">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="64b07-202">Rebuild and run the app.</span></span> <span data-ttu-id="64b07-203">Dodaj do testowania nowego kodu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="64b07-203">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
