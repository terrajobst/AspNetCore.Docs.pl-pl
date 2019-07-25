---
title: Tworzenie i używanie składników ASP.NET Core Razor
author: guardrex
description: Dowiedz się, jak tworzyć i używać składników Razor, w tym jak powiązać z danymi, obsługiwać zdarzenia i zarządzać cyklem życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2019
uid: blazor/components
ms.openlocfilehash: 0bca723ba32d96a69bff1b3138051611d94b2a37
ms.sourcegitcommit: 16502797ea749e2690feaa5e652a65b89c007c89
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68483151"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="d6dc9-103">Tworzenie i używanie składników ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="d6dc9-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="d6dc9-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d6dc9-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="d6dc9-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6dc9-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d6dc9-106">Aplikacje Blazor są kompilowane przy użyciu *składników*programu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="d6dc9-107">Składnik jest niezależnym fragmentem interfejsu użytkownika (UI), takim jak strona, okno dialogowe lub formularz.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="d6dc9-108">Składnik zawiera znaczniki HTML i logikę przetwarzania wymagane do iniekcji danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="d6dc9-109">Składniki są elastyczne i lekkie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="d6dc9-110">Mogą być zagnieżdżane, ponownie używane i udostępniane między projektami.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="d6dc9-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="d6dc9-111">Component classes</span></span>

<span data-ttu-id="d6dc9-112">Składniki są zaimplementowane w plikach składników [Razor](xref:mvc/views/razor) ( *. Razor*) przy użyciu kombinacji C# i znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="d6dc9-113">Składnik w Blazor jest formalnie określany jako *składnik Razor*.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="d6dc9-114">Składniki można tworzyć przy użyciu rozszerzenia pliku *. cshtml* , o ile pliki są identyfikowane jako pliki składników Razor przy użyciu `_RazorComponentInclude` właściwości MSBuild.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-114">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="d6dc9-115">Na przykład aplikacja, która określa, że wszystkie pliki *. cshtml* w folderze *Pages* powinny być traktowane jako pliki składników Razor:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-115">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="d6dc9-116">Interfejs użytkownika dla składnika jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="d6dc9-117">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="d6dc9-118">Po skompilowaniu aplikacji logika znaczników HTML i C# renderowania jest konwertowana na klasę składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="d6dc9-119">Nazwa wygenerowanej klasy jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="d6dc9-120">Elementy członkowskie klasy składnika są zdefiniowane w `@code` bloku.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="d6dc9-121">`@code` W bloku stan składnika (właściwości, pola) jest określany przy użyciu metod obsługi zdarzeń lub definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="d6dc9-122">Dozwolony jest więcej `@code` niż jeden blok.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="d6dc9-123">W poprzednich wersjach ASP.NET Core `@functions` bloki zostały użyte do tego samego celu co `@code` bloki.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-123">In previous versions of ASP.NET Core, `@functions` blocks were used for the same purpose as `@code` blocks.</span></span> <span data-ttu-id="d6dc9-124">`@functions`bloki nadal działają, ale zalecamy użycie `@code` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-124">`@functions` blocks continue to work, but we recommend using the `@code` directive.</span></span>

<span data-ttu-id="d6dc9-125">Elementy członkowskie składnika mogą być następnie używane jako część logiki renderowania składnika przy użyciu C# wyrażeń, które zaczynają `@`się od.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-125">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="d6dc9-126">Na przykład C# pole jest renderowane przez utworzenie prefiksu `@` do nazwy pola.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="d6dc9-127">Poniższy przykład szacuje i renderuje:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="d6dc9-128">`_headingFontStyle`na wartość właściwości CSS dla elementu `font-style`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="d6dc9-129">`_headingText`do zawartości `<h1>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="d6dc9-130">Po pierwszym wyrenderowaniu składnika składnik generuje jego drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="d6dc9-131">Blazor następnie porównuje nowe drzewo renderowania z poprzednią i zastosuje wszelkie modyfikacje Document Object Model przeglądarki (DOM).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="d6dc9-132">Składniki są zwykłymi C# klasami i mogą być umieszczane w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="d6dc9-133">Składniki, które generują strony sieci Web, zwykle znajdują się w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="d6dc9-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="d6dc9-134">Składniki niestronicowe są często umieszczane w  folderze udostępnionym lub w folderze niestandardowym dodanym do projektu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="d6dc9-135">Aby użyć folderu niestandardowego, należy dodać przestrzeń nazw folderu niestandardowego do składnika nadrzędnego lub do pliku *_Imports. Razor* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-135">To use a custom folder, either add the custom folder's namespace to the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="d6dc9-136">Na przykład następująca przestrzeń nazw sprawia, że składniki w  folderze Components są dostępne, gdy główna przestrzeń nazw `WebApplication`aplikacji to:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="d6dc9-137">Integrowanie składników w aplikacjach Razor Pages i MVC</span><span class="sxs-lookup"><span data-stu-id="d6dc9-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="d6dc9-138">Używaj składników z istniejącymi aplikacjami Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="d6dc9-139">Nie ma potrzeby ponownego zapisywania istniejących stron lub widoków w celu używania składników Razor.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="d6dc9-140">Gdy strona lub widok są renderowane, składniki są wstępnie renderowane w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="d6dc9-141">Aby renderować składnik ze strony lub widoku, użyj `RenderComponentAsync<TComponent>` metody pomocnika HTML:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-141">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="d6dc9-142">Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true".</span><span class="sxs-lookup"><span data-stu-id="d6dc9-142">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="d6dc9-143">Składniki nie mogą używać scenariuszy dotyczących widoków i stron, takich jak częściowe widoki i sekcje.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-143">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="d6dc9-144">Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-144">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="d6dc9-145">Aby uzyskać więcej informacji na temat sposobu renderowania składników i zarządzania stanem składnika w aplikacjach po stronie serwera Blazor, zobacz <xref:blazor/hosting-models> artykuł.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-145">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="using-components"></a><span data-ttu-id="d6dc9-146">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="d6dc9-146">Using components</span></span>

<span data-ttu-id="d6dc9-147">Składniki mogą zawierać inne składniki, deklarując je za pomocą składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-147">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="d6dc9-148">Znaczniki użycia składnika wyglądają jak tag HTML, gdzie nazwa znacznika jest typem składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-148">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="d6dc9-149">Poniższy znacznik w *indeksie. Razor* renderuje `HeadingComponent` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-149">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="d6dc9-150">*Składniki/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-150">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

## <a name="component-parameters"></a><span data-ttu-id="d6dc9-151">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="d6dc9-151">Component parameters</span></span>

<span data-ttu-id="d6dc9-152">Składniki mogą mieć *Parametry składnika*, które są zdefiniowane przy użyciu właściwości ( zwykle niepubliczny) w `[Parameter]` klasie składnika z atrybutem.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-152">Components can have *component parameters*, which are defined using properties (usually *non-public*) on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="d6dc9-153">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-153">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="d6dc9-154">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-154">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="d6dc9-155">W poniższym przykładzie `ParentComponent` `Title` Właściwość ustawia wartość właściwości `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-155">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="d6dc9-156">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-156">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="d6dc9-157">Zawartość podrzędna</span><span class="sxs-lookup"><span data-stu-id="d6dc9-157">Child content</span></span>

<span data-ttu-id="d6dc9-158">Składniki mogą ustawiać zawartość innego składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-158">Components can set the content of another component.</span></span> <span data-ttu-id="d6dc9-159">Składnik Assigner zawiera zawartość między tagami, które określają składnik do odbioru.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-159">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="d6dc9-160">W poniższym przykładzie `ChildComponent` `ChildContent` ma właściwość, która reprezentuje `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-160">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="d6dc9-161">Wartość `ChildContent` jest umieszczana w znacznikach składnika, gdzie zawartość powinna być renderowana.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-161">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="d6dc9-162">Wartość `ChildContent` jest odbierana ze składnika nadrzędnego i renderowany w `panel-body`panelu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-162">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="d6dc9-163">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-163">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="d6dc9-164">Właściwość otrzymująca `RenderFragment` zawartość musi być nazywana `ChildContent` Konwencją.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-164">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="d6dc9-165">Poniższe elementy `ParentComponent` mogą zapewnić zawartość do `ChildComponent` renderowania przez `<ChildComponent>` umieszczenie zawartości wewnątrz tagów.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-165">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="d6dc9-166">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-166">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="d6dc9-167">Korzystając atrybutów i dowolne parametry</span><span class="sxs-lookup"><span data-stu-id="d6dc9-167">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="d6dc9-168">Składniki mogą przechwytywać i renderować dodatkowe atrybuty oprócz zadeklarowanych parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-168">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="d6dc9-169">Dodatkowe atrybuty mogą być przechwytywane w słowniku, a następnie *splatted* do elementu, gdy składnik jest renderowany przy `@attributes` użyciu dyrektywy Razor.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-169">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the `@attributes` Razor directive.</span></span> <span data-ttu-id="d6dc9-170">Ten scenariusz jest przydatny podczas definiowania składnika, który generuje element znaczników, który obsługuje różne dostosowania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-170">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="d6dc9-171">Na przykład może być żmudnym do definiowania atrybutów oddzielnie dla `<input>` , który obsługuje wiele parametrów.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-171">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="d6dc9-172">W `<input>` poniższym przykładzie pierwszy element (`id="useIndividualParams"`) używa pojedynczych parametrów składnika, podczas gdy drugi `<input>` element (`id="useAttributesDict"`) używa atrybutu korzystając:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-172">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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
    private string Maxlength { get; set; } = "10";

    [Parameter]
    private string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    private string Required { get; set; } = "required";

    [Parameter]
    private string Size { get; set; } = "50";

    [Parameter]
    private Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" }, 
            { "placeholder", "Input placeholder text" }, 
            { "required", "true" }, 
            { "size", "50" }
        };
}
```

<span data-ttu-id="d6dc9-173">Typ parametru musi być zaimplementowany `IEnumerable<KeyValuePair<string, object>>` przy użyciu kluczy ciągu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-173">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="d6dc9-174">Korzystanie `IReadOnlyDictionary<string, object>` z programu jest również opcją w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-174">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="d6dc9-175">Renderowane `<input>` elementy korzystające z obu metod są identyczne:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-175">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="true"
       size="50">
```

<span data-ttu-id="d6dc9-176">Aby zaakceptować dowolne atrybuty, zdefiniuj parametr składnika przy użyciu `[Parameter]` atrybutu `CaptureUnmatchedAttributes` z właściwością ustawioną na `true`:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-176">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedAttributes` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedAttributes = true)]
    private Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="d6dc9-177">`CaptureUnmatchedAttributes` Właściwość on`[Parameter]` umożliwia dopasowanie parametru do wszystkich atrybutów, które nie są zgodne z żadnym innym parametrem.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-177">The `CaptureUnmatchedAttributes` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="d6dc9-178">Składnik może definiować tylko jeden parametr z `CaptureUnmatchedAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-178">A component can only define a single parameter with `CaptureUnmatchedAttributes`.</span></span> <span data-ttu-id="d6dc9-179">Typ właściwości używany z elementem `CaptureUnmatchedAttributes` musi być możliwy do przypisania `Dictionary<string, object>` z klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-179">The property type used with `CaptureUnmatchedAttributes` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="d6dc9-180">`IEnumerable<KeyValuePair<string, object>>`lub `IReadOnlyDictionary<string, object>` są również opcje w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-180">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="d6dc9-181">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="d6dc9-181">Data binding</span></span>

<span data-ttu-id="d6dc9-182">Powiązanie danych zarówno ze składnikami, jak i elementami modelu dom jest `@bind` realizowane przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-182">Data binding to both components and DOM elements is accomplished with the `@bind` attribute.</span></span> <span data-ttu-id="d6dc9-183">Poniższy przykład wiąże `_italicsCheck` pole z zaznaczonym stanem pola wyboru:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-183">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="d6dc9-184">Gdy to pole wyboru jest zaznaczone i wyczyszczone, wartość właściwości jest aktualizowana `true` odpowiednio `false`do i.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-184">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="d6dc9-185">To pole wyboru jest aktualizowane w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, a nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-185">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="d6dc9-186">Ze względu na to, że składniki są renderowane po wykonaniu kodu procedury obsługi zdarzeń, aktualizacje właściwości są zwykle odzwierciedlane natychmiast w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-186">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="d6dc9-187">Używanie `@bind` `<input @bind="CurrentValue" />`z właściwością () jest zasadniczo równoważne z następującymi: `CurrentValue`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-187">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="d6dc9-188">Gdy składnik jest renderowany, `value` element wejściowy pochodzi `CurrentValue` z właściwości.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-188">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="d6dc9-189">Gdy użytkownik wpisze w polu tekstowym, `onchange` zdarzenie jest wyzwalane, `CurrentValue` a właściwość jest ustawiona na wartość zmieniona.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-189">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="d6dc9-190">W rzeczywistości generowanie kodu jest nieco bardziej skomplikowane, ponieważ `@bind` obsługuje kilka przypadków, w których są wykonywane konwersje typów.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-190">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="d6dc9-191">W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia `value` z atrybutem i obsługuje zmiany przy użyciu zarejestrowanej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-191">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="d6dc9-192">Oprócz obsługi `onchange` zdarzeń ze `@bind` składnią właściwość lub pole można powiązać przy użyciu `@bind-value` innych zdarzeń, `event` określając atrybut z parametrem.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-192">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an `@bind-value` attribute with an `event` parameter.</span></span> <span data-ttu-id="d6dc9-193">Poniższy przykład wiąże się z `CurrentValue` właściwością `oninput` zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-193">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="d6dc9-194">W przeciwieństwie do `onchange`, które jest wyzwalane, gdy `oninput` element utraci fokus, jest uruchamiany po zmianie wartości pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-194">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="d6dc9-195">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="d6dc9-195">**Format strings**</span></span>

<span data-ttu-id="d6dc9-196">Powiązanie danych działa z <xref:System.DateTime> ciągami formatu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-196">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="d6dc9-197">W tej chwili nie są dostępne inne wyrażenia formatu, takie jak formaty walutowe lub liczbowe.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-197">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="d6dc9-198">Ten `@bind:format` atrybut określa format daty, który ma zostać zastosowany `value` do `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-198">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="d6dc9-199">Format jest również używany do analizowania wartości w przypadku `onchange` wystąpienia zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-199">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="d6dc9-200">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="d6dc9-200">**Component parameters**</span></span>

<span data-ttu-id="d6dc9-201">Powiązanie rozpoznaje również parametry składnika, gdzie `@bind-{property}` można powiązać wartość właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-201">Binding also recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="d6dc9-202">Następujący składnik podrzędny (`ChildComponent`) `Year` ma parametr składnika i `YearChanged` wywołanie zwrotne:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-202">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="d6dc9-203">`EventCallback<T>`wyjaśniono w sekcji [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="d6dc9-203">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="d6dc9-204">Poniższy składnik nadrzędny używa `ChildComponent` i wiąże `ParentYear` parametr `Year` z elementu nadrzędnego z parametrem w składniku podrzędnym:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-204">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="d6dc9-205">Załadowanie `ParentComponent` powoduje utworzenie następującej adjustacji:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-205">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="d6dc9-206">Jeśli wartość `ParentYear` właściwości zostanie zmieniona przez wybranie przycisku `ParentComponent`w `ChildComponent` , `Year` właściwość jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-206">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="d6dc9-207">Nowa wartość `Year` jest renderowana w interfejsie użytkownika podczas jego `ParentComponent` renderowania:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-207">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="d6dc9-208">Parametr jest możliwy do powiązania, ponieważ ma zdarzenie towarzyszące `YearChanged` `Year` pasujące do typu parametru. `Year`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-208">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="d6dc9-209">Zgodnie z Konwencją, `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo równoważne zapisowi:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-209">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="d6dc9-210">Ogólnie rzecz biorąc, właściwość może być powiązana z odpowiednią obsługą zdarzeń `@bind-property:event` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-210">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="d6dc9-211">Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` użyciem następujących dwóch atrybutów:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-211">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="d6dc9-212">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="d6dc9-212">Event handling</span></span>

<span data-ttu-id="d6dc9-213">Składniki Razor zapewniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-213">Razor components provide event handling features.</span></span> <span data-ttu-id="d6dc9-214">Dla atrybutu elementu HTML o nazwie `on<event>` (na `onclick` przykład i `onsubmit`) z wartością typu delegata składniki Razor traktują wartość atrybutu jako procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-214">For an HTML element attribute named `on<event>` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="d6dc9-215">Nazwa atrybutu zawsze zaczyna się od `@on`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-215">The attribute's name always starts with `@on`.</span></span>

<span data-ttu-id="d6dc9-216">Poniższy kod wywołuje metodę, `UpdateHeading` gdy przycisk zostanie wybrany w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-216">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="d6dc9-217">Poniższy kod wywołuje metodę, `CheckChanged` gdy pole wyboru zostanie zmienione w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-217">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckboxChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="d6dc9-218">Procedury obsługi zdarzeń mogą również być asynchroniczne i zwracać <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-218">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="d6dc9-219">Nie ma potrzeby ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-219">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="d6dc9-220">Wyjątki są rejestrowane, gdy wystąpią.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-220">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="d6dc9-221">W poniższym przykładzie `UpdateHeading` jest wywoływana asynchronicznie po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-221">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="d6dc9-222">W przypadku niektórych zdarzeń dozwolone są typy argumentów zdarzeń specyficznych dla zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-222">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="d6dc9-223">Jeśli dostęp do jednego z tych typów zdarzeń nie jest konieczny, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-223">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="d6dc9-224">Obsługiwane [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/UIEventArgs.cs) są przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-224">Supported [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/UIEventArgs.cs) are shown in the following table.</span></span>

| <span data-ttu-id="d6dc9-225">Zdarzenie</span><span class="sxs-lookup"><span data-stu-id="d6dc9-225">Event</span></span> | <span data-ttu-id="d6dc9-226">Class</span><span class="sxs-lookup"><span data-stu-id="d6dc9-226">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="d6dc9-227">Schowek</span><span class="sxs-lookup"><span data-stu-id="d6dc9-227">Clipboard</span></span> | `UIClipboardEventArgs` |
| <span data-ttu-id="d6dc9-228">Przeciągnij</span><span class="sxs-lookup"><span data-stu-id="d6dc9-228">Drag</span></span>  | <span data-ttu-id="d6dc9-229">`UIDragEventArgs`służy do przechowywania przeciąganych danych podczas operacji przeciągania i upuszczania oraz może zawierać jeden lub więcej `UIDataTransferItem`. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-229">`UIDragEventArgs` &ndash; `DataTransfer` is used to hold the dragged data during a drag and drop operation and may hold one or more `UIDataTransferItem`.</span></span> <span data-ttu-id="d6dc9-230">`UIDataTransferItem`reprezentuje jeden element danych przeciągania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-230">`UIDataTransferItem` represents one drag data item.</span></span> |
| <span data-ttu-id="d6dc9-231">Błąd</span><span class="sxs-lookup"><span data-stu-id="d6dc9-231">Error</span></span> | `UIErrorEventArgs` |
| <span data-ttu-id="d6dc9-232">Fokus</span><span class="sxs-lookup"><span data-stu-id="d6dc9-232">Focus</span></span> | <span data-ttu-id="d6dc9-233">`UIFocusEventArgs`Nie obejmuje obsługi dla `relatedTarget`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="d6dc9-233">`UIFocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="d6dc9-234">`<input>`stąp</span><span class="sxs-lookup"><span data-stu-id="d6dc9-234">`<input>` change</span></span> | `UIChangeEventArgs` |
| <span data-ttu-id="d6dc9-235">Klawiatury</span><span class="sxs-lookup"><span data-stu-id="d6dc9-235">Keyboard</span></span> | `UIKeyboardEventArgs` |
| <span data-ttu-id="d6dc9-236">Wskaźnik</span><span class="sxs-lookup"><span data-stu-id="d6dc9-236">Mouse</span></span> | `UIMouseEventArgs` |
| <span data-ttu-id="d6dc9-237">Wskaźnik myszy</span><span class="sxs-lookup"><span data-stu-id="d6dc9-237">Mouse pointer</span></span> | `UIPointerEventArgs` |
| <span data-ttu-id="d6dc9-238">Kółko myszy</span><span class="sxs-lookup"><span data-stu-id="d6dc9-238">Mouse wheel</span></span> | `UIWheelEventArgs` |
| <span data-ttu-id="d6dc9-239">Postęp</span><span class="sxs-lookup"><span data-stu-id="d6dc9-239">Progress</span></span> | `UIProgressEventArgs` |
| <span data-ttu-id="d6dc9-240">Dotyk</span><span class="sxs-lookup"><span data-stu-id="d6dc9-240">Touch</span></span> | <span data-ttu-id="d6dc9-241">`UITouchEventArgs`&ndash; reprezentujepojedynczypunktkontaktunaurządzeniuz`UITouchPoint` wrażliwym dotknięciem.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-241">`UITouchEventArgs` &ndash; `UITouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="d6dc9-242">Aby uzyskać informacje o zachowaniu właściwości i obsłudze zdarzeń zdarzeń w powyższej tabeli, zobacz [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/UIEventArgs.cs) w źródle odwołania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-242">For information on the properties and event handling behavior of the events in the preceding table, see [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/UIEventArgs.cs) in the reference source.</span></span>
  
<span data-ttu-id="d6dc9-243">Wyrażenia lambda mogą być również używane:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-243">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="d6dc9-244">Często wygodnie jest blisko dodatkowych wartości, na przykład podczas iteracji na zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-244">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="d6dc9-245">Poniższy przykład tworzy trzy przyciski, z których każdy wywołuje `UpdateHeading` przekazanie argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`), po wybraniu w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-245">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="d6dc9-246">**Nie** używaj zmiennej Loop (`i`) w `for` pętli bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-246">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="d6dc9-247">W przeciwnym razie ta sama zmienna jest używana przez wszystkie wyrażenia `i`lambda, co sprawia, że wartość jest taka sama we wszystkich lambdach.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-247">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="d6dc9-248">Zawsze Przechwytuj wartość w zmiennej lokalnej (`buttonNumber` w poprzednim przykładzie), a następnie użyj jej.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-248">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="d6dc9-249">EventCallback</span><span class="sxs-lookup"><span data-stu-id="d6dc9-249">EventCallback</span></span>

<span data-ttu-id="d6dc9-250">Typowym scenariuszem ze składnikami zagnieżdżonymi jest uruchomienie metody składnika nadrzędnego, gdy występuje&mdash;zdarzenie składnika podrzędnego, gdy wystąpi zdarzenie w elemencie `onclick` podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-250">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="d6dc9-251">Aby uwidocznić zdarzenia między składnikami, `EventCallback`Użyj.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-251">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="d6dc9-252">Składnik nadrzędny może przypisać metodę wywołania zwrotnego do składnika `EventCallback`podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-252">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="d6dc9-253">W przykładowej aplikacji pokazano, jak `onclick` program obsługi przycisku został `EventCallback` skonfigurowany tak, aby otrzymać delegata z przykładu `ParentComponent`. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-253">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="d6dc9-254">Typ ma wartość, która jest odpowiednia dla `onclick` zdarzenia z urządzenia peryferyjnego: `UIMouseEventArgs` `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-254">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="d6dc9-255">Ustawia element podrzędny `ShowMessage`dojegometody: `EventCallback<T>` `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-255">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="d6dc9-256">Gdy przycisk zostanie wybrany w `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-256">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="d6dc9-257">`ParentComponent` Metoda`ShowMessage` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-257">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="d6dc9-258">`messageText`jest aktualizowany i wyświetlany w `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-258">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="d6dc9-259">Wywołanie `StateHasChanged` nie jest wymagane w metodzie wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-259">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="d6dc9-260">`StateHasChanged`jest automatycznie wywoływana w celu odrenderowania `ParentComponent`elementu, podobnie jak zdarzenia podrzędne wyzwala ponowne renderowanie składnika w obsłudze zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-260">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="d6dc9-261">`EventCallback`i `EventCallback<T>` Zezwalaj na asynchroniczne Delegaty.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-261">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="d6dc9-262">`EventCallback<T>`jest silnie wpisana i wymaga określonego typu argumentu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-262">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="d6dc9-263">`EventCallback`jest słabo wpisywany i zezwala na dowolny typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-263">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="d6dc9-264">`EventCallback<T>` Wywołaj `EventCallback` lub z`InvokeAsync` i await <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-264">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="d6dc9-265">Używaj `EventCallback` i`EventCallback<T>` dla parametrów składnika Obsługa zdarzeń i powiązania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-265">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="d6dc9-266">Preferuj silnie wpisaną `EventCallback<T>` `EventCallback`wartość.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-266">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="d6dc9-267">`EventCallback<T>`zapewnia lepszą opinię o błędach dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-267">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="d6dc9-268">Podobnie jak w przypadku innych programów obsługi zdarzeń interfejsu użytkownika, określenie parametru zdarzenia jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-268">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="d6dc9-269">Użyj `EventCallback` w przypadku braku wartości przekazywania do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-269">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="d6dc9-270">Przechwyć odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="d6dc9-270">Capture references to components</span></span>

<span data-ttu-id="d6dc9-271">Odwołania do składników zapewniają sposób odwoływania się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia, takie `Show` jak `Reset`lub.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-271">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="d6dc9-272">Aby przechwycić odwołanie do składnika, Dodaj `@ref` atrybut do składnika podrzędnego, a następnie Zdefiniuj pole o tej samej nazwie i tym samym typie co składnik podrzędny.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-272">To capture a component reference, add a `@ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

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

<span data-ttu-id="d6dc9-273">Gdy składnik jest renderowany, `loginDialog` pole zostanie wypełnione `MyLoginDialog` wystąpieniem składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-273">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="d6dc9-274">Następnie można wywołać metody .NET w wystąpieniu składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-274">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6dc9-275">Zmienna jest wypełniana tylko po wyrenderowaniu składnika, a jego wyjście `MyLoginDialog` zawiera element. `loginDialog`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-275">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="d6dc9-276">Do tego momentu nie ma niczego do odwołania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-276">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="d6dc9-277">Aby manipulować odwołaniami do składników po zakończeniu renderowania składnika, użyj `OnAfterRenderAsync` metod lub. `OnAfterRender`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-277">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="d6dc9-278">Podczas przechwytywania odwołań do składników użycie podobnej składni do [przechwytywania odwołań do elementów](xref:blazor/javascript-interop#capture-references-to-elements)nie jest funkcją międzyoperacyjności [języka JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="d6dc9-278">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="d6dc9-279">Odwołania do składników nie są przenoszone&mdash;do kodu JavaScript, są używane tylko w kodzie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-279">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="d6dc9-280">**Nie** należy używać odwołań do składników do mutacji stanu składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-280">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="d6dc9-281">Zamiast tego należy używać zwykłych parametrów deklaratywnych do przekazywania danych do składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-281">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="d6dc9-282">Użycie normalnych parametrów deklaratywnych powoduje, że składniki podrzędne, które automatycznie uruchamiają się w prawidłowym czasie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-282">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="d6dc9-283">Służy @key do kontrolowania zachowywania elementów i składników</span><span class="sxs-lookup"><span data-stu-id="d6dc9-283">Use @key to control the preservation of elements and components</span></span>

<span data-ttu-id="d6dc9-284">Podczas renderowania listy elementów lub składników oraz elementów lub składników, które następnie zmieniają się, algorytm diff Blazor musi zdecydować, które z poprzednich elementów lub składników mogą być zachowywane i jak obiekty modelu powinny być mapowane na nie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-284">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="d6dc9-285">Zwykle ten proces jest automatyczny i można go zignorować, ale istnieją przypadki, w których może być konieczne sterowanie procesem.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-285">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="d6dc9-286">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-286">Consider the following example:</span></span>

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

<span data-ttu-id="d6dc9-287">Zawartość `People` kolekcji może ulec zmianie z wstawionymi, usuniętymi lub z ponownymi zamówieniami.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-287">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="d6dc9-288">Gdy składnik jest przerenderowany, `<DetailsEditor>` składnik może ulec zmianie, aby otrzymywać różne `Details` wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-288">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="d6dc9-289">Może to spowodować bardziej złożone odwzorowanie niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-289">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="d6dc9-290">W niektórych przypadkach odzyskanie może prowadzić do zauważalnych różnic w zachowaniu, takich jak brak fokusu elementu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-290">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="d6dc9-291">Proces mapowania można kontrolować przy użyciu `@key` atrybutu dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-291">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="d6dc9-292">`@key`powoduje, że algorytm różnicowego gwarantuje zachowywanie elementów lub składników na podstawie wartości klucza:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-292">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="d6dc9-293">`<DetailsEditor>` `person` Po zmianie `People` kolekcji algorytm różnicowy zachowuje skojarzenie między wystąpieniami i wystąpieniami:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-293">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="d6dc9-294">Jeśli element `Person` zostanie usunięty `People` z listy, tylko odpowiednie `<DetailsEditor>` wystąpienie zostanie usunięte z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-294">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="d6dc9-295">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-295">Other instances are left unchanged.</span></span>
* <span data-ttu-id="d6dc9-296">Jeśli na liście `<DetailsEditor>` zostaniewstawionywpewnymmiejscu,jednonowewystąpieniezostaniewstawionew`Person` odpowiednim położeniu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-296">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="d6dc9-297">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-297">Other instances are left unchanged.</span></span>
* <span data-ttu-id="d6dc9-298">W `Person` przypadku ponownego uporządkowania wpisów odpowiednie `<DetailsEditor>` wystąpienia są zachowywane i uporządkowane w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-298">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="d6dc9-299">W niektórych scenariuszach użycie programu `@key` minimalizuje złożoność operacji renderowania i pozwala uniknąć potencjalnych problemów związanych ze stanem częściowych elementów modelu dom, takich jak pozycja fokusu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-299">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6dc9-300">Klucze są lokalne dla każdego elementu kontenera lub składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-300">Keys are local to each container element or component.</span></span> <span data-ttu-id="d6dc9-301">Klucze nie są porównywane globalnie w całym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-301">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="d6dc9-302">Kiedy używać@key</span><span class="sxs-lookup"><span data-stu-id="d6dc9-302">When to use @key</span></span>

<span data-ttu-id="d6dc9-303">Zazwyczaj warto używać `@key` zawsze, gdy lista jest renderowana (na przykład `@foreach` w bloku) i odpowiednia `@key`wartość istnieje do zdefiniowania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-303">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="d6dc9-304">Można również użyć `@key` , aby uniemożliwić Blazor z zachowaniem poddrzewa elementu lub składnika, gdy zmieniany jest obiekt:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-304">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="d6dc9-305">Jeśli `@currentPerson` zmiany `<div>` , dyrektywa Blazor wymusza odrzucanie całości i jego obiektów podrzędnych oraz ponowne skompilowanie poddrzewa w interfejsie użytkownika za pomocą nowych elementów i składników. `@key`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-305">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="d6dc9-306">Może to być przydatne, jeśli zachodzi konieczność zagwarantowania, że stan `@currentPerson` interfejsu użytkownika nie jest zachowywany w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-306">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="d6dc9-307">Kiedy nie używać@key</span><span class="sxs-lookup"><span data-stu-id="d6dc9-307">When not to use @key</span></span>

<span data-ttu-id="d6dc9-308">W przypadku różnicowania w programie `@key`występuje koszt wydajności.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-308">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="d6dc9-309">Koszt wydajności nie jest duży, ale określa `@key` tylko, czy kontrolowanie reguł utrwalania elementu lub składnika przynosi korzyści dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-309">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="d6dc9-310">Nawet jeśli `@key` nie jest używany, Blazor zachowuje elementy podrzędne i wystąpienia składników tak dużo, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-310">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="d6dc9-311">Jedyną zaletą korzystania z `@key` programu jest kontrola nad sposobem, w *jaki* wystąpienia modelu są mapowane na zachowane wystąpienia składników, zamiast algorytmu różnicowego, wybierając mapowanie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-311">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="d6dc9-312">Jakie wartości mają być używane przez@key</span><span class="sxs-lookup"><span data-stu-id="d6dc9-312">What values to use for @key</span></span>

<span data-ttu-id="d6dc9-313">Ogólnie rzecz biorąc, warto podać jeden z następujących rodzajów wartości dla `@key`:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-313">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="d6dc9-314">Wystąpienia obiektów modelu (na przykład `Person` wystąpienie takie jak w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-314">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="d6dc9-315">Zapewnia to zachowywanie na podstawie równości odwołań do obiektów.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-315">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="d6dc9-316">Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`lub `Guid`).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-316">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="d6dc9-317">Należy unikać dostarczania wartości, która może nieoczekiwanie powodować konflikt.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-317">Avoid supplying a value that can clash unexpectedly.</span></span> <span data-ttu-id="d6dc9-318">Jeśli `@key="@someObject.GetHashCode()"` jest podany, mogą wystąpić nieoczekiwane konflikty, ponieważ kody skrótów niepowiązanych obiektów mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-318">If `@key="@someObject.GetHashCode()"` is supplied, unexpected clashes may occur because the hash codes of unrelated objects can be the same.</span></span> <span data-ttu-id="d6dc9-319">Jeśli w tym `@key` samym elemencie nadrzędnym są żądane wartości powodujące `@key` konflikt, wartości nie będą honorowane.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-319">If clashing `@key` values are requested within the same parent, the `@key` values won't be honored.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="d6dc9-320">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="d6dc9-320">Lifecycle methods</span></span>

<span data-ttu-id="d6dc9-321">`OnInitAsync`i `OnInit` wykonaj kod w celu zainicjowania składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-321">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="d6dc9-322">Aby wykonać operację asynchroniczną, użyj `OnInitAsync` `await` słowa kluczowego i dla operacji:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-322">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="d6dc9-323">W przypadku operacji synchronicznych należy `OnInit`użyć:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-323">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="d6dc9-324">`OnParametersSetAsync`i `OnParametersSet` są wywoływane, gdy składnik otrzymał parametry od jego elementu nadrzędnego i wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-324">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="d6dc9-325">Te metody są wykonywane po zainicjowaniu składnika i za każdym razem, gdy składnik jest renderowany:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-325">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="d6dc9-326">`OnAfterRenderAsync`i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-326">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="d6dc9-327">Odwołania do elementów i składników są wypełniane w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-327">Element and component references are populated at this point.</span></span> <span data-ttu-id="d6dc9-328">Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-328">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="d6dc9-329">Obsługuj niekompletne akcje asynchroniczne podczas renderowania</span><span class="sxs-lookup"><span data-stu-id="d6dc9-329">Handle incomplete async actions at render</span></span>

<span data-ttu-id="d6dc9-330">Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogą nie zostać zakończone przed renderowaniem składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-330">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="d6dc9-331">Obiekty mogą być `null` lub uzupełniane wypełniane danymi podczas wykonywania metody cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-331">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="d6dc9-332">Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-332">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="d6dc9-333">Renderuj zastępcze elementy interfejsu użytkownika (na przykład komunikat ładowania), gdy obiekty `null`są.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-333">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="d6dc9-334">W składniku `OnInitAsync` szablonów Blazor został zastąpiony do asynchronicznie odbierania danych prognozy (`forecasts`). `FetchData`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-334">In the `FetchData` component of the Blazor templates, `OnInitAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="d6dc9-335">Gdy `forecasts` tak `null`jest, zostanie wyświetlony komunikat ładowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-335">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="d6dc9-336">Po zakończeniu `OnInitAsync` zwracany przez program składnik zostanie przerenderowany ze zaktualizowanym stanem. `Task`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-336">After the `Task` returned by `OnInitAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="d6dc9-337">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-337">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="d6dc9-338">Wykonaj kod przed ustawieniem parametrów</span><span class="sxs-lookup"><span data-stu-id="d6dc9-338">Execute code before parameters are set</span></span>

<span data-ttu-id="d6dc9-339">`SetParameters`można zastąpić, aby wykonać kod przed ustawieniem parametrów:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-339">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="d6dc9-340">Jeśli `base.SetParameters` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-340">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="d6dc9-341">Na przykład parametry przychodzące nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-341">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="d6dc9-342">Pomiń odświeżanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="d6dc9-342">Suppress refreshing of the UI</span></span>

<span data-ttu-id="d6dc9-343">`ShouldRender`można zastąpić, aby pominąć odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-343">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="d6dc9-344">Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-344">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="d6dc9-345">Nawet jeśli `ShouldRender` jest zastępowany, składnik jest zawsze początkowo renderowany.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-345">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="d6dc9-346">Usuwanie składnika z interfejsem IDisposable</span><span class="sxs-lookup"><span data-stu-id="d6dc9-346">Component disposal with IDisposable</span></span>

<span data-ttu-id="d6dc9-347">W przypadku zaimplementowania <xref:System.IDisposable>składnika [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-347">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="d6dc9-348">Poniższy składnik używa `@implements IDisposable` `Dispose` i metody:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-348">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="d6dc9-349">Routing</span><span class="sxs-lookup"><span data-stu-id="d6dc9-349">Routing</span></span>

<span data-ttu-id="d6dc9-350">Routing w Blazor jest realizowany przez dostarczenie szablonu trasy do każdego dostępnego składnika w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-350">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="d6dc9-351">Gdy plik Razor z `@page` dyrektywą jest kompilowany, wygenerowana Klasa ma <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określony szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-351">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="d6dc9-352">W czasie wykonywania router szuka klas składników za pomocą `RouteAttribute` i renderuje, w zależności od tego, który składnik ma szablon trasy zgodny z żądanym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-352">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="d6dc9-353">Do składnika można zastosować wiele szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-353">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="d6dc9-354">Poniższy składnik odpowiada na żądania dla `/BlazorRoute` i: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-354">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="d6dc9-355">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="d6dc9-355">Route parameters</span></span>

<span data-ttu-id="d6dc9-356">Składniki mogą odbierać parametry trasy z szablonu trasy dostarczonego w `@page` dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-356">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="d6dc9-357">Router używa parametrów trasy, aby wypełnić odpowiednie parametry składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-357">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="d6dc9-358">*Składnik parametru trasy*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-358">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="d6dc9-359">Parametry opcjonalne nie są obsługiwane, więc `@page` dwie dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-359">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="d6dc9-360">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-360">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="d6dc9-361">Druga `@page` dyrektywa `Text` przyjmuje parametr Route i przypisuje wartość do właściwości. `{text}`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-361">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="d6dc9-362">Dziedziczenie klasy podstawowej dla środowiska "powiązanego z kodem"</span><span class="sxs-lookup"><span data-stu-id="d6dc9-362">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="d6dc9-363">Pliki składników mieszają znaczniki HTML C# i przetwarzają kod w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-363">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="d6dc9-364">`@inherits` Dyrektywa może służyć do udostępniania aplikacji Blazor z interfejsem "związanym z kodem", który oddziela oznakowanie składników od przetwarzania kodu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-364">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="d6dc9-365">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasę bazową `BlazorRocksBase`,, w celu zapewnienia właściwości i metod składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-365">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="d6dc9-366">*Strony/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-366">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="d6dc9-367">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-367">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="d6dc9-368">Klasa bazowa powinna pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-368">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="d6dc9-369">Importuj składniki</span><span class="sxs-lookup"><span data-stu-id="d6dc9-369">Import components</span></span>

<span data-ttu-id="d6dc9-370">Przestrzeń nazw składnika utworzone przy użyciu Razor jest oparta na:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-370">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="d6dc9-371">Projekt `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-371">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="d6dc9-372">Ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-372">The path from the project root to the component.</span></span> <span data-ttu-id="d6dc9-373">Na przykład, `ComponentsSample/Pages/Index.razor` znajduje się w przestrzeni `ComponentsSample.Pages`nazw.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-373">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="d6dc9-374">Składniki przestrzegają C# reguł powiązań nazw.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-374">Components follow C# name binding rules.</span></span> <span data-ttu-id="d6dc9-375">W przypadku elementu *index. Razor*wszystkie składniki znajdujące się w tym samym folderze, *stronach*i folderze nadrzędnym ( *ComponentsSample*) znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-375">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="d6dc9-376">Składniki zdefiniowane w innej przestrzeni nazw można wprowadzić do zakresu przy użyciu dyrektywy [ \@using używającej](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-376">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="d6dc9-377">Jeśli inny składnik, `NavMenu.razor`,,, istnieje w `ComponentsSample/Shared/`folderze, składnik może być używany w `Index.razor` programie z następującą `@using` instrukcją:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-377">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="d6dc9-378">Do składników można także odwoływać się za pomocą ich w pełni kwalifikowanych nazw, co [ \@](xref:mvc/views/razor#using) eliminuje konieczność stosowania dyrektywy using:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-378">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="d6dc9-379">`global::` Kwalifikacja nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-379">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="d6dc9-380">Importowanie składników przy użyciu `using` instrukcji z aliasami ( `@using Foo = Bar`np.) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-380">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="d6dc9-381">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-381">Partially qualified names aren't supported.</span></span> <span data-ttu-id="d6dc9-382">Na przykład dodawanie `@using ComponentsSample` i odwoływanie `NavMenu.razor` się `<Shared.NavMenu></Shared.NavMenu>` za pomocą nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-382">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="razor-support"></a><span data-ttu-id="d6dc9-383">Obsługa Razor</span><span class="sxs-lookup"><span data-stu-id="d6dc9-383">Razor support</span></span>

<span data-ttu-id="d6dc9-384">**Dyrektywy Razor**</span><span class="sxs-lookup"><span data-stu-id="d6dc9-384">**Razor directives**</span></span>

<span data-ttu-id="d6dc9-385">Dyrektywy Razor przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-385">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="d6dc9-386">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="d6dc9-386">Directive</span></span> | <span data-ttu-id="d6dc9-387">Opis</span><span class="sxs-lookup"><span data-stu-id="d6dc9-387">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="d6dc9-388">\@kodu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-388">\@code</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="d6dc9-389">Dodaje blok C# kodu do składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-389">Adds a C# code block to a component.</span></span> <span data-ttu-id="d6dc9-390">`@code`jest aliasem `@functions`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-390">`@code` is an alias of `@functions`.</span></span> <span data-ttu-id="d6dc9-391">`@code`jest zalecane w `@functions`przypadku.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-391">`@code` is recommended over `@functions`.</span></span> <span data-ttu-id="d6dc9-392">Dozwolony jest więcej `@code` niż jeden blok.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-392">More than one `@code` block is permissible.</span></span> |
| [<span data-ttu-id="d6dc9-393">\@obowiązki</span><span class="sxs-lookup"><span data-stu-id="d6dc9-393">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="d6dc9-394">Dodaje blok C# kodu do składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-394">Adds a C# code block to a component.</span></span> <span data-ttu-id="d6dc9-395">Wybierz `@code` opcję `@functions` powyżej C# dla bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-395">Choose `@code` over `@functions` for C# code blocks.</span></span> |
| `@implements` | <span data-ttu-id="d6dc9-396">Implementuje interfejs dla wygenerowanej klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-396">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="d6dc9-397">\@inherit</span><span class="sxs-lookup"><span data-stu-id="d6dc9-397">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="d6dc9-398">Zapewnia pełną kontrolę nad klasą, którą dziedziczy składnik.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-398">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="d6dc9-399">\@dodanie</span><span class="sxs-lookup"><span data-stu-id="d6dc9-399">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="d6dc9-400">Włącza iniekcję usługi z [kontenera usługi](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-400">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d6dc9-401">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-401">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="d6dc9-402">Określa składnik układu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-402">Specifies a layout component.</span></span> <span data-ttu-id="d6dc9-403">Składniki układu są używane do uniknięcia duplikowania kodu i niespójności.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-403">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="d6dc9-404">\@stronic</span><span class="sxs-lookup"><span data-stu-id="d6dc9-404">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="d6dc9-405">Określa, że składnik powinien obsługiwać żądania bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-405">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="d6dc9-406">`@page` Dyrektywa może być określona z trasą i opcjonalnymi parametrami.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-406">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="d6dc9-407">W przeciwieństwie do Razor Pages `@page` dyrektywa nie musi być pierwszą dyrektywą w górnej części pliku.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-407">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="d6dc9-408">Aby uzyskać więcej informacji, zobacz [Routing](xref:blazor/routing).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-408">For more information, see [Routing](xref:blazor/routing).</span></span> |
| [<span data-ttu-id="d6dc9-409">\@użyciu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-409">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="d6dc9-410">C# Dodajedyrektywędoklasywygenerowanegoskładnika.`using`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-410">Adds the C# `using` directive to the generated component class.</span></span> <span data-ttu-id="d6dc9-411">Obejmuje to również wszystkie składniki zdefiniowane w tej przestrzeni nazw do zakresu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-411">This also brings all the components defined in that namespace into scope.</span></span> |
| [<span data-ttu-id="d6dc9-412">\@obszaru</span><span class="sxs-lookup"><span data-stu-id="d6dc9-412">\@namespace</span></span>](xref:mvc/views/razor#section-6) | <span data-ttu-id="d6dc9-413">Ustawia przestrzeń nazw wygenerowanej klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-413">Sets the namespace of the generated component class.</span></span> |
| [<span data-ttu-id="d6dc9-414">\@przypisane</span><span class="sxs-lookup"><span data-stu-id="d6dc9-414">\@attribute</span></span>](xref:mvc/views/razor#section-7) | <span data-ttu-id="d6dc9-415">Dodaje atrybut do klasy wygenerowanego składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-415">Adds an attribute to the generated component class.</span></span> |

<span data-ttu-id="d6dc9-416">**Warunkowe atrybuty elementu HTML**</span><span class="sxs-lookup"><span data-stu-id="d6dc9-416">**Conditional HTML element attributes**</span></span>

<span data-ttu-id="d6dc9-417">Atrybuty elementu HTML są warunkowo renderowane na podstawie wartości .NET.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-417">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="d6dc9-418">Jeśli wartość jest `false` lub `null`, atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-418">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="d6dc9-419">Jeśli wartość to `true`, atrybut jest renderowany jako zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-419">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="d6dc9-420">W poniższym przykładzie, określa `IsCompleted` , czy `checked` jest renderowany w znacznikach elementu:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-420">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="d6dc9-421">Jeśli `IsCompleted` jest`true`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-421">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="d6dc9-422">Jeśli `IsCompleted` jest`false`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-422">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="d6dc9-423">**Dodatkowe informacje na temat Razor**</span><span class="sxs-lookup"><span data-stu-id="d6dc9-423">**Additional information on Razor**</span></span>

<span data-ttu-id="d6dc9-424">Aby uzyskać więcej informacji na temat Razor, zobacz [informacje dotyczące składnia Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-424">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="d6dc9-425">Nieprzetworzony kod HTML</span><span class="sxs-lookup"><span data-stu-id="d6dc9-425">Raw HTML</span></span>

<span data-ttu-id="d6dc9-426">Ciągi są zwykle renderowane przy użyciu węzłów tekstowych DOM, co oznacza, że wszystkie znaczniki, które mogą zawierać, są ignorowane i traktowane jako tekst literału.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-426">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="d6dc9-427">Aby renderować nieprzetworzony kod HTML, zawiń zawartość HTML w `MarkupString` wartości.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-427">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="d6dc9-428">Wartość jest analizowana jako plik HTML lub SVG i wstawiona do modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-428">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="d6dc9-429">Renderowanie nieprzetworzonego kodu HTML zbudowanego z dowolnego niezaufanego źródła stanowi **zagrożenie bezpieczeństwa** i należy je unikać!</span><span class="sxs-lookup"><span data-stu-id="d6dc9-429">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="d6dc9-430">W poniższym przykładzie pokazano, `MarkupString` jak za pomocą typu dodać blok statycznej zawartości HTML do renderowanego danych wyjściowych składnika:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-430">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="d6dc9-431">Składniki z szablonami</span><span class="sxs-lookup"><span data-stu-id="d6dc9-431">Templated components</span></span>

<span data-ttu-id="d6dc9-432">Składniki z szablonami są składnikami, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które mogą być następnie używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-432">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="d6dc9-433">Składniki z szablonami umożliwiają tworzenie składników wyższego poziomu, które są większe niż zwykłe składniki.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-433">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="d6dc9-434">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-434">A couple of examples include:</span></span>

* <span data-ttu-id="d6dc9-435">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka, wierszy i stopki tabeli.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-435">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="d6dc9-436">Składnik listy, który umożliwia użytkownikowi określenie szablonu do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-436">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="d6dc9-437">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-437">Template parameters</span></span>

<span data-ttu-id="d6dc9-438">Składnik szablonu jest definiowany przez określenie co najmniej jednego parametru składnika typu `RenderFragment` lub. `RenderFragment<T>`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-438">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="d6dc9-439">Fragment renderowania reprezentuje segment interfejsu użytkownika, który jest renderowany przez składnik.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-439">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="d6dc9-440">Fragment renderowania opcjonalnie przyjmuje parametr, który można określić podczas wywoływania fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-440">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="d6dc9-441">`TableTemplate`składnika</span><span class="sxs-lookup"><span data-stu-id="d6dc9-441">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="d6dc9-442">W przypadku korzystania z składnika z szablonem parametry szablonu można określić za pomocą elementów podrzędnych, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="d6dc9-442">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="d6dc9-443">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-443">Template context parameters</span></span>

<span data-ttu-id="d6dc9-444">Argumenty składnika `RenderFragment<T>` typu przekazane jako elementy mają niejawny parametr o `context` nazwie (na przykład z poprzedniego przykładu `@context.PetId`kodu), ale można zmienić nazwę parametru przy użyciu `Context` atrybutu w elemencie podrzędnym postaci.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-444">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="d6dc9-445">W poniższym przykładzie `RowTemplate` `Context` atrybut elementu określa `pet` parametr:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-445">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="d6dc9-446">Alternatywnie można określić `Context` atrybut dla elementu składnika.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-446">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="d6dc9-447">Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-447">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="d6dc9-448">Może to być przydatne, jeśli chcesz określić nazwę parametru zawartości dla niejawnej zawartości podrzędnej (bez żadnego elementu podrzędnego otoki).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-448">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="d6dc9-449">W poniższym przykładzie `Context` atrybut pojawia się `TableTemplate` na elemencie i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-449">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="d6dc9-450">Składniki typu rodzajowego</span><span class="sxs-lookup"><span data-stu-id="d6dc9-450">Generic-typed components</span></span>

<span data-ttu-id="d6dc9-451">Składniki z szablonami są często wpisywane ogólnie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-451">Templated components are often generically typed.</span></span> <span data-ttu-id="d6dc9-452">Na przykład, składnik ogólny `ListViewTemplate` może służyć do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-452">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="d6dc9-453">Aby zdefiniować składnik ogólny, użyj `@typeparam` dyrektywy do określenia parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-453">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="d6dc9-454">W przypadku używania składników o typie ogólnym parametr typu jest wnioskowany, jeśli jest to możliwe:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-454">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="d6dc9-455">W przeciwnym razie parametr typu musi być jawnie określony przy użyciu atrybutu, który jest zgodny z nazwą parametru typu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-455">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="d6dc9-456">W poniższym przykładzie `TItem="Pet"` określa typ:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-456">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="d6dc9-457">Wartości kaskadowe i parametry</span><span class="sxs-lookup"><span data-stu-id="d6dc9-457">Cascading values and parameters</span></span>

<span data-ttu-id="d6dc9-458">W niektórych scenariuszach nie można przepływać danych z składnika nadrzędnego do składnika potomnego przy użyciu [parametrów składnika](#component-parameters), zwłaszcza gdy istnieje kilka warstw składników.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-458">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="d6dc9-459">Wartości kaskadowe i parametry rozwiązują ten problem, zapewniając wygodną metodę dla składnika nadrzędnego, aby zapewnić wartość wszystkim jej składnikom potomnym.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-459">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="d6dc9-460">Kaskadowe wartości i parametry również zapewniają podejście do współrzędnych składników.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-460">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="d6dc9-461">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-461">Theme example</span></span>

<span data-ttu-id="d6dc9-462">W poniższym przykładzie z przykładowej aplikacji `ThemeInfo` Klasa określa informacje o motywie, aby przetworzyć hierarchię składników w taki sposób, aby wszystkie przyciski w danej części aplikacji miały ten sam styl.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-462">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="d6dc9-463">*UIThemeClasses/themeinfo wskazuje. cs*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-463">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="d6dc9-464">Składnik nadrzędny może zapewnić kaskadową wartość przy użyciu składnika wartości kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-464">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="d6dc9-465">`CascadingValue` Składnik otacza poddrzewo hierarchii składników i dostarcza jedną wartość do wszystkich składników w tym poddrzewie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-465">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="d6dc9-466">Przykładowo aplikacja Przykładowa określa informacje o motywie`ThemeInfo`() w jednej z układów aplikacji jako parametr kaskadowy dla wszystkich składników, które tworzą treść `@Body` układu właściwości.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-466">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="d6dc9-467">`ButtonClass`ma przypisaną wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-467">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="d6dc9-468">Każdy składnik podrzędny może wykorzystać tę właściwość za pomocą `ThemeInfo` obiektu kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-468">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="d6dc9-469">`CascadingValuesParametersLayout`składnika</span><span class="sxs-lookup"><span data-stu-id="d6dc9-469">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="d6dc9-470">Aby korzystać z wartości kaskadowych, składniki deklarują kaskadowe parametry przy użyciu `[CascadingParameter]` atrybutu lub na podstawie wartości nazwy ciągu:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-470">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="d6dc9-471">Powiązanie z wartością nazwy ciągu jest istotne, jeśli istnieje wiele wartości kaskadowych tego samego typu i trzeba je odróżnić w ramach tego samego poddrzewa.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-471">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="d6dc9-472">Wartości kaskadowe są powiązane z parametrami kaskadowymi według typu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-472">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="d6dc9-473">W przykładowej aplikacji `CascadingValuesParametersTheme` składnik wiąże `ThemeInfo` wartość kaskadową z parametrem kaskadowym.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-473">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="d6dc9-474">Parametr służy do ustawiania klasy CSS dla jednego z przycisków wyświetlanych przez składnik.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-474">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="d6dc9-475">`CascadingValuesParametersTheme`składnika</span><span class="sxs-lookup"><span data-stu-id="d6dc9-475">`CascadingValuesParametersTheme` component:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="d6dc9-476">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="d6dc9-476">TabSet example</span></span>

<span data-ttu-id="d6dc9-477">Parametry kaskadowe umożliwiają również współdziałanie składników w hierarchii składników.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-477">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="d6dc9-478">Rozważmy na przykład następujący przykład *TabSet* w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-478">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="d6dc9-479">Przykładowa aplikacja ma `ITab` interfejs, który implementuje karty:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-479">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="d6dc9-480">Składnik używa składnika, który zawiera kilka `Tab` składników: `TabSet` `CascadingValuesParametersTabSet`</span><span class="sxs-lookup"><span data-stu-id="d6dc9-480">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="d6dc9-481">Składniki podrzędne `Tab` nie są jawnie przenoszone jako parametry `TabSet`do.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-481">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="d6dc9-482">Zamiast tego składniki podrzędne `Tab` są częścią zawartości `TabSet`elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-482">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="d6dc9-483">Jednak nadal musi wiedzieć o każdym `Tab` składniku, aby można było renderować nagłówki i aktywną kartę. `TabSet` Aby umożliwić tę koordynację bez konieczności stosowania dodatkowego kodu `TabSet` , składnik *może stanowić wartość kaskadową* , która następnie jest wybierana przez składniki potomne `Tab` .</span><span class="sxs-lookup"><span data-stu-id="d6dc9-483">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="d6dc9-484">`TabSet`składnika</span><span class="sxs-lookup"><span data-stu-id="d6dc9-484">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="d6dc9-485">Składniki potomne `Tab` przechwytują zawierający `TabSet` jako parametr kaskadowy, więc `Tab` składniki dodają się do `TabSet` i koordynują, na której karcie jest aktywna.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-485">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="d6dc9-486">`Tab`składnika</span><span class="sxs-lookup"><span data-stu-id="d6dc9-486">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="d6dc9-487">Szablony Razor</span><span class="sxs-lookup"><span data-stu-id="d6dc9-487">Razor templates</span></span>

<span data-ttu-id="d6dc9-488">Fragmenty renderowania można definiować przy użyciu składni szablonu Razor.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-488">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="d6dc9-489">Szablony Razor są sposobem definiowania fragmentu interfejsu użytkownika i przyjmuje następujący format:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-489">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="d6dc9-490">Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości oraz renderowania szablonów bezpośrednio w składniku.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-490">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="d6dc9-491">Fragmenty renderowania mogą być również przekazane jako argumenty do [składników](#templated-components)z szablonem.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-491">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="d6dc9-492">Renderowane dane wyjściowe poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-492">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="d6dc9-493">Ręczna logika RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="d6dc9-493">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="d6dc9-494">`Microsoft.AspNetCore.Components.RenderTree`zapewnia metody manipulowania składnikami i elementami, w tym ręczne Kompilowanie C# składników w kodzie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-494">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="d6dc9-495">Korzystanie z `RenderTreeBuilder` programu do tworzenia składników jest zaawansowanym scenariuszem.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-495">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="d6dc9-496">Nieprawidłowo sformułowany składnik (na przykład niezamknięty tag znacznika) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-496">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="d6dc9-497">Rozważmy następujący `PetDetails` składnik, który można ręcznie utworzyć w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-497">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    private string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="d6dc9-498">W poniższym przykładzie pętla w `CreateComponent` metodzie generuje trzy `PetDetails` składniki.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-498">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="d6dc9-499">Podczas wywoływania `RenderTreeBuilder` metod tworzenia składników (`OpenComponent` i `AddAttribute`) numery sekwencji są numerami wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-499">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="d6dc9-500">Algorytm Blazor różnica polega na numerach sekwencji odpowiadających odrębnym wierszom kodu, a nie odrębnym wywoływaniu wywołań.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-500">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="d6dc9-501">Podczas tworzenia składnika przy użyciu `RenderTreeBuilder` metod umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-501">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="d6dc9-502">**Użycie obliczenia lub licznika do wygenerowania numeru sekwencji może prowadzić do niskiej wydajności.**</span><span class="sxs-lookup"><span data-stu-id="d6dc9-502">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="d6dc9-503">Aby uzyskać więcej informacji, zobacz sekcję [numery sekwencji powiązane z numerami wierszy kodu i kolejnością niewykonania](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="d6dc9-503">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="d6dc9-504">`BuiltContent`składnika</span><span class="sxs-lookup"><span data-stu-id="d6dc9-504">`BuiltContent` component:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="d6dc9-505">Numery sekwencji odnoszą się do numerów wierszy kodu, a nie kolejności wykonywania</span><span class="sxs-lookup"><span data-stu-id="d6dc9-505">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="d6dc9-506">Pliki `.razor` Blazor są zawsze kompilowane.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-506">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="d6dc9-507">Jest to znakomita korzyść `.razor` , ponieważ krok kompilowania może służyć do iniekcji informacji, które zwiększają wydajność aplikacji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-507">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="d6dc9-508">Najważniejszym przykładem tych ulepszeń są *numery sekwencji*.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-508">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="d6dc9-509">Numery sekwencji wskazują na środowisko uruchomieniowe, które pochodzą z różnych i uporządkowanych wierszy kodu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-509">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="d6dc9-510">Środowisko uruchomieniowe używa tych informacji do generowania wydajnych różnic drzewa w czasie liniowym, które są znacznie szybsze niż zwykle jest to możliwe dla algorytmu różnicowego drzewa ogólnego.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-510">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="d6dc9-511">Rozważmy następujący prosty `.razor` plik:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-511">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="d6dc9-512">Poprzedni kod kompiluje się w taki sposób, aby wyglądał następująco:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-512">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="d6dc9-513">Gdy kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-513">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="d6dc9-514">Sequence</span><span class="sxs-lookup"><span data-stu-id="d6dc9-514">Sequence</span></span> | <span data-ttu-id="d6dc9-515">Typ</span><span class="sxs-lookup"><span data-stu-id="d6dc9-515">Type</span></span>      | <span data-ttu-id="d6dc9-516">Dane</span><span class="sxs-lookup"><span data-stu-id="d6dc9-516">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="d6dc9-517">0</span><span class="sxs-lookup"><span data-stu-id="d6dc9-517">0</span></span>        | <span data-ttu-id="d6dc9-518">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-518">Text node</span></span> | <span data-ttu-id="d6dc9-519">Pierwszego</span><span class="sxs-lookup"><span data-stu-id="d6dc9-519">First</span></span>  |
| <span data-ttu-id="d6dc9-520">1</span><span class="sxs-lookup"><span data-stu-id="d6dc9-520">1</span></span>        | <span data-ttu-id="d6dc9-521">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-521">Text node</span></span> | <span data-ttu-id="d6dc9-522">Sekunda</span><span class="sxs-lookup"><span data-stu-id="d6dc9-522">Second</span></span> |

<span data-ttu-id="d6dc9-523">Wyobraź sobie `someFlag` , `false`że zostanie ona przerenderowana, a znaczniki są renderowane ponownie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-523">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="d6dc9-524">Tym razem Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-524">This time, the builder receives:</span></span>

| <span data-ttu-id="d6dc9-525">Sequence</span><span class="sxs-lookup"><span data-stu-id="d6dc9-525">Sequence</span></span> | <span data-ttu-id="d6dc9-526">Typ</span><span class="sxs-lookup"><span data-stu-id="d6dc9-526">Type</span></span>       | <span data-ttu-id="d6dc9-527">Dane</span><span class="sxs-lookup"><span data-stu-id="d6dc9-527">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="d6dc9-528">1</span><span class="sxs-lookup"><span data-stu-id="d6dc9-528">1</span></span>        | <span data-ttu-id="d6dc9-529">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-529">Text node</span></span>  | <span data-ttu-id="d6dc9-530">Sekunda</span><span class="sxs-lookup"><span data-stu-id="d6dc9-530">Second</span></span> |

<span data-ttu-id="d6dc9-531">Gdy środowisko uruchomieniowe wykonuje porównanie, zobaczy, że element w sekwencji `0` został usunięty, więc generuje następujący skrypt uproszczonej *edycji*:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-531">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="d6dc9-532">Usuń pierwszy węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-532">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="d6dc9-533">Co się stało z błędami w przypadku wygenerowania numerów sekwencyjnych</span><span class="sxs-lookup"><span data-stu-id="d6dc9-533">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="d6dc9-534">Wyobraź sobie, że została zapisana następująca logika konstruktora drzewa renderowania:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-534">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="d6dc9-535">Teraz pierwsze dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-535">Now, the first output is:</span></span>

| <span data-ttu-id="d6dc9-536">Sequence</span><span class="sxs-lookup"><span data-stu-id="d6dc9-536">Sequence</span></span> | <span data-ttu-id="d6dc9-537">Typ</span><span class="sxs-lookup"><span data-stu-id="d6dc9-537">Type</span></span>      | <span data-ttu-id="d6dc9-538">Dane</span><span class="sxs-lookup"><span data-stu-id="d6dc9-538">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="d6dc9-539">0</span><span class="sxs-lookup"><span data-stu-id="d6dc9-539">0</span></span>        | <span data-ttu-id="d6dc9-540">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-540">Text node</span></span> | <span data-ttu-id="d6dc9-541">Pierwszego</span><span class="sxs-lookup"><span data-stu-id="d6dc9-541">First</span></span>  |
| <span data-ttu-id="d6dc9-542">1</span><span class="sxs-lookup"><span data-stu-id="d6dc9-542">1</span></span>        | <span data-ttu-id="d6dc9-543">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-543">Text node</span></span> | <span data-ttu-id="d6dc9-544">Sekunda</span><span class="sxs-lookup"><span data-stu-id="d6dc9-544">Second</span></span> |

<span data-ttu-id="d6dc9-545">Ten wynik jest identyczny z poprzednim przypadkiem, dlatego nie istnieją żadne negatywne problemy.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-545">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="d6dc9-546">`someFlag`znajduje `false` się na drugim renderingu, a dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-546">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="d6dc9-547">Sequence</span><span class="sxs-lookup"><span data-stu-id="d6dc9-547">Sequence</span></span> | <span data-ttu-id="d6dc9-548">Typ</span><span class="sxs-lookup"><span data-stu-id="d6dc9-548">Type</span></span>      | <span data-ttu-id="d6dc9-549">Dane</span><span class="sxs-lookup"><span data-stu-id="d6dc9-549">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="d6dc9-550">0</span><span class="sxs-lookup"><span data-stu-id="d6dc9-550">0</span></span>        | <span data-ttu-id="d6dc9-551">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="d6dc9-551">Text node</span></span> | <span data-ttu-id="d6dc9-552">Sekunda</span><span class="sxs-lookup"><span data-stu-id="d6dc9-552">Second</span></span> |

<span data-ttu-id="d6dc9-553">Tym razem algorytm diff widzi, że pojawiły się *dwie* zmiany, a algorytm generuje następujący skrypt edycji:</span><span class="sxs-lookup"><span data-stu-id="d6dc9-553">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="d6dc9-554">Zmień wartość pierwszego węzła tekstowego na `Second`.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-554">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="d6dc9-555">Usuń drugi węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-555">Remove the second text node.</span></span>

<span data-ttu-id="d6dc9-556">Generowanie numerów sekwencji utraciło wszystkie przydatne informacje o tym, gdzie znajdują się `if/else` gałęzie i pętle w oryginalnym kodzie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-556">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="d6dc9-557">Wynikiem tego jest różnica **dwa razy** , tak długo, jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-557">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="d6dc9-558">Jest to prosty przykład.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-558">This is a trivial example.</span></span> <span data-ttu-id="d6dc9-559">W bardziej realistycznych przypadkach ze złożonymi i głęboko zagnieżdżonymi strukturami, szczególnie w przypadku pętli, koszt wydajności jest bardziej poważny.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-559">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="d6dc9-560">Zamiast natychmiastowego identyfikowania, które bloki lub gałęzie pętli zostały wstawione lub usunięte, algorytm różnicowy musi reprezentować się w drzewach renderowania i zwykle tworzyć dużo dłużej edytowane skrypty, ponieważ nie są w nim poinformowani o sposobie starych i nowych struktur odnoszą się do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-560">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="d6dc9-561">Wskazówki i wnioski</span><span class="sxs-lookup"><span data-stu-id="d6dc9-561">Guidance and conclusions</span></span>

* <span data-ttu-id="d6dc9-562">Wydajność aplikacji ma wpływ na to, że numery sekwencji są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-562">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="d6dc9-563">Struktura nie może automatycznie tworzyć własnych numerów sekwencji w czasie wykonywania, ponieważ niezbędne informacje nie istnieją, chyba że są przechwytywane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-563">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="d6dc9-564">Nie zapisuj długich bloków logiki wykonywanej `RenderTreeBuilder` ręcznie.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-564">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="d6dc9-565">Preferuj `.razor` pliki i Zezwalaj kompilatorowi na rozpatruje numery sekwencji.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-565">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="d6dc9-566">Jeśli numery sekwencji są stałee, algorytm diff wymaga tylko zwiększenia wartości sekwencji.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-566">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="d6dc9-567">Początkowa wartość i przerwy są nieistotne.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-567">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="d6dc9-568">Jedną z wiarygodnych opcji jest użycie numeru wiersza kodu jako numeru sekwencyjnego lub rozpoczęcie od zera i zwiększenie według wartości lub setek (lub dowolnego preferowanego interwału).</span><span class="sxs-lookup"><span data-stu-id="d6dc9-568">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="d6dc9-569">Blazor używa numerów sekwencji, podczas gdy inne struktury interfejsu użytkownika porównujące drzewa nie są używane.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-569">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="d6dc9-570">Różnica jest znacznie szybsza, gdy są używane numery sekwencji, a Blazor ma zalety kroku kompilacji, który zajmuje się automatycznie numerami sekwencyjnymi dla deweloperów tworzących `.razor` pliki.</span><span class="sxs-lookup"><span data-stu-id="d6dc9-570">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>
