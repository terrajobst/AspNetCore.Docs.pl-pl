---
title: Tworzenie i używanie składników Razor
author: guardrex
description: Informacje o sposobie tworzenia i używania składników Razor, w tym jak powiązać z danymi, obsługa zdarzeń i Zarządzaj cyklami życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/17/2019
uid: blazor/components
ms.openlocfilehash: 610572c232f41210c60afcae0a660cbb808be65e
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705626"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="f0d2d-103">Tworzenie i używanie składników Razor</span><span class="sxs-lookup"><span data-stu-id="f0d2d-103">Create and use Razor Components</span></span>

<span data-ttu-id="f0d2d-104">Przez [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), i [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="f0d2d-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="f0d2d-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="f0d2d-106">Zobacz [wprowadzenie](xref:blazor/get-started) tematu wstępnie wymaganych składników.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-106">See the [Get started](xref:blazor/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="f0d2d-107">Blazor aplikacje są tworzone przy użyciu *składniki*.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-107">Blazor apps are built using *components*.</span></span> <span data-ttu-id="f0d2d-108">Składnik jest niezależna fragmentu interfejsu użytkownika (UI), takich jak strony, okno dialogowe lub formularza.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="f0d2d-109">Składnik zawiera kod znaczników HTML i logika przetwarzania wymagane w celu wstrzyknięcia danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-109">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="f0d2d-110">Składniki są elastyczne i uproszczone.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-110">Components are flexible and lightweight.</span></span> <span data-ttu-id="f0d2d-111">Mogą być zagnieżdżone, ponownie i współużytkowane między projektami.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-111">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="f0d2d-112">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="f0d2d-112">Component classes</span></span>

<span data-ttu-id="f0d2d-113">Składniki są zazwyczaj implementowane w plikach Razor składnika (*.razor*) przy użyciu kombinacji C# i kod znaczników HTML (*.cshtml* pliki są używane w aplikacjach Blazor).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-113">Components are typically implemented in Razor Component files (*.razor*) using a combination of C# and HTML markup (*.cshtml* files are used in Blazor apps).</span></span>

<span data-ttu-id="f0d2d-114">Składniki można tworzyć w aplikacjach Blazor przy użyciu *.cshtml* rozszerzenie pliku, tak długo, jak pliki są identyfikowane jako pliki składnika Razor przy użyciu `_RazorComponentInclude` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-114">Components can be authored in Blazor apps using the *.cshtml* file extension as long as the files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="f0d2d-115">Na przykład aplikacja utworzona za pomocą szablonu Razor składnika Określa, że wszystkie *.cshtml* plików w obszarze *składniki* folder powinien być traktowany jako plików składników Razor:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-115">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components files:</span></span>

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="f0d2d-116">W interfejsie użytkownika dla składnika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="f0d2d-117">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="f0d2d-118">Gdy aplikacja jest kompilowana, kod znaczników HTML i C# logiki renderowania są konwertowane na klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="f0d2d-119">Nazwa wygenerowanej klasy odpowiada nazwie pliku.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="f0d2d-120">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku (więcej niż jeden `@functions` bloku jest dozwolone).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-120">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="f0d2d-121">W `@functions` bloku, stan składników (właściwości, pola) jest określony za pomocą metody obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-121">In the `@functions` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="f0d2d-122">Elementy członkowskie składnika mogą posłużyć jako część składnika przez renderowanie przy użyciu logiki C# wyrażeń, które zaczyna się `@`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-122">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="f0d2d-123">Na przykład C# pole jest renderowane przez dodanie przedrostka `@` na nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-123">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="f0d2d-124">Poniższy przykład daje w wyniku i renderuje:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-124">The following example evaluates and renders:</span></span>

* <span data-ttu-id="f0d2d-125">`_headingFontStyle` wartości właściwości CSS dla `font-style`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-125">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="f0d2d-126">`_headingText` w treści `<h1>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-126">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="f0d2d-127">Po początkowo renderowania składnik składnika generuje jej drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-127">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="f0d2d-128">Blazor następnie porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszystkie zmiany do przeglądarki w modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-128">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="f0d2d-129">Integrowanie składników aplikacji stronami Razor i programem MVC</span><span class="sxs-lookup"><span data-stu-id="f0d2d-129">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="f0d2d-130">Składniki za pomocą istniejących aplikacji stronami Razor i programem MVC.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-130">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="f0d2d-131">Nie ma potrzeby ponownego wpisywania istniejących stron lub widoków w celu używania składników Razor.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-131">There's no need to rewrite existing pages or views to use Razor Components.</span></span> <span data-ttu-id="f0d2d-132">Po wyrenderowaniu strony lub widoku składniki są prerendered&dagger; w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-132">When the page or view is rendered, components are prerendered&dagger; at the same time.</span></span> 

> [!NOTE]
> <span data-ttu-id="f0d2d-133">&dagger;Prerendering po stronie serwera jest domyślnie włączona dla Blazor po stronie serwera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-133">&dagger;Server-side prerendering is enabled for Blazor server-side apps by default.</span></span> <span data-ttu-id="f0d2d-134">Aplikacje Blazor po stronie klienta będzie obsługiwać prerendering w nadchodzącym wydaniu wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-134">Client-side Blazor apps will support prerendering in the upcoming Preview 4 release.</span></span> <span data-ttu-id="f0d2d-135">Aby uzyskać więcej informacji, zobacz [aktualizacji szablonów/oprogramowanie pośredniczące MapFallbackToPage/pliku](https://github.com/aspnet/AspNetCore/issues/8852).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-135">For more information, see [Update templates/middleware to use MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span></span>

<span data-ttu-id="f0d2d-136">Aby renderować składnika ze strony lub widok, należy użyć `RenderComponentAsync<TComponent>` metody pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-136">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="f0d2d-137">Składniki, renderowane przy użyciu widoków i stron nie są jeszcze interactive w wersji 3 (wersja zapoznawcza).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-137">Components rendered from pages and views aren't yet interactive in the Preview 3 release.</span></span> <span data-ttu-id="f0d2d-138">Na przykład naciśnięcie przycisku nie spowoduje wyzwolenia wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-138">For example, selecting a button doesn't trigger a method call.</span></span> <span data-ttu-id="f0d2d-139">Przyszłych wersji zapoznawczej będzie to ograniczenie adresów, a następnie dodaj wsparcie dla składników renderowania przy użyciu normalnych składni elementów i atrybutów.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-139">A future preview will address this limitation and add support for rendering components using the normal element and attribute syntax.</span></span>

<span data-ttu-id="f0d2d-140">Gdy widoków i stron można użyć składników, prawdą nie dotyczy.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-140">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="f0d2d-141">Składniki nie można używać w scenariuszach specyficznych dla widoku i strony, takich jak widoki częściowe i sekcji.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-141">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="f0d2d-142">Aby użyć logiki z widoku częściowego w składniku, współczynnik logiki widoku częściowego do składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-142">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

## <a name="using-components"></a><span data-ttu-id="f0d2d-143">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="f0d2d-143">Using components</span></span>

<span data-ttu-id="f0d2d-144">Składniki mogą zawierać inne składniki, deklarując je przy użyciu składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-144">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="f0d2d-145">Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-145">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="f0d2d-146">Renderuje następujące znaczniki `HeadingComponent` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-146">The following markup renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a><span data-ttu-id="f0d2d-147">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="f0d2d-147">Component parameters</span></span>

<span data-ttu-id="f0d2d-148">Składniki mogą mieć *Parametry składnika*, które są definiowane za pomocą *niepublicznych* ozdobione właściwości klasy składnika `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-148">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="f0d2d-149">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-149">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="f0d2d-150">W poniższym przykładzie `ParentComponent` ustawia wartość `Title` właściwość `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-150">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="f0d2d-151">*Składnik nadrzędny*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-151">*Parent component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5-6)]

<span data-ttu-id="f0d2d-152">*Składnik podrzędnych*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-152">*Child component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=11-12)]

## <a name="child-content"></a><span data-ttu-id="f0d2d-153">Zawartość elementu podrzędnego</span><span class="sxs-lookup"><span data-stu-id="f0d2d-153">Child content</span></span>

<span data-ttu-id="f0d2d-154">Składniki można ustawić zawartości innego składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-154">Components can set the content of another component.</span></span> <span data-ttu-id="f0d2d-155">Przypisywanie składnik udostępnia zawartości między tagami, które określają odbieranie składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-155">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="f0d2d-156">Na przykład `ParentComponent` może zapewnić zawartości dla renderowania przez składnik podrzędnych przez umieszczenie zawartość wewnątrz `<ChildComponent>` tagów.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-156">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="f0d2d-157">*Składnik nadrzędny*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-157">*Parent component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=7-8)]

<span data-ttu-id="f0d2d-158">Składnik podrzędny ma `ChildContent` właściwość, która reprezentuje `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-158">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="f0d2d-159">Wartość `ChildContent` jest umieszczony w znaczniku elementu podrzędnego, której zawartość ma być renderowany.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-159">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="f0d2d-160">W poniższym przykładzie wartość `ChildContent` jest otrzymane od składnika nadrzędnego i renderowania wewnątrz panelu Bootstrap `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-160">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="f0d2d-161">*Składnik podrzędnych*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-161">*Child component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="f0d2d-162">Odbieranie właściwość `RenderFragment` zawartość, musi nosić `ChildContent` przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-162">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="f0d2d-163">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="f0d2d-163">Data binding</span></span>

<span data-ttu-id="f0d2d-164">Powiązanie danych do składników i elementów DOM odbywa się za pomocą `bind` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-164">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="f0d2d-165">Poniższy przykład tworzy powiązanie `ItalicsCheck` właściwość z polem wyboru zaznaczone stanu:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-165">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

<span data-ttu-id="f0d2d-166">Gdy pole wyboru jest zaznaczone, a następnie wyczyszczone, wartość właściwości jest aktualizowany do `true` i `false`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-166">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="f0d2d-167">Pole wyboru jest aktualizowana w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-167">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="f0d2d-168">Ponieważ składniki renderować się po wykonaniu kod procedury obsługi zdarzeń, aktualizacje właściwości są zazwyczaj odzwierciedlane w interfejsie użytkownika od razu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-168">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="f0d2d-169">Za pomocą `bind` z `CurrentValue` właściwości (`<input bind="@CurrentValue">`) jest zasadniczo odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-169">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue">`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

<span data-ttu-id="f0d2d-170">Po wyrenderowaniu składnika `value` danego elementu wejściowego pochodzi z `CurrentValue` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-170">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="f0d2d-171">Gdy użytkownik wpisuje w polu tekstowym `onchange` zdarzenie jest generowane i `CurrentValue` zostaje ustalona zmieniona wartość.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-171">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="f0d2d-172">W rzeczywistości generowania kodu jest nieco bardziej złożone, ponieważ `bind` obsługuje kilka przypadków, w którym konwersje są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-172">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="f0d2d-173">W zasadzie `bind` kojarzy bieżącą wartość wyrażenia z `value` atrybutu i uchwytów zmiany przy użyciu zarejestrowanego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-173">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="f0d2d-174">Oprócz `onchange`, właściwości mogą być powiązane przy użyciu innych zdarzeń, takich jak `oninput` polegające na dokładniejsze o tym, co można powiązać:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-174">In addition to `onchange`, the property can be bound using other events like `oninput` by being more explicit about what to bind to:</span></span>

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

<span data-ttu-id="f0d2d-175">W odróżnieniu od `onchange`, `oninput` generowane dla każdego znaku, która jest wprowadzana do pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-175">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

<span data-ttu-id="f0d2d-176">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="f0d2d-176">**Format strings**</span></span>

<span data-ttu-id="f0d2d-177">Powiązanie danych w programach <xref:System.DateTime> ciągi formatujące.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-177">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="f0d2d-178">Inne wyrażenia formatu, takie jak waluta lub formaty liczbowe nie są dostępne w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-178">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="f0d2d-179">`format-value` Atrybut określa format daty do zastosowania do `value` z `input` elementu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-179">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="f0d2d-180">Format umożliwia również przeanalizować wartość po `onchange` wystąpi zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-180">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="f0d2d-181">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="f0d2d-181">**Component parameters**</span></span>

<span data-ttu-id="f0d2d-182">Powiązanie rozpoznaje również Parametry składnika, gdzie `bind-{property}` można powiązać wartości właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-182">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="f0d2d-183">Używa następującego składnika `ChildComponent` i wiąże `ParentYear` parametru z obiektu `Year` parametru w składniku podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-183">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="f0d2d-184">Składnik nadrzędny:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-184">Parent component:</span></span>

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

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="f0d2d-185">Element podrzędny:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-185">Child component:</span></span>

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

<span data-ttu-id="f0d2d-186">Trwa ładowanie `ParentComponent` tworzy następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-186">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="f0d2d-187">Jeśli wartość `ParentYear` zmianie właściwości, wybierając przycisk w `ParentComponent`, `Year` właściwość `ChildComponent` jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-187">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="f0d2d-188">Nowa wartość `Year` jest wyświetlana w Interfejsie użytkownika po `ParentComponent` jest rerendered:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-188">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="f0d2d-189">`Year` Parametr jest możliwej do wiązania, ponieważ ma ona towarzyszące `YearChanged` zdarzeń, który jest zgodny z typem `Year` parametru.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-189">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="f0d2d-190">Zgodnie z Konwencją `<ChildComponent bind-Year="@ParentYear" />` jest zasadniczo odpowiednikiem pisania,</span><span class="sxs-lookup"><span data-stu-id="f0d2d-190">By convention, `<ChildComponent bind-Year="@ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

<span data-ttu-id="f0d2d-191">Ogólnie rzecz biorąc, można powiązać właściwości z odpowiedni program obsługi zdarzeń, za pomocą `bind-property-event` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-191">In general, a property can be bound to a corresponding event handler using `bind-property-event` attribute.</span></span>

## <a name="event-handling"></a><span data-ttu-id="f0d2d-192">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="f0d2d-192">Event handling</span></span>

<span data-ttu-id="f0d2d-193">Składniki razor udostępniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-193">Razor Components provide event handling features.</span></span> <span data-ttu-id="f0d2d-194">Atrybut elementu HTML o nazwie `on<event>` (na przykład `onclick`, `onsubmit`) z wartością wpisane delegata składniki Razor traktuje wartość atrybutu jako program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-194">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="f0d2d-195">Nazwa atrybutu zawsze zaczyna się od `on`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-195">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="f0d2d-196">Poniższy kod wywoła `UpdateHeading` metodę po wybraniu przycisku w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-196">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="f0d2d-197">Poniższy kod wywoła `CheckboxChanged` metody, gdy pole wyboru jest zmieniana w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-197">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="f0d2d-198">Programy obsługi zdarzeń można też asynchronicznego i zwracają <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-198">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="f0d2d-199">Nie ma konieczności ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-199">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="f0d2d-200">Wyjątki są rejestrowane w momencie ich wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-200">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="f0d2d-201">Niektóre zdarzenia są dozwolone typy argumentów zdarzenia określonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-201">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="f0d2d-202">Jeśli dostęp do jednego z następujących typów zdarzeń nie jest to konieczne, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-202">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="f0d2d-203">Lista argumentów zdarzeń obsługiwanych jest:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-203">The list of supported event arguments is:</span></span>

* <span data-ttu-id="f0d2d-204">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="f0d2d-204">UIEventArgs</span></span>
* <span data-ttu-id="f0d2d-205">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="f0d2d-205">UIChangeEventArgs</span></span>
* <span data-ttu-id="f0d2d-206">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="f0d2d-206">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="f0d2d-207">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="f0d2d-207">UIMouseEventArgs</span></span>

<span data-ttu-id="f0d2d-208">Wyrażenia lambda może również służyć:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-208">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="f0d2d-209">Często jest to wygodne zamknąć dodatkowe wartości, takich jak podczas iteracji w zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-209">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="f0d2d-210">Poniższy przykład tworzy trzy przyciski wszystkich, która wywołuje metodę `UpdateHeading` przekazywaniem argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`) w przypadku wybrania w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-210">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="f0d2d-211">Czy **nie** zmienna pętli (`i`) w `for` pętli bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-211">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="f0d2d-212">W przeciwnym razie tę samą zmienną jest używany przez wszystkie wyrażenia lambda, powodując `i`przez wartość, która ma być taka sama we wszystkich wyrażenia lambda.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-212">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="f0d2d-213">Zawsze odzwierciedla jej wartość w zmiennej lokalnej (`buttonNumber` w powyższym przykładzie), a następnie użyj go.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-213">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="f0d2d-214">EventCallback</span><span class="sxs-lookup"><span data-stu-id="f0d2d-214">EventCallback</span></span>

<span data-ttu-id="f0d2d-215">Typowy scenariusz w przypadku zagnieżdżonych składników jest wymaganą do uruchomienia elementu nadrzędnego metodę składnika, gdy wystąpi zdarzenie składnika podrzędnego&mdash;na przykład, gdy `onclick` wystąpi zdarzenie w podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-215">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="f0d2d-216">Aby udostępnić zdarzenia dotyczące składników, należy użyć `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-216">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="f0d2d-217">Składnik nadrzędny można przypisać metodę wywołania zwrotnego do składnika podrzędnego `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-217">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="f0d2d-218">Składnik podrzędnych Przykładowa aplikacja pokazuje, jak przycisk `onclick` program obsługi jest skonfigurowany do otrzymywać `EventCallback` delegatem przykładowy składnik nadrzędny.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-218">The Child component in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's Parent component.</span></span> <span data-ttu-id="f0d2d-219">`EventCallback` Jest wypełniana `UIMouseEventArgs`, która jest odpowiednia dla `onclick` zdarzeń za pomocą urządzenia peryferyjne:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-219">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=5-7,17-18)]

<span data-ttu-id="f0d2d-220">Składnik nadrzędny ustawia elementu podrzędnego `EventCallback<T>` do jego `ShowMessage` metody:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-220">The Parent component sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="f0d2d-221">Po wybraniu przycisku w składniku podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-221">When the button is selected in the Child component:</span></span>

* <span data-ttu-id="f0d2d-222">Składnik nadrzędny `ShowMessage` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-222">The Parent component's `ShowMessage` method is called.</span></span> <span data-ttu-id="f0d2d-223">`messageText` są aktualizowane i wyświetlane w składniku nadrzędnym.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-223">`messageText` is updated and displayed in the Parent component.</span></span>
* <span data-ttu-id="f0d2d-224">Wywołanie `StateHasChanged` nie jest wymagane w przypadku metody wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-224">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="f0d2d-225">`StateHasChanged` wywoływana jest automatycznie rerender składnik nadrzędny tak samo, jak zdarzenia podrzędne wyzwolić składnika rerendering w procedurze obsługi zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-225">`StateHasChanged` is called automatically to rerender the Parent component, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="f0d2d-226">`EventCallback` i `EventCallback<T>` zezwolić delegatów asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-226">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="f0d2d-227">`EventCallback<T>` Zdecydowanie jest wpisane i wymaga określonych argumentów.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-227">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="f0d2d-228">`EventCallback` słabo został wpisany oraz umożliwia dowolny typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-228">`EventCallback` is weakly typed and allows any argument type.</span></span>

```chstml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; }" />

@function {
    string messageText;
}
```

<span data-ttu-id="f0d2d-229">Wywoływanie `EventCallback` lub `EventCallback<T>` z `InvokeAsync` i await <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-229">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="f0d2d-230">Użyj `EventCallback` i `EventCallback<T>` dla zdarzenia, obsługi i wiązania parametrów na części.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-230">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span> <span data-ttu-id="f0d2d-231">Nie używaj `EventCallback` i `EventCallback<T>` dla zawartość elementu podrzędnego&mdash;nadal używać `RenderFragment` i `RenderFragment<T>` dla zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-231">Don't use `EventCallback` and `EventCallback<T>` for child content&mdash;continue to use `RenderFragment` and `RenderFragment<T>` for child content.</span></span>

<span data-ttu-id="f0d2d-232">Preferuj silnie typizowaną `EventCallback<T>`, które zapewniają lepszy błąd dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-232">Prefer the strongly typed `EventCallback<T>`, which provides better error feedback to users of the component.</span></span> <span data-ttu-id="f0d2d-233">Podobnie jak inne procedury obsługi zdarzeń interfejsu użytkownika, określając parametr zdarzeń jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-233">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="f0d2d-234">Użyj `EventCallback` po żadnej wartości przekazanej do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-234">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="f0d2d-235">Przechwytywanie odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="f0d2d-235">Capture references to components</span></span>

<span data-ttu-id="f0d2d-236">Składnik odwołuje się do zapewnienia get sposób odwołania do wystąpienia składnika, dzięki czemu można wysyłać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-236">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="f0d2d-237">Do przechwycenia odwołania do składników, należy dodać `ref` atrybutu do elementu podrzędnego, a następnie zdefiniować pole o tej samej nazwie i typie jako element podrzędny.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-237">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="f0d2d-238">Po wyrenderowaniu składnika `loginDialog` pole jest wypełniane `MyLoginDialog` wystąpienie składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-238">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="f0d2d-239">Następnie można wywoływać metod .NET w instancji składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-239">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f0d2d-240">`loginDialog` Zmiennej tylko jest wypełniana po składnik jest renderowana i jego dane wyjściowe obejmują `MyLoginDialog` elementu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-240">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="f0d2d-241">Do tego momentu nie ma nic do odwołania.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-241">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="f0d2d-242">Do manipulowania odwołania do składników, po zakończeniu renderowania składnik, należy użyć `OnAfterRenderAsync` lub `OnAfterRender` metody.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-242">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="f0d2d-243">Podczas gdy przechwytywania odwołania do składników używa składni podobnie [przechwytywania odwołania do elementu](xref:blazor/javascript-interop#capture-references-to-elements), nie jest [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop) funkcji.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-243">While capturing component references uses a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="f0d2d-244">Odwołania do składników nie są przekazywane do kodu JavaScript; są one używane tylko w kodzie .NET.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-244">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="f0d2d-245">Czy **nie** umożliwia odwołania do składników mutować stan składnikach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-245">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="f0d2d-246">Zamiast tego należy użyć normalnego parametry deklaratywne, aby przekazać dane do elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-246">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="f0d2d-247">To sprawia, że podrzędnych automatycznie rerender w właściwym czasie.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-247">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="f0d2d-248">Cykl życia metody</span><span class="sxs-lookup"><span data-stu-id="f0d2d-248">Lifecycle methods</span></span>

<span data-ttu-id="f0d2d-249">`OnInitAsync` i `OnInit` wykonanie kodu, aby zainicjować składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-249">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="f0d2d-250">Aby wykonać operację asynchroniczną, użyj `OnInitAsync` i `await` — słowo kluczowe o nieudanej operacji:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-250">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="f0d2d-251">Operacja synchroniczna, można użyć `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-251">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="f0d2d-252">`OnParametersSetAsync` i `OnParametersSet` są wywoływane, gdy składnik, który otrzyma parametry od jego elementu nadrzędnego, a wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-252">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="f0d2d-253">Te metody są wykonywane po zainicjowaniu składnik, a następnie każdorazowo składnika renderowania:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-253">These methods are executed after component initialization and then each time the component is rendered:</span></span>

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

<span data-ttu-id="f0d2d-254">`OnAfterRenderAsync` i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-254">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="f0d2d-255">Odwołania do elementu, jak i składnika zostaną wypełnione na tym etapie.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-255">Element and component references are populated at this point.</span></span> <span data-ttu-id="f0d2d-256">Aby wykonać kroki dodatkowe inicjowanie przy użyciu renderowanej zawartości, takich jak aktywacja bibliotek JavaScript innych firm, które działają na renderowanych elementów DOM, należy użyć tego etapu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-256">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

<span data-ttu-id="f0d2d-257">`SetParameters` może zostać zastąpiona w celu wykonania kodu, zanim parametry są ustawione:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-257">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="f0d2d-258">Jeśli `base.SetParameters` nie jest wywoływana, niestandardowy kod może interpretować przychodzących wartości parametrów w sposób wymagane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-258">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="f0d2d-259">Na przykład przychodzące parametry nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-259">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="f0d2d-260">`ShouldRender` może zostać zastąpiona w celu pomijania odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-260">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="f0d2d-261">Jeśli implementacja zwraca `true`, interfejs użytkownika są odświeżane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-261">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="f0d2d-262">Nawet wtedy, gdy `ShouldRender` jest została zastąpiona, składnik jest zawsze wstępnie renderowane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-262">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="f0d2d-263">Usuwanie składnika z interfejsu IDisposable</span><span class="sxs-lookup"><span data-stu-id="f0d2d-263">Component disposal with IDisposable</span></span>

<span data-ttu-id="f0d2d-264">Jeśli składnik implementuje <xref:System.IDisposable>, [Dispose — metoda](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-264">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="f0d2d-265">Używa następującego składnika `@implements IDisposable` i `Dispose` metody:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-265">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="f0d2d-266">Routing</span><span class="sxs-lookup"><span data-stu-id="f0d2d-266">Routing</span></span>

<span data-ttu-id="f0d2d-267">Routing w Blazor odbywa się, podając szablonu trasy na każdej części dostępne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-267">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="f0d2d-268">Gdy plik Razor za pomocą `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-268">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="f0d2d-269">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-269">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="f0d2d-270">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-270">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="f0d2d-271">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-271">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="f0d2d-272">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="f0d2d-272">Route parameters</span></span>

<span data-ttu-id="f0d2d-273">Składniki mogą odbierać parametrów trasy z szablonu trasy dostępnego w `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-273">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="f0d2d-274">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-274">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="f0d2d-275">*Składnik parametru trasy*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-275">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

<span data-ttu-id="f0d2d-276">Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-276">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="f0d2d-277">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-277">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="f0d2d-278">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-278">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="f0d2d-279">Dziedziczenie klasy bazowej dla środowiska "związane z kodem"</span><span class="sxs-lookup"><span data-stu-id="f0d2d-279">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="f0d2d-280">Pliki składników mieszać kod znaczników HTML i C# przetwarzania kodu w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-280">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="f0d2d-281">`@inherits` Dyrektywy może służyć do zapewnienia Blazor aplikacji w środowisku "związane z kodem" oddzielający składnika znaczników, od przetwarzania kodu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-281">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="f0d2d-282">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasy bazowej, `BlazorRocksBase`w celu zapewnienia składnika właściwości i metody.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-282">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="f0d2d-283">*Składnik Blazor Rocks*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-283">*Blazor Rocks component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

<span data-ttu-id="f0d2d-284">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-284">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="f0d2d-285">Klasa bazowa powinien pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-285">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="f0d2d-286">Importowanie elementów</span><span class="sxs-lookup"><span data-stu-id="f0d2d-286">Import components</span></span>

<span data-ttu-id="f0d2d-287">Przestrzeń nazw składnika utworzone przy użyciu Razor opiera się na:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-287">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="f0d2d-288">Projekt `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-288">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="f0d2d-289">Ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-289">The path from the project root to the component.</span></span> <span data-ttu-id="f0d2d-290">Na przykład `ComponentsSample/Pages/Index.razor` znajduje się w przestrzeni nazw `ComponentsSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-290">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="f0d2d-291">Postępuj zgodnie z składniki C# nazwy zasad powiązania.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-291">Components follow C# name binding rules.</span></span> <span data-ttu-id="f0d2d-292">W przypadku właściwości *Index.razor*, wszystkie składniki w tym samym folderze *stron*i folder nadrzędny *ComponentsSample*, znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-292">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="f0d2d-293">Składniki zdefiniowane w innej przestrzeni nazw może być wprowadzana do zakresu przy użyciu firmy Razor [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-293">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="f0d2d-294">Jeśli inny składnik `NavMenu.razor`, istnieje w folderze `ComponentsSample/Shared/`, składnika mogą być używane w `Index.razor` następującym `@using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-294">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="f0d2d-295">Składniki mogą się też odwoływać przy użyciu ich w pełni kwalifikowanych nazw, która eliminuje potrzebę [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-295">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="f0d2d-296">`global::` Kwalifikacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-296">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="f0d2d-297">Importowanie elementów z aliasem `using` instrukcji (na przykład `@using Foo = Bar`) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-297">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="f0d2d-298">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-298">Partially qualified names aren't supported.</span></span> <span data-ttu-id="f0d2d-299">Na przykład dodanie `@using ComponentsSample` i odwoływanie się do `NavMenu.razor` z `<Shared.NavMenu></Shared.NavMenu>` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-299">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="razor-support"></a><span data-ttu-id="f0d2d-300">Obsługa razor</span><span class="sxs-lookup"><span data-stu-id="f0d2d-300">Razor support</span></span>

<span data-ttu-id="f0d2d-301">**Dyrektywy razor**</span><span class="sxs-lookup"><span data-stu-id="f0d2d-301">**Razor directives**</span></span>

<span data-ttu-id="f0d2d-302">W poniższej tabeli przedstawiono dyrektywy razor.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-302">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="f0d2d-303">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="f0d2d-303">Directive</span></span> | <span data-ttu-id="f0d2d-304">Opis</span><span class="sxs-lookup"><span data-stu-id="f0d2d-304">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="f0d2d-305">\@Funkcje</span><span class="sxs-lookup"><span data-stu-id="f0d2d-305">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="f0d2d-306">Dodaje C# blok kodu do składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-306">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="f0d2d-307">Implementuje interfejs dla klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-307">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="f0d2d-308">\@Inherits</span><span class="sxs-lookup"><span data-stu-id="f0d2d-308">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="f0d2d-309">Zapewnia pełną kontrolę nad klasę, która dziedziczy składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-309">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="f0d2d-310">\@wstrzykiwanie</span><span class="sxs-lookup"><span data-stu-id="f0d2d-310">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="f0d2d-311">Włącza usługi iniekcji z [kontener usługi](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-311">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f0d2d-312">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-312">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="f0d2d-313">Określa składnik układu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-313">Specifies a layout component.</span></span> <span data-ttu-id="f0d2d-314">Składniki układu są używane, aby uniknąć zduplikowania kodu i niespójności.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-314">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="f0d2d-315">\@page</span><span class="sxs-lookup"><span data-stu-id="f0d2d-315">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="f0d2d-316">Określa, że składnik obsługi żądań bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-316">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="f0d2d-317">`@page` Dyrektywy można określić za pomocą trasy i opcjonalnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-317">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="f0d2d-318">W przeciwieństwie do stron Razor `@page` dyrektywy nie musi być pierwszą dyrektywę w górnej części pliku.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-318">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="f0d2d-319">Aby uzyskać więcej informacji, zobacz [Routing](xref:blazor/routing).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-319">For more information, see [Routing](xref:blazor/routing).</span></span> |
| [<span data-ttu-id="f0d2d-320">\@za pomocą</span><span class="sxs-lookup"><span data-stu-id="f0d2d-320">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="f0d2d-321">Dodaje C# `using` dyrektywy do klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-321">Adds the C# `using` directive to the generated component class.</span></span> <span data-ttu-id="f0d2d-322">Udostępniono też wszystkich składników, które są zdefiniowane w tej przestrzeni nazw do zakresu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-322">This also brings all the components defined in that namespace into scope.</span></span> |

<span data-ttu-id="f0d2d-323">**Atrybuty warunkowe**</span><span class="sxs-lookup"><span data-stu-id="f0d2d-323">**Conditional attributes**</span></span>

<span data-ttu-id="f0d2d-324">Atrybuty warunkowe są renderowane na podstawie wartości platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-324">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="f0d2d-325">Jeśli wartość jest `false` lub `null`, ten atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-325">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="f0d2d-326">Jeśli wartość jest `true`, ten atrybut jest renderowany zminimalizowane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-326">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="f0d2d-327">W poniższym przykładzie `IsCompleted` Określa, czy `checked` jest renderowany w znacznikach formantu:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-327">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="f0d2d-328">Jeśli `IsCompleted` jest `true`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-328">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked>
```

<span data-ttu-id="f0d2d-329">Jeśli `IsCompleted` jest `false`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-329">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox">
```

<span data-ttu-id="f0d2d-330">**Dodatkowe informacje na temat Razor**</span><span class="sxs-lookup"><span data-stu-id="f0d2d-330">**Additional information on Razor**</span></span>

<span data-ttu-id="f0d2d-331">Aby uzyskać więcej informacji na temat Razor, zobacz [dokumentacja składni Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-331">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="f0d2d-332">Surowy kod HTML</span><span class="sxs-lookup"><span data-stu-id="f0d2d-332">Raw HTML</span></span>

<span data-ttu-id="f0d2d-333">Ciągi są zwykle renderowane przy użyciu modelu DOM węzły tekstowe, co oznacza, że żadnych znaczników, które mogą zawierać jest ignorowany i traktowany jako literał tekstowy.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-333">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="f0d2d-334">Aby renderować kod HTML, opakowywanie zawartości HTML w `MarkupString` wartość.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-334">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="f0d2d-335">Wartość jest analizowany jako HTML lub SVG i wstawiony DOM.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-335">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="f0d2d-336">Renderowanie surowe HTML skonstruowany z dowolnego niezaufanych źródło jest **zagrożenie dla bezpieczeństwa** i należy ich unikać!</span><span class="sxs-lookup"><span data-stu-id="f0d2d-336">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="f0d2d-337">Poniższy przykład pokazuje użycie `MarkupString` typu do dodania bloku zawartości statycznej HTML do wyniku renderowania składnika:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-337">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="f0d2d-338">Składniki oparte na szablonach</span><span class="sxs-lookup"><span data-stu-id="f0d2d-338">Templated components</span></span>

<span data-ttu-id="f0d2d-339">Oparte na szablonach składniki są składniki, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które następnie mogą być używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-339">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="f0d2d-340">Oparte na szablonach składniki umożliwiają tworzenie składników wyższego poziomu, które są bardziej wielokrotnego użytku, niż regularne składników.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-340">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="f0d2d-341">Zawiera kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-341">A couple of examples include:</span></span>

* <span data-ttu-id="f0d2d-342">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka tabeli, wierszy i stopki.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-342">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="f0d2d-343">Składnik listy, który umożliwia użytkownikowi określić szablon służący do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-343">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="f0d2d-344">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="f0d2d-344">Template parameters</span></span>

<span data-ttu-id="f0d2d-345">Oparte na szablonach składnika jest zdefiniowany, określając jeden lub więcej parametrów składnika typu `RenderFragment` lub `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-345">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="f0d2d-346">Fragment renderowania reprezentuje segment interfejsu użytkownika, który jest renderowany przez składnik.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-346">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="f0d2d-347">Fragment renderowania opcjonalnie przyjmuje parametr, który można określić, gdy jest wywoływany fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-347">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="f0d2d-348">*Części szablonu tabeli*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-348">*Table Template component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="f0d2d-349">Podczas korzystania z szablonem składnik, można określić parametry szablonu przy użyciu elementy podrzędne, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="f0d2d-349">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="f0d2d-350">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="f0d2d-350">Template context parameters</span></span>

<span data-ttu-id="f0d2d-351">Składnik argumentów typu `RenderFragment<T>` przekazany jako elementy mają niejawny parametr o nazwie `context` (na przykład w poprzednim przykładzie kodu `@context.PetId`), można jednak zmienić przy użyciu nazwy parametru `Context` atrybutu w elemencie podrzędnym element.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-351">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="f0d2d-352">W poniższym przykładzie `RowTemplate` elementu `Context` Określa atrybut `pet` parametru:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-352">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="f0d2d-353">Alternatywnie, można określić `Context` atrybutu w elemencie składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-353">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="f0d2d-354">Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-354">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="f0d2d-355">Może to być przydatne, jeśli chcesz określić nazwę zawartości parametru zawartość elementu podrzędnego niejawne (bez żadnych zawijania elementu podrzędnego).</span><span class="sxs-lookup"><span data-stu-id="f0d2d-355">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="f0d2d-356">W poniższym przykładzie `Context` atrybutu jest wyświetlany na `TableTemplate` elementu i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-356">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="f0d2d-357">Składniki z kontrolą typów ogólnych</span><span class="sxs-lookup"><span data-stu-id="f0d2d-357">Generic-typed components</span></span>

<span data-ttu-id="f0d2d-358">Oparte na szablonach składniki są często objęte wpisane.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-358">Templated components are often generically typed.</span></span> <span data-ttu-id="f0d2d-359">Na przykład ogólnego składnika szablon widoku listy może zostać użyty do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-359">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="f0d2d-360">Aby zdefiniować element ogólny, należy użyć `@typeparam` dyrektywy, aby określić parametry typu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-360">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="f0d2d-361">*Części szablonu ListView*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-361">*ListView Template component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

<span data-ttu-id="f0d2d-362">Podczas korzystania z kontrolą typów ogólnych składników, jeśli jest to możliwe jest wnioskowany parametr typu:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-362">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="f0d2d-363">W przeciwnym razie parametru typu muszą być jawnie określone za pomocą atrybutu, który odpowiada nazwie parametru typu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-363">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="f0d2d-364">W poniższym przykładzie `TItem="Pet"` Określa typ:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-364">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="f0d2d-365">Kaskadowe wartości i parametry</span><span class="sxs-lookup"><span data-stu-id="f0d2d-365">Cascading values and parameters</span></span>

<span data-ttu-id="f0d2d-366">W niektórych przypadkach jest wygodne, przepływ danych ze składnika nadrzędnego za pomocą składnika podrzędny [Parametry składnika](#component-parameters), szczególnie w przypadku kilku warstw składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-366">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="f0d2d-367">Kaskadowe wartości i parametry rozwiązują ten problem, zapewniając wygodny sposób w celu Podaj wartość, aby wszystkie jego elementy potomne składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-367">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="f0d2d-368">Kaskadowe wartości i parametry oferują również podejście składników do zapewnienia koordynacji.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-368">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="f0d2d-369">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="f0d2d-369">Theme example</span></span>

<span data-ttu-id="f0d2d-370">W następującym *motyw* przykład z przykładowej aplikacji `ThemeInfo` klasa określa informacje o motywie przepływ w dół hierarchii składnika tak, aby udostępnić wszystkie przyciski w ramach danego część aplikacji, ten styl.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-370">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="f0d2d-371">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-371">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="f0d2d-372">Składnik nadrzędny można podać wartość kaskadowych za pomocą składnika kaskadowa wartość.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-372">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="f0d2d-373">Składnik wartości kaskadowych otacza poddrzewo hierarchii składników i dostarcza pojedynczą wartość dla wszystkich składników w ramach tego poddrzewa.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-373">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="f0d2d-374">Na przykład przykładowa aplikacja określa informacje o motywie (`ThemeInfo`) w jednej aplikacji układów jako parametr kaskadowych dla wszystkich składników, które tworzą treści układ `@Body` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-374">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="f0d2d-375">`ButtonClass` jest przypisywana wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-375">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="f0d2d-376">Dowolny składnik podrzędny mogą używać tej właściwości, za pośrednictwem `ThemeInfo` cascading obiektu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-376">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="f0d2d-377">*Kaskadowe składnik wartości parametrów układ*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-377">*Cascading Values Parameters Layout component*:</span></span>

```cshtml
@inherits LayoutComponentBase
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
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="f0d2d-378">Zapewnienie korzystania z wartości kaskadowych, składniki deklarować parametrów kaskadowych przy użyciu `[CascadingParameter]` atrybut lub na podstawie wartości ciągu nazwy:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-378">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="f0d2d-379">Powiązanie z wartością nazwy ciągu jest istotne, jeśli masz wiele kaskadowych wartości tego samego typu i potrzebujesz odróżnić je w ramach tej samej poddrzewo.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-379">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="f0d2d-380">Kaskadowe wartości są powiązane kaskadowych parametry według typu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-380">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="f0d2d-381">W przykładowej aplikacji składnika kaskadowych wartości parametrów motyw wiąże `ThemeInfo` kaskadowa wartość parametru kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-381">In the sample app, the Cascading Values Parameters Theme component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="f0d2d-382">Parametr służy do ustawiania klasę CSS dla jednego z przyciski wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-382">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="f0d2d-383">*Kaskadowe składnik wartości parametrów motyw*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-383">*Cascading Values Parameters Theme component*:</span></span>

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
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="f0d2d-384">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="f0d2d-384">TabSet example</span></span>

<span data-ttu-id="f0d2d-385">Parametry kaskadowych również włączyć składników do współpracy w całej hierarchii składnika.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-385">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="f0d2d-386">Na przykład, należy wziąć pod uwagę następujące *TabSet* przykładu w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-386">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="f0d2d-387">Przykładowa aplikacja ma `ITab` interfejs, który karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-387">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="f0d2d-388">Składnik kaskadowych wartości parametrów TabSet używa ustawienia karty składnik, który zawiera kilka składników karty:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-388">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="f0d2d-389">Składniki karty podrzędnej jawnie nie są przekazywane jako parametry można ustawić kartę.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-389">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="f0d2d-390">Zamiast tego składniki karty podrzędnej są częścią zawartość elementu podrzędnego zestawu kartę.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-390">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="f0d2d-391">Jednak ustawienia karty nadal musi wiedzieć o poszczególnych składników karty, dzięki czemu można renderować, nagłówki i aktywną kartę. Włączyć koordynacja bez konieczności dodatkowego kodu, Ustaw kartę składnika *może zapewnić sama jako wartość kaskadowych* , następnie zostaje pobrana przez podrzędny składniki kartę.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-391">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="f0d2d-392">*Składnik TabSet*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-392">*TabSet component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="f0d2d-393">Podrzędny przechwytywania składniki kartę zawierającego ustawienia karty jako parametr kaskadowych, więc składniki kartę dodają same siebie do ustawienia karty i współrzędnych karty, które jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-393">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="f0d2d-394">*Karta składników*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-394">*Tab component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="f0d2d-395">Szablony razor</span><span class="sxs-lookup"><span data-stu-id="f0d2d-395">Razor templates</span></span>

<span data-ttu-id="f0d2d-396">Renderowanie fragmentów można zdefiniować przy użyciu składni szablonów Razor.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-396">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="f0d2d-397">Szablony razor służą do definiowania fragmentu interfejsu użytkownika i założono następujący format:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-397">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="f0d2d-398">Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-398">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="f0d2d-399">*Składnik Szablony razor*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-399">*Razor Templates component*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="f0d2d-400">Renderowanie fragmentów zdefiniowane przy użyciu Razor szablony mogą być przekazywane jako argumenty do składników oparte na szablonach lub renderowane bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-400">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="f0d2d-401">Na przykład szablony poprzedniego bezpośrednio są renderowane w następującym kodem Razor:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-401">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="f0d2d-402">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-402">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="f0d2d-403">Ręczne logiki RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="f0d2d-403">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="f0d2d-404">`Microsoft.AspNetCore.Components.RenderTree` udostępnia metody do manipulowania składników i elementów, w tym ręcznie w kompilowanie składników C# kodu.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-404">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="f0d2d-405">Korzystanie z `RenderTreeBuilder` do tworzenia składników to zaawansowany scenariusz.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-405">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="f0d2d-406">Źle sformułowane składników (na przykład tag niezamknięty znaczników) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-406">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="f0d2d-407">Należy wziąć pod uwagę następujący składnik Pet szczegóły mogą być ręcznie wbudowane w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-407">Consider the following Pet Details component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="f0d2d-408">W poniższym przykładzie pętli w `CreateComponent` metoda generuje trzy składniki Pet szczegóły.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-408">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="f0d2d-409">Podczas wywoływania `RenderTreeBuilder` metody tworzenia składników (`OpenComponent` i `AddAttribute`), numery sekwencyjne są numery wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-409">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="f0d2d-410">Algorytm różnica Blazor opiera się na numery sekwencyjne distinct wierszy kodu, a nie odrębne wywołania wywołania.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-410">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="f0d2d-411">Podczas tworzenia składnika za pomocą `RenderTreeBuilder` metody, umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="f0d2d-411">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="f0d2d-412">**Za pomocą obliczeń lub licznika do generowania numer sekwencyjny może prowadzić do pogorszenia wydajności.**</span><span class="sxs-lookup"><span data-stu-id="f0d2d-412">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span>

<span data-ttu-id="f0d2d-413">*Wbudowane składnika zawartość*:</span><span class="sxs-lookup"><span data-stu-id="f0d2d-413">*Built Content component*:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```
