---
title: Utwórz swoją pierwszą aplikację Blazor
author: guardrex
description: Tworzenie aplikacji Blazor krok po kroku.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 310eb211f68076d6f52d6427940e07736d833e5b
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614877"
---
# <a name="build-your-first-blazor-app"></a>Utwórz swoją pierwszą aplikację Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

W tym samouczku przedstawiono sposób tworzenia aplikacji za pomocą składników Razor po stronie serwera lub Blazor po stronie klienta.

Środowisko przy użyciu platformy ASP.NET Core Razor składniki serwera (*zalecane*):

* Postępuj zgodnie z *Razor składników środowiska* ze wskazówkami zawartymi w <xref:blazor/get-started#server-side-razor-components-experience> do tworzenia projektu na podstawie składników Razor.
* Nadaj projektowi nazwę `RazorComponents`.

Aby uzyskać doświadczenie w korzystaniu z Blazor:

* Postępuj zgodnie z *środowisko Blazor* ze wskazówkami zawartymi w <xref:blazor/get-started#client-side-blazor-experience> do tworzenia projektu na podstawie Blazor.
* Nadaj projektowi nazwę `Blazor`.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)). Zobacz następujące tematy dotyczące wymagań wstępnych:

## <a name="build-components"></a>Tworzenie składników

1. Przejdź do każdego z trzech stron aplikacji w *składniki/strony* folder (*stron* w Blazor): Strona główna licznik i pobierania danych. Te strony są implementowane przez pliki Razor składników: *Index.razor*, *Counter.razor*, i *FetchData.razor*. (Blazor w dalszym ciągu używa *.cshtml* rozszerzenie pliku: *Index.cshtml*, *Counter.cshtml*, i *FetchData.cshtml*.)

1. Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Blazor zapewnia lepsze przy użyciu podejścia C#.

1. Zapoznania się z implementacją składnikiem licznika w *Counter.razor* pliku.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* w Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   Interfejs użytkownika składnika licznika jest zdefiniowana za pomocą kodu HTML. Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor). Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji. Nazwa wygenerowanej klasy .NET zgodny z nazwą pliku.

   Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku. W `@functions` blokować stan składnika (właściwości, pola) i metod są określone dla obsługi zdarzeń lub Definiowanie logiki innych składników. Te elementy członkowskie są następnie używane w ramach składnika renderowania logiki oraz obsługi zdarzeń.

   Gdy **kliknij mnie** wybrany przycisk:

   * Zarejestrowane składnik licznika `onclick` program obsługi jest wywoływany ( `IncrementCount` metody).
   * Składnik licznika generuje drzewo jego renderowania.
   * Nowe drzewo renderowania w porównaniu do poprzedniego.
   * Modyfikacje do modelu DOM (Document Object) są stosowane. Liczba wyświetlanych jest aktualizowana.

1. Modyfikowanie C# logiki licznika składnika zwiększenie liczby dwa, a nie jeden.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Ponownie skompiluj i uruchom aplikację, aby zobaczyć zmiany. Wybierz **kliknij mnie** przycisk i zwiększa licznik przez dwa.

## <a name="use-components"></a>Używanie składników

Uwzględnij składnika w innym składniku przy użyciu składni notacji HTML.

1. Dodaj składnik licznika do składnik indeksu (dom) aplikacji przez dodanie `<Counter />` elementu składnik indeksu.

   Jeśli używasz Blazor dla tego środowiska, składnik ankiety monitu (`<SurveyPrompt>` elementu) znajduje się w składnik indeksu. Zastąp `<SurveyPrompt>` element z `<Counter>` elementu.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* w Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. Ponownie skompiluj i uruchom aplikację. Strona główna ma swój własny licznika.

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą także mieć parametrów. Składnik parametry są definiowane za pomocą właściwości niepubliczne klasy składnika ozdobione `[Parameter]`. Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.

1. Aktualizowanie składnika `@functions` C# kodu:

   * Dodaj `IncrementAmount` ozdobione właściwość `[Parameter]` atrybutu.
   * Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* w Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu. Ustaw wartość do zwiększenia licznika dziesięciu.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* w Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. Załaduj ponownie stronę główną. Licznik rośnie przez dziesięć za każdym razem **kliknij mnie** przycisk jest zaznaczony. Licznik w przyrostach strony liczników za pomocą jednej.

## <a name="route-to-components"></a>Kierowanie do składników

`@page` Dyrektywę w górnej części *Counter.razor* pliku Określa, że ten składnik jest routingu punkt końcowy. Składnik licznika obsługuje żądania wysyłane do `/Counter`. Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal można używać składnika przez inne składniki.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Zarejestrowane w kontenerze usługi app Services są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). Wstrzyknięcie usług do składnika za pomocą `@inject` dyrektywy.

Sprawdź dyrektywy składnika FetchData w przykładowej aplikacji.

Przykładowa aplikacja składniki Razor `WeatherForecastService` usługi jest zarejestrowana jako [pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji. `@inject` Dyrektywy służy do dodania wystąpienia programu `WeatherForecastService` usługi do składnika.

*Components/Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Składnik FetchData używa usługi wprowadzonego jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

W przykładowej aplikacji w wersji Blazor `HttpClient` są wstrzykiwane można uzyskać prognozę pogody dane z *weather.json* w pliku *wwwroot/przykładowe data* folderu:

*Pages/FetchData.cshtml*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

W obu przykładowe aplikacje [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli danych o pogodzie:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Tworzenie listy zadań do wykonania

Dodawanie nowego składnika do aplikacji, która implementuje listy zadań do wykonania prostego.

1. Dodaj pusty plik do przykładowej aplikacji:

   * Dla składników Razor środowiska, Dodaj *Todo.razor* plik *składniki/strony* folderu.
   * Środowisko pracy Blazor, Dodaj *Todo.cshtml* plik *stron* folderu.

1. Podaj początkowe znaczniki dla składnika:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Dodaj składnik zadań do wykonania na pasku nawigacyjnym.

   Składnik NavMenu (*Components/Shared/NavMenu.razor* lub *Shared/NavMenu.cshtml* w Blazor) jest używana w układzie aplikacji. Układy są składniki, które pozwalają uniknąć duplikowania zawartości w aplikacji. Aby uzyskać więcej informacji, zobacz <xref:blazor/layouts>.

   Dodaj `<NavLink>` składnika Todo, dodając następujące znaczniki elementu listy poniżej istniejące elementy listy w *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* w Blazor) pliku:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Ponownie skompiluj i uruchom aplikację. Odwiedź nową stronę Todo, aby upewnić się, czy działa link do składnika zadań do wykonania.

1. Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasa, która reprezentuje element todo. Należy użyć następującego C# kod `TodoItem` klasy:

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. Wróć do części Todo (*Components/Pages/Todo.razor* lub *Pages/Todo.cshtml* w Blazor):

   * Dodaj pole do zadań do wykonania w `@functions` bloku. Składnik Todo to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.
   * Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadań do wykonania do listy. Dodaj przycisk poniżej listy i tekstowych danych wejściowych:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Ponownie skompiluj i uruchom aplikację. Gdy **dodawania zadań do wykonania** przycisk jest zaznaczony, nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest powiązaną przycisku.

1. Dodaj `AddTodo` metodę, aby składnik zadań do wykonania i zarejestruj go dla przycisku kliknie, za pomocą `onclick` atrybutu:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   `AddTodo` C# Metoda jest wywoływana po wybraniu przycisku.

1. Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` ciąg pola i powiązać wartość wprowadzania przy użyciu tekstu `bind` atrybutu:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy. Wyczyść wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Ponownie skompiluj i uruchom aplikację. Niektórych zadań do wykonania należy dodać do listy rzeczy do zrobienia, aby przetestować nowy kod.

1. Tekst tytułu dla każdego elementu todo zyski, można edytować i pola wyboru mogą pomóc użytkownikowi informacje o ukończonych elementów. Dodaj dane wejściowe pola wyboru dla każdego elementu todo i powiązać jej wartość na `IsDone` właściwości. Zmiana `@todo.Title` do `<input>` powiązany element `@todo.Title`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `<h1>` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie są kompletne (`IsDone` jest `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Ukończone składnika Todo (*Components/Pages/Todo.razor* lub *Pages/Todo.cshtml* w Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. Ponownie skompiluj i uruchom aplikację. Dodaj do testowania nowego kodu do wykonania.

## <a name="publish-and-deploy-the-app"></a>Publikowanie i wdrażanie aplikacji

Aby opublikować aplikację, zobacz <xref:host-and-deploy/blazor/index>.
