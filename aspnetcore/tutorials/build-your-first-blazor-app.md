---
title: Utwórz swoją pierwszą aplikację Blazor
author: guardrex
description: Tworzenie aplikacji Blazor krok po kroku i szybko Naucz się podstawowych funkcji w ramach składników Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668129"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="955ce-103">Utwórz swoją pierwszą aplikację Blazor</span><span class="sxs-lookup"><span data-stu-id="955ce-103">Build your first Blazor app</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="955ce-104">W tym samouczku tworzenie aplikacji Blazor krok po kroku i szybko Naucz się podstawowych funkcji w ramach składników Razor.</span><span class="sxs-lookup"><span data-stu-id="955ce-104">In this tutorial, you build a Blazor app step-by-step and quickly learn the basic features of the Razor Components framework.</span></span>

<span data-ttu-id="955ce-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="955ce-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="955ce-106">Zobacz [wprowadzenie](xref:razor-components/get-started) tematu wstępnie wymaganych składników.</span><span class="sxs-lookup"><span data-stu-id="955ce-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="955ce-107">Aby utworzyć projekt w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="955ce-107">To create the project in Visual Studio:</span></span>

1. <span data-ttu-id="955ce-108">Wybierz **pliku** > **nowe** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="955ce-108">Select **File** > **New** > **Project**.</span></span> <span data-ttu-id="955ce-109">Wybierz **Web** > **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="955ce-109">Select **Web** > **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="955ce-110">Nazwa projektu "BlazorApp1" w **nazwa** pola.</span><span class="sxs-lookup"><span data-stu-id="955ce-110">Name the project "BlazorApp1" in the **Name** field.</span></span> <span data-ttu-id="955ce-111">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="955ce-111">Select **OK**.</span></span>

    ![Nowy projekt ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. <span data-ttu-id="955ce-113">**Nowa aplikacja internetowa ASP.NET Core** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="955ce-113">The **New ASP.NET Core Web Application** dialog appears.</span></span> <span data-ttu-id="955ce-114">Upewnij się, że **platformy .NET Core** wybrano u góry.</span><span class="sxs-lookup"><span data-stu-id="955ce-114">Make sure **.NET Core** is selected at the top.</span></span> <span data-ttu-id="955ce-115">Select **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="955ce-115">Select **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="955ce-116">Wybierz **Blazor** szablonu, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="955ce-116">Choose the **Blazor** template and select **OK**.</span></span>

    ![Nowe okno dialogowe aplikacji Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. <span data-ttu-id="955ce-118">Po utworzeniu projektu naciśnij **Ctrl-F5** do uruchomienia aplikacji *bez debugera*.</span><span class="sxs-lookup"><span data-stu-id="955ce-118">Once the project is created, press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="955ce-119">Uruchamianie przy użyciu debugera (**F5**) nie jest obsługiwana w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="955ce-119">Running with the debugger (**F5**) isn't supported at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="955ce-120">W przeciwnym razie przy użyciu programu Visual Studio Utwórz aplikację Blazor w wierszu polecenia w systemie Windows, macOS lub Linux:</span><span class="sxs-lookup"><span data-stu-id="955ce-120">If not using Visual Studio, create the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> <span data-ttu-id="955ce-121">Przejdź do aplikacji przy użyciu adresu localhost i porcie podanym w danych wyjściowych okna konsoli po `dotnet run` jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="955ce-121">Navigate to the app using the localhost address and port provided in the console window output after `dotnet run` is executed.</span></span> <span data-ttu-id="955ce-122">Użyj **Ctrl-C** w oknie konsoli zamknięcie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="955ce-122">Use **Ctrl-C** in the console window to shutdown the app.</span></span>

<span data-ttu-id="955ce-123">Aplikacja Blazor działa w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="955ce-123">The Blazor app runs in the browser:</span></span>

![Strona główna aplikacji Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a><span data-ttu-id="955ce-125">Tworzenie składników</span><span class="sxs-lookup"><span data-stu-id="955ce-125">Build components</span></span>

1. <span data-ttu-id="955ce-126">Przejdź do wszystkich aplikacji trzy strony: Strona główna licznik i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="955ce-126">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span>

    <span data-ttu-id="955ce-127">Te trzy strony są implementowane przez trzy pliki Razor w *stron* folderu: *Index.cshtml*, *Counter.cshtml*, i *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="955ce-127">These three pages are implemented by the three Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span> <span data-ttu-id="955ce-128">Każdy z tych plików implementuje składnik, który został skompilowany i wykonany po stronie klienta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="955ce-128">Each of these files implements a component that's compiled and executed client-side in the browser.</span></span>

1. <span data-ttu-id="955ce-129">Wybierz przycisk na stronie licznika.</span><span class="sxs-lookup"><span data-stu-id="955ce-129">Select the button on the Counter page.</span></span>

    ![Strona główna aplikacji Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    <span data-ttu-id="955ce-131">Każdorazowo, gdy przycisk jest zaznaczony, licznik jest zwiększany bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="955ce-131">Each time the button is selected, the counter is incremented without a page refresh.</span></span> <span data-ttu-id="955ce-132">Normalnie tego rodzaju zachowania po stronie klienta jest obsługiwane w języku JavaScript. ale w tym miejscu jest zaimplementowana w C# i platformy .NET przez `Counter` składnika.</span><span class="sxs-lookup"><span data-stu-id="955ce-132">Normally, this kind of client-side behavior is handled in JavaScript; but here, it's implemented in C# and .NET by the `Counter` component.</span></span>

1. <span data-ttu-id="955ce-133">Zapoznaj się z implementacji `Counter` składnika w *Counter.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="955ce-133">Take a look at the implementation of the `Counter` component in the *Counter.cshtml* file:</span></span>

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    <span data-ttu-id="955ce-134">W interfejsie użytkownika dla `Counter` składnik jest definiowana za pomocą normalnego HTML.</span><span class="sxs-lookup"><span data-stu-id="955ce-134">The UI for the `Counter` component is defined using normal HTML.</span></span> <span data-ttu-id="955ce-135">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="955ce-135">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="955ce-136">Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="955ce-136">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="955ce-137">Nazwa wygenerowanej klasy .NET odpowiada nazwie pliku.</span><span class="sxs-lookup"><span data-stu-id="955ce-137">The name of the generated .NET class matches the name of the file.</span></span>

    <span data-ttu-id="955ce-138">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="955ce-138">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="955ce-139">W `@functions` blokować stan składnika (właściwości, pola) i metod można określić dla obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="955ce-139">In the `@functions` block, component state (properties, fields) and methods can be specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="955ce-140">Następnie można te elementy członkowskie w ramach składnika renderowania logiki oraz obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="955ce-140">These members can then be used as part of the component's rendering logic and for handling events.</span></span>

    <span data-ttu-id="955ce-141">Po wybraniu przycisku `Counter` zarejestrowane składnika `onclick` program obsługi jest wywoływany ( `IncrementCount` metoda) i `Counter` składnika generuje drzewo jego renderowania.</span><span class="sxs-lookup"><span data-stu-id="955ce-141">When the button is selected, the `Counter` component's registered `onclick` handler is called (the `IncrementCount` method) and the `Counter` component regenerates its render tree.</span></span> <span data-ttu-id="955ce-142">Blazor porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszelkie modyfikacje w przeglądarce modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="955ce-142">Blazor compares the new render tree against the previous one and applies any modifications to the browser Document Object Model (DOM).</span></span> <span data-ttu-id="955ce-143">Liczba wyświetlanych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="955ce-143">The displayed count is updated.</span></span>

1. <span data-ttu-id="955ce-144">Zaktualizuj kod znaczników dla `Counter` składnika nagłówek najwyższego poziomu więcej *ekscytujące*.</span><span class="sxs-lookup"><span data-stu-id="955ce-144">Update the markup for the `Counter` component to make the top-level header more *exciting*.</span></span>

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. <span data-ttu-id="955ce-145">Również zmodyfikować C# logiki `Counter` składnika zwiększenie liczby dwa, a nie jeden.</span><span class="sxs-lookup"><span data-stu-id="955ce-145">Also modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. <span data-ttu-id="955ce-146">Odśwież stronę licznika w przeglądarce, aby zobaczyć zmiany.</span><span class="sxs-lookup"><span data-stu-id="955ce-146">Refresh the counter page in the browser to see the changes.</span></span>

    ![Ekscytujące licznika](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a><span data-ttu-id="955ce-148">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="955ce-148">Use components</span></span>

<span data-ttu-id="955ce-149">Po zdefiniowaniu składnik składnika może służyć do implementowania innych składników.</span><span class="sxs-lookup"><span data-stu-id="955ce-149">After a component is defined, the component can be used to implement other components.</span></span> <span data-ttu-id="955ce-150">Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.</span><span class="sxs-lookup"><span data-stu-id="955ce-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

1. <span data-ttu-id="955ce-151">Dodaj `Counter` składnik do strony głównej aplikacji (*Index.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="955ce-151">Add a `Counter` component to the Home page of the app (*Index.cshtml*).</span></span>

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. <span data-ttu-id="955ce-152">Odśwież stronę główną w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="955ce-152">Refresh the home page in the browser.</span></span> <span data-ttu-id="955ce-153">Należy zwrócić uwagę osobne wystąpienie `Counter` składnika na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="955ce-153">Note the separate instance of the `Counter` component on the Home page.</span></span>

    ![Strona główna Blazor za pomocą licznika](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a><span data-ttu-id="955ce-155">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="955ce-155">Component parameters</span></span>

<span data-ttu-id="955ce-156">Składniki mogą też istnieć parametry, które są definiowane przy użyciu prywatnych właściwości w klasie składnika ozdobione `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="955ce-156">Components can also have parameters, which are defined using private properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="955ce-157">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="955ce-157">Use attributes to specify arguments for a component in markup.</span></span> 

1. <span data-ttu-id="955ce-158">Aktualizacja `Counter` składnik, aby mieć `IncrementAmount` parametr, który ma domyślnie wartość 1.</span><span class="sxs-lookup"><span data-stu-id="955ce-158">Update the `Counter` component to have an `IncrementAmount` parameter that defaults to 1.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="955ce-159">Z programu Visual Studio można szybko dodać parametr składnika za pomocą `para` fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="955ce-159">From Visual Studio, you can quickly add a component parameter by using the `para` snippet.</span></span> <span data-ttu-id="955ce-160">Typ `para` , a następnie naciśnij klawisz `Tab` dwa razy.</span><span class="sxs-lookup"><span data-stu-id="955ce-160">Type `para` and then press the `Tab` key twice.</span></span>

1. <span data-ttu-id="955ce-161">Na stronie głównej (*Index.cshtml*), zmienić wielkość przyrostu `Counter` do 10, ustawiając atrybut, który pasuje do nazwy składnika Właściwość `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="955ce-161">On the Home page (*Index.cshtml*), change the increment amount for the `Counter` to 10 by setting an attribute that matches the name of the component's property for `IncrementCount`.</span></span>

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. <span data-ttu-id="955ce-162">Ponownie załaduj stronę.</span><span class="sxs-lookup"><span data-stu-id="955ce-162">Reload the page.</span></span>

    <span data-ttu-id="955ce-163">Licznik w przyrostach teraz strony głównej, 10, gdy wartość licznika, na stronie licznika nadal zwiększa się o 1.</span><span class="sxs-lookup"><span data-stu-id="955ce-163">The counter on the Home page now increments by 10, while the counter on the Counter page still increments by 1.</span></span>

    ![Liczba Blazor przez dziesięć](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a><span data-ttu-id="955ce-165">Kierowanie do składników</span><span class="sxs-lookup"><span data-stu-id="955ce-165">Route to components</span></span>

<span data-ttu-id="955ce-166">`@page` Dyrektywę w górnej części *Counter.cshtml* pliku Określa, że ten składnik jest strona, do którego można przekierować żądania.</span><span class="sxs-lookup"><span data-stu-id="955ce-166">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a page to which requests can be routed.</span></span> <span data-ttu-id="955ce-167">W szczególności `Counter` składnik obsługi żądań wysyłanych do `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="955ce-167">Specifically, the `Counter` component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="955ce-168">Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal składnika mogą być używane przez inne składniki.</span><span class="sxs-lookup"><span data-stu-id="955ce-168">Without the `@page` directive, the component wouldn't handle routed requests, but the component could still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="955ce-169">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="955ce-169">Dependency injection</span></span>

<span data-ttu-id="955ce-170">Usługi zarejestrowane przy użyciu dostawcy usługi app service są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="955ce-170">Services registered with the app's service provider are available to components via [dependency injection (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="955ce-171">Usługi można dodane do składnika za pomocą `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="955ce-171">Services can be injected into a component using the `@inject` directive.</span></span>

<span data-ttu-id="955ce-172">Zapoznaj się z implementacji `FetchData` składnika w *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="955ce-172">Take a look at the implementation of the `FetchData` component in *FetchData.cshtml*.</span></span> <span data-ttu-id="955ce-173">`@inject` Dyrektywy służy do dodania [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) wystąpienie do składnika.</span><span class="sxs-lookup"><span data-stu-id="955ce-173">The `@inject` directive is used to inject an [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) instance into the component.</span></span>

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

<span data-ttu-id="955ce-174">`FetchData` Składnik używa wprowadzonego `HttpClient` do pobierania danych JSON z serwera, gdy składnik zostanie zainicjowany.</span><span class="sxs-lookup"><span data-stu-id="955ce-174">The `FetchData` component uses the injected `HttpClient` to retrieve JSON data from the server when the component is initialized.</span></span> <span data-ttu-id="955ce-175">Dzieje się w tle `HttpClient` dostarczone przez Blazor jest implementowana w czasie wykonywania za pomocą międzyoperacyjności języka JavaScript do wywołania interfejsu API Fetch bazowego przeglądarki do wysłania żądania (z C#, jest możliwe do wywołania dowolnego biblioteki lub przeglądarka interfejsu API języka JavaScript).</span><span class="sxs-lookup"><span data-stu-id="955ce-175">Under the covers, the `HttpClient` provided by the Blazor runtime is implemented using JavaScript interop to call the underlying browser's Fetch API to send the request (from C#, it's possible to call any JavaScript library or browser API).</span></span> <span data-ttu-id="955ce-176">Pobrane dane jest przeprowadzona deserializacja `forecasts` C# zmiennej jako tablicę `WeatherForecast` obiektów.</span><span class="sxs-lookup"><span data-stu-id="955ce-176">The retrieved data is deserialized into the `forecasts` C# variable as an array of `WeatherForecast` objects.</span></span>

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

<span data-ttu-id="955ce-177">A `@foreach` pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli o pogodzie.</span><span class="sxs-lookup"><span data-stu-id="955ce-177">A `@foreach` loop is used to render each forecast instance as a row in the weather table.</span></span>

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a><span data-ttu-id="955ce-178">Tworzenie listy zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="955ce-178">Build a todo list</span></span>

<span data-ttu-id="955ce-179">Dodaj nową stronę do aplikacji, która implementuje listy zadań do wykonania prostego.</span><span class="sxs-lookup"><span data-stu-id="955ce-179">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="955ce-180">Dodaj pusty plik tekstowy do *stron* folder o nazwie *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="955ce-180">Add an empty text file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="955ce-181">Podaj początkowe znaczniki dla strony.</span><span class="sxs-lookup"><span data-stu-id="955ce-181">Provide the initial markup for the page.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. <span data-ttu-id="955ce-182">Dodaj stronę zadań do wykonania na pasku nawigacyjnym, aktualizując *Shared/NavMenu.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="955ce-182">Add the Todo page to the navigation bar by updating *Shared/NavMenu.cshtml*.</span></span> <span data-ttu-id="955ce-183">Dodaj `NavLink` strony Todo, dodając następujące znaczniki elementu listy poniżej istniejące elementy listy.</span><span class="sxs-lookup"><span data-stu-id="955ce-183">Add a `NavLink` for the Todo page by adding the following list item markup below the existing list items.</span></span>

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. <span data-ttu-id="955ce-184">Odśwież aplikację w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="955ce-184">Refresh the app in the browser.</span></span> <span data-ttu-id="955ce-185">Zobacz stronę nowych zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="955ce-185">See the new Todo page.</span></span>

    ![Blazor todo start](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. <span data-ttu-id="955ce-187">Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasy do reprezentowania zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="955ce-187">Add a *TodoItem.cs* file to the root of the project to hold a class to represent the todo items.</span></span>

1. <span data-ttu-id="955ce-188">Należy użyć następującego C# kod `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="955ce-188">Use the following C# code for the `TodoItem` class.</span></span>

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. <span data-ttu-id="955ce-189">Wróć do `Todo` składnika w *Todo.cshtml* i dodać pole do zadań do wykonania w `@functions` bloku.</span><span class="sxs-lookup"><span data-stu-id="955ce-189">Go back to the `Todo` component in *Todo.cshtml* and add a field for the todos in a `@functions` block.</span></span> <span data-ttu-id="955ce-190">`Todo` Składnik to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="955ce-190">The `Todo` component uses this field to maintain the state of the todo list.</span></span>

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="955ce-191">Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy.</span><span class="sxs-lookup"><span data-stu-id="955ce-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. <span data-ttu-id="955ce-192">Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadań do wykonania do listy.</span><span class="sxs-lookup"><span data-stu-id="955ce-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="955ce-193">Dodawanie tekstowych danych wejściowych i przycisk poniżej listy.</span><span class="sxs-lookup"><span data-stu-id="955ce-193">Add a text input and a button below the list.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="955ce-194">Odśwież przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="955ce-194">Refresh the browser.</span></span>

    ![Dodawanie przycisku todo](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    <span data-ttu-id="955ce-196">Nic się nie dzieje po **dodawania zadań do wykonania** przycisk jest zaznaczony, ponieważ żadna procedura obsługi zdarzeń jest powiązaną przycisku.</span><span class="sxs-lookup"><span data-stu-id="955ce-196">Nothing happens when the **Add todo** button is selected because no event handler is wired up to the button.</span></span>

1. <span data-ttu-id="955ce-197">Dodaj `AddTodo` metody `Todo` składnika i zarejestruj go dla przycisku kliknie, za pomocą `onclick` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="955ce-197">Add an `AddTodo` method to the `Todo` component and register it for button clicks using the `onclick` attribute.</span></span>

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    <span data-ttu-id="955ce-198">`AddTodo` C# Metoda jest wywoływana za każdym razem, gdy przycisk jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="955ce-198">The `AddTodo` C# method is called every time the button is selected.</span></span>

1. <span data-ttu-id="955ce-199">Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` ciąg pola i powiązać wartość wprowadzania przy użyciu tekstu `bind` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="955ce-199">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute.</span></span>

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. <span data-ttu-id="955ce-200">Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy.</span><span class="sxs-lookup"><span data-stu-id="955ce-200">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="955ce-201">Nie zapomnij wyczyścić wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="955ce-201">Don't forget to clear the value of the text input by setting `newTodo` to an empty string.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. <span data-ttu-id="955ce-202">Odśwież przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="955ce-202">Refresh the browser.</span></span> <span data-ttu-id="955ce-203">Niektórych zadań do wykonania należy dodać do listy rzeczy do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="955ce-203">Add some todos to the todo list.</span></span>

    ![Dodawanie zadań do wykonania](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. <span data-ttu-id="955ce-205">Wreszcie co to jest lista czynności do wykonania, bez pola wyboru?</span><span class="sxs-lookup"><span data-stu-id="955ce-205">Lastly, what's a todo list without check boxes?</span></span> <span data-ttu-id="955ce-206">Tekst tytułu dla każdego elementu todo zyski, można edytować także.</span><span class="sxs-lookup"><span data-stu-id="955ce-206">The title text for each todo item can be made editable as well.</span></span> <span data-ttu-id="955ce-207">Dodaj dane wejściowe pola wyboru i tekst wejściowy dla każdego elementu todo i ich wartości, aby powiązać `Title` i `IsDone` właściwości, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="955ce-207">Add a check box input and text input for each todo item and bind their values to the `Title` and `IsDone` properties, respectively.</span></span>

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. <span data-ttu-id="955ce-208">Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `h1` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie zostały jeszcze wykonane (`IsDone` jest `false`).</span><span class="sxs-lookup"><span data-stu-id="955ce-208">To verify that these values are bound, update the `h1` header to show a count of the number of todo items that are not yet done (`IsDone` is `false`).</span></span>

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. <span data-ttu-id="955ce-209">Gotowy `Todo` składnik powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="955ce-209">The completed `Todo` component should look like this:</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

<span data-ttu-id="955ce-210">Odśwież aplikację w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="955ce-210">Refresh the app in the browser.</span></span> <span data-ttu-id="955ce-211">Spróbuj dodać niektóre elementy zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="955ce-211">Try adding some todo items.</span></span>

![Zakończono listy zadań do wykonania Blazor](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a><span data-ttu-id="955ce-213">Publikowanie i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="955ce-213">Publish and deploy</span></span>

<span data-ttu-id="955ce-214">Korzystając z programu Visual Studio, wykonaj następujące kroki, aby opublikować aplikację Blazor zadań do wykonania na platformie Azure:</span><span class="sxs-lookup"><span data-stu-id="955ce-214">When using Visual Studio, perform the following steps to publish the Todo Blazor app to Azure:</span></span>

1. <span data-ttu-id="955ce-215">Kliknij prawym przyciskiem myszy nad projektem w **Eksploratora rozwiązań** i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="955ce-215">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="955ce-216">W **wybierz lokalizację docelową publikowania** okno dialogowe, wybierz opcję **usługi App Service** i **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="955ce-216">In the **Pick a publish target** dialog, select **App Service** and **Create New**.</span></span> <span data-ttu-id="955ce-217">Wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="955ce-217">Select **Publish**.</span></span>

    ![Wybierz lokalizację docelową publikowania](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. <span data-ttu-id="955ce-219">W **Tworzenie usługi App Service** okno dialogowe, wybierz nazwę aplikacji i wybierz subskrypcję, grupy zasobów, plan hostingu.</span><span class="sxs-lookup"><span data-stu-id="955ce-219">In the **Create App Service** dialog, choose a name for the app and select the subscription, resource group, and hosting plan.</span></span> <span data-ttu-id="955ce-220">Wybierz **Utwórz** do tworzenia usługi app service i publikowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="955ce-220">Select **Create** to create the app service and publish the app.</span></span>

    ![Utwórz usługę app service](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

<span data-ttu-id="955ce-222">Poczekaj około minucie dla aplikacji do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="955ce-222">Wait a minute or so for the app to be deployed.</span></span>

<span data-ttu-id="955ce-223">Aplikacja powinna teraz być uruchomiona na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="955ce-223">The app should now be running in Azure.</span></span> <span data-ttu-id="955ce-224">Oznacz element todo, aby utworzyć pierwszą aplikację Blazor jako *gotowe*.</span><span class="sxs-lookup"><span data-stu-id="955ce-224">Mark the todo item to build your first Blazor app as *done*.</span></span> <span data-ttu-id="955ce-225">Brawo!</span><span class="sxs-lookup"><span data-stu-id="955ce-225">Nice job!</span></span>

![Blazor na platformie Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> <span data-ttu-id="955ce-227">W przeciwnym razie przy użyciu programu Visual Studio, Opublikuj aplikację Blazor w wierszu polecenia w systemie Windows, macOS lub Linux:</span><span class="sxs-lookup"><span data-stu-id="955ce-227">If not using Visual Studio, publish the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet publish -c Release
> ```
>
> <span data-ttu-id="955ce-228">Wdrożenie jest tworzony w */bin/wersji/\<platforma docelowa > / publish* folderu.</span><span class="sxs-lookup"><span data-stu-id="955ce-228">The deployment is created in the */bin/Release/\<target-framework>/publish* folder.</span></span> <span data-ttu-id="955ce-229">Przenieś zawartość *publikowania* folderu na serwerze lub innej usługi hostingu.</span><span class="sxs-lookup"><span data-stu-id="955ce-229">Move the contents of the *publish* folder to the server or hosting service.</span></span>
>
> <span data-ttu-id="955ce-230">Aby uzyskać więcej informacji, zobacz [hosta i wdrażanie](xref:host-and-deploy/razor-components/index#publish-the-app) tematu.</span><span class="sxs-lookup"><span data-stu-id="955ce-230">For more information, see the [Host and deploy](xref:host-and-deploy/razor-components/index#publish-the-app) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="955ce-231">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="955ce-231">Additional resources</span></span>

<span data-ttu-id="955ce-232">Dla bardziej zaangażowane Blazor przykładowej aplikacji, zapoznaj się z [lotu Finder próbki](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="955ce-232">For a more involved Blazor sample app, check out the [Flight Finder sample](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) on GitHub.</span></span>

![Lotu Blazor wyszukiwania](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
