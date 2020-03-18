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
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="1e0a5-103">Obsługa zdarzeń Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e0a5-103">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="1e0a5-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1e0a5-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="1e0a5-105">Składniki Razor zapewniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-105">Razor components provide event handling features.</span></span> <span data-ttu-id="1e0a5-106">Dla atrybutu elementu HTML o nazwie [`@on{EVENT}`](xref:mvc/views/razor#onevent) (na przykład `@onclick`) z wartością typu delegata składnik Razor traktuje wartość atrybutu jako procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-106">For an HTML element attribute named [`@on{EVENT}`](xref:mvc/views/razor#onevent) (for example, `@onclick`) with a delegate-typed value, a Razor component treats the attribute's value as an event handler.</span></span>

<span data-ttu-id="1e0a5-107">Poniższy kod wywołuje metodę `UpdateHeading`, gdy przycisk zostanie wybrany w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-107">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="1e0a5-108">Poniższy kod wywołuje metodę `CheckChanged`, gdy pole wyboru zostanie zmienione w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-108">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="1e0a5-109">Procedury obsługi zdarzeń mogą również być asynchroniczne i zwracać <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-109">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="1e0a5-110">Nie ma potrzeby ręcznego wywoływania [StateHasChanged](xref:blazor/lifecycle#state-changes).</span><span class="sxs-lookup"><span data-stu-id="1e0a5-110">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="1e0a5-111">Wyjątki są rejestrowane, gdy wystąpią.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-111">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="1e0a5-112">W poniższym przykładzie `UpdateHeading` jest wywoływana asynchronicznie po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-112">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

## <a name="event-argument-types"></a><span data-ttu-id="1e0a5-113">Typy argumentów zdarzeń</span><span class="sxs-lookup"><span data-stu-id="1e0a5-113">Event argument types</span></span>

<span data-ttu-id="1e0a5-114">W przypadku niektórych zdarzeń dozwolone są typy argumentów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-114">For some events, event argument types are permitted.</span></span> <span data-ttu-id="1e0a5-115">Określanie typu zdarzenia w wywołaniu metody jest konieczne tylko wtedy, gdy typ zdarzenia jest używany w metodzie.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-115">Specifying an event type in the method call is only necessary if the event type is used in the method.</span></span>

<span data-ttu-id="1e0a5-116">Obsługiwane `EventArgs` przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-116">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="1e0a5-117">Wydarzenie</span><span class="sxs-lookup"><span data-stu-id="1e0a5-117">Event</span></span>            | <span data-ttu-id="1e0a5-118">Klasa</span><span class="sxs-lookup"><span data-stu-id="1e0a5-118">Class</span></span>                | <span data-ttu-id="1e0a5-119">Zdarzenia i uwagi dotyczące modelu DOM</span><span class="sxs-lookup"><span data-stu-id="1e0a5-119">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="1e0a5-120">Schowek</span><span class="sxs-lookup"><span data-stu-id="1e0a5-120">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="1e0a5-121">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-121">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="1e0a5-122">Przeciągnij</span><span class="sxs-lookup"><span data-stu-id="1e0a5-122">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="1e0a5-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="1e0a5-124">`DataTransfer` i `DataTransferItem` przechowywać przeciągane dane elementu.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-124">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="1e0a5-125">Błąd</span><span class="sxs-lookup"><span data-stu-id="1e0a5-125">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="1e0a5-126">Wydarzenie</span><span class="sxs-lookup"><span data-stu-id="1e0a5-126">Event</span></span>            | `EventArgs`          | <span data-ttu-id="1e0a5-127">*Ogólne*</span><span class="sxs-lookup"><span data-stu-id="1e0a5-127">*General*</span></span><br><span data-ttu-id="1e0a5-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="1e0a5-129">*Schowek*</span><span class="sxs-lookup"><span data-stu-id="1e0a5-129">*Clipboard*</span></span><br><span data-ttu-id="1e0a5-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="1e0a5-131">*Dane wejściowe*</span><span class="sxs-lookup"><span data-stu-id="1e0a5-131">*Input*</span></span><br><span data-ttu-id="1e0a5-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart``onsubmit`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="1e0a5-133">*Multimedialny*</span><span class="sxs-lookup"><span data-stu-id="1e0a5-133">*Media*</span></span><br><span data-ttu-id="1e0a5-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="1e0a5-135">Fokus</span><span class="sxs-lookup"><span data-stu-id="1e0a5-135">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="1e0a5-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="1e0a5-137">Nie obejmuje obsługi `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-137">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="1e0a5-138">Dane wejściowe</span><span class="sxs-lookup"><span data-stu-id="1e0a5-138">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="1e0a5-139">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-139">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="1e0a5-140">Klawiatura</span><span class="sxs-lookup"><span data-stu-id="1e0a5-140">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="1e0a5-141">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-141">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="1e0a5-142">Wskaźnik</span><span class="sxs-lookup"><span data-stu-id="1e0a5-142">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="1e0a5-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="1e0a5-144">Wskaźnik myszy</span><span class="sxs-lookup"><span data-stu-id="1e0a5-144">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="1e0a5-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="1e0a5-146">Kółko myszy</span><span class="sxs-lookup"><span data-stu-id="1e0a5-146">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="1e0a5-147">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-147">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="1e0a5-148">Postęp</span><span class="sxs-lookup"><span data-stu-id="1e0a5-148">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="1e0a5-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress``ontimeout`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="1e0a5-150">Dotyk</span><span class="sxs-lookup"><span data-stu-id="1e0a5-150">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="1e0a5-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave``ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="1e0a5-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="1e0a5-152">`TouchPoint` reprezentuje pojedynczy punkt kontaktu na urządzeniu dotykowym.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-152">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="1e0a5-153">Więcej informacji zawierają następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-153">For more information, see the following resources:</span></span>

* <span data-ttu-id="1e0a5-154">[Klasy EventArgs w źródle odwołania ASP.NET Core (gałąź dotnet/aspnetcore Release/3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="1e0a5-154">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="1e0a5-155">[Powiadomienia MDN Web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; zawiera informacje o tym, które elementy HTML obsługują każde wydarzenie dom.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-155">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="1e0a5-156">Wyrażenia lambda</span><span class="sxs-lookup"><span data-stu-id="1e0a5-156">Lambda expressions</span></span>

<span data-ttu-id="1e0a5-157">[Wyrażenia lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) mogą być również używane:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-157">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="1e0a5-158">Często wygodnie jest blisko dodatkowych wartości, na przykład podczas iteracji na zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-158">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="1e0a5-159">Poniższy przykład tworzy trzy przyciski, z których każdy wywołuje `UpdateHeading` przekazywaniem argumentu zdarzenia (`MouseEventArgs`) i numerem przycisku (`buttonNumber`), po wybraniu w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-159">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="1e0a5-160">**Nie** używaj zmiennej loop (`i`) w pętli `for` bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-160">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="1e0a5-161">W przeciwnym razie ta sama zmienna jest używana przez wszystkie wyrażenia lambda, co sprawia, że wartość `i`być taka sama we wszystkich lambdach.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-161">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="1e0a5-162">Zawsze Przechwytuj wartość w zmiennej lokalnej (`buttonNumber` w poprzednim przykładzie), a następnie użyj jej.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-162">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="1e0a5-163">EventCallback</span><span class="sxs-lookup"><span data-stu-id="1e0a5-163">EventCallback</span></span>

<span data-ttu-id="1e0a5-164">Typowym scenariuszem ze składnikami zagnieżdżonymi jest potrzeba uruchomienia metody składnika nadrzędnego w przypadku wystąpienia zdarzenia podrzędnego składnika&mdash;na przykład gdy zdarzenie `onclick` wystąpi w elemencie podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-164">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="1e0a5-165">Aby uwidocznić zdarzenia między składnikami, użyj `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-165">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="1e0a5-166">Składnik nadrzędny może przypisać metodę wywołania zwrotnego do `EventCallback`składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-166">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="1e0a5-167">`ChildComponent` w przykładowej aplikacji (*Components/ChildComponent. Razor*) pokazuje, jak program obsługi `onclick` przycisku został skonfigurowany tak, aby otrzymać delegata `EventCallback` z `ParentComponent`próbki.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-167">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="1e0a5-168">`EventCallback` jest wpisana z `MouseEventArgs`, która jest odpowiednia dla zdarzenia `onclick` z urządzenia peryferyjnego:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-168">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="1e0a5-169">`ParentComponent` ustawia `EventCallback<T>` elementu podrzędnego (`OnClickCallback`) na jego metodę `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-169">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="1e0a5-170">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-170">*Pages/ParentComponent.razor*:</span></span>

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

<span data-ttu-id="1e0a5-171">Gdy przycisk zostanie wybrany w `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-171">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="1e0a5-172">Metoda `ShowMessage` `ParentComponent`jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-172">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="1e0a5-173">`_messageText` jest aktualizowany i wyświetlany w `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-173">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="1e0a5-174">Wywołanie [StateHasChanged](xref:blazor/lifecycle#state-changes) nie jest wymagane w metodzie wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="1e0a5-174">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="1e0a5-175">`StateHasChanged` jest automatycznie wywoływana w celu odrenderowania `ParentComponent`, podobnie jak zdarzenia podrzędne wyzwalają ponowne renderowanie składnika w obsłudze zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-175">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="1e0a5-176">`EventCallback` i `EventCallback<T>` Zezwalaj na asynchroniczne Delegaty.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-176">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="1e0a5-177">`EventCallback<T>` jest silnie wpisana i wymaga określonego typu argumentu.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-177">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="1e0a5-178">`EventCallback` jest słabo wpisywany i dopuszcza każdy typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-178">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="1e0a5-179">Wywołaj `EventCallback` lub `EventCallback<T>` z `InvokeAsync` i oczekują na <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-179">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="1e0a5-180">Użyj `EventCallback` i `EventCallback<T>` do obsługi zdarzeń i parametrów składnika powiązania.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-180">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="1e0a5-181">Preferuj silnie wpisaną `EventCallback<T>` przez `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-181">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="1e0a5-182">`EventCallback<T>` zapewnia lepszą opinię o błędach dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-182">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="1e0a5-183">Podobnie jak w przypadku innych programów obsługi zdarzeń interfejsu użytkownika, określenie parametru zdarzenia jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-183">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="1e0a5-184">Użyj `EventCallback`, gdy nie zostanie przeniesiona wartość do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-184">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="1e0a5-185">Zapobiegaj akcjom domyślnym</span><span class="sxs-lookup"><span data-stu-id="1e0a5-185">Prevent default actions</span></span>

<span data-ttu-id="1e0a5-186">Użyj atrybutu dyrektywy [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) , aby zapobiec domyślnej akcji dla zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-186">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="1e0a5-187">Po wybraniu klucza na urządzeniu wejściowym, gdy fokus elementu znajduje się w polu tekstowym, przeglądarka zwykle wyświetla znak klucza w polu tekstowym.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-187">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="1e0a5-188">W poniższym przykładzie zachowanie domyślne jest blokowane przez określenie atrybutu dyrektywy `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-188">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="1e0a5-189">Licznik przyrostu i klucz **+** nie są przechwytywane do wartości elementu `<input>`:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-189">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

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

<span data-ttu-id="1e0a5-190">Określanie atrybutu `@on{EVENT}:preventDefault` bez wartości jest równoważne z `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-190">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="1e0a5-191">Wartość atrybutu może również być wyrażeniem.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-191">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="1e0a5-192">W poniższym przykładzie `_shouldPreventDefault` jest polem `bool` ustawionym na `true` lub `false`:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-192">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="1e0a5-193">Procedura obsługi zdarzeń nie jest wymagana, aby zapobiec akcji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-193">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="1e0a5-194">Procedury obsługi zdarzeń i zapobiegania domyślnym scenariuszom akcji mogą być używane niezależnie.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-194">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="1e0a5-195">Zatrzymaj propagację zdarzeń</span><span class="sxs-lookup"><span data-stu-id="1e0a5-195">Stop event propagation</span></span>

<span data-ttu-id="1e0a5-196">Użyj atrybutu dyrektywy [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) , aby zatrzymać propagację zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="1e0a5-196">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="1e0a5-197">W poniższym przykładzie, zaznaczając pole wyboru, Zapobiegaj kliknięciu zdarzeń z drugiego elementu podrzędnego `<div>` od propagowania do `<div>`nadrzędnego:</span><span class="sxs-lookup"><span data-stu-id="1e0a5-197">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

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
