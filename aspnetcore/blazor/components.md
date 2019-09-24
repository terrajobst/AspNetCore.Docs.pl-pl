---
title: Tworzenie i używanie składników ASP.NET Core Razor
author: guardrex
description: Dowiedz się, jak tworzyć i używać składników Razor, w tym jak powiązać z danymi, obsługiwać zdarzenia i zarządzać cyklem życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/components
ms.openlocfilehash: 28e908968bd77c61da72d1bcc6032e580d15541b
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207271"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="d629e-103">Tworzenie i używanie składników ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="d629e-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="d629e-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d629e-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="d629e-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d629e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d629e-106">Aplikacje Blazor są kompilowane przy użyciu *składników*programu.</span><span class="sxs-lookup"><span data-stu-id="d629e-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="d629e-107">Składnik jest niezależnym fragmentem interfejsu użytkownika (UI), takim jak strona, okno dialogowe lub formularz.</span><span class="sxs-lookup"><span data-stu-id="d629e-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="d629e-108">Składnik zawiera znaczniki HTML i logikę przetwarzania wymagane do iniekcji danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="d629e-109">Składniki są elastyczne i lekkie.</span><span class="sxs-lookup"><span data-stu-id="d629e-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="d629e-110">Mogą być zagnieżdżane, ponownie używane i udostępniane między projektami.</span><span class="sxs-lookup"><span data-stu-id="d629e-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="d629e-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="d629e-111">Component classes</span></span>

<span data-ttu-id="d629e-112">Składniki są zaimplementowane w plikach składników [Razor](xref:mvc/views/razor) ( *. Razor*) przy użyciu kombinacji C# i znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="d629e-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="d629e-113">Składnik w Blazor jest formalnie określany jako *składnik Razor*.</span><span class="sxs-lookup"><span data-stu-id="d629e-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="d629e-114">Nazwa składnika musi rozpoczynać się wielką literą.</span><span class="sxs-lookup"><span data-stu-id="d629e-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="d629e-115">Na przykład *MyCoolComponent. Razor* jest prawidłowy, a *MyCoolComponent. Razor* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="d629e-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="d629e-116">Interfejs użytkownika dla składnika jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="d629e-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="d629e-117">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d629e-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="d629e-118">Po skompilowaniu aplikacji logika znaczników HTML i C# renderowania jest konwertowana na klasę składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="d629e-119">Nazwa wygenerowanej klasy jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="d629e-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="d629e-120">Elementy członkowskie klasy składnika są zdefiniowane w `@code` bloku.</span><span class="sxs-lookup"><span data-stu-id="d629e-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="d629e-121">`@code` W bloku stan składnika (właściwości, pola) jest określany przy użyciu metod obsługi zdarzeń lub definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="d629e-122">Dozwolony jest więcej `@code` niż jeden blok.</span><span class="sxs-lookup"><span data-stu-id="d629e-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="d629e-123">W poprzednich wersjach ASP.NET Core 3,0 `@functions` bloki zostały użyte do tego samego celu co `@code` bloki w składnikach Razor.</span><span class="sxs-lookup"><span data-stu-id="d629e-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="d629e-124">`@functions`bloki kontynuują działanie w składnikach Razor, ale zalecamy używanie `@code` bloku w ASP.NET Core 3,0 wersja zapoznawcza 6 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="d629e-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="d629e-125">Składowe składnika mogą być używane jako część logiki renderowania składnika przy użyciu C# wyrażeń, które zaczynają `@`się od.</span><span class="sxs-lookup"><span data-stu-id="d629e-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="d629e-126">Na przykład C# pole jest renderowane przez utworzenie prefiksu `@` do nazwy pola.</span><span class="sxs-lookup"><span data-stu-id="d629e-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="d629e-127">Poniższy przykład szacuje i renderuje:</span><span class="sxs-lookup"><span data-stu-id="d629e-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="d629e-128">`_headingFontStyle`na wartość właściwości CSS dla elementu `font-style`.</span><span class="sxs-lookup"><span data-stu-id="d629e-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="d629e-129">`_headingText`do zawartości `<h1>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d629e-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="d629e-130">Po pierwszym wyrenderowaniu składnika składnik generuje jego drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d629e-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="d629e-131">Blazor następnie porównuje nowe drzewo renderowania z poprzednią i zastosuje wszelkie modyfikacje Document Object Model przeglądarki (DOM).</span><span class="sxs-lookup"><span data-stu-id="d629e-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="d629e-132">Składniki są zwykłymi C# klasami i mogą być umieszczane w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d629e-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="d629e-133">Składniki, które generują strony sieci Web, zwykle znajdują się w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="d629e-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="d629e-134">Składniki niestronicowe są często umieszczane w folderze udostępnionym lub w folderze niestandardowym dodanym do projektu.</span><span class="sxs-lookup"><span data-stu-id="d629e-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="d629e-135">Aby użyć folderu niestandardowego, należy dodać przestrzeń nazw folderu niestandardowego do składnika nadrzędnego lub do pliku *_Imports. Razor* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="d629e-136">Na przykład następująca przestrzeń nazw sprawia, że składniki w folderze Components są dostępne, gdy główna przestrzeń nazw `WebApplication`aplikacji to:</span><span class="sxs-lookup"><span data-stu-id="d629e-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="d629e-137">Integrowanie składników w aplikacjach Razor Pages i MVC</span><span class="sxs-lookup"><span data-stu-id="d629e-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="d629e-138">Używaj składników z istniejącymi aplikacjami Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="d629e-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="d629e-139">Nie ma potrzeby ponownego zapisywania istniejących stron lub widoków w celu używania składników Razor.</span><span class="sxs-lookup"><span data-stu-id="d629e-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="d629e-140">Gdy strona lub widok są renderowane, składniki są wstępnie renderowane w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="d629e-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="d629e-141">Aby renderować składnik ze strony lub widoku, użyj `RenderComponentAsync<TComponent>` metody pomocnika HTML:</span><span class="sxs-lookup"><span data-stu-id="d629e-141">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="d629e-142">Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true".</span><span class="sxs-lookup"><span data-stu-id="d629e-142">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="d629e-143">Składniki nie mogą używać scenariuszy dotyczących widoków i stron, takich jak częściowe widoki i sekcje.</span><span class="sxs-lookup"><span data-stu-id="d629e-143">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="d629e-144">Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.</span><span class="sxs-lookup"><span data-stu-id="d629e-144">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="d629e-145">Aby uzyskać więcej informacji na temat sposobu renderowania składników i zarządzania stanem składnika w aplikacjach serwera Blazor, zobacz <xref:blazor/hosting-models> artykuł.</span><span class="sxs-lookup"><span data-stu-id="d629e-145">For more information on how components are rendered and component state is managed in Blazor Server apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="d629e-146">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="d629e-146">Use components</span></span>

<span data-ttu-id="d629e-147">Składniki mogą zawierać inne składniki, deklarując je za pomocą składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="d629e-147">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="d629e-148">Znaczniki użycia składnika wyglądają jak tag HTML, gdzie nazwa znacznika jest typem składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-148">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="d629e-149">W powiązaniu atrybutu rozróżniana jest wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="d629e-149">Attribute binding is case sensitive.</span></span> <span data-ttu-id="d629e-150">Na przykład `@bind` prawidłowe i `@Bind` jest nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="d629e-150">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="d629e-151">Poniższy znacznik w *indeksie. Razor* renderuje `HeadingComponent` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="d629e-151">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="d629e-152">*Składniki/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d629e-152">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="d629e-153">Jeśli składnik zawiera element HTML z wielką literą, która nie jest zgodna z nazwą składnika, jest emitowane ostrzeżenie wskazujące, że element ma nieoczekiwaną nazwę.</span><span class="sxs-lookup"><span data-stu-id="d629e-153">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="d629e-154">`@using` Dodanie instrukcji dla przestrzeni nazw składnika sprawia, że składnik jest dostępny, co spowoduje usunięcie tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="d629e-154">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="d629e-155">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="d629e-155">Component parameters</span></span>

<span data-ttu-id="d629e-156">Składniki mogą mieć *Parametry składnika*, które są zdefiniowane przy użyciu właściwości publicznych w klasie składnika z `[Parameter]` atrybutem.</span><span class="sxs-lookup"><span data-stu-id="d629e-156">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="d629e-157">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="d629e-157">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="d629e-158">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d629e-158">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="d629e-159">W poniższym przykładzie `ParentComponent` `Title` Właściwość ustawia wartość właściwości `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="d629e-159">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="d629e-160">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d629e-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="d629e-161">Zawartość podrzędna</span><span class="sxs-lookup"><span data-stu-id="d629e-161">Child content</span></span>

<span data-ttu-id="d629e-162">Składniki mogą ustawiać zawartość innego składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-162">Components can set the content of another component.</span></span> <span data-ttu-id="d629e-163">Składnik Assigner zawiera zawartość między tagami, które określają składnik do odbioru.</span><span class="sxs-lookup"><span data-stu-id="d629e-163">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="d629e-164">W poniższym przykładzie `ChildComponent` `ChildContent` ma właściwość, która reprezentuje element `RenderFragment`, który reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="d629e-164">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="d629e-165">Wartość `ChildContent` jest umieszczana w znacznikach składnika, gdzie zawartość powinna być renderowana.</span><span class="sxs-lookup"><span data-stu-id="d629e-165">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="d629e-166">Wartość `ChildContent` jest odbierana ze składnika nadrzędnego i renderowany w `panel-body`panelu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d629e-166">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="d629e-167">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d629e-167">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="d629e-168">Właściwość otrzymująca `RenderFragment` zawartość musi być nazywana `ChildContent` Konwencją.</span><span class="sxs-lookup"><span data-stu-id="d629e-168">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="d629e-169">Poniższe elementy `ParentComponent` mogą zapewnić zawartość do `ChildComponent` renderowania przez `<ChildComponent>` umieszczenie zawartości wewnątrz tagów.</span><span class="sxs-lookup"><span data-stu-id="d629e-169">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="d629e-170">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d629e-170">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="d629e-171">Korzystając atrybutów i dowolne parametry</span><span class="sxs-lookup"><span data-stu-id="d629e-171">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="d629e-172">Składniki mogą przechwytywać i renderować dodatkowe atrybuty oprócz zadeklarowanych parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-172">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="d629e-173">Dodatkowe atrybuty mogą być przechwytywane w słowniku, a następnie *splatted* do elementu, gdy składnik jest renderowany przy [@attributes](xref:mvc/views/razor#attributes) użyciu dyrektywy Razor.</span><span class="sxs-lookup"><span data-stu-id="d629e-173">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="d629e-174">Ten scenariusz jest przydatny podczas definiowania składnika, który generuje element znaczników, który obsługuje różne dostosowania.</span><span class="sxs-lookup"><span data-stu-id="d629e-174">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="d629e-175">Na przykład może być żmudnym do definiowania atrybutów oddzielnie dla `<input>` , który obsługuje wiele parametrów.</span><span class="sxs-lookup"><span data-stu-id="d629e-175">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="d629e-176">W `<input>` poniższym przykładzie pierwszy element (`id="useIndividualParams"`) używa pojedynczych parametrów składnika, podczas gdy drugi `<input>` element (`id="useAttributesDict"`) używa atrybutu korzystając:</span><span class="sxs-lookup"><span data-stu-id="d629e-176">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="d629e-177">Typ parametru musi być zaimplementowany `IEnumerable<KeyValuePair<string, object>>` przy użyciu kluczy ciągu.</span><span class="sxs-lookup"><span data-stu-id="d629e-177">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="d629e-178">Korzystanie `IReadOnlyDictionary<string, object>` z programu jest również opcją w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="d629e-178">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="d629e-179">Renderowane `<input>` elementy korzystające z obu metod są identyczne:</span><span class="sxs-lookup"><span data-stu-id="d629e-179">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="d629e-180">Aby zaakceptować dowolne atrybuty, zdefiniuj parametr składnika przy użyciu `[Parameter]` atrybutu `CaptureUnmatchedValues` z właściwością ustawioną na `true`:</span><span class="sxs-lookup"><span data-stu-id="d629e-180">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="d629e-181">`CaptureUnmatchedValues` Właściwość on`[Parameter]` umożliwia dopasowanie parametru do wszystkich atrybutów, które nie są zgodne z żadnym innym parametrem.</span><span class="sxs-lookup"><span data-stu-id="d629e-181">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="d629e-182">Składnik może definiować tylko jeden parametr z `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="d629e-182">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="d629e-183">Typ właściwości używany z elementem `CaptureUnmatchedValues` musi być możliwy do przypisania `Dictionary<string, object>` z klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="d629e-183">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="d629e-184">`IEnumerable<KeyValuePair<string, object>>`lub `IReadOnlyDictionary<string, object>` są również opcje w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="d629e-184">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="d629e-185">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="d629e-185">Data binding</span></span>

<span data-ttu-id="d629e-186">Powiązanie danych zarówno ze składnikami, jak i elementami modelu dom jest [@bind](xref:mvc/views/razor#bind) realizowane przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d629e-186">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="d629e-187">Poniższy przykład wiąże `_italicsCheck` pole z zaznaczonym stanem pola wyboru:</span><span class="sxs-lookup"><span data-stu-id="d629e-187">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="d629e-188">Gdy to pole wyboru jest zaznaczone i wyczyszczone, wartość właściwości jest aktualizowana `true` odpowiednio `false`do i.</span><span class="sxs-lookup"><span data-stu-id="d629e-188">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="d629e-189">To pole wyboru jest aktualizowane w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, a nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="d629e-189">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="d629e-190">Ze względu na to, że składniki są renderowane po wykonaniu kodu procedury obsługi zdarzeń, aktualizacje właściwości są zwykle odzwierciedlane natychmiast w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-190">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="d629e-191">Używanie `@bind` `<input @bind="CurrentValue" />`z właściwością () jest zasadniczo równoważne z następującymi: `CurrentValue`</span><span class="sxs-lookup"><span data-stu-id="d629e-191">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="d629e-192">Gdy składnik jest renderowany, `value` element wejściowy pochodzi `CurrentValue` z właściwości.</span><span class="sxs-lookup"><span data-stu-id="d629e-192">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="d629e-193">Gdy użytkownik wpisze w polu tekstowym, `onchange` zdarzenie jest wyzwalane, `CurrentValue` a właściwość jest ustawiona na wartość zmieniona.</span><span class="sxs-lookup"><span data-stu-id="d629e-193">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="d629e-194">W rzeczywistości generowanie kodu jest nieco bardziej skomplikowane, ponieważ `@bind` obsługuje kilka przypadków, w których są wykonywane konwersje typów.</span><span class="sxs-lookup"><span data-stu-id="d629e-194">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="d629e-195">W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia `value` z atrybutem i obsługuje zmiany przy użyciu zarejestrowanej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="d629e-195">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="d629e-196">`onchange` Oprócz obsługi[@bind-value:event](xref:mvc/views/razor#bind)zdarzeń ze `@bind` składnią właściwość lub pole można powiązać przy użyciu [@bind-value](xref:mvc/views/razor#bind) innych zdarzeń, `event` określając atrybut z parametrem ().</span><span class="sxs-lookup"><span data-stu-id="d629e-196">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="d629e-197">Poniższy przykład wiąże się z `CurrentValue` właściwością `oninput` zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="d629e-197">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="d629e-198">W przeciwieństwie do `onchange`, które jest wyzwalane, gdy `oninput` element utraci fokus, jest uruchamiany po zmianie wartości pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="d629e-198">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="d629e-199">**Wartości niemożliwy do przeanalizowania**</span><span class="sxs-lookup"><span data-stu-id="d629e-199">**Unparsable values**</span></span>

<span data-ttu-id="d629e-200">Gdy użytkownik dostarczy wartość niemożliwy do przeanalizowania do elementu powiązanego z danymi, wartość niemożliwy do przeanalizowania jest automatycznie przywracana do poprzedniej wartości po wyzwoleniu zdarzenia bind.</span><span class="sxs-lookup"><span data-stu-id="d629e-200">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="d629e-201">Rozważmy następujący scenariusz:</span><span class="sxs-lookup"><span data-stu-id="d629e-201">Consider the following scenario:</span></span>

* <span data-ttu-id="d629e-202">Element jest powiązany `int` z typem `123`z początkową wartością: `<input>`</span><span class="sxs-lookup"><span data-stu-id="d629e-202">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="d629e-203">Użytkownik aktualizuje wartość elementu `123.45` na liście na stronie i zmienia fokus elementu.</span><span class="sxs-lookup"><span data-stu-id="d629e-203">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="d629e-204">W poprzednim scenariuszu wartość elementu jest przywracana `123`.</span><span class="sxs-lookup"><span data-stu-id="d629e-204">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="d629e-205">Gdy wartość `123.45` zostanie odrzucona na korzyść oryginalnej `123`wartości, użytkownik rozumie, że ich wartość nie została zaakceptowana.</span><span class="sxs-lookup"><span data-stu-id="d629e-205">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="d629e-206">Domyślnie powiązanie dotyczy `onchange` zdarzenia elementu (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="d629e-206">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="d629e-207">Użyj `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` , aby ustawić inne zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="d629e-207">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="d629e-208">W przypadku`@bind-value:event="oninput"`zdarzenia () jego wersja następuje po naciśnięciu klawisza, które wprowadza niemożliwy do przeanalizowania wartość. `oninput`</span><span class="sxs-lookup"><span data-stu-id="d629e-208">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="d629e-209">Podczas określania wartości `oninput` docelowej dla zdarzenia `int`z typem związanym użytkownik nie jest w trakcie wpisywania `.` znaku.</span><span class="sxs-lookup"><span data-stu-id="d629e-209">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="d629e-210">`.` Znak zostanie natychmiast usunięty, więc użytkownik otrzymuje natychmiastową opinię, że dozwolone są tylko liczby całkowite.</span><span class="sxs-lookup"><span data-stu-id="d629e-210">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="d629e-211">Istnieją scenariusze, w których przywrócenie wartości `oninput` zdarzenia nie jest idealne, na przykład wtedy, gdy użytkownik powinien mieć możliwość wyczyszczenia `<input>` wartości, która nie może być przewidziana.</span><span class="sxs-lookup"><span data-stu-id="d629e-211">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="d629e-212">Alternatywy obejmują:</span><span class="sxs-lookup"><span data-stu-id="d629e-212">Alternatives include:</span></span>

* <span data-ttu-id="d629e-213">Nie używaj `oninput` zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d629e-213">Don't use the `oninput` event.</span></span> <span data-ttu-id="d629e-214">Użyj zdarzenia domyślnego `onchange` (`@bind="{PROPERTY OR FIELD}"`), gdzie nieprawidłowa wartość nie jest przywracana, dopóki element nie utraci fokusu.</span><span class="sxs-lookup"><span data-stu-id="d629e-214">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="d629e-215">Powiąż z typem dopuszczającym wartość `int?` null `string`, taką jak lub, i podaj logikę niestandardową do obsługi nieprawidłowych wpisów.</span><span class="sxs-lookup"><span data-stu-id="d629e-215">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="d629e-216">Użyj [składnika walidacji formularza](xref:blazor/forms-validation), takiego jak `InputNumber` lub `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="d629e-216">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="d629e-217">Składniki walidacji formularza mają wbudowaną obsługę zarządzania nieprawidłowymi danymi wejściowymi.</span><span class="sxs-lookup"><span data-stu-id="d629e-217">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="d629e-218">Składniki walidacji formularza:</span><span class="sxs-lookup"><span data-stu-id="d629e-218">Form validation components:</span></span>
  * <span data-ttu-id="d629e-219">Zezwalaj użytkownikowi na dostarczenie nieprawidłowych danych wejściowych i odbieranie błędów walidacji w skojarzonym `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="d629e-219">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="d629e-220">Wyświetlaj błędy walidacji w interfejsie użytkownika bez zakłócania wprowadzania dodatkowych danych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-220">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="d629e-221">**Globalizacja**</span><span class="sxs-lookup"><span data-stu-id="d629e-221">**Globalization**</span></span>

<span data-ttu-id="d629e-222">`@bind`wartości są sformatowane do wyświetlania i analizowane przy użyciu reguł bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="d629e-222">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="d629e-223">Dla bieżącej kultury można uzyskać dostęp z <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> właściwości.</span><span class="sxs-lookup"><span data-stu-id="d629e-223">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="d629e-224">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) jest używany dla następujących typów pól (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="d629e-224">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="d629e-225">Poprzednie typy pól:</span><span class="sxs-lookup"><span data-stu-id="d629e-225">The preceding field types:</span></span>

* <span data-ttu-id="d629e-226">Są wyświetlane przy użyciu odpowiednich reguł formatowania opartych na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d629e-226">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="d629e-227">Nie może zawierać tekstu o dowolnym formacie.</span><span class="sxs-lookup"><span data-stu-id="d629e-227">Can't contain free-form text.</span></span>
* <span data-ttu-id="d629e-228">Podaj charakterystykę interakcji użytkownika w oparciu o implementację przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="d629e-228">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="d629e-229">Następujące typy pól mają określone wymagania dotyczące formatowania i nie są obecnie obsługiwane przez Blazor, ponieważ nie są obsługiwane przez wszystkie główne przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="d629e-229">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="d629e-230">`@bind`obsługuje parametr, aby zapewnić analizę i formatowanie wartości. <xref:System.Globalization.CultureInfo?displayProperty=fullName> `@bind:culture`</span><span class="sxs-lookup"><span data-stu-id="d629e-230">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="d629e-231">Określanie kultury nie jest zalecane w `date` przypadku używania typów pól i. `number`</span><span class="sxs-lookup"><span data-stu-id="d629e-231">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="d629e-232">`date`i `number` ma wbudowaną obsługę Blazor, która dostarcza wymaganą kulturę.</span><span class="sxs-lookup"><span data-stu-id="d629e-232">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="d629e-233">Aby uzyskać informacje na temat sposobu ustawiania kultury użytkownika, zobacz sekcję [Lokalizacja](#localization) .</span><span class="sxs-lookup"><span data-stu-id="d629e-233">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="d629e-234">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="d629e-234">**Format strings**</span></span>

<span data-ttu-id="d629e-235">Powiązanie danych działa z <xref:System.DateTime> ciągami formatu [@bind:format](xref:mvc/views/razor#bind)przy użyciu.</span><span class="sxs-lookup"><span data-stu-id="d629e-235">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="d629e-236">W tej chwili nie są dostępne inne wyrażenia formatu, takie jak formaty walutowe lub liczbowe.</span><span class="sxs-lookup"><span data-stu-id="d629e-236">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="d629e-237">W poprzednim kodzie `<input>` typ pola (`type`) elementu jest wartością domyślną `text`.</span><span class="sxs-lookup"><span data-stu-id="d629e-237">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="d629e-238">`@bind:format`jest obsługiwana w celu powiązania następujących typów .NET:</span><span class="sxs-lookup"><span data-stu-id="d629e-238">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="d629e-239"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="d629e-239"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="d629e-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="d629e-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="d629e-241">Ten `@bind:format` atrybut określa format daty, który ma zostać zastosowany `value` do `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d629e-241">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="d629e-242">Format jest również używany do analizowania wartości w przypadku `onchange` wystąpienia zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d629e-242">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="d629e-243">Określanie formatu dla `date` typu pola nie jest zalecane, ponieważ Blazor ma wbudowaną obsługę formatowania dat.</span><span class="sxs-lookup"><span data-stu-id="d629e-243">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="d629e-244">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="d629e-244">**Component parameters**</span></span>

<span data-ttu-id="d629e-245">Powiązanie rozpoznaje parametry składnika, gdzie `@bind-{property}` można powiązać wartość właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="d629e-245">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="d629e-246">Następujący składnik podrzędny (`ChildComponent`) `Year` ma parametr składnika i `YearChanged` wywołanie zwrotne:</span><span class="sxs-lookup"><span data-stu-id="d629e-246">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="d629e-247">`EventCallback<T>`wyjaśniono w sekcji [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="d629e-247">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="d629e-248">Poniższy składnik nadrzędny używa `ChildComponent` i wiąże `ParentYear` parametr `Year` z elementu nadrzędnego z parametrem w składniku podrzędnym:</span><span class="sxs-lookup"><span data-stu-id="d629e-248">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
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

<span data-ttu-id="d629e-249">Załadowanie `ParentComponent` powoduje utworzenie następującej adjustacji:</span><span class="sxs-lookup"><span data-stu-id="d629e-249">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="d629e-250">Jeśli wartość `ParentYear` właściwości zostanie zmieniona przez wybranie przycisku `ParentComponent`w `ChildComponent` , `Year` właściwość jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="d629e-250">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="d629e-251">Nowa wartość `Year` jest renderowana w interfejsie użytkownika podczas jego `ParentComponent` renderowania:</span><span class="sxs-lookup"><span data-stu-id="d629e-251">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="d629e-252">Parametr jest możliwy do powiązania, ponieważ ma zdarzenie towarzyszące `YearChanged` `Year` pasujące do typu parametru. `Year`</span><span class="sxs-lookup"><span data-stu-id="d629e-252">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="d629e-253">Zgodnie z Konwencją, `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo równoważne zapisowi:</span><span class="sxs-lookup"><span data-stu-id="d629e-253">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="d629e-254">Ogólnie rzecz biorąc, właściwość może być powiązana z odpowiednią obsługą zdarzeń `@bind-property:event` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d629e-254">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="d629e-255">Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` użyciem następujących dwóch atrybutów:</span><span class="sxs-lookup"><span data-stu-id="d629e-255">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="d629e-256">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="d629e-256">Event handling</span></span>

<span data-ttu-id="d629e-257">Składniki Razor zapewniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="d629e-257">Razor components provide event handling features.</span></span> <span data-ttu-id="d629e-258">Dla atrybutu elementu HTML o nazwie `on{event}` (na `onclick` przykład i `onsubmit`) z wartością typu delegata składniki Razor traktują wartość atrybutu jako procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="d629e-258">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="d629e-259">Nazwa atrybutu jest zawsze sformatowana [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="d629e-259">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="d629e-260">Poniższy kod wywołuje metodę, `UpdateHeading` gdy przycisk zostanie wybrany w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d629e-260">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
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

<span data-ttu-id="d629e-261">Poniższy kod wywołuje metodę, `CheckChanged` gdy pole wyboru zostanie zmienione w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d629e-261">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="d629e-262">Procedury obsługi zdarzeń mogą również być asynchroniczne i zwracać <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="d629e-262">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="d629e-263">Nie ma potrzeby ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="d629e-263">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="d629e-264">Wyjątki są rejestrowane, gdy wystąpią.</span><span class="sxs-lookup"><span data-stu-id="d629e-264">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="d629e-265">W poniższym przykładzie `UpdateHeading` jest wywoływana asynchronicznie po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="d629e-265">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
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

### <a name="event-argument-types"></a><span data-ttu-id="d629e-266">Typy argumentów zdarzeń</span><span class="sxs-lookup"><span data-stu-id="d629e-266">Event argument types</span></span>

<span data-ttu-id="d629e-267">W przypadku niektórych zdarzeń dozwolone są typy argumentów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="d629e-267">For some events, event argument types are permitted.</span></span> <span data-ttu-id="d629e-268">Jeśli dostęp do jednego z tych typów zdarzeń nie jest konieczny, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="d629e-268">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="d629e-269">Obsługiwane [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) są przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="d629e-269">Supported [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) are shown in the following table.</span></span>

| <span data-ttu-id="d629e-270">Zdarzenie</span><span class="sxs-lookup"><span data-stu-id="d629e-270">Event</span></span> | <span data-ttu-id="d629e-271">Class</span><span class="sxs-lookup"><span data-stu-id="d629e-271">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="d629e-272">Schowek</span><span class="sxs-lookup"><span data-stu-id="d629e-272">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="d629e-273">Przeciągnij</span><span class="sxs-lookup"><span data-stu-id="d629e-273">Drag</span></span>             | <span data-ttu-id="d629e-274">`DragEventArgs`&ndash; iprzytrzymaj`DataTransferItem` przeciągane dane elementu. `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="d629e-274">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="d629e-275">Błąd</span><span class="sxs-lookup"><span data-stu-id="d629e-275">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="d629e-276">Fokus</span><span class="sxs-lookup"><span data-stu-id="d629e-276">Focus</span></span>            | <span data-ttu-id="d629e-277">`FocusEventArgs`Nie obejmuje obsługi dla `relatedTarget`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="d629e-277">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="d629e-278">`<input>`stąp</span><span class="sxs-lookup"><span data-stu-id="d629e-278">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="d629e-279">Klawiatura</span><span class="sxs-lookup"><span data-stu-id="d629e-279">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="d629e-280">Wskaźnik</span><span class="sxs-lookup"><span data-stu-id="d629e-280">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="d629e-281">Wskaźnik myszy</span><span class="sxs-lookup"><span data-stu-id="d629e-281">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="d629e-282">Kółko myszy</span><span class="sxs-lookup"><span data-stu-id="d629e-282">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="d629e-283">Postęp</span><span class="sxs-lookup"><span data-stu-id="d629e-283">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="d629e-284">Dotyk</span><span class="sxs-lookup"><span data-stu-id="d629e-284">Touch</span></span>            | <span data-ttu-id="d629e-285">`TouchEventArgs`&ndash; reprezentujepojedynczypunktkontaktunaurządzeniuz`TouchPoint` wrażliwym dotknięciem.</span><span class="sxs-lookup"><span data-stu-id="d629e-285">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="d629e-286">Aby uzyskać informacje o zachowaniu właściwości i obsłudze zdarzeń zdarzeń w powyższej tabeli, zobacz [EventArgs Classes in source Reference (ASPNET/AspNetCore Release/3.0-preview9)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="d629e-286">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0-preview9 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="d629e-287">Wyrażenia lambda</span><span class="sxs-lookup"><span data-stu-id="d629e-287">Lambda expressions</span></span>

<span data-ttu-id="d629e-288">Wyrażenia lambda mogą być również używane:</span><span class="sxs-lookup"><span data-stu-id="d629e-288">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="d629e-289">Często wygodnie jest blisko dodatkowych wartości, na przykład podczas iteracji na zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="d629e-289">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="d629e-290">Poniższy przykład tworzy trzy przyciski, z których każdy wywołuje `UpdateHeading` przekazanie argumentu zdarzenia (`MouseEventArgs`) i jego numer przycisku (`buttonNumber`), po wybraniu w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d629e-290">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="d629e-291">**Nie** używaj zmiennej Loop (`i`) w `for` pętli bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="d629e-291">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="d629e-292">W przeciwnym razie ta sama zmienna jest używana przez wszystkie wyrażenia `i`lambda, co sprawia, że wartość jest taka sama we wszystkich lambdach.</span><span class="sxs-lookup"><span data-stu-id="d629e-292">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="d629e-293">Zawsze Przechwytuj wartość w zmiennej lokalnej (`buttonNumber` w poprzednim przykładzie), a następnie użyj jej.</span><span class="sxs-lookup"><span data-stu-id="d629e-293">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="d629e-294">EventCallback</span><span class="sxs-lookup"><span data-stu-id="d629e-294">EventCallback</span></span>

<span data-ttu-id="d629e-295">Typowym scenariuszem ze składnikami zagnieżdżonymi jest uruchomienie metody składnika nadrzędnego, gdy występuje&mdash;zdarzenie składnika podrzędnego, gdy wystąpi zdarzenie w elemencie `onclick` podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="d629e-295">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="d629e-296">Aby uwidocznić zdarzenia między składnikami, `EventCallback`Użyj.</span><span class="sxs-lookup"><span data-stu-id="d629e-296">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="d629e-297">Składnik nadrzędny może przypisać metodę wywołania zwrotnego do składnika `EventCallback`podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d629e-297">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="d629e-298">W przykładowej aplikacji pokazano, jak `onclick` program obsługi przycisku został `EventCallback` skonfigurowany tak, aby otrzymać delegata z przykładu `ParentComponent`. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="d629e-298">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="d629e-299">Typ ma wartość, która jest odpowiednia dla `onclick` zdarzenia z urządzenia peryferyjnego: `MouseEventArgs` `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="d629e-299">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="d629e-300">Ustawia element podrzędny `ShowMessage`dojegometody: `EventCallback<T>` `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="d629e-300">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="d629e-301">Gdy przycisk zostanie wybrany w `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="d629e-301">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="d629e-302">`ParentComponent` Metoda`ShowMessage` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="d629e-302">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="d629e-303">`messageText`jest aktualizowany i wyświetlany w `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="d629e-303">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="d629e-304">Wywołanie `StateHasChanged` nie jest wymagane w metodzie wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="d629e-304">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="d629e-305">`StateHasChanged`jest automatycznie wywoływana w celu odrenderowania `ParentComponent`elementu, podobnie jak zdarzenia podrzędne wyzwala ponowne renderowanie składnika w obsłudze zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d629e-305">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="d629e-306">`EventCallback`i `EventCallback<T>` Zezwalaj na asynchroniczne Delegaty.</span><span class="sxs-lookup"><span data-stu-id="d629e-306">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="d629e-307">`EventCallback<T>`jest silnie wpisana i wymaga określonego typu argumentu.</span><span class="sxs-lookup"><span data-stu-id="d629e-307">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="d629e-308">`EventCallback`jest słabo wpisywany i zezwala na dowolny typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="d629e-308">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="d629e-309">`EventCallback<T>` Wywołaj `EventCallback` lub z`InvokeAsync` i await <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="d629e-309">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="d629e-310">Używaj `EventCallback` i`EventCallback<T>` dla parametrów składnika Obsługa zdarzeń i powiązania.</span><span class="sxs-lookup"><span data-stu-id="d629e-310">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="d629e-311">Preferuj silnie wpisaną `EventCallback<T>` `EventCallback`wartość.</span><span class="sxs-lookup"><span data-stu-id="d629e-311">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="d629e-312">`EventCallback<T>`zapewnia lepszą opinię o błędach dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-312">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="d629e-313">Podobnie jak w przypadku innych programów obsługi zdarzeń interfejsu użytkownika, określenie parametru zdarzenia jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="d629e-313">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="d629e-314">Użyj `EventCallback` w przypadku braku wartości przekazywania do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="d629e-314">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="chained-bind"></a><span data-ttu-id="d629e-315">Powiązanie łańcuchowe</span><span class="sxs-lookup"><span data-stu-id="d629e-315">Chained bind</span></span>

<span data-ttu-id="d629e-316">Typowy scenariusz polega na łańcuchu parametru powiązanego z danymi do elementu strony w danych wyjściowych składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-316">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="d629e-317">Ten scenariusz jest nazywany *powiązaniem łańcuchowym* , ponieważ wiele poziomów powiązań występuje jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="d629e-317">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="d629e-318">Nie można zaimplementować powiązania łańcuchowego przy użyciu `@bind` składni w elemencie strony.</span><span class="sxs-lookup"><span data-stu-id="d629e-318">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="d629e-319">Program obsługi zdarzeń i wartość muszą być określone osobno.</span><span class="sxs-lookup"><span data-stu-id="d629e-319">The event handler and value must be specified separately.</span></span> <span data-ttu-id="d629e-320">Składnik nadrzędny, jednak może używać `@bind` składni z parametrem składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-320">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="d629e-321">Następujący `PasswordField` składnik (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="d629e-321">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="d629e-322">Ustawia wartość `Password` elementu na właściwość. `<input>`</span><span class="sxs-lookup"><span data-stu-id="d629e-322">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="d629e-323">Uwidacznia zmiany `Password` właściwości w składniku nadrzędnym z [EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="d629e-323">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

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
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="d629e-324">`PasswordField` Składnik jest używany w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="d629e-324">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="d629e-325">Aby przeprowadzić sprawdzenia lub błędy pułapki dla hasła w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d629e-325">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="d629e-326">Utwórz pole zapasowe dla `Password` (`password` w poniższym przykładowym kodzie).</span><span class="sxs-lookup"><span data-stu-id="d629e-326">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="d629e-327">Wykonaj testy lub błędy pułapek w `Password` metodzie ustawiającej.</span><span class="sxs-lookup"><span data-stu-id="d629e-327">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="d629e-328">Poniższy przykład przedstawia natychmiastową opinię dla użytkownika, jeśli w wartości hasła jest używana spacja:</span><span class="sxs-lookup"><span data-stu-id="d629e-328">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
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
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="d629e-329">Przechwyć odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="d629e-329">Capture references to components</span></span>

<span data-ttu-id="d629e-330">Odwołania do składników zapewniają sposób odwoływania się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia, takie `Show` jak `Reset`lub.</span><span class="sxs-lookup"><span data-stu-id="d629e-330">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="d629e-331">Aby przechwycić odwołanie do składnika:</span><span class="sxs-lookup"><span data-stu-id="d629e-331">To capture a component reference:</span></span>

* <span data-ttu-id="d629e-332">[@ref](xref:mvc/views/razor#ref) Dodaj atrybut do składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d629e-332">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="d629e-333">Zdefiniuj pole z tym samym typem co składnik podrzędny.</span><span class="sxs-lookup"><span data-stu-id="d629e-333">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="d629e-334">Gdy składnik jest renderowany, `loginDialog` pole zostanie wypełnione `MyLoginDialog` wystąpieniem składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d629e-334">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="d629e-335">Następnie można wywołać metody .NET w wystąpieniu składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-335">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d629e-336">Zmienna jest wypełniana tylko po wyrenderowaniu składnika, a jego wyjście `MyLoginDialog` zawiera element. `loginDialog`</span><span class="sxs-lookup"><span data-stu-id="d629e-336">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="d629e-337">Do tego momentu nie ma niczego do odwołania.</span><span class="sxs-lookup"><span data-stu-id="d629e-337">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="d629e-338">Aby manipulować odwołaniami do składników po zakończeniu renderowania składnika, użyj `OnAfterRenderAsync` metod lub. `OnAfterRender`</span><span class="sxs-lookup"><span data-stu-id="d629e-338">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="d629e-339">Podczas przechwytywania odwołań do składników użycie podobnej składni do [przechwytywania odwołań do elementów](xref:blazor/javascript-interop#capture-references-to-elements)nie jest funkcją międzyoperacyjności [języka JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="d629e-339">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="d629e-340">Odwołania do składników nie są przenoszone&mdash;do kodu JavaScript, są używane tylko w kodzie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d629e-340">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="d629e-341">**Nie** należy używać odwołań do składników do mutacji stanu składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="d629e-341">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="d629e-342">Zamiast tego należy używać zwykłych parametrów deklaratywnych do przekazywania danych do składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="d629e-342">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="d629e-343">Użycie normalnych parametrów deklaratywnych powoduje, że składniki podrzędne, które automatycznie uruchamiają się w prawidłowym czasie.</span><span class="sxs-lookup"><span data-stu-id="d629e-343">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="d629e-344">Wywołaj metody składnika zewnętrznie, aby zaktualizować stan</span><span class="sxs-lookup"><span data-stu-id="d629e-344">Invoke component methods externally to update state</span></span>

<span data-ttu-id="d629e-345">Blazor używa w `SynchronizationContext` celu wymuszenia pojedynczego wątku logicznego wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d629e-345">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="d629e-346">W tym `SynchronizationContext`przypadku są wykonywane metody cyklu życia składnika i wszystkie wywołania zwrotne zdarzeń wywoływane przez Blazor.</span><span class="sxs-lookup"><span data-stu-id="d629e-346">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="d629e-347">W przypadku zdarzenia składnik należy zaktualizować w oparciu o zdarzenie zewnętrzne, takie jak czasomierz lub inne powiadomienia, przy użyciu `InvokeAsync` metody, która będzie wysyłana do `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="d629e-347">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="d629e-348">Rozważmy na przykład *usługę powiadamiania* , która może powiadomić dowolny składnik nasłuchujący zaktualizowanego stanu:</span><span class="sxs-lookup"><span data-stu-id="d629e-348">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="d629e-349">Użycie programu w celu zaktualizowania składnika: `NotifierService`</span><span class="sxs-lookup"><span data-stu-id="d629e-349">Usage of the `NotifierService` to update a component:</span></span>

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="d629e-350">W poprzednim przykładzie `NotifierService` wywołuje `OnNotify` metodę składnika poza Blazor. `SynchronizationContext`</span><span class="sxs-lookup"><span data-stu-id="d629e-350">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="d629e-351">`InvokeAsync`służy do przełączania do poprawnego kontekstu i kolejki renderowania.</span><span class="sxs-lookup"><span data-stu-id="d629e-351">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="d629e-352">Użyj \@klawisza, aby kontrolować zachowywanie elementów i składników</span><span class="sxs-lookup"><span data-stu-id="d629e-352">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="d629e-353">Podczas renderowania listy elementów lub składników oraz elementów lub składników, które następnie zmieniają się, algorytm diff Blazor musi zdecydować, które z poprzednich elementów lub składników mogą być zachowywane i jak obiekty modelu powinny być mapowane na nie.</span><span class="sxs-lookup"><span data-stu-id="d629e-353">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="d629e-354">Zwykle ten proces jest automatyczny i można go zignorować, ale istnieją przypadki, w których może być konieczne sterowanie procesem.</span><span class="sxs-lookup"><span data-stu-id="d629e-354">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="d629e-355">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="d629e-355">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="d629e-356">Zawartość `People` kolekcji może ulec zmianie z wstawionymi, usuniętymi lub z ponownymi zamówieniami.</span><span class="sxs-lookup"><span data-stu-id="d629e-356">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="d629e-357">Gdy składnik jest przerenderowany, `<DetailsEditor>` składnik może ulec zmianie, aby otrzymywać różne `Details` wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="d629e-357">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="d629e-358">Może to spowodować bardziej złożone odwzorowanie niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="d629e-358">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="d629e-359">W niektórych przypadkach odzyskanie może prowadzić do zauważalnych różnic w zachowaniu, takich jak brak fokusu elementu.</span><span class="sxs-lookup"><span data-stu-id="d629e-359">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="d629e-360">Proces mapowania można kontrolować przy użyciu `@key` atrybutu dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="d629e-360">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="d629e-361">`@key`powoduje, że algorytm różnicowego gwarantuje zachowywanie elementów lub składników na podstawie wartości klucza:</span><span class="sxs-lookup"><span data-stu-id="d629e-361">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="d629e-362">`<DetailsEditor>` `person` Po zmianie `People` kolekcji algorytm różnicowy zachowuje skojarzenie między wystąpieniami i wystąpieniami:</span><span class="sxs-lookup"><span data-stu-id="d629e-362">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="d629e-363">Jeśli element `Person` zostanie usunięty `People` z listy, tylko odpowiednie `<DetailsEditor>` wystąpienie zostanie usunięte z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-363">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="d629e-364">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="d629e-364">Other instances are left unchanged.</span></span>
* <span data-ttu-id="d629e-365">Jeśli na liście `<DetailsEditor>` zostaniewstawionywpewnymmiejscu,jednonowewystąpieniezostaniewstawionew`Person` odpowiednim położeniu.</span><span class="sxs-lookup"><span data-stu-id="d629e-365">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="d629e-366">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="d629e-366">Other instances are left unchanged.</span></span>
* <span data-ttu-id="d629e-367">W `Person` przypadku ponownego uporządkowania wpisów odpowiednie `<DetailsEditor>` wystąpienia są zachowywane i uporządkowane w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-367">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="d629e-368">W niektórych scenariuszach użycie programu `@key` minimalizuje złożoność operacji renderowania i pozwala uniknąć potencjalnych problemów związanych ze stanem częściowych elementów modelu dom, takich jak pozycja fokusu.</span><span class="sxs-lookup"><span data-stu-id="d629e-368">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d629e-369">Klucze są lokalne dla każdego elementu kontenera lub składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-369">Keys are local to each container element or component.</span></span> <span data-ttu-id="d629e-370">Klucze nie są porównywane globalnie w całym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="d629e-370">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="d629e-371">Kiedy używać \@klucza</span><span class="sxs-lookup"><span data-stu-id="d629e-371">When to use \@key</span></span>

<span data-ttu-id="d629e-372">Zazwyczaj warto używać `@key` zawsze, gdy lista jest renderowana (na przykład `@foreach` w bloku) i odpowiednia `@key`wartość istnieje do zdefiniowania.</span><span class="sxs-lookup"><span data-stu-id="d629e-372">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="d629e-373">Można również użyć `@key` , aby uniemożliwić Blazor z zachowaniem poddrzewa elementu lub składnika, gdy zmieniany jest obiekt:</span><span class="sxs-lookup"><span data-stu-id="d629e-373">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="d629e-374">Jeśli `@currentPerson` zmiany `<div>` , dyrektywa Blazor wymusza odrzucanie całości i jego obiektów podrzędnych oraz ponowne skompilowanie poddrzewa w interfejsie użytkownika za pomocą nowych elementów i składników. `@key`</span><span class="sxs-lookup"><span data-stu-id="d629e-374">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="d629e-375">Może to być przydatne, jeśli zachodzi konieczność zagwarantowania, że stan `@currentPerson` interfejsu użytkownika nie jest zachowywany w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="d629e-375">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="d629e-376">Kiedy nie używać \@klucza</span><span class="sxs-lookup"><span data-stu-id="d629e-376">When not to use \@key</span></span>

<span data-ttu-id="d629e-377">W przypadku różnicowania w programie `@key`występuje koszt wydajności.</span><span class="sxs-lookup"><span data-stu-id="d629e-377">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="d629e-378">Koszt wydajności nie jest duży, ale określa `@key` tylko, czy kontrolowanie reguł utrwalania elementu lub składnika przynosi korzyści dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-378">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="d629e-379">Nawet jeśli `@key` nie jest używany, Blazor zachowuje elementy podrzędne i wystąpienia składników tak dużo, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="d629e-379">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="d629e-380">Jedyną zaletą korzystania z `@key` programu jest kontrola nad sposobem, w *jaki* wystąpienia modelu są mapowane na zachowane wystąpienia składników, zamiast algorytmu różnicowego, wybierając mapowanie.</span><span class="sxs-lookup"><span data-stu-id="d629e-380">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="d629e-381">Wartości, które mają być \@używane dla klucza</span><span class="sxs-lookup"><span data-stu-id="d629e-381">What values to use for \@key</span></span>

<span data-ttu-id="d629e-382">Ogólnie rzecz biorąc, warto podać jeden z następujących rodzajów wartości dla `@key`:</span><span class="sxs-lookup"><span data-stu-id="d629e-382">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="d629e-383">Wystąpienia obiektów modelu (na przykład `Person` wystąpienie takie jak w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="d629e-383">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="d629e-384">Zapewnia to zachowywanie na podstawie równości odwołań do obiektów.</span><span class="sxs-lookup"><span data-stu-id="d629e-384">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="d629e-385">Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`lub `Guid`).</span><span class="sxs-lookup"><span data-stu-id="d629e-385">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="d629e-386">Upewnij się, że wartości `@key` używane do nie kolidują.</span><span class="sxs-lookup"><span data-stu-id="d629e-386">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="d629e-387">Jeśli w tym samym elemencie nadrzędnym zostaną wykryte wartości powodujące konflikt, Blazor zgłasza wyjątek, ponieważ nie może on w sposób jednoznaczny mapować starych elementów lub składników na nowe elementy lub składniki.</span><span class="sxs-lookup"><span data-stu-id="d629e-387">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="d629e-388">Używaj tylko odrębnych wartości, takich jak wystąpienia obiektów lub wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="d629e-388">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="d629e-389">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="d629e-389">Lifecycle methods</span></span>

<span data-ttu-id="d629e-390">`OnInitializedAsync`i `OnInitialized` wykonaj kod w celu zainicjowania składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-390">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="d629e-391">Aby wykonać operację asynchroniczną, użyj `OnInitializedAsync` `await` słowa kluczowego i dla operacji:</span><span class="sxs-lookup"><span data-stu-id="d629e-391">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="d629e-392">W przypadku operacji synchronicznych należy `OnInitialized`użyć:</span><span class="sxs-lookup"><span data-stu-id="d629e-392">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="d629e-393">`OnParametersSetAsync`i `OnParametersSet` są wywoływane, gdy składnik otrzymał parametry od jego elementu nadrzędnego i wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="d629e-393">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="d629e-394">Te metody są wykonywane po zainicjowaniu składnika i za każdym razem, gdy składnik jest renderowany:</span><span class="sxs-lookup"><span data-stu-id="d629e-394">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="d629e-395">`OnAfterRenderAsync`i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-395">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="d629e-396">Odwołania do elementów i składników są wypełniane w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="d629e-396">Element and component references are populated at this point.</span></span> <span data-ttu-id="d629e-397">Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.</span><span class="sxs-lookup"><span data-stu-id="d629e-397">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="d629e-398">`OnAfterRender`*nie jest wywoływana podczas renderowania na serwerze.*</span><span class="sxs-lookup"><span data-stu-id="d629e-398">`OnAfterRender` *is not called when prerendering on the server.*</span></span>

<span data-ttu-id="d629e-399">Parametr dla `OnAfterRenderAsync` i`OnAfterRender` jest: `firstRender`</span><span class="sxs-lookup"><span data-stu-id="d629e-399">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="d629e-400">`true` Ustawiana przy pierwszym wywołaniu wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-400">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="d629e-401">Zapewnia, że operacja inicjowania jest wykonywana tylko raz.</span><span class="sxs-lookup"><span data-stu-id="d629e-401">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="d629e-402">Obsługuj niekompletne akcje asynchroniczne podczas renderowania</span><span class="sxs-lookup"><span data-stu-id="d629e-402">Handle incomplete async actions at render</span></span>

<span data-ttu-id="d629e-403">Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogą nie zostać zakończone przed renderowaniem składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-403">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="d629e-404">Obiekty mogą być `null` lub uzupełniane wypełniane danymi podczas wykonywania metody cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="d629e-404">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="d629e-405">Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane.</span><span class="sxs-lookup"><span data-stu-id="d629e-405">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="d629e-406">Renderuj zastępcze elementy interfejsu użytkownika (na przykład komunikat ładowania), gdy obiekty `null`są.</span><span class="sxs-lookup"><span data-stu-id="d629e-406">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="d629e-407">W składniku `OnInitializedAsync` szablonów Blazor został zastąpiony do asynchronicznie odbierania danych prognozy (`forecasts`). `FetchData`</span><span class="sxs-lookup"><span data-stu-id="d629e-407">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="d629e-408">Gdy `forecasts` tak `null`jest, zostanie wyświetlony komunikat ładowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-408">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="d629e-409">Po zakończeniu `OnInitializedAsync` zwracany przez program składnik zostanie przerenderowany ze zaktualizowanym stanem. `Task`</span><span class="sxs-lookup"><span data-stu-id="d629e-409">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="d629e-410">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d629e-410">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="d629e-411">Wykonaj kod przed ustawieniem parametrów</span><span class="sxs-lookup"><span data-stu-id="d629e-411">Execute code before parameters are set</span></span>

<span data-ttu-id="d629e-412">`SetParameters`można zastąpić, aby wykonać kod przed ustawieniem parametrów:</span><span class="sxs-lookup"><span data-stu-id="d629e-412">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="d629e-413">Jeśli `base.SetParameters` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="d629e-413">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="d629e-414">Na przykład parametry przychodzące nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="d629e-414">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="d629e-415">Pomiń odświeżanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="d629e-415">Suppress refreshing of the UI</span></span>

<span data-ttu-id="d629e-416">`ShouldRender`można zastąpić, aby pominąć odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-416">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="d629e-417">Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony.</span><span class="sxs-lookup"><span data-stu-id="d629e-417">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="d629e-418">Nawet jeśli `ShouldRender` jest zastępowany, składnik jest zawsze początkowo renderowany.</span><span class="sxs-lookup"><span data-stu-id="d629e-418">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="d629e-419">Usuwanie składnika z interfejsem IDisposable</span><span class="sxs-lookup"><span data-stu-id="d629e-419">Component disposal with IDisposable</span></span>

<span data-ttu-id="d629e-420">W przypadku zaimplementowania <xref:System.IDisposable>składnika [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-420">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="d629e-421">Poniższy składnik używa `@implements IDisposable` `Dispose` i metody:</span><span class="sxs-lookup"><span data-stu-id="d629e-421">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="d629e-422">Routing</span><span class="sxs-lookup"><span data-stu-id="d629e-422">Routing</span></span>

<span data-ttu-id="d629e-423">Routing w Blazor jest realizowany przez dostarczenie szablonu trasy do każdego dostępnego składnika w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-423">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="d629e-424">Gdy plik Razor z `@page` dyrektywą jest kompilowany, wygenerowana Klasa ma <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określony szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="d629e-424">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="d629e-425">W czasie wykonywania router szuka klas składników za pomocą `RouteAttribute` i renderuje, w zależności od tego, który składnik ma szablon trasy zgodny z żądanym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="d629e-425">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="d629e-426">Do składnika można zastosować wiele szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="d629e-426">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="d629e-427">Poniższy składnik odpowiada na żądania dla `/BlazorRoute` i: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="d629e-427">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="d629e-428">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="d629e-428">Route parameters</span></span>

<span data-ttu-id="d629e-429">Składniki mogą odbierać parametry trasy z szablonu trasy dostarczonego w `@page` dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="d629e-429">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="d629e-430">Router używa parametrów trasy, aby wypełnić odpowiednie parametry składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-430">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="d629e-431">*Składnik parametru trasy*:</span><span class="sxs-lookup"><span data-stu-id="d629e-431">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="d629e-432">Parametry opcjonalne nie są obsługiwane, więc `@page` dwie dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="d629e-432">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="d629e-433">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="d629e-433">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="d629e-434">Druga `@page` dyrektywa `Text` przyjmuje parametr Route i przypisuje wartość do właściwości. `{text}`</span><span class="sxs-lookup"><span data-stu-id="d629e-434">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="d629e-435">Dziedziczenie klasy podstawowej dla środowiska "powiązanego z kodem"</span><span class="sxs-lookup"><span data-stu-id="d629e-435">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="d629e-436">Pliki składników mieszają znaczniki HTML C# i przetwarzają kod w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="d629e-436">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="d629e-437">`@inherits` Dyrektywa może służyć do udostępniania aplikacji Blazor z interfejsem "związanym z kodem", który oddziela oznakowanie składników od przetwarzania kodu.</span><span class="sxs-lookup"><span data-stu-id="d629e-437">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="d629e-438">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasę bazową `BlazorRocksBase`,, w celu zapewnienia właściwości i metod składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-438">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="d629e-439">*Strony/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d629e-439">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="d629e-440">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="d629e-440">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="d629e-441">Klasa bazowa powinna pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="d629e-441">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="d629e-442">Importuj składniki</span><span class="sxs-lookup"><span data-stu-id="d629e-442">Import components</span></span>

<span data-ttu-id="d629e-443">Przestrzeń nazw składnika utworzone przy użyciu Razor jest oparta na:</span><span class="sxs-lookup"><span data-stu-id="d629e-443">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="d629e-444">Projekt `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="d629e-444">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="d629e-445">Ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-445">The path from the project root to the component.</span></span> <span data-ttu-id="d629e-446">Na przykład, `ComponentsSample/Pages/Index.razor` znajduje się w przestrzeni `ComponentsSample.Pages`nazw.</span><span class="sxs-lookup"><span data-stu-id="d629e-446">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="d629e-447">Składniki przestrzegają C# reguł powiązań nazw.</span><span class="sxs-lookup"><span data-stu-id="d629e-447">Components follow C# name binding rules.</span></span> <span data-ttu-id="d629e-448">W przypadku elementu *index. Razor*wszystkie składniki znajdujące się w tym samym folderze, *stronach*i folderze nadrzędnym ( *ComponentsSample*) znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="d629e-448">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="d629e-449">Składniki zdefiniowane w innej przestrzeni nazw można wprowadzić do zakresu przy użyciu dyrektywy [ \@using używającej](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="d629e-449">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="d629e-450">Jeśli inny składnik, `NavMenu.razor`,,, istnieje w `ComponentsSample/Shared/`folderze, składnik może być używany w `Index.razor` programie z następującą `@using` instrukcją:</span><span class="sxs-lookup"><span data-stu-id="d629e-450">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="d629e-451">Do składników można także odwoływać się za pomocą ich w pełni kwalifikowanych nazw, co [ \@](xref:mvc/views/razor#using) eliminuje konieczność stosowania dyrektywy using:</span><span class="sxs-lookup"><span data-stu-id="d629e-451">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="d629e-452">`global::` Kwalifikacja nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="d629e-452">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="d629e-453">Importowanie składników przy użyciu `using` instrukcji z aliasami ( `@using Foo = Bar`np.) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d629e-453">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="d629e-454">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d629e-454">Partially qualified names aren't supported.</span></span> <span data-ttu-id="d629e-455">Na przykład dodawanie `@using ComponentsSample` i odwoływanie `NavMenu.razor` się `<Shared.NavMenu></Shared.NavMenu>` za pomocą nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d629e-455">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="d629e-456">Warunkowe atrybuty elementu HTML</span><span class="sxs-lookup"><span data-stu-id="d629e-456">Conditional HTML element attributes</span></span>

<span data-ttu-id="d629e-457">Atrybuty elementu HTML są warunkowo renderowane na podstawie wartości .NET.</span><span class="sxs-lookup"><span data-stu-id="d629e-457">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="d629e-458">Jeśli wartość jest `false` lub `null`, atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="d629e-458">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="d629e-459">Jeśli wartość to `true`, atrybut jest renderowany jako zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="d629e-459">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="d629e-460">W poniższym przykładzie, określa `IsCompleted` , czy `checked` jest renderowany w znacznikach elementu:</span><span class="sxs-lookup"><span data-stu-id="d629e-460">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="d629e-461">Jeśli `IsCompleted` jest`true`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="d629e-461">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="d629e-462">Jeśli `IsCompleted` jest`false`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="d629e-462">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="d629e-463">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="d629e-463">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="d629e-464">Niektóre atrybuty HTML, takie jak [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), nie działają prawidłowo, gdy typem .NET jest `bool`.</span><span class="sxs-lookup"><span data-stu-id="d629e-464">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="d629e-465">W takich przypadkach należy użyć `string` typu zamiast. `bool`</span><span class="sxs-lookup"><span data-stu-id="d629e-465">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="d629e-466">Nieprzetworzony kod HTML</span><span class="sxs-lookup"><span data-stu-id="d629e-466">Raw HTML</span></span>

<span data-ttu-id="d629e-467">Ciągi są zwykle renderowane przy użyciu węzłów tekstowych DOM, co oznacza, że wszystkie znaczniki, które mogą zawierać, są ignorowane i traktowane jako tekst literału.</span><span class="sxs-lookup"><span data-stu-id="d629e-467">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="d629e-468">Aby renderować nieprzetworzony kod HTML, zawiń zawartość HTML w `MarkupString` wartości.</span><span class="sxs-lookup"><span data-stu-id="d629e-468">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="d629e-469">Wartość jest analizowana jako plik HTML lub SVG i wstawiona do modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="d629e-469">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="d629e-470">Renderowanie nieprzetworzonego kodu HTML zbudowanego z dowolnego niezaufanego źródła stanowi **zagrożenie bezpieczeństwa** i należy je unikać!</span><span class="sxs-lookup"><span data-stu-id="d629e-470">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="d629e-471">W poniższym przykładzie pokazano, `MarkupString` jak za pomocą typu dodać blok statycznej zawartości HTML do renderowanego danych wyjściowych składnika:</span><span class="sxs-lookup"><span data-stu-id="d629e-471">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="d629e-472">Składniki z szablonami</span><span class="sxs-lookup"><span data-stu-id="d629e-472">Templated components</span></span>

<span data-ttu-id="d629e-473">Składniki z szablonami są składnikami, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które mogą być następnie używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-473">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="d629e-474">Składniki z szablonami umożliwiają tworzenie składników wyższego poziomu, które są większe niż zwykłe składniki.</span><span class="sxs-lookup"><span data-stu-id="d629e-474">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="d629e-475">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="d629e-475">A couple of examples include:</span></span>

* <span data-ttu-id="d629e-476">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka, wierszy i stopki tabeli.</span><span class="sxs-lookup"><span data-stu-id="d629e-476">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="d629e-477">Składnik listy, który umożliwia użytkownikowi określenie szablonu do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="d629e-477">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="d629e-478">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="d629e-478">Template parameters</span></span>

<span data-ttu-id="d629e-479">Składnik szablonu jest definiowany przez określenie co najmniej jednego parametru składnika typu `RenderFragment` lub. `RenderFragment<T>`</span><span class="sxs-lookup"><span data-stu-id="d629e-479">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="d629e-480">Fragment renderowania reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="d629e-480">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="d629e-481">`RenderFragment<T>`przyjmuje parametr typu, który można określić podczas wywoływania fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="d629e-481">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="d629e-482">`TableTemplate`składnika</span><span class="sxs-lookup"><span data-stu-id="d629e-482">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="d629e-483">W przypadku korzystania z składnika z szablonem parametry szablonu można określić za pomocą elementów podrzędnych, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="d629e-483">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="pets">
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

### <a name="template-context-parameters"></a><span data-ttu-id="d629e-484">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="d629e-484">Template context parameters</span></span>

<span data-ttu-id="d629e-485">Argumenty składnika `RenderFragment<T>` typu przekazane jako elementy mają niejawny parametr o `context` nazwie (na przykład z poprzedniego przykładu `@context.PetId`kodu), ale można zmienić nazwę parametru przy użyciu `Context` atrybutu w elemencie podrzędnym postaci.</span><span class="sxs-lookup"><span data-stu-id="d629e-485">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="d629e-486">W poniższym przykładzie `RowTemplate` `Context` atrybut elementu określa `pet` parametr:</span><span class="sxs-lookup"><span data-stu-id="d629e-486">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="pets">
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

<span data-ttu-id="d629e-487">Alternatywnie można określić `Context` atrybut dla elementu składnika.</span><span class="sxs-lookup"><span data-stu-id="d629e-487">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="d629e-488">Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="d629e-488">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="d629e-489">Może to być przydatne, jeśli chcesz określić nazwę parametru zawartości dla niejawnej zawartości podrzędnej (bez żadnego elementu podrzędnego otoki).</span><span class="sxs-lookup"><span data-stu-id="d629e-489">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="d629e-490">W poniższym przykładzie `Context` atrybut pojawia się `TableTemplate` na elemencie i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="d629e-490">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="pets" Context="pet">
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

### <a name="generic-typed-components"></a><span data-ttu-id="d629e-491">Składniki typu rodzajowego</span><span class="sxs-lookup"><span data-stu-id="d629e-491">Generic-typed components</span></span>

<span data-ttu-id="d629e-492">Składniki z szablonami są często wpisywane ogólnie.</span><span class="sxs-lookup"><span data-stu-id="d629e-492">Templated components are often generically typed.</span></span> <span data-ttu-id="d629e-493">Na przykład, składnik ogólny `ListViewTemplate` może służyć do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="d629e-493">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="d629e-494">Aby zdefiniować składnik ogólny, użyj [@typeparam](xref:mvc/views/razor#typeparam) dyrektywy do określenia parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="d629e-494">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="d629e-495">W przypadku używania składników o typie ogólnym parametr typu jest wnioskowany, jeśli jest to możliwe:</span><span class="sxs-lookup"><span data-stu-id="d629e-495">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="d629e-496">W przeciwnym razie parametr typu musi być jawnie określony przy użyciu atrybutu, który jest zgodny z nazwą parametru typu.</span><span class="sxs-lookup"><span data-stu-id="d629e-496">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="d629e-497">W poniższym przykładzie `TItem="Pet"` określa typ:</span><span class="sxs-lookup"><span data-stu-id="d629e-497">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="d629e-498">Wartości kaskadowe i parametry</span><span class="sxs-lookup"><span data-stu-id="d629e-498">Cascading values and parameters</span></span>

<span data-ttu-id="d629e-499">W niektórych scenariuszach nie można przepływać danych z składnika nadrzędnego do składnika potomnego przy użyciu [parametrów składnika](#component-parameters), zwłaszcza gdy istnieje kilka warstw składników.</span><span class="sxs-lookup"><span data-stu-id="d629e-499">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="d629e-500">Wartości kaskadowe i parametry rozwiązują ten problem, zapewniając wygodną metodę dla składnika nadrzędnego, aby zapewnić wartość wszystkim jej składnikom potomnym.</span><span class="sxs-lookup"><span data-stu-id="d629e-500">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="d629e-501">Kaskadowe wartości i parametry również zapewniają podejście do współrzędnych składników.</span><span class="sxs-lookup"><span data-stu-id="d629e-501">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="d629e-502">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="d629e-502">Theme example</span></span>

<span data-ttu-id="d629e-503">W poniższym przykładzie z przykładowej aplikacji `ThemeInfo` Klasa określa informacje o motywie, aby przetworzyć hierarchię składników w taki sposób, aby wszystkie przyciski w danej części aplikacji miały ten sam styl.</span><span class="sxs-lookup"><span data-stu-id="d629e-503">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="d629e-504">*UIThemeClasses/themeinfo wskazuje. cs*:</span><span class="sxs-lookup"><span data-stu-id="d629e-504">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="d629e-505">Składnik nadrzędny może zapewnić kaskadową wartość przy użyciu składnika wartości kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="d629e-505">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="d629e-506">`CascadingValue` Składnik otacza poddrzewo hierarchii składników i dostarcza jedną wartość do wszystkich składników w tym poddrzewie.</span><span class="sxs-lookup"><span data-stu-id="d629e-506">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="d629e-507">Przykładowo aplikacja Przykładowa określa informacje o motywie`ThemeInfo`() w jednej z układów aplikacji jako parametr kaskadowy dla wszystkich składników, które tworzą treść `@Body` układu właściwości.</span><span class="sxs-lookup"><span data-stu-id="d629e-507">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="d629e-508">`ButtonClass`ma przypisaną wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="d629e-508">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="d629e-509">Każdy składnik podrzędny może wykorzystać tę właściwość za pomocą `ThemeInfo` obiektu kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="d629e-509">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="d629e-510">`CascadingValuesParametersLayout`składnika</span><span class="sxs-lookup"><span data-stu-id="d629e-510">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
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

<span data-ttu-id="d629e-511">Aby korzystać z wartości kaskadowych, składniki deklarują kaskadowe parametry przy użyciu `[CascadingParameter]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d629e-511">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="d629e-512">Wartości kaskadowe są powiązane z parametrami kaskadowymi według typu.</span><span class="sxs-lookup"><span data-stu-id="d629e-512">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="d629e-513">W przykładowej aplikacji `CascadingValuesParametersTheme` składnik wiąże `ThemeInfo` wartość kaskadową z parametrem kaskadowym.</span><span class="sxs-lookup"><span data-stu-id="d629e-513">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="d629e-514">Parametr służy do ustawiania klasy CSS dla jednego z przycisków wyświetlanych przez składnik.</span><span class="sxs-lookup"><span data-stu-id="d629e-514">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="d629e-515">`CascadingValuesParametersTheme`składnika</span><span class="sxs-lookup"><span data-stu-id="d629e-515">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="d629e-516">Aby przetworzyć kaskadowo wiele wartości tego samego typu w ramach tego samego poddrzewa `Name` , podaj unikatowy `CascadingValue` ciąg dla każdego składnika `CascadingParameter`i odpowiadający mu.</span><span class="sxs-lookup"><span data-stu-id="d629e-516">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="d629e-517">W poniższym przykładzie dwa `CascadingValue` składniki kaskaduje różne wystąpienia o `MyCascadingType` nazwie:</span><span class="sxs-lookup"><span data-stu-id="d629e-517">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="d629e-518">W składniku potomnym, kaskadowe parametry odbierają swoje wartości z odpowiednich kaskadowych wartości w składniku nadrzędnym według nazwy:</span><span class="sxs-lookup"><span data-stu-id="d629e-518">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="d629e-519">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="d629e-519">TabSet example</span></span>

<span data-ttu-id="d629e-520">Parametry kaskadowe umożliwiają również współdziałanie składników w hierarchii składników.</span><span class="sxs-lookup"><span data-stu-id="d629e-520">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="d629e-521">Rozważmy na przykład następujący przykład *TabSet* w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="d629e-521">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="d629e-522">Przykładowa aplikacja ma `ITab` interfejs, który implementuje karty:</span><span class="sxs-lookup"><span data-stu-id="d629e-522">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="d629e-523">Składnik używa składnika, który zawiera kilka `Tab` składników: `TabSet` `CascadingValuesParametersTabSet`</span><span class="sxs-lookup"><span data-stu-id="d629e-523">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="d629e-524">Składniki podrzędne `Tab` nie są jawnie przenoszone jako parametry `TabSet`do.</span><span class="sxs-lookup"><span data-stu-id="d629e-524">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="d629e-525">Zamiast tego składniki podrzędne `Tab` są częścią zawartości `TabSet`elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d629e-525">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="d629e-526">Jednak nadal musi wiedzieć o każdym `Tab` składniku, aby można było renderować nagłówki i aktywną kartę. `TabSet` Aby umożliwić tę koordynację bez konieczności stosowania dodatkowego kodu `TabSet` , składnik *może stanowić wartość kaskadową* , która następnie jest wybierana przez składniki potomne `Tab` .</span><span class="sxs-lookup"><span data-stu-id="d629e-526">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="d629e-527">`TabSet`składnika</span><span class="sxs-lookup"><span data-stu-id="d629e-527">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="d629e-528">Składniki potomne `Tab` przechwytują zawierający `TabSet` jako parametr kaskadowy, więc `Tab` składniki dodają się do `TabSet` i koordynują, na której karcie jest aktywna.</span><span class="sxs-lookup"><span data-stu-id="d629e-528">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="d629e-529">`Tab`składnika</span><span class="sxs-lookup"><span data-stu-id="d629e-529">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="d629e-530">Szablony Razor</span><span class="sxs-lookup"><span data-stu-id="d629e-530">Razor templates</span></span>

<span data-ttu-id="d629e-531">Fragmenty renderowania można definiować przy użyciu składni szablonu Razor.</span><span class="sxs-lookup"><span data-stu-id="d629e-531">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="d629e-532">Szablony Razor są sposobem definiowania fragmentu interfejsu użytkownika i przyjmuje następujący format:</span><span class="sxs-lookup"><span data-stu-id="d629e-532">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="d629e-533">Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości oraz renderowania szablonów bezpośrednio w składniku.</span><span class="sxs-lookup"><span data-stu-id="d629e-533">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="d629e-534">Fragmenty renderowania mogą być również przekazane jako argumenty do [składników](#templated-components)z szablonem.</span><span class="sxs-lookup"><span data-stu-id="d629e-534">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="d629e-535">Renderowane dane wyjściowe poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="d629e-535">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="d629e-536">Ręczna logika RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="d629e-536">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="d629e-537">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`zapewnia metody manipulowania składnikami i elementami, w tym ręczne Kompilowanie C# składników w kodzie.</span><span class="sxs-lookup"><span data-stu-id="d629e-537">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="d629e-538">Korzystanie z `RenderTreeBuilder` programu do tworzenia składników jest zaawansowanym scenariuszem.</span><span class="sxs-lookup"><span data-stu-id="d629e-538">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="d629e-539">Nieprawidłowo sformułowany składnik (na przykład niezamknięty tag znacznika) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="d629e-539">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="d629e-540">Rozważmy następujący `PetDetails` składnik, który można ręcznie utworzyć w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="d629e-540">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="d629e-541">W poniższym przykładzie pętla w `CreateComponent` metodzie generuje trzy `PetDetails` składniki.</span><span class="sxs-lookup"><span data-stu-id="d629e-541">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="d629e-542">Podczas wywoływania `RenderTreeBuilder` metod tworzenia składników (`OpenComponent` i `AddAttribute`) numery sekwencji są numerami wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="d629e-542">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="d629e-543">Algorytm Blazor różnica polega na numerach sekwencji odpowiadających odrębnym wierszom kodu, a nie odrębnym wywoływaniu wywołań.</span><span class="sxs-lookup"><span data-stu-id="d629e-543">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="d629e-544">Podczas tworzenia składnika przy użyciu `RenderTreeBuilder` metod umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="d629e-544">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="d629e-545">**Użycie obliczenia lub licznika do wygenerowania numeru sekwencji może prowadzić do niskiej wydajności.**</span><span class="sxs-lookup"><span data-stu-id="d629e-545">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="d629e-546">Aby uzyskać więcej informacji, zobacz sekcję [numery sekwencji powiązane z numerami wierszy kodu i kolejnością niewykonania](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="d629e-546">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="d629e-547">`BuiltContent`składnika</span><span class="sxs-lookup"><span data-stu-id="d629e-547">`BuiltContent` component:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
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

> <span data-ttu-id="d629e-548">! WYŚWIETLANIA Typy w programie `Microsoft.AspNetCore.Components.RenderTree` umożliwiają przetwarzanie *wyników* operacji renderowania.</span><span class="sxs-lookup"><span data-stu-id="d629e-548">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="d629e-549">Są to wewnętrzne szczegóły implementacji platformy Blazor Framework.</span><span class="sxs-lookup"><span data-stu-id="d629e-549">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="d629e-550">Te typy powinny być uznawane za *niestabilne* i mogą ulec zmianie w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="d629e-550">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="d629e-551">Numery sekwencji odnoszą się do numerów wierszy kodu, a nie kolejności wykonywania</span><span class="sxs-lookup"><span data-stu-id="d629e-551">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="d629e-552">Pliki `.razor` Blazor są zawsze kompilowane.</span><span class="sxs-lookup"><span data-stu-id="d629e-552">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="d629e-553">Jest to znakomita korzyść `.razor` , ponieważ krok kompilowania może służyć do iniekcji informacji, które zwiększają wydajność aplikacji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d629e-553">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="d629e-554">Najważniejszym przykładem tych ulepszeń są *numery sekwencji*.</span><span class="sxs-lookup"><span data-stu-id="d629e-554">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="d629e-555">Numery sekwencji wskazują na środowisko uruchomieniowe, które pochodzą z różnych i uporządkowanych wierszy kodu.</span><span class="sxs-lookup"><span data-stu-id="d629e-555">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="d629e-556">Środowisko uruchomieniowe używa tych informacji do generowania wydajnych różnic drzewa w czasie liniowym, które są znacznie szybsze niż zwykle jest to możliwe dla algorytmu różnicowego drzewa ogólnego.</span><span class="sxs-lookup"><span data-stu-id="d629e-556">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="d629e-557">Rozważmy następujący prosty `.razor` plik:</span><span class="sxs-lookup"><span data-stu-id="d629e-557">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="d629e-558">Poprzedni kod kompiluje się w taki sposób, aby wyglądał następująco:</span><span class="sxs-lookup"><span data-stu-id="d629e-558">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="d629e-559">Gdy kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="d629e-559">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="d629e-560">Sequence</span><span class="sxs-lookup"><span data-stu-id="d629e-560">Sequence</span></span> | <span data-ttu-id="d629e-561">Typ</span><span class="sxs-lookup"><span data-stu-id="d629e-561">Type</span></span>      | <span data-ttu-id="d629e-562">Dane</span><span class="sxs-lookup"><span data-stu-id="d629e-562">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="d629e-563">0</span><span class="sxs-lookup"><span data-stu-id="d629e-563">0</span></span>        | <span data-ttu-id="d629e-564">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d629e-564">Text node</span></span> | <span data-ttu-id="d629e-565">pierwszego</span><span class="sxs-lookup"><span data-stu-id="d629e-565">First</span></span>  |
| <span data-ttu-id="d629e-566">1</span><span class="sxs-lookup"><span data-stu-id="d629e-566">1</span></span>        | <span data-ttu-id="d629e-567">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d629e-567">Text node</span></span> | <span data-ttu-id="d629e-568">Sekunda</span><span class="sxs-lookup"><span data-stu-id="d629e-568">Second</span></span> |

<span data-ttu-id="d629e-569">Wyobraź sobie `someFlag` , `false`że zostanie ona przerenderowana, a znaczniki są renderowane ponownie.</span><span class="sxs-lookup"><span data-stu-id="d629e-569">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="d629e-570">Tym razem Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="d629e-570">This time, the builder receives:</span></span>

| <span data-ttu-id="d629e-571">Sequence</span><span class="sxs-lookup"><span data-stu-id="d629e-571">Sequence</span></span> | <span data-ttu-id="d629e-572">Typ</span><span class="sxs-lookup"><span data-stu-id="d629e-572">Type</span></span>       | <span data-ttu-id="d629e-573">Dane</span><span class="sxs-lookup"><span data-stu-id="d629e-573">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="d629e-574">1</span><span class="sxs-lookup"><span data-stu-id="d629e-574">1</span></span>        | <span data-ttu-id="d629e-575">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d629e-575">Text node</span></span>  | <span data-ttu-id="d629e-576">Sekunda</span><span class="sxs-lookup"><span data-stu-id="d629e-576">Second</span></span> |

<span data-ttu-id="d629e-577">Gdy środowisko uruchomieniowe wykonuje porównanie, zobaczy, że element w sekwencji `0` został usunięty, więc generuje następujący skrypt uproszczonej *edycji*:</span><span class="sxs-lookup"><span data-stu-id="d629e-577">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="d629e-578">Usuń pierwszy węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="d629e-578">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="d629e-579">Co się stało z błędami w przypadku wygenerowania numerów sekwencyjnych</span><span class="sxs-lookup"><span data-stu-id="d629e-579">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="d629e-580">Wyobraź sobie, że została zapisana następująca logika konstruktora drzewa renderowania:</span><span class="sxs-lookup"><span data-stu-id="d629e-580">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="d629e-581">Teraz pierwsze dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="d629e-581">Now, the first output is:</span></span>

| <span data-ttu-id="d629e-582">Sequence</span><span class="sxs-lookup"><span data-stu-id="d629e-582">Sequence</span></span> | <span data-ttu-id="d629e-583">Typ</span><span class="sxs-lookup"><span data-stu-id="d629e-583">Type</span></span>      | <span data-ttu-id="d629e-584">Dane</span><span class="sxs-lookup"><span data-stu-id="d629e-584">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="d629e-585">0</span><span class="sxs-lookup"><span data-stu-id="d629e-585">0</span></span>        | <span data-ttu-id="d629e-586">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d629e-586">Text node</span></span> | <span data-ttu-id="d629e-587">pierwszego</span><span class="sxs-lookup"><span data-stu-id="d629e-587">First</span></span>  |
| <span data-ttu-id="d629e-588">1</span><span class="sxs-lookup"><span data-stu-id="d629e-588">1</span></span>        | <span data-ttu-id="d629e-589">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d629e-589">Text node</span></span> | <span data-ttu-id="d629e-590">Sekunda</span><span class="sxs-lookup"><span data-stu-id="d629e-590">Second</span></span> |

<span data-ttu-id="d629e-591">Ten wynik jest identyczny z poprzednim przypadkiem, dlatego nie istnieją żadne negatywne problemy.</span><span class="sxs-lookup"><span data-stu-id="d629e-591">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="d629e-592">`someFlag`znajduje `false` się na drugim renderingu, a dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="d629e-592">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="d629e-593">Sequence</span><span class="sxs-lookup"><span data-stu-id="d629e-593">Sequence</span></span> | <span data-ttu-id="d629e-594">Typ</span><span class="sxs-lookup"><span data-stu-id="d629e-594">Type</span></span>      | <span data-ttu-id="d629e-595">Dane</span><span class="sxs-lookup"><span data-stu-id="d629e-595">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="d629e-596">0</span><span class="sxs-lookup"><span data-stu-id="d629e-596">0</span></span>        | <span data-ttu-id="d629e-597">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d629e-597">Text node</span></span> | <span data-ttu-id="d629e-598">Sekunda</span><span class="sxs-lookup"><span data-stu-id="d629e-598">Second</span></span> |

<span data-ttu-id="d629e-599">Tym razem algorytm diff widzi, że pojawiły się *dwie* zmiany, a algorytm generuje następujący skrypt edycji:</span><span class="sxs-lookup"><span data-stu-id="d629e-599">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="d629e-600">Zmień wartość pierwszego węzła tekstowego na `Second`.</span><span class="sxs-lookup"><span data-stu-id="d629e-600">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="d629e-601">Usuń drugi węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="d629e-601">Remove the second text node.</span></span>

<span data-ttu-id="d629e-602">Generowanie numerów sekwencji utraciło wszystkie przydatne informacje o tym, gdzie znajdują się `if/else` gałęzie i pętle w oryginalnym kodzie.</span><span class="sxs-lookup"><span data-stu-id="d629e-602">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="d629e-603">Wynikiem tego jest różnica **dwa razy** , tak długo, jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="d629e-603">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="d629e-604">Jest to prosty przykład.</span><span class="sxs-lookup"><span data-stu-id="d629e-604">This is a trivial example.</span></span> <span data-ttu-id="d629e-605">W bardziej realistycznych przypadkach ze złożonymi i głęboko zagnieżdżonymi strukturami, szczególnie w przypadku pętli, koszt wydajności jest bardziej poważny.</span><span class="sxs-lookup"><span data-stu-id="d629e-605">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="d629e-606">Zamiast natychmiastowego identyfikowania, które bloki lub gałęzie pętli zostały wstawione lub usunięte, algorytm różnicowy musi reprezentować się w drzewach renderowania i zwykle tworzyć dużo dłużej edytowane skrypty, ponieważ nie są w nim poinformowani o sposobie starych i nowych struktur odnoszą się do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="d629e-606">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="d629e-607">Wskazówki i wnioski</span><span class="sxs-lookup"><span data-stu-id="d629e-607">Guidance and conclusions</span></span>

* <span data-ttu-id="d629e-608">Wydajność aplikacji ma wpływ na to, że numery sekwencji są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="d629e-608">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="d629e-609">Struktura nie może automatycznie tworzyć własnych numerów sekwencji w czasie wykonywania, ponieważ niezbędne informacje nie istnieją, chyba że są przechwytywane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-609">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="d629e-610">Nie zapisuj długich bloków logiki wykonywanej `RenderTreeBuilder` ręcznie.</span><span class="sxs-lookup"><span data-stu-id="d629e-610">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="d629e-611">Preferuj `.razor` pliki i Zezwalaj kompilatorowi na rozpatruje numery sekwencji.</span><span class="sxs-lookup"><span data-stu-id="d629e-611">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="d629e-612">Jeśli `RenderTreeBuilder` nie możesz uniknąć ręcznej logiki, Podziel długie bloki kodu na mniejsze fragmenty `OpenRegion` / `CloseRegion` opakowane.</span><span class="sxs-lookup"><span data-stu-id="d629e-612">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="d629e-613">Każdy region ma własne oddzielne miejsce numerów sekwencyjnych, więc można uruchomić ponownie od zera (lub dowolnego innego numeru) w każdym regionie.</span><span class="sxs-lookup"><span data-stu-id="d629e-613">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="d629e-614">Jeśli numery sekwencji są stałee, algorytm diff wymaga tylko zwiększenia wartości sekwencji.</span><span class="sxs-lookup"><span data-stu-id="d629e-614">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="d629e-615">Początkowa wartość i przerwy są nieistotne.</span><span class="sxs-lookup"><span data-stu-id="d629e-615">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="d629e-616">Jedną z wiarygodnych opcji jest użycie numeru wiersza kodu jako numeru sekwencyjnego lub rozpoczęcie od zera i zwiększenie według wartości lub setek (lub dowolnego preferowanego interwału).</span><span class="sxs-lookup"><span data-stu-id="d629e-616">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="d629e-617">Blazor używa numerów sekwencji, podczas gdy inne struktury interfejsu użytkownika porównujące drzewa nie są używane.</span><span class="sxs-lookup"><span data-stu-id="d629e-617">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="d629e-618">Różnica jest znacznie szybsza, gdy są używane numery sekwencji, a Blazor ma zalety kroku kompilacji, który zajmuje się automatycznie numerami sekwencyjnymi dla deweloperów tworzących `.razor` pliki.</span><span class="sxs-lookup"><span data-stu-id="d629e-618">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="d629e-619">Lokalizacja</span><span class="sxs-lookup"><span data-stu-id="d629e-619">Localization</span></span>

<span data-ttu-id="d629e-620">Aplikacje serwera Blazor są zlokalizowane przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="d629e-620">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="d629e-621">Oprogramowanie pośredniczące wybiera odpowiednią kulturę dla użytkowników żądających zasobów z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-621">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="d629e-622">Kulturę można ustawić przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="d629e-622">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="d629e-623">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="d629e-623">Cookies</span></span>](#cookies)
* [<span data-ttu-id="d629e-624">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="d629e-624">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="d629e-625">Aby uzyskać więcej informacji i przykładów, <xref:fundamentals/localization>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="d629e-625">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="d629e-626">Cookie</span><span class="sxs-lookup"><span data-stu-id="d629e-626">Cookies</span></span>

<span data-ttu-id="d629e-627">Plik cookie kultury lokalizacji może utrzymywać kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-627">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="d629e-628">Plik cookie jest tworzony przez `OnGet` metodę strony hosta aplikacji (*strony/host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="d629e-628">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="d629e-629">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie na kolejnych żądaniach, aby ustawić kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-629">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="d629e-630">Użycie pliku cookie zapewnia, że połączenie z użyciem protokołu WebSocket może prawidłowo propagować kulturę.</span><span class="sxs-lookup"><span data-stu-id="d629e-630">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="d629e-631">Jeśli schematy lokalizacji są oparte na ścieżce URL lub ciągu zapytania, schemat może nie być w stanie współdziałać z usługą WebSockets, więc nie będzie można zachować kultury.</span><span class="sxs-lookup"><span data-stu-id="d629e-631">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="d629e-632">W związku z tym zalecanym podejściem jest użycie pliku cookie kultury lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-632">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="d629e-633">Każda technika może służyć do przypisywania kultury, jeśli kultura jest utrwalona w pliku cookie lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-633">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="d629e-634">Jeśli aplikacja ma już ustalony schemat lokalizacji dla ASP.NET Core po stronie serwera, Kontynuuj korzystanie z istniejącej infrastruktury lokalizacji aplikacji i Ustaw plik cookie kultury lokalizacji w schemacie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-634">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="d629e-635">Poniższy przykład pokazuje, jak ustawić bieżącą kulturę w pliku cookie, który może zostać odczytany przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-635">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="d629e-636">Utwórz plik *Pages/hosta. cshtml. cs* z następującą zawartością w aplikacji Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="d629e-636">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="d629e-637">Lokalizacja jest obsługiwana w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d629e-637">Localization is handled in the app:</span></span>

1. <span data-ttu-id="d629e-638">Przeglądarka wysyła początkowe żądanie HTTP do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-638">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="d629e-639">Kultura jest przypisana przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="d629e-639">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="d629e-640">Metoda w *_Host. cshtml. cs* utrzymuje kulturę w pliku cookie jako część odpowiedzi. `OnGet`</span><span class="sxs-lookup"><span data-stu-id="d629e-640">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="d629e-641">Przeglądarka otwiera połączenie WebSocket, aby utworzyć interaktywną sesję serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="d629e-641">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="d629e-642">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie i przypisuje kulturę.</span><span class="sxs-lookup"><span data-stu-id="d629e-642">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="d629e-643">Sesja serwera Blazor jest rozpoczynana z poprawną kulturą.</span><span class="sxs-lookup"><span data-stu-id="d629e-643">The Blazor Server session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="d629e-644">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="d629e-644">Provide UI to choose the culture</span></span>

<span data-ttu-id="d629e-645">Aby zapewnić interfejs użytkownika, aby umożliwić użytkownikowi wybranie kultury, zalecane jest *podejście oparte na przekierowaniu* .</span><span class="sxs-lookup"><span data-stu-id="d629e-645">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="d629e-646">Ten proces jest podobny do tego, co się dzieje w aplikacji sieci Web, gdy użytkownik próbuje uzyskać&mdash;dostęp do bezpiecznego zasobu, użytkownik zostanie przekierowany do strony logowania, a następnie przekierowany z powrotem do oryginalnego zasobu.</span><span class="sxs-lookup"><span data-stu-id="d629e-646">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="d629e-647">Aplikacja utrzymuje wybraną kulturę użytkownika za pośrednictwem przekierowania do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d629e-647">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="d629e-648">Kontroler ustawia wybraną kulturę użytkownika na plik cookie i przekierowuje użytkownika z powrotem do oryginalnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="d629e-648">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="d629e-649">Ustanów punkt końcowy HTTP na serwerze, aby ustawić kulturę wybraną przez użytkownika w pliku cookie i wykonać przekierowanie z powrotem do oryginalnego identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="d629e-649">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="d629e-650">Użyj wyniku `LocalRedirect` działania, aby zapobiec atakom typu Open redirect.</span><span class="sxs-lookup"><span data-stu-id="d629e-650">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="d629e-651">Aby uzyskać więcej informacji, zobacz <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="d629e-651">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="d629e-652">Poniższy składnik przedstawia przykład sposobu wykonywania wstępnego przekierowania, gdy użytkownik wybierze kulturę:</span><span class="sxs-lookup"><span data-stu-id="d629e-652">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="d629e-653">Korzystanie z scenariuszy lokalizacji platformy .NET w aplikacjach Blazor</span><span class="sxs-lookup"><span data-stu-id="d629e-653">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="d629e-654">W aplikacjach Blazor dostępne są następujące scenariusze dotyczące lokalizacji i globalizacji platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="d629e-654">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="d629e-655">. System zasobów netto</span><span class="sxs-lookup"><span data-stu-id="d629e-655">.NET's resources system</span></span>
* <span data-ttu-id="d629e-656">Formatowanie liczb i dat specyficznych dla kultury</span><span class="sxs-lookup"><span data-stu-id="d629e-656">Culture-specific number and date formatting</span></span>

<span data-ttu-id="d629e-657">`@bind` Funkcja Blazor wykonuje globalizację w oparciu o bieżącą kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d629e-657">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="d629e-658">Aby uzyskać więcej informacji, zobacz sekcję [powiązanie danych](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="d629e-658">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="d629e-659">Obecnie obsługiwane są ograniczone zestawy ASP.NET Core scenariuszy lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="d629e-659">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="d629e-660">`IStringLocalizer<>`*jest obsługiwany* w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="d629e-660">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="d629e-661">`IHtmlLocalizer<>`Lokalizacje adnotacji danych są ASP.NET Core w scenariuszach MVC i nie są obsługiwane w aplikacjach Blazor. `IViewLocalizer<>`</span><span class="sxs-lookup"><span data-stu-id="d629e-661">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="d629e-662">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="d629e-662">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="d629e-663">Skalowalne obrazy wektorowe (SVG)</span><span class="sxs-lookup"><span data-stu-id="d629e-663">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="d629e-664">Ponieważ Blazor renderuje HTML, obrazy obsługiwane przez przeglądarkę, w tym obrazy*SVG (Scalable*Vector Graphics), są obsługiwane przez `<img>` Tag:</span><span class="sxs-lookup"><span data-stu-id="d629e-664">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="d629e-665">Podobnie Obrazy SVG są obsługiwane w regułach CSS pliku arkusza stylów (*CSS*):</span><span class="sxs-lookup"><span data-stu-id="d629e-665">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="d629e-666">Jednak wbudowane znaczniki SVG nie są obsługiwane we wszystkich scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="d629e-666">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="d629e-667">Jeśli umieścisz `<svg>` tag bezpośrednio w pliku składnika ( *. Razor*), podstawowe renderowanie obrazu jest obsługiwane, ale wiele scenariuszy zaawansowanych nie jest jeszcze obsługiwanych.</span><span class="sxs-lookup"><span data-stu-id="d629e-667">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="d629e-668">Na przykład `<use>` Tagi nie są obecnie przestrzegane i `@bind` nie mogą być używane z niektórymi tagami SVG.</span><span class="sxs-lookup"><span data-stu-id="d629e-668">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="d629e-669">Oczekujemy, że te ograniczenia są opisane w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="d629e-669">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d629e-670">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d629e-670">Additional resources</span></span>

* <span data-ttu-id="d629e-671"><xref:security/blazor/server>&ndash; Zawiera wskazówki dotyczące tworzenia aplikacji serwera Blazor, które muszą będą konkurować o z wyczerpaniem zasobów.</span><span class="sxs-lookup"><span data-stu-id="d629e-671"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
