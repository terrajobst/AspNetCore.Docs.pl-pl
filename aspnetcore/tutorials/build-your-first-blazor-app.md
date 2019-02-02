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
# <a name="build-your-first-blazor-app"></a>Utwórz swoją pierwszą aplikację Blazor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

W tym samouczku tworzenie aplikacji Blazor krok po kroku i szybko Naucz się podstawowych funkcji w ramach składników Razor.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)). Zobacz [wprowadzenie](xref:razor-components/get-started) tematu wstępnie wymaganych składników.

Aby utworzyć projekt w programie Visual Studio:

1. Wybierz **pliku** > **nowe** > **projektu**. Wybierz **Web** > **aplikacji sieci Web platformy ASP.NET Core**. Nazwa projektu "BlazorApp1" w **nazwa** pola. Kliknij przycisk **OK**.

    ![Nowy projekt ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. **Nowa aplikacja internetowa ASP.NET Core** zostanie wyświetlone okno dialogowe. Upewnij się, że **platformy .NET Core** wybrano u góry. Select **ASP.NET Core 2.1**. Wybierz **Blazor** szablonu, a następnie wybierz **OK**.

    ![Nowe okno dialogowe aplikacji Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. Po utworzeniu projektu naciśnij **Ctrl-F5** do uruchomienia aplikacji *bez debugera*. Uruchamianie przy użyciu debugera (**F5**) nie jest obsługiwana w tej chwili.

> [!NOTE]
> W przeciwnym razie przy użyciu programu Visual Studio Utwórz aplikację Blazor w wierszu polecenia w systemie Windows, macOS lub Linux:
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> Przejdź do aplikacji przy użyciu adresu localhost i porcie podanym w danych wyjściowych okna konsoli po `dotnet run` jest wykonywany. Użyj **Ctrl-C** w oknie konsoli zamknięcie aplikacji.

Aplikacja Blazor działa w przeglądarce:

![Strona główna aplikacji Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a>Tworzenie składników

1. Przejdź do wszystkich aplikacji trzy strony: Strona główna licznik i pobierania danych.

    Te trzy strony są implementowane przez trzy pliki Razor w *stron* folderu: *Index.cshtml*, *Counter.cshtml*, i *FetchData.cshtml*. Każdy z tych plików implementuje składnik, który został skompilowany i wykonany po stronie klienta w przeglądarce.

1. Wybierz przycisk na stronie licznika.

    ![Strona główna aplikacji Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    Każdorazowo, gdy przycisk jest zaznaczony, licznik jest zwiększany bez odświeżania strony. Normalnie tego rodzaju zachowania po stronie klienta jest obsługiwane w języku JavaScript. ale w tym miejscu jest zaimplementowana w C# i platformy .NET przez `Counter` składnika.

1. Zapoznaj się z implementacji `Counter` składnika w *Counter.cshtml* pliku:

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

    W interfejsie użytkownika dla `Counter` składnik jest definiowana za pomocą normalnego HTML. Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji. Nazwa wygenerowanej klasy .NET odpowiada nazwie pliku.

    Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku. W `@functions` blokować stan składnika (właściwości, pola) i metod można określić dla obsługi zdarzeń lub Definiowanie logiki innych składników. Następnie można te elementy członkowskie w ramach składnika renderowania logiki oraz obsługi zdarzeń.

    Po wybraniu przycisku `Counter` zarejestrowane składnika `onclick` program obsługi jest wywoływany ( `IncrementCount` metoda) i `Counter` składnika generuje drzewo jego renderowania. Blazor porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszelkie modyfikacje w przeglądarce modelu DOM (Document Object). Liczba wyświetlanych jest aktualizowana.

1. Zaktualizuj kod znaczników dla `Counter` składnika nagłówek najwyższego poziomu więcej *ekscytujące*.

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. Również zmodyfikować C# logiki `Counter` składnika zwiększenie liczby dwa, a nie jeden.

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. Odśwież stronę licznika w przeglądarce, aby zobaczyć zmiany.

    ![Ekscytujące licznika](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a>Używanie składników

Po zdefiniowaniu składnik składnika może służyć do implementowania innych składników. Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.

1. Dodaj `Counter` składnik do strony głównej aplikacji (*Index.cshtml*).

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. Odśwież stronę główną w przeglądarce. Należy zwrócić uwagę osobne wystąpienie `Counter` składnika na stronie głównej.

    ![Strona główna Blazor za pomocą licznika](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą też istnieć parametry, które są definiowane przy użyciu prywatnych właściwości w klasie składnika ozdobione `[Parameter]`. Używanie atrybutów, aby określić argumenty dla składnika w znacznikach. 

1. Aktualizacja `Counter` składnik, aby mieć `IncrementAmount` parametr, który ma domyślnie wartość 1.

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
    > Z programu Visual Studio można szybko dodać parametr składnika za pomocą `para` fragmentu kodu. Typ `para` , a następnie naciśnij klawisz `Tab` dwa razy.

1. Na stronie głównej (*Index.cshtml*), zmienić wielkość przyrostu `Counter` do 10, ustawiając atrybut, który pasuje do nazwy składnika Właściwość `IncrementCount`.

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. Ponownie załaduj stronę.

    Licznik w przyrostach teraz strony głównej, 10, gdy wartość licznika, na stronie licznika nadal zwiększa się o 1.

    ![Liczba Blazor przez dziesięć](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a>Kierowanie do składników

`@page` Dyrektywę w górnej części *Counter.cshtml* pliku Określa, że ten składnik jest strona, do którego można przekierować żądania. W szczególności `Counter` składnik obsługi żądań wysyłanych do `/Counter`. Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal składnika mogą być używane przez inne składniki.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Usługi zarejestrowane przy użyciu dostawcy usługi app service są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Usługi można dodane do składnika za pomocą `@inject` dyrektywy.

Zapoznaj się z implementacji `FetchData` składnika w *FetchData.cshtml*. `@inject` Dyrektywy służy do dodania [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) wystąpienie do składnika.

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

`FetchData` Składnik używa wprowadzonego `HttpClient` do pobierania danych JSON z serwera, gdy składnik zostanie zainicjowany. Dzieje się w tle `HttpClient` dostarczone przez Blazor jest implementowana w czasie wykonywania za pomocą międzyoperacyjności języka JavaScript do wywołania interfejsu API Fetch bazowego przeglądarki do wysłania żądania (z C#, jest możliwe do wywołania dowolnego biblioteki lub przeglądarka interfejsu API języka JavaScript). Pobrane dane jest przeprowadzona deserializacja `forecasts` C# zmiennej jako tablicę `WeatherForecast` obiektów.

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

A `@foreach` pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli o pogodzie.

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

## <a name="build-a-todo-list"></a>Tworzenie listy zadań do wykonania

Dodaj nową stronę do aplikacji, która implementuje listy zadań do wykonania prostego.

1. Dodaj pusty plik tekstowy do *stron* folder o nazwie *Todo.cshtml*.

1. Podaj początkowe znaczniki dla strony.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. Dodaj stronę zadań do wykonania na pasku nawigacyjnym, aktualizując *Shared/NavMenu.cshtml*. Dodaj `NavLink` strony Todo, dodając następujące znaczniki elementu listy poniżej istniejące elementy listy.

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. Odśwież aplikację w przeglądarce. Zobacz stronę nowych zadań do wykonania.

    ![Blazor todo start](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasy do reprezentowania zadań do wykonania.

1. Należy użyć następującego C# kod `TodoItem` klasy.

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. Wróć do `Todo` składnika w *Todo.cshtml* i dodać pole do zadań do wykonania w `@functions` bloku. `Todo` Składnik to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy.

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

1. Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadań do wykonania do listy. Dodawanie tekstowych danych wejściowych i przycisk poniżej listy.

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

1. Odśwież przeglądarkę.

    ![Dodawanie przycisku todo](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    Nic się nie dzieje po **dodawania zadań do wykonania** przycisk jest zaznaczony, ponieważ żadna procedura obsługi zdarzeń jest powiązaną przycisku.

1. Dodaj `AddTodo` metody `Todo` składnika i zarejestruj go dla przycisku kliknie, za pomocą `onclick` atrybutu.

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

    `AddTodo` C# Metoda jest wywoływana za każdym razem, gdy przycisk jest zaznaczony.

1. Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` ciąg pola i powiązać wartość wprowadzania przy użyciu tekstu `bind` atrybutu.

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy. Nie zapomnij wyczyścić wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg.

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

1. Odśwież przeglądarkę. Niektórych zadań do wykonania należy dodać do listy rzeczy do zrobienia.

    ![Dodawanie zadań do wykonania](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. Wreszcie co to jest lista czynności do wykonania, bez pola wyboru? Tekst tytułu dla każdego elementu todo zyski, można edytować także. Dodaj dane wejściowe pola wyboru i tekst wejściowy dla każdego elementu todo i ich wartości, aby powiązać `Title` i `IsDone` właściwości, odpowiednio.

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

1. Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `h1` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie zostały jeszcze wykonane (`IsDone` jest `false`).

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. Gotowy `Todo` składnik powinien wyglądać następująco:

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

Odśwież aplikację w przeglądarce. Spróbuj dodać niektóre elementy zadań do wykonania.

![Zakończono listy zadań do wykonania Blazor](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a>Publikowanie i wdrażanie

Korzystając z programu Visual Studio, wykonaj następujące kroki, aby opublikować aplikację Blazor zadań do wykonania na platformie Azure:

1. Kliknij prawym przyciskiem myszy nad projektem w **Eksploratora rozwiązań** i wybierz **Publikuj**.

1. W **wybierz lokalizację docelową publikowania** okno dialogowe, wybierz opcję **usługi App Service** i **Utwórz nowy**. Wybierz **publikowania**.

    ![Wybierz lokalizację docelową publikowania](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. W **Tworzenie usługi App Service** okno dialogowe, wybierz nazwę aplikacji i wybierz subskrypcję, grupy zasobów, plan hostingu. Wybierz **Utwórz** do tworzenia usługi app service i publikowania aplikacji.

    ![Utwórz usługę app service](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

Poczekaj około minucie dla aplikacji do wdrożenia.

Aplikacja powinna teraz być uruchomiona na platformie Azure. Oznacz element todo, aby utworzyć pierwszą aplikację Blazor jako *gotowe*. Brawo!

![Blazor na platformie Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> W przeciwnym razie przy użyciu programu Visual Studio, Opublikuj aplikację Blazor w wierszu polecenia w systemie Windows, macOS lub Linux:
>
> ```console
> dotnet publish -c Release
> ```
>
> Wdrożenie jest tworzony w */bin/wersji/\<platforma docelowa > / publish* folderu. Przenieś zawartość *publikowania* folderu na serwerze lub innej usługi hostingu.
>
> Aby uzyskać więcej informacji, zobacz [hosta i wdrażanie](xref:host-and-deploy/razor-components/index#publish-the-app) tematu.

## <a name="additional-resources"></a>Dodatkowe zasoby

Dla bardziej zaangażowane Blazor przykładowej aplikacji, zapoznaj się z [lotu Finder próbki](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) w witrynie GitHub.

![Lotu Blazor wyszukiwania](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
