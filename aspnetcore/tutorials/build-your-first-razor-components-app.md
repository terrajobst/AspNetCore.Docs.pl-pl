---
title: Tworzenie pierwszej aplikacji składniki Razor
author: guardrex
description: Tworzenie składników Razor aplikacji krok po kroku i pojęcia dotyczące podstawowych składników Razor framework.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 4bf3884d5d9575ebf2a09237e364b37fa1b35246
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854607"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="76873-103">Tworzenie pierwszej aplikacji składniki Razor</span><span class="sxs-lookup"><span data-stu-id="76873-103">Build your first Razor Components app</span></span>

<span data-ttu-id="76873-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="76873-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="76873-105">W tym samouczku dowiesz się, jak utworzyć aplikację składniki Razor i pokazuje podstawowe pojęcia framework składniki Razor.</span><span class="sxs-lookup"><span data-stu-id="76873-105">This tutorial shows you how to build a Razor Components app and demonstrates basic Razor Components framework concepts.</span></span>

<span data-ttu-id="76873-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="76873-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="76873-107">Zobacz [wprowadzenie](xref:razor-components/get-started) tematu wstępnie wymaganych składników.</span><span class="sxs-lookup"><span data-stu-id="76873-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="create-an-app-from-the-razor-components-template"></a><span data-ttu-id="76873-108">Tworzenie aplikacji na podstawie szablonu Razor składników</span><span class="sxs-lookup"><span data-stu-id="76873-108">Create an app from the Razor Components template</span></span>

<span data-ttu-id="76873-109">Postępuj zgodnie ze wskazówkami w [wprowadzenie](xref:razor-components/get-started) tematu, aby utworzyć projekt składniki Razor na podstawie szablonu Razor składników.</span><span class="sxs-lookup"><span data-stu-id="76873-109">Follow the guidance in the [Get started](xref:razor-components/get-started) topic to create a Razor Components project from the Razor Components template.</span></span> <span data-ttu-id="76873-110">Nazwij rozwiązanie *WebApplication1*.</span><span class="sxs-lookup"><span data-stu-id="76873-110">Name the solution *WebApplication1*.</span></span> <span data-ttu-id="76873-111">Za pomocą poleceń interfejsu wiersza polecenia platformy .NET Core, można użyć programu Visual Studio lub powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="76873-111">You can use Visual Studio or a command shell with .NET Core CLI commands.</span></span>

## <a name="build-components"></a><span data-ttu-id="76873-112">Tworzenie składników</span><span class="sxs-lookup"><span data-stu-id="76873-112">Build components</span></span>

1. <span data-ttu-id="76873-113">Przejdź do wszystkich aplikacji trzy strony: Strona główna licznik i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="76873-113">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="76873-114">Te strony są implementowane przez pliki Razor w *WebApplication1.App/Pages* folderu: *Index.cshtml*, *Counter.cshtml*, i *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="76873-114">These pages are implemented by Razor files in the *WebApplication1.App/Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span>

1. <span data-ttu-id="76873-115">Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="76873-115">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="76873-116">Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.</span><span class="sxs-lookup"><span data-stu-id="76873-116">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="76873-117">Zapoznania się z implementacją składnikiem licznika w *Counter.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="76873-117">Examine the implementation of the Counter component in the *Counter.cshtml* file.</span></span>

   <span data-ttu-id="76873-118">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="76873-118">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   <span data-ttu-id="76873-119">Interfejs użytkownika składnika licznika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="76873-119">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="76873-120">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="76873-120">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="76873-121">Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="76873-121">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="76873-122">Nazwa wygenerowanej klasy .NET zgodny z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="76873-122">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="76873-123">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="76873-123">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="76873-124">W `@functions` blokować stan składnika (właściwości, pola) i metod są określone dla obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="76873-124">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="76873-125">Te elementy członkowskie są następnie używane w ramach składnika renderowania logiki oraz obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="76873-125">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="76873-126">Gdy **kliknij mnie** wybrany przycisk:</span><span class="sxs-lookup"><span data-stu-id="76873-126">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="76873-127">Zarejestrowane składnik licznika `onclick` program obsługi jest wywoływany ( `IncrementCount` metody).</span><span class="sxs-lookup"><span data-stu-id="76873-127">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="76873-128">Składnik licznika generuje drzewo jego renderowania.</span><span class="sxs-lookup"><span data-stu-id="76873-128">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="76873-129">Nowe drzewo renderowania w porównaniu do poprzedniego.</span><span class="sxs-lookup"><span data-stu-id="76873-129">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="76873-130">Modyfikacje do modelu DOM (Document Object) są stosowane.</span><span class="sxs-lookup"><span data-stu-id="76873-130">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="76873-131">Liczba wyświetlanych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="76873-131">The displayed count is updated.</span></span>

1. <span data-ttu-id="76873-132">Modyfikowanie C# logiki licznika składnika zwiększenie liczby dwa, a nie jeden.</span><span class="sxs-lookup"><span data-stu-id="76873-132">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. <span data-ttu-id="76873-133">Ponownie skompiluj i uruchom aplikację, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="76873-133">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="76873-134">Wybierz **kliknij mnie** przycisk i zwiększa licznik przez dwa.</span><span class="sxs-lookup"><span data-stu-id="76873-134">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="76873-135">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="76873-135">Use components</span></span>

<span data-ttu-id="76873-136">Uwzględnij składnika w innym składniku przy użyciu składni notacji HTML.</span><span class="sxs-lookup"><span data-stu-id="76873-136">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="76873-137">Dodaj składnik licznika do aplikacji, składnik indeksu (strona główna), dodając `<Counter />` elementu składnik indeksu.</span><span class="sxs-lookup"><span data-stu-id="76873-137">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="76873-138">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="76873-138">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. <span data-ttu-id="76873-139">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="76873-139">Rebuild and run the app.</span></span> <span data-ttu-id="76873-140">Strona główna ma swój własny licznika.</span><span class="sxs-lookup"><span data-stu-id="76873-140">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="76873-141">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="76873-141">Component parameters</span></span>

<span data-ttu-id="76873-142">Składniki mogą także mieć parametrów.</span><span class="sxs-lookup"><span data-stu-id="76873-142">Components can also have parameters.</span></span> <span data-ttu-id="76873-143">Składnik parametry są definiowane za pomocą właściwości niepubliczne klasy składnika ozdobione `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="76873-143">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="76873-144">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="76873-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="76873-145">Aktualizowanie składnika `@functions` C# kodu:</span><span class="sxs-lookup"><span data-stu-id="76873-145">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="76873-146">Dodaj `IncrementAmount` ozdobione właściwość `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="76873-146">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="76873-147">Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="76873-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="76873-148">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="76873-148">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/WebApplication1/WebApplication1.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="76873-149">Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.</span><span class="sxs-lookup"><span data-stu-id="76873-149">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="76873-150">Ustaw wartość do zwiększenia licznika dziesięciu.</span><span class="sxs-lookup"><span data-stu-id="76873-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="76873-151">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="76873-151">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/WebApplication1/WebApplication1.App/Pages/Index.cshtml?highlight=7)]

1. <span data-ttu-id="76873-152">Ponownie załaduj stronę.</span><span class="sxs-lookup"><span data-stu-id="76873-152">Reload the page.</span></span> <span data-ttu-id="76873-153">Licznik strony głównej rośnie przez dziesięć za każdym razem **kliknij mnie** przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="76873-153">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="76873-154">Licznik na *licznika* stronie zwiększa o jeden.</span><span class="sxs-lookup"><span data-stu-id="76873-154">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="76873-155">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="76873-155">Route to components</span></span>

<span data-ttu-id="76873-156">`@page` Dyrektywę w górnej części *Counter.cshtml* pliku Określa, że ten składnik jest routingu punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="76873-156">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="76873-157">Składnik licznika obsługuje żądania wysyłane do `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="76873-157">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="76873-158">Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal można używać składnika przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="76873-158">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="76873-159">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="76873-159">Dependency injection</span></span>

<span data-ttu-id="76873-160">Zarejestrowane w kontenerze usługi app Services są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="76873-160">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="76873-161">Wstrzyknięcie usług do składnika za pomocą `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="76873-161">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="76873-162">Sprawdź dyrektywy składnika FetchData (*WebApplication1.App/Pages/FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="76873-162">Examine the directives of the FetchData component (*WebApplication1.App/Pages/FetchData.cshtml*).</span></span> <span data-ttu-id="76873-163">`@inject` Dyrektywy służy do dodania wystąpienia programu `WeatherForecastService` usługi do składnika:</span><span class="sxs-lookup"><span data-stu-id="76873-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

<span data-ttu-id="76873-164">`WeatherForecastService` Usługi jest zarejestrowana jako [pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76873-164">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="76873-165">Składnik FetchData używa usługi wprowadzonego jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:</span><span class="sxs-lookup"><span data-stu-id="76873-165">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

<span data-ttu-id="76873-166">A [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli danych o pogodzie:</span><span class="sxs-lookup"><span data-stu-id="76873-166">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="76873-167">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="76873-167">Build a todo list</span></span>

<span data-ttu-id="76873-168">Dodaj nową stronę do aplikacji, która implementuje listy zadań do wykonania prostego.</span><span class="sxs-lookup"><span data-stu-id="76873-168">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="76873-169">Dodaj pusty plik do *WebApplication1.App/Pages* folder o nazwie *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="76873-169">Add an empty file to the *WebApplication1.App/Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="76873-170">Podaj początkowe znaczniki dla strony:</span><span class="sxs-lookup"><span data-stu-id="76873-170">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="76873-171">Na stronie zadań do wykonania należy dodać do paska nawigacyjnego.</span><span class="sxs-lookup"><span data-stu-id="76873-171">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="76873-172">Składnik NavMenu (*WebApplication1/Shared/NavMenu.csthml*) jest używana w układzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76873-172">The NavMenu component (*WebApplication1/Shared/NavMenu.csthml*) is used in the app's layout.</span></span> <span data-ttu-id="76873-173">Układy są składniki, które pozwalają uniknąć duplikowania zawartości w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76873-173">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="76873-174">Aby uzyskać więcej informacji, zobacz <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="76873-174">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="76873-175">Dodaj `<NavLink>` strony Todo, dodając następujące znaczniki elementu listy poniżej istniejące elementy listy w *WebApplication1/Shared/NavMenu.csthml* pliku:</span><span class="sxs-lookup"><span data-stu-id="76873-175">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *WebApplication1/Shared/NavMenu.csthml* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="76873-176">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="76873-176">Rebuild and run the app.</span></span> <span data-ttu-id="76873-177">Odwiedź nową stronę Todo, aby upewnić się, czy działa link do strony zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="76873-177">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="76873-178">Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasa, która reprezentuje element todo.</span><span class="sxs-lookup"><span data-stu-id="76873-178">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="76873-179">Należy użyć następującego C# kod `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="76873-179">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/WebApplication1/WebApplication1.App/TodoItem.cs)]

1. <span data-ttu-id="76873-180">Wróć do części Todo (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="76873-180">Return to the Todo component (*Todo.cshtml*):</span></span>

   * <span data-ttu-id="76873-181">Dodaj pole do zadań do wykonania w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="76873-181">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="76873-182">Składnik Todo to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="76873-182">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="76873-183">Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy.</span><span class="sxs-lookup"><span data-stu-id="76873-183">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. <span data-ttu-id="76873-184">Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadań do wykonania do listy.</span><span class="sxs-lookup"><span data-stu-id="76873-184">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="76873-185">Dodaj przycisk poniżej listy i tekstowych danych wejściowych:</span><span class="sxs-lookup"><span data-stu-id="76873-185">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. <span data-ttu-id="76873-186">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="76873-186">Rebuild and run the app.</span></span> <span data-ttu-id="76873-187">Gdy **dodawania zadań do wykonania** przycisk jest zaznaczony, nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest powiązaną przycisku.</span><span class="sxs-lookup"><span data-stu-id="76873-187">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="76873-188">Dodaj `AddTodo` metodę, aby składnik zadań do wykonania i zarejestruj go dla przycisku kliknie, za pomocą `onclick` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="76873-188">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   <span data-ttu-id="76873-189">`AddTodo` C# Metoda jest wywoływana po wybraniu przycisku.</span><span class="sxs-lookup"><span data-stu-id="76873-189">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="76873-190">Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` ciąg pola i powiązać wartość wprowadzania przy użyciu tekstu `bind` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="76873-190">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="76873-191">Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy.</span><span class="sxs-lookup"><span data-stu-id="76873-191">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="76873-192">Wyczyść wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="76873-192">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. <span data-ttu-id="76873-193">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="76873-193">Rebuild and run the app.</span></span> <span data-ttu-id="76873-194">Niektórych zadań do wykonania należy dodać do listy rzeczy do zrobienia, aby przetestować nowy kod.</span><span class="sxs-lookup"><span data-stu-id="76873-194">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="76873-195">Tekst tytułu dla każdego elementu todo zyski, można edytować i pola wyboru mogą pomóc użytkownikowi informacje o ukończonych elementów.</span><span class="sxs-lookup"><span data-stu-id="76873-195">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="76873-196">Dodaj dane wejściowe pola wyboru dla każdego elementu todo i powiązać jej wartość na `IsDone` właściwości.</span><span class="sxs-lookup"><span data-stu-id="76873-196">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="76873-197">Zmiana `@todo.Title` do `<input>` powiązany element `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="76873-197">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. <span data-ttu-id="76873-198">Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `<h1>` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie są kompletne (`IsDone` jest `false`).</span><span class="sxs-lookup"><span data-stu-id="76873-198">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="76873-199">Ukończone składnika Todo (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="76873-199">The completed Todo component (*Todo.cshtml*):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/WebApplication1/WebApplication1.App/Pages/Todo.cshtml)]

1. <span data-ttu-id="76873-200">Ponownie skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="76873-200">Rebuild and run the app.</span></span> <span data-ttu-id="76873-201">Dodaj do testowania nowego kodu do wykonania.</span><span class="sxs-lookup"><span data-stu-id="76873-201">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="76873-202">Publikowanie i wdrażanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="76873-202">Publish and deploy the app</span></span>

<span data-ttu-id="76873-203">Aby opublikować aplikację, zobacz <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="76873-203">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
