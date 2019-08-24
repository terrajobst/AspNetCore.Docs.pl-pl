---
title: Tworzenie pierwszej aplikacji Blazor
author: guardrex
description: Tworzenie aplikacji Blazor krok po kroku.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 2f0410eb75a6ef3f3d361c75bce7e79ae6d42a61
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017500"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="baea7-103">Tworzenie pierwszej aplikacji Blazor</span><span class="sxs-lookup"><span data-stu-id="baea7-103">Build your first Blazor app</span></span>

<span data-ttu-id="baea7-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="baea7-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="baea7-105">W tym samouczku pokazano, jak skompilować i zmodyfikować aplikację Blazor.</span><span class="sxs-lookup"><span data-stu-id="baea7-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="baea7-106">Postępuj zgodnie ze wskazówkami zawartymi w <xref:blazor/get-started> artykule, aby utworzyć projekt Blazor dla tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="baea7-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="baea7-107">Nazwij projekt *todolist*.</span><span class="sxs-lookup"><span data-stu-id="baea7-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="baea7-108">Składniki kompilacji</span><span class="sxs-lookup"><span data-stu-id="baea7-108">Build components</span></span>

1. <span data-ttu-id="baea7-109">Przejdź do każdej z trzech stron aplikacji w folderze *strony* : Dane na stronie głównej, licznika i pobierania.</span><span class="sxs-lookup"><span data-stu-id="baea7-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="baea7-110">Te strony są implementowane przez pliki składników Razor *index. Razor*, *Counter. Razor*i *FetchData. Razor*.</span><span class="sxs-lookup"><span data-stu-id="baea7-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="baea7-111">Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="baea7-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="baea7-112">Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale Blazor zapewnia lepsze podejście przy użyciu C#.</span><span class="sxs-lookup"><span data-stu-id="baea7-112">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="baea7-113">Sprawdzanie implementacji `Counter` składnika w pliku *Counter. Razor* .</span><span class="sxs-lookup"><span data-stu-id="baea7-113">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="baea7-114">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="baea7-114">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="baea7-115">Interfejs użytkownika `Counter` składnika jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="baea7-115">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="baea7-116">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="baea7-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="baea7-117">Logika znaczników HTML C# i renderowania jest konwertowana na klasę składników w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="baea7-117">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="baea7-118">Nazwa wygenerowanej klasy .NET jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="baea7-118">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="baea7-119">Elementy członkowskie klasy składnika są zdefiniowane w `@code` bloku.</span><span class="sxs-lookup"><span data-stu-id="baea7-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="baea7-120">`@code` W bloku, stan składnika (właściwości, pola) i metody są określone dla obsługi zdarzeń lub do definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="baea7-120">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="baea7-121">Te elementy członkowskie są następnie używane jako część logiki renderowania składnika i dla zdarzeń obsługi.</span><span class="sxs-lookup"><span data-stu-id="baea7-121">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="baea7-122">Po wybraniu przycisku **kliknij do mnie** :</span><span class="sxs-lookup"><span data-stu-id="baea7-122">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="baea7-123">Zarejestrowana`onclick` procedura`IncrementCount` obsługi jest wywoływana (Metoda). `Counter`</span><span class="sxs-lookup"><span data-stu-id="baea7-123">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="baea7-124">`Counter` Składnik ponownie generuje drzewo renderowania.</span><span class="sxs-lookup"><span data-stu-id="baea7-124">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="baea7-125">Nowe drzewo renderowania jest porównywane z poprzednią.</span><span class="sxs-lookup"><span data-stu-id="baea7-125">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="baea7-126">Stosowane są tylko modyfikacje Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="baea7-126">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="baea7-127">Wyświetlana liczba jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="baea7-127">The displayed count is updated.</span></span>

1. <span data-ttu-id="baea7-128">Zmodyfikuj C# logikę `Counter` składnika, aby zwiększyć liczbę o dwa zamiast jednego.</span><span class="sxs-lookup"><span data-stu-id="baea7-128">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="baea7-129">Skompiluj ponownie i uruchom aplikację, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="baea7-129">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="baea7-130">Wybierz przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="baea7-130">Select the **Click me** button.</span></span> <span data-ttu-id="baea7-131">Licznik jest zwiększany o dwa.</span><span class="sxs-lookup"><span data-stu-id="baea7-131">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="baea7-132">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="baea7-132">Use components</span></span>

<span data-ttu-id="baea7-133">Uwzględnij składnik w innym składniku przy użyciu składni języka HTML.</span><span class="sxs-lookup"><span data-stu-id="baea7-133">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="baea7-134">`Index` `<Counter />` Dodaj składnik do składnika aplikacji ,`Index` dodając element do składnika (*index. Razor*). `Counter`</span><span class="sxs-lookup"><span data-stu-id="baea7-134">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="baea7-135">Jeśli używasz programu Blazor Client dla tego środowiska, `SurveyPrompt` składnik jest używany `Index` przez składnik.</span><span class="sxs-lookup"><span data-stu-id="baea7-135">If you're using Blazor client-side for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="baea7-136">Zastąp `<Counter />`elementelementem. `<SurveyPrompt>`</span><span class="sxs-lookup"><span data-stu-id="baea7-136">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="baea7-137">Jeśli używasz aplikacji po stronie serwera Blazor dla tego środowiska, Dodaj `<Counter />` element `Index` do składnika:</span><span class="sxs-lookup"><span data-stu-id="baea7-137">If you're using a Blazor server-side app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="baea7-138">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="baea7-138">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="baea7-139">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="baea7-139">Rebuild and run the app.</span></span> <span data-ttu-id="baea7-140">`Index` Składnik ma swój własny licznik.</span><span class="sxs-lookup"><span data-stu-id="baea7-140">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="baea7-141">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="baea7-141">Component parameters</span></span>

<span data-ttu-id="baea7-142">Składniki mogą także mieć parametry.</span><span class="sxs-lookup"><span data-stu-id="baea7-142">Components can also have parameters.</span></span> <span data-ttu-id="baea7-143">Parametry składnika są definiowane przy użyciu właściwości publicznych w klasie składnika z `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="baea7-143">Component parameters are defined using public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="baea7-144">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="baea7-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="baea7-145">Zaktualizuj `@code` C# kod składnika:</span><span class="sxs-lookup"><span data-stu-id="baea7-145">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="baea7-146">Dodaj właściwość dekoracyjną `[Parameter]` z atrybutem. `IncrementAmount`</span><span class="sxs-lookup"><span data-stu-id="baea7-146">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="baea7-147">Zmień metodę, aby `IncrementAmount` użyć `currentCount`podczas zwiększania wartości. `IncrementCount`</span><span class="sxs-lookup"><span data-stu-id="baea7-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="baea7-148">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="baea7-148">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="baea7-149">Określ parametr w elemencie`<Counter>`składnikaprzyużyciuatrybutu. `Index` `IncrementAmount`</span><span class="sxs-lookup"><span data-stu-id="baea7-149">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="baea7-150">Ustaw wartość, aby zwiększyć licznik o dziesięć.</span><span class="sxs-lookup"><span data-stu-id="baea7-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="baea7-151">*Pages/index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="baea7-151">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="baea7-152">Załaduj `Index` ponownie składnik.</span><span class="sxs-lookup"><span data-stu-id="baea7-152">Reload the `Index` component.</span></span> <span data-ttu-id="baea7-153">Licznik jest zwiększany o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** .</span><span class="sxs-lookup"><span data-stu-id="baea7-153">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="baea7-154">Licznik w `Counter` składniku kontynuuje zwiększanie o jeden.</span><span class="sxs-lookup"><span data-stu-id="baea7-154">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="baea7-155">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="baea7-155">Route to components</span></span>

<span data-ttu-id="baea7-156">Dyrektywa w górnej części `Counter` pliku *Counter. Razor* określa, że składnik jest punktem końcowym routingu. `@page`</span><span class="sxs-lookup"><span data-stu-id="baea7-156">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="baea7-157">Składnik obsługuje żądania wysyłane do `/counter`. `Counter`</span><span class="sxs-lookup"><span data-stu-id="baea7-157">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="baea7-158">`@page` Bez dyrektywy składnik nie obsługuje rozesłanych żądań, ale składnik nadal może być używany przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="baea7-158">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="baea7-159">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="baea7-159">Dependency injection</span></span>

<span data-ttu-id="baea7-160">Usługi zarejestrowane w kontenerze usługi aplikacji są dostępne dla składników za pośrednictwem [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="baea7-160">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="baea7-161">Wsuń usługi do składnika za pomocą `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="baea7-161">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="baea7-162">Badanie dyrektyw `FetchData` składnika.</span><span class="sxs-lookup"><span data-stu-id="baea7-162">Examine the directives of the `FetchData` component.</span></span>

<span data-ttu-id="baea7-163">W przypadku korzystania z aplikacji `WeatherForecastService` po stronie serwera Blazor usługa jest rejestrowana jako [Pojedyncza](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="baea7-163">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="baea7-164">Dyrektywa służy do wstrzykiwania wystąpienia `WeatherForecastService` usługi do składnika. `@inject`</span><span class="sxs-lookup"><span data-stu-id="baea7-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="baea7-165">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="baea7-165">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="baea7-166">Składnik używa wstrzykniętej usługi jako `ForecastService`, `WeatherForecast` aby pobrać tablicę obiektów: `FetchData`</span><span class="sxs-lookup"><span data-stu-id="baea7-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="baea7-167">W przypadku pracy z aplikacją Blazor po stronie klienta program `HttpClient` wprowadza dane prognozy pogody z pliku *Pogoda. JSON* w folderze *wwwroot/Sample-Data* :</span><span class="sxs-lookup"><span data-stu-id="baea7-167">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="baea7-168">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="baea7-168">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="baea7-169">Pętla foreach jest używana do renderowania każdego wystąpienia prognozy jako wiersza w tabeli danych o pogodzie: [ \@](/dotnet/csharp/language-reference/keywords/foreach-in)</span><span class="sxs-lookup"><span data-stu-id="baea7-169">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="baea7-170">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="baea7-170">Build a todo list</span></span>

<span data-ttu-id="baea7-171">Dodaj do aplikacji nowy składnik implementujący prostą listę zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="baea7-171">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="baea7-172">Dodaj pusty plik o nazwie do *zrobienia. Razor* do aplikacji w folderze *Pages* :</span><span class="sxs-lookup"><span data-stu-id="baea7-172">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="baea7-173">Podaj początkowe znaczniki dla składnika:</span><span class="sxs-lookup"><span data-stu-id="baea7-173">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="baea7-174">`Todo` Dodaj składnik do paska nawigacyjnego.</span><span class="sxs-lookup"><span data-stu-id="baea7-174">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="baea7-175">Składnik (*Shared/NavMenu. Razor*) jest używany w układzie aplikacji. `NavMenu`</span><span class="sxs-lookup"><span data-stu-id="baea7-175">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="baea7-176">Układy są składnikami, które umożliwiają uniknięcie duplikowania zawartości w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="baea7-176">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="baea7-177">Dodaj element dla składnika, dodając następujący znacznik elementu listy poniżej istniejących elementów listy w pliku *Shared/NavMenu. Razor:* `Todo` `<NavLink>`</span><span class="sxs-lookup"><span data-stu-id="baea7-177">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="baea7-178">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="baea7-178">Rebuild and run the app.</span></span> <span data-ttu-id="baea7-179">Odwiedź stronę nowych zadań do wykonania, aby upewnić się, `Todo` że łącze do składnika działa.</span><span class="sxs-lookup"><span data-stu-id="baea7-179">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="baea7-180">Dodaj plik *TodoItem.cs* do katalogu głównego projektu, aby pomieścić klasę reprezentującą element do wykonania.</span><span class="sxs-lookup"><span data-stu-id="baea7-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="baea7-181">Użyj następującego C# kodu dla `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="baea7-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="baea7-182">Wróć do `Todo` składnika (*strony/do zrobienia. Razor*):</span><span class="sxs-lookup"><span data-stu-id="baea7-182">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="baea7-183">Dodaj pole do zadań do wykonania w `@code` bloku.</span><span class="sxs-lookup"><span data-stu-id="baea7-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="baea7-184">`Todo` Składnik używa tego pola do obsługi stanu listy zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="baea7-184">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="baea7-185">Dodaj znaczniki listy nieuporządkowane i `foreach` pętlę, aby renderować każdy element do wykonania jako element listy (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="baea7-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="baea7-186">Aplikacja wymaga elementów interfejsu użytkownika do dodawania do listy elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="baea7-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="baea7-187">Dodaj tekst wejściowy (`<input>`) i przycisk (`<button>`) poniżej listy nieuporządkowanej (`<ul>...</ul>`):</span><span class="sxs-lookup"><span data-stu-id="baea7-187">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="baea7-188">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="baea7-188">Rebuild and run the app.</span></span> <span data-ttu-id="baea7-189">Po wybraniu przycisku **Dodaj do zrobienia** nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest w trybie przewodowym.</span><span class="sxs-lookup"><span data-stu-id="baea7-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="baea7-190">Dodaj metodę do składnika i zarejestruj ją `@onclick` w celu wybrania opcji przy użyciu atrybutu. `Todo` `AddTodo`</span><span class="sxs-lookup"><span data-stu-id="baea7-190">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="baea7-191">C# wywoływana, gdy przycisk jest zaznaczony `AddTodo` :</span><span class="sxs-lookup"><span data-stu-id="baea7-191">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="baea7-192">Aby uzyskać tytuł nowego elementu do wykonania, należy `newTodo` dodać pole ciągu u góry `@code` bloku i powiązać je z wartością tekstu wejściowego `<input>` przy użyciu `bind` atrybutu w elemencie:</span><span class="sxs-lookup"><span data-stu-id="baea7-192">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="baea7-193">Zaktualizuj metodę, aby dodać z określonym tytułem do listy. `TodoItem` `AddTodo`</span><span class="sxs-lookup"><span data-stu-id="baea7-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="baea7-194">Wyczyść wartość pola wprowadzanie tekstu przez ustawienie `newTodo` do pustego ciągu:</span><span class="sxs-lookup"><span data-stu-id="baea7-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="baea7-195">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="baea7-195">Rebuild and run the app.</span></span> <span data-ttu-id="baea7-196">Dodaj do listy zadań do zrobienia elementy do wykonania, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="baea7-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="baea7-197">Tekst tytułu dla każdego elementu do wykonania można edytować, a pole wyboru może pomóc użytkownikowi śledzić elementy ukończone.</span><span class="sxs-lookup"><span data-stu-id="baea7-197">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="baea7-198">Dodaj pole wyboru dla każdego elementu do wykonania i powiąż jego wartość z `IsDone` właściwością.</span><span class="sxs-lookup"><span data-stu-id="baea7-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="baea7-199">Zmień `@todo.Title` `@todo.Title`na elementpowiązanyz:`<input>`</span><span class="sxs-lookup"><span data-stu-id="baea7-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="baea7-200">Aby sprawdzić, czy te wartości są powiązane, zaktualizuj `<h1>` nagłówek, aby pokazać liczbę elementów do wykonania, które nie zostały zakończone (`IsDone` is `false`).</span><span class="sxs-lookup"><span data-stu-id="baea7-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="baea7-201">Ukończony `Todo` składnik (*strony/do zrobienia. Razor*):</span><span class="sxs-lookup"><span data-stu-id="baea7-201">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="baea7-202">Skompiluj ponownie i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="baea7-202">Rebuild and run the app.</span></span> <span data-ttu-id="baea7-203">Dodaj elementy do wykonania, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="baea7-203">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
