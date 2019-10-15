---
title: Tworzenie pierwszej aplikacji Blazor
author: guardrex
description: Tworzenie aplikacji Blazor krok po kroku.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 1c0492106b888665c23932f16cc83d22947956d2
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334050"
---
# <a name="build-your-first-blazor-app"></a>Tworzenie pierwszej aplikacji Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

W tym samouczku pokazano, jak skompilować i zmodyfikować aplikację Blazor.

Postępuj zgodnie ze wskazówkami w artykule <xref:blazor/get-started>, aby utworzyć projekt Blazor dla tego samouczka. Nazwij projekt *todolist*.

## <a name="build-components"></a>Składniki kompilacji

1. Przejdź do każdej z trzech stron aplikacji w folderze *Pages* : Home, Counter i Fetch Data. Te strony są implementowane przez pliki składników Razor *index. Razor*, *Counter. Razor*i *FetchData. Razor*.

1. Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale Blazor zapewnia lepsze podejście przy użyciu C#.

1. Sprawdzanie implementacji składnika `Counter` w pliku *Counter. Razor* .

   *Pages/Counter. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   Interfejs użytkownika składnika `Counter` jest definiowany przy użyciu języka HTML. Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor). Logika znaczników HTML C# i renderowania jest konwertowana na klasę składników w czasie kompilacji. Nazwa wygenerowanej klasy .NET jest zgodna z nazwą pliku.

   Elementy członkowskie klasy składnika są zdefiniowane w bloku `@code`. W bloku `@code` stan składnika (właściwości, pola) i metody są określone dla obsługi zdarzeń lub do definiowania innej logiki składnika. Te elementy członkowskie są następnie używane jako część logiki renderowania składnika i dla zdarzeń obsługi.

   Po wybraniu przycisku **kliknij do mnie** :

   * Zarejestrowano procedurę obsługi `onclick` składnika `Counter` (Metoda `IncrementCount`).
   * Składnik `Counter` generuje ponownie drzewo renderowania.
   * Nowe drzewo renderowania jest porównywane z poprzednią.
   * Stosowane są tylko modyfikacje Document Object Model (DOM). Wyświetlana liczba jest aktualizowana.

1. Zmodyfikuj C# logikę składnika `Counter`, aby zwiększyć liczbę o dwa zamiast jednego.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Skompiluj ponownie i uruchom aplikację, aby zobaczyć zmiany. Wybierz przycisk **kliknij mnie** . Licznik jest zwiększany o dwa.

## <a name="use-components"></a>Używanie składników

Uwzględnij składnik w innym składniku przy użyciu składni języka HTML.

1. Dodaj składnik `Counter` do składnika `Index` aplikacji, dodając element `<Counter />` do składnika `Index` (*index. Razor*).

   Jeśli używasz zestawu webBlazor dla tego środowiska, składnik `SurveyPrompt` jest używany przez składnik `Index`. Zastąp element `<SurveyPrompt>` elementem `<Counter />`. Jeśli używasz aplikacji serwera Blazor dla tego środowiska, Dodaj element `<Counter />` do składnika `Index`:

   *Pages/index. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Skompiluj ponownie i uruchom aplikację. Składnik `Index` ma swój własny licznik.

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą także mieć parametry. Parametry składnika są definiowane przy użyciu właściwości publicznych w klasie składnika z atrybutem `[Parameter]`. Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.

1. Zaktualizuj kod `@code` C# składnika:

   * Dodaj publiczną właściwość `IncrementAmount` z atrybutem `[Parameter]`.
   * Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.

   *Pages/Counter. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Określ parametr `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu. Ustaw wartość, aby zwiększyć licznik o dziesięć.

   *Pages/index. Razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. Załaduj ponownie składnik `Index`. Licznik jest zwiększany o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** . Licznik w składniku `Counter` kontynuuje zwiększanie o jeden.

## <a name="route-to-components"></a>Kierowanie do składników

Dyrektywa `@page` w górnej części pliku *Counter. Razor* określa, że składnik `Counter` jest punktem końcowym routingu. Składnik `Counter` obsługuje żądania wysyłane do `/counter`. Bez dyrektywy `@page` składnik nie obsługuje rozesłanych żądań, ale składnik nadal może być używany przez inne składniki.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

### <a name="blazor-server-experience"></a>Środowisko serwera Blazor

W przypadku korzystania z aplikacji serwera Blazor usługa `WeatherForecastService` jest zarejestrowana jako [Pojedyncza](xref:fundamentals/dependency-injection#service-lifetimes) w `Startup.ConfigureServices`. Wystąpienie usługi jest dostępne w całej aplikacji za pośrednictwem [iniekcji zależności (di)](xref:fundamentals/dependency-injection):

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

Dyrektywa `@inject` służy do wstrzykiwania wystąpienia usługi `WeatherForecastService` do składnika `FetchData`.

*Strony/FetchData. Razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Składnik `FetchData` używa wstrzykniętej usługi jako `ForecastService`, aby pobrać tablicę obiektów `WeatherForecast`:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a>Środowisko webassembly Blazor

W przypadku korzystania z aplikacji Blazor webassembly `HttpClient` jest wstrzykiwana w celu uzyskania danych prognozy pogody z pliku *Pogoda. JSON* w folderze *wwwroot/Sample-Data* .

*Strony/FetchData. Razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

Pętla [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) służy do renderowania każdego wystąpienia prognozy jako wiersza w tabeli danych o pogodzie:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Tworzenie listy zadań do wykonania

Dodaj do aplikacji nowy składnik implementujący prostą listę zadań do wykonania.

1. Dodaj pusty plik o nazwie do *zrobienia. Razor* do aplikacji w folderze *Pages* :

1. Podaj początkowe znaczniki dla składnika:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Dodaj składnik `Todo` na pasku nawigacyjnym.

   Składnik `NavMenu` (*Shared/NavMenu. Razor*) jest używany w układzie aplikacji. Układy są składnikami, które umożliwiają uniknięcie duplikowania zawartości w aplikacji.

   Dodaj element `<NavLink>` dla składnika `Todo`, dodając następujący znacznik elementu listy poniżej istniejących elementów listy w pliku *Shared/NavMenu. Razor* :

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Skompiluj ponownie i uruchom aplikację. Odwiedź stronę Nowa czynność, aby upewnić się, że łącze do składnika `Todo` działa.

1. Dodaj plik *TodoItem.cs* do katalogu głównego projektu, aby pomieścić klasę reprezentującą element do wykonania. Użyj następującego C# kodu dla klasy `TodoItem`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Wróć do składnika `Todo` (*strony/do zrobienia. Razor*):

   * Dodaj pole dla elementów do wykonania w bloku `@code`. Składnik `Todo` używa tego pola do obsługi stanu listy zadań do wykonania.
   * Dodaj znaczniki listy nieuporządkowane i pętlę `foreach`, aby renderować każdy element do wykonania jako element listy (`<li>`).

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Aplikacja wymaga elementów interfejsu użytkownika do dodawania do listy elementów do wykonania. Dodaj tekst wejściowy (`<input>`) i przycisk (`<button>`) poniżej listy nieuporządkowanej (`<ul>...</ul>`):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Skompiluj ponownie i uruchom aplikację. Po wybraniu przycisku **Dodaj do zrobienia** nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest w trybie przewodowym.

1. Dodaj metodę `AddTodo` do składnika `Todo` i zarejestruj ją dla opcji wyboru przy użyciu atrybutu `@onclick`. Metoda `AddTodo` C# jest wywoływana, gdy przycisk jest zaznaczony:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Aby uzyskać tytuł nowego elementu do wykonania, należy dodać pole ciągu `newTodo` w górnej części bloku `@code` i powiązać je z wartością wejściową tekstu przy użyciu atrybutu `bind` w elemencie `<input>`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. Zaktualizuj metodę `AddTodo`, aby dodać do listy `TodoItem` o określonym tytule. Wyczyść wartość pola wprowadzanie tekstu przez ustawienie `newTodo` do pustego ciągu:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Skompiluj ponownie i uruchom aplikację. Dodaj do listy zadań do zrobienia elementy do wykonania, aby przetestować nowy kod.

1. Tekst tytułu dla każdego elementu do wykonania można edytować, a pole wyboru może pomóc użytkownikowi śledzić elementy ukończone. Dodaj pole wyboru dla każdego elementu do wykonania i powiąż jego wartość z właściwością `IsDone`. Zmień `@todo.Title` na element `<input>` powiązany z `@todo.Title`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Aby sprawdzić, czy te wartości są powiązane, zaktualizuj nagłówek `<h1>`, aby pokazać liczbę elementów do wykonania, które nie zostały zakończone (`IsDone` jest `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Ukończony składnik `Todo` (*strony/zadanie. Razor*):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Skompiluj ponownie i uruchom aplikację. Dodaj elementy do wykonania, aby przetestować nowy kod.

> [!div class="nextstepaction"]
> <xref:blazor/components>
