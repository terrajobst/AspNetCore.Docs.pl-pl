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
# <a name="build-your-first-blazor-app"></a>Utwórz swoją pierwszą aplikację Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

W tym samouczku dowiesz się, jak tworzyć i modyfikować aplikację Blazor.

Postępuj zgodnie ze wskazówkami w <xref:blazor/get-started> artykuł, aby utworzyć projekt Blazor na potrzeby tego samouczka. Nadaj projektowi nazwę *ToDoList*.

## <a name="build-components"></a>Tworzenie składników

1. Przejdź do każdego z trzech stron aplikacji w *stron* folderu: Strona główna licznik i pobierania danych. Te strony są implementowane przez pliki składników Razor *Index.razor*, *Counter.razor*, i *FetchData.razor*.

1. Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Blazor zapewnia lepsze przy użyciu podejścia C#.

1. Sprawdź wykonania `Counter` składnika w *Counter.razor* pliku.

   *Pages/Counter.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   Interfejs użytkownika z `Counter` składnika jest definiowana za pomocą kodu HTML. Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor). Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji. Nazwa wygenerowanej klasy .NET zgodny z nazwą pliku.

   Elementy członkowskie klasy składników są zdefiniowane w `@code` bloku. W `@code` blokować stan składnika (właściwości, pola) i metod są określone dla obsługi zdarzeń lub Definiowanie logiki innych składników. Te elementy członkowskie są następnie używane w ramach składnika renderowania logiki oraz obsługi zdarzeń.

   Gdy **kliknij mnie** wybrany przycisk:

   * `Counter` Zarejestrowane składnika `onclick` program obsługi jest wywoływany ( `IncrementCount` metody).
   * `Counter` Składnika generuje drzewo jego renderowania.
   * Nowe drzewo renderowania w porównaniu do poprzedniego.
   * Modyfikacje do modelu DOM (Document Object) są stosowane. Liczba wyświetlanych jest aktualizowana.

1. Modyfikowanie C# logiki `Counter` składnika zwiększenie liczby dwa, a nie jeden.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Ponownie skompiluj i uruchom aplikację, aby zobaczyć zmiany. Wybierz **kliknij mnie** przycisku. Zwiększa licznik przez dwa.

## <a name="use-components"></a>Używanie składników

Obejmują składnika w innym składniku przy użyciu składni HTML.

1. Dodaj `Counter` składnika w aplikacji `Index` składnika, dodając `<Counter />` elementu `Index` składnika (*Index.razor*).

   Jeśli używasz Blazor po stronie klienta dla tego środowiska `SurveyPrompt` składnik jest używany przez `Index` składnika. Zastąp `<SurveyPrompt>` element z `<Counter />` elementu. Jeśli używasz aplikacji po stronie serwera Blazor dla tego środowiska, należy dodać `<Counter />` elementu `Index` składników:

   *Pages/Index.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Ponownie skompiluj i uruchom aplikację. `Index` Składnik ma swoje własne licznika.

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą także mieć parametrów. Składnik parametry są definiowane za pomocą właściwości niepubliczne klasy składnika ozdobione `[Parameter]`. Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.

1. Aktualizowanie składnika `@code` C# kodu:

   * Dodaj `IncrementAmount` ozdobione właściwość `[Parameter]` atrybutu.
   * Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.

   *Pages/Counter.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Określ `IncrementAmount` parametru w `Index` składnika `<Counter>` elementu za pomocą atrybutu. Ustaw wartość do zwiększenia licznika dziesięciu.

   *Pages/Index.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. Załaduj ponownie `Index` składnika. Licznik rośnie przez dziesięć za każdym razem **kliknij mnie** przycisk jest zaznaczony. Licznik `Counter` składnika w dalszym ciągu o jeden.

## <a name="route-to-components"></a>Kierowanie do składników

`@page` Dyrektywę w górnej części *Counter.razor* pliku Określa, że `Counter` składnik jest routingu punkt końcowy. `Counter` Składnik obsługi żądań wysyłanych do `/counter`. Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal można używać składnika przez inne składniki.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Zarejestrowane w kontenerze usługi app Services są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). Wstrzyknięcie usług do składnika za pomocą `@inject` dyrektywy.

Sprawdź dyrektyw dotyczących `FetchData` składnika.

Jeśli praca z aplikacją po stronie serwera Blazor `WeatherForecastService` usługi jest zarejestrowana jako [pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji. `@inject` Dyrektywy służy do dodania wystąpienia programu `WeatherForecastService` usługi do składnika.

*Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

`FetchData` Składnik używa usługi wprowadzonego jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Pracy z aplikacją po stronie klienta Blazor `HttpClient` są wstrzykiwane mają zostać pobrane dane prognozy pogody *weather.json* w pliku *wwwroot/przykładowe data* folderu:

*Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

A [ \@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli danych o pogodzie:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a>Tworzenie listy zadań do wykonania

Dodawanie nowego składnika do aplikacji, która implementuje listy zadań do wykonania prostego.

1. Dodaj pusty plik o nazwie *Todo.razor* do aplikacji w *stron* folderu:

1. Podaj początkowe znaczniki dla składnika:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Dodaj `Todo` składnika na pasku nawigacyjnym.

   `NavMenu` Składnika (*Shared/NavMenu.razor*) jest używana w układzie aplikacji. Układy są składniki, które pozwalają uniknąć duplikowania zawartości w aplikacji.

   Dodaj `<NavLink>` elementu `Todo` składnika, dodając następujące znaczniki elementu listy poniżej istniejącej listy elementów w *Shared/NavMenu.razor* pliku:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Ponownie skompiluj i uruchom aplikację. Odwiedź nową stronę Todo, aby potwierdzić, że link do `Todo` działania składnika.

1. Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasa, która reprezentuje element todo. Należy użyć następującego C# kod `TodoItem` klasy:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Wróć do `Todo` składnika (*Pages/Todo.razor*):

   * Dodaj pole do wykonania w `@code` bloku. `Todo` Składnik to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.
   * Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy (`<li>`).

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadania do wykonania do listy. Dodaj tekstowe dane wejściowe (`<input>`) i przycisk (`<button>`) poniżej nieuporządkowaną listę (`<ul>...</ul>`):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Ponownie skompiluj i uruchom aplikację. Gdy **dodawania zadań do wykonania** przycisk jest zaznaczony, nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest powiązaną przycisku.

1. Dodaj `AddTodo` metody `Todo` składnika i zarejestruj je dla przycisku wyborów przy użyciu `@onclick` atrybutu. `AddTodo` C# Metoda jest wywoływana po wybraniu przycisku:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` pola ciągu w górnej części `@code` blokowania i powiązać wartość Tekst wejściowy przy użyciu `bind` atrybutu w `<input>` elementu:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="@newTodo" />
   ```

1. Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy. Wyczyść wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Ponownie skompiluj i uruchom aplikację. Niektóre zadania do wykonania należy dodać do listy rzeczy do zrobienia, aby przetestować nowy kod.

1. Tekst tytułu dla każdego elementu todo zyski, można edytować, a pole wyboru może pomóc użytkownikowi informacje o ukończonych elementów. Dodaj dane wejściowe pola wyboru dla każdego elementu todo i powiązać jej wartość na `IsDone` właściwości. Zmiana `@todo.Title` do `<input>` powiązany element `@todo.Title`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `<h1>` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie są kompletne (`IsDone` jest `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Gotowy `Todo` składnika (*Pages/Todo.razor*):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Ponownie skompiluj i uruchom aplikację. Dodaj do testowania nowego kodu do wykonania.

> [!div class="nextstepaction"]
> <xref:blazor/components>
