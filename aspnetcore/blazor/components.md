---
title: Tworzenie i używanie składników ASP.NET Core Razor
author: guardrex
description: Dowiedz się, jak tworzyć i używać składników Razor, w tym jak powiązać z danymi, obsługiwać zdarzenia i zarządzać cyklem życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/components
ms.openlocfilehash: 5361c506f112cbb74865c3819f0b3bd578a1705a
ms.sourcegitcommit: 38cac2552029fc19428722bb204ff9e16eb94225
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2019
ms.locfileid: "69573088"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="4908a-103">Tworzenie i używanie składników ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="4908a-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="4908a-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="4908a-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="4908a-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4908a-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4908a-106">Aplikacje Blazor są kompilowane przy użyciu *składników*programu.</span><span class="sxs-lookup"><span data-stu-id="4908a-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="4908a-107">Składnik jest niezależnym fragmentem interfejsu użytkownika (UI), takim jak strona, okno dialogowe lub formularz.</span><span class="sxs-lookup"><span data-stu-id="4908a-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="4908a-108">Składnik zawiera znaczniki HTML i logikę przetwarzania wymagane do iniekcji danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="4908a-109">Składniki są elastyczne i lekkie.</span><span class="sxs-lookup"><span data-stu-id="4908a-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="4908a-110">Mogą być zagnieżdżane, ponownie używane i udostępniane między projektami.</span><span class="sxs-lookup"><span data-stu-id="4908a-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="4908a-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="4908a-111">Component classes</span></span>

<span data-ttu-id="4908a-112">Składniki są zaimplementowane w plikach składników [Razor](xref:mvc/views/razor) ( *. Razor*) przy użyciu kombinacji C# i znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="4908a-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="4908a-113">Składnik w Blazor jest formalnie określany jako *składnik Razor*.</span><span class="sxs-lookup"><span data-stu-id="4908a-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="4908a-114">Nazwa składnika musi rozpoczynać się wielką literą.</span><span class="sxs-lookup"><span data-stu-id="4908a-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="4908a-115">Na przykład *MyCoolComponent. Razor* jest prawidłowy, a *MyCoolComponent. Razor* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="4908a-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="4908a-116">Składniki można tworzyć przy użyciu rozszerzenia pliku *. cshtml* , o ile pliki są identyfikowane jako pliki składników Razor przy użyciu `_RazorComponentInclude` właściwości MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4908a-116">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="4908a-117">Na przykład aplikacja, która określa, że wszystkie pliki *. cshtml* w folderze *Pages* powinny być traktowane jako pliki składników Razor:</span><span class="sxs-lookup"><span data-stu-id="4908a-117">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="4908a-118">Interfejs użytkownika dla składnika jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="4908a-118">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="4908a-119">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="4908a-119">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="4908a-120">Po skompilowaniu aplikacji logika znaczników HTML i C# renderowania jest konwertowana na klasę składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-120">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="4908a-121">Nazwa wygenerowanej klasy jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="4908a-121">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="4908a-122">Elementy członkowskie klasy składnika są zdefiniowane w `@code` bloku.</span><span class="sxs-lookup"><span data-stu-id="4908a-122">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="4908a-123">`@code` W bloku stan składnika (właściwości, pola) jest określany przy użyciu metod obsługi zdarzeń lub definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-123">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="4908a-124">Dozwolony jest więcej `@code` niż jeden blok.</span><span class="sxs-lookup"><span data-stu-id="4908a-124">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="4908a-125">W poprzednich wersjach ASP.NET Core 3,0 `@functions` bloki zostały użyte do tego samego celu co `@code` bloki w składnikach Razor.</span><span class="sxs-lookup"><span data-stu-id="4908a-125">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="4908a-126">`@functions`bloki kontynuują działanie w składnikach Razor, ale zalecamy używanie `@code` bloku w ASP.NET Core 3,0 wersja zapoznawcza 6 lub nowsza.</span><span class="sxs-lookup"><span data-stu-id="4908a-126">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="4908a-127">Składowe składnika mogą być używane jako część logiki renderowania składnika przy użyciu C# wyrażeń, które zaczynają `@`się od.</span><span class="sxs-lookup"><span data-stu-id="4908a-127">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="4908a-128">Na przykład C# pole jest renderowane przez utworzenie prefiksu `@` do nazwy pola.</span><span class="sxs-lookup"><span data-stu-id="4908a-128">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="4908a-129">Poniższy przykład szacuje i renderuje:</span><span class="sxs-lookup"><span data-stu-id="4908a-129">The following example evaluates and renders:</span></span>

* <span data-ttu-id="4908a-130">`_headingFontStyle`na wartość właściwości CSS dla elementu `font-style`.</span><span class="sxs-lookup"><span data-stu-id="4908a-130">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="4908a-131">`_headingText`do zawartości `<h1>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4908a-131">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="4908a-132">Po pierwszym wyrenderowaniu składnika składnik generuje jego drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="4908a-132">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="4908a-133">Blazor następnie porównuje nowe drzewo renderowania z poprzednią i zastosuje wszelkie modyfikacje Document Object Model przeglądarki (DOM).</span><span class="sxs-lookup"><span data-stu-id="4908a-133">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="4908a-134">Składniki są zwykłymi C# klasami i mogą być umieszczane w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="4908a-134">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="4908a-135">Składniki, które generują strony sieci Web, zwykle znajdują się w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="4908a-135">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="4908a-136">Składniki niestronicowe są często umieszczane w folderze udostępnionym lub w folderze niestandardowym dodanym do projektu.</span><span class="sxs-lookup"><span data-stu-id="4908a-136">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="4908a-137">Aby użyć folderu niestandardowego, należy dodać przestrzeń nazw folderu niestandardowego do składnika nadrzędnego lub do pliku *_Imports. Razor* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-137">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="4908a-138">Na przykład następująca przestrzeń nazw sprawia, że składniki w folderze Components są dostępne, gdy główna przestrzeń nazw `WebApplication`aplikacji to:</span><span class="sxs-lookup"><span data-stu-id="4908a-138">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="4908a-139">Integrowanie składników w aplikacjach Razor Pages i MVC</span><span class="sxs-lookup"><span data-stu-id="4908a-139">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="4908a-140">Używaj składników z istniejącymi aplikacjami Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="4908a-140">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="4908a-141">Nie ma potrzeby ponownego zapisywania istniejących stron lub widoków w celu używania składników Razor.</span><span class="sxs-lookup"><span data-stu-id="4908a-141">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="4908a-142">Gdy strona lub widok są renderowane, składniki są wstępnie renderowane w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="4908a-142">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="4908a-143">Aby renderować składnik ze strony lub widoku, użyj `RenderComponentAsync<TComponent>` metody pomocnika HTML:</span><span class="sxs-lookup"><span data-stu-id="4908a-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="4908a-144">Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true".</span><span class="sxs-lookup"><span data-stu-id="4908a-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="4908a-145">Składniki nie mogą używać scenariuszy dotyczących widoków i stron, takich jak częściowe widoki i sekcje.</span><span class="sxs-lookup"><span data-stu-id="4908a-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="4908a-146">Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.</span><span class="sxs-lookup"><span data-stu-id="4908a-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="4908a-147">Aby uzyskać więcej informacji na temat sposobu renderowania składników i zarządzania stanem składnika w aplikacjach po stronie serwera Blazor, zobacz <xref:blazor/hosting-models> artykuł.</span><span class="sxs-lookup"><span data-stu-id="4908a-147">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="4908a-148">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="4908a-148">Use components</span></span>

<span data-ttu-id="4908a-149">Składniki mogą zawierać inne składniki, deklarując je za pomocą składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="4908a-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="4908a-150">Znaczniki użycia składnika wyglądają jak tag HTML, gdzie nazwa znacznika jest typem składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="4908a-151">W powiązaniu atrybutu rozróżniana jest wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="4908a-151">Attribute binding is case sensitive.</span></span> <span data-ttu-id="4908a-152">Na przykład `@bind` prawidłowe i `@Bind` jest nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="4908a-152">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="4908a-153">Poniższy znacznik w *indeksie. Razor* renderuje `HeadingComponent` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="4908a-153">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="4908a-154">*Składniki/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4908a-154">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="4908a-155">Jeśli składnik zawiera element HTML z wielką literą, która nie jest zgodna z nazwą składnika, jest emitowane ostrzeżenie wskazujące, że element ma nieoczekiwaną nazwę.</span><span class="sxs-lookup"><span data-stu-id="4908a-155">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="4908a-156">`@using` Dodanie instrukcji dla przestrzeni nazw składnika sprawia, że składnik jest dostępny, co spowoduje usunięcie tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="4908a-156">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="4908a-157">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="4908a-157">Component parameters</span></span>

<span data-ttu-id="4908a-158">Składniki mogą mieć *Parametry składnika*, które są zdefiniowane przy użyciu właściwości publicznych w klasie składnika z `[Parameter]` atrybutem.</span><span class="sxs-lookup"><span data-stu-id="4908a-158">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="4908a-159">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="4908a-159">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="4908a-160">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4908a-160">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="4908a-161">W poniższym przykładzie `ParentComponent` `Title` Właściwość ustawia wartość właściwości `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="4908a-161">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="4908a-162">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4908a-162">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="4908a-163">Zawartość podrzędna</span><span class="sxs-lookup"><span data-stu-id="4908a-163">Child content</span></span>

<span data-ttu-id="4908a-164">Składniki mogą ustawiać zawartość innego składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-164">Components can set the content of another component.</span></span> <span data-ttu-id="4908a-165">Składnik Assigner zawiera zawartość między tagami, które określają składnik do odbioru.</span><span class="sxs-lookup"><span data-stu-id="4908a-165">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="4908a-166">W poniższym przykładzie `ChildComponent` `ChildContent` ma właściwość, która reprezentuje element `RenderFragment`, który reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="4908a-166">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="4908a-167">Wartość `ChildContent` jest umieszczana w znacznikach składnika, gdzie zawartość powinna być renderowana.</span><span class="sxs-lookup"><span data-stu-id="4908a-167">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="4908a-168">Wartość `ChildContent` jest odbierana ze składnika nadrzędnego i renderowany w `panel-body`panelu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="4908a-168">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="4908a-169">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4908a-169">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="4908a-170">Właściwość otrzymująca `RenderFragment` zawartość musi być nazywana `ChildContent` Konwencją.</span><span class="sxs-lookup"><span data-stu-id="4908a-170">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="4908a-171">Poniższe elementy `ParentComponent` mogą zapewnić zawartość do `ChildComponent` renderowania przez `<ChildComponent>` umieszczenie zawartości wewnątrz tagów.</span><span class="sxs-lookup"><span data-stu-id="4908a-171">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="4908a-172">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4908a-172">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="4908a-173">Korzystając atrybutów i dowolne parametry</span><span class="sxs-lookup"><span data-stu-id="4908a-173">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="4908a-174">Składniki mogą przechwytywać i renderować dodatkowe atrybuty oprócz zadeklarowanych parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-174">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="4908a-175">Dodatkowe atrybuty mogą być przechwytywane w słowniku, a następnie *splatted* do elementu, gdy składnik jest renderowany przy [@attributes](xref:mvc/views/razor#attributes) użyciu dyrektywy Razor.</span><span class="sxs-lookup"><span data-stu-id="4908a-175">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="4908a-176">Ten scenariusz jest przydatny podczas definiowania składnika, który generuje element znaczników, który obsługuje różne dostosowania.</span><span class="sxs-lookup"><span data-stu-id="4908a-176">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="4908a-177">Na przykład może być żmudnym do definiowania atrybutów oddzielnie dla `<input>` , który obsługuje wiele parametrów.</span><span class="sxs-lookup"><span data-stu-id="4908a-177">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="4908a-178">W `<input>` poniższym przykładzie pierwszy element (`id="useIndividualParams"`) używa pojedynczych parametrów składnika, podczas gdy drugi `<input>` element (`id="useAttributesDict"`) używa atrybutu korzystając:</span><span class="sxs-lookup"><span data-stu-id="4908a-178">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="4908a-179">Typ parametru musi być zaimplementowany `IEnumerable<KeyValuePair<string, object>>` przy użyciu kluczy ciągu.</span><span class="sxs-lookup"><span data-stu-id="4908a-179">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="4908a-180">Korzystanie `IReadOnlyDictionary<string, object>` z programu jest również opcją w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="4908a-180">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="4908a-181">Renderowane `<input>` elementy korzystające z obu metod są identyczne:</span><span class="sxs-lookup"><span data-stu-id="4908a-181">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="4908a-182">Aby zaakceptować dowolne atrybuty, zdefiniuj parametr składnika przy użyciu `[Parameter]` atrybutu `CaptureUnmatchedValues` z właściwością ustawioną na `true`:</span><span class="sxs-lookup"><span data-stu-id="4908a-182">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedAttributes = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="4908a-183">`CaptureUnmatchedValues` Właściwość on`[Parameter]` umożliwia dopasowanie parametru do wszystkich atrybutów, które nie są zgodne z żadnym innym parametrem.</span><span class="sxs-lookup"><span data-stu-id="4908a-183">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="4908a-184">Składnik może definiować tylko jeden parametr z `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="4908a-184">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="4908a-185">Typ właściwości używany z elementem `CaptureUnmatchedValues` musi być możliwy do przypisania `Dictionary<string, object>` z klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="4908a-185">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="4908a-186">`IEnumerable<KeyValuePair<string, object>>`lub `IReadOnlyDictionary<string, object>` są również opcje w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="4908a-186">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="4908a-187">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="4908a-187">Data binding</span></span>

<span data-ttu-id="4908a-188">Powiązanie danych zarówno ze składnikami, jak i elementami modelu dom jest [@bind](xref:mvc/views/razor#bind) realizowane przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4908a-188">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="4908a-189">Poniższy przykład wiąże `_italicsCheck` pole z zaznaczonym stanem pola wyboru:</span><span class="sxs-lookup"><span data-stu-id="4908a-189">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="4908a-190">Gdy to pole wyboru jest zaznaczone i wyczyszczone, wartość właściwości jest aktualizowana `true` odpowiednio `false`do i.</span><span class="sxs-lookup"><span data-stu-id="4908a-190">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="4908a-191">To pole wyboru jest aktualizowane w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, a nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="4908a-191">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="4908a-192">Ze względu na to, że składniki są renderowane po wykonaniu kodu procedury obsługi zdarzeń, aktualizacje właściwości są zwykle odzwierciedlane natychmiast w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-192">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="4908a-193">Używanie `@bind` `<input @bind="CurrentValue" />`z właściwością () jest zasadniczo równoważne z następującymi: `CurrentValue`</span><span class="sxs-lookup"><span data-stu-id="4908a-193">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="4908a-194">Gdy składnik jest renderowany, `value` element wejściowy pochodzi `CurrentValue` z właściwości.</span><span class="sxs-lookup"><span data-stu-id="4908a-194">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="4908a-195">Gdy użytkownik wpisze w polu tekstowym, `onchange` zdarzenie jest wyzwalane, `CurrentValue` a właściwość jest ustawiona na wartość zmieniona.</span><span class="sxs-lookup"><span data-stu-id="4908a-195">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="4908a-196">W rzeczywistości generowanie kodu jest nieco bardziej skomplikowane, ponieważ `@bind` obsługuje kilka przypadków, w których są wykonywane konwersje typów.</span><span class="sxs-lookup"><span data-stu-id="4908a-196">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="4908a-197">W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia `value` z atrybutem i obsługuje zmiany przy użyciu zarejestrowanej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="4908a-197">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="4908a-198">`onchange` Oprócz obsługi[@bind-value:event](xref:mvc/views/razor#bind)zdarzeń ze `@bind` składnią właściwość lub pole można powiązać przy użyciu [@bind-value](xref:mvc/views/razor#bind) innych zdarzeń, `event` określając atrybut z parametrem ().</span><span class="sxs-lookup"><span data-stu-id="4908a-198">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="4908a-199">Poniższy przykład wiąże się z `CurrentValue` właściwością `oninput` zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="4908a-199">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="4908a-200">W przeciwieństwie do `onchange`, które jest wyzwalane, gdy `oninput` element utraci fokus, jest uruchamiany po zmianie wartości pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="4908a-200">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="4908a-201">**Globalizacja**</span><span class="sxs-lookup"><span data-stu-id="4908a-201">**Globalization**</span></span>

<span data-ttu-id="4908a-202">`@bind`wartości są sformatowane do wyświetlania i analizowane przy użyciu reguł bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="4908a-202">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="4908a-203">Dla bieżącej kultury można uzyskać dostęp z <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> właściwości.</span><span class="sxs-lookup"><span data-stu-id="4908a-203">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="4908a-204">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) jest używany dla następujących typów pól (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="4908a-204">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="4908a-205">Poprzednie typy pól:</span><span class="sxs-lookup"><span data-stu-id="4908a-205">The preceding field types:</span></span>

* <span data-ttu-id="4908a-206">Są wyświetlane przy użyciu odpowiednich reguł formatowania opartych na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="4908a-206">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="4908a-207">Nie może zawierać tekstu o dowolnym formacie.</span><span class="sxs-lookup"><span data-stu-id="4908a-207">Can't contain free-form text.</span></span>
* <span data-ttu-id="4908a-208">Podaj charakterystykę interakcji użytkownika w oparciu o implementację przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4908a-208">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="4908a-209">Następujące typy pól mają określone wymagania dotyczące formatowania i nie są obecnie obsługiwane przez Blazor, ponieważ nie są obsługiwane przez wszystkie główne przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="4908a-209">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="4908a-210">`@bind`obsługuje parametr, aby zapewnić analizę i formatowanie wartości. <xref:System.Globalization.CultureInfo?displayProperty=fullName> `@bind:culture`</span><span class="sxs-lookup"><span data-stu-id="4908a-210">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="4908a-211">Określanie kultury nie jest zalecane w `date` przypadku używania typów pól i. `number`</span><span class="sxs-lookup"><span data-stu-id="4908a-211">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="4908a-212">`date`i `number` ma wbudowaną obsługę Blazor, która dostarcza wymaganą kulturę.</span><span class="sxs-lookup"><span data-stu-id="4908a-212">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="4908a-213">Aby uzyskać informacje na temat sposobu ustawiania kultury użytkownika, zobacz sekcję [Lokalizacja](#localization) .</span><span class="sxs-lookup"><span data-stu-id="4908a-213">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="4908a-214">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="4908a-214">**Format strings**</span></span>

<span data-ttu-id="4908a-215">Powiązanie danych działa z <xref:System.DateTime> ciągami formatu [@bind:format](xref:mvc/views/razor#bind)przy użyciu.</span><span class="sxs-lookup"><span data-stu-id="4908a-215">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="4908a-216">W tej chwili nie są dostępne inne wyrażenia formatu, takie jak formaty walutowe lub liczbowe.</span><span class="sxs-lookup"><span data-stu-id="4908a-216">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="4908a-217">W poprzednim kodzie `<input>` typ pola (`type`) elementu jest wartością domyślną `text`.</span><span class="sxs-lookup"><span data-stu-id="4908a-217">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="4908a-218">`@bind:format`jest obsługiwana w celu powiązania następujących typów .NET:</span><span class="sxs-lookup"><span data-stu-id="4908a-218">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="4908a-219"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="4908a-219"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="4908a-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="4908a-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="4908a-221">Ten `@bind:format` atrybut określa format daty, który ma zostać zastosowany `value` do `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4908a-221">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="4908a-222">Format jest również używany do analizowania wartości w przypadku `onchange` wystąpienia zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="4908a-222">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="4908a-223">Określanie formatu dla `date` typu pola nie jest zalecane, ponieważ Blazor ma wbudowaną obsługę formatowania dat.</span><span class="sxs-lookup"><span data-stu-id="4908a-223">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="4908a-224">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="4908a-224">**Component parameters**</span></span>

<span data-ttu-id="4908a-225">Powiązanie rozpoznaje parametry składnika, gdzie `@bind-{property}` można powiązać wartość właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="4908a-225">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="4908a-226">Następujący składnik podrzędny (`ChildComponent`) `Year` ma parametr składnika i `YearChanged` wywołanie zwrotne:</span><span class="sxs-lookup"><span data-stu-id="4908a-226">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="4908a-227">`EventCallback<T>`wyjaśniono w sekcji [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="4908a-227">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="4908a-228">Poniższy składnik nadrzędny używa `ChildComponent` i wiąże `ParentYear` parametr `Year` z elementu nadrzędnego z parametrem w składniku podrzędnym:</span><span class="sxs-lookup"><span data-stu-id="4908a-228">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="4908a-229">Załadowanie `ParentComponent` powoduje utworzenie następującej adjustacji:</span><span class="sxs-lookup"><span data-stu-id="4908a-229">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="4908a-230">Jeśli wartość `ParentYear` właściwości zostanie zmieniona przez wybranie przycisku `ParentComponent`w `ChildComponent` , `Year` właściwość jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="4908a-230">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="4908a-231">Nowa wartość `Year` jest renderowana w interfejsie użytkownika podczas jego `ParentComponent` renderowania:</span><span class="sxs-lookup"><span data-stu-id="4908a-231">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="4908a-232">Parametr jest możliwy do powiązania, ponieważ ma zdarzenie towarzyszące `YearChanged` `Year` pasujące do typu parametru. `Year`</span><span class="sxs-lookup"><span data-stu-id="4908a-232">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="4908a-233">Zgodnie z Konwencją, `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo równoważne zapisowi:</span><span class="sxs-lookup"><span data-stu-id="4908a-233">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="4908a-234">Ogólnie rzecz biorąc, właściwość może być powiązana z odpowiednią obsługą zdarzeń `@bind-property:event` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4908a-234">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="4908a-235">Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` użyciem następujących dwóch atrybutów:</span><span class="sxs-lookup"><span data-stu-id="4908a-235">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="4908a-236">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="4908a-236">Event handling</span></span>

<span data-ttu-id="4908a-237">Składniki Razor zapewniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4908a-237">Razor components provide event handling features.</span></span> <span data-ttu-id="4908a-238">Dla atrybutu elementu HTML o nazwie `on{event}` (na `onclick` przykład i `onsubmit`) z wartością typu delegata składniki Razor traktują wartość atrybutu jako procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4908a-238">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="4908a-239">Nazwa atrybutu jest zawsze sformatowana [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="4908a-239">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="4908a-240">Poniższy kod wywołuje metodę, `UpdateHeading` gdy przycisk zostanie wybrany w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="4908a-240">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="4908a-241">Poniższy kod wywołuje metodę, `CheckChanged` gdy pole wyboru zostanie zmienione w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="4908a-241">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="4908a-242">Procedury obsługi zdarzeń mogą również być asynchroniczne i zwracać <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="4908a-242">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="4908a-243">Nie ma potrzeby ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="4908a-243">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="4908a-244">Wyjątki są rejestrowane, gdy wystąpią.</span><span class="sxs-lookup"><span data-stu-id="4908a-244">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="4908a-245">W poniższym przykładzie `UpdateHeading` jest wywoływana asynchronicznie po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="4908a-245">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="4908a-246">Typy argumentów zdarzeń</span><span class="sxs-lookup"><span data-stu-id="4908a-246">Event argument types</span></span>

<span data-ttu-id="4908a-247">W przypadku niektórych zdarzeń dozwolone są typy argumentów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4908a-247">For some events, event argument types are permitted.</span></span> <span data-ttu-id="4908a-248">Jeśli dostęp do jednego z tych typów zdarzeń nie jest konieczny, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="4908a-248">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="4908a-249">Obsługiwane [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) są przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="4908a-249">Supported [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) are shown in the following table.</span></span>

| <span data-ttu-id="4908a-250">Zdarzenie</span><span class="sxs-lookup"><span data-stu-id="4908a-250">Event</span></span> | <span data-ttu-id="4908a-251">Class</span><span class="sxs-lookup"><span data-stu-id="4908a-251">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="4908a-252">Schowek</span><span class="sxs-lookup"><span data-stu-id="4908a-252">Clipboard</span></span> | `UIClipboardEventArgs` |
| <span data-ttu-id="4908a-253">Przeciągnij</span><span class="sxs-lookup"><span data-stu-id="4908a-253">Drag</span></span>  | <span data-ttu-id="4908a-254">`UIDragEventArgs`służy do przechowywania przeciąganych danych podczas operacji przeciągania i upuszczania oraz może zawierać jeden lub więcej `UIDataTransferItem`. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="4908a-254">`UIDragEventArgs` &ndash; `DataTransfer` is used to hold the dragged data during a drag and drop operation and may hold one or more `UIDataTransferItem`.</span></span> <span data-ttu-id="4908a-255">`UIDataTransferItem`reprezentuje jeden element danych przeciągania.</span><span class="sxs-lookup"><span data-stu-id="4908a-255">`UIDataTransferItem` represents one drag data item.</span></span> |
| <span data-ttu-id="4908a-256">Błąd</span><span class="sxs-lookup"><span data-stu-id="4908a-256">Error</span></span> | `UIErrorEventArgs` |
| <span data-ttu-id="4908a-257">Fokus</span><span class="sxs-lookup"><span data-stu-id="4908a-257">Focus</span></span> | <span data-ttu-id="4908a-258">`UIFocusEventArgs`Nie obejmuje obsługi dla `relatedTarget`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="4908a-258">`UIFocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="4908a-259">`<input>`stąp</span><span class="sxs-lookup"><span data-stu-id="4908a-259">`<input>` change</span></span> | `UIChangeEventArgs` |
| <span data-ttu-id="4908a-260">Klawiatura</span><span class="sxs-lookup"><span data-stu-id="4908a-260">Keyboard</span></span> | `UIKeyboardEventArgs` |
| <span data-ttu-id="4908a-261">Wskaźnik</span><span class="sxs-lookup"><span data-stu-id="4908a-261">Mouse</span></span> | `UIMouseEventArgs` |
| <span data-ttu-id="4908a-262">Wskaźnik myszy</span><span class="sxs-lookup"><span data-stu-id="4908a-262">Mouse pointer</span></span> | `UIPointerEventArgs` |
| <span data-ttu-id="4908a-263">Kółko myszy</span><span class="sxs-lookup"><span data-stu-id="4908a-263">Mouse wheel</span></span> | `UIWheelEventArgs` |
| <span data-ttu-id="4908a-264">Postęp</span><span class="sxs-lookup"><span data-stu-id="4908a-264">Progress</span></span> | `UIProgressEventArgs` |
| <span data-ttu-id="4908a-265">Dotyk</span><span class="sxs-lookup"><span data-stu-id="4908a-265">Touch</span></span> | <span data-ttu-id="4908a-266">`UITouchEventArgs`&ndash; reprezentujepojedynczypunktkontaktunaurządzeniuz`UITouchPoint` wrażliwym dotknięciem.</span><span class="sxs-lookup"><span data-stu-id="4908a-266">`UITouchEventArgs` &ndash; `UITouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="4908a-267">Aby uzyskać informacje o zachowaniu właściwości i obsłudze zdarzeń zdarzeń w powyższej tabeli, zobacz [klasy EventArgs w źródle odwołania](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span><span class="sxs-lookup"><span data-stu-id="4908a-267">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="4908a-268">Wyrażenia lambda</span><span class="sxs-lookup"><span data-stu-id="4908a-268">Lambda expressions</span></span>

<span data-ttu-id="4908a-269">Wyrażenia lambda mogą być również używane:</span><span class="sxs-lookup"><span data-stu-id="4908a-269">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="4908a-270">Często wygodnie jest blisko dodatkowych wartości, na przykład podczas iteracji na zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="4908a-270">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="4908a-271">Poniższy przykład tworzy trzy przyciski, z których każdy wywołuje `UpdateHeading` przekazanie argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`), po wybraniu w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="4908a-271">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="4908a-272">**Nie** używaj zmiennej Loop (`i`) w `for` pętli bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="4908a-272">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="4908a-273">W przeciwnym razie ta sama zmienna jest używana przez wszystkie wyrażenia `i`lambda, co sprawia, że wartość jest taka sama we wszystkich lambdach.</span><span class="sxs-lookup"><span data-stu-id="4908a-273">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="4908a-274">Zawsze Przechwytuj wartość w zmiennej lokalnej (`buttonNumber` w poprzednim przykładzie), a następnie użyj jej.</span><span class="sxs-lookup"><span data-stu-id="4908a-274">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="4908a-275">EventCallback</span><span class="sxs-lookup"><span data-stu-id="4908a-275">EventCallback</span></span>

<span data-ttu-id="4908a-276">Typowym scenariuszem ze składnikami zagnieżdżonymi jest uruchomienie metody składnika nadrzędnego, gdy występuje&mdash;zdarzenie składnika podrzędnego, gdy wystąpi zdarzenie w elemencie `onclick` podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="4908a-276">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="4908a-277">Aby uwidocznić zdarzenia między składnikami, `EventCallback`Użyj.</span><span class="sxs-lookup"><span data-stu-id="4908a-277">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="4908a-278">Składnik nadrzędny może przypisać metodę wywołania zwrotnego do składnika `EventCallback`podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4908a-278">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="4908a-279">W przykładowej aplikacji pokazano, jak `onclick` program obsługi przycisku został `EventCallback` skonfigurowany tak, aby otrzymać delegata z przykładu `ParentComponent`. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="4908a-279">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="4908a-280">Typ ma wartość, która jest odpowiednia dla `onclick` zdarzenia z urządzenia peryferyjnego: `UIMouseEventArgs` `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="4908a-280">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="4908a-281">Ustawia element podrzędny `ShowMessage`dojegometody: `EventCallback<T>` `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="4908a-281">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="4908a-282">Gdy przycisk zostanie wybrany w `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="4908a-282">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="4908a-283">`ParentComponent` Metoda`ShowMessage` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="4908a-283">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="4908a-284">`messageText`jest aktualizowany i wyświetlany w `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="4908a-284">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="4908a-285">Wywołanie `StateHasChanged` nie jest wymagane w metodzie wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="4908a-285">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="4908a-286">`StateHasChanged`jest automatycznie wywoływana w celu odrenderowania `ParentComponent`elementu, podobnie jak zdarzenia podrzędne wyzwala ponowne renderowanie składnika w obsłudze zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4908a-286">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="4908a-287">`EventCallback`i `EventCallback<T>` Zezwalaj na asynchroniczne Delegaty.</span><span class="sxs-lookup"><span data-stu-id="4908a-287">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="4908a-288">`EventCallback<T>`jest silnie wpisana i wymaga określonego typu argumentu.</span><span class="sxs-lookup"><span data-stu-id="4908a-288">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="4908a-289">`EventCallback`jest słabo wpisywany i zezwala na dowolny typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="4908a-289">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="4908a-290">`EventCallback<T>` Wywołaj `EventCallback` lub z`InvokeAsync` i await <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="4908a-290">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="4908a-291">Używaj `EventCallback` i`EventCallback<T>` dla parametrów składnika Obsługa zdarzeń i powiązania.</span><span class="sxs-lookup"><span data-stu-id="4908a-291">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="4908a-292">Preferuj silnie wpisaną `EventCallback<T>` `EventCallback`wartość.</span><span class="sxs-lookup"><span data-stu-id="4908a-292">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="4908a-293">`EventCallback<T>`zapewnia lepszą opinię o błędach dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-293">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="4908a-294">Podobnie jak w przypadku innych programów obsługi zdarzeń interfejsu użytkownika, określenie parametru zdarzenia jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="4908a-294">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="4908a-295">Użyj `EventCallback` w przypadku braku wartości przekazywania do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="4908a-295">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="4908a-296">Przechwyć odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="4908a-296">Capture references to components</span></span>

<span data-ttu-id="4908a-297">Odwołania do składników zapewniają sposób odwoływania się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia, takie `Show` jak `Reset`lub.</span><span class="sxs-lookup"><span data-stu-id="4908a-297">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="4908a-298">Aby przechwycić odwołanie do składnika:</span><span class="sxs-lookup"><span data-stu-id="4908a-298">To capture a component reference:</span></span>

* <span data-ttu-id="4908a-299">[@ref](xref:mvc/views/razor#ref) Dodaj atrybut do składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4908a-299">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="4908a-300">Zdefiniuj pole z tym samym typem co składnik podrzędny.</span><span class="sxs-lookup"><span data-stu-id="4908a-300">Define a field with the same type as the child component.</span></span>
* <span data-ttu-id="4908a-301">`@ref:suppressField` Podaj parametr, który pomija generowanie pól zapasowych.</span><span class="sxs-lookup"><span data-stu-id="4908a-301">Provide the `@ref:suppressField` parameter, which suppresses backing field generation.</span></span> <span data-ttu-id="4908a-302">Aby uzyskać więcej informacji, zobacz [usuwanie automatycznej obsługi pola do @ref tworzenia kopii zapasowych w programie 3.0.0-preview9](https://github.com/aspnet/Announcements/issues/381).</span><span class="sxs-lookup"><span data-stu-id="4908a-302">For more information, see [Removing automatic backing field support for @ref in 3.0.0-preview9](https://github.com/aspnet/Announcements/issues/381).</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" @ref:suppressField ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="4908a-303">Gdy składnik jest renderowany, `loginDialog` pole zostanie wypełnione `MyLoginDialog` wystąpieniem składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4908a-303">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="4908a-304">Następnie można wywołać metody .NET w wystąpieniu składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-304">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4908a-305">Zmienna jest wypełniana tylko po wyrenderowaniu składnika, a jego wyjście `MyLoginDialog` zawiera element. `loginDialog`</span><span class="sxs-lookup"><span data-stu-id="4908a-305">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="4908a-306">Do tego momentu nie ma niczego do odwołania.</span><span class="sxs-lookup"><span data-stu-id="4908a-306">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="4908a-307">Aby manipulować odwołaniami do składników po zakończeniu renderowania składnika, użyj `OnAfterRenderAsync` metod lub. `OnAfterRender`</span><span class="sxs-lookup"><span data-stu-id="4908a-307">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<!-- HOLD https://github.com/aspnet/AspNetCore.Docs/pull/13818
Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.

The Razor compiler automatically generates a backing field for element and component references when using [@ref](xref:mvc/views/razor#ref). In the following example, there's no need to create a `myLoginDialog` field for the `LoginDialog` component:

```cshtml
<LoginDialog @ref="myLoginDialog" ... />

@code {
    private void OnSomething()
    {
        myLoginDialog.Show();
    }
}
```

When the component is rendered, the generated `myLoginDialog` field is populated with the `LoginDialog` component instance. You can then invoke .NET methods on the component instance.

In some cases, a backing field is required. For example, declare a backing field when referencing generic components. To suppress backing field generation, specify the `@ref:suppressField` parameter.

> [!IMPORTANT]
> The generated `myLoginDialog` variable is only populated after the component is rendered and its output includes the `LoginDialog` element. Until that point, there's nothing to reference. To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.
-->

<span data-ttu-id="4908a-308">Podczas przechwytywania odwołań do składników użycie podobnej składni do [przechwytywania odwołań do elementów](xref:blazor/javascript-interop#capture-references-to-elements)nie jest funkcją międzyoperacyjności [języka JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="4908a-308">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="4908a-309">Odwołania do składników nie są przenoszone&mdash;do kodu JavaScript, są używane tylko w kodzie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4908a-309">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="4908a-310">**Nie** należy używać odwołań do składników do mutacji stanu składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="4908a-310">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="4908a-311">Zamiast tego należy używać zwykłych parametrów deklaratywnych do przekazywania danych do składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="4908a-311">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="4908a-312">Użycie normalnych parametrów deklaratywnych powoduje, że składniki podrzędne, które automatycznie uruchamiają się w prawidłowym czasie.</span><span class="sxs-lookup"><span data-stu-id="4908a-312">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="4908a-313">Użyj \@klawisza, aby kontrolować zachowywanie elementów i składników</span><span class="sxs-lookup"><span data-stu-id="4908a-313">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="4908a-314">Podczas renderowania listy elementów lub składników oraz elementów lub składników, które następnie zmieniają się, algorytm diff Blazor musi zdecydować, które z poprzednich elementów lub składników mogą być zachowywane i jak obiekty modelu powinny być mapowane na nie.</span><span class="sxs-lookup"><span data-stu-id="4908a-314">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="4908a-315">Zwykle ten proces jest automatyczny i można go zignorować, ale istnieją przypadki, w których może być konieczne sterowanie procesem.</span><span class="sxs-lookup"><span data-stu-id="4908a-315">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="4908a-316">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="4908a-316">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="4908a-317">Zawartość `People` kolekcji może ulec zmianie z wstawionymi, usuniętymi lub z ponownymi zamówieniami.</span><span class="sxs-lookup"><span data-stu-id="4908a-317">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="4908a-318">Gdy składnik jest przerenderowany, `<DetailsEditor>` składnik może ulec zmianie, aby otrzymywać różne `Details` wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="4908a-318">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="4908a-319">Może to spowodować bardziej złożone odwzorowanie niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="4908a-319">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="4908a-320">W niektórych przypadkach odzyskanie może prowadzić do zauważalnych różnic w zachowaniu, takich jak brak fokusu elementu.</span><span class="sxs-lookup"><span data-stu-id="4908a-320">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="4908a-321">Proces mapowania można kontrolować przy użyciu `@key` atrybutu dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="4908a-321">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="4908a-322">`@key`powoduje, że algorytm różnicowego gwarantuje zachowywanie elementów lub składników na podstawie wartości klucza:</span><span class="sxs-lookup"><span data-stu-id="4908a-322">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="4908a-323">`<DetailsEditor>` `person` Po zmianie `People` kolekcji algorytm różnicowy zachowuje skojarzenie między wystąpieniami i wystąpieniami:</span><span class="sxs-lookup"><span data-stu-id="4908a-323">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="4908a-324">Jeśli element `Person` zostanie usunięty `People` z listy, tylko odpowiednie `<DetailsEditor>` wystąpienie zostanie usunięte z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-324">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="4908a-325">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="4908a-325">Other instances are left unchanged.</span></span>
* <span data-ttu-id="4908a-326">Jeśli na liście `<DetailsEditor>` zostaniewstawionywpewnymmiejscu,jednonowewystąpieniezostaniewstawionew`Person` odpowiednim położeniu.</span><span class="sxs-lookup"><span data-stu-id="4908a-326">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="4908a-327">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="4908a-327">Other instances are left unchanged.</span></span>
* <span data-ttu-id="4908a-328">W `Person` przypadku ponownego uporządkowania wpisów odpowiednie `<DetailsEditor>` wystąpienia są zachowywane i uporządkowane w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-328">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="4908a-329">W niektórych scenariuszach użycie programu `@key` minimalizuje złożoność operacji renderowania i pozwala uniknąć potencjalnych problemów związanych ze stanem częściowych elementów modelu dom, takich jak pozycja fokusu.</span><span class="sxs-lookup"><span data-stu-id="4908a-329">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4908a-330">Klucze są lokalne dla każdego elementu kontenera lub składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-330">Keys are local to each container element or component.</span></span> <span data-ttu-id="4908a-331">Klucze nie są porównywane globalnie w całym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="4908a-331">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="4908a-332">Kiedy używać \@klucza</span><span class="sxs-lookup"><span data-stu-id="4908a-332">When to use \@key</span></span>

<span data-ttu-id="4908a-333">Zazwyczaj warto używać `@key` zawsze, gdy lista jest renderowana (na przykład `@foreach` w bloku) i odpowiednia `@key`wartość istnieje do zdefiniowania.</span><span class="sxs-lookup"><span data-stu-id="4908a-333">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="4908a-334">Można również użyć `@key` , aby uniemożliwić Blazor z zachowaniem poddrzewa elementu lub składnika, gdy zmieniany jest obiekt:</span><span class="sxs-lookup"><span data-stu-id="4908a-334">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="4908a-335">Jeśli `@currentPerson` zmiany `<div>` , dyrektywa Blazor wymusza odrzucanie całości i jego obiektów podrzędnych oraz ponowne skompilowanie poddrzewa w interfejsie użytkownika za pomocą nowych elementów i składników. `@key`</span><span class="sxs-lookup"><span data-stu-id="4908a-335">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="4908a-336">Może to być przydatne, jeśli zachodzi konieczność zagwarantowania, że stan `@currentPerson` interfejsu użytkownika nie jest zachowywany w przypadku zmiany.</span><span class="sxs-lookup"><span data-stu-id="4908a-336">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="4908a-337">Kiedy nie używać \@klucza</span><span class="sxs-lookup"><span data-stu-id="4908a-337">When not to use \@key</span></span>

<span data-ttu-id="4908a-338">W przypadku różnicowania w programie `@key`występuje koszt wydajności.</span><span class="sxs-lookup"><span data-stu-id="4908a-338">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="4908a-339">Koszt wydajności nie jest duży, ale określa `@key` tylko, czy kontrolowanie reguł utrwalania elementu lub składnika przynosi korzyści dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-339">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="4908a-340">Nawet jeśli `@key` nie jest używany, Blazor zachowuje elementy podrzędne i wystąpienia składników tak dużo, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="4908a-340">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="4908a-341">Jedyną zaletą korzystania z `@key` programu jest kontrola nad sposobem, w *jaki* wystąpienia modelu są mapowane na zachowane wystąpienia składników, zamiast algorytmu różnicowego, wybierając mapowanie.</span><span class="sxs-lookup"><span data-stu-id="4908a-341">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="4908a-342">Wartości, które mają być \@używane dla klucza</span><span class="sxs-lookup"><span data-stu-id="4908a-342">What values to use for \@key</span></span>

<span data-ttu-id="4908a-343">Ogólnie rzecz biorąc, warto podać jeden z następujących rodzajów wartości dla `@key`:</span><span class="sxs-lookup"><span data-stu-id="4908a-343">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="4908a-344">Wystąpienia obiektów modelu (na przykład `Person` wystąpienie takie jak w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="4908a-344">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="4908a-345">Zapewnia to zachowywanie na podstawie równości odwołań do obiektów.</span><span class="sxs-lookup"><span data-stu-id="4908a-345">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="4908a-346">Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`lub `Guid`).</span><span class="sxs-lookup"><span data-stu-id="4908a-346">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="4908a-347">Upewnij się, że wartości `@key` używane do nie kolidują.</span><span class="sxs-lookup"><span data-stu-id="4908a-347">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="4908a-348">Jeśli w tym samym elemencie nadrzędnym zostaną wykryte wartości powodujące konflikt, Blazor zgłasza wyjątek, ponieważ nie może on w sposób jednoznaczny mapować starych elementów lub składników na nowe elementy lub składniki.</span><span class="sxs-lookup"><span data-stu-id="4908a-348">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="4908a-349">Używaj tylko odrębnych wartości, takich jak wystąpienia obiektów lub wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="4908a-349">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="4908a-350">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="4908a-350">Lifecycle methods</span></span>

<span data-ttu-id="4908a-351">`OnInitializedAsync`i `OnInitialized` wykonaj kod w celu zainicjowania składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-351">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="4908a-352">Aby wykonać operację asynchroniczną, użyj `OnInitializedAsync` `await` słowa kluczowego i dla operacji:</span><span class="sxs-lookup"><span data-stu-id="4908a-352">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="4908a-353">W przypadku operacji synchronicznych należy `OnInitialized`użyć:</span><span class="sxs-lookup"><span data-stu-id="4908a-353">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="4908a-354">`OnParametersSetAsync`i `OnParametersSet` są wywoływane, gdy składnik otrzymał parametry od jego elementu nadrzędnego i wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="4908a-354">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="4908a-355">Te metody są wykonywane po zainicjowaniu składnika i za każdym razem, gdy składnik jest renderowany:</span><span class="sxs-lookup"><span data-stu-id="4908a-355">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="4908a-356">`OnAfterRenderAsync`i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-356">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="4908a-357">Odwołania do elementów i składników są wypełniane w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="4908a-357">Element and component references are populated at this point.</span></span> <span data-ttu-id="4908a-358">Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.</span><span class="sxs-lookup"><span data-stu-id="4908a-358">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="4908a-359">Obsługuj niekompletne akcje asynchroniczne podczas renderowania</span><span class="sxs-lookup"><span data-stu-id="4908a-359">Handle incomplete async actions at render</span></span>

<span data-ttu-id="4908a-360">Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogą nie zostać zakończone przed renderowaniem składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-360">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="4908a-361">Obiekty mogą być `null` lub uzupełniane wypełniane danymi podczas wykonywania metody cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="4908a-361">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="4908a-362">Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane.</span><span class="sxs-lookup"><span data-stu-id="4908a-362">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="4908a-363">Renderuj zastępcze elementy interfejsu użytkownika (na przykład komunikat ładowania), gdy obiekty `null`są.</span><span class="sxs-lookup"><span data-stu-id="4908a-363">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="4908a-364">W składniku `OnInitializedAsync` szablonów Blazor został zastąpiony do asynchronicznie odbierania danych prognozy (`forecasts`). `FetchData`</span><span class="sxs-lookup"><span data-stu-id="4908a-364">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="4908a-365">Gdy `forecasts` tak `null`jest, zostanie wyświetlony komunikat ładowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-365">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="4908a-366">Po zakończeniu `OnInitializedAsync` zwracany przez program składnik zostanie przerenderowany ze zaktualizowanym stanem. `Task`</span><span class="sxs-lookup"><span data-stu-id="4908a-366">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="4908a-367">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4908a-367">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="4908a-368">Wykonaj kod przed ustawieniem parametrów</span><span class="sxs-lookup"><span data-stu-id="4908a-368">Execute code before parameters are set</span></span>

<span data-ttu-id="4908a-369">`SetParameters`można zastąpić, aby wykonać kod przed ustawieniem parametrów:</span><span class="sxs-lookup"><span data-stu-id="4908a-369">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="4908a-370">Jeśli `base.SetParameters` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="4908a-370">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="4908a-371">Na przykład parametry przychodzące nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="4908a-371">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="4908a-372">Pomiń odświeżanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="4908a-372">Suppress refreshing of the UI</span></span>

<span data-ttu-id="4908a-373">`ShouldRender`można zastąpić, aby pominąć odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-373">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="4908a-374">Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony.</span><span class="sxs-lookup"><span data-stu-id="4908a-374">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="4908a-375">Nawet jeśli `ShouldRender` jest zastępowany, składnik jest zawsze początkowo renderowany.</span><span class="sxs-lookup"><span data-stu-id="4908a-375">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="4908a-376">Usuwanie składnika z interfejsem IDisposable</span><span class="sxs-lookup"><span data-stu-id="4908a-376">Component disposal with IDisposable</span></span>

<span data-ttu-id="4908a-377">W przypadku zaimplementowania <xref:System.IDisposable>składnika [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-377">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="4908a-378">Poniższy składnik używa `@implements IDisposable` `Dispose` i metody:</span><span class="sxs-lookup"><span data-stu-id="4908a-378">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="4908a-379">Routing</span><span class="sxs-lookup"><span data-stu-id="4908a-379">Routing</span></span>

<span data-ttu-id="4908a-380">Routing w Blazor jest realizowany przez dostarczenie szablonu trasy do każdego dostępnego składnika w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-380">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="4908a-381">Gdy plik Razor z `@page` dyrektywą jest kompilowany, wygenerowana Klasa ma <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określony szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="4908a-381">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="4908a-382">W czasie wykonywania router szuka klas składników za pomocą `RouteAttribute` i renderuje, w zależności od tego, który składnik ma szablon trasy zgodny z żądanym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="4908a-382">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="4908a-383">Do składnika można zastosować wiele szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="4908a-383">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="4908a-384">Poniższy składnik odpowiada na żądania dla `/BlazorRoute` i: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="4908a-384">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="4908a-385">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="4908a-385">Route parameters</span></span>

<span data-ttu-id="4908a-386">Składniki mogą odbierać parametry trasy z szablonu trasy dostarczonego w `@page` dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="4908a-386">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="4908a-387">Router używa parametrów trasy, aby wypełnić odpowiednie parametry składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-387">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="4908a-388">*Składnik parametru trasy*:</span><span class="sxs-lookup"><span data-stu-id="4908a-388">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="4908a-389">Parametry opcjonalne nie są obsługiwane, więc `@page` dwie dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="4908a-389">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="4908a-390">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="4908a-390">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="4908a-391">Druga `@page` dyrektywa `Text` przyjmuje parametr Route i przypisuje wartość do właściwości. `{text}`</span><span class="sxs-lookup"><span data-stu-id="4908a-391">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="4908a-392">Dziedziczenie klasy podstawowej dla środowiska "powiązanego z kodem"</span><span class="sxs-lookup"><span data-stu-id="4908a-392">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="4908a-393">Pliki składników mieszają znaczniki HTML C# i przetwarzają kod w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="4908a-393">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="4908a-394">`@inherits` Dyrektywa może służyć do udostępniania aplikacji Blazor z interfejsem "związanym z kodem", który oddziela oznakowanie składników od przetwarzania kodu.</span><span class="sxs-lookup"><span data-stu-id="4908a-394">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="4908a-395">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasę bazową `BlazorRocksBase`,, w celu zapewnienia właściwości i metod składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-395">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="4908a-396">*Strony/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4908a-396">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="4908a-397">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="4908a-397">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="4908a-398">Klasa bazowa powinna pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="4908a-398">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="4908a-399">Importuj składniki</span><span class="sxs-lookup"><span data-stu-id="4908a-399">Import components</span></span>

<span data-ttu-id="4908a-400">Przestrzeń nazw składnika utworzone przy użyciu Razor jest oparta na:</span><span class="sxs-lookup"><span data-stu-id="4908a-400">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="4908a-401">Projekt `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="4908a-401">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="4908a-402">Ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-402">The path from the project root to the component.</span></span> <span data-ttu-id="4908a-403">Na przykład, `ComponentsSample/Pages/Index.razor` znajduje się w przestrzeni `ComponentsSample.Pages`nazw.</span><span class="sxs-lookup"><span data-stu-id="4908a-403">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="4908a-404">Składniki przestrzegają C# reguł powiązań nazw.</span><span class="sxs-lookup"><span data-stu-id="4908a-404">Components follow C# name binding rules.</span></span> <span data-ttu-id="4908a-405">W przypadku elementu *index. Razor*wszystkie składniki znajdujące się w tym samym folderze, *stronach*i folderze nadrzędnym ( *ComponentsSample*) znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="4908a-405">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="4908a-406">Składniki zdefiniowane w innej przestrzeni nazw można wprowadzić do zakresu przy użyciu dyrektywy [ \@using używającej](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="4908a-406">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="4908a-407">Jeśli inny składnik, `NavMenu.razor`,,, istnieje w `ComponentsSample/Shared/`folderze, składnik może być używany w `Index.razor` programie z następującą `@using` instrukcją:</span><span class="sxs-lookup"><span data-stu-id="4908a-407">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="4908a-408">Do składników można także odwoływać się za pomocą ich w pełni kwalifikowanych nazw, co [ \@](xref:mvc/views/razor#using) eliminuje konieczność stosowania dyrektywy using:</span><span class="sxs-lookup"><span data-stu-id="4908a-408">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="4908a-409">`global::` Kwalifikacja nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="4908a-409">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="4908a-410">Importowanie składników przy użyciu `using` instrukcji z aliasami ( `@using Foo = Bar`np.) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4908a-410">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="4908a-411">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4908a-411">Partially qualified names aren't supported.</span></span> <span data-ttu-id="4908a-412">Na przykład dodawanie `@using ComponentsSample` i odwoływanie `NavMenu.razor` się `<Shared.NavMenu></Shared.NavMenu>` za pomocą nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4908a-412">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="4908a-413">Warunkowe atrybuty elementu HTML</span><span class="sxs-lookup"><span data-stu-id="4908a-413">Conditional HTML element attributes</span></span>

<span data-ttu-id="4908a-414">Atrybuty elementu HTML są warunkowo renderowane na podstawie wartości .NET.</span><span class="sxs-lookup"><span data-stu-id="4908a-414">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="4908a-415">Jeśli wartość jest `false` lub `null`, atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="4908a-415">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="4908a-416">Jeśli wartość to `true`, atrybut jest renderowany jako zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="4908a-416">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="4908a-417">W poniższym przykładzie, określa `IsCompleted` , czy `checked` jest renderowany w znacznikach elementu:</span><span class="sxs-lookup"><span data-stu-id="4908a-417">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="4908a-418">Jeśli `IsCompleted` jest`true`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="4908a-418">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="4908a-419">Jeśli `IsCompleted` jest`false`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="4908a-419">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="4908a-420">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="4908a-420">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="4908a-421">Nieprzetworzony kod HTML</span><span class="sxs-lookup"><span data-stu-id="4908a-421">Raw HTML</span></span>

<span data-ttu-id="4908a-422">Ciągi są zwykle renderowane przy użyciu węzłów tekstowych DOM, co oznacza, że wszystkie znaczniki, które mogą zawierać, są ignorowane i traktowane jako tekst literału.</span><span class="sxs-lookup"><span data-stu-id="4908a-422">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="4908a-423">Aby renderować nieprzetworzony kod HTML, zawiń zawartość HTML w `MarkupString` wartości.</span><span class="sxs-lookup"><span data-stu-id="4908a-423">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="4908a-424">Wartość jest analizowana jako plik HTML lub SVG i wstawiona do modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="4908a-424">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="4908a-425">Renderowanie nieprzetworzonego kodu HTML zbudowanego z dowolnego niezaufanego źródła stanowi **zagrożenie bezpieczeństwa** i należy je unikać!</span><span class="sxs-lookup"><span data-stu-id="4908a-425">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="4908a-426">W poniższym przykładzie pokazano, `MarkupString` jak za pomocą typu dodać blok statycznej zawartości HTML do renderowanego danych wyjściowych składnika:</span><span class="sxs-lookup"><span data-stu-id="4908a-426">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="4908a-427">Składniki z szablonami</span><span class="sxs-lookup"><span data-stu-id="4908a-427">Templated components</span></span>

<span data-ttu-id="4908a-428">Składniki z szablonami są składnikami, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które mogą być następnie używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-428">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="4908a-429">Składniki z szablonami umożliwiają tworzenie składników wyższego poziomu, które są większe niż zwykłe składniki.</span><span class="sxs-lookup"><span data-stu-id="4908a-429">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="4908a-430">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="4908a-430">A couple of examples include:</span></span>

* <span data-ttu-id="4908a-431">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka, wierszy i stopki tabeli.</span><span class="sxs-lookup"><span data-stu-id="4908a-431">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="4908a-432">Składnik listy, który umożliwia użytkownikowi określenie szablonu do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="4908a-432">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="4908a-433">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="4908a-433">Template parameters</span></span>

<span data-ttu-id="4908a-434">Składnik szablonu jest definiowany przez określenie co najmniej jednego parametru składnika typu `RenderFragment` lub. `RenderFragment<T>`</span><span class="sxs-lookup"><span data-stu-id="4908a-434">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="4908a-435">Fragment renderowania reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="4908a-435">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="4908a-436">`RenderFragment<T>`przyjmuje parametr typu, który można określić podczas wywoływania fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="4908a-436">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="4908a-437">`TableTemplate`składnika</span><span class="sxs-lookup"><span data-stu-id="4908a-437">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="4908a-438">W przypadku korzystania z składnika z szablonem parametry szablonu można określić za pomocą elementów podrzędnych, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="4908a-438">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="4908a-439">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="4908a-439">Template context parameters</span></span>

<span data-ttu-id="4908a-440">Argumenty składnika `RenderFragment<T>` typu przekazane jako elementy mają niejawny parametr o `context` nazwie (na przykład z poprzedniego przykładu `@context.PetId`kodu), ale można zmienić nazwę parametru przy użyciu `Context` atrybutu w elemencie podrzędnym postaci.</span><span class="sxs-lookup"><span data-stu-id="4908a-440">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="4908a-441">W poniższym przykładzie `RowTemplate` `Context` atrybut elementu określa `pet` parametr:</span><span class="sxs-lookup"><span data-stu-id="4908a-441">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="4908a-442">Alternatywnie można określić `Context` atrybut dla elementu składnika.</span><span class="sxs-lookup"><span data-stu-id="4908a-442">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="4908a-443">Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="4908a-443">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="4908a-444">Może to być przydatne, jeśli chcesz określić nazwę parametru zawartości dla niejawnej zawartości podrzędnej (bez żadnego elementu podrzędnego otoki).</span><span class="sxs-lookup"><span data-stu-id="4908a-444">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="4908a-445">W poniższym przykładzie `Context` atrybut pojawia się `TableTemplate` na elemencie i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="4908a-445">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="4908a-446">Składniki typu rodzajowego</span><span class="sxs-lookup"><span data-stu-id="4908a-446">Generic-typed components</span></span>

<span data-ttu-id="4908a-447">Składniki z szablonami są często wpisywane ogólnie.</span><span class="sxs-lookup"><span data-stu-id="4908a-447">Templated components are often generically typed.</span></span> <span data-ttu-id="4908a-448">Na przykład, składnik ogólny `ListViewTemplate` może służyć do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="4908a-448">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="4908a-449">Aby zdefiniować składnik ogólny, użyj `@typeparam` dyrektywy do określenia parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="4908a-449">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="4908a-450">W przypadku używania składników o typie ogólnym parametr typu jest wnioskowany, jeśli jest to możliwe:</span><span class="sxs-lookup"><span data-stu-id="4908a-450">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="4908a-451">W przeciwnym razie parametr typu musi być jawnie określony przy użyciu atrybutu, który jest zgodny z nazwą parametru typu.</span><span class="sxs-lookup"><span data-stu-id="4908a-451">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="4908a-452">W poniższym przykładzie `TItem="Pet"` określa typ:</span><span class="sxs-lookup"><span data-stu-id="4908a-452">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="4908a-453">Wartości kaskadowe i parametry</span><span class="sxs-lookup"><span data-stu-id="4908a-453">Cascading values and parameters</span></span>

<span data-ttu-id="4908a-454">W niektórych scenariuszach nie można przepływać danych z składnika nadrzędnego do składnika potomnego przy użyciu [parametrów składnika](#component-parameters), zwłaszcza gdy istnieje kilka warstw składników.</span><span class="sxs-lookup"><span data-stu-id="4908a-454">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="4908a-455">Wartości kaskadowe i parametry rozwiązują ten problem, zapewniając wygodną metodę dla składnika nadrzędnego, aby zapewnić wartość wszystkim jej składnikom potomnym.</span><span class="sxs-lookup"><span data-stu-id="4908a-455">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="4908a-456">Kaskadowe wartości i parametry również zapewniają podejście do współrzędnych składników.</span><span class="sxs-lookup"><span data-stu-id="4908a-456">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="4908a-457">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="4908a-457">Theme example</span></span>

<span data-ttu-id="4908a-458">W poniższym przykładzie z przykładowej aplikacji `ThemeInfo` Klasa określa informacje o motywie, aby przetworzyć hierarchię składników w taki sposób, aby wszystkie przyciski w danej części aplikacji miały ten sam styl.</span><span class="sxs-lookup"><span data-stu-id="4908a-458">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="4908a-459">*UIThemeClasses/themeinfo wskazuje. cs*:</span><span class="sxs-lookup"><span data-stu-id="4908a-459">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="4908a-460">Składnik nadrzędny może zapewnić kaskadową wartość przy użyciu składnika wartości kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="4908a-460">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="4908a-461">`CascadingValue` Składnik otacza poddrzewo hierarchii składników i dostarcza jedną wartość do wszystkich składników w tym poddrzewie.</span><span class="sxs-lookup"><span data-stu-id="4908a-461">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="4908a-462">Przykładowo aplikacja Przykładowa określa informacje o motywie`ThemeInfo`() w jednej z układów aplikacji jako parametr kaskadowy dla wszystkich składników, które tworzą treść `@Body` układu właściwości.</span><span class="sxs-lookup"><span data-stu-id="4908a-462">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="4908a-463">`ButtonClass`ma przypisaną wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="4908a-463">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="4908a-464">Każdy składnik podrzędny może wykorzystać tę właściwość za pomocą `ThemeInfo` obiektu kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="4908a-464">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="4908a-465">`CascadingValuesParametersLayout`składnika</span><span class="sxs-lookup"><span data-stu-id="4908a-465">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="4908a-466">Aby korzystać z wartości kaskadowych, składniki deklarują kaskadowe parametry przy użyciu `[CascadingParameter]` atrybutu lub na podstawie wartości nazwy ciągu:</span><span class="sxs-lookup"><span data-stu-id="4908a-466">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="4908a-467">Powiązanie z wartością nazwy ciągu jest istotne, jeśli istnieje wiele wartości kaskadowych tego samego typu i trzeba je odróżnić w ramach tego samego poddrzewa.</span><span class="sxs-lookup"><span data-stu-id="4908a-467">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="4908a-468">Wartości kaskadowe są powiązane z parametrami kaskadowymi według typu.</span><span class="sxs-lookup"><span data-stu-id="4908a-468">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="4908a-469">W przykładowej aplikacji `CascadingValuesParametersTheme` składnik wiąże `ThemeInfo` wartość kaskadową z parametrem kaskadowym.</span><span class="sxs-lookup"><span data-stu-id="4908a-469">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="4908a-470">Parametr służy do ustawiania klasy CSS dla jednego z przycisków wyświetlanych przez składnik.</span><span class="sxs-lookup"><span data-stu-id="4908a-470">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="4908a-471">`CascadingValuesParametersTheme`składnika</span><span class="sxs-lookup"><span data-stu-id="4908a-471">`CascadingValuesParametersTheme` component:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="4908a-472">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="4908a-472">TabSet example</span></span>

<span data-ttu-id="4908a-473">Parametry kaskadowe umożliwiają również współdziałanie składników w hierarchii składników.</span><span class="sxs-lookup"><span data-stu-id="4908a-473">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="4908a-474">Rozważmy na przykład następujący przykład *TabSet* w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="4908a-474">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="4908a-475">Przykładowa aplikacja ma `ITab` interfejs, który implementuje karty:</span><span class="sxs-lookup"><span data-stu-id="4908a-475">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="4908a-476">Składnik używa składnika, który zawiera kilka `Tab` składników: `TabSet` `CascadingValuesParametersTabSet`</span><span class="sxs-lookup"><span data-stu-id="4908a-476">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="4908a-477">Składniki podrzędne `Tab` nie są jawnie przenoszone jako parametry `TabSet`do.</span><span class="sxs-lookup"><span data-stu-id="4908a-477">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="4908a-478">Zamiast tego składniki podrzędne `Tab` są częścią zawartości `TabSet`elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4908a-478">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="4908a-479">Jednak nadal musi wiedzieć o każdym `Tab` składniku, aby można było renderować nagłówki i aktywną kartę. `TabSet` Aby umożliwić tę koordynację bez konieczności stosowania dodatkowego kodu `TabSet` , składnik *może stanowić wartość kaskadową* , która następnie jest wybierana przez składniki potomne `Tab` .</span><span class="sxs-lookup"><span data-stu-id="4908a-479">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="4908a-480">`TabSet`składnika</span><span class="sxs-lookup"><span data-stu-id="4908a-480">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="4908a-481">Składniki potomne `Tab` przechwytują zawierający `TabSet` jako parametr kaskadowy, więc `Tab` składniki dodają się do `TabSet` i koordynują, na której karcie jest aktywna.</span><span class="sxs-lookup"><span data-stu-id="4908a-481">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="4908a-482">`Tab`składnika</span><span class="sxs-lookup"><span data-stu-id="4908a-482">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="4908a-483">Szablony Razor</span><span class="sxs-lookup"><span data-stu-id="4908a-483">Razor templates</span></span>

<span data-ttu-id="4908a-484">Fragmenty renderowania można definiować przy użyciu składni szablonu Razor.</span><span class="sxs-lookup"><span data-stu-id="4908a-484">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="4908a-485">Szablony Razor są sposobem definiowania fragmentu interfejsu użytkownika i przyjmuje następujący format:</span><span class="sxs-lookup"><span data-stu-id="4908a-485">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="4908a-486">Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości oraz renderowania szablonów bezpośrednio w składniku.</span><span class="sxs-lookup"><span data-stu-id="4908a-486">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="4908a-487">Fragmenty renderowania mogą być również przekazane jako argumenty do [składników](#templated-components)z szablonem.</span><span class="sxs-lookup"><span data-stu-id="4908a-487">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="4908a-488">Renderowane dane wyjściowe poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="4908a-488">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="4908a-489">Ręczna logika RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="4908a-489">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="4908a-490">`Microsoft.AspNetCore.Components.RenderTree`zapewnia metody manipulowania składnikami i elementami, w tym ręczne Kompilowanie C# składników w kodzie.</span><span class="sxs-lookup"><span data-stu-id="4908a-490">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="4908a-491">Korzystanie z `RenderTreeBuilder` programu do tworzenia składników jest zaawansowanym scenariuszem.</span><span class="sxs-lookup"><span data-stu-id="4908a-491">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="4908a-492">Nieprawidłowo sformułowany składnik (na przykład niezamknięty tag znacznika) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="4908a-492">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="4908a-493">Rozważmy następujący `PetDetails` składnik, który można ręcznie utworzyć w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="4908a-493">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="4908a-494">W poniższym przykładzie pętla w `CreateComponent` metodzie generuje trzy `PetDetails` składniki.</span><span class="sxs-lookup"><span data-stu-id="4908a-494">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="4908a-495">Podczas wywoływania `RenderTreeBuilder` metod tworzenia składników (`OpenComponent` i `AddAttribute`) numery sekwencji są numerami wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="4908a-495">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="4908a-496">Algorytm Blazor różnica polega na numerach sekwencji odpowiadających odrębnym wierszom kodu, a nie odrębnym wywoływaniu wywołań.</span><span class="sxs-lookup"><span data-stu-id="4908a-496">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="4908a-497">Podczas tworzenia składnika przy użyciu `RenderTreeBuilder` metod umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="4908a-497">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="4908a-498">**Użycie obliczenia lub licznika do wygenerowania numeru sekwencji może prowadzić do niskiej wydajności.**</span><span class="sxs-lookup"><span data-stu-id="4908a-498">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="4908a-499">Aby uzyskać więcej informacji, zobacz sekcję [numery sekwencji powiązane z numerami wierszy kodu i kolejnością niewykonania](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="4908a-499">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="4908a-500">`BuiltContent`składnika</span><span class="sxs-lookup"><span data-stu-id="4908a-500">`BuiltContent` component:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="4908a-501">Numery sekwencji odnoszą się do numerów wierszy kodu, a nie kolejności wykonywania</span><span class="sxs-lookup"><span data-stu-id="4908a-501">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="4908a-502">Pliki `.razor` Blazor są zawsze kompilowane.</span><span class="sxs-lookup"><span data-stu-id="4908a-502">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="4908a-503">Jest to znakomita korzyść `.razor` , ponieważ krok kompilowania może służyć do iniekcji informacji, które zwiększają wydajność aplikacji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4908a-503">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="4908a-504">Najważniejszym przykładem tych ulepszeń są *numery sekwencji*.</span><span class="sxs-lookup"><span data-stu-id="4908a-504">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="4908a-505">Numery sekwencji wskazują na środowisko uruchomieniowe, które pochodzą z różnych i uporządkowanych wierszy kodu.</span><span class="sxs-lookup"><span data-stu-id="4908a-505">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="4908a-506">Środowisko uruchomieniowe używa tych informacji do generowania wydajnych różnic drzewa w czasie liniowym, które są znacznie szybsze niż zwykle jest to możliwe dla algorytmu różnicowego drzewa ogólnego.</span><span class="sxs-lookup"><span data-stu-id="4908a-506">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="4908a-507">Rozważmy następujący prosty `.razor` plik:</span><span class="sxs-lookup"><span data-stu-id="4908a-507">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="4908a-508">Poprzedni kod kompiluje się w taki sposób, aby wyglądał następująco:</span><span class="sxs-lookup"><span data-stu-id="4908a-508">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="4908a-509">Gdy kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="4908a-509">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="4908a-510">Sequence</span><span class="sxs-lookup"><span data-stu-id="4908a-510">Sequence</span></span> | <span data-ttu-id="4908a-511">Typ</span><span class="sxs-lookup"><span data-stu-id="4908a-511">Type</span></span>      | <span data-ttu-id="4908a-512">Dane</span><span class="sxs-lookup"><span data-stu-id="4908a-512">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="4908a-513">0</span><span class="sxs-lookup"><span data-stu-id="4908a-513">0</span></span>        | <span data-ttu-id="4908a-514">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4908a-514">Text node</span></span> | <span data-ttu-id="4908a-515">Pierwszego</span><span class="sxs-lookup"><span data-stu-id="4908a-515">First</span></span>  |
| <span data-ttu-id="4908a-516">1</span><span class="sxs-lookup"><span data-stu-id="4908a-516">1</span></span>        | <span data-ttu-id="4908a-517">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4908a-517">Text node</span></span> | <span data-ttu-id="4908a-518">Sekunda</span><span class="sxs-lookup"><span data-stu-id="4908a-518">Second</span></span> |

<span data-ttu-id="4908a-519">Wyobraź sobie `someFlag` , `false`że zostanie ona przerenderowana, a znaczniki są renderowane ponownie.</span><span class="sxs-lookup"><span data-stu-id="4908a-519">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="4908a-520">Tym razem Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="4908a-520">This time, the builder receives:</span></span>

| <span data-ttu-id="4908a-521">Sequence</span><span class="sxs-lookup"><span data-stu-id="4908a-521">Sequence</span></span> | <span data-ttu-id="4908a-522">Typ</span><span class="sxs-lookup"><span data-stu-id="4908a-522">Type</span></span>       | <span data-ttu-id="4908a-523">Dane</span><span class="sxs-lookup"><span data-stu-id="4908a-523">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="4908a-524">1</span><span class="sxs-lookup"><span data-stu-id="4908a-524">1</span></span>        | <span data-ttu-id="4908a-525">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4908a-525">Text node</span></span>  | <span data-ttu-id="4908a-526">Sekunda</span><span class="sxs-lookup"><span data-stu-id="4908a-526">Second</span></span> |

<span data-ttu-id="4908a-527">Gdy środowisko uruchomieniowe wykonuje porównanie, zobaczy, że element w sekwencji `0` został usunięty, więc generuje następujący skrypt uproszczonej *edycji*:</span><span class="sxs-lookup"><span data-stu-id="4908a-527">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="4908a-528">Usuń pierwszy węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="4908a-528">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="4908a-529">Co się stało z błędami w przypadku wygenerowania numerów sekwencyjnych</span><span class="sxs-lookup"><span data-stu-id="4908a-529">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="4908a-530">Wyobraź sobie, że została zapisana następująca logika konstruktora drzewa renderowania:</span><span class="sxs-lookup"><span data-stu-id="4908a-530">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="4908a-531">Teraz pierwsze dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="4908a-531">Now, the first output is:</span></span>

| <span data-ttu-id="4908a-532">Sequence</span><span class="sxs-lookup"><span data-stu-id="4908a-532">Sequence</span></span> | <span data-ttu-id="4908a-533">Typ</span><span class="sxs-lookup"><span data-stu-id="4908a-533">Type</span></span>      | <span data-ttu-id="4908a-534">Dane</span><span class="sxs-lookup"><span data-stu-id="4908a-534">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="4908a-535">0</span><span class="sxs-lookup"><span data-stu-id="4908a-535">0</span></span>        | <span data-ttu-id="4908a-536">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4908a-536">Text node</span></span> | <span data-ttu-id="4908a-537">Pierwszego</span><span class="sxs-lookup"><span data-stu-id="4908a-537">First</span></span>  |
| <span data-ttu-id="4908a-538">1</span><span class="sxs-lookup"><span data-stu-id="4908a-538">1</span></span>        | <span data-ttu-id="4908a-539">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4908a-539">Text node</span></span> | <span data-ttu-id="4908a-540">Sekunda</span><span class="sxs-lookup"><span data-stu-id="4908a-540">Second</span></span> |

<span data-ttu-id="4908a-541">Ten wynik jest identyczny z poprzednim przypadkiem, dlatego nie istnieją żadne negatywne problemy.</span><span class="sxs-lookup"><span data-stu-id="4908a-541">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="4908a-542">`someFlag`znajduje `false` się na drugim renderingu, a dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="4908a-542">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="4908a-543">Sequence</span><span class="sxs-lookup"><span data-stu-id="4908a-543">Sequence</span></span> | <span data-ttu-id="4908a-544">Typ</span><span class="sxs-lookup"><span data-stu-id="4908a-544">Type</span></span>      | <span data-ttu-id="4908a-545">Dane</span><span class="sxs-lookup"><span data-stu-id="4908a-545">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="4908a-546">0</span><span class="sxs-lookup"><span data-stu-id="4908a-546">0</span></span>        | <span data-ttu-id="4908a-547">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4908a-547">Text node</span></span> | <span data-ttu-id="4908a-548">Sekunda</span><span class="sxs-lookup"><span data-stu-id="4908a-548">Second</span></span> |

<span data-ttu-id="4908a-549">Tym razem algorytm diff widzi, że pojawiły się *dwie* zmiany, a algorytm generuje następujący skrypt edycji:</span><span class="sxs-lookup"><span data-stu-id="4908a-549">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="4908a-550">Zmień wartość pierwszego węzła tekstowego na `Second`.</span><span class="sxs-lookup"><span data-stu-id="4908a-550">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="4908a-551">Usuń drugi węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="4908a-551">Remove the second text node.</span></span>

<span data-ttu-id="4908a-552">Generowanie numerów sekwencji utraciło wszystkie przydatne informacje o tym, gdzie znajdują się `if/else` gałęzie i pętle w oryginalnym kodzie.</span><span class="sxs-lookup"><span data-stu-id="4908a-552">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="4908a-553">Wynikiem tego jest różnica **dwa razy** , tak długo, jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4908a-553">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="4908a-554">Jest to prosty przykład.</span><span class="sxs-lookup"><span data-stu-id="4908a-554">This is a trivial example.</span></span> <span data-ttu-id="4908a-555">W bardziej realistycznych przypadkach ze złożonymi i głęboko zagnieżdżonymi strukturami, szczególnie w przypadku pętli, koszt wydajności jest bardziej poważny.</span><span class="sxs-lookup"><span data-stu-id="4908a-555">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="4908a-556">Zamiast natychmiastowego identyfikowania, które bloki lub gałęzie pętli zostały wstawione lub usunięte, algorytm różnicowy musi reprezentować się w drzewach renderowania i zwykle tworzyć dużo dłużej edytowane skrypty, ponieważ nie są w nim poinformowani o sposobie starych i nowych struktur odnoszą się do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="4908a-556">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="4908a-557">Wskazówki i wnioski</span><span class="sxs-lookup"><span data-stu-id="4908a-557">Guidance and conclusions</span></span>

* <span data-ttu-id="4908a-558">Wydajność aplikacji ma wpływ na to, że numery sekwencji są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="4908a-558">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="4908a-559">Struktura nie może automatycznie tworzyć własnych numerów sekwencji w czasie wykonywania, ponieważ niezbędne informacje nie istnieją, chyba że są przechwytywane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-559">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="4908a-560">Nie zapisuj długich bloków logiki wykonywanej `RenderTreeBuilder` ręcznie.</span><span class="sxs-lookup"><span data-stu-id="4908a-560">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="4908a-561">Preferuj `.razor` pliki i Zezwalaj kompilatorowi na rozpatruje numery sekwencji.</span><span class="sxs-lookup"><span data-stu-id="4908a-561">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="4908a-562">Jeśli numery sekwencji są stałee, algorytm diff wymaga tylko zwiększenia wartości sekwencji.</span><span class="sxs-lookup"><span data-stu-id="4908a-562">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="4908a-563">Początkowa wartość i przerwy są nieistotne.</span><span class="sxs-lookup"><span data-stu-id="4908a-563">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="4908a-564">Jedną z wiarygodnych opcji jest użycie numeru wiersza kodu jako numeru sekwencyjnego lub rozpoczęcie od zera i zwiększenie według wartości lub setek (lub dowolnego preferowanego interwału).</span><span class="sxs-lookup"><span data-stu-id="4908a-564">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="4908a-565">Blazor używa numerów sekwencji, podczas gdy inne struktury interfejsu użytkownika porównujące drzewa nie są używane.</span><span class="sxs-lookup"><span data-stu-id="4908a-565">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="4908a-566">Różnica jest znacznie szybsza, gdy są używane numery sekwencji, a Blazor ma zalety kroku kompilacji, który zajmuje się automatycznie numerami sekwencyjnymi dla deweloperów tworzących `.razor` pliki.</span><span class="sxs-lookup"><span data-stu-id="4908a-566">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="4908a-567">Lokalizacja</span><span class="sxs-lookup"><span data-stu-id="4908a-567">Localization</span></span>

<span data-ttu-id="4908a-568">Blazor aplikacje po stronie serwera są zlokalizowane przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="4908a-568">Blazor server-side apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="4908a-569">Oprogramowanie pośredniczące wybiera odpowiednią kulturę dla użytkowników żądających zasobów z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-569">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="4908a-570">Kulturę można ustawić przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="4908a-570">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="4908a-571">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="4908a-571">Cookies</span></span>](#cookies)
* [<span data-ttu-id="4908a-572">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="4908a-572">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="4908a-573">Aby uzyskać więcej informacji i przykładów, <xref:fundamentals/localization>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="4908a-573">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="4908a-574">Cookie</span><span class="sxs-lookup"><span data-stu-id="4908a-574">Cookies</span></span>

<span data-ttu-id="4908a-575">Plik cookie kultury lokalizacji może utrzymywać kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-575">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="4908a-576">Plik cookie jest tworzony przez `OnGet` metodę strony hosta aplikacji (*strony/host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="4908a-576">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="4908a-577">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie na kolejnych żądaniach, aby ustawić kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-577">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="4908a-578">Użycie pliku cookie zapewnia, że połączenie z użyciem protokołu WebSocket może prawidłowo propagować kulturę.</span><span class="sxs-lookup"><span data-stu-id="4908a-578">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="4908a-579">Jeśli schematy lokalizacji są oparte na ścieżce URL lub ciągu zapytania, schemat może nie być w stanie współdziałać z usługą WebSockets, więc nie będzie można zachować kultury.</span><span class="sxs-lookup"><span data-stu-id="4908a-579">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="4908a-580">W związku z tym zalecanym podejściem jest użycie pliku cookie kultury lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-580">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="4908a-581">Każda technika może służyć do przypisywania kultury, jeśli kultura jest utrwalona w pliku cookie lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-581">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="4908a-582">Jeśli aplikacja ma już ustalony schemat lokalizacji dla ASP.NET Core po stronie serwera, Kontynuuj korzystanie z istniejącej infrastruktury lokalizacji aplikacji i Ustaw plik cookie kultury lokalizacji w schemacie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-582">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="4908a-583">Poniższy przykład pokazuje, jak ustawić bieżącą kulturę w pliku cookie, który może zostać odczytany przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-583">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="4908a-584">Utwórz plik *strony/hosta. cshtml. cs* z następującą zawartością w aplikacji po stronie serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="4908a-584">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor server-side app:</span></span>

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

<span data-ttu-id="4908a-585">Lokalizacja jest obsługiwana w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4908a-585">Localization is handled in the app:</span></span>

1. <span data-ttu-id="4908a-586">Przeglądarka wysyła początkowe żądanie HTTP do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-586">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="4908a-587">Kultura jest przypisana przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4908a-587">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="4908a-588">Metoda w *_Host. cshtml. cs* utrzymuje kulturę w pliku cookie jako część odpowiedzi. `OnGet`</span><span class="sxs-lookup"><span data-stu-id="4908a-588">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="4908a-589">Przeglądarka otwiera połączenie WebSocket, aby utworzyć interaktywną sesję po stronie serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4908a-589">The browser opens a WebSocket connection to create an interactive Blazor server-side session.</span></span>
1. <span data-ttu-id="4908a-590">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie i przypisuje kulturę.</span><span class="sxs-lookup"><span data-stu-id="4908a-590">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="4908a-591">Sesja po stronie serwera Blazor rozpoczyna się od poprawnej kultury.</span><span class="sxs-lookup"><span data-stu-id="4908a-591">The Blazor server-side session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="4908a-592">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="4908a-592">Provide UI to choose the culture</span></span>

<span data-ttu-id="4908a-593">Aby zapewnić interfejs użytkownika, aby umożliwić użytkownikowi wybranie kultury, zalecane jest *podejście oparte na* przekierowaniu.</span><span class="sxs-lookup"><span data-stu-id="4908a-593">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="4908a-594">Ten proces jest podobny do tego, co się dzieje w aplikacji sieci Web, gdy użytkownik próbuje uzyskać&mdash;dostęp do bezpiecznego zasobu, użytkownik zostanie przekierowany do strony logowania, a następnie przekierowany z powrotem do oryginalnego zasobu.</span><span class="sxs-lookup"><span data-stu-id="4908a-594">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="4908a-595">Aplikacja utrzymuje wybraną kulturę użytkownika za pośrednictwem przekierowania do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4908a-595">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="4908a-596">Kontroler ustawia wybraną kulturę użytkownika na plik cookie i przekierowuje użytkownika z powrotem do oryginalnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="4908a-596">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="4908a-597">Ustanów punkt końcowy HTTP na serwerze, aby ustawić kulturę wybraną przez użytkownika w pliku cookie i wykonać przekierowanie z powrotem do oryginalnego identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="4908a-597">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="4908a-598">Użyj wyniku `LocalRedirect` działania, aby zapobiec atakom typu Open redirect.</span><span class="sxs-lookup"><span data-stu-id="4908a-598">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="4908a-599">Aby uzyskać więcej informacji, zobacz <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="4908a-599">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="4908a-600">Poniższy składnik przedstawia przykład sposobu wykonywania wstępnego przekierowania, gdy użytkownik wybierze kulturę:</span><span class="sxs-lookup"><span data-stu-id="4908a-600">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject IUriHelper UriHelper

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(UIChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(UriHelper.GetAbsoluteUri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        UriHelper.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="4908a-601">Korzystanie z scenariuszy lokalizacji platformy .NET w aplikacjach Blazor</span><span class="sxs-lookup"><span data-stu-id="4908a-601">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="4908a-602">W aplikacjach Blazor dostępne są następujące scenariusze dotyczące lokalizacji i globalizacji platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="4908a-602">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="4908a-603">. System zasobów netto</span><span class="sxs-lookup"><span data-stu-id="4908a-603">.NET's resources system</span></span>
* <span data-ttu-id="4908a-604">Formatowanie liczb i dat specyficznych dla kultury</span><span class="sxs-lookup"><span data-stu-id="4908a-604">Culture-specific number and date formatting</span></span>

<span data-ttu-id="4908a-605">`@bind` Funkcja Blazor wykonuje globalizację w oparciu o bieżącą kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4908a-605">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="4908a-606">Aby uzyskać więcej informacji, zobacz sekcję [powiązanie danych](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="4908a-606">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="4908a-607">Obecnie obsługiwane są ograniczone zestawy ASP.NET Core scenariuszy lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="4908a-607">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="4908a-608">`IStringLocalizer<>`*jest obsługiwany* w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="4908a-608">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="4908a-609">`IHtmlLocalizer<>`Lokalizacje adnotacji danych są ASP.NET Core w scenariuszach MVC i nie są obsługiwane w aplikacjach Blazor. `IViewLocalizer<>`</span><span class="sxs-lookup"><span data-stu-id="4908a-609">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="4908a-610">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="4908a-610">For more information, see <xref:fundamentals/localization>.</span></span>
