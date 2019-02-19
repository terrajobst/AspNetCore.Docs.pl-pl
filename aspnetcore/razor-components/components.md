---
title: Tworzenie i używanie składników Razor
author: guardrex
description: Informacje o sposobie tworzenia i używania składników Razor, w tym jak powiązać z danymi, obsługa zdarzeń i Zarządzaj cyklami życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: razor-components/components
ms.openlocfilehash: d1e8075ff1ac6695c98973e6abd8480a78761d92
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410424"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="fb1bf-103">Tworzenie i używanie składników Razor</span><span class="sxs-lookup"><span data-stu-id="fb1bf-103">Create and use Razor Components</span></span>

<span data-ttu-id="fb1bf-104">Przez [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), i [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="fb1bf-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="fb1bf-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="fb1bf-106">Zobacz [wprowadzenie](xref:razor-components/get-started) tematu wstępnie wymaganych składników.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="fb1bf-107">Razor składniki aplikacji są tworzone przy użyciu *składniki*.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="fb1bf-108">Składnik jest niezależna fragmentu interfejsu użytkownika (UI), takich jak strony, okno dialogowe lub formularza.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="fb1bf-109">Składnik zawiera kod znaczników HTML i logika przetwarzania wymagane w celu wstrzyknięcia danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-109">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="fb1bf-110">Składniki są elastyczne i uproszczone.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-110">Components are flexible and lightweight.</span></span> <span data-ttu-id="fb1bf-111">Mogą być zagnieżdżone, ponownie i współużytkowane między projektami.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-111">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="fb1bf-112">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="fb1bf-112">Component classes</span></span>

<span data-ttu-id="fb1bf-113">Składniki są zazwyczaj implementowani w *.cshtml* plików przy użyciu kombinacji C# i kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-113">Components are typically implemented in *.cshtml* files using a combination of C# and HTML markup.</span></span> <span data-ttu-id="fb1bf-114">W interfejsie użytkownika dla składnika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-114">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="fb1bf-115">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-115">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="fb1bf-116">Podczas kompilowania aplikacji Razor składników, kod znaczników HTML i C# logiki renderowania są konwertowane na klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-116">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="fb1bf-117">Nazwa wygenerowanej klasy odpowiada nazwie pliku.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-117">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="fb1bf-118">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku (więcej niż jeden `@functions` bloku jest dozwolone).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-118">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="fb1bf-119">W `@functions` bloku, stan składników (właściwości, pola) została określona wraz z metody obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-119">In the `@functions` block, component state (properties, fields) is specified along with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="fb1bf-120">Elementy członkowskie składnika mogą posłużyć jako część składnika przez renderowanie przy użyciu logiki C# wyrażeń, które zaczyna się `@`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-120">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="fb1bf-121">Na przykład C# pole jest renderowane przez dodanie przedrostka `@` na nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-121">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="fb1bf-122">Poniższy przykład daje w wyniku i renderuje:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-122">The following example evaluates and renders:</span></span>

* <span data-ttu-id="fb1bf-123">`_headingFontStyle` wartości właściwości CSS dla `font-style`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-123">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="fb1bf-124">`_headingText` w treści `<h1>` elementu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-124">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="fb1bf-125">Po początkowo renderowania składnik składnika generuje jej drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-125">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="fb1bf-126">Składniki razor następnie porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszystkie zmiany do przeglądarki w modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-126">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="using-components"></a><span data-ttu-id="fb1bf-127">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="fb1bf-127">Using components</span></span>

<span data-ttu-id="fb1bf-128">Składniki mogą zawierać inne składniki, deklarując je przy użyciu składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-128">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="fb1bf-129">Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-129">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="fb1bf-130">Renderuje następujące znaczniki `HeadingComponent` (*HeadingComponent.cshtml*) wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-130">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a><span data-ttu-id="fb1bf-131">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="fb1bf-131">Component parameters</span></span>

<span data-ttu-id="fb1bf-132">Składniki mogą mieć *Parametry składnika*, które są definiowane za pomocą *niepublicznych* ozdobione właściwości klasy składnika `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-132">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="fb1bf-133">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-133">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="fb1bf-134">W poniższym przykładzie `ParentComponent` ustawia wartość `Title` właściwość `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-134">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="fb1bf-135">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-135">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

<span data-ttu-id="fb1bf-136">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-136">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="fb1bf-137">Zawartość elementu podrzędnego</span><span class="sxs-lookup"><span data-stu-id="fb1bf-137">Child content</span></span>

<span data-ttu-id="fb1bf-138">Składniki można ustawić zawartości innego składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-138">Components can set the content of another component.</span></span> <span data-ttu-id="fb1bf-139">Przypisywanie składnik udostępnia zawartości między tagami, które określają odbieranie składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-139">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="fb1bf-140">Na przykład `ParentComponent` może zapewnić zawartości dla renderowania przez składnik podrzędnych przez umieszczenie zawartość wewnątrz `<ChildComponent>` tagów.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-140">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="fb1bf-141">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-141">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

<span data-ttu-id="fb1bf-142">Składnik podrzędny ma `ChildContent` właściwość, która reprezentuje `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-142">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="fb1bf-143">Wartość `ChildContent` jest umieszczony w znaczniku elementu podrzędnego, której zawartość ma być renderowany.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-143">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="fb1bf-144">W poniższym przykładzie wartość `ChildContent` jest otrzymane od składnika nadrzędnego i renderowania wewnątrz panelu Bootstrap `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-144">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="fb1bf-145">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-145">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="fb1bf-146">Odbieranie właściwość `RenderFragment` zawartość, musi nosić `ChildContent` przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-146">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="fb1bf-147">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="fb1bf-147">Data binding</span></span>

<span data-ttu-id="fb1bf-148">Powiązanie danych do składników i elementów DOM odbywa się za pomocą `bind` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-148">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="fb1bf-149">Poniższy przykład tworzy powiązanie `ItalicsCheck` właściwość z polem wyboru zaznaczone stanu:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-149">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck" />
```

<span data-ttu-id="fb1bf-150">Gdy pole wyboru jest zaznaczone, a następnie wyczyszczone, wartość właściwości jest aktualizowany do `true` i `false`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-150">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="fb1bf-151">Pole wyboru jest aktualizowana w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-151">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="fb1bf-152">Ponieważ składniki renderować się po wykonaniu kod procedury obsługi zdarzeń, aktualizacje właściwości są zazwyczaj odzwierciedlane w interfejsie użytkownika od razu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-152">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="fb1bf-153">Za pomocą `bind` z `CurrentValue` właściwości (`<input bind="@CurrentValue" />`) jest zasadniczo odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-153">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="fb1bf-154">Po wyrenderowaniu składnika `value` danego elementu wejściowego pochodzi z `CurrentValue` właściwości.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-154">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="fb1bf-155">Gdy użytkownik wpisuje w polu tekstowym `onchange` zdarzenie jest generowane i `CurrentValue` zostaje ustalona zmieniona wartość.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-155">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="fb1bf-156">W rzeczywistości generowania kodu jest nieco bardziej złożone, ponieważ `bind` obsługuje kilka przypadków, w którym konwersje są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-156">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="fb1bf-157">W zasadzie `bind` kojarzy bieżącą wartość wyrażenia z `value` atrybutu i uchwytów zmiany przy użyciu zarejestrowanego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-157">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="fb1bf-158">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="fb1bf-158">**Format strings**</span></span>

<span data-ttu-id="fb1bf-159">Powiązanie danych w programach <xref:System.DateTime> ciągi formatujące.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-159">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="fb1bf-160">Inne wyrażenia formatu, takie jak waluta lub formaty liczbowe nie są dostępne w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-160">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="fb1bf-161">`format-value` Atrybut określa format daty do zastosowania do `value` z `input` elementu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-161">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="fb1bf-162">Format umożliwia również przeanalizować wartość po `onchange` wystąpi zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-162">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="fb1bf-163">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="fb1bf-163">**Component parameters**</span></span>

<span data-ttu-id="fb1bf-164">Powiązanie rozpoznaje również Parametry składnika, gdzie `bind-{property}` można powiązać wartości właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-164">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="fb1bf-165">Używa następującego składnika `ChildComponent` i wiąże `ParentYear` parametru z obiektu `Year` parametru w składniku podrzędne:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-165">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="fb1bf-166">Składnik nadrzędny:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-166">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="fb1bf-167">Element podrzędny:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-167">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

<span data-ttu-id="fb1bf-168">`Year` Parametr jest możliwej do wiązania, ponieważ ma ona towarzyszące `YearChanged` zdarzeń, który jest zgodny z typem `Year` parametru.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-168">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="fb1bf-169">Trwa ładowanie `ParentComponent` tworzy następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-169">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="fb1bf-170">Jeśli wartość `ParentYear` zmianie właściwości, wybierając przycisk w `ParentComponent`, `Year` właściwość `ChildComponent` jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-170">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="fb1bf-171">Nowa wartość `Year` jest wyświetlana w Interfejsie użytkownika po `ParentComponent` jest rerendered:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-171">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

## <a name="event-handling"></a><span data-ttu-id="fb1bf-172">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="fb1bf-172">Event handling</span></span>

<span data-ttu-id="fb1bf-173">Składniki razor udostępniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-173">Razor Components provide event handling features.</span></span> <span data-ttu-id="fb1bf-174">Atrybut elementu HTML o nazwie `on<event>` (na przykład `onclick`, `onsubmit`) z wartością wpisane delegata składniki Razor traktuje wartość atrybutu jako program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-174">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="fb1bf-175">Nazwa atrybutu zawsze zaczyna się od `on`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-175">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="fb1bf-176">Poniższy kod wywoła `UpdateHeading` metodę po wybraniu przycisku w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-176">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="fb1bf-177">Poniższy kod wywoła `CheckboxChanged` metody, gdy pole wyboru jest zmieniana w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-177">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="fb1bf-178">Programy obsługi zdarzeń można też asynchronicznego i zwracają <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-178">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="fb1bf-179">Nie ma konieczności ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-179">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="fb1bf-180">Wyjątki są rejestrowane w momencie ich wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-180">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="fb1bf-181">Niektóre zdarzenia są dozwolone typy argumentów zdarzenia określonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-181">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="fb1bf-182">Jeśli dostęp do jednego z następujących typów zdarzeń nie jest to konieczne, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-182">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="fb1bf-183">Lista argumentów zdarzeń obsługiwanych jest:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-183">The list of supported event arguments is:</span></span>

* <span data-ttu-id="fb1bf-184">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="fb1bf-184">UIEventArgs</span></span>
* <span data-ttu-id="fb1bf-185">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="fb1bf-185">UIChangeEventArgs</span></span>
* <span data-ttu-id="fb1bf-186">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="fb1bf-186">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="fb1bf-187">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="fb1bf-187">UIMouseEventArgs</span></span>

<span data-ttu-id="fb1bf-188">Wyrażenia lambda może również służyć:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-188">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="fb1bf-189">Często jest to wygodne zamknąć dodatkowe wartości, takich jak podczas iteracji w zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-189">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="fb1bf-190">Poniższy przykład tworzy trzy przyciski wszystkich, która wywołuje metodę `UpdateHeading` przekazywaniem argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`) w przypadku wybrania w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-190">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary" 
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    string message = "Select a button to learn its position.";

    void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="fb1bf-191">Przechwytywanie odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="fb1bf-191">Capture references to components</span></span>

<span data-ttu-id="fb1bf-192">Składnik odwołuje się do zapewnienia get sposób odwołania do wystąpienia składnika, dzięki czemu można wysyłać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-192">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="fb1bf-193">Do przechwycenia odwołania do składników, należy dodać `ref` atrybutu do elementu podrzędnego, a następnie zdefiniować pole o tej samej nazwie i typie jako element podrzędny.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-193">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    MyLoginDialog loginDialog;

    void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="fb1bf-194">Po wyrenderowaniu składnika `loginDialog` pole jest wypełniane `MyLoginDialog` wystąpienie składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-194">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="fb1bf-195">Następnie można wywoływać metod .NET w instancji składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-195">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb1bf-196">`loginDialog` Zmiennej tylko jest wypełniana po składnik jest renderowana i jego dane wyjściowe obejmują `MyLoginDialog` elementu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-196">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="fb1bf-197">Do tego momentu nie ma nic do odwołania.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-197">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="fb1bf-198">Do manipulowania odwołania do składników, po zakończeniu renderowania składnik, należy użyć `OnAfterRenderAsync` lub `OnAfterRender` metody.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-198">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="fb1bf-199">Podczas gdy przechwytywania odwołania do składników używa składni podobnie [przechwytywania odwołania do elementu](xref:razor-components/javascript-interop#capture-references-to-elements), nie jest [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop) funkcji.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-199">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="fb1bf-200">Odwołania do składników nie są przekazywane do kodu JavaScript; są one używane tylko w kodzie .NET.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-200">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="fb1bf-201">Czy **nie** umożliwia odwołania do składników mutować stan składnikach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-201">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="fb1bf-202">Zamiast tego należy użyć normalnego parametry deklaratywne, aby przekazać dane do elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-202">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="fb1bf-203">To sprawia, że podrzędnych automatycznie rerender w właściwym czasie.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-203">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="fb1bf-204">Cykl życia metody</span><span class="sxs-lookup"><span data-stu-id="fb1bf-204">Lifecycle methods</span></span>

<span data-ttu-id="fb1bf-205">`OnInitAsync` i `OnInit` wykonanie kodu, aby zainicjować składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-205">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="fb1bf-206">Aby wykonać operację asynchroniczną, użyj `OnInitAsync` i `await` — słowo kluczowe o nieudanej operacji:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-206">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="fb1bf-207">Operacja synchroniczna, można użyć `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-207">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="fb1bf-208">`OnParametersSetAsync` i `OnParametersSet` są wywoływane, gdy składnik, który otrzyma parametry od jego elementu nadrzędnego, a wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-208">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="fb1bf-209">Te metody są wykonywane po zainicjowaniu składnik, a następnie każdorazowo składnika renderowania:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-209">These methods are executed after component initialization and then each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="fb1bf-210">`OnAfterRenderAsync` i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-210">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="fb1bf-211">Odwołania do elementu, jak i składnika zostaną wypełnione na tym etapie.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-211">Element and component references are populated at this point.</span></span> <span data-ttu-id="fb1bf-212">Aby wykonać kroki dodatkowe inicjowanie przy użyciu renderowanej zawartości, takich jak aktywacja bibliotek JavaScript innych firm, które działają na renderowanych elementów DOM, należy użyć tego etapu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-212">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

<span data-ttu-id="fb1bf-213">`SetParameters` może zostać zastąpiona w celu wykonania kodu, zanim parametry są ustawione:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-213">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="fb1bf-214">Jeśli `base.SetParameters` nie jest wywoływana, niestandardowy kod może interpretować przychodzących wartości parametrów w sposób wymagane.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-214">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="fb1bf-215">Na przykład przychodzące parametry nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-215">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="fb1bf-216">`ShouldRender` może zostać zastąpiona w celu pomijania odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-216">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="fb1bf-217">Jeśli implementacja zwraca `true`, interfejs użytkownika są odświeżane.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-217">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="fb1bf-218">Nawet wtedy, gdy `ShouldRender` jest została zastąpiona, składnik jest zawsze wstępnie renderowane.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-218">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="fb1bf-219">Usuwanie składnika z interfejsu IDisposable</span><span class="sxs-lookup"><span data-stu-id="fb1bf-219">Component disposal with IDisposable</span></span>

<span data-ttu-id="fb1bf-220">Jeśli składnik implementuje <xref:System.IDisposable>, [Dispose — metoda](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-220">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="fb1bf-221">Używa następującego składnika `@implements IDisposable` i `Dispose` metody:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-221">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="fb1bf-222">Routing</span><span class="sxs-lookup"><span data-stu-id="fb1bf-222">Routing</span></span>

<span data-ttu-id="fb1bf-223">Routing w składnikach Razor odbywa się, podając szablonu trasy na każdej części dostępne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-223">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="fb1bf-224">Gdy *.cshtml* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-224">When a *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="fb1bf-225">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-225">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="fb1bf-226">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-226">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="fb1bf-227">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-227">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="fb1bf-228">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="fb1bf-228">Route parameters</span></span>

<span data-ttu-id="fb1bf-229">Składniki mogą odbierać parametrów trasy z szablonu trasy dostępnego w `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-229">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="fb1bf-230">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-230">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="fb1bf-231">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-231">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

<span data-ttu-id="fb1bf-232">Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-232">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="fb1bf-233">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-233">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="fb1bf-234">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-234">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="fb1bf-235">Dziedziczenie klasy bazowej dla środowiska "związane z kodem"</span><span class="sxs-lookup"><span data-stu-id="fb1bf-235">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="fb1bf-236">Pliki składników (*.cshtml*) mieszać kod znaczników HTML i C# przetwarzania kodu w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-236">Component files (*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="fb1bf-237">`@inherits` Dyrektywy może służyć do zapewnienia Razor składniki aplikacji w środowisku "związane z kodem" oddzielający składnika znaczników, od przetwarzania kodu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-237">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="fb1bf-238">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) pokazuje, jak składnik może dziedziczyć klasy bazowej, `BlazorRocksBase`w celu zapewnienia składnika właściwości i metody.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-238">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="fb1bf-239">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-239">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

<span data-ttu-id="fb1bf-240">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-240">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="fb1bf-241">Klasa bazowa powinien pochodzić od `BlazorComponent`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-241">The base class should derive from `BlazorComponent`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="fb1bf-242">Obsługa razor</span><span class="sxs-lookup"><span data-stu-id="fb1bf-242">Razor support</span></span>

<span data-ttu-id="fb1bf-243">**Dyrektywy razor**</span><span class="sxs-lookup"><span data-stu-id="fb1bf-243">**Razor directives**</span></span>

<span data-ttu-id="fb1bf-244">W poniższej tabeli przedstawiono dyrektywy razor.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-244">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="fb1bf-245">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="fb1bf-245">Directive</span></span> | <span data-ttu-id="fb1bf-246">Opis</span><span class="sxs-lookup"><span data-stu-id="fb1bf-246">Description</span></span> |
| --------- | ----------- |
| [@functions](xref:mvc/views/razor#section-5) | <span data-ttu-id="fb1bf-247">Dodaje C# blok kodu do składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-247">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="fb1bf-248">Implementuje interfejs dla klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-248">Implements an interface for the generated component class.</span></span> |
| [@inherits](xref:mvc/views/razor#section-3) | <span data-ttu-id="fb1bf-249">Zapewnia pełną kontrolę nad klasę, która dziedziczy składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-249">Provides full control of the class that the component inherits.</span></span> |
| [@inject](xref:mvc/views/razor#section-4) | <span data-ttu-id="fb1bf-250">Włącza usługi iniekcji z [kontener usługi](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-250">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="fb1bf-251">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-251">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="fb1bf-252">Określa składnik układu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-252">Specifies a layout component.</span></span> <span data-ttu-id="fb1bf-253">Składniki układu są używane, aby uniknąć zduplikowania kodu i niespójności.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-253">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [@page](xref:razor-pages/index#razor-pages) | <span data-ttu-id="fb1bf-254">Określa, że składnik obsługi żądań bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-254">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="fb1bf-255">`@page` Dyrektywy można określić za pomocą trasy i opcjonalnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-255">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="fb1bf-256">W przeciwieństwie do stron Razor `@page` dyrektywy nie musi być pierwszą dyrektywę w górnej części pliku.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-256">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="fb1bf-257">Aby uzyskać więcej informacji, zobacz [Routing](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-257">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [@using](xref:mvc/views/razor#using) | <span data-ttu-id="fb1bf-258">Dodaje C# `using` dyrektywy do klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-258">Adds the C# `using` directive to the generated component class.</span></span> |
| [@addTagHelper](xref:mvc/views/razor#tag-helpers) | <span data-ttu-id="fb1bf-259">Użyj `@addTagHelper` użycie składnika w innym zestawie niż zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-259">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

<span data-ttu-id="fb1bf-260">**Atrybuty warunkowe**</span><span class="sxs-lookup"><span data-stu-id="fb1bf-260">**Conditional attributes**</span></span>

<span data-ttu-id="fb1bf-261">Atrybuty warunkowe są renderowane na podstawie wartości platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-261">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="fb1bf-262">Jeśli wartość jest `false` lub `null`, ten atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-262">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="fb1bf-263">Jeśli wartość jest `true`, ten atrybut jest renderowany zminimalizowane.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-263">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="fb1bf-264">W poniższym przykładzie `IsCompleted` Określa, czy `checked` jest renderowany w znacznikach formantu:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-264">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="fb1bf-265">Jeśli `IsCompleted` jest `true`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-265">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="fb1bf-266">Jeśli `IsCompleted` jest `false`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-266">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="fb1bf-267">**Dodatkowe informacje na temat Razor**</span><span class="sxs-lookup"><span data-stu-id="fb1bf-267">**Additional information on Razor**</span></span>

<span data-ttu-id="fb1bf-268">Aby uzyskać więcej informacji na temat Razor, zobacz [dokumentacja składni Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-268">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="fb1bf-269">Surowy kod HTML</span><span class="sxs-lookup"><span data-stu-id="fb1bf-269">Raw HTML</span></span>

<span data-ttu-id="fb1bf-270">Ciągi są zwykle renderowane przy użyciu modelu DOM węzły tekstowe, co oznacza, że żadnych znaczników, które mogą zawierać jest ignorowany i traktowany jako literał tekstowy.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-270">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="fb1bf-271">Aby renderować kod HTML, opakowywanie zawartości HTML w `MarkupString` wartość.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-271">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="fb1bf-272">Wartość jest analizowany jako HTML lub SVG i wstawiony DOM.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-272">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="fb1bf-273">Renderowanie surowe HTML skonstruowany z dowolnego niezaufanych źródło jest **zagrożenie dla bezpieczeństwa** i należy ich unikać!</span><span class="sxs-lookup"><span data-stu-id="fb1bf-273">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="fb1bf-274">Poniższy przykład pokazuje użycie `MarkupString` typu do dodania bloku zawartości statycznej HTML do wyniku renderowania składnika:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-274">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    string myMarkup = "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="fb1bf-275">Składniki oparte na szablonach</span><span class="sxs-lookup"><span data-stu-id="fb1bf-275">Templated components</span></span>

<span data-ttu-id="fb1bf-276">Oparte na szablonach składniki są składniki, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które następnie mogą być używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-276">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="fb1bf-277">Oparte na szablonach składniki umożliwiają tworzenie składników wyższego poziomu, które są bardziej wielokrotnego użytku, niż regularne składników.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-277">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="fb1bf-278">Zawiera kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-278">A couple of examples include:</span></span>

* <span data-ttu-id="fb1bf-279">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka tabeli, wierszy i stopki.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-279">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="fb1bf-280">Składnik listy, który umożliwia użytkownikowi określić szablon służący do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-280">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="fb1bf-281">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="fb1bf-281">Template parameters</span></span>

<span data-ttu-id="fb1bf-282">Oparte na szablonach składnika jest zdefiniowany, określając jeden lub więcej parametrów składnika typu `RenderFragment` lub `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-282">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="fb1bf-283">Fragment renderowania reprezentuje segment interfejsu użytkownika, który jest renderowany przez składnik.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-283">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="fb1bf-284">Fragment renderowania opcjonalnie przyjmuje parametr, który można określić, gdy jest wywoływany fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-284">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="fb1bf-285">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-285">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="fb1bf-286">Podczas korzystania z szablonem składnik, można określić parametry szablonu przy użyciu elementy podrzędne, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="fb1bf-286">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="fb1bf-287">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="fb1bf-287">Template context parameters</span></span>

<span data-ttu-id="fb1bf-288">Składnik argumentów typu `RenderFragment<T>` przekazany jako elementy mają niejawny parametr o nazwie `context` (na przykład w poprzednim przykładzie kodu `@context.PetId`), można jednak zmienić przy użyciu nazwy parametru `Context` atrybutu w elemencie podrzędnym element.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-288">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="fb1bf-289">W poniższym przykładzie `RowTemplate` elementu `Context` Określa atrybut `pet` parametru:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-289">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="fb1bf-290">Alternatywnie, można określić `Context` atrybutu w elemencie składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-290">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="fb1bf-291">Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-291">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="fb1bf-292">Może to być przydatne, jeśli chcesz określić nazwę zawartości parametru zawartość elementu podrzędnego niejawne (bez żadnych zawijania elementu podrzędnego).</span><span class="sxs-lookup"><span data-stu-id="fb1bf-292">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="fb1bf-293">W poniższym przykładzie `Context` atrybutu jest wyświetlany na `TableTemplate` elementu i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-293">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="fb1bf-294">Składniki z kontrolą typów ogólnych</span><span class="sxs-lookup"><span data-stu-id="fb1bf-294">Generic-typed components</span></span>

<span data-ttu-id="fb1bf-295">Oparte na szablonach składniki są często objęte wpisane.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-295">Templated components are often generically typed.</span></span> <span data-ttu-id="fb1bf-296">Na przykład ogólnego składnika szablon widoku listy może zostać użyty do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-296">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="fb1bf-297">Aby zdefiniować element ogólny, należy użyć `@typeparam` dyrektywy, aby określić parametry typu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-297">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="fb1bf-298">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-298">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml?highlight=1)]

<span data-ttu-id="fb1bf-299">Podczas korzystania z kontrolą typów ogólnych składników, jeśli jest to możliwe jest wnioskowany parametr typu:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-299">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="fb1bf-300">W przeciwnym razie parametru typu muszą być jawnie określone za pomocą atrybutu, który odpowiada nazwie parametru typu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-300">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="fb1bf-301">W poniższym przykładzie `TItem="Pet"` Określa typ:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-301">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="fb1bf-302">Kaskadowe wartości i parametry</span><span class="sxs-lookup"><span data-stu-id="fb1bf-302">Cascading values and parameters</span></span>

<span data-ttu-id="fb1bf-303">W niektórych przypadkach jest wygodne, przepływ danych ze składnika nadrzędnego za pomocą składnika podrzędny [Parametry składnika](#component-parameters), szczególnie w przypadku kilku warstw składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-303">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="fb1bf-304">Kaskadowe wartości i parametry rozwiązują ten problem, zapewniając wygodny sposób w celu Podaj wartość, aby wszystkie jego elementy potomne składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-304">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="fb1bf-305">Kaskadowe wartości i parametry oferują również podejście składników do zapewnienia koordynacji.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-305">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="fb1bf-306">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="fb1bf-306">Theme example</span></span>

<span data-ttu-id="fb1bf-307">W następującym *motyw* przykład z przykładowej aplikacji `ThemeInfo` klasa określa informacje o motywie przepływ w dół hierarchii składnika tak, aby udostępnić wszystkie przyciski w ramach danego część aplikacji, ten styl.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-307">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="fb1bf-308">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-308">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="fb1bf-309">Składnik nadrzędny można podać wartość kaskadowych za pomocą składnika kaskadowa wartość.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-309">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="fb1bf-310">Składnik wartości kaskadowych otacza poddrzewo hierarchii składników i dostarcza pojedynczą wartość dla wszystkich składników w ramach tego poddrzewa.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-310">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="fb1bf-311">Na przykład przykładowa aplikacja określa informacje o motywie (`ThemeInfo`) w jednej aplikacji układów jako parametr kaskadowych dla wszystkich składników, które tworzą treści układ `@Body` właściwości.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-311">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="fb1bf-312">`ButtonClass` jest przypisywana wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-312">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="fb1bf-313">Dowolny składnik podrzędny mogą używać tej właściwości, za pośrednictwem `ThemeInfo` cascading obiektu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-313">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="fb1bf-314">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-314">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

```cshtml
@inherits BlazorLayoutComponent
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="fb1bf-315">Zapewnienie korzystania z wartości kaskadowych, składniki deklarować parametrów kaskadowych przy użyciu `[CascadingParameter]` atrybut lub na podstawie wartości ciągu nazwy:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-315">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")] PermInfo Permissions { get; set; }
```

<span data-ttu-id="fb1bf-316">Powiązanie z wartością nazwy ciągu jest istotne, jeśli masz wiele kaskadowych wartości tego samego typu i potrzebujesz odróżnić je w ramach tej samej poddrzewo.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-316">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="fb1bf-317">Kaskadowe wartości są powiązane kaskadowych parametry według typu.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-317">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="fb1bf-318">W przykładowej aplikacji składnika kaskadowych wartości parametrów motyw wiąże `ThemeInfo` kaskadowa wartość parametru kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-318">In the sample app, the Cascading Values Parameters Theme component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="fb1bf-319">Parametr służy do ustawiania klasę CSS dla jednego z przyciski wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-319">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="fb1bf-320">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-320">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="fb1bf-321">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="fb1bf-321">TabSet example</span></span>

<span data-ttu-id="fb1bf-322">Parametry kaskadowych również włączyć składników do współpracy w całej hierarchii składnika.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-322">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="fb1bf-323">Na przykład, należy wziąć pod uwagę następujące *TabSet* przykładu w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-323">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="fb1bf-324">Przykładowa aplikacja ma `ITab` interfejs, który karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-324">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="fb1bf-325">Składnik kaskadowych wartości parametrów TabSet używa ustawienia karty składnik, który zawiera kilka składników karty:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-325">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="fb1bf-326">Składniki karty podrzędnej jawnie nie są przekazywane jako parametry można ustawić kartę.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-326">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="fb1bf-327">Zamiast tego składniki karty podrzędnej są częścią zawartość elementu podrzędnego zestawu kartę.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-327">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="fb1bf-328">Jednak ustawienia karty nadal musi wiedzieć o poszczególnych składników karty, dzięki czemu można renderować, nagłówki i aktywną kartę. Włączyć koordynacja bez konieczności dodatkowego kodu, Ustaw kartę składnika *może zapewnić sama jako wartość kaskadowych* , następnie zostaje pobrana przez podrzędny składniki kartę.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-328">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="fb1bf-329">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-329">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="fb1bf-330">Podrzędny przechwytywania składniki kartę zawierającego ustawienia karty jako parametr kaskadowych, więc składniki kartę dodają same siebie do ustawienia karty i współrzędnych karty, które jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-330">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="fb1bf-331">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-331">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="fb1bf-332">Szablony razor</span><span class="sxs-lookup"><span data-stu-id="fb1bf-332">Razor templates</span></span>

<span data-ttu-id="fb1bf-333">Renderowanie fragmentów można zdefiniować przy użyciu składni szablonów Razor.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-333">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="fb1bf-334">Szablony razor służą do definiowania fragmentu interfejsu użytkownika i założono następujący format:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-334">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="fb1bf-335">Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-335">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="fb1bf-336">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-336">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="fb1bf-337">Renderowanie fragmentów zdefiniowane przy użyciu Razor szablony mogą być przekazywane jako argumenty do składników oparte na szablonach lub renderowane bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="fb1bf-337">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="fb1bf-338">Na przykład szablony poprzedniego bezpośrednio są renderowane w następującym kodem Razor:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-338">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="fb1bf-339">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="fb1bf-339">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```
