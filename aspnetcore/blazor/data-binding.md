---
title: ASP.NET Core Blazor powiązania danych
author: guardrex
description: Dowiedz się więcej na temat scenariuszy powiązań danych dla składników i elementów DOM w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: c38e6095d4e93d3eead10fa8bb0356b2113bb220
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453234"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="2cb6e-103">ASP.NET Core Blazor powiązania danych</span><span class="sxs-lookup"><span data-stu-id="2cb6e-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="2cb6e-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="2cb6e-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="2cb6e-105">Powiązanie danych zarówno ze składnikami, jak i elementami modelu DOM jest realizowane przy użyciu atrybutu [`@bind`](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="2cb6e-105">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="2cb6e-106">Poniższy przykład wiąże Właściwość `CurrentValue` z wartością pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-106">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="2cb6e-107">Gdy pole tekstowe utraci fokus, wartość właściwości jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="2cb6e-108">Pole tekstowe jest aktualizowane w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, a nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="2cb6e-109">Ponieważ składniki renderują się po wykonaniu kodu procedury obsługi zdarzeń, aktualizacje właściwości są *zwykle* odzwierciedlane w interfejsie użytkownika natychmiast po wyzwoleniu programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="2cb6e-110">Używanie `@bind` z właściwością `CurrentValue` (`<input @bind="CurrentValue" />`) jest zasadniczo równoważne z następującymi:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="2cb6e-111">Gdy składnik jest renderowany, `value` elementu wejściowego pochodzi z właściwości `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="2cb6e-112">Gdy użytkownik wpisze w polu tekstowym i zmieni fokus elementu, zdarzenie `onchange` jest wyzwalane, a właściwość `CurrentValue` jest ustawiona na wartość zmieniona.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="2cb6e-113">W rzeczywistości generowanie kodu jest bardziej skomplikowane, ponieważ `@bind` obsługuje przypadki, w których są wykonywane konwersje typów.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="2cb6e-114">W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia z atrybutem `value` i obsługuje zmiany przy użyciu zarejestrowanej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="2cb6e-115">Oprócz obsługi zdarzeń `onchange` ze składnią `@bind`, właściwość lub pole można powiązać przy użyciu innych zdarzeń, określając atrybut [`@bind-value`](xref:mvc/views/razor#bind) z `event` parametrem ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="2cb6e-115">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="2cb6e-116">Poniższy przykład wiąże `CurrentValue` właściwość dla zdarzenia `oninput`:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-116">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="2cb6e-117">W przeciwieństwie do `onchange`, które jest wyzwalane, gdy element utraci fokus, `oninput` uruchamiany, gdy wartość pola tekstowego ulegnie zmianie.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="2cb6e-118">`@bind-value` w powyższym przykładzie powiązania:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-118">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="2cb6e-119">Określone wyrażenie (`CurrentValue`) do atrybutu `value` elementu.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-119">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="2cb6e-120">Delegowanie zdarzenia zmiany do zdarzenia określonego przez `@bind-value:event`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-120">A change event delegate to the event specified by `@bind-value:event`.</span></span>

### <a name="unparsable-values"></a><span data-ttu-id="2cb6e-121">Wartości niemożliwy do przeanalizowania</span><span class="sxs-lookup"><span data-stu-id="2cb6e-121">Unparsable values</span></span>

<span data-ttu-id="2cb6e-122">Gdy użytkownik dostarczy wartość niemożliwy do przeanalizowania do elementu powiązanego z danymi, wartość niemożliwy do przeanalizowania jest automatycznie przywracana do poprzedniej wartości po wyzwoleniu zdarzenia bind.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-122">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="2cb6e-123">Poniżej przedstawiono przykładowy scenariusz:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-123">Consider the following scenario:</span></span>

* <span data-ttu-id="2cb6e-124">Element `<input>` jest powiązany z typem `int` z początkową wartością `123`:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-124">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="2cb6e-125">Użytkownik aktualizuje wartość elementu do `123.45` na stronie i zmienia fokus elementu.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-125">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="2cb6e-126">W poprzednim scenariuszu wartość elementu jest przywracana do `123`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-126">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="2cb6e-127">Gdy wartość `123.45` zostanie odrzucona na korzyść oryginalnej wartości `123`, użytkownik rozumie, że ich wartość nie została zaakceptowana.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-127">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="2cb6e-128">Domyślnie powiązanie dotyczy zdarzenia `onchange` elementu (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="2cb6e-128">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="2cb6e-129">Użyj `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`, aby ustawić inne zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-129">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="2cb6e-130">W przypadku zdarzenia `oninput` (`@bind-value:event="oninput"`) następuje rewersja po naciśnięciu klawisza, które wprowadza niemożliwy do przeanalizowania wartość.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-130">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="2cb6e-131">Podczas określania wartości docelowej zdarzenia `oninput` przy użyciu typu powiązanego z `int`użytkownik nie będzie wpisywać znaku `.`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-131">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="2cb6e-132">Znak `.` zostanie natychmiast usunięty, więc użytkownik otrzymuje natychmiastową opinię, że dozwolone są tylko liczby całkowite.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-132">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="2cb6e-133">Istnieją scenariusze, w których przywrócenie wartości w zdarzeniu `oninput` nie jest idealne, na przykład wtedy, gdy użytkownik powinien mieć możliwość wyczyszczenia wartości `<input>` niemożliwej do przeanalizowania.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-133">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="2cb6e-134">Alternatywy obejmują:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-134">Alternatives include:</span></span>

* <span data-ttu-id="2cb6e-135">Nie używaj zdarzenia `oninput`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-135">Don't use the `oninput` event.</span></span> <span data-ttu-id="2cb6e-136">Użyj domyślnego zdarzenia `onchange` (`@bind="{PROPERTY OR FIELD}"`), w którym niedozwolona wartość nie zostanie przywrócona, dopóki element nie utraci fokusu.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-136">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="2cb6e-137">Powiąż z typem dopuszczającym wartość null, takim jak `int?` lub `string`, i podaj logikę niestandardową do obsługi nieprawidłowych wpisów.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-137">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="2cb6e-138">Użyj [składnika walidacji formularza](xref:blazor/forms-validation), takiego jak `InputNumber` lub `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-138">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="2cb6e-139">Składniki walidacji formularza mają wbudowaną obsługę zarządzania nieprawidłowymi danymi wejściowymi.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-139">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="2cb6e-140">Składniki walidacji formularza:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-140">Form validation components:</span></span>
  * <span data-ttu-id="2cb6e-141">Zezwalaj użytkownikowi na dostarczenie nieprawidłowych danych wejściowych i otrzymywanie błędów walidacji w skojarzonym `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-141">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="2cb6e-142">Wyświetlaj błędy walidacji w interfejsie użytkownika bez zakłócania wprowadzania dodatkowych danych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-142">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

### <a name="format-strings"></a><span data-ttu-id="2cb6e-143">Ciągi formatujące</span><span class="sxs-lookup"><span data-stu-id="2cb6e-143">Format strings</span></span>

<span data-ttu-id="2cb6e-144">Powiązanie danych działa z ciągami formatu <xref:System.DateTime> przy użyciu [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="2cb6e-144">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="2cb6e-145">W tej chwili nie są dostępne inne wyrażenia formatu, takie jak formaty walutowe lub liczbowe.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-145">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="2cb6e-146">W powyższym kodzie, typ pola `<input>` elementu (`type`) domyślnie `text`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-146">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="2cb6e-147">`@bind:format` jest obsługiwana w celu powiązania następujących typów .NET:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-147">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="2cb6e-148"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="2cb6e-148"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="2cb6e-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="2cb6e-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="2cb6e-150">Atrybut `@bind:format` określa format daty, który ma zostać zastosowany do `value` elementu `<input>`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-150">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="2cb6e-151">Format jest również używany do analizowania wartości, gdy wystąpi zdarzenie `onchange`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-151">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="2cb6e-152">Określanie formatu dla typu pola `date` nie jest zalecane, ponieważ Blazor ma wbudowaną obsługę formatowania dat.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-152">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="2cb6e-153">Pomimo zalecenia, należy używać tylko formatu daty `yyyy-MM-dd`, aby powiązania działały prawidłowo, jeśli format jest dostarczany z typem pola `date`:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-153">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="2cb6e-154">Powiązanie element nadrzędny-to-Child z parametrami składnika</span><span class="sxs-lookup"><span data-stu-id="2cb6e-154">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="2cb6e-155">Powiązanie rozpoznaje parametry składnika, gdzie `@bind-{property}` może powiązać wartość właściwości z składnika nadrzędnego w dół ze składnikiem podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-155">Binding recognizes component parameters, where `@bind-{property}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="2cb6e-156">Powiązanie z elementu podrzędnego z elementem nadrzędnym jest omówione w [powiązaniu podrzędnie-to-Parent z częściowym powiązaniem powiązania](#child-to-parent-binding-with-chained-bind) .</span><span class="sxs-lookup"><span data-stu-id="2cb6e-156">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="2cb6e-157">Poniższy składnik podrzędny (`ChildComponent`) ma parametr składnika `Year` i wywołanie zwrotne `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-157">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="2cb6e-158">`EventCallback<T>` wyjaśniono w <xref:blazor/event-handling#eventcallback>.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-158">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="2cb6e-159">Poniższy składnik nadrzędny używa:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-159">The following parent component uses:</span></span>

* <span data-ttu-id="2cb6e-160">`ChildComponent` i wiąże parametr `ParentYear` z elementu nadrzędnego z parametrem `Year` w składniku podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-160">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="2cb6e-161">Zdarzenie `onclick` służy do wyzwalania metody `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-161">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="2cb6e-162">Aby uzyskać więcej informacji, zobacz <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-162">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="2cb6e-163">Załadowanie `ParentComponent` powoduje utworzenie następującej adjustacji:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-163">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="2cb6e-164">Jeśli wartość właściwości `ParentYear` zostanie zmieniona przez wybranie przycisku w `ParentComponent`, zostanie zaktualizowana właściwość `Year` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-164">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="2cb6e-165">Nowa wartość `Year` jest renderowana w interfejsie użytkownika podczas renderowania `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-165">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="2cb6e-166">Parametr `Year` jest możliwy do powiązania, ponieważ zawiera zdarzenie pomocnika `YearChanged` pasujące do typu parametru `Year`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-166">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="2cb6e-167">Zgodnie z Konwencją `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo równoważne zapisowi:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-167">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="2cb6e-168">Ogólnie rzecz biorąc, właściwość może być powiązana z odpowiednią obsługą zdarzeń przy użyciu atrybutu `@bind-property:event`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-168">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="2cb6e-169">Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` przy użyciu następujących dwóch atrybutów:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-169">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="2cb6e-170">Powiązanie elementu podrzędnego z elementem nadrzędnym z powiązaniem łańcuchowym</span><span class="sxs-lookup"><span data-stu-id="2cb6e-170">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="2cb6e-171">Typowy scenariusz polega na łańcuchu parametru powiązanego z danymi do elementu strony w danych wyjściowych składnika.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-171">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="2cb6e-172">Ten scenariusz jest nazywany *powiązaniem łańcuchowym* , ponieważ wiele poziomów powiązań występuje jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-172">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="2cb6e-173">Nie można zaimplementować powiązania łańcuchowego z składnią `@bind` w elemencie strony.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-173">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="2cb6e-174">Program obsługi zdarzeń i wartość muszą być określone osobno.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-174">The event handler and value must be specified separately.</span></span> <span data-ttu-id="2cb6e-175">Składnik nadrzędny, jednak może używać składni `@bind`ej z parametrem składnika.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-175">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="2cb6e-176">Następujący składnik `PasswordField` (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="2cb6e-176">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="2cb6e-177">Ustawia wartość elementu `<input>` na Właściwość `Password`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-177">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="2cb6e-178">Uwidacznia zmiany właściwości `Password` w składniku nadrzędnym z [EventCallback](xref:blazor/event-handling#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="2cb6e-178">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="2cb6e-179">Używa zdarzenia `onclick` służy do wyzwalania metody `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-179">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="2cb6e-180">Aby uzyskać więcej informacji, zobacz <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-180">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="2cb6e-181">Składnik `PasswordField` jest używany w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-181">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="2cb6e-182">Aby przeprowadzić sprawdzenia lub błędy pułapki dla hasła w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-182">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="2cb6e-183">Utwórz pole zapasowe dla `Password` (`_password` w poniższym przykładowym kodzie).</span><span class="sxs-lookup"><span data-stu-id="2cb6e-183">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="2cb6e-184">Wykonaj testy lub błędy pułapki w metodzie ustawiającej `Password`.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-184">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="2cb6e-185">Poniższy przykład przedstawia natychmiastową opinię dla użytkownika, jeśli w wartości hasła jest używana spacja:</span><span class="sxs-lookup"><span data-stu-id="2cb6e-185">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

### <a name="radio-buttons"></a><span data-ttu-id="2cb6e-186">Przyciski radiowe</span><span class="sxs-lookup"><span data-stu-id="2cb6e-186">Radio buttons</span></span>

<span data-ttu-id="2cb6e-187">Aby uzyskać informacje na temat tworzenia powiązań z przyciskami radiowymi w formularzu, zobacz <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="2cb6e-187">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
