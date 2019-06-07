---
title: Tworzenie i używanie składników Razor
author: guardrex
description: Informacje o sposobie tworzenia i używania składników Razor, w tym jak powiązać z danymi, obsługa zdarzeń i Zarządzaj cyklami życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/05/2019
uid: blazor/components
ms.openlocfilehash: fdd755a245b0ef9697b500c734a44fac8942f068
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750143"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="43f72-103">Tworzenie i używanie składników Razor</span><span class="sxs-lookup"><span data-stu-id="43f72-103">Create and use Razor components</span></span>

<span data-ttu-id="43f72-104">Przez [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), i [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="43f72-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="43f72-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43f72-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="43f72-106">Blazor aplikacje są tworzone przy użyciu *składniki*.</span><span class="sxs-lookup"><span data-stu-id="43f72-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="43f72-107">Składnik jest niezależna fragmentu interfejsu użytkownika (UI), takich jak strony, okno dialogowe lub formularza.</span><span class="sxs-lookup"><span data-stu-id="43f72-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="43f72-108">Składnik zawiera kod znaczników HTML i logika przetwarzania wymagane w celu wstrzyknięcia danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="43f72-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="43f72-109">Składniki są elastyczne i uproszczone.</span><span class="sxs-lookup"><span data-stu-id="43f72-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="43f72-110">Mogą być zagnieżdżone, ponownie i współużytkowane między projektami.</span><span class="sxs-lookup"><span data-stu-id="43f72-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="43f72-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="43f72-111">Component classes</span></span>

<span data-ttu-id="43f72-112">Składniki są implementowane w [Razor](xref:mvc/views/razor) pliki składników ( *.razor*) przy użyciu kombinacji C# i kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="43f72-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span>

<span data-ttu-id="43f72-113">Składniki mogą być tworzone za pomocą *.cshtml* rozszerzenie pliku, tak długo, jak pliki są identyfikowane jako pliki składnika Razor przy użyciu `_RazorComponentInclude` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="43f72-113">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="43f72-114">Na przykład aplikację, która określa, że wszystkie *.cshtml* plików w obszarze *stron* folder powinien być traktowany jako plików składników Razor:</span><span class="sxs-lookup"><span data-stu-id="43f72-114">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="43f72-115">W interfejsie użytkownika dla składnika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="43f72-115">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="43f72-116">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="43f72-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="43f72-117">Gdy aplikacja jest kompilowana, kod znaczników HTML i C# logiki renderowania są konwertowane na klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-117">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="43f72-118">Nazwa wygenerowanej klasy odpowiada nazwie pliku.</span><span class="sxs-lookup"><span data-stu-id="43f72-118">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="43f72-119">Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku (więcej niż jeden `@functions` bloku jest dozwolone).</span><span class="sxs-lookup"><span data-stu-id="43f72-119">Members of the component class are defined in an `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="43f72-120">W `@functions` bloku, stan składników (właściwości, pola) jest określony za pomocą metody obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="43f72-120">In the `@functions` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="43f72-121">Elementy członkowskie składnika mogą posłużyć jako część składnika przez renderowanie przy użyciu logiki C# wyrażeń, które zaczyna się `@`.</span><span class="sxs-lookup"><span data-stu-id="43f72-121">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="43f72-122">Na przykład C# pole jest renderowane przez dodanie przedrostka `@` na nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="43f72-122">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="43f72-123">Poniższy przykład daje w wyniku i renderuje:</span><span class="sxs-lookup"><span data-stu-id="43f72-123">The following example evaluates and renders:</span></span>

* <span data-ttu-id="43f72-124">`_headingFontStyle` wartości właściwości CSS dla `font-style`.</span><span class="sxs-lookup"><span data-stu-id="43f72-124">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="43f72-125">`_headingText` w treści `<h1>` elementu.</span><span class="sxs-lookup"><span data-stu-id="43f72-125">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="43f72-126">Po początkowo renderowania składnik składnika generuje jej drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="43f72-126">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="43f72-127">Blazor następnie porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszystkie zmiany do przeglądarki w modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="43f72-127">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="43f72-128">Składniki są zwykłe C# klasy i można umieścić w dowolnym miejscu w obrębie projektu.</span><span class="sxs-lookup"><span data-stu-id="43f72-128">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="43f72-129">Składniki, które zwykle tworzą stron sieci Web znajdują się w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="43f72-129">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="43f72-130">Składniki strony inne niż często są umieszczane w *Shared* folder lub folder niestandardowy dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="43f72-130">Non-page components are frequently placed into the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="43f72-131">Aby korzystać z folderu niestandardowego, Dodaj niestandardowego folderu obszaru nazw do składnika nadrzędnego lub w aplikacji  *\_Imports.razor* pliku.</span><span class="sxs-lookup"><span data-stu-id="43f72-131">To use a custom folder, either add the custom folder's namespace to the parent component or to the app's *\_Imports.razor* file.</span></span> <span data-ttu-id="43f72-132">Na przykład następująca przestrzeń nazw sprawia, że składniki *składniki* dostępne w przypadku aplikacji głównej przestrzeni nazw jest folder `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="43f72-132">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="43f72-133">Integrowanie składników aplikacji stronami Razor i programem MVC</span><span class="sxs-lookup"><span data-stu-id="43f72-133">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="43f72-134">Składniki za pomocą istniejących aplikacji stronami Razor i programem MVC.</span><span class="sxs-lookup"><span data-stu-id="43f72-134">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="43f72-135">Nie ma potrzeby ponownego wpisywania istniejących stron lub widoków w celu używania składników Razor.</span><span class="sxs-lookup"><span data-stu-id="43f72-135">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="43f72-136">Po wyrenderowaniu strony lub widoku składniki są prerendered&dagger; w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="43f72-136">When the page or view is rendered, components are prerendered&dagger; at the same time.</span></span> 

> [!NOTE]
> <span data-ttu-id="43f72-137">&dagger;Prerendering po stronie serwera jest domyślnie włączona dla Blazor po stronie serwera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43f72-137">&dagger;Server-side prerendering is enabled for Blazor server-side apps by default.</span></span> <span data-ttu-id="43f72-138">Aplikacje Blazor po stronie klienta będzie obsługiwać prerendering w nadchodzącej wersji 5 (wersja zapoznawcza).</span><span class="sxs-lookup"><span data-stu-id="43f72-138">Client-side Blazor apps will support prerendering in the upcoming Preview 5 release.</span></span> <span data-ttu-id="43f72-139">Aby uzyskać więcej informacji, zobacz [aktualizacji szablonów/oprogramowanie pośredniczące MapFallbackToPage/pliku](https://github.com/aspnet/AspNetCore/issues/8852).</span><span class="sxs-lookup"><span data-stu-id="43f72-139">For more information, see [Update templates/middleware to use MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span></span>

<span data-ttu-id="43f72-140">Aby renderować składnika ze strony lub widok, należy użyć `RenderComponentAsync<TComponent>` metody pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="43f72-140">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="43f72-141">Gdy widoków i stron można użyć składników, prawdą nie dotyczy.</span><span class="sxs-lookup"><span data-stu-id="43f72-141">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="43f72-142">Składniki nie można używać w scenariuszach specyficznych dla widoku i strony, takich jak widoki częściowe i sekcji.</span><span class="sxs-lookup"><span data-stu-id="43f72-142">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="43f72-143">Aby użyć logiki z widoku częściowego w składniku, współczynnik logiki widoku częściowego do składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-143">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="43f72-144">Aby uzyskać więcej informacji na temat sposobu Wyrenderowana i składnika, stan składników odbywa się w aplikacji po stronie serwera Blazor, zobacz <xref:blazor/hosting-models> artykułu.</span><span class="sxs-lookup"><span data-stu-id="43f72-144">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="using-components"></a><span data-ttu-id="43f72-145">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="43f72-145">Using components</span></span>

<span data-ttu-id="43f72-146">Składniki mogą zawierać inne składniki, deklarując je przy użyciu składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="43f72-146">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="43f72-147">Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-147">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="43f72-148">Następujące znaczniki w *Index.razor* renderuje `HeadingComponent` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="43f72-148">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="43f72-149">*Components/HeadingComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="43f72-149">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

## <a name="component-parameters"></a><span data-ttu-id="43f72-150">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="43f72-150">Component parameters</span></span>

<span data-ttu-id="43f72-151">Składniki mogą mieć *Parametry składnika*, które są definiowane za pomocą *niepublicznych* właściwości klasy składnika za pomocą `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="43f72-151">Components can have *component parameters*, which are defined using *non-public* properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="43f72-152">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="43f72-152">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="43f72-153">W poniższym przykładzie `ParentComponent` ustawia wartość `Title` właściwość `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="43f72-153">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="43f72-154">*Pages/ParentComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="43f72-154">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

<span data-ttu-id="43f72-155">*Components/ChildComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="43f72-155">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

## <a name="child-content"></a><span data-ttu-id="43f72-156">Zawartość elementu podrzędnego</span><span class="sxs-lookup"><span data-stu-id="43f72-156">Child content</span></span>

<span data-ttu-id="43f72-157">Składniki można ustawić zawartości innego składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-157">Components can set the content of another component.</span></span> <span data-ttu-id="43f72-158">Przypisywanie składnik udostępnia zawartości między tagami, które określają odbieranie składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-158">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="43f72-159">Na przykład `ParentComponent` może zapewnić zawartości dla renderowania przez składnik podrzędnych przez umieszczenie zawartość wewnątrz `<ChildComponent>` tagów.</span><span class="sxs-lookup"><span data-stu-id="43f72-159">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="43f72-160">*Pages/ParentComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="43f72-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

<span data-ttu-id="43f72-161">Składnik podrzędny ma `ChildContent` właściwość, która reprezentuje `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="43f72-161">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="43f72-162">Wartość `ChildContent` jest umieszczony w znaczniku elementu podrzędnego, której zawartość ma być renderowany.</span><span class="sxs-lookup"><span data-stu-id="43f72-162">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="43f72-163">W poniższym przykładzie wartość `ChildContent` jest otrzymane od składnika nadrzędnego i renderowania wewnątrz panelu Bootstrap `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="43f72-163">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="43f72-164">*Components/ChildComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="43f72-164">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="43f72-165">Odbieranie właściwość `RenderFragment` zawartość, musi nosić `ChildContent` przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="43f72-165">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="43f72-166">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="43f72-166">Data binding</span></span>

<span data-ttu-id="43f72-167">Powiązanie danych do składników i elementów DOM odbywa się za pomocą `bind` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="43f72-167">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="43f72-168">Poniższy przykład tworzy powiązanie `_italicsCheck` pole do pola wyboru zaznaczone stanu:</span><span class="sxs-lookup"><span data-stu-id="43f72-168">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck" />
```

<span data-ttu-id="43f72-169">Gdy pole wyboru jest zaznaczone, a następnie wyczyszczone, wartość właściwości jest aktualizowany do `true` i `false`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="43f72-169">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="43f72-170">Pole wyboru jest aktualizowana w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="43f72-170">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="43f72-171">Ponieważ składniki renderować się po wykonaniu kod procedury obsługi zdarzeń, aktualizacje właściwości są zazwyczaj odzwierciedlane w interfejsie użytkownika od razu.</span><span class="sxs-lookup"><span data-stu-id="43f72-171">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="43f72-172">Za pomocą `bind` z `CurrentValue` właściwości (`<input bind="@CurrentValue" />`) jest zasadniczo odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="43f72-172">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="43f72-173">Po wyrenderowaniu składnika `value` danego elementu wejściowego pochodzi z `CurrentValue` właściwości.</span><span class="sxs-lookup"><span data-stu-id="43f72-173">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="43f72-174">Gdy użytkownik wpisuje w polu tekstowym `onchange` zdarzenie jest generowane i `CurrentValue` zostaje ustalona zmieniona wartość.</span><span class="sxs-lookup"><span data-stu-id="43f72-174">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="43f72-175">W rzeczywistości generowania kodu jest nieco bardziej złożone, ponieważ `bind` obsługuje kilka przypadków, w którym konwersje są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="43f72-175">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="43f72-176">W zasadzie `bind` kojarzy bieżącą wartość wyrażenia z `value` atrybutu i uchwytów zmiany przy użyciu zarejestrowanego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="43f72-176">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="43f72-177">Oprócz `onchange`, właściwości mogą być powiązane przy użyciu innych zdarzeń, takich jak `oninput` polegające na dokładniejsze o tym, co można powiązać:</span><span class="sxs-lookup"><span data-stu-id="43f72-177">In addition to `onchange`, the property can be bound using other events like `oninput` by being more explicit about what to bind to:</span></span>

```cshtml
<input type="text" bind-value-oninput="@CurrentValue" />
```

<span data-ttu-id="43f72-178">W odróżnieniu od `onchange`, `oninput` generowane dla każdego znaku, która jest wprowadzana do pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="43f72-178">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

<span data-ttu-id="43f72-179">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="43f72-179">**Format strings**</span></span>

<span data-ttu-id="43f72-180">Powiązanie danych w programach <xref:System.DateTime> ciągi formatujące.</span><span class="sxs-lookup"><span data-stu-id="43f72-180">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="43f72-181">Inne wyrażenia formatu, takie jak waluta lub formaty liczbowe nie są dostępne w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="43f72-181">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="43f72-182">`format-value` Atrybut określa format daty do zastosowania do `value` z `input` elementu.</span><span class="sxs-lookup"><span data-stu-id="43f72-182">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="43f72-183">Format umożliwia również przeanalizować wartość po `onchange` wystąpi zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="43f72-183">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="43f72-184">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="43f72-184">**Component parameters**</span></span>

<span data-ttu-id="43f72-185">Powiązanie rozpoznaje również Parametry składnika, gdzie `bind-{property}` można powiązać wartości właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="43f72-185">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="43f72-186">Używa następującego składnika `ChildComponent` i wiąże `ParentYear` parametru z obiektu `Year` parametru w składniku podrzędne:</span><span class="sxs-lookup"><span data-stu-id="43f72-186">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="43f72-187">Składnik nadrzędny:</span><span class="sxs-lookup"><span data-stu-id="43f72-187">Parent component:</span></span>

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

<span data-ttu-id="43f72-188">Element podrzędny:</span><span class="sxs-lookup"><span data-stu-id="43f72-188">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="43f72-189">`EventCallback<T>` została wyjaśniona w [EventCallback](#eventcallback) sekcji.</span><span class="sxs-lookup"><span data-stu-id="43f72-189">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="43f72-190">Trwa ładowanie `ParentComponent` tworzy następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="43f72-190">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="43f72-191">Jeśli wartość `ParentYear` zmianie właściwości, wybierając przycisk w `ParentComponent`, `Year` właściwość `ChildComponent` jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="43f72-191">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="43f72-192">Nowa wartość `Year` jest wyświetlana w Interfejsie użytkownika po `ParentComponent` jest rerendered:</span><span class="sxs-lookup"><span data-stu-id="43f72-192">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="43f72-193">`Year` Parametr jest możliwej do wiązania, ponieważ ma ona towarzyszące `YearChanged` zdarzeń, który jest zgodny z typem `Year` parametru.</span><span class="sxs-lookup"><span data-stu-id="43f72-193">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="43f72-194">Zgodnie z Konwencją `<ChildComponent bind-Year="@ParentYear" />` jest zasadniczo odpowiednikiem pisania,</span><span class="sxs-lookup"><span data-stu-id="43f72-194">By convention, `<ChildComponent bind-Year="@ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
<ChildComponent bind-Year-YearChanged="@ParentYear" />
```

<span data-ttu-id="43f72-195">Ogólnie rzecz biorąc, można powiązać właściwości z odpowiedni program obsługi zdarzeń, za pomocą `bind-property-event` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="43f72-195">In general, a property can be bound to a corresponding event handler using `bind-property-event` attribute.</span></span>

## <a name="event-handling"></a><span data-ttu-id="43f72-196">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="43f72-196">Event handling</span></span>

<span data-ttu-id="43f72-197">Składniki razor udostępniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="43f72-197">Razor components provide event handling features.</span></span> <span data-ttu-id="43f72-198">Atrybut elementu HTML o nazwie `on<event>` (na przykład `onclick`, `onsubmit`) z wartością wpisane delegata składniki Razor traktuje wartość atrybutu jako program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="43f72-198">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="43f72-199">Nazwa atrybutu zawsze zaczyna się od `on`.</span><span class="sxs-lookup"><span data-stu-id="43f72-199">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="43f72-200">Poniższy kod wywoła `UpdateHeading` metodę po wybraniu przycisku w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="43f72-200">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="43f72-201">Poniższy kod wywoła `CheckboxChanged` metody, gdy pole wyboru jest zmieniana w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="43f72-201">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="43f72-202">Programy obsługi zdarzeń można też asynchronicznego i zwracają <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="43f72-202">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="43f72-203">Nie ma konieczności ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="43f72-203">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="43f72-204">Wyjątki są rejestrowane w momencie ich wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="43f72-204">Exceptions are logged when they occur.</span></span>

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

<span data-ttu-id="43f72-205">Niektóre zdarzenia są dozwolone typy argumentów zdarzenia określonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="43f72-205">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="43f72-206">Jeśli dostęp do jednego z następujących typów zdarzeń nie jest to konieczne, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="43f72-206">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="43f72-207">Lista argumentów zdarzeń obsługiwanych jest:</span><span class="sxs-lookup"><span data-stu-id="43f72-207">The list of supported event arguments is:</span></span>

* <span data-ttu-id="43f72-208">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="43f72-208">UIEventArgs</span></span>
* <span data-ttu-id="43f72-209">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="43f72-209">UIChangeEventArgs</span></span>
* <span data-ttu-id="43f72-210">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="43f72-210">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="43f72-211">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="43f72-211">UIMouseEventArgs</span></span>

<span data-ttu-id="43f72-212">Wyrażenia lambda może również służyć:</span><span class="sxs-lookup"><span data-stu-id="43f72-212">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="43f72-213">Często jest to wygodne zamknąć dodatkowe wartości, takich jak podczas iteracji w zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="43f72-213">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="43f72-214">Poniższy przykład tworzy trzy przyciski wszystkich, która wywołuje metodę `UpdateHeading` przekazywaniem argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`) w przypadku wybrania w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="43f72-214">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="43f72-215">Czy **nie** zmienna pętli (`i`) w `for` pętli bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="43f72-215">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="43f72-216">W przeciwnym razie tę samą zmienną jest używany przez wszystkie wyrażenia lambda, powodując `i`przez wartość, która ma być taka sama we wszystkich wyrażenia lambda.</span><span class="sxs-lookup"><span data-stu-id="43f72-216">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="43f72-217">Zawsze odzwierciedla jej wartość w zmiennej lokalnej (`buttonNumber` w powyższym przykładzie), a następnie użyj go.</span><span class="sxs-lookup"><span data-stu-id="43f72-217">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="43f72-218">EventCallback</span><span class="sxs-lookup"><span data-stu-id="43f72-218">EventCallback</span></span>

<span data-ttu-id="43f72-219">Typowy scenariusz w przypadku zagnieżdżonych składników jest wymaganą do uruchomienia elementu nadrzędnego metodę składnika, gdy wystąpi zdarzenie składnika podrzędnego&mdash;na przykład, gdy `onclick` wystąpi zdarzenie w podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="43f72-219">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="43f72-220">Aby udostępnić zdarzenia dotyczące składników, należy użyć `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="43f72-220">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="43f72-221">Składnik nadrzędny można przypisać metodę wywołania zwrotnego do składnika podrzędnego `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="43f72-221">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="43f72-222">Składnik podrzędnych Przykładowa aplikacja pokazuje, jak przycisk `onclick` program obsługi jest skonfigurowany do otrzymywać `EventCallback` delegatem przykładowy składnik nadrzędny.</span><span class="sxs-lookup"><span data-stu-id="43f72-222">The Child component in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's Parent component.</span></span> <span data-ttu-id="43f72-223">`EventCallback` Jest wypełniana `UIMouseEventArgs`, która jest odpowiednia dla `onclick` zdarzeń za pomocą urządzenia peryferyjne:</span><span class="sxs-lookup"><span data-stu-id="43f72-223">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="43f72-224">Składnik nadrzędny ustawia elementu podrzędnego `EventCallback<T>` do jego `ShowMessage` metody:</span><span class="sxs-lookup"><span data-stu-id="43f72-224">The Parent component sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="43f72-225">Po wybraniu przycisku w składniku podrzędne:</span><span class="sxs-lookup"><span data-stu-id="43f72-225">When the button is selected in the Child component:</span></span>

* <span data-ttu-id="43f72-226">Składnik nadrzędny `ShowMessage` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="43f72-226">The Parent component's `ShowMessage` method is called.</span></span> <span data-ttu-id="43f72-227">`messageText` są aktualizowane i wyświetlane w składniku nadrzędnym.</span><span class="sxs-lookup"><span data-stu-id="43f72-227">`messageText` is updated and displayed in the Parent component.</span></span>
* <span data-ttu-id="43f72-228">Wywołanie `StateHasChanged` nie jest wymagane w przypadku metody wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="43f72-228">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="43f72-229">`StateHasChanged` wywoływana jest automatycznie rerender składnik nadrzędny tak samo, jak zdarzenia podrzędne wyzwolić składnika rerendering w procedurze obsługi zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="43f72-229">`StateHasChanged` is called automatically to rerender the Parent component, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="43f72-230">`EventCallback` i `EventCallback<T>` zezwolić delegatów asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="43f72-230">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="43f72-231">`EventCallback<T>` Zdecydowanie jest wpisane i wymaga określonych argumentów.</span><span class="sxs-lookup"><span data-stu-id="43f72-231">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="43f72-232">`EventCallback` słabo został wpisany oraz umożliwia dowolny typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="43f72-232">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@functions {
    private string messageText;
}
```

<span data-ttu-id="43f72-233">Wywoływanie `EventCallback` lub `EventCallback<T>` z `InvokeAsync` i await <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="43f72-233">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="43f72-234">Użyj `EventCallback` i `EventCallback<T>` dla zdarzenia, obsługi i wiązania parametrów na części.</span><span class="sxs-lookup"><span data-stu-id="43f72-234">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span> <span data-ttu-id="43f72-235">Nie używaj `EventCallback` i `EventCallback<T>` dla zawartość elementu podrzędnego&mdash;nadal używać `RenderFragment` i `RenderFragment<T>` dla zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="43f72-235">Don't use `EventCallback` and `EventCallback<T>` for child content&mdash;continue to use `RenderFragment` and `RenderFragment<T>` for child content.</span></span>

<span data-ttu-id="43f72-236">Preferuj silnie typizowaną `EventCallback<T>`, które zapewniają lepszy błąd dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-236">Prefer the strongly typed `EventCallback<T>`, which provides better error feedback to users of the component.</span></span> <span data-ttu-id="43f72-237">Podobnie jak inne procedury obsługi zdarzeń interfejsu użytkownika, określając parametr zdarzeń jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="43f72-237">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="43f72-238">Użyj `EventCallback` po żadnej wartości przekazanej do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="43f72-238">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="43f72-239">Przechwytywanie odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="43f72-239">Capture references to components</span></span>

<span data-ttu-id="43f72-240">Odwołania do składników zapewnia sposób odwołania wystąpienie składnika, dzięki czemu można wysyłać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`.</span><span class="sxs-lookup"><span data-stu-id="43f72-240">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="43f72-241">Do przechwycenia odwołania do składników, należy dodać `ref` atrybutu do elementu podrzędnego, a następnie zdefiniować pole o tej samej nazwie i typie jako element podrzędny.</span><span class="sxs-lookup"><span data-stu-id="43f72-241">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

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

<span data-ttu-id="43f72-242">Po wyrenderowaniu składnika `loginDialog` pole jest wypełniane `MyLoginDialog` wystąpienie składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="43f72-242">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="43f72-243">Następnie można wywoływać metod .NET w instancji składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-243">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="43f72-244">`loginDialog` Zmiennej tylko jest wypełniana po składnik jest renderowana i jego dane wyjściowe obejmują `MyLoginDialog` elementu.</span><span class="sxs-lookup"><span data-stu-id="43f72-244">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="43f72-245">Do tego momentu nie ma nic do odwołania.</span><span class="sxs-lookup"><span data-stu-id="43f72-245">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="43f72-246">Do manipulowania odwołania do składników, po zakończeniu renderowania składnik, należy użyć `OnAfterRenderAsync` lub `OnAfterRender` metody.</span><span class="sxs-lookup"><span data-stu-id="43f72-246">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="43f72-247">Podczas przechwytywania odwołania do składników użyj podobnej składni do [przechwytywania odwołania do elementu](xref:blazor/javascript-interop#capture-references-to-elements), nie jest [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop) funkcji.</span><span class="sxs-lookup"><span data-stu-id="43f72-247">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="43f72-248">Odwołania do składników nie są przekazywane do kodu w języku JavaScript&mdash;są używane tylko w kodzie .NET.</span><span class="sxs-lookup"><span data-stu-id="43f72-248">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="43f72-249">Czy **nie** umożliwia odwołania do składników mutować stan składnikach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="43f72-249">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="43f72-250">Zamiast tego należy użyć normalnego parametry deklaratywne, aby przekazać dane do elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="43f72-250">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="43f72-251">To sprawia, że podrzędnych automatycznie rerender w właściwym czasie.</span><span class="sxs-lookup"><span data-stu-id="43f72-251">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="43f72-252">Cykl życia metody</span><span class="sxs-lookup"><span data-stu-id="43f72-252">Lifecycle methods</span></span>

<span data-ttu-id="43f72-253">`OnInitAsync` i `OnInit` wykonanie kodu, aby zainicjować składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-253">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="43f72-254">Aby wykonać operację asynchroniczną, użyj `OnInitAsync` i `await` — słowo kluczowe o nieudanej operacji:</span><span class="sxs-lookup"><span data-stu-id="43f72-254">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="43f72-255">Operacja synchroniczna, można użyć `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="43f72-255">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="43f72-256">`OnParametersSetAsync` i `OnParametersSet` są wywoływane, gdy składnik, który otrzyma parametry od jego elementu nadrzędnego, a wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="43f72-256">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="43f72-257">Te metody są wykonywane po zainicjowaniu składnika i każdym składniku jest renderowany:</span><span class="sxs-lookup"><span data-stu-id="43f72-257">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="43f72-258">`OnAfterRenderAsync` i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-258">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="43f72-259">Odwołania do elementu, jak i składnika zostaną wypełnione na tym etapie.</span><span class="sxs-lookup"><span data-stu-id="43f72-259">Element and component references are populated at this point.</span></span> <span data-ttu-id="43f72-260">Aby wykonać kroki dodatkowe inicjowanie przy użyciu renderowanej zawartości, takich jak aktywacja bibliotek JavaScript innych firm, które działają na renderowanych elementów DOM, należy użyć tego etapu.</span><span class="sxs-lookup"><span data-stu-id="43f72-260">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

<span data-ttu-id="43f72-261">`SetParameters` może zostać zastąpiona w celu wykonania kodu, zanim parametry są ustawione:</span><span class="sxs-lookup"><span data-stu-id="43f72-261">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="43f72-262">Jeśli `base.SetParameters` nie jest wywoływana, niestandardowy kod może interpretować przychodzących wartości parametrów w sposób wymagane.</span><span class="sxs-lookup"><span data-stu-id="43f72-262">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="43f72-263">Na przykład przychodzące parametry nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="43f72-263">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="43f72-264">`ShouldRender` może zostać zastąpiona w celu pomijania odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="43f72-264">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="43f72-265">Jeśli implementacja zwraca `true`, interfejs użytkownika są odświeżane.</span><span class="sxs-lookup"><span data-stu-id="43f72-265">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="43f72-266">Nawet wtedy, gdy `ShouldRender` jest została zastąpiona, składnik jest zawsze wstępnie renderowane.</span><span class="sxs-lookup"><span data-stu-id="43f72-266">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="43f72-267">Usuwanie składnika z interfejsu IDisposable</span><span class="sxs-lookup"><span data-stu-id="43f72-267">Component disposal with IDisposable</span></span>

<span data-ttu-id="43f72-268">Jeśli składnik implementuje <xref:System.IDisposable>, [Dispose — metoda](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="43f72-268">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="43f72-269">Używa następującego składnika `@implements IDisposable` i `Dispose` metody:</span><span class="sxs-lookup"><span data-stu-id="43f72-269">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="43f72-270">Routing</span><span class="sxs-lookup"><span data-stu-id="43f72-270">Routing</span></span>

<span data-ttu-id="43f72-271">Routing w Blazor odbywa się, podając szablonu trasy na każdej części dostępne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43f72-271">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="43f72-272">Gdy plik Razor za pomocą `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="43f72-272">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="43f72-273">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="43f72-273">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="43f72-274">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-274">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="43f72-275">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="43f72-275">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="43f72-276">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="43f72-276">Route parameters</span></span>

<span data-ttu-id="43f72-277">Składniki mogą odbierać parametrów trasy z szablonu trasy dostępnego w `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="43f72-277">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="43f72-278">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-278">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="43f72-279">*Składnik parametru trasy*:</span><span class="sxs-lookup"><span data-stu-id="43f72-279">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="43f72-280">Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="43f72-280">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="43f72-281">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="43f72-281">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="43f72-282">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="43f72-282">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="43f72-283">Dziedziczenie klasy bazowej dla środowiska "związane z kodem"</span><span class="sxs-lookup"><span data-stu-id="43f72-283">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="43f72-284">Pliki składników mieszać kod znaczników HTML i C# przetwarzania kodu w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="43f72-284">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="43f72-285">`@inherits` Dyrektywy może służyć do zapewnienia Blazor aplikacji w środowisku "związane z kodem" oddzielający składnika znaczników, od przetwarzania kodu.</span><span class="sxs-lookup"><span data-stu-id="43f72-285">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="43f72-286">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasy bazowej, `BlazorRocksBase`w celu zapewnienia składnika właściwości i metody.</span><span class="sxs-lookup"><span data-stu-id="43f72-286">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="43f72-287">*Składnik Blazor Rocks*:</span><span class="sxs-lookup"><span data-stu-id="43f72-287">*Blazor Rocks component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="43f72-288">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="43f72-288">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="43f72-289">Klasa bazowa powinien pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="43f72-289">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="43f72-290">Importowanie elementów</span><span class="sxs-lookup"><span data-stu-id="43f72-290">Import components</span></span>

<span data-ttu-id="43f72-291">Przestrzeń nazw składnika utworzone przy użyciu Razor opiera się na:</span><span class="sxs-lookup"><span data-stu-id="43f72-291">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="43f72-292">Projekt `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="43f72-292">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="43f72-293">Ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-293">The path from the project root to the component.</span></span> <span data-ttu-id="43f72-294">Na przykład `ComponentsSample/Pages/Index.razor` znajduje się w przestrzeni nazw `ComponentsSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="43f72-294">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="43f72-295">Postępuj zgodnie z składniki C# nazwy zasad powiązania.</span><span class="sxs-lookup"><span data-stu-id="43f72-295">Components follow C# name binding rules.</span></span> <span data-ttu-id="43f72-296">W przypadku właściwości *Index.razor*, wszystkie składniki w tym samym folderze *stron*i folder nadrzędny *ComponentsSample*, znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="43f72-296">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="43f72-297">Składniki zdefiniowane w innej przestrzeni nazw może być wprowadzana do zakresu przy użyciu firmy Razor [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="43f72-297">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="43f72-298">Jeśli inny składnik `NavMenu.razor`, istnieje w folderze `ComponentsSample/Shared/`, składnika mogą być używane w `Index.razor` następującym `@using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="43f72-298">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="43f72-299">Składniki mogą się też odwoływać przy użyciu ich w pełni kwalifikowanych nazw, która eliminuje potrzebę [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="43f72-299">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="43f72-300">`global::` Kwalifikacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="43f72-300">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="43f72-301">Importowanie elementów z aliasem `using` instrukcji (na przykład `@using Foo = Bar`) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="43f72-301">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="43f72-302">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="43f72-302">Partially qualified names aren't supported.</span></span> <span data-ttu-id="43f72-303">Na przykład dodanie `@using ComponentsSample` i odwoływanie się do `NavMenu.razor` z `<Shared.NavMenu></Shared.NavMenu>` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="43f72-303">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="razor-support"></a><span data-ttu-id="43f72-304">Obsługa razor</span><span class="sxs-lookup"><span data-stu-id="43f72-304">Razor support</span></span>

<span data-ttu-id="43f72-305">**Dyrektywy razor**</span><span class="sxs-lookup"><span data-stu-id="43f72-305">**Razor directives**</span></span>

<span data-ttu-id="43f72-306">W poniższej tabeli przedstawiono dyrektywy razor.</span><span class="sxs-lookup"><span data-stu-id="43f72-306">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="43f72-307">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="43f72-307">Directive</span></span> | <span data-ttu-id="43f72-308">Opis</span><span class="sxs-lookup"><span data-stu-id="43f72-308">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="43f72-309">\@Funkcje</span><span class="sxs-lookup"><span data-stu-id="43f72-309">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="43f72-310">Dodaje C# blok kodu do składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-310">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="43f72-311">Implementuje interfejs dla klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-311">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="43f72-312">\@Inherits</span><span class="sxs-lookup"><span data-stu-id="43f72-312">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="43f72-313">Zapewnia pełną kontrolę nad klasę, która dziedziczy składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-313">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="43f72-314">\@wstrzykiwanie</span><span class="sxs-lookup"><span data-stu-id="43f72-314">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="43f72-315">Włącza usługi iniekcji z [kontener usługi](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="43f72-315">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="43f72-316">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="43f72-316">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="43f72-317">Określa składnik układu.</span><span class="sxs-lookup"><span data-stu-id="43f72-317">Specifies a layout component.</span></span> <span data-ttu-id="43f72-318">Składniki układu są używane, aby uniknąć zduplikowania kodu i niespójności.</span><span class="sxs-lookup"><span data-stu-id="43f72-318">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="43f72-319">\@page</span><span class="sxs-lookup"><span data-stu-id="43f72-319">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="43f72-320">Określa, że składnik obsługi żądań bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="43f72-320">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="43f72-321">`@page` Dyrektywy można określić za pomocą trasy i opcjonalnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="43f72-321">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="43f72-322">W przeciwieństwie do stron Razor `@page` dyrektywy nie musi być pierwszą dyrektywę w górnej części pliku.</span><span class="sxs-lookup"><span data-stu-id="43f72-322">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="43f72-323">Aby uzyskać więcej informacji, zobacz [Routing](xref:blazor/routing).</span><span class="sxs-lookup"><span data-stu-id="43f72-323">For more information, see [Routing](xref:blazor/routing).</span></span> |
| [<span data-ttu-id="43f72-324">\@za pomocą</span><span class="sxs-lookup"><span data-stu-id="43f72-324">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="43f72-325">Dodaje C# `using` dyrektywy do klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-325">Adds the C# `using` directive to the generated component class.</span></span> <span data-ttu-id="43f72-326">Udostępniono też wszystkich składników, które są zdefiniowane w tej przestrzeni nazw do zakresu.</span><span class="sxs-lookup"><span data-stu-id="43f72-326">This also brings all the components defined in that namespace into scope.</span></span> |

<span data-ttu-id="43f72-327">**Atrybuty warunkowe**</span><span class="sxs-lookup"><span data-stu-id="43f72-327">**Conditional attributes**</span></span>

<span data-ttu-id="43f72-328">Atrybuty warunkowe są renderowane na podstawie wartości platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="43f72-328">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="43f72-329">Jeśli wartość jest `false` lub `null`, ten atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="43f72-329">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="43f72-330">Jeśli wartość jest `true`, ten atrybut jest renderowany zminimalizowane.</span><span class="sxs-lookup"><span data-stu-id="43f72-330">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="43f72-331">W poniższym przykładzie `IsCompleted` Określa, czy `checked` jest renderowany w znacznikach formantu:</span><span class="sxs-lookup"><span data-stu-id="43f72-331">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="43f72-332">Jeśli `IsCompleted` jest `true`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="43f72-332">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="43f72-333">Jeśli `IsCompleted` jest `false`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="43f72-333">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="43f72-334">**Dodatkowe informacje na temat Razor**</span><span class="sxs-lookup"><span data-stu-id="43f72-334">**Additional information on Razor**</span></span>

<span data-ttu-id="43f72-335">Aby uzyskać więcej informacji na temat Razor, zobacz [dokumentacja składni Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="43f72-335">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="43f72-336">Surowy kod HTML</span><span class="sxs-lookup"><span data-stu-id="43f72-336">Raw HTML</span></span>

<span data-ttu-id="43f72-337">Ciągi są zwykle renderowane przy użyciu modelu DOM węzły tekstowe, co oznacza, że żadnych znaczników, które mogą zawierać jest ignorowany i traktowany jako literał tekstowy.</span><span class="sxs-lookup"><span data-stu-id="43f72-337">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="43f72-338">Aby renderować kod HTML, opakowywanie zawartości HTML w `MarkupString` wartość.</span><span class="sxs-lookup"><span data-stu-id="43f72-338">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="43f72-339">Wartość jest analizowany jako HTML lub SVG i wstawiony DOM.</span><span class="sxs-lookup"><span data-stu-id="43f72-339">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="43f72-340">Renderowanie surowe HTML skonstruowany z dowolnego niezaufanych źródło jest **zagrożenie dla bezpieczeństwa** i należy ich unikać!</span><span class="sxs-lookup"><span data-stu-id="43f72-340">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="43f72-341">Poniższy przykład pokazuje użycie `MarkupString` typu do dodania bloku zawartości statycznej HTML do wyniku renderowania składnika:</span><span class="sxs-lookup"><span data-stu-id="43f72-341">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="43f72-342">Składniki oparte na szablonach</span><span class="sxs-lookup"><span data-stu-id="43f72-342">Templated components</span></span>

<span data-ttu-id="43f72-343">Oparte na szablonach składniki są składniki, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które następnie mogą być używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-343">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="43f72-344">Oparte na szablonach składniki umożliwiają tworzenie składników wyższego poziomu, które są bardziej wielokrotnego użytku, niż regularne składników.</span><span class="sxs-lookup"><span data-stu-id="43f72-344">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="43f72-345">Zawiera kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="43f72-345">A couple of examples include:</span></span>

* <span data-ttu-id="43f72-346">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka tabeli, wierszy i stopki.</span><span class="sxs-lookup"><span data-stu-id="43f72-346">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="43f72-347">Składnik listy, który umożliwia użytkownikowi określić szablon służący do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="43f72-347">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="43f72-348">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="43f72-348">Template parameters</span></span>

<span data-ttu-id="43f72-349">Oparte na szablonach składnika jest zdefiniowany, określając jeden lub więcej parametrów składnika typu `RenderFragment` lub `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="43f72-349">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="43f72-350">Fragment renderowania reprezentuje segment interfejsu użytkownika, który jest renderowany przez składnik.</span><span class="sxs-lookup"><span data-stu-id="43f72-350">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="43f72-351">Fragment renderowania opcjonalnie przyjmuje parametr, który można określić, gdy jest wywoływany fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="43f72-351">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="43f72-352">*Części szablonu tabeli*:</span><span class="sxs-lookup"><span data-stu-id="43f72-352">*Table Template component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="43f72-353">Podczas korzystania z szablonem składnik, można określić parametry szablonu przy użyciu elementy podrzędne, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="43f72-353">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="43f72-354">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="43f72-354">Template context parameters</span></span>

<span data-ttu-id="43f72-355">Składnik argumentów typu `RenderFragment<T>` przekazany jako elementy mają niejawny parametr o nazwie `context` (na przykład w poprzednim przykładzie kodu `@context.PetId`), można jednak zmienić przy użyciu nazwy parametru `Context` atrybutu w elemencie podrzędnym element.</span><span class="sxs-lookup"><span data-stu-id="43f72-355">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="43f72-356">W poniższym przykładzie `RowTemplate` elementu `Context` Określa atrybut `pet` parametru:</span><span class="sxs-lookup"><span data-stu-id="43f72-356">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="43f72-357">Alternatywnie, można określić `Context` atrybutu w elemencie składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-357">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="43f72-358">Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="43f72-358">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="43f72-359">Może to być przydatne, jeśli chcesz określić nazwę zawartości parametru zawartość elementu podrzędnego niejawne (bez żadnych zawijania elementu podrzędnego).</span><span class="sxs-lookup"><span data-stu-id="43f72-359">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="43f72-360">W poniższym przykładzie `Context` atrybutu jest wyświetlany na `TableTemplate` elementu i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="43f72-360">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="43f72-361">Składniki z kontrolą typów ogólnych</span><span class="sxs-lookup"><span data-stu-id="43f72-361">Generic-typed components</span></span>

<span data-ttu-id="43f72-362">Oparte na szablonach składniki są często objęte wpisane.</span><span class="sxs-lookup"><span data-stu-id="43f72-362">Templated components are often generically typed.</span></span> <span data-ttu-id="43f72-363">Na przykład ogólnego składnika szablon widoku listy może zostać użyty do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="43f72-363">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="43f72-364">Aby zdefiniować element ogólny, należy użyć `@typeparam` dyrektywy, aby określić parametry typu.</span><span class="sxs-lookup"><span data-stu-id="43f72-364">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="43f72-365">*Części szablonu ListView*:</span><span class="sxs-lookup"><span data-stu-id="43f72-365">*ListView Template component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="43f72-366">Podczas korzystania z kontrolą typów ogólnych składników, jeśli jest to możliwe jest wnioskowany parametr typu:</span><span class="sxs-lookup"><span data-stu-id="43f72-366">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="43f72-367">W przeciwnym razie parametru typu muszą być jawnie określone za pomocą atrybutu, który odpowiada nazwie parametru typu.</span><span class="sxs-lookup"><span data-stu-id="43f72-367">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="43f72-368">W poniższym przykładzie `TItem="Pet"` Określa typ:</span><span class="sxs-lookup"><span data-stu-id="43f72-368">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="43f72-369">Kaskadowe wartości i parametry</span><span class="sxs-lookup"><span data-stu-id="43f72-369">Cascading values and parameters</span></span>

<span data-ttu-id="43f72-370">W niektórych przypadkach jest wygodne, przepływ danych ze składnika nadrzędnego za pomocą składnika podrzędny [Parametry składnika](#component-parameters), szczególnie w przypadku kilku warstw składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-370">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="43f72-371">Kaskadowe wartości i parametry rozwiązują ten problem, zapewniając wygodny sposób w celu Podaj wartość, aby wszystkie jego elementy potomne składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="43f72-371">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="43f72-372">Kaskadowe wartości i parametry oferują również podejście składników do zapewnienia koordynacji.</span><span class="sxs-lookup"><span data-stu-id="43f72-372">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="43f72-373">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="43f72-373">Theme example</span></span>

<span data-ttu-id="43f72-374">W następującym *motyw* przykład z przykładowej aplikacji `ThemeInfo` klasa określa informacje o motywie przepływ w dół hierarchii składnika tak, aby udostępnić wszystkie przyciski w ramach danego część aplikacji, ten styl.</span><span class="sxs-lookup"><span data-stu-id="43f72-374">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="43f72-375">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="43f72-375">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="43f72-376">Składnik nadrzędny można podać wartość kaskadowych za pomocą składnika kaskadowa wartość.</span><span class="sxs-lookup"><span data-stu-id="43f72-376">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="43f72-377">Składnik wartości kaskadowych otacza poddrzewo hierarchii składników i dostarcza pojedynczą wartość dla wszystkich składników w ramach tego poddrzewa.</span><span class="sxs-lookup"><span data-stu-id="43f72-377">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="43f72-378">Na przykład przykładowa aplikacja określa informacje o motywie (`ThemeInfo`) w jednej aplikacji układów jako parametr kaskadowych dla wszystkich składników, które tworzą treści układ `@Body` właściwości.</span><span class="sxs-lookup"><span data-stu-id="43f72-378">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="43f72-379">`ButtonClass` jest przypisywana wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="43f72-379">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="43f72-380">Dowolny składnik podrzędny mogą używać tej właściwości, za pośrednictwem `ThemeInfo` cascading obiektu.</span><span class="sxs-lookup"><span data-stu-id="43f72-380">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="43f72-381">*Kaskadowe składnik wartości parametrów układ*:</span><span class="sxs-lookup"><span data-stu-id="43f72-381">*Cascading Values Parameters Layout component*:</span></span>

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

<span data-ttu-id="43f72-382">Zapewnienie korzystania z wartości kaskadowych, składniki deklarować parametrów kaskadowych przy użyciu `[CascadingParameter]` atrybut lub na podstawie wartości ciągu nazwy:</span><span class="sxs-lookup"><span data-stu-id="43f72-382">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="43f72-383">Powiązanie z wartością nazwy ciągu jest istotne, jeśli masz wiele kaskadowych wartości tego samego typu i potrzebujesz odróżnić je w ramach tej samej poddrzewo.</span><span class="sxs-lookup"><span data-stu-id="43f72-383">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="43f72-384">Kaskadowe wartości są powiązane kaskadowych parametry według typu.</span><span class="sxs-lookup"><span data-stu-id="43f72-384">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="43f72-385">W przykładowej aplikacji wiąże składnika kaskadowych wartości parametrów motyw `ThemeInfo` kaskadowa wartość parametru kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="43f72-385">In the sample app, the Cascading Values Parameters Theme component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="43f72-386">Parametr służy do ustawiania klasę CSS dla jednego z przyciski wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="43f72-386">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="43f72-387">*Kaskadowe składnik wartości parametrów motyw*:</span><span class="sxs-lookup"><span data-stu-id="43f72-387">*Cascading Values Parameters Theme component*:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="43f72-388">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="43f72-388">TabSet example</span></span>

<span data-ttu-id="43f72-389">Parametry kaskadowych również włączyć składników do współpracy w całej hierarchii składnika.</span><span class="sxs-lookup"><span data-stu-id="43f72-389">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="43f72-390">Na przykład, należy wziąć pod uwagę następujące *TabSet* przykładu w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43f72-390">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="43f72-391">Przykładowa aplikacja ma `ITab` interfejs, który karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="43f72-391">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="43f72-392">Składnik kaskadowych wartości parametrów TabSet używa ustawienia karty składnik, który zawiera kilka składników karty:</span><span class="sxs-lookup"><span data-stu-id="43f72-392">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="43f72-393">Składniki karty podrzędnej jawnie nie są przekazywane jako parametry można ustawić kartę.</span><span class="sxs-lookup"><span data-stu-id="43f72-393">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="43f72-394">Zamiast tego składniki karty podrzędnej są częścią zawartość elementu podrzędnego zestawu kartę.</span><span class="sxs-lookup"><span data-stu-id="43f72-394">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="43f72-395">Jednak ustawienia karty nadal musi wiedzieć o poszczególnych składników karty, dzięki czemu można renderować, nagłówki i aktywną kartę. Włączyć koordynacja bez konieczności dodatkowego kodu, Ustaw kartę składnika *może zapewnić sama jako wartość kaskadowych* , następnie zostaje pobrana przez podrzędny składniki kartę.</span><span class="sxs-lookup"><span data-stu-id="43f72-395">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="43f72-396">*Składnik TabSet*:</span><span class="sxs-lookup"><span data-stu-id="43f72-396">*TabSet component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="43f72-397">Podrzędny przechwytywania składniki kartę zawierającego ustawienia karty jako parametr kaskadowych, więc składniki kartę dodają same siebie do ustawienia karty i współrzędnych karty, które jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="43f72-397">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="43f72-398">*Karta składników*:</span><span class="sxs-lookup"><span data-stu-id="43f72-398">*Tab component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="43f72-399">Szablony razor</span><span class="sxs-lookup"><span data-stu-id="43f72-399">Razor templates</span></span>

<span data-ttu-id="43f72-400">Renderowanie fragmentów można zdefiniować przy użyciu składni szablonów Razor.</span><span class="sxs-lookup"><span data-stu-id="43f72-400">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="43f72-401">Szablony razor służą do definiowania fragmentu interfejsu użytkownika i założono następujący format:</span><span class="sxs-lookup"><span data-stu-id="43f72-401">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="43f72-402">Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="43f72-402">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="43f72-403">*Składnik Szablony razor*:</span><span class="sxs-lookup"><span data-stu-id="43f72-403">*Razor Templates component*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="43f72-404">Renderowanie fragmentów zdefiniowane przy użyciu Razor szablony mogą być przekazywane jako argumenty do składników oparte na szablonach lub renderowane bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="43f72-404">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="43f72-405">Na przykład szablony poprzedniego bezpośrednio są renderowane w następującym kodem Razor:</span><span class="sxs-lookup"><span data-stu-id="43f72-405">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="43f72-406">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="43f72-406">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="43f72-407">Ręczne logiki RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="43f72-407">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="43f72-408">`Microsoft.AspNetCore.Components.RenderTree` udostępnia metody do manipulowania składników i elementów, w tym ręcznie w kompilowanie składników C# kodu.</span><span class="sxs-lookup"><span data-stu-id="43f72-408">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="43f72-409">Korzystanie z `RenderTreeBuilder` do tworzenia składników to zaawansowany scenariusz.</span><span class="sxs-lookup"><span data-stu-id="43f72-409">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="43f72-410">Źle sformułowane składników (na przykład tag niezamknięty znaczników) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="43f72-410">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="43f72-411">Należy wziąć pod uwagę następujący składnik Pet szczegóły mogą być ręcznie wbudowane w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="43f72-411">Consider the following Pet Details component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="43f72-412">W poniższym przykładzie pętli w `CreateComponent` metoda generuje trzy składniki Pet szczegóły.</span><span class="sxs-lookup"><span data-stu-id="43f72-412">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="43f72-413">Podczas wywoływania `RenderTreeBuilder` metody tworzenia składników (`OpenComponent` i `AddAttribute`), numery sekwencyjne są numery wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="43f72-413">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="43f72-414">Algorytm różnica Blazor opiera się na numery sekwencyjne distinct wierszy kodu, a nie odrębne wywołania wywołania.</span><span class="sxs-lookup"><span data-stu-id="43f72-414">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="43f72-415">Podczas tworzenia składnika za pomocą `RenderTreeBuilder` metody, umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="43f72-415">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="43f72-416">**Za pomocą obliczeń lub licznika do generowania numer sekwencyjny może prowadzić do pogorszenia wydajności.**</span><span class="sxs-lookup"><span data-stu-id="43f72-416">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="43f72-417">Aby uzyskać więcej informacji, zobacz [sekwencji liczb odnoszą się do kolejności cyfry i nie wykonywania linii kodu](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) sekcji.</span><span class="sxs-lookup"><span data-stu-id="43f72-417">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="43f72-418">*Wbudowane składnika zawartość*:</span><span class="sxs-lookup"><span data-stu-id="43f72-418">*Built Content component*:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="43f72-419">Numery sekwencyjne odnoszą się do kolejności cyfry i nie wykonywania wiersza kodu</span><span class="sxs-lookup"><span data-stu-id="43f72-419">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="43f72-420">Blazor `.razor` pliki są zawsze kompilowane.</span><span class="sxs-lookup"><span data-stu-id="43f72-420">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="43f72-421">Jest to potencjalnie dużą zaletą dla `.razor` kroku kompilacji można się posłużyć do dodania informacji, który zwiększa wydajność aplikacji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="43f72-421">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="43f72-422">Przykład klawisza ulepszeniami obejmują *sekwencji numerów*.</span><span class="sxs-lookup"><span data-stu-id="43f72-422">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="43f72-423">Numery sekwencyjne wskazuje środowiska uruchomieniowego, które dane wyjściowe pochodzą które odrębne i uporządkowanych wiersze kodu.</span><span class="sxs-lookup"><span data-stu-id="43f72-423">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="43f72-424">Środowisko wykonawcze używa tych informacji do wygenerowania różnic wydajne drzewa liniowo, który jest znacznie szybsze niż zazwyczaj dla algorytmu diff drzewo Ogólne.</span><span class="sxs-lookup"><span data-stu-id="43f72-424">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="43f72-425">Należy wziąć pod uwagę następujące proste `.razor` pliku:</span><span class="sxs-lookup"><span data-stu-id="43f72-425">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="43f72-426">To kompiluje, aby podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="43f72-426">This compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="43f72-427">Kiedy ten kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, otrzymuje konstruktora:</span><span class="sxs-lookup"><span data-stu-id="43f72-427">When this code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="43f72-428">Sequence</span><span class="sxs-lookup"><span data-stu-id="43f72-428">Sequence</span></span> | <span data-ttu-id="43f72-429">Typ</span><span class="sxs-lookup"><span data-stu-id="43f72-429">Type</span></span>      | <span data-ttu-id="43f72-430">Dane</span><span class="sxs-lookup"><span data-stu-id="43f72-430">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="43f72-431">0</span><span class="sxs-lookup"><span data-stu-id="43f72-431">0</span></span>        | <span data-ttu-id="43f72-432">Węzeł tekstowy</span><span class="sxs-lookup"><span data-stu-id="43f72-432">Text node</span></span> | <span data-ttu-id="43f72-433">pierwszy</span><span class="sxs-lookup"><span data-stu-id="43f72-433">First</span></span>  |
| <span data-ttu-id="43f72-434">1</span><span class="sxs-lookup"><span data-stu-id="43f72-434">1</span></span>        | <span data-ttu-id="43f72-435">Węzeł tekstowy</span><span class="sxs-lookup"><span data-stu-id="43f72-435">Text node</span></span> | <span data-ttu-id="43f72-436">Sekunda</span><span class="sxs-lookup"><span data-stu-id="43f72-436">Second</span></span> |

<span data-ttu-id="43f72-437">Teraz załóżmy, że `someFlag` staje się `false`, i możemy ponownie renderowania.</span><span class="sxs-lookup"><span data-stu-id="43f72-437">Now imagine that `someFlag` becomes `false`, and we render again.</span></span> <span data-ttu-id="43f72-438">Tym razem odbiera konstruktora:</span><span class="sxs-lookup"><span data-stu-id="43f72-438">This time, the builder receives:</span></span>

| <span data-ttu-id="43f72-439">Sequence</span><span class="sxs-lookup"><span data-stu-id="43f72-439">Sequence</span></span> | <span data-ttu-id="43f72-440">Typ</span><span class="sxs-lookup"><span data-stu-id="43f72-440">Type</span></span>       | <span data-ttu-id="43f72-441">Dane</span><span class="sxs-lookup"><span data-stu-id="43f72-441">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="43f72-442">1</span><span class="sxs-lookup"><span data-stu-id="43f72-442">1</span></span>        | <span data-ttu-id="43f72-443">Węzeł tekstowy</span><span class="sxs-lookup"><span data-stu-id="43f72-443">Text node</span></span>  | <span data-ttu-id="43f72-444">Sekunda</span><span class="sxs-lookup"><span data-stu-id="43f72-444">Second</span></span> |

<span data-ttu-id="43f72-445">Gdy środowisko uruchomieniowe wykonuje różnic, widzi, elementu w sekwencji `0` została wyjęta, co generuje następujące proste *Przeprowadź edycję skryptu*:</span><span class="sxs-lookup"><span data-stu-id="43f72-445">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="43f72-446">Usuń pierwszy węzeł tekstowy.</span><span class="sxs-lookup"><span data-stu-id="43f72-446">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="43f72-447">Co się nie uda, jeśli programowo wygenerować numery sekwencyjne</span><span class="sxs-lookup"><span data-stu-id="43f72-447">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="43f72-448">Sobie wyobrazić, autorem Poniższa logika konstruktora rendertree:</span><span class="sxs-lookup"><span data-stu-id="43f72-448">Imagine instead that you wrote the following rendertree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="43f72-449">Po pierwsze dane wyjściowe będą:</span><span class="sxs-lookup"><span data-stu-id="43f72-449">Now the first output would be:</span></span>

<span data-ttu-id="43f72-450">| Sekwencja | Typ | Dane || :------: | --------- | :--- : | | 0 | Węzeł tekstowy | Pierwszy || 1 | Węzeł tekstowy | Drugi |</span><span class="sxs-lookup"><span data-stu-id="43f72-450">| Sequence | Type      | Data   | | :------: | --------- | :--- : | | 0        | Text node | First  | | 1        | Text node | Second |</span></span>

<span data-ttu-id="43f72-451">Ten wynik jest identyczne z poprzednich przypadkiem, więc Brak problemów ujemna.</span><span class="sxs-lookup"><span data-stu-id="43f72-451">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="43f72-452">W drugiej renderowanie, gdy `someFlag` jest `false`, dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="43f72-452">On the second render when `someFlag` is `false`, the output is:</span></span>

| <span data-ttu-id="43f72-453">Sequence</span><span class="sxs-lookup"><span data-stu-id="43f72-453">Sequence</span></span> | <span data-ttu-id="43f72-454">Typ</span><span class="sxs-lookup"><span data-stu-id="43f72-454">Type</span></span>      | <span data-ttu-id="43f72-455">Dane</span><span class="sxs-lookup"><span data-stu-id="43f72-455">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="43f72-456">0</span><span class="sxs-lookup"><span data-stu-id="43f72-456">0</span></span>        | <span data-ttu-id="43f72-457">Węzeł tekstowy</span><span class="sxs-lookup"><span data-stu-id="43f72-457">Text node</span></span> | <span data-ttu-id="43f72-458">Sekunda</span><span class="sxs-lookup"><span data-stu-id="43f72-458">Second</span></span> |

<span data-ttu-id="43f72-459">Tym razem algorytm diff widzi, który *dwóch* nastąpiły zmiany, a algorytm generuje poniższy skrypt edycji:</span><span class="sxs-lookup"><span data-stu-id="43f72-459">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="43f72-460">Zmień wartość pierwszy węzeł tekstowy w celu `Second`.</span><span class="sxs-lookup"><span data-stu-id="43f72-460">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="43f72-461">Usuń drugi węzeł tekstowy.</span><span class="sxs-lookup"><span data-stu-id="43f72-461">Remove the second text node.</span></span>

<span data-ttu-id="43f72-462">Generowanie numery sekwencyjne utracił przydatne informacje o tym, gdzie `if/else` gałęzie i pętle znajdowały się w kodzie oryginalnym.</span><span class="sxs-lookup"><span data-stu-id="43f72-462">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="43f72-463">Skutkuje to różnic **dwa razy dłużej** tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="43f72-463">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="43f72-464">Jest to uproszczony przykład.</span><span class="sxs-lookup"><span data-stu-id="43f72-464">This is a trivial example.</span></span> <span data-ttu-id="43f72-465">W przypadku bardziej realistycznego o złożone i głęboko zagnieżdżonych struktur, a w szczególności z pętli przeprowadzanie bardziej dotkliwych jest spadek wydajności.</span><span class="sxs-lookup"><span data-stu-id="43f72-465">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="43f72-466">Zamiast natychmiast identyfikowanie, które bloki pętli lub gałęzi zostało wstawionych lub usunięty, algorytm różnicowego musi recurse głęboko do drzewa renderowania i zazwyczaj skompilować znacznie dłużej edycji skryptów, ponieważ jest on misinformed o tym, jak starych i nowych struktur odnoszą się do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="43f72-466">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="43f72-467">Wskazówki i wniosków</span><span class="sxs-lookup"><span data-stu-id="43f72-467">Guidance and conclusions</span></span>

* <span data-ttu-id="43f72-468">Wydajność aplikacji wystąpi, jeśli numerów sekwencji są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="43f72-468">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="43f72-469">Struktura nie można utworzyć liczby sekwencji automatycznie w czasie wykonywania, ponieważ nie istnieje niezbędne informacje, o ile nie są przechwytywane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="43f72-469">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="43f72-470">Nie zapisuj długie bloki konstrukcyjne ręcznie zaimplementowane `RenderTreeBuilder` logiki.</span><span class="sxs-lookup"><span data-stu-id="43f72-470">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="43f72-471">Preferuj `.razor` pliki i umożliwić kompilatorowi do czynienia z numerami sekwencji.</span><span class="sxs-lookup"><span data-stu-id="43f72-471">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="43f72-472">Jeśli numery sekwencyjne są zapisane na stałe, algorytm diff wymaga jedynie, czy numery sekwencyjne wzrost wartości.</span><span class="sxs-lookup"><span data-stu-id="43f72-472">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="43f72-473">Wartość początkowa i luki są nieistotne.</span><span class="sxs-lookup"><span data-stu-id="43f72-473">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="43f72-474">Jedną z opcji uzasadnione jest wykorzystanie numer wiersza kodu jako numer sekwencyjny lub zacznij od zera i zwiększenia z nich lub setki (lub dowolnym preferowanym interwale).</span><span class="sxs-lookup"><span data-stu-id="43f72-474">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="43f72-475">Blazor używa numerów sekwencji, podczas gdy innych platform tworzenia interfejsu użytkownika porównywanie drzewa nie są używane.</span><span class="sxs-lookup"><span data-stu-id="43f72-475">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="43f72-476">Porównywanie jest znacznie szybszy, numery sekwencyjne są używane, gdy Blazor ma tę zaletę krok kompilacji, który dotyczy numerów sekwencyjnych automatycznie dla deweloperów autorstwa `.razor` plików.</span><span class="sxs-lookup"><span data-stu-id="43f72-476">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>
