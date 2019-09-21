---
title: Tworzenie pierwszej aplikacji Blazor
author: guardrex
description: Tworzenie aplikacji Blazor krok po kroku.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 10feb5467a6a6b5a43e0df739fa72902af9854da
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168356"
---
# <a name="build-your-first-blazor-app"></a>Tworzenie pierwszej aplikacji Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

W tym samouczku pokazano, jak skompilować i zmodyfikować aplikację Blazor.

Postępuj zgodnie ze wskazówkami zawartymi w <xref:blazor/get-started> artykule, aby utworzyć projekt Blazor dla tego samouczka. Nazwij projekt *todolist*.

## <a name="build-components"></a>Składniki kompilacji

1. Przejdź do każdej z trzech stron aplikacji w folderze *strony* : Dane na stronie głównej, licznika i pobierania. Te strony są implementowane przez pliki składników Razor *index. Razor*, *Counter. Razor*i *FetchData. Razor*.

1. Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale Blazor zapewnia lepsze podejście przy użyciu C#.

1. Sprawdzanie implementacji `Counter` składnika w pliku *Counter. Razor* .

   *Pages/Counter. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   Interfejs użytkownika `Counter` składnika jest definiowany przy użyciu języka HTML. Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor). Logika znaczników HTML C# i renderowania jest konwertowana na klasę składników w czasie kompilacji. Nazwa wygenerowanej klasy .NET jest zgodna z nazwą pliku.

   Elementy członkowskie klasy składnika są zdefiniowane w `@code` bloku. `@code` W bloku, stan składnika (właściwości, pola) i metody są określone dla obsługi zdarzeń lub do definiowania innej logiki składnika. Te elementy członkowskie są następnie używane jako część logiki renderowania składnika i dla zdarzeń obsługi.

   Po wybraniu przycisku **kliknij do mnie** :

   * Zarejestrowana`onclick` procedura`IncrementCount` obsługi jest wywoływana (Metoda). `Counter`
   * `Counter` Składnik ponownie generuje drzewo renderowania.
   * Nowe drzewo renderowania jest porównywane z poprzednią.
   * Stosowane są tylko modyfikacje Document Object Model (DOM). Wyświetlana liczba jest aktualizowana.

1. Zmodyfikuj C# logikę `Counter` składnika, aby zwiększyć liczbę o dwa zamiast jednego.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Skompiluj ponownie i uruchom aplikację, aby zobaczyć zmiany. Wybierz przycisk **kliknij mnie** . Licznik jest zwiększany o dwa.

## <a name="use-components"></a>Używanie składników

Uwzględnij składnik w innym składniku przy użyciu składni języka HTML.

1. `Index` `<Counter />` Dodaj składnik do składnika aplikacji ,`Index` dodając element do składnika (*index. Razor*). `Counter`

   Jeśli używasz zestawu webBlazor dla tego środowiska, `SurveyPrompt` składnik jest używany `Index` przez składnik. Zastąp `<Counter />`elementelementem. `<SurveyPrompt>` Jeśli używasz aplikacji serwera Blazor dla tego środowiska, Dodaj `<Counter />` element `Index` do składnika:

   *Pages/index. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Skompiluj ponownie i uruchom aplikację. `Index` Składnik ma swój własny licznik.

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą także mieć parametry. Parametry składnika są definiowane przy użyciu właściwości publicznych w klasie składnika z `[Parameter]` atrybutem. Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.

1. Zaktualizuj `@code` C# kod składnika:

   * Dodaj publiczną `IncrementAmount` Właściwość `[Parameter]` z atrybutem.
   * Zmień metodę, aby `IncrementAmount` użyć `currentCount`podczas zwiększania wartości. `IncrementCount`

   *Pages/Counter. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Określ parametr w elemencie`<Counter>`składnikaprzyużyciuatrybutu. `Index` `IncrementAmount` Ustaw wartość, aby zwiększyć licznik o dziesięć.

   *Pages/index. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. Załaduj `Index` ponownie składnik. Licznik jest zwiększany o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** . Licznik w `Counter` składniku kontynuuje zwiększanie o jeden.

## <a name="route-to-components"></a>Kierowanie do składników

Dyrektywa w górnej części `Counter` pliku *Counter. Razor* określa, że składnik jest punktem końcowym routingu. `@page` Składnik obsługuje żądania wysyłane do `/counter`. `Counter` `@page` Bez dyrektywy składnik nie obsługuje rozesłanych żądań, ale składnik nadal może być używany przez inne składniki.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

`WeatherForecastService` W`Startup.ConfigureServices`przypadku korzystania z aplikacji serwera Blazor usługa jest rejestrowana jako [Pojedyncza](xref:fundamentals/dependency-injection#service-lifetimes) . Wystąpienie usługi jest dostępne w całej aplikacji za pośrednictwem [iniekcji zależności (di)](xref:fundamentals/dependency-injection):

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

Dyrektywa służy do wstrzykiwania wystąpienia `WeatherForecastService` usługi do `FetchData` składnika. `@inject`

*Strony/FetchData. Razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Składnik używa wstrzykniętej usługi jako `ForecastService`, `WeatherForecast` aby pobrać tablicę obiektów: `FetchData`

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

W przypadku pracy z aplikacją `HttpClient` Blazor webassembly wprowadza się w celu uzyskania danych prognozy pogody z pliku *Pogoda. JSON* w folderze *wwwroot/Sample-Data* .

*Strony/FetchData. Razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

Pętla foreach jest używana do renderowania każdego wystąpienia prognozy jako wiersza w tabeli danych o pogodzie: [ \@](/dotnet/csharp/language-reference/keywords/foreach-in)

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Tworzenie listy zadań do wykonania

Dodaj do aplikacji nowy składnik implementujący prostą listę zadań do wykonania.

1. Dodaj pusty plik o nazwie do *zrobienia. Razor* do aplikacji w folderze *Pages* :

1. Podaj początkowe znaczniki dla składnika:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. `Todo` Dodaj składnik do paska nawigacyjnego.

   Składnik (*Shared/NavMenu. Razor*) jest używany w układzie aplikacji. `NavMenu` Układy są składnikami, które umożliwiają uniknięcie duplikowania zawartości w aplikacji.

   Dodaj element dla składnika, dodając następujący znacznik elementu listy poniżej istniejących elementów listy w pliku *Shared/NavMenu. Razor:* `Todo` `<NavLink>`

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Skompiluj ponownie i uruchom aplikację. Odwiedź stronę nowych zadań do wykonania, aby upewnić się, `Todo` że łącze do składnika działa.

1. Dodaj plik *TodoItem.cs* do katalogu głównego projektu, aby pomieścić klasę reprezentującą element do wykonania. Użyj następującego C# kodu dla `TodoItem` klasy:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Wróć do `Todo` składnika (*strony/do zrobienia. Razor*):

   * Dodaj pole do zadań do wykonania w `@code` bloku. `Todo` Składnik używa tego pola do obsługi stanu listy zadań do wykonania.
   * Dodaj znaczniki listy nieuporządkowane i `foreach` pętlę, aby renderować każdy element do wykonania jako element listy (`<li>`).

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Aplikacja wymaga elementów interfejsu użytkownika do dodawania do listy elementów do wykonania. Dodaj tekst wejściowy (`<input>`) i przycisk (`<button>`) poniżej listy nieuporządkowanej (`<ul>...</ul>`):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Skompiluj ponownie i uruchom aplikację. Po wybraniu przycisku **Dodaj do zrobienia** nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest w trybie przewodowym.

1. Dodaj metodę do składnika i zarejestruj ją `@onclick` w celu wybrania opcji przy użyciu atrybutu. `Todo` `AddTodo` C# wywoływana, gdy przycisk jest zaznaczony `AddTodo` :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Aby uzyskać tytuł nowego elementu do wykonania, należy `newTodo` dodać pole ciągu u góry `@code` bloku i powiązać je z wartością tekstu wejściowego `<input>` przy użyciu `bind` atrybutu w elemencie:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. Zaktualizuj metodę, aby dodać z określonym tytułem do listy. `TodoItem` `AddTodo` Wyczyść wartość pola wprowadzanie tekstu przez ustawienie `newTodo` do pustego ciągu:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Skompiluj ponownie i uruchom aplikację. Dodaj do listy zadań do zrobienia elementy do wykonania, aby przetestować nowy kod.

1. Tekst tytułu dla każdego elementu do wykonania można edytować, a pole wyboru może pomóc użytkownikowi śledzić elementy ukończone. Dodaj pole wyboru dla każdego elementu do wykonania i powiąż jego wartość z `IsDone` właściwością. Zmień `@todo.Title` `@todo.Title`na elementpowiązanyz:`<input>`

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Aby sprawdzić, czy te wartości są powiązane, zaktualizuj `<h1>` nagłówek, aby pokazać liczbę elementów do wykonania, które nie zostały zakończone (`IsDone` is `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Ukończony `Todo` składnik (*strony/do zrobienia. Razor*):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Skompiluj ponownie i uruchom aplikację. Dodaj elementy do wykonania, aby przetestować nowy kod.

> [!div class="nextstepaction"]
> <xref:blazor/components>
