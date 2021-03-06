---
title: Obsługa zdarzeń Blazor ASP.NET Core
author: guardrex
description: Dowiedz się więcej o funkcjach obsługi zdarzeń Blazor, takich jak typy argumentów zdarzeń, wywołania zwrotne zdarzeń i zarządzanie domyślnymi zdarzeniami przeglądarki.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/event-handling
ms.openlocfilehash: c144841805e07a136f153c25a78c7f9af7c5801b
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511369"
---
# <a name="aspnet-core-blazor-event-handling"></a>Obsługa zdarzeń Blazor ASP.NET Core

Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)

Składniki Razor zapewniają funkcje obsługi zdarzeń. Dla atrybutu elementu HTML o nazwie [`@on{EVENT}`](xref:mvc/views/razor#onevent) (na przykład `@onclick`) z wartością typu delegata składnik Razor traktuje wartość atrybutu jako procedurę obsługi zdarzeń.

Poniższy kod wywołuje metodę `UpdateHeading`, gdy przycisk zostanie wybrany w interfejsie użytkownika:

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

Poniższy kod wywołuje metodę `CheckChanged`, gdy pole wyboru zostanie zmienione w interfejsie użytkownika:

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

Procedury obsługi zdarzeń mogą również być asynchroniczne i zwracać <xref:System.Threading.Tasks.Task>. Nie ma potrzeby ręcznego wywoływania [StateHasChanged](xref:blazor/lifecycle#state-changes). Wyjątki są rejestrowane, gdy wystąpią.

W poniższym przykładzie `UpdateHeading` jest wywoływana asynchronicznie po wybraniu przycisku:

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

## <a name="event-argument-types"></a>Typy argumentów zdarzeń

W przypadku niektórych zdarzeń dozwolone są typy argumentów zdarzeń. Określanie typu zdarzenia w wywołaniu metody jest konieczne tylko wtedy, gdy typ zdarzenia jest używany w metodzie.

Obsługiwane `EventArgs` przedstawiono w poniższej tabeli.

| Wydarzenie            | Klasa                | Zdarzenia i uwagi dotyczące modelu DOM |
| ---------------- | -------------------- | -------------------- |
| Schowek        | `ClipboardEventArgs` | `oncut`, `oncopy`, `onpaste` |
| Przeciągnij             | `DragEventArgs`      | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`<br><br>`DataTransfer` i `DataTransferItem` przechowywać przeciągane dane elementu. |
| Błąd            | `ErrorEventArgs`     | `onerror` |
| Wydarzenie            | `EventArgs`          | *Ogólne*<br>`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`<br><br>*Schowek*<br>`onbeforecut`, `onbeforecopy`, `onbeforepaste`<br><br>*Dane wejściowe*<br>`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart``onsubmit`<br><br>*Multimedialny*<br>`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting` |
| Fokus            | `FocusEventArgs`     | `onfocus`, `onblur`, `onfocusin`, `onfocusout`<br><br>Nie obejmuje obsługi `relatedTarget`. |
| Dane wejściowe            | `ChangeEventArgs`    | `onchange`, `oninput` |
| Klawiatura         | `KeyboardEventArgs`  | `onkeydown`, `onkeypress`, `onkeyup` |
| Wskaźnik            | `MouseEventArgs`     | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` |
| Wskaźnik myszy    | `PointerEventArgs`   | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` |
| Kółko myszy      | `WheelEventArgs`     | `onwheel`, `onmousewheel` |
| Postęp         | `ProgressEventArgs`  | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress``ontimeout` |
| Dotyk            | `TouchEventArgs`     | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave``ontouchcancel`<br><br>`TouchPoint` reprezentuje pojedynczy punkt kontaktu na urządzeniu dotykowym. |

Więcej informacji zawierają następujące zasoby:

* [Klasy EventArgs w źródle odwołania ASP.NET Core (gałąź dotnet/aspnetcore Release/3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).
* [Powiadomienia MDN Web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; zawiera informacje o tym, które elementy HTML obsługują każde wydarzenie dom.

## <a name="lambda-expressions"></a>Wyrażenia lambda

[Wyrażenia lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) mogą być również używane:

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Często wygodnie jest blisko dodatkowych wartości, na przykład podczas iteracji na zestawie elementów. Poniższy przykład tworzy trzy przyciski, z których każdy wywołuje `UpdateHeading` przekazywaniem argumentu zdarzenia (`MouseEventArgs`) i numerem przycisku (`buttonNumber`), po wybraniu w interfejsie użytkownika:

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> **Nie** używaj zmiennej loop (`i`) w pętli `for` bezpośrednio w wyrażeniu lambda. W przeciwnym razie ta sama zmienna jest używana przez wszystkie wyrażenia lambda, co sprawia, że wartość `i`być taka sama we wszystkich lambdach. Zawsze Przechwytuj wartość w zmiennej lokalnej (`buttonNumber` w poprzednim przykładzie), a następnie użyj jej.

## <a name="eventcallback"></a>EventCallback

Typowym scenariuszem ze składnikami zagnieżdżonymi jest potrzeba uruchomienia metody składnika nadrzędnego w przypadku wystąpienia zdarzenia podrzędnego składnika&mdash;na przykład gdy zdarzenie `onclick` wystąpi w elemencie podrzędnym. Aby uwidocznić zdarzenia między składnikami, użyj `EventCallback`. Składnik nadrzędny może przypisać metodę wywołania zwrotnego do `EventCallback`składnika podrzędnego.

`ChildComponent` w przykładowej aplikacji (*Components/ChildComponent. Razor*) pokazuje, jak program obsługi `onclick` przycisku został skonfigurowany tak, aby otrzymać delegata `EventCallback` z `ParentComponent`próbki. `EventCallback` jest wpisana z `MouseEventArgs`, która jest odpowiednia dla zdarzenia `onclick` z urządzenia peryferyjnego:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

`ParentComponent` ustawia `EventCallback<T>` elementu podrzędnego (`OnClickCallback`) na jego metodę `ShowMessage`.

*Strony/ParentComponent. Razor*:

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

Gdy przycisk zostanie wybrany w `ChildComponent`:

* Metoda `ShowMessage` `ParentComponent`jest wywoływana. `_messageText` jest aktualizowany i wyświetlany w `ParentComponent`.
* Wywołanie [StateHasChanged](xref:blazor/lifecycle#state-changes) nie jest wymagane w metodzie wywołania zwrotnego (`ShowMessage`). `StateHasChanged` jest automatycznie wywoływana w celu odrenderowania `ParentComponent`, podobnie jak zdarzenia podrzędne wyzwalają ponowne renderowanie składnika w obsłudze zdarzeń, które są wykonywane w ramach elementu podrzędnego.

`EventCallback` i `EventCallback<T>` Zezwalaj na asynchroniczne Delegaty. `EventCallback<T>` jest silnie wpisana i wymaga określonego typu argumentu. `EventCallback` jest słabo wpisywany i dopuszcza każdy typ argumentu.

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

Wywołaj `EventCallback` lub `EventCallback<T>` z `InvokeAsync` i oczekują na <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Użyj `EventCallback` i `EventCallback<T>` do obsługi zdarzeń i parametrów składnika powiązania.

Preferuj silnie wpisaną `EventCallback<T>` przez `EventCallback`. `EventCallback<T>` zapewnia lepszą opinię o błędach dla użytkowników składnika. Podobnie jak w przypadku innych programów obsługi zdarzeń interfejsu użytkownika, określenie parametru zdarzenia jest opcjonalne. Użyj `EventCallback`, gdy nie zostanie przeniesiona wartość do wywołania zwrotnego.

## <a name="prevent-default-actions"></a>Zapobiegaj akcjom domyślnym

Użyj atrybutu dyrektywy [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) , aby zapobiec domyślnej akcji dla zdarzenia.

Po wybraniu klucza na urządzeniu wejściowym, gdy fokus elementu znajduje się w polu tekstowym, przeglądarka zwykle wyświetla znak klucza w polu tekstowym. W poniższym przykładzie zachowanie domyślne jest blokowane przez określenie atrybutu dyrektywy `@onkeypress:preventDefault`. Licznik przyrostu i klucz **+** nie są przechwytywane do wartości elementu `<input>`:

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

Określanie atrybutu `@on{EVENT}:preventDefault` bez wartości jest równoważne z `@on{EVENT}:preventDefault="true"`.

Wartość atrybutu może również być wyrażeniem. W poniższym przykładzie `_shouldPreventDefault` jest polem `bool` ustawionym na `true` lub `false`:

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

Procedura obsługi zdarzeń nie jest wymagana, aby zapobiec akcji domyślnej. Procedury obsługi zdarzeń i zapobiegania domyślnym scenariuszom akcji mogą być używane niezależnie.

## <a name="stop-event-propagation"></a>Zatrzymaj propagację zdarzeń

Użyj atrybutu dyrektywy [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) , aby zatrzymać propagację zdarzeń.

W poniższym przykładzie, zaznaczając pole wyboru, Zapobiegaj kliknięciu zdarzeń z drugiego elementu podrzędnego `<div>` od propagowania do `<div>`nadrzędnego:

```razor
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```
