---
title: Tworzenie pierwszej aplikacji Blazor
author: guardrex
description: Kompiluj aplikację Blazor krok po kroku.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/20/2020
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 138057c2ceb9ed01bdf958c01f5cf2275387df23
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989431"
---
# <a name="build-your-first-opno-locblazor-app"></a>Tworzenie pierwszej aplikacji Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

W tym samouczku pokazano, jak skompilować i zmodyfikować aplikację Blazor.

## <a name="build-components"></a>Składniki kompilacji

1. Postępuj zgodnie ze wskazówkami w artykule <xref:blazor/get-started>, aby utworzyć projekt Blazor dla tego samouczka. Nazwij projekt *todolist*.

1. Przejdź do każdej z trzech stron aplikacji w folderze *Pages* : Home, Counter i Fetch Data. Te strony są implementowane przez pliki składników Razor *index. Razor*, *Counter. Razor*i *FetchData. Razor*.

1. Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript. W przypadku Blazormożna pisać C# zamiast tego.

1. Sprawdzanie implementacji składnika `Counter` w pliku *Counter. Razor* .

   *Pages/Counter. Razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   Interfejs użytkownika składnika `Counter` jest definiowany przy użyciu języka HTML. Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor). Logika znaczników HTML C# i renderowania jest konwertowana na klasę składników w czasie kompilacji. Nazwa wygenerowanej klasy .NET jest zgodna z nazwą pliku.

   Elementy członkowskie klasy składnika są zdefiniowane w bloku `@code`. W bloku `@code`, stan składnika (właściwości, pola) i metody są określone dla obsługi zdarzeń lub do definiowania innej logiki składnika. Te elementy członkowskie są następnie używane jako część logiki renderowania składnika i dla zdarzeń obsługi.

   Po wybraniu przycisku **kliknij do mnie** :

   * Zarejestrowana procedura obsługi `onclick` składnika `Counter` jest wywoływana (Metoda `IncrementCount`).
   * Składnik `Counter` ponownie generuje drzewo renderowania.
   * Nowe drzewo renderowania jest porównywane z poprzednią.
   * Stosowane są tylko modyfikacje Document Object Model (DOM). Wyświetlana liczba jest aktualizowana.

1. Zmodyfikuj C# logikę składnika `Counter`, aby zwiększyć liczbę o dwa zamiast jednego.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Skompiluj ponownie i uruchom aplikację, aby zobaczyć zmiany. Wybierz przycisk **kliknij mnie** . Licznik jest zwiększany o dwa.

## <a name="use-components"></a>Używanie składników

Uwzględnij składnik w innym składniku przy użyciu składni języka HTML.

1. Dodaj składnik `Counter` do składnika `Index` aplikacji, dodając element `<Counter />` do składnika `Index` (*index. Razor*).

   Jeśli używasz Blazor webassembly dla tego środowiska, składnik `SurveyPrompt` jest używany przez składnik `Index`. Zastąp element `<SurveyPrompt>` elementem `<Counter />`. Jeśli używasz aplikacji serwera Blazor dla tego środowiska, Dodaj element `<Counter />` do składnika `Index`:

   *Pages/index. Razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Skompiluj ponownie i uruchom aplikację. Składnik `Index` ma swój własny licznik.

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą także mieć parametry. Parametry składnika są definiowane przy użyciu właściwości publicznych w klasie składnika z atrybutem `[Parameter]`. Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.

1. Zaktualizuj kod `@code` C# składnika w następujący sposób:

   * Dodaj publiczną właściwość `IncrementAmount` z atrybutem `[Parameter]`.
   * Zmień metodę `IncrementCount`, aby użyć właściwości `IncrementAmount` podczas zwiększania wartości `currentCount`.

   *Pages/Counter. Razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. Określ parametr `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu. Ustaw wartość, aby zwiększyć licznik o dziesięć.

   *Pages/index. Razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. Załaduj ponownie składnik `Index`. Licznik jest zwiększany o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** . Licznik w składniku `Counter` w dalszym ciągu zwiększa się o jeden.

## <a name="route-to-components"></a>Kierowanie do składników

Dyrektywa `@page` w górnej części pliku *Counter. Razor* określa, że składnik `Counter` jest punktem końcowym routingu. Składnik `Counter` obsługuje żądania wysyłane do `/counter`. Bez dyrektywy `@page` składnik nie obsługuje rozesłanych żądań, ale składnik nadal może być używany przez inne składniki.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

### <a name="opno-locblazor-server-experience"></a>środowisko serwera Blazor

W przypadku korzystania z aplikacji Blazor Server usługa `WeatherForecastService` zostanie zarejestrowana jako [Pojedyncza](xref:fundamentals/dependency-injection#service-lifetimes) w `Startup.ConfigureServices`. Wystąpienie usługi jest dostępne w całej aplikacji za pośrednictwem [iniekcji zależności (di)](xref:fundamentals/dependency-injection):

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

Dyrektywa `@inject` służy do wstrzykiwania wystąpienia usługi `WeatherForecastService` do składnika `FetchData`.

*Strony/FetchData. Razor*:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Składnik `FetchData` używa wstrzykniętej usługi jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a>Blazor środowisko webassembly

W przypadku pracy z aplikacją Blazor webassembly `HttpClient` jest wstrzykiwana w celu uzyskania danych prognozy pogody z pliku *Pogoda. JSON* w folderze *wwwroot/Sample-Data* .

*Strony/FetchData. Razor*:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

Pętla [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) jest używana do renderowania każdego wystąpienia prognozy jako wiersza w tabeli danych o pogodzie:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Tworzenie listy zadań do wykonania

Dodaj do aplikacji nowy składnik implementujący prostą listę zadań do wykonania.

1. Dodaj nowy składnik `Todo` Razor do aplikacji w folderze *Pages* . W programie Visual Studio kliknij prawym przyciskiem myszy folder **strony** i wybierz polecenie **Dodaj** > **nowy element** > **składnik Razor**. Nadaj nazwę plikowi do *zrobienia. Razor*. W innych środowiskach programistycznych Dodaj pusty plik do folderu **Pages** o nazwie do *zrobienia. Razor*.

1. Podaj początkowe znaczniki dla składnika:

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. Dodaj składnik `Todo` na pasku nawigacyjnym.

   Składnik `NavMenu` (*Shared/NavMenu. Razor*) jest używany w układzie aplikacji. Układy są składnikami, które umożliwiają uniknięcie duplikowania zawartości w aplikacji.

   Dodaj element `<NavLink>` dla składnika `Todo`, dodając następujący znacznik elementu listy poniżej istniejących elementów listy w pliku *Shared/NavMenu. Razor* :

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Skompiluj ponownie i uruchom aplikację. Odwiedź stronę Nowa czynność, aby upewnić się, że łącze do składnika `Todo` działa.

1. Dodaj plik *TodoItem.cs* do katalogu głównego projektu, aby pomieścić klasę reprezentującą element do wykonania. Użyj następującego C# kodu dla klasy `TodoItem`:

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Wróć do składnika `Todo` (*strony/do zrobienia. Razor*):

   * Dodaj pole dla elementów do wykonania w bloku `@code`. Składnik `Todo` używa tego pola do obsługi stanu listy zadań do wykonania.
   * Dodaj znaczniki listy nieuporządkowane i pętlę `foreach`, aby renderować każdy element do wykonania jako element listy (`<li>`).

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Aplikacja wymaga elementów interfejsu użytkownika do dodawania do listy elementów do wykonania. Dodaj tekst wejściowy (`<input>`) i przycisk (`<button>`) poniżej listy nieuporządkowanej (`<ul>...</ul>`):

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Skompiluj ponownie i uruchom aplikację. Po wybraniu przycisku **Dodaj do zrobienia** nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest w trybie przewodowym.

1. Dodaj metodę `AddTodo` do składnika `Todo` i zarejestruj ją w celu wybrania opcji przy użyciu atrybutu `@onclick`. Metoda `AddTodo` C# jest wywoływana, gdy przycisk jest zaznaczony:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Aby uzyskać tytuł nowego elementu do wykonania, należy dodać pole ciągu `newTodo` w górnej części bloku `@code` i powiązać je z wartością wejściową tekstu przy użyciu atrybutu `bind` w elemencie `<input>`:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. Zaktualizuj metodę `AddTodo`, aby dodać do listy `TodoItem` z określonym tytułem. Wyczyść wartość pola wejściowego tekstu przez ustawienie `newTodo` do pustego ciągu:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Skompiluj ponownie i uruchom aplikację. Dodaj do listy zadań do zrobienia elementy do wykonania, aby przetestować nowy kod.

1. Tekst tytułu dla każdego elementu do wykonania można edytować, a pole wyboru może pomóc użytkownikowi śledzić elementy ukończone. Dodaj pole wyboru dla każdego elementu do wykonania i powiąż jego wartość z właściwością `IsDone`. Zmień `@todo.Title` na element `<input>` powiązany z `@todo.Title`:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Aby sprawdzić, czy te wartości są powiązane, zaktualizuj nagłówek `<h3>`, aby pokazać liczbę elementów do wykonania, które nie zostały ukończone (`IsDone` jest `false`).

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. Ukończony składnik `Todo` (*strony/do zrobienia. Razor*):

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Skompiluj ponownie i uruchom aplikację. Dodaj elementy do wykonania, aby przetestować nowy kod.

> [!div class="nextstepaction"]
> <xref:blazor/components>
