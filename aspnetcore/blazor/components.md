---
title: Tworzenie i używanie składników Razor
author: guardrex
description: Informacje o sposobie tworzenia i używania składników Razor, w tym jak powiązać z danymi, obsługa zdarzeń i Zarządzaj cyklami życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/12/2019
uid: blazor/components
ms.openlocfilehash: f88497195d9a108a4b8890522078736e335c5b0a
ms.sourcegitcommit: 739a3d7ca4fd2908ea0984940eca589a96359482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67040692"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="50cdb-103">Tworzenie i używanie składników Razor</span><span class="sxs-lookup"><span data-stu-id="50cdb-103">Create and use Razor components</span></span>

<span data-ttu-id="50cdb-104">Przez [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), i [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="50cdb-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="50cdb-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="50cdb-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="50cdb-106">Blazor aplikacje są tworzone przy użyciu *składniki*.</span><span class="sxs-lookup"><span data-stu-id="50cdb-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="50cdb-107">Składnik jest niezależna fragmentu interfejsu użytkownika (UI), takich jak strony, okno dialogowe lub formularza.</span><span class="sxs-lookup"><span data-stu-id="50cdb-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="50cdb-108">Składnik zawiera kod znaczników HTML i logika przetwarzania wymagane w celu wstrzyknięcia danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="50cdb-109">Składniki są elastyczne i uproszczone.</span><span class="sxs-lookup"><span data-stu-id="50cdb-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="50cdb-110">Mogą być zagnieżdżone, ponownie i współużytkowane między projektami.</span><span class="sxs-lookup"><span data-stu-id="50cdb-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="50cdb-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="50cdb-111">Component classes</span></span>

<span data-ttu-id="50cdb-112">Składniki są implementowane w [Razor](xref:mvc/views/razor) pliki składników ( *.razor*) przy użyciu kombinacji C# i kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="50cdb-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span>

<span data-ttu-id="50cdb-113">Składniki mogą być tworzone za pomocą *.cshtml* rozszerzenie pliku, tak długo, jak pliki są identyfikowane jako pliki składnika Razor przy użyciu `_RazorComponentInclude` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="50cdb-113">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="50cdb-114">Na przykład aplikację, która określa, że wszystkie *.cshtml* plików w obszarze *stron* folder powinien być traktowany jako plików składników Razor:</span><span class="sxs-lookup"><span data-stu-id="50cdb-114">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="50cdb-115">W interfejsie użytkownika dla składnika jest zdefiniowana za pomocą kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="50cdb-115">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="50cdb-116">Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="50cdb-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="50cdb-117">Gdy aplikacja jest kompilowana, kod znaczników HTML i C# logiki renderowania są konwertowane na klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-117">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="50cdb-118">Nazwa wygenerowanej klasy odpowiada nazwie pliku.</span><span class="sxs-lookup"><span data-stu-id="50cdb-118">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="50cdb-119">Elementy członkowskie klasy składników są zdefiniowane w `@code` bloku.</span><span class="sxs-lookup"><span data-stu-id="50cdb-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="50cdb-120">W `@code` bloku, stan składników (właściwości, pola) jest określony za pomocą metody obsługi zdarzeń lub Definiowanie logiki innych składników.</span><span class="sxs-lookup"><span data-stu-id="50cdb-120">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="50cdb-121">Więcej niż jeden `@code` bloku jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="50cdb-121">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="50cdb-122">W poprzednich wersjach programu ASP.NET Core `@functions` bloki były używane dla tego samego celu co `@code` bloków.</span><span class="sxs-lookup"><span data-stu-id="50cdb-122">In previous versions of ASP.NET Core, `@functions` blocks were used for the same purpose as `@code` blocks.</span></span> <span data-ttu-id="50cdb-123">`@functions` bloki w dalszym ciągu działać, ale firma Microsoft zaleca używanie `@code` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-123">`@functions` blocks continue to work, but we recommend using the `@code` directive.</span></span>

<span data-ttu-id="50cdb-124">Elementy członkowskie składnika mogą posłużyć jako część składnika przez renderowanie przy użyciu logiki C# wyrażeń, które zaczyna się `@`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-124">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="50cdb-125">Na przykład C# pole jest renderowane przez dodanie przedrostka `@` na nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="50cdb-125">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="50cdb-126">Poniższy przykład daje w wyniku i renderuje:</span><span class="sxs-lookup"><span data-stu-id="50cdb-126">The following example evaluates and renders:</span></span>

* <span data-ttu-id="50cdb-127">`_headingFontStyle` wartości właściwości CSS dla `font-style`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-127">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="50cdb-128">`_headingText` w treści `<h1>` elementu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-128">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="50cdb-129">Po początkowo renderowania składnik składnika generuje jej drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="50cdb-129">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="50cdb-130">Blazor następnie porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszystkie zmiany do przeglądarki w modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="50cdb-130">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="50cdb-131">Składniki są zwykłe C# klasy i można umieścić w dowolnym miejscu w obrębie projektu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-131">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="50cdb-132">Składniki, które zwykle tworzą stron sieci Web znajdują się w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-132">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="50cdb-133">Składniki strony inne niż często są umieszczane w *Shared* folder lub folder niestandardowy dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-133">Non-page components are frequently placed into the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="50cdb-134">Aby korzystać z folderu niestandardowego, Dodaj niestandardowego folderu obszaru nazw do składnika nadrzędnego lub w aplikacji  *\_Imports.razor* pliku.</span><span class="sxs-lookup"><span data-stu-id="50cdb-134">To use a custom folder, either add the custom folder's namespace to the parent component or to the app's *\_Imports.razor* file.</span></span> <span data-ttu-id="50cdb-135">Na przykład następująca przestrzeń nazw sprawia, że składniki *składniki* dostępne w przypadku aplikacji głównej przestrzeni nazw jest folder `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="50cdb-135">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="50cdb-136">Integrowanie składników aplikacji stronami Razor i programem MVC</span><span class="sxs-lookup"><span data-stu-id="50cdb-136">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="50cdb-137">Składniki za pomocą istniejących aplikacji stronami Razor i programem MVC.</span><span class="sxs-lookup"><span data-stu-id="50cdb-137">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="50cdb-138">Nie ma potrzeby ponownego wpisywania istniejących stron lub widoków w celu używania składników Razor.</span><span class="sxs-lookup"><span data-stu-id="50cdb-138">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="50cdb-139">Po wyrenderowaniu strony lub widoku składniki są prerendered&dagger; w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-139">When the page or view is rendered, components are prerendered&dagger; at the same time.</span></span> 

> [!NOTE]
> <span data-ttu-id="50cdb-140">&dagger;Prerendering po stronie serwera jest domyślnie włączona dla Blazor po stronie serwera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-140">&dagger;Server-side prerendering is enabled for Blazor server-side apps by default.</span></span> <span data-ttu-id="50cdb-141">Aplikacje Blazor po stronie klienta będzie obsługiwać prerendering w nadchodzącej wersji 5 (wersja zapoznawcza).</span><span class="sxs-lookup"><span data-stu-id="50cdb-141">Client-side Blazor apps will support prerendering in the upcoming Preview 5 release.</span></span> <span data-ttu-id="50cdb-142">Aby uzyskać więcej informacji, zobacz [aktualizacji szablonów/oprogramowanie pośredniczące MapFallbackToPage/pliku](https://github.com/aspnet/AspNetCore/issues/8852).</span><span class="sxs-lookup"><span data-stu-id="50cdb-142">For more information, see [Update templates/middleware to use MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span></span>

<span data-ttu-id="50cdb-143">Aby renderować składnika ze strony lub widok, należy użyć `RenderComponentAsync<TComponent>` metody pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="50cdb-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="50cdb-144">Gdy widoków i stron można użyć składników, prawdą nie dotyczy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="50cdb-145">Składniki nie można używać w scenariuszach specyficznych dla widoku i strony, takich jak widoki częściowe i sekcji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="50cdb-146">Aby użyć logiki z widoku częściowego w składniku, współczynnik logiki widoku częściowego do składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="50cdb-147">Aby uzyskać więcej informacji na temat sposobu Wyrenderowana i składnika, stan składników odbywa się w aplikacji po stronie serwera Blazor, zobacz <xref:blazor/hosting-models> artykułu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-147">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="using-components"></a><span data-ttu-id="50cdb-148">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="50cdb-148">Using components</span></span>

<span data-ttu-id="50cdb-149">Składniki mogą zawierać inne składniki, deklarując je przy użyciu składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="50cdb-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="50cdb-150">Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="50cdb-151">Następujące znaczniki w *Index.razor* renderuje `HeadingComponent` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="50cdb-151">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="50cdb-152">*Components/HeadingComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-152">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

## <a name="component-parameters"></a><span data-ttu-id="50cdb-153">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="50cdb-153">Component parameters</span></span>

<span data-ttu-id="50cdb-154">Składniki mogą mieć *Parametry składnika*, które są definiowane za pomocą *niepublicznych* właściwości klasy składnika za pomocą `[Parameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-154">Components can have *component parameters*, which are defined using *non-public* properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="50cdb-155">Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.</span><span class="sxs-lookup"><span data-stu-id="50cdb-155">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="50cdb-156">W poniższym przykładzie `ParentComponent` ustawia wartość `Title` właściwość `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-156">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="50cdb-157">*Pages/ParentComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-157">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

<span data-ttu-id="50cdb-158">*Components/ChildComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-158">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

## <a name="child-content"></a><span data-ttu-id="50cdb-159">Zawartość elementu podrzędnego</span><span class="sxs-lookup"><span data-stu-id="50cdb-159">Child content</span></span>

<span data-ttu-id="50cdb-160">Składniki można ustawić zawartości innego składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-160">Components can set the content of another component.</span></span> <span data-ttu-id="50cdb-161">Przypisywanie składnik udostępnia zawartości między tagami, które określają odbieranie składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-161">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="50cdb-162">Na przykład `ParentComponent` może zapewnić zawartości dla renderowania przez składnik podrzędnych przez umieszczenie zawartość wewnątrz `<ChildComponent>` tagów.</span><span class="sxs-lookup"><span data-stu-id="50cdb-162">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="50cdb-163">*Pages/ParentComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-163">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

<span data-ttu-id="50cdb-164">Składnik podrzędny ma `ChildContent` właściwość, która reprezentuje `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-164">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="50cdb-165">Wartość `ChildContent` jest umieszczony w znaczniku elementu podrzędnego, której zawartość ma być renderowany.</span><span class="sxs-lookup"><span data-stu-id="50cdb-165">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="50cdb-166">W poniższym przykładzie wartość `ChildContent` jest otrzymane od składnika nadrzędnego i renderowania wewnątrz panelu Bootstrap `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-166">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="50cdb-167">*Components/ChildComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-167">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="50cdb-168">Odbieranie właściwość `RenderFragment` zawartość, musi nosić `ChildContent` przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="50cdb-168">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="50cdb-169">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="50cdb-169">Data binding</span></span>

<span data-ttu-id="50cdb-170">Powiązanie danych do składników i elementów DOM odbywa się za pomocą `@bind` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-170">Data binding to both components and DOM elements is accomplished with the `@bind` attribute.</span></span> <span data-ttu-id="50cdb-171">Poniższy przykład tworzy powiązanie `_italicsCheck` pole do pola wyboru zaznaczone stanu:</span><span class="sxs-lookup"><span data-stu-id="50cdb-171">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="50cdb-172">Gdy pole wyboru jest zaznaczone, a następnie wyczyszczone, wartość właściwości jest aktualizowany do `true` i `false`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="50cdb-172">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="50cdb-173">Pole wyboru jest aktualizowana w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="50cdb-173">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="50cdb-174">Ponieważ składniki renderować się po wykonaniu kod procedury obsługi zdarzeń, aktualizacje właściwości są zazwyczaj odzwierciedlane w interfejsie użytkownika od razu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-174">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="50cdb-175">Za pomocą `@bind` z `CurrentValue` właściwości (`<input @bind="CurrentValue" />`) jest zasadniczo odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="50cdb-175">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="50cdb-176">Po wyrenderowaniu składnika `value` danego elementu wejściowego pochodzi z `CurrentValue` właściwości.</span><span class="sxs-lookup"><span data-stu-id="50cdb-176">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="50cdb-177">Gdy użytkownik wpisuje w polu tekstowym `onchange` zdarzenie jest generowane i `CurrentValue` zostaje ustalona zmieniona wartość.</span><span class="sxs-lookup"><span data-stu-id="50cdb-177">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="50cdb-178">W rzeczywistości generowania kodu jest nieco bardziej złożone, ponieważ `@bind` obsługuje kilka przypadków, w którym konwersje są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-178">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="50cdb-179">W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia z `value` atrybutu i uchwytów zmiany przy użyciu zarejestrowanego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="50cdb-179">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="50cdb-180">Oprócz `onchange`, właściwości mogą być powiązane przy użyciu innych zdarzeń, takich jak `oninput` , dodając `@bind` atrybutem `event` parametru:</span><span class="sxs-lookup"><span data-stu-id="50cdb-180">In addition to `onchange`, the property can be bound using other events like `oninput` by adding a `@bind` attribute with an `event` parameter:</span></span>

```cshtml
<input type="text" @bind-value="@CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="50cdb-181">W odróżnieniu od `onchange`, `oninput` generowane dla każdego znaku, która jest wprowadzana do pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="50cdb-181">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

<span data-ttu-id="50cdb-182">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="50cdb-182">**Format strings**</span></span>

<span data-ttu-id="50cdb-183">Powiązanie danych w programach <xref:System.DateTime> ciągi formatujące.</span><span class="sxs-lookup"><span data-stu-id="50cdb-183">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="50cdb-184">Inne wyrażenia formatu, takie jak waluta lub formaty liczbowe nie są dostępne w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="50cdb-184">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="50cdb-185">`@bind:format` Atrybut określa format daty do zastosowania do `value` z `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-185">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="50cdb-186">Format umożliwia również przeanalizować wartość po `onchange` wystąpi zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-186">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="50cdb-187">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="50cdb-187">**Component parameters**</span></span>

<span data-ttu-id="50cdb-188">Powiązanie rozpoznaje również Parametry składnika, gdzie `@bind-{property}` można powiązać wartości właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="50cdb-188">Binding also recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="50cdb-189">Używa następującego składnika `ChildComponent` i wiąże `ParentYear` parametru z obiektu `Year` parametru w składniku podrzędne:</span><span class="sxs-lookup"><span data-stu-id="50cdb-189">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="50cdb-190">Składnik nadrzędny:</span><span class="sxs-lookup"><span data-stu-id="50cdb-190">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="50cdb-191">Element podrzędny:</span><span class="sxs-lookup"><span data-stu-id="50cdb-191">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="50cdb-192">`EventCallback<T>` została wyjaśniona w [EventCallback](#eventcallback) sekcji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-192">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="50cdb-193">Trwa ładowanie `ParentComponent` tworzy następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="50cdb-193">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="50cdb-194">Jeśli wartość `ParentYear` zmianie właściwości, wybierając przycisk w `ParentComponent`, `Year` właściwość `ChildComponent` jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="50cdb-194">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="50cdb-195">Nowa wartość `Year` jest wyświetlana w Interfejsie użytkownika po `ParentComponent` jest rerendered:</span><span class="sxs-lookup"><span data-stu-id="50cdb-195">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="50cdb-196">`Year` Parametr jest możliwej do wiązania, ponieważ ma ona towarzyszące `YearChanged` zdarzeń, który jest zgodny z typem `Year` parametru.</span><span class="sxs-lookup"><span data-stu-id="50cdb-196">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="50cdb-197">Zgodnie z Konwencją `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo odpowiednikiem pisania,</span><span class="sxs-lookup"><span data-stu-id="50cdb-197">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="50cdb-198">Ogólnie rzecz biorąc, można powiązać właściwości z odpowiedni program obsługi zdarzeń, za pomocą `@bind-property:event` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-198">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="50cdb-199">Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` przy użyciu następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="50cdb-199">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<FooComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="50cdb-200">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="50cdb-200">Event handling</span></span>

<span data-ttu-id="50cdb-201">Składniki razor udostępniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="50cdb-201">Razor components provide event handling features.</span></span> <span data-ttu-id="50cdb-202">Atrybut elementu HTML o nazwie `on<event>` (na przykład `onclick`, `onsubmit`) z wartością wpisane delegata składniki Razor traktuje wartość atrybutu jako program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="50cdb-202">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="50cdb-203">Nazwa atrybutu zawsze zaczyna się od `@on`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-203">The attribute's name always starts with `@on`.</span></span>

<span data-ttu-id="50cdb-204">Poniższy kod wywoła `UpdateHeading` metodę po wybraniu przycisku w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="50cdb-204">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="@UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="50cdb-205">Poniższy kod wywoła `CheckboxChanged` metody, gdy pole wyboru jest zmieniana w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="50cdb-205">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="@CheckboxChanged" />

@code {
    private void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="50cdb-206">Programy obsługi zdarzeń można też asynchronicznego i zwracają <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="50cdb-206">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="50cdb-207">Nie ma konieczności ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-207">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="50cdb-208">Wyjątki są rejestrowane w momencie ich wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="50cdb-208">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" @onclick="@UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="50cdb-209">Niektóre zdarzenia są dozwolone typy argumentów zdarzenia określonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="50cdb-209">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="50cdb-210">Jeśli dostęp do jednego z następujących typów zdarzeń nie jest to konieczne, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="50cdb-210">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="50cdb-211">Lista argumentów zdarzeń obsługiwanych jest:</span><span class="sxs-lookup"><span data-stu-id="50cdb-211">The list of supported event arguments is:</span></span>

* <span data-ttu-id="50cdb-212">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="50cdb-212">UIEventArgs</span></span>
* <span data-ttu-id="50cdb-213">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="50cdb-213">UIChangeEventArgs</span></span>
* <span data-ttu-id="50cdb-214">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="50cdb-214">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="50cdb-215">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="50cdb-215">UIMouseEventArgs</span></span>

<span data-ttu-id="50cdb-216">Wyrażenia lambda może również służyć:</span><span class="sxs-lookup"><span data-stu-id="50cdb-216">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="50cdb-217">Często jest to wygodne zamknąć dodatkowe wartości, takich jak podczas iteracji w zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="50cdb-217">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="50cdb-218">Poniższy przykład tworzy trzy przyciski wszystkich, która wywołuje metodę `UpdateHeading` przekazywaniem argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`) w przypadku wybrania w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="50cdb-218">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="50cdb-219">Czy **nie** zmienna pętli (`i`) w `for` pętli bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="50cdb-219">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="50cdb-220">W przeciwnym razie tę samą zmienną jest używany przez wszystkie wyrażenia lambda, powodując `i`przez wartość, która ma być taka sama we wszystkich wyrażenia lambda.</span><span class="sxs-lookup"><span data-stu-id="50cdb-220">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="50cdb-221">Zawsze odzwierciedla jej wartość w zmiennej lokalnej (`buttonNumber` w powyższym przykładzie), a następnie użyj go.</span><span class="sxs-lookup"><span data-stu-id="50cdb-221">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="50cdb-222">EventCallback</span><span class="sxs-lookup"><span data-stu-id="50cdb-222">EventCallback</span></span>

<span data-ttu-id="50cdb-223">Typowy scenariusz w przypadku zagnieżdżonych składników jest wymaganą do uruchomienia elementu nadrzędnego metodę składnika, gdy wystąpi zdarzenie składnika podrzędnego&mdash;na przykład, gdy `onclick` wystąpi zdarzenie w podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="50cdb-223">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="50cdb-224">Aby udostępnić zdarzenia dotyczące składników, należy użyć `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-224">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="50cdb-225">Składnik nadrzędny można przypisać metodę wywołania zwrotnego do składnika podrzędnego `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-225">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="50cdb-226">Składnik podrzędnych Przykładowa aplikacja pokazuje, jak przycisk `onclick` program obsługi jest skonfigurowany do otrzymywać `EventCallback` delegatem przykładowy składnik nadrzędny.</span><span class="sxs-lookup"><span data-stu-id="50cdb-226">The Child component in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's Parent component.</span></span> <span data-ttu-id="50cdb-227">`EventCallback` Jest wypełniana `UIMouseEventArgs`, która jest odpowiednia dla `onclick` zdarzeń za pomocą urządzenia peryferyjne:</span><span class="sxs-lookup"><span data-stu-id="50cdb-227">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="50cdb-228">Składnik nadrzędny ustawia elementu podrzędnego `EventCallback<T>` do jego `ShowMessage` metody:</span><span class="sxs-lookup"><span data-stu-id="50cdb-228">The Parent component sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="50cdb-229">Po wybraniu przycisku w składniku podrzędne:</span><span class="sxs-lookup"><span data-stu-id="50cdb-229">When the button is selected in the Child component:</span></span>

* <span data-ttu-id="50cdb-230">Składnik nadrzędny `ShowMessage` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="50cdb-230">The Parent component's `ShowMessage` method is called.</span></span> <span data-ttu-id="50cdb-231">`messageText` są aktualizowane i wyświetlane w składniku nadrzędnym.</span><span class="sxs-lookup"><span data-stu-id="50cdb-231">`messageText` is updated and displayed in the Parent component.</span></span>
* <span data-ttu-id="50cdb-232">Wywołanie `StateHasChanged` nie jest wymagane w przypadku metody wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="50cdb-232">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="50cdb-233">`StateHasChanged` wywoływana jest automatycznie rerender składnik nadrzędny tak samo, jak zdarzenia podrzędne wyzwolić składnika rerendering w procedurze obsługi zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="50cdb-233">`StateHasChanged` is called automatically to rerender the Parent component, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="50cdb-234">`EventCallback` i `EventCallback<T>` zezwolić delegatów asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="50cdb-234">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="50cdb-235">`EventCallback<T>` Zdecydowanie jest wpisane i wymaga określonych argumentów.</span><span class="sxs-lookup"><span data-stu-id="50cdb-235">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="50cdb-236">`EventCallback` słabo został wpisany oraz umożliwia dowolny typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-236">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="50cdb-237">Wywoływanie `EventCallback` lub `EventCallback<T>` z `InvokeAsync` i await <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="50cdb-237">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="50cdb-238">Użyj `EventCallback` i `EventCallback<T>` dla zdarzenia, obsługi i wiązania parametrów na części.</span><span class="sxs-lookup"><span data-stu-id="50cdb-238">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span> <span data-ttu-id="50cdb-239">Nie używaj `EventCallback` i `EventCallback<T>` dla zawartość elementu podrzędnego&mdash;nadal używać `RenderFragment` i `RenderFragment<T>` dla zawartość elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="50cdb-239">Don't use `EventCallback` and `EventCallback<T>` for child content&mdash;continue to use `RenderFragment` and `RenderFragment<T>` for child content.</span></span>

<span data-ttu-id="50cdb-240">Preferuj silnie typizowaną `EventCallback<T>`, które zapewniają lepszy błąd dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-240">Prefer the strongly typed `EventCallback<T>`, which provides better error feedback to users of the component.</span></span> <span data-ttu-id="50cdb-241">Podobnie jak inne procedury obsługi zdarzeń interfejsu użytkownika, określając parametr zdarzeń jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="50cdb-241">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="50cdb-242">Użyj `EventCallback` po żadnej wartości przekazanej do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="50cdb-242">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="50cdb-243">Przechwytywanie odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="50cdb-243">Capture references to components</span></span>

<span data-ttu-id="50cdb-244">Odwołania do składników zapewnia sposób odwołania wystąpienie składnika, dzięki czemu można wysyłać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-244">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="50cdb-245">Do przechwycenia odwołania do składników, należy dodać `@ref` atrybutu do elementu podrzędnego, a następnie zdefiniować pole o tej samej nazwie i typie jako element podrzędny.</span><span class="sxs-lookup"><span data-stu-id="50cdb-245">To capture a component reference, add a `@ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="50cdb-246">Po wyrenderowaniu składnika `loginDialog` pole jest wypełniane `MyLoginDialog` wystąpienie składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="50cdb-246">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="50cdb-247">Następnie można wywoływać metod .NET w instancji składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-247">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50cdb-248">`loginDialog` Zmiennej tylko jest wypełniana po składnik jest renderowana i jego dane wyjściowe obejmują `MyLoginDialog` elementu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-248">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="50cdb-249">Do tego momentu nie ma nic do odwołania.</span><span class="sxs-lookup"><span data-stu-id="50cdb-249">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="50cdb-250">Do manipulowania odwołania do składników, po zakończeniu renderowania składnik, należy użyć `OnAfterRenderAsync` lub `OnAfterRender` metody.</span><span class="sxs-lookup"><span data-stu-id="50cdb-250">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="50cdb-251">Podczas przechwytywania odwołania do składników użyj podobnej składni do [przechwytywania odwołania do elementu](xref:blazor/javascript-interop#capture-references-to-elements), nie jest [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop) funkcji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-251">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="50cdb-252">Odwołania do składników nie są przekazywane do kodu w języku JavaScript&mdash;są używane tylko w kodzie .NET.</span><span class="sxs-lookup"><span data-stu-id="50cdb-252">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="50cdb-253">Czy **nie** umożliwia odwołania do składników mutować stan składnikach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="50cdb-253">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="50cdb-254">Zamiast tego należy użyć normalnego parametry deklaratywne, aby przekazać dane do elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="50cdb-254">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="50cdb-255">To sprawia, że podrzędnych automatycznie rerender w właściwym czasie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-255">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="50cdb-256">Użyj @key do kontrolowania zachowania elementów i składniki</span><span class="sxs-lookup"><span data-stu-id="50cdb-256">Use @key to control the preservation of elements and components</span></span>

<span data-ttu-id="50cdb-257">Podczas renderowania listę elementów lub składniki i elementy lub składniki później zmienić, algorytm porównywanie Blazor firmy należy zdecydować, które z poprzednich elementów lub składniki mogą być zachowywane przez i jak obiekty modelu powinny być mapowane do nich.</span><span class="sxs-lookup"><span data-stu-id="50cdb-257">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="50cdb-258">Zwykle ten proces odbywa się automatycznie i można zignorować, ale istnieją przypadki, gdzie możesz chcieć kontrolować ten proces.</span><span class="sxs-lookup"><span data-stu-id="50cdb-258">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="50cdb-259">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="50cdb-259">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="50cdb-260">Zawartość `People` kolekcji mogą ulec zmianie z wstawiania, usunięty lub nowo porządkować wpisów.</span><span class="sxs-lookup"><span data-stu-id="50cdb-260">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="50cdb-261">Gdy składnik rerenders, `<DetailsEditor>` składnika mogą ulec zmianie, aby otrzymać inny `Details` wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="50cdb-261">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="50cdb-262">Może to spowodować rerendering bardziej skomplikowane niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="50cdb-262">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="50cdb-263">W niektórych przypadkach rerendering może prowadzić do widocznych różnicami, takie jak element utracone fokus.</span><span class="sxs-lookup"><span data-stu-id="50cdb-263">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="50cdb-264">Proces mapowania mogą być kontrolowane za pomocą `@key` atrybutu dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-264">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="50cdb-265">`@key` powoduje, że algorytm porównywanie zagwarantowanie zachowywania elementów lub składników opartych na wartość klucza:</span><span class="sxs-lookup"><span data-stu-id="50cdb-265">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="50cdb-266">Gdy `People` zmiany kolekcji algorytm porównywanie zachowuje skojarzenie między `<DetailsEditor>` wystąpień i `person` wystąpień:</span><span class="sxs-lookup"><span data-stu-id="50cdb-266">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="50cdb-267">Jeśli `Person` są usuwane z `People` listy, tylko odpowiednie `<DetailsEditor>` wystąpienie zostanie usunięte z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-267">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="50cdb-268">Pozostałe wystąpienia są pozostawione bez zmian.</span><span class="sxs-lookup"><span data-stu-id="50cdb-268">Other instances are left unchanged.</span></span>
* <span data-ttu-id="50cdb-269">Jeśli `Person` zostanie wstawione w niektórych pozycji na liście, jeden nowy `<DetailsEditor>` wystąpienia zostanie wstawione w tym w odpowiednim miejscu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-269">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="50cdb-270">Pozostałe wystąpienia są pozostawione bez zmian.</span><span class="sxs-lookup"><span data-stu-id="50cdb-270">Other instances are left unchanged.</span></span>
* <span data-ttu-id="50cdb-271">Jeśli `Person` wpisy są reorganizowane, odpowiedni `<DetailsEditor>` wystąpienia są zachowywane i nowo porządkować w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-271">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="50cdb-272">W niektórych przypadkach użycie `@key` zmniejsza złożoność rerendering i pozwala uniknąć potencjalnych problemów za pomocą stanowe części DOM zmiany, takie jak pozycja fokus.</span><span class="sxs-lookup"><span data-stu-id="50cdb-272">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50cdb-273">Klucze są lokalne dla każdego elementu kontenera lub składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-273">Keys are local to each container element or component.</span></span> <span data-ttu-id="50cdb-274">Klucze są *nie* porównane globalnie w dokumencie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-274">Keys are *not* compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="50cdb-275">Kiedy należy używać @key</span><span class="sxs-lookup"><span data-stu-id="50cdb-275">When to use @key</span></span>

<span data-ttu-id="50cdb-276">Zazwyczaj warto użyć `@key` zawsze, gdy lista jest renderowany (na przykład w `@foreach` bloku) oraz odpowiednią wartość istnieje w celu definiowania `@key`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-276">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="50cdb-277">Można również użyć `@key` aby zapobiec Blazor zachowywanie poddrzewo elementu lub składnika, gdy ulegnie zmianie obiektu:</span><span class="sxs-lookup"><span data-stu-id="50cdb-277">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="50cdb-278">Jeśli `@currentPerson` zmian `@key` atrybutu dyrektywy wymusza Blazor można odrzucić cały `<div>` i jego elementy podrzędne, jak i ponownej kompilacji poddrzewo w Interfejsie użytkownika za pomocą nowych elementów i składników.</span><span class="sxs-lookup"><span data-stu-id="50cdb-278">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="50cdb-279">Może to być przydatne, jeśli potrzebujesz gwarantuje nie stan interfejsu użytkownika są zachowywane podczas `@currentPerson` zmiany.</span><span class="sxs-lookup"><span data-stu-id="50cdb-279">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="50cdb-280">Kiedy nie należy używać @key</span><span class="sxs-lookup"><span data-stu-id="50cdb-280">When not to use @key</span></span>

<span data-ttu-id="50cdb-281">Występuje po negatywnie na wydajność, porównywanie za pomocą `@key`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-281">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="50cdb-282">Spadek wydajności nie jest duża, ale tylko określić `@key` Jeżeli kontrolowanie zachowania element lub składnika reguły korzystania z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-282">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="50cdb-283">Nawet wtedy, gdy `@key` nie jest używany, Blazor zachowuje wystąpienia elementu, jak i składnika podrzędnego możliwie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-283">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="50cdb-284">Tylko zaletą używania `@key` jest kontrola nad *jak* wystąpień modelu są mapowane na wystąpieniach zachowanych składnika, zamiast algorytm porównywanie, wybierając mapowania.</span><span class="sxs-lookup"><span data-stu-id="50cdb-284">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="50cdb-285">Wartości, których można użyć dla @key</span><span class="sxs-lookup"><span data-stu-id="50cdb-285">What values to use for @key</span></span>

<span data-ttu-id="50cdb-286">Ogólnie rzecz biorąc, dobrym pomysłem będzie podać jedną z następujących rodzajów wartość `@key`:</span><span class="sxs-lookup"><span data-stu-id="50cdb-286">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="50cdb-287">Model wystąpień obiektu (na przykład `Person` wystąpienia, tak jak w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="50cdb-287">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="50cdb-288">Gwarantuje to zachowanie oparte na równość odwołań obiektu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-288">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="50cdb-289">Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`, lub `Guid`).</span><span class="sxs-lookup"><span data-stu-id="50cdb-289">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="50cdb-290">Należy unikać podawania wartość, która może zostać nieoczekiwanie kolidują.</span><span class="sxs-lookup"><span data-stu-id="50cdb-290">Avoid supplying a value that can clash unexpectedly.</span></span> <span data-ttu-id="50cdb-291">Jeśli `@key="@someObject.GetHashCode()"` jest podany, mogą wystąpić nieoczekiwane kolizji kody skrótów niepowiązanych obiektów mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="50cdb-291">If `@key="@someObject.GetHashCode()"` is supplied, unexpected clashes may occur because the hash codes of unrelated objects can be the same.</span></span> <span data-ttu-id="50cdb-292">W przypadku konfliktu `@key` wartości są żądane w ramach tego samego nadrzędnego `@key` wartości nie będą uznawane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-292">If clashing `@key` values are requested within the same parent, the `@key` values won't be honored.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="50cdb-293">Cykl życia metody</span><span class="sxs-lookup"><span data-stu-id="50cdb-293">Lifecycle methods</span></span>

<span data-ttu-id="50cdb-294">`OnInitAsync` i `OnInit` wykonanie kodu, aby zainicjować składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-294">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="50cdb-295">Aby wykonać operację asynchroniczną, użyj `OnInitAsync` i `await` — słowo kluczowe o nieudanej operacji:</span><span class="sxs-lookup"><span data-stu-id="50cdb-295">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="50cdb-296">Operacja synchroniczna, można użyć `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="50cdb-296">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="50cdb-297">`OnParametersSetAsync` i `OnParametersSet` są wywoływane, gdy składnik, który otrzyma parametry od jego elementu nadrzędnego, a wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="50cdb-297">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="50cdb-298">Te metody są wykonywane po zainicjowaniu składnika i każdym składniku jest renderowany:</span><span class="sxs-lookup"><span data-stu-id="50cdb-298">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="50cdb-299">`OnAfterRenderAsync` i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-299">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="50cdb-300">Odwołania do elementu, jak i składnika zostaną wypełnione na tym etapie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-300">Element and component references are populated at this point.</span></span> <span data-ttu-id="50cdb-301">Aby wykonać kroki dodatkowe inicjowanie przy użyciu renderowanej zawartości, takich jak aktywacja bibliotek JavaScript innych firm, które działają na renderowanych elementów DOM, należy użyć tego etapu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-301">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

<span data-ttu-id="50cdb-302">`SetParameters` może zostać zastąpiona w celu wykonania kodu, zanim parametry są ustawione:</span><span class="sxs-lookup"><span data-stu-id="50cdb-302">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="50cdb-303">Jeśli `base.SetParameters` nie jest wywoływana, niestandardowy kod może interpretować przychodzących wartości parametrów w sposób wymagane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-303">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="50cdb-304">Na przykład przychodzące parametry nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-304">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="50cdb-305">`ShouldRender` może zostać zastąpiona w celu pomijania odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-305">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="50cdb-306">Jeśli implementacja zwraca `true`, interfejs użytkownika są odświeżane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-306">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="50cdb-307">Nawet wtedy, gdy `ShouldRender` jest została zastąpiona, składnik jest zawsze wstępnie renderowane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-307">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="50cdb-308">Usuwanie składnika z interfejsu IDisposable</span><span class="sxs-lookup"><span data-stu-id="50cdb-308">Component disposal with IDisposable</span></span>

<span data-ttu-id="50cdb-309">Jeśli składnik implementuje <xref:System.IDisposable>, [Dispose — metoda](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-309">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="50cdb-310">Używa następującego składnika `@implements IDisposable` i `Dispose` metody:</span><span class="sxs-lookup"><span data-stu-id="50cdb-310">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="50cdb-311">Routing</span><span class="sxs-lookup"><span data-stu-id="50cdb-311">Routing</span></span>

<span data-ttu-id="50cdb-312">Routing w Blazor odbywa się, podając szablonu trasy na każdej części dostępne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-312">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="50cdb-313">Gdy plik Razor za pomocą `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-313">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="50cdb-314">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="50cdb-314">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="50cdb-315">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-315">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="50cdb-316">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="50cdb-316">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="50cdb-317">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="50cdb-317">Route parameters</span></span>

<span data-ttu-id="50cdb-318">Składniki mogą odbierać parametrów trasy z szablonu trasy dostępnego w `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-318">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="50cdb-319">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-319">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="50cdb-320">*Składnik parametru trasy*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-320">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="50cdb-321">Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-321">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="50cdb-322">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="50cdb-322">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="50cdb-323">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="50cdb-323">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="50cdb-324">Dziedziczenie klasy bazowej dla środowiska "związane z kodem"</span><span class="sxs-lookup"><span data-stu-id="50cdb-324">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="50cdb-325">Pliki składników mieszać kod znaczników HTML i C# przetwarzania kodu w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="50cdb-325">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="50cdb-326">`@inherits` Dyrektywy może służyć do zapewnienia Blazor aplikacji w środowisku "związane z kodem" oddzielający składnika znaczników, od przetwarzania kodu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-326">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="50cdb-327">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasy bazowej, `BlazorRocksBase`w celu zapewnienia składnika właściwości i metody.</span><span class="sxs-lookup"><span data-stu-id="50cdb-327">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="50cdb-328">*Składnik Blazor Rocks*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-328">*Blazor Rocks component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="50cdb-329">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-329">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="50cdb-330">Klasa bazowa powinien pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-330">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="50cdb-331">Importowanie elementów</span><span class="sxs-lookup"><span data-stu-id="50cdb-331">Import components</span></span>

<span data-ttu-id="50cdb-332">Przestrzeń nazw składnika utworzone przy użyciu Razor opiera się na:</span><span class="sxs-lookup"><span data-stu-id="50cdb-332">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="50cdb-333">Projekt `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-333">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="50cdb-334">Ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-334">The path from the project root to the component.</span></span> <span data-ttu-id="50cdb-335">Na przykład `ComponentsSample/Pages/Index.razor` znajduje się w przestrzeni nazw `ComponentsSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-335">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="50cdb-336">Postępuj zgodnie z składniki C# nazwy zasad powiązania.</span><span class="sxs-lookup"><span data-stu-id="50cdb-336">Components follow C# name binding rules.</span></span> <span data-ttu-id="50cdb-337">W przypadku właściwości *Index.razor*, wszystkie składniki w tym samym folderze *stron*i folder nadrzędny *ComponentsSample*, znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-337">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="50cdb-338">Składniki zdefiniowane w innej przestrzeni nazw może być wprowadzana do zakresu przy użyciu firmy Razor [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-338">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="50cdb-339">Jeśli inny składnik `NavMenu.razor`, istnieje w folderze `ComponentsSample/Shared/`, składnika mogą być używane w `Index.razor` następującym `@using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="50cdb-339">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="50cdb-340">Składniki mogą się też odwoływać przy użyciu ich w pełni kwalifikowanych nazw, która eliminuje potrzebę [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="50cdb-340">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="50cdb-341">`global::` Kwalifikacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-341">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="50cdb-342">Importowanie elementów z aliasem `using` instrukcji (na przykład `@using Foo = Bar`) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-342">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="50cdb-343">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-343">Partially qualified names aren't supported.</span></span> <span data-ttu-id="50cdb-344">Na przykład dodanie `@using ComponentsSample` i odwoływanie się do `NavMenu.razor` z `<Shared.NavMenu></Shared.NavMenu>` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-344">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="razor-support"></a><span data-ttu-id="50cdb-345">Obsługa razor</span><span class="sxs-lookup"><span data-stu-id="50cdb-345">Razor support</span></span>

<span data-ttu-id="50cdb-346">**Dyrektywy razor**</span><span class="sxs-lookup"><span data-stu-id="50cdb-346">**Razor directives**</span></span>

<span data-ttu-id="50cdb-347">W poniższej tabeli przedstawiono dyrektywy razor.</span><span class="sxs-lookup"><span data-stu-id="50cdb-347">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="50cdb-348">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="50cdb-348">Directive</span></span> | <span data-ttu-id="50cdb-349">Opis</span><span class="sxs-lookup"><span data-stu-id="50cdb-349">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="50cdb-350">\@Kod</span><span class="sxs-lookup"><span data-stu-id="50cdb-350">\@code</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="50cdb-351">Dodaje C# blok kodu do składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-351">Adds a C# code block to a component.</span></span> <span data-ttu-id="50cdb-352">`@code` jest aliasem `@functions`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-352">`@code` is an alias of `@functions`.</span></span> <span data-ttu-id="50cdb-353">`@code` Zaleca się za pośrednictwem `@functions`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-353">`@code` is recommended over `@functions`.</span></span> <span data-ttu-id="50cdb-354">Więcej niż jeden `@code` bloku jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="50cdb-354">More than one `@code` block is permissible.</span></span> |
| [<span data-ttu-id="50cdb-355">\@Funkcje</span><span class="sxs-lookup"><span data-stu-id="50cdb-355">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="50cdb-356">Dodaje C# blok kodu do składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-356">Adds a C# code block to a component.</span></span> <span data-ttu-id="50cdb-357">Wybierz `@code` za pośrednictwem `@functions` dla C# bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-357">Choose `@code` over `@functions` for C# code blocks.</span></span> |
| `@implements` | <span data-ttu-id="50cdb-358">Implementuje interfejs dla klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-358">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="50cdb-359">\@Inherits</span><span class="sxs-lookup"><span data-stu-id="50cdb-359">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="50cdb-360">Zapewnia pełną kontrolę nad klasę, która dziedziczy składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-360">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="50cdb-361">\@wstrzykiwanie</span><span class="sxs-lookup"><span data-stu-id="50cdb-361">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="50cdb-362">Włącza usługi iniekcji z [kontener usługi](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="50cdb-362">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="50cdb-363">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="50cdb-363">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="50cdb-364">Określa składnik układu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-364">Specifies a layout component.</span></span> <span data-ttu-id="50cdb-365">Składniki układu są używane, aby uniknąć zduplikowania kodu i niespójności.</span><span class="sxs-lookup"><span data-stu-id="50cdb-365">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="50cdb-366">\@page</span><span class="sxs-lookup"><span data-stu-id="50cdb-366">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="50cdb-367">Określa, że składnik obsługi żądań bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="50cdb-367">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="50cdb-368">`@page` Dyrektywy można określić za pomocą trasy i opcjonalnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="50cdb-368">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="50cdb-369">W przeciwieństwie do stron Razor `@page` dyrektywy nie musi być pierwszą dyrektywę w górnej części pliku.</span><span class="sxs-lookup"><span data-stu-id="50cdb-369">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="50cdb-370">Aby uzyskać więcej informacji, zobacz [Routing](xref:blazor/routing).</span><span class="sxs-lookup"><span data-stu-id="50cdb-370">For more information, see [Routing](xref:blazor/routing).</span></span> |
| [<span data-ttu-id="50cdb-371">\@za pomocą</span><span class="sxs-lookup"><span data-stu-id="50cdb-371">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="50cdb-372">Dodaje C# `using` dyrektywy do klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-372">Adds the C# `using` directive to the generated component class.</span></span> <span data-ttu-id="50cdb-373">Udostępniono też wszystkich składników, które są zdefiniowane w tej przestrzeni nazw do zakresu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-373">This also brings all the components defined in that namespace into scope.</span></span> |
| [<span data-ttu-id="50cdb-374">\@Namespace</span><span class="sxs-lookup"><span data-stu-id="50cdb-374">\@namespace</span></span>](xref:mvc/views/razor#section-6) | <span data-ttu-id="50cdb-375">Ustawia obszar nazw, klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-375">Sets the namespace of the generated component class.</span></span> |
| [<span data-ttu-id="50cdb-376">\@Atrybut</span><span class="sxs-lookup"><span data-stu-id="50cdb-376">\@attribute</span></span>](xref:mvc/views/razor#section-7) | <span data-ttu-id="50cdb-377">Dodaje atrybut do klasy wygenerowanej składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-377">Adds an attribute to the generated component class.</span></span> |

<span data-ttu-id="50cdb-378">**Atrybuty warunkowe**</span><span class="sxs-lookup"><span data-stu-id="50cdb-378">**Conditional attributes**</span></span>

<span data-ttu-id="50cdb-379">Atrybuty warunkowe są renderowane na podstawie wartości platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="50cdb-379">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="50cdb-380">Jeśli wartość jest `false` lub `null`, ten atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="50cdb-380">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="50cdb-381">Jeśli wartość jest `true`, ten atrybut jest renderowany zminimalizowane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-381">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="50cdb-382">W poniższym przykładzie `IsCompleted` Określa, czy `checked` jest renderowany w znacznikach formantu:</span><span class="sxs-lookup"><span data-stu-id="50cdb-382">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="50cdb-383">Jeśli `IsCompleted` jest `true`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="50cdb-383">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="50cdb-384">Jeśli `IsCompleted` jest `false`, pole jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="50cdb-384">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="50cdb-385">**Dodatkowe informacje na temat Razor**</span><span class="sxs-lookup"><span data-stu-id="50cdb-385">**Additional information on Razor**</span></span>

<span data-ttu-id="50cdb-386">Aby uzyskać więcej informacji na temat Razor, zobacz [dokumentacja składni Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="50cdb-386">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="50cdb-387">Surowy kod HTML</span><span class="sxs-lookup"><span data-stu-id="50cdb-387">Raw HTML</span></span>

<span data-ttu-id="50cdb-388">Ciągi są zwykle renderowane przy użyciu modelu DOM węzły tekstowe, co oznacza, że żadnych znaczników, które mogą zawierać jest ignorowany i traktowany jako literał tekstowy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-388">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="50cdb-389">Aby renderować kod HTML, opakowywanie zawartości HTML w `MarkupString` wartość.</span><span class="sxs-lookup"><span data-stu-id="50cdb-389">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="50cdb-390">Wartość jest analizowany jako HTML lub SVG i wstawiony DOM.</span><span class="sxs-lookup"><span data-stu-id="50cdb-390">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="50cdb-391">Renderowanie surowe HTML skonstruowany z dowolnego niezaufanych źródło jest **zagrożenie dla bezpieczeństwa** i należy ich unikać!</span><span class="sxs-lookup"><span data-stu-id="50cdb-391">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="50cdb-392">Poniższy przykład pokazuje użycie `MarkupString` typu do dodania bloku zawartości statycznej HTML do wyniku renderowania składnika:</span><span class="sxs-lookup"><span data-stu-id="50cdb-392">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="50cdb-393">Składniki oparte na szablonach</span><span class="sxs-lookup"><span data-stu-id="50cdb-393">Templated components</span></span>

<span data-ttu-id="50cdb-394">Oparte na szablonach składniki są składniki, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które następnie mogą być używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-394">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="50cdb-395">Oparte na szablonach składniki umożliwiają tworzenie składników wyższego poziomu, które są bardziej wielokrotnego użytku, niż regularne składników.</span><span class="sxs-lookup"><span data-stu-id="50cdb-395">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="50cdb-396">Zawiera kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="50cdb-396">A couple of examples include:</span></span>

* <span data-ttu-id="50cdb-397">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka tabeli, wierszy i stopki.</span><span class="sxs-lookup"><span data-stu-id="50cdb-397">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="50cdb-398">Składnik listy, który umożliwia użytkownikowi określić szablon służący do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="50cdb-398">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="50cdb-399">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="50cdb-399">Template parameters</span></span>

<span data-ttu-id="50cdb-400">Oparte na szablonach składnika jest zdefiniowany, określając jeden lub więcej parametrów składnika typu `RenderFragment` lub `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-400">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="50cdb-401">Fragment renderowania reprezentuje segment interfejsu użytkownika, który jest renderowany przez składnik.</span><span class="sxs-lookup"><span data-stu-id="50cdb-401">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="50cdb-402">Fragment renderowania opcjonalnie przyjmuje parametr, który można określić, gdy jest wywoływany fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="50cdb-402">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="50cdb-403">*Części szablonu tabeli*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-403">*Table Template component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="50cdb-404">Podczas korzystania z szablonem składnik, można określić parametry szablonu przy użyciu elementy podrzędne, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="50cdb-404">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="50cdb-405">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="50cdb-405">Template context parameters</span></span>

<span data-ttu-id="50cdb-406">Składnik argumentów typu `RenderFragment<T>` przekazany jako elementy mają niejawny parametr o nazwie `context` (na przykład w poprzednim przykładzie kodu `@context.PetId`), można jednak zmienić przy użyciu nazwy parametru `Context` atrybutu w elemencie podrzędnym element.</span><span class="sxs-lookup"><span data-stu-id="50cdb-406">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="50cdb-407">W poniższym przykładzie `RowTemplate` elementu `Context` Określa atrybut `pet` parametru:</span><span class="sxs-lookup"><span data-stu-id="50cdb-407">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="50cdb-408">Alternatywnie, można określić `Context` atrybutu w elemencie składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-408">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="50cdb-409">Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-409">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="50cdb-410">Może to być przydatne, jeśli chcesz określić nazwę zawartości parametru zawartość elementu podrzędnego niejawne (bez żadnych zawijania elementu podrzędnego).</span><span class="sxs-lookup"><span data-stu-id="50cdb-410">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="50cdb-411">W poniższym przykładzie `Context` atrybutu jest wyświetlany na `TableTemplate` elementu i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="50cdb-411">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="50cdb-412">Składniki z kontrolą typów ogólnych</span><span class="sxs-lookup"><span data-stu-id="50cdb-412">Generic-typed components</span></span>

<span data-ttu-id="50cdb-413">Oparte na szablonach składniki są często objęte wpisane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-413">Templated components are often generically typed.</span></span> <span data-ttu-id="50cdb-414">Na przykład ogólnego składnika szablon widoku listy może zostać użyty do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="50cdb-414">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="50cdb-415">Aby zdefiniować element ogólny, należy użyć `@typeparam` dyrektywy, aby określić parametry typu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-415">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="50cdb-416">*Części szablonu ListView*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-416">*ListView Template component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="50cdb-417">Podczas korzystania z kontrolą typów ogólnych składników, jeśli jest to możliwe jest wnioskowany parametr typu:</span><span class="sxs-lookup"><span data-stu-id="50cdb-417">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="50cdb-418">W przeciwnym razie parametru typu muszą być jawnie określone za pomocą atrybutu, który odpowiada nazwie parametru typu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-418">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="50cdb-419">W poniższym przykładzie `TItem="Pet"` Określa typ:</span><span class="sxs-lookup"><span data-stu-id="50cdb-419">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="50cdb-420">Kaskadowe wartości i parametry</span><span class="sxs-lookup"><span data-stu-id="50cdb-420">Cascading values and parameters</span></span>

<span data-ttu-id="50cdb-421">W niektórych przypadkach jest wygodne, przepływ danych ze składnika nadrzędnego za pomocą składnika podrzędny [Parametry składnika](#component-parameters), szczególnie w przypadku kilku warstw składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-421">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="50cdb-422">Kaskadowe wartości i parametry rozwiązują ten problem, zapewniając wygodny sposób w celu Podaj wartość, aby wszystkie jego elementy potomne składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="50cdb-422">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="50cdb-423">Kaskadowe wartości i parametry oferują również podejście składników do zapewnienia koordynacji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-423">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="50cdb-424">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="50cdb-424">Theme example</span></span>

<span data-ttu-id="50cdb-425">W następującym *motyw* przykład z przykładowej aplikacji `ThemeInfo` klasa określa informacje o motywie przepływ w dół hierarchii składnika tak, aby udostępnić wszystkie przyciski w ramach danego część aplikacji, ten styl.</span><span class="sxs-lookup"><span data-stu-id="50cdb-425">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="50cdb-426">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-426">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="50cdb-427">Składnik nadrzędny można podać wartość kaskadowych za pomocą składnika kaskadowa wartość.</span><span class="sxs-lookup"><span data-stu-id="50cdb-427">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="50cdb-428">Składnik wartości kaskadowych otacza poddrzewo hierarchii składników i dostarcza pojedynczą wartość dla wszystkich składników w ramach tego poddrzewa.</span><span class="sxs-lookup"><span data-stu-id="50cdb-428">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="50cdb-429">Na przykład przykładowa aplikacja określa informacje o motywie (`ThemeInfo`) w jednej aplikacji układów jako parametr kaskadowych dla wszystkich składników, które tworzą treści układ `@Body` właściwości.</span><span class="sxs-lookup"><span data-stu-id="50cdb-429">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="50cdb-430">`ButtonClass` jest przypisywana wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-430">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="50cdb-431">Dowolny składnik podrzędny mogą używać tej właściwości, za pośrednictwem `ThemeInfo` cascading obiektu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-431">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="50cdb-432">*Kaskadowe składnik wartości parametrów układ*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-432">*Cascading Values Parameters Layout component*:</span></span>

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

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="50cdb-433">Zapewnienie korzystania z wartości kaskadowych, składniki deklarować parametrów kaskadowych przy użyciu `[CascadingParameter]` atrybut lub na podstawie wartości ciągu nazwy:</span><span class="sxs-lookup"><span data-stu-id="50cdb-433">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="50cdb-434">Powiązanie z wartością nazwy ciągu jest istotne, jeśli masz wiele kaskadowych wartości tego samego typu i potrzebujesz odróżnić je w ramach tej samej poddrzewo.</span><span class="sxs-lookup"><span data-stu-id="50cdb-434">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="50cdb-435">Kaskadowe wartości są powiązane kaskadowych parametry według typu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-435">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="50cdb-436">W przykładowej aplikacji wiąże składnika kaskadowych wartości parametrów motyw `ThemeInfo` kaskadowa wartość parametru kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="50cdb-436">In the sample app, the Cascading Values Parameters Theme component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="50cdb-437">Parametr służy do ustawiania klasę CSS dla jednego z przyciski wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="50cdb-437">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="50cdb-438">*Kaskadowe składnik wartości parametrów motyw*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-438">*Cascading Values Parameters Theme component*:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="50cdb-439">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="50cdb-439">TabSet example</span></span>

<span data-ttu-id="50cdb-440">Parametry kaskadowych również włączyć składników do współpracy w całej hierarchii składnika.</span><span class="sxs-lookup"><span data-stu-id="50cdb-440">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="50cdb-441">Na przykład, należy wziąć pod uwagę następujące *TabSet* przykładu w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-441">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="50cdb-442">Przykładowa aplikacja ma `ITab` interfejs, który karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="50cdb-442">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="50cdb-443">Składnik kaskadowych wartości parametrów TabSet używa ustawienia karty składnik, który zawiera kilka składników karty:</span><span class="sxs-lookup"><span data-stu-id="50cdb-443">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="50cdb-444">Składniki karty podrzędnej jawnie nie są przekazywane jako parametry można ustawić kartę.</span><span class="sxs-lookup"><span data-stu-id="50cdb-444">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="50cdb-445">Zamiast tego składniki karty podrzędnej są częścią zawartość elementu podrzędnego zestawu kartę.</span><span class="sxs-lookup"><span data-stu-id="50cdb-445">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="50cdb-446">Jednak ustawienia karty nadal musi wiedzieć o poszczególnych składników karty, dzięki czemu można renderować, nagłówki i aktywną kartę. Włączyć koordynacja bez konieczności dodatkowego kodu, Ustaw kartę składnika *może zapewnić sama jako wartość kaskadowych* , następnie zostaje pobrana przez podrzędny składniki kartę.</span><span class="sxs-lookup"><span data-stu-id="50cdb-446">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="50cdb-447">*Składnik TabSet*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-447">*TabSet component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="50cdb-448">Podrzędny przechwytywania składniki kartę zawierającego ustawienia karty jako parametr kaskadowych, więc składniki kartę dodają same siebie do ustawienia karty i współrzędnych karty, które jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="50cdb-448">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="50cdb-449">*Karta składników*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-449">*Tab component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="50cdb-450">Szablony razor</span><span class="sxs-lookup"><span data-stu-id="50cdb-450">Razor templates</span></span>

<span data-ttu-id="50cdb-451">Renderowanie fragmentów można zdefiniować przy użyciu składni szablonów Razor.</span><span class="sxs-lookup"><span data-stu-id="50cdb-451">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="50cdb-452">Szablony razor służą do definiowania fragmentu interfejsu użytkownika i założono następujący format:</span><span class="sxs-lookup"><span data-stu-id="50cdb-452">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="50cdb-453">Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="50cdb-453">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="50cdb-454">*Składnik Szablony razor*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-454">*Razor Templates component*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="50cdb-455">Renderowanie fragmentów zdefiniowane przy użyciu Razor szablony mogą być przekazywane jako argumenty do składników oparte na szablonach lub renderowane bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="50cdb-455">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="50cdb-456">Na przykład szablony poprzedniego bezpośrednio są renderowane w następującym kodem Razor:</span><span class="sxs-lookup"><span data-stu-id="50cdb-456">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="50cdb-457">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="50cdb-457">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="50cdb-458">Ręczne logiki RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="50cdb-458">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="50cdb-459">`Microsoft.AspNetCore.Components.RenderTree` udostępnia metody do manipulowania składników i elementów, w tym ręcznie w kompilowanie składników C# kodu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-459">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="50cdb-460">Korzystanie z `RenderTreeBuilder` do tworzenia składników to zaawansowany scenariusz.</span><span class="sxs-lookup"><span data-stu-id="50cdb-460">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="50cdb-461">Źle sformułowane składników (na przykład tag niezamknięty znaczników) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-461">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="50cdb-462">Należy wziąć pod uwagę następujący składnik Pet szczegóły mogą być ręcznie wbudowane w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="50cdb-462">Consider the following Pet Details component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@code
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="50cdb-463">W poniższym przykładzie pętli w `CreateComponent` metoda generuje trzy składniki Pet szczegóły.</span><span class="sxs-lookup"><span data-stu-id="50cdb-463">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="50cdb-464">Podczas wywoływania `RenderTreeBuilder` metody tworzenia składników (`OpenComponent` i `AddAttribute`), numery sekwencyjne są numery wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="50cdb-464">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="50cdb-465">Algorytm różnica Blazor opiera się na numery sekwencyjne distinct wierszy kodu, a nie odrębne wywołania wywołania.</span><span class="sxs-lookup"><span data-stu-id="50cdb-465">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="50cdb-466">Podczas tworzenia składnika za pomocą `RenderTreeBuilder` metody, umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-466">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="50cdb-467">**Za pomocą obliczeń lub licznika do generowania numer sekwencyjny może prowadzić do pogorszenia wydajności.**</span><span class="sxs-lookup"><span data-stu-id="50cdb-467">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="50cdb-468">Aby uzyskać więcej informacji, zobacz [sekwencji liczb odnoszą się do kolejności cyfry i nie wykonywania linii kodu](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) sekcji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-468">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="50cdb-469">*Wbudowane składnika zawartość*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-469">*Built Content component*:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="@RenderComponent">
    Create three Pet Details components
</button>

@code {
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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="50cdb-470">Numery sekwencyjne odnoszą się do kolejności cyfry i nie wykonywania wiersza kodu</span><span class="sxs-lookup"><span data-stu-id="50cdb-470">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="50cdb-471">Blazor `.razor` pliki są zawsze kompilowane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-471">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="50cdb-472">Jest to potencjalnie dużą zaletą dla `.razor` kroku kompilacji można się posłużyć do dodania informacji, który zwiększa wydajność aplikacji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="50cdb-472">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="50cdb-473">Przykład klawisza ulepszeniami obejmują *sekwencji numerów*.</span><span class="sxs-lookup"><span data-stu-id="50cdb-473">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="50cdb-474">Numery sekwencyjne wskazuje środowiska uruchomieniowego, które dane wyjściowe pochodzą które odrębne i uporządkowanych wiersze kodu.</span><span class="sxs-lookup"><span data-stu-id="50cdb-474">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="50cdb-475">Środowisko wykonawcze używa tych informacji do wygenerowania różnic wydajne drzewa liniowo, który jest znacznie szybsze niż zazwyczaj dla algorytmu diff drzewo Ogólne.</span><span class="sxs-lookup"><span data-stu-id="50cdb-475">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="50cdb-476">Należy wziąć pod uwagę następujące proste `.razor` pliku:</span><span class="sxs-lookup"><span data-stu-id="50cdb-476">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="50cdb-477">To kompiluje, aby podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="50cdb-477">This compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="50cdb-478">Kiedy ten kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, otrzymuje konstruktora:</span><span class="sxs-lookup"><span data-stu-id="50cdb-478">When this code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="50cdb-479">Sequence</span><span class="sxs-lookup"><span data-stu-id="50cdb-479">Sequence</span></span> | <span data-ttu-id="50cdb-480">Typ</span><span class="sxs-lookup"><span data-stu-id="50cdb-480">Type</span></span>      | <span data-ttu-id="50cdb-481">Dane</span><span class="sxs-lookup"><span data-stu-id="50cdb-481">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="50cdb-482">0</span><span class="sxs-lookup"><span data-stu-id="50cdb-482">0</span></span>        | <span data-ttu-id="50cdb-483">Węzeł tekstowy</span><span class="sxs-lookup"><span data-stu-id="50cdb-483">Text node</span></span> | <span data-ttu-id="50cdb-484">pierwszy</span><span class="sxs-lookup"><span data-stu-id="50cdb-484">First</span></span>  |
| <span data-ttu-id="50cdb-485">1</span><span class="sxs-lookup"><span data-stu-id="50cdb-485">1</span></span>        | <span data-ttu-id="50cdb-486">Węzeł tekstowy</span><span class="sxs-lookup"><span data-stu-id="50cdb-486">Text node</span></span> | <span data-ttu-id="50cdb-487">Sekunda</span><span class="sxs-lookup"><span data-stu-id="50cdb-487">Second</span></span> |

<span data-ttu-id="50cdb-488">Teraz załóżmy, że `someFlag` staje się `false`, i możemy ponownie renderowania.</span><span class="sxs-lookup"><span data-stu-id="50cdb-488">Now imagine that `someFlag` becomes `false`, and we render again.</span></span> <span data-ttu-id="50cdb-489">Tym razem odbiera konstruktora:</span><span class="sxs-lookup"><span data-stu-id="50cdb-489">This time, the builder receives:</span></span>

| <span data-ttu-id="50cdb-490">Sequence</span><span class="sxs-lookup"><span data-stu-id="50cdb-490">Sequence</span></span> | <span data-ttu-id="50cdb-491">Typ</span><span class="sxs-lookup"><span data-stu-id="50cdb-491">Type</span></span>       | <span data-ttu-id="50cdb-492">Dane</span><span class="sxs-lookup"><span data-stu-id="50cdb-492">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="50cdb-493">1</span><span class="sxs-lookup"><span data-stu-id="50cdb-493">1</span></span>        | <span data-ttu-id="50cdb-494">Węzeł tekstowy</span><span class="sxs-lookup"><span data-stu-id="50cdb-494">Text node</span></span>  | <span data-ttu-id="50cdb-495">Sekunda</span><span class="sxs-lookup"><span data-stu-id="50cdb-495">Second</span></span> |

<span data-ttu-id="50cdb-496">Gdy środowisko uruchomieniowe wykonuje różnic, widzi, elementu w sekwencji `0` została wyjęta, co generuje następujące proste *Przeprowadź edycję skryptu*:</span><span class="sxs-lookup"><span data-stu-id="50cdb-496">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="50cdb-497">Usuń pierwszy węzeł tekstowy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-497">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="50cdb-498">Co się nie uda, jeśli programowo wygenerować numery sekwencyjne</span><span class="sxs-lookup"><span data-stu-id="50cdb-498">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="50cdb-499">Sobie wyobrazić, autorem Poniższa logika konstruktora rendertree:</span><span class="sxs-lookup"><span data-stu-id="50cdb-499">Imagine instead that you wrote the following rendertree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="50cdb-500">Po pierwsze dane wyjściowe będą:</span><span class="sxs-lookup"><span data-stu-id="50cdb-500">Now the first output would be:</span></span>

<span data-ttu-id="50cdb-501">| Sekwencja | Typ | Dane || :------: | --------- | :--- : | | 0 | Węzeł tekstowy | Pierwszy || 1 | Węzeł tekstowy | Drugi |</span><span class="sxs-lookup"><span data-stu-id="50cdb-501">| Sequence | Type      | Data   | | :------: | --------- | :--- : | | 0        | Text node | First  | | 1        | Text node | Second |</span></span>

<span data-ttu-id="50cdb-502">Ten wynik jest identyczne z poprzednich przypadkiem, więc Brak problemów ujemna.</span><span class="sxs-lookup"><span data-stu-id="50cdb-502">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="50cdb-503">W drugiej renderowanie, gdy `someFlag` jest `false`, dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="50cdb-503">On the second render when `someFlag` is `false`, the output is:</span></span>

| <span data-ttu-id="50cdb-504">Sequence</span><span class="sxs-lookup"><span data-stu-id="50cdb-504">Sequence</span></span> | <span data-ttu-id="50cdb-505">Typ</span><span class="sxs-lookup"><span data-stu-id="50cdb-505">Type</span></span>      | <span data-ttu-id="50cdb-506">Dane</span><span class="sxs-lookup"><span data-stu-id="50cdb-506">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="50cdb-507">0</span><span class="sxs-lookup"><span data-stu-id="50cdb-507">0</span></span>        | <span data-ttu-id="50cdb-508">Węzeł tekstowy</span><span class="sxs-lookup"><span data-stu-id="50cdb-508">Text node</span></span> | <span data-ttu-id="50cdb-509">Sekunda</span><span class="sxs-lookup"><span data-stu-id="50cdb-509">Second</span></span> |

<span data-ttu-id="50cdb-510">Tym razem algorytm diff widzi, który *dwóch* nastąpiły zmiany, a algorytm generuje poniższy skrypt edycji:</span><span class="sxs-lookup"><span data-stu-id="50cdb-510">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="50cdb-511">Zmień wartość pierwszy węzeł tekstowy w celu `Second`.</span><span class="sxs-lookup"><span data-stu-id="50cdb-511">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="50cdb-512">Usuń drugi węzeł tekstowy.</span><span class="sxs-lookup"><span data-stu-id="50cdb-512">Remove the second text node.</span></span>

<span data-ttu-id="50cdb-513">Generowanie numery sekwencyjne utracił przydatne informacje o tym, gdzie `if/else` gałęzie i pętle znajdowały się w kodzie oryginalnym.</span><span class="sxs-lookup"><span data-stu-id="50cdb-513">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="50cdb-514">Skutkuje to różnic **dwa razy dłużej** tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="50cdb-514">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="50cdb-515">Jest to uproszczony przykład.</span><span class="sxs-lookup"><span data-stu-id="50cdb-515">This is a trivial example.</span></span> <span data-ttu-id="50cdb-516">W przypadku bardziej realistycznego o złożone i głęboko zagnieżdżonych struktur, a w szczególności z pętli przeprowadzanie bardziej dotkliwych jest spadek wydajności.</span><span class="sxs-lookup"><span data-stu-id="50cdb-516">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="50cdb-517">Zamiast natychmiast identyfikowanie, które bloki pętli lub gałęzi zostało wstawionych lub usunięty, algorytm różnicowego musi recurse głęboko do drzewa renderowania i zazwyczaj skompilować znacznie dłużej edycji skryptów, ponieważ jest on misinformed o tym, jak starych i nowych struktur odnoszą się do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="50cdb-517">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="50cdb-518">Wskazówki i wniosków</span><span class="sxs-lookup"><span data-stu-id="50cdb-518">Guidance and conclusions</span></span>

* <span data-ttu-id="50cdb-519">Wydajność aplikacji wystąpi, jeśli numerów sekwencji są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="50cdb-519">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="50cdb-520">Struktura nie można utworzyć liczby sekwencji automatycznie w czasie wykonywania, ponieważ nie istnieje niezbędne informacje, o ile nie są przechwytywane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-520">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="50cdb-521">Nie zapisuj długie bloki konstrukcyjne ręcznie zaimplementowane `RenderTreeBuilder` logiki.</span><span class="sxs-lookup"><span data-stu-id="50cdb-521">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="50cdb-522">Preferuj `.razor` pliki i umożliwić kompilatorowi do czynienia z numerami sekwencji.</span><span class="sxs-lookup"><span data-stu-id="50cdb-522">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="50cdb-523">Jeśli numery sekwencyjne są zapisane na stałe, algorytm diff wymaga jedynie, czy numery sekwencyjne wzrost wartości.</span><span class="sxs-lookup"><span data-stu-id="50cdb-523">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="50cdb-524">Wartość początkowa i luki są nieistotne.</span><span class="sxs-lookup"><span data-stu-id="50cdb-524">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="50cdb-525">Jedną z opcji uzasadnione jest wykorzystanie numer wiersza kodu jako numer sekwencyjny lub zacznij od zera i zwiększenia z nich lub setki (lub dowolnym preferowanym interwale).</span><span class="sxs-lookup"><span data-stu-id="50cdb-525">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="50cdb-526">Blazor używa numerów sekwencji, podczas gdy innych platform tworzenia interfejsu użytkownika porównywanie drzewa nie są używane.</span><span class="sxs-lookup"><span data-stu-id="50cdb-526">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="50cdb-527">Porównywanie jest znacznie szybszy, numery sekwencyjne są używane, gdy Blazor ma tę zaletę krok kompilacji, który dotyczy numerów sekwencyjnych automatycznie dla deweloperów autorstwa `.razor` plików.</span><span class="sxs-lookup"><span data-stu-id="50cdb-527">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>
