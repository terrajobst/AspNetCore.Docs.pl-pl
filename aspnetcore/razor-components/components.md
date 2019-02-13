---
title: Tworzenie i używanie składników Razor
author: guardrex
description: Informacje o sposobie tworzenia i używania składników Razor, w tym jak powiązać z danymi, obsługa zdarzeń i Zarządzaj cyklami życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/components
ms.openlocfilehash: 8f0a2c7ab7650894010e39ba3cbdcc6a97e04069
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159534"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="c7f86-103">Tworzenie i używanie składników Razor</span><span class="sxs-lookup"><span data-stu-id="c7f86-103">Create and use Razor Components</span></span>

<span data-ttu-id="c7f86-104">Przez [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), i [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="c7f86-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="c7f86-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c7f86-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="c7f86-106">Zobacz [wprowadzenie](xref:razor-components/get-started) tematu wstępnie wymaganych składników.</span><span class="sxs-lookup"><span data-stu-id="c7f86-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="c7f86-107">Razor składniki aplikacji są tworzone przy użyciu *składniki*.</span><span class="sxs-lookup"><span data-stu-id="c7f86-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="c7f86-108">Składnik jest niezależna fragmentu interfejsu użytkownika (UI), takich jak strony, okno dialogowe lub formularza.</span><span class="sxs-lookup"><span data-stu-id="c7f86-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="c7f86-109">Składnik zawiera znacznik HTML do renderowania wraz z logiki przetwarzania potrzebne do Wstawianie danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-109">A component includes both the HTML markup to render along with the processing logic needed to inject data or respond to UI events.</span></span> <span data-ttu-id="c7f86-110">Składniki są elastyczne i uproszczone i może być zagnieżdżony, ponownie, a także współużytkowane między projektami.</span><span class="sxs-lookup"><span data-stu-id="c7f86-110">Components are flexible and lightweight, and they can be nested, reused, and shared between projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="c7f86-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="c7f86-111">Component classes</span></span>

<span data-ttu-id="c7f86-112">Składniki są zazwyczaj implementowani w  *\*.cshtml* plików przy użyciu kombinacji C# i kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="c7f86-112">Components are typically implemented in *\*.cshtml* files using a combination of C# and HTML markup.</span></span> <span data-ttu-id="c7f86-113">W interfejsie użytkownika dla składnika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="c7f86-113">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="c7f86-114">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="c7f86-114">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="c7f86-115">Podczas kompilowania aplikacji Razor składników, kod znaczników HTML i C# logiki renderowania są konwertowane na klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-115">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="c7f86-116">Nazwa wygenerowanej klasy odpowiada nazwie pliku.</span><span class="sxs-lookup"><span data-stu-id="c7f86-116">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="c7f86-117">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku (więcej niż jeden `@functions` bloku jest dozwolone).</span><span class="sxs-lookup"><span data-stu-id="c7f86-117">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="c7f86-118">W `@functions` bloku, stan składników (właściwości, pola) została określona wraz z metody obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="c7f86-118">In the `@functions` block, component state (properties, fields) is specified along with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="c7f86-119">Elementy członkowskie składnika mogą posłużyć jako część składnika przez renderowanie przy użyciu logiki C# wyrażeń, które zaczyna się `@`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-119">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="c7f86-120">Na przykład C# pole jest renderowane przez dodanie przedrostka `@` na nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="c7f86-120">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="c7f86-121">Poniższy przykład daje w wyniku i renderuje:</span><span class="sxs-lookup"><span data-stu-id="c7f86-121">The following example evaluates and renders:</span></span>

* <span data-ttu-id="c7f86-122">`_headingFontStyle` wartości właściwości CSS dla `font-style`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-122">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="c7f86-123">`_headingText` w treści `<h1>` elementu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-123">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="c7f86-124">Po początkowo renderowania składnik składnika generuje jej drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c7f86-124">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="c7f86-125">Składniki razor następnie porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszystkie zmiany do przeglądarki w modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="c7f86-125">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="using-components"></a><span data-ttu-id="c7f86-126">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="c7f86-126">Using components</span></span>

<span data-ttu-id="c7f86-127">Składniki mogą zawierać inne składniki, deklarując je przy użyciu składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="c7f86-127">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="c7f86-128">Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-128">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="c7f86-129">Renderuje następujące znaczniki `HeadingComponent` (*HeadingComponent.cshtml*) wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="c7f86-129">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?start=11&end=11)]

## <a name="component-parameters"></a><span data-ttu-id="c7f86-130">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="c7f86-130">Component parameters</span></span>

<span data-ttu-id="c7f86-131">Składniki mogą mieć *Parametry składnika*, które są definiowane za pomocą *niepublicznych* ozdobione właściwości klasy składnika `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-131">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="c7f86-132">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="c7f86-132">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="c7f86-133">W poniższym przykładzie `ParentComponent` ustawia wartość `Title` właściwość `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="c7f86-133">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="c7f86-134">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-134">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=5)]

<span data-ttu-id="c7f86-135">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-135">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="c7f86-136">Zawartość elementu podrzędnego</span><span class="sxs-lookup"><span data-stu-id="c7f86-136">Child content</span></span>

<span data-ttu-id="c7f86-137">Składniki można ustawić zawartość w innym składniku.</span><span class="sxs-lookup"><span data-stu-id="c7f86-137">Components can set the content in another component.</span></span> <span data-ttu-id="c7f86-138">Przypisywanie składnik udostępnia zawartości między tagami, które określają odbieranie składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-138">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="c7f86-139">Na przykład `ParentComponent` można udostępnić zawartość, która ma być renderowany przy platformy `ChildComponent` , umieszczając zawartość wewnątrz  **\<ChildComponent >** tagów.</span><span class="sxs-lookup"><span data-stu-id="c7f86-139">For example, a `ParentComponent` can provide content that is to be rendered by a `ChildComponent` by placing the content inside **\<ChildComponent>** tags.</span></span>

<span data-ttu-id="c7f86-140">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-140">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=6)]

<span data-ttu-id="c7f86-141">Składnik podrzędny ma `ChildContent` właściwość, która reprezentuje `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-141">The child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="c7f86-142">Wartość `ChildContent` jest umieszczony w znaczniku elementu podrzędnego, której zawartość ma być renderowany.</span><span class="sxs-lookup"><span data-stu-id="c7f86-142">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="c7f86-143">W poniższym przykładzie wartość `ChildContent` jest otrzymane od składnika nadrzędnego i renderowania wewnątrz panelu Bootstrap `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-143">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="c7f86-144">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-144">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="c7f86-145">Odbieranie właściwość `RenderFragment` zawartość, musi nosić `ChildContent` przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="c7f86-145">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="c7f86-146">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="c7f86-146">Data binding</span></span>

<span data-ttu-id="c7f86-147">Powiązanie danych do składników i elementów DOM odbywa się za pomocą `bind` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-147">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="c7f86-148">Poniższy przykład tworzy powiązanie `ItalicsCheck` właściwość z polem wyboru zaznaczone stanu:</span><span class="sxs-lookup"><span data-stu-id="c7f86-148">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" bind="@_italicsCheck" />
```

<span data-ttu-id="c7f86-149">Gdy pole wyboru jest zaznaczone, a następnie wyczyszczone, wartość właściwości jest aktualizowany do `true` i `false`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="c7f86-149">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="c7f86-150">Pole wyboru jest aktualizowana w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="c7f86-150">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="c7f86-151">Ponieważ składniki renderować się po wykonaniu kod procedury obsługi zdarzeń, aktualizacje właściwości są zazwyczaj odzwierciedlane w interfejsie użytkownika od razu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-151">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="c7f86-152">Za pomocą `bind` z `CurrentValue` właściwości (`<input bind="@CurrentValue" />`) jest zasadniczo odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c7f86-152">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="c7f86-153">Po wyrenderowaniu składnika `value` danego elementu wejściowego pochodzi z `CurrentValue` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c7f86-153">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="c7f86-154">Gdy użytkownik wpisuje w polu tekstowym `onchange` jest uruchamiany i `CurrentValue` zostaje ustalona zmieniona wartość.</span><span class="sxs-lookup"><span data-stu-id="c7f86-154">When the user types in the text box, the `onchange` is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="c7f86-155">W rzeczywistości generowania kodu jest nieco bardziej złożone, ponieważ `bind` zajmuje się kilka przypadków, w którym konwersje są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="c7f86-155">In reality, the code generation is a little more complex because `bind` deals with a few cases where type conversions are performed.</span></span> <span data-ttu-id="c7f86-156">W zasadzie `bind` kojarzy bieżącą wartość wyrażenia z `value` atrybutu i uchwytów zmiany przy użyciu zarejestrowanego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="c7f86-156">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="c7f86-157">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="c7f86-157">**Format strings**</span></span>

<span data-ttu-id="c7f86-158">Powiązanie danych w programach [daty/godziny](https://docs.microsoft.com/dotnet/api/system.datetime) formatowanie ciągów (ale nie inne wyrażenia w tej chwili, takich jak waluta lub liczba podzielony na fragmenty):</span><span class="sxs-lookup"><span data-stu-id="c7f86-158">Data binding works with [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime) format strings (but not other format expressions at this time, such as currency or number formats):</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="c7f86-159">`format-value` Atrybut określa format daty do zastosowania do `value` z `input` elementu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-159">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="c7f86-160">Format umożliwia również przeanalizować wartość po `onchange` wystąpi zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="c7f86-160">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="c7f86-161">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="c7f86-161">**Component parameters**</span></span>

<span data-ttu-id="c7f86-162">Powiązanie rozpoznaje również Parametry składnika, gdzie `bind-{property}` można powiązać wartości właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="c7f86-162">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="c7f86-163">Używa następującego składnika `ChildComponent` i wiąże `ParentYear` parametru z obiektu `Year` parametru w składniku podrzędne:</span><span class="sxs-lookup"><span data-stu-id="c7f86-163">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="c7f86-164">Składnik nadrzędny:</span><span class="sxs-lookup"><span data-stu-id="c7f86-164">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">Change Year to 1986</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="c7f86-165">Element podrzędny:</span><span class="sxs-lookup"><span data-stu-id="c7f86-165">Child component:</span></span>

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

<span data-ttu-id="c7f86-166">`Year` Parametr jest możliwej do wiązania, ponieważ ma ona towarzyszące `YearChanged` zdarzeń, który jest zgodny z typem `Year` parametru.</span><span class="sxs-lookup"><span data-stu-id="c7f86-166">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="c7f86-167">Trwa ładowanie `ParentComponent` tworzy następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="c7f86-167">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="c7f86-168">Jeśli wartość `ParentYear` zmianie właściwości, wybierając przycisk w `ParentComponent`, `Year` właściwość `ChildComponent` jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="c7f86-168">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="c7f86-169">Nowa wartość `Year` jest wyświetlana w Interfejsie użytkownika po `ParentComponent` jest rerendered:</span><span class="sxs-lookup"><span data-stu-id="c7f86-169">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

## <a name="event-handling"></a><span data-ttu-id="c7f86-170">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="c7f86-170">Event handling</span></span>

<span data-ttu-id="c7f86-171">Składniki razor udostępniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c7f86-171">Razor Components provide event handling features.</span></span> <span data-ttu-id="c7f86-172">Atrybut elementu HTML o nazwie `on<event>` (na przykład `onclick`, `onsubmit`) z wartością wpisane delegata składniki Razor traktuje wartość atrybutu jako program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c7f86-172">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="c7f86-173">Nazwa atrybutu zawsze zaczyna się od `on`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-173">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="c7f86-174">Poniższy kod wywoła `UpdateHeading` metodę po wybraniu przycisku w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="c7f86-174">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="c7f86-175">Poniższy kod wywoła `CheckboxChanged` metody, gdy pole wyboru jest zmieniana w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="c7f86-175">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="c7f86-176">Programy obsługi zdarzeń można też asynchronicznego i zwracają `Task`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-176">Event handlers can also be asynchronous and return a `Task`.</span></span> <span data-ttu-id="c7f86-177">Nie ma konieczności ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-177">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="c7f86-178">Wyjątki są rejestrowane w momencie ich wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c7f86-178">Exceptions are logged when they occur.</span></span>

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

<span data-ttu-id="c7f86-179">Niektóre zdarzenia są dozwolone typy argumentów zdarzenia określonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c7f86-179">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="c7f86-180">Jeśli dostęp do jednego z następujących typów zdarzeń nie jest to konieczne, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="c7f86-180">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="c7f86-181">Lista argumentów zdarzeń obsługiwanych jest:</span><span class="sxs-lookup"><span data-stu-id="c7f86-181">The list of supported event arguments is:</span></span>

* <span data-ttu-id="c7f86-182">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="c7f86-182">UIEventArgs</span></span>
* <span data-ttu-id="c7f86-183">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="c7f86-183">UIChangeEventArgs</span></span>
* <span data-ttu-id="c7f86-184">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="c7f86-184">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="c7f86-185">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="c7f86-185">UIMouseEventArgs</span></span>

<span data-ttu-id="c7f86-186">Wyrażenia lambda może również służyć:</span><span class="sxs-lookup"><span data-stu-id="c7f86-186">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="c7f86-187">Często jest to wygodne zamknąć dodatkowe wartości, takich jak podczas iteracji w zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="c7f86-187">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="c7f86-188">Poniższy przykład tworzy trzy przyciski wszystkich, która wywołuje metodę `UpdateHeading` przekazywaniem argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`) w przypadku wybrania w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="c7f86-188">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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

## <a name="capture-references-to-components"></a><span data-ttu-id="c7f86-189">Przechwytywanie odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="c7f86-189">Capture references to components</span></span>

<span data-ttu-id="c7f86-190">Składnik odwołuje się do zapewnienia get sposób odwołania do wystąpienia składnika, dzięki czemu można wysyłać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-190">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="c7f86-191">Do przechwycenia odwołania do składników, należy dodać `ref` atrybutu do elementu podrzędnego, a następnie zdefiniować pole o tej samej nazwie i typie jako element podrzędny.</span><span class="sxs-lookup"><span data-stu-id="c7f86-191">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

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

<span data-ttu-id="c7f86-192">Po wyrenderowaniu składnika `loginDialog` pole jest wypełniane `MyLoginDialog` wystąpienie składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="c7f86-192">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="c7f86-193">Następnie można wywoływać metod .NET w instancji składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-193">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7f86-194">`loginDialog` Zmiennej tylko jest wypełniana po składnik jest renderowana i jego dane wyjściowe obejmują `MyLoginDialog` elementu, ponieważ do tego czasu nie ma nic do odwołania.</span><span class="sxs-lookup"><span data-stu-id="c7f86-194">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element because until then there is nothing to reference.</span></span> <span data-ttu-id="c7f86-195">Do manipulowania odwołania do składników, po zakończeniu renderowania składnik, należy użyć `OnAfterRenderAsync` lub `OnAfterRender` metody cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="c7f86-195">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` lifecycle methods.</span></span>

<span data-ttu-id="c7f86-196">Podczas gdy przechwytywania odwołania do składników używa składni podobnie [przechwytywania odwołania do elementu](xref:razor-components/javascript-interop#capture-references-to-elements), nie jest [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop) funkcji.</span><span class="sxs-lookup"><span data-stu-id="c7f86-196">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="c7f86-197">Odwołania do składników nie są przekazywane do kodu JavaScript; są one używane tylko w kodzie .NET.</span><span class="sxs-lookup"><span data-stu-id="c7f86-197">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="c7f86-198">Czy **nie** umożliwia odwołania do składników mutować stan składnikach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="c7f86-198">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="c7f86-199">Zamiast tego należy zawsze używać normalnego parametry deklaratywne do przekazywania danych do elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="c7f86-199">Instead, always use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="c7f86-200">To sprawia, że podrzędnych automatycznie rerender w właściwym czasie.</span><span class="sxs-lookup"><span data-stu-id="c7f86-200">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="c7f86-201">Cykl życia metody</span><span class="sxs-lookup"><span data-stu-id="c7f86-201">Lifecycle methods</span></span>

<span data-ttu-id="c7f86-202">`OnInitAsync` i `OnInit` wykonanie kodu po zainicjowaniu składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-202">`OnInitAsync` and `OnInit` execute code after the component has been initialized.</span></span> <span data-ttu-id="c7f86-203">Aby wykonać operację asynchroniczną, użyj `OnInitAsync` i użyj `await` — słowo kluczowe:</span><span class="sxs-lookup"><span data-stu-id="c7f86-203">To perform an asynchronous operation, use `OnInitAsync` and use the `await` keyword:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="c7f86-204">Operacja synchroniczna, można użyć `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="c7f86-204">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="c7f86-205">`OnParametersSetAsync` i `OnParametersSet` są wywoływane, gdy składnik, który otrzyma parametry od jego elementu nadrzędnego, a wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="c7f86-205">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="c7f86-206">Te metody są wykonywane po `OnInit` podczas inicjowania składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-206">These methods are executed after `OnInit` during component initialization.</span></span>

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

<span data-ttu-id="c7f86-207">`OnAfterRenderAsync` i `OnAfterRender` jest wywoływana za każdym razem po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-207">`OnAfterRenderAsync` and `OnAfterRender` are called each time after a component has finished rendering.</span></span> <span data-ttu-id="c7f86-208">Odwołania do elementu, jak i składnika zostaną wypełnione na tym etapie.</span><span class="sxs-lookup"><span data-stu-id="c7f86-208">Element and component references are populated at this point.</span></span> <span data-ttu-id="c7f86-209">Aby wykonać kroki dodatkowe inicjowanie przy użyciu renderowanej zawartości, takich jak aktywacja bibliotek JavaScript innych firm, które działają na renderowanych elementów DOM, należy użyć tego etapu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-209">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

<span data-ttu-id="c7f86-210">`SetParameters` może zostać zastąpiona w celu wykonania kodu, zanim parametry są ustawione:</span><span class="sxs-lookup"><span data-stu-id="c7f86-210">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="c7f86-211">Jeśli `base.SetParameters` nie jest wywoływana, niestandardowy kod może interpretować przychodzących wartości parametrów w sposób wymagane.</span><span class="sxs-lookup"><span data-stu-id="c7f86-211">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="c7f86-212">Na przykład przychodzące parametry nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="c7f86-212">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="c7f86-213">`ShouldRender` może zostać zastąpiona w celu pomijania odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-213">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="c7f86-214">Jeśli implementacja zwraca `true`, interfejs użytkownika są odświeżane.</span><span class="sxs-lookup"><span data-stu-id="c7f86-214">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="c7f86-215">Nawet wtedy, gdy `ShouldRender` jest została zastąpiona, składnik jest zawsze wstępnie renderowane.</span><span class="sxs-lookup"><span data-stu-id="c7f86-215">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="c7f86-216">Usuwanie składnika z interfejsu IDisposable</span><span class="sxs-lookup"><span data-stu-id="c7f86-216">Component disposal with IDisposable</span></span>

<span data-ttu-id="c7f86-217">Jeśli składnik implementuje [interfejsu IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable), [Dispose — metoda](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-217">If a component implements [IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable), the [Dispose method](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="c7f86-218">Używa następującego składnika `@implements IDisposable` i `Dispose` metody:</span><span class="sxs-lookup"><span data-stu-id="c7f86-218">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="c7f86-219">Routing</span><span class="sxs-lookup"><span data-stu-id="c7f86-219">Routing</span></span>

<span data-ttu-id="c7f86-220">Routing w składnikach Razor odbywa się, podając szablonu trasy na każdej części dostępne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7f86-220">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="c7f86-221">Gdy  *\*.cshtml* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje [RouteAttribute](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="c7f86-221">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="c7f86-222">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c7f86-222">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="c7f86-223">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-223">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="c7f86-224">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="c7f86-224">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

## <a name="route-parameters"></a><span data-ttu-id="c7f86-225">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="c7f86-225">Route parameters</span></span>

<span data-ttu-id="c7f86-226">Składniki mogą odbierać parametrów trasy z szablonu trasy dostępnego w `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="c7f86-226">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="c7f86-227">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-227">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="c7f86-228">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-228">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=9)]

<span data-ttu-id="c7f86-229">Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c7f86-229">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="c7f86-230">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="c7f86-230">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="c7f86-231">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c7f86-231">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="c7f86-232">Dziedziczenie klasy bazowej dla środowiska "związane z kodem"</span><span class="sxs-lookup"><span data-stu-id="c7f86-232">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="c7f86-233">Pliki składników (*\*.cshtml*) mieszać kod znaczników HTML i C# przetwarzania kodu w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="c7f86-233">Component files (*\*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="c7f86-234">`@inherits` Dyrektywy może służyć do zapewnienia Razor składniki aplikacji w środowisku "związane z kodem" oddzielający składnika znaczników, od przetwarzania kodu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-234">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="c7f86-235">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) pokazuje, jak składnik może dziedziczyć klasy bazowej, `BlazorRocksBase`w celu zapewnienia składnika właściwości i metody.</span><span class="sxs-lookup"><span data-stu-id="c7f86-235">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="c7f86-236">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-236">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?start=1&end=8)]

<span data-ttu-id="c7f86-237">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-237">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="c7f86-238">Klasa bazowa powinien pochodzić od `BlazorComponent`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-238">The base class should derive from `BlazorComponent`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="c7f86-239">Obsługa razor</span><span class="sxs-lookup"><span data-stu-id="c7f86-239">Razor support</span></span>

<span data-ttu-id="c7f86-240">**Dyrektywy razor**</span><span class="sxs-lookup"><span data-stu-id="c7f86-240">**Razor directives**</span></span>

<span data-ttu-id="c7f86-241">W poniższej tabeli przedstawiono dyrektywy razor.</span><span class="sxs-lookup"><span data-stu-id="c7f86-241">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="c7f86-242">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="c7f86-242">Directive</span></span> | <span data-ttu-id="c7f86-243">Opis</span><span class="sxs-lookup"><span data-stu-id="c7f86-243">Description</span></span> |
| --------- | ----------- |
| [@functions](https://docs.microsoft.com/aspnet/core/mvc/views/razor#functions) | <span data-ttu-id="c7f86-244">Dodaje C# blok kodu do składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-244">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="c7f86-245">Implementuje interfejs dla klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-245">Implements an interface for the generated component class.</span></span> |
| [@inherits](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inherits) | <span data-ttu-id="c7f86-246">Zapewnia pełną kontrolę nad klasę, która dziedziczy składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-246">Provides full control of the class that the component inherits.</span></span> |
| [@inject](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inject) | <span data-ttu-id="c7f86-247">Włącza usługi iniekcji z [kontener usługi](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c7f86-247">Enables service injection from the [service container](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="c7f86-248">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](https://docs.microsoft.com/aspnet/core/mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c7f86-248">For more information, see [Dependency injection into views](https://docs.microsoft.com/aspnet/core/mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="c7f86-249">Określa składnik układu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-249">Specifies a layout component.</span></span> <span data-ttu-id="c7f86-250">Składniki układu są używane, aby uniknąć zduplikowania kodu i niespójności.</span><span class="sxs-lookup"><span data-stu-id="c7f86-250">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [@page](https://docs.microsoft.com/aspnet/core/mvc/razor-pages#razor-pages) | <span data-ttu-id="c7f86-251">Określa, że składnik obsługi żądań bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c7f86-251">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="c7f86-252">`@page` Dyrektywy można określić za pomocą trasy i opcjonalnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="c7f86-252">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="c7f86-253">W przeciwieństwie do stron Razor `@page` dyrektywy nie musi być pierwszą dyrektywę w górnej części pliku.</span><span class="sxs-lookup"><span data-stu-id="c7f86-253">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="c7f86-254">Aby uzyskać więcej informacji, zobacz [Routing](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="c7f86-254">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [@using](https://docs.microsoft.com/aspnet/core/mvc/views/razor#using) | <span data-ttu-id="c7f86-255">Dodaje C# `using` dyrektywy do klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-255">Adds the C# `using` directive to the generated component class.</span></span> |
| [@addTagHelper](https://docs.microsoft.com/aspnet/core/mvc/views/razor#tag-helpers) | <span data-ttu-id="c7f86-256">Użyj `@addTagHelper` użycie składnika w innym zestawie niż zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7f86-256">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

<span data-ttu-id="c7f86-257">**Atrybuty warunkowe**</span><span class="sxs-lookup"><span data-stu-id="c7f86-257">**Conditional attributes**</span></span>

<span data-ttu-id="c7f86-258">Atrybuty warunkowe są renderowane na podstawie wartości platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="c7f86-258">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="c7f86-259">Jeśli wartość jest `false` lub `null`, ten atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="c7f86-259">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="c7f86-260">Jeśli wartość jest `true`, ten atrybut jest renderowany zminimalizowane.</span><span class="sxs-lookup"><span data-stu-id="c7f86-260">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="c7f86-261">W poniższym przykładzie `IsCompleted` Określa, czy `checked` jest renderowany w znacznikach formantu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-261">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup.</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="c7f86-262">Jeśli `IsCompleted` jest `true`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="c7f86-262">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="c7f86-263">Jeśli `IsCompleted` jest `false`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="c7f86-263">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="c7f86-264">**Dodatkowe informacje na temat Razor**</span><span class="sxs-lookup"><span data-stu-id="c7f86-264">**Additional information on Razor**</span></span>

<span data-ttu-id="c7f86-265">Aby uzyskać więcej informacji na temat Razor, zobacz [dokumentacja składni Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="c7f86-265">For more information on Razor, see the [Razor syntax reference](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="c7f86-266">Należy pamiętać, że nie wszystkie funkcje aparatu Razor są dostępne w składnikach Razor w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="c7f86-266">Note that not all of the features of Razor are available in Razor Components at this time.</span></span>

## <a name="raw-html"></a><span data-ttu-id="c7f86-267">Surowy kod HTML</span><span class="sxs-lookup"><span data-stu-id="c7f86-267">Raw HTML</span></span>

<span data-ttu-id="c7f86-268">Ciągi są zwykle renderowane przy użyciu modelu DOM węzły tekstowe, co oznacza, że żadnych znaczników, które mogą zawierać jest ignorowany i traktowany jako literał tekstowy.</span><span class="sxs-lookup"><span data-stu-id="c7f86-268">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="c7f86-269">Aby renderować kod HTML, opakowywanie zawartości HTML w `MarkupString` wartości, który następnie jest analizowany jako HTML lub SVG i wstawiony DOM.</span><span class="sxs-lookup"><span data-stu-id="c7f86-269">To render raw HTML, wrap the HTML content in a `MarkupString` value, which is then parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="c7f86-270">Renderowanie surowe HTML skonstruowany z dowolnego niezaufanych źródło jest **zagrożenie dla bezpieczeństwa** i należy ich unikać!</span><span class="sxs-lookup"><span data-stu-id="c7f86-270">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="c7f86-271">Poniższy przykład pokazuje użycie `MarkupString` typu do dodania bloku zawartości statycznej HTML do wyniku renderowania składnika:</span><span class="sxs-lookup"><span data-stu-id="c7f86-271">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    string myMarkup = "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="c7f86-272">Składniki oparte na szablonach</span><span class="sxs-lookup"><span data-stu-id="c7f86-272">Templated components</span></span>

<span data-ttu-id="c7f86-273">Oparte na szablonach składniki są składniki, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które następnie mogą być używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-273">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="c7f86-274">Oparte na szablonach składniki umożliwiają tworzenie składników wyższego poziomu, które są bardziej wielokrotnego użytku, niż regularne składników.</span><span class="sxs-lookup"><span data-stu-id="c7f86-274">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="c7f86-275">Zawiera kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="c7f86-275">A couple of examples include:</span></span>

* <span data-ttu-id="c7f86-276">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka tabeli, wierszy i stopki.</span><span class="sxs-lookup"><span data-stu-id="c7f86-276">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="c7f86-277">Składnik listy, który umożliwia użytkownikowi określić szablon służący do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="c7f86-277">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="c7f86-278">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="c7f86-278">Template parameters</span></span>

<span data-ttu-id="c7f86-279">Oparte na szablonach składnika jest zdefiniowany, określając jeden lub więcej parametrów składnika typu `RenderFragment` lub `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-279">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="c7f86-280">Fragment renderowania reprezentuje segment interfejsu użytkownika, który jest renderowany przez składnik.</span><span class="sxs-lookup"><span data-stu-id="c7f86-280">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="c7f86-281">Fragment renderowania opcjonalnie przyjmuje parametr, który można określić, gdy jest wywoływany fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="c7f86-281">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="c7f86-282">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-282">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="c7f86-283">Podczas korzystania z szablonem składnik, można określić parametry szablonu przy użyciu elementy podrzędne, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="c7f86-283">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="c7f86-284">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="c7f86-284">Template context parameters</span></span>

<span data-ttu-id="c7f86-285">Składnik argumentów typu `RenderFragment<T>` przekazany jako elementy mają niejawny parametr o nazwie `context` (na przykład w poprzednim przykładzie kodu `@context.PetId`), można jednak zmienić przy użyciu nazwy parametru `Context` atrybutu w elemencie podrzędnym element.</span><span class="sxs-lookup"><span data-stu-id="c7f86-285">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="c7f86-286">W poniższym przykładzie `RowTemplate` elementu `Context` Określa atrybut `pet` parametru:</span><span class="sxs-lookup"><span data-stu-id="c7f86-286">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="c7f86-287">Alternatywnie, można określić `Context` atrybutu w elemencie składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-287">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="c7f86-288">Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-288">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="c7f86-289">Może to być przydatne, jeśli chcesz określić nazwę zawartości parametru zawartość elementu podrzędnego niejawne (bez żadnych zawijania elementu podrzędnego).</span><span class="sxs-lookup"><span data-stu-id="c7f86-289">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="c7f86-290">W poniższym przykładzie `Context` atrybutu jest wyświetlany na `TableTemplate` elementu i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="c7f86-290">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="c7f86-291">Składniki z kontrolą typów ogólnych</span><span class="sxs-lookup"><span data-stu-id="c7f86-291">Generic-typed components</span></span>

<span data-ttu-id="c7f86-292">Oparte na szablonach składniki są często objęte wpisane.</span><span class="sxs-lookup"><span data-stu-id="c7f86-292">Templated components are often generically typed.</span></span> <span data-ttu-id="c7f86-293">Na przykład element ListView ogólny może zostać użyty do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="c7f86-293">For example, a generic ListView component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="c7f86-294">Aby zdefiniować element ogólny, należy użyć `@typeparam` dyrektywy, aby określić parametry typu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-294">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="c7f86-295">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-295">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml?highlight=1)]

<span data-ttu-id="c7f86-296">Podczas korzystania z kontrolą typów ogólnych składników, jeśli jest to możliwe jest wnioskowany parametr typu:</span><span class="sxs-lookup"><span data-stu-id="c7f86-296">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="c7f86-297">W przeciwnym razie parametru typu muszą być jawnie określone za pomocą atrybutu, który odpowiada nazwie parametru typu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-297">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="c7f86-298">W poniższym przykładzie `TItem="Pet"` Określa typ:</span><span class="sxs-lookup"><span data-stu-id="c7f86-298">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="c7f86-299">Kaskadowe wartości i parametry</span><span class="sxs-lookup"><span data-stu-id="c7f86-299">Cascading values and parameters</span></span>

<span data-ttu-id="c7f86-300">W niektórych przypadkach jest wygodne, przepływ danych ze składnika nadrzędnego za pomocą składnika podrzędny [Parametry składnika](#component-parameters), szczególnie w przypadku kilku warstw składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-300">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="c7f86-301">Kaskadowe wartości i parametry rozwiązują ten problem, zapewniając wygodny sposób w celu Podaj wartość, aby wszystkie jego elementy potomne składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="c7f86-301">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="c7f86-302">Kaskadowe wartości i parametry oferują również podejście składników do zapewnienia koordynacji.</span><span class="sxs-lookup"><span data-stu-id="c7f86-302">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="c7f86-303">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="c7f86-303">Theme example</span></span>

<span data-ttu-id="c7f86-304">W następującym *motyw* przykład z przykładowej aplikacji `ThemeInfo` klasa określa informacje o motywie przepływ w dół hierarchii składnika tak, aby udostępnić wszystkie przyciski w ramach danego część aplikacji, ten styl.</span><span class="sxs-lookup"><span data-stu-id="c7f86-304">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="c7f86-305">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-305">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="c7f86-306">Składnik nadrzędny może zapewnić kaskadowych przy użyciu wartości `CascadingValue` składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-306">An ancestor component can provide a cascading value using the `CascadingValue` component.</span></span> <span data-ttu-id="c7f86-307">`CascadingValue` Składnika otacza poddrzewo hierarchii składników i dostarcza pojedynczą wartość dla wszystkich składników w ramach tego poddrzewa.</span><span class="sxs-lookup"><span data-stu-id="c7f86-307">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="c7f86-308">Na przykład przykładowa aplikacja określa informacje o motywie (`ThemeInfo`) w jednej aplikacji układów jako parametr kaskadowych dla wszystkich składników, które tworzą treści układ `@Body` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c7f86-308">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="c7f86-309">`ButtonClass` jest przypisywana wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-309">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="c7f86-310">Dowolny składnik podrzędny mogą używać tej właściwości, za pośrednictwem `ThemeInfo` cascading obiektu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-310">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="c7f86-311">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-311">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

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

<span data-ttu-id="c7f86-312">Zapewnienie korzystania z wartości kaskadowych, składniki deklarować parametrów kaskadowych przy użyciu `[CascadingParameter]` atrybut lub na podstawie wartości ciągu nazwy:</span><span class="sxs-lookup"><span data-stu-id="c7f86-312">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")] PermInfo Permissions { get; set; }
```

<span data-ttu-id="c7f86-313">Powiązanie z wartością nazwy ciągu jest istotne, jeśli masz wiele kaskadowych wartości tego samego typu i potrzebujesz odróżnić je w ramach tej samej poddrzewo.</span><span class="sxs-lookup"><span data-stu-id="c7f86-313">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="c7f86-314">Kaskadowe wartości są powiązane kaskadowych parametry według typu.</span><span class="sxs-lookup"><span data-stu-id="c7f86-314">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="c7f86-315">W przykładowej aplikacji `CascadingValuesParametersTheme` składnika jest powiązana `ThemeInfo` kaskadowa wartość parametru kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="c7f86-315">In the sample app, the `CascadingValuesParametersTheme` component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="c7f86-316">Parametr służy do ustawiania klasę CSS dla jednego z przyciski wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="c7f86-316">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="c7f86-317">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-317">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="c7f86-318">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="c7f86-318">TabSet example</span></span>

<span data-ttu-id="c7f86-319">Parametry kaskadowych również włączyć składników do współpracy w całej hierarchii składnika.</span><span class="sxs-lookup"><span data-stu-id="c7f86-319">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="c7f86-320">Na przykład, należy wziąć pod uwagę następujące *TabSet* przykładu w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7f86-320">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="c7f86-321">Przykładowa aplikacja ma `ITab` interfejs, który karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="c7f86-321">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="c7f86-322">`CascadingValuesParametersTabSet` Składnik używa `TabSet` składnik, który zawiera kilka `Tab` składników:</span><span class="sxs-lookup"><span data-stu-id="c7f86-322">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="c7f86-323">Element podrzędny `Tab` składniki jawnie nie są przekazywane jako parametry do `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-323">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="c7f86-324">Zamiast tego elementu podrzędnego `Tab` składniki są częścią zawartość elementu podrzędnego `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="c7f86-324">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="c7f86-325">Jednak `TabSet` musi wiedzieć o każdym `Tab` , dzięki czemu można renderować, nagłówki i aktywną kartę. Można włączyć koordynacja bez konieczności dodatkowego kodu `TabSet` składnika *może zapewnić sama jako wartość kaskadowych* , następnie zostaje pobrana przez podrzędny `Tab` składników.</span><span class="sxs-lookup"><span data-stu-id="c7f86-325">However, the `TabSet` still needs to know about each `Tab` so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="c7f86-326">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-326">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="c7f86-327">Podrzędny `Tab` składniki przechwytywania, zawierający `TabSet` jako parametr kaskadowych, więc `Tab` składniki dodania użytkownika do `TabSet` i współrzędnych, na którym `Tab` jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="c7f86-327">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which `Tab` is active.</span></span>

<span data-ttu-id="c7f86-328">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-328">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="c7f86-329">Szablony razor</span><span class="sxs-lookup"><span data-stu-id="c7f86-329">Razor templates</span></span>

<span data-ttu-id="c7f86-330">Renderowanie fragmentów można zdefiniować przy użyciu składni szablonów Razor.</span><span class="sxs-lookup"><span data-stu-id="c7f86-330">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="c7f86-331">Szablony razor służą do definiowania fragmentu interfejsu użytkownika i założono następujący format:</span><span class="sxs-lookup"><span data-stu-id="c7f86-331">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="c7f86-332">Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="c7f86-332">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="c7f86-333">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7f86-333">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="c7f86-334">Renderowanie fragmentów zdefiniowane przy użyciu Razor szablony mogą być przekazywane jako argumenty do składników oparte na szablonach lub renderowane bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c7f86-334">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="c7f86-335">Na przykład szablony poprzedniego bezpośrednio są renderowane w następującym kodem Razor:</span><span class="sxs-lookup"><span data-stu-id="c7f86-335">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="c7f86-336">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="c7f86-336">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```
