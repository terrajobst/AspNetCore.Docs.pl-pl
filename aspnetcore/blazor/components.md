---
title: Tworzenie i używanie składników ASP.NET Core Razor
author: guardrex
description: Dowiedz się, jak tworzyć i używać składników Razor, w tym jak powiązać z danymi, obsługiwać zdarzenia i zarządzać cyklem życia składników.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: bc1d07aef9cd60b89343a034168daa6754f4421b
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306507"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="75488-103">Tworzenie i używanie składników ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="75488-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="75488-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="75488-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="75488-105">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75488-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="75488-106">aplikacje Blazor są kompilowane przy użyciu *składników*programu.</span><span class="sxs-lookup"><span data-stu-id="75488-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="75488-107">Składnik jest niezależnym fragmentem interfejsu użytkownika (UI), takim jak strona, okno dialogowe lub formularz.</span><span class="sxs-lookup"><span data-stu-id="75488-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="75488-108">Składnik zawiera znaczniki HTML i logikę przetwarzania wymagane do iniekcji danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="75488-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="75488-109">Składniki są elastyczne i lekkie.</span><span class="sxs-lookup"><span data-stu-id="75488-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="75488-110">Mogą być zagnieżdżane, ponownie używane i udostępniane między projektami.</span><span class="sxs-lookup"><span data-stu-id="75488-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="75488-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="75488-111">Component classes</span></span>

<span data-ttu-id="75488-112">Składniki są zaimplementowane w plikach składników [Razor](xref:mvc/views/razor) ( *. Razor*) przy użyciu kombinacji C# i znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="75488-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="75488-113">Składnik Blazor jest formalnie określany jako *składnik Razor*.</span><span class="sxs-lookup"><span data-stu-id="75488-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="75488-114">Nazwa składnika musi rozpoczynać się wielką literą.</span><span class="sxs-lookup"><span data-stu-id="75488-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="75488-115">Na przykład *MyCoolComponent. Razor* jest prawidłowy, a *MyCoolComponent. Razor* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="75488-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="75488-116">Interfejs użytkownika dla składnika jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="75488-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="75488-117">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="75488-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="75488-118">Po skompilowaniu aplikacji logika znaczników HTML i C# renderowania jest konwertowana na klasę składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="75488-119">Nazwa wygenerowanej klasy jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="75488-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="75488-120">Elementy członkowskie klasy składnika są zdefiniowane w bloku `@code`.</span><span class="sxs-lookup"><span data-stu-id="75488-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="75488-121">W bloku `@code` stan składnika (właściwości, pola) jest określany przy użyciu metod obsługi zdarzeń lub definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="75488-122">Dozwolony jest więcej niż jeden blok `@code`.</span><span class="sxs-lookup"><span data-stu-id="75488-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="75488-123">Składowe składnika mogą być używane jako część logiki renderowania składnika przy użyciu C# wyrażeń, które zaczynają się od `@`.</span><span class="sxs-lookup"><span data-stu-id="75488-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="75488-124">Na przykład C# pole jest renderowane przez utworzenie prefiksu `@` na nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="75488-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="75488-125">Poniższy przykład szacuje i renderuje:</span><span class="sxs-lookup"><span data-stu-id="75488-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="75488-126">`_headingFontStyle` wartość właściwości CSS dla `font-style`.</span><span class="sxs-lookup"><span data-stu-id="75488-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="75488-127">`_headingText` do zawartości elementu `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="75488-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="75488-128">Po pierwszym wyrenderowaniu składnika składnik generuje jego drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="75488-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="75488-129"> następnie porównuje nowe drzewo renderowania z poprzednią i zastosuje wszelkie modyfikacje Document Object Model (DOM) przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="75488-129"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="75488-130">Składniki są zwykłymi C# klasami i mogą być umieszczane w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="75488-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="75488-131">Składniki, które generują strony sieci Web, zwykle znajdują się w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="75488-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="75488-132">Składniki niestronicowe są często umieszczane w folderze *udostępnionym* lub w folderze niestandardowym dodanym do projektu.</span><span class="sxs-lookup"><span data-stu-id="75488-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="75488-133">Zazwyczaj przestrzeń nazw składnika pochodzi od głównej przestrzeni nazw aplikacji i lokalizacji składnika (folderu) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="75488-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="75488-134">Jeśli główna przestrzeń nazw aplikacji jest `BlazorApp` a składnik `Counter` znajduje się w folderze *strony* :</span><span class="sxs-lookup"><span data-stu-id="75488-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="75488-135">Przestrzeń nazw składnika `Counter` jest `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="75488-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="75488-136">W pełni kwalifikowana nazwa typu składnika jest `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="75488-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="75488-137">Aby uzyskać więcej informacji, zobacz sekcję [Importowanie składników](#import-components) .</span><span class="sxs-lookup"><span data-stu-id="75488-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="75488-138">Aby użyć folderu niestandardowego, należy dodać przestrzeń nazw folderu niestandardowego do składnika nadrzędnego lub pliku *_Imports. Razor* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="75488-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="75488-139">Na przykład następująca przestrzeń nazw sprawia, że składniki w folderze *Components* są dostępne, gdy główna przestrzeń nazw aplikacji jest `BlazorApp`:</span><span class="sxs-lookup"><span data-stu-id="75488-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a><span data-ttu-id="75488-140">Statyczne zasoby</span><span class="sxs-lookup"><span data-stu-id="75488-140">Static assets</span></span>

Blazor<span data-ttu-id="75488-141"> jest zgodna z Konwencją ASP.NET Core aplikacji umieszczających statyczne zasoby w [folderze głównym katalogu głównego (wwwroot)](xref:fundamentals/index#web-root)projektu.</span><span class="sxs-lookup"><span data-stu-id="75488-141"> follows the convention of ASP.NET Core apps placing static assets under the project's [web root (wwwroot) folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="75488-142">Użyj ścieżki względnej podstawowe (`/`), aby odwołać się do katalogu głównego sieci Web dla statycznego elementu zawartości.</span><span class="sxs-lookup"><span data-stu-id="75488-142">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="75488-143">W poniższym przykładzie *logo. png* znajduje się fizycznie w folderze *{Project root}/wwwroot/images* :</span><span class="sxs-lookup"><span data-stu-id="75488-143">In the following example, *logo.png* is physically located in the *{PROJECT ROOT}/wwwroot/images* folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

<span data-ttu-id="75488-144">Składniki Razor **nie** obsługują notacji z ukośnikiem (`~/`).</span><span class="sxs-lookup"><span data-stu-id="75488-144">Razor components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="75488-145">Aby uzyskać informacje na temat ustawiania ścieżki podstawowej aplikacji, zobacz <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="75488-145">For information on setting an app's base path, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="75488-146">Pomocnicy tagów nie są obsługiwani w składnikach</span><span class="sxs-lookup"><span data-stu-id="75488-146">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="75488-147">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) nie są obsługiwani w składnikach Razor (pliki *. Razor* ).</span><span class="sxs-lookup"><span data-stu-id="75488-147">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="75488-148">Aby zapewnić funkcje podobne do pomocnika tagów w Blazor, Utwórz składnik o tej samej funkcji, co pomocnik tagów i użyj składnika zamiast.</span><span class="sxs-lookup"><span data-stu-id="75488-148">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="75488-149">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="75488-149">Use components</span></span>

<span data-ttu-id="75488-150">Składniki mogą zawierać inne składniki, deklarując je za pomocą składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="75488-150">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="75488-151">Znaczniki użycia składnika wyglądają jak tag HTML, gdzie nazwa znacznika jest typem składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-151">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="75488-152">Poniższy znacznik w *indeksie. Razor* renderuje wystąpienie `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="75488-152">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="75488-153">*Składniki/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-153">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="75488-154">Jeśli składnik zawiera element HTML z wielką literą, która nie jest zgodna z nazwą składnika, jest emitowane ostrzeżenie wskazujące, że element ma nieoczekiwaną nazwę.</span><span class="sxs-lookup"><span data-stu-id="75488-154">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="75488-155">Dodanie `@using` dyrektywy dla przestrzeni nazw składnika sprawia, że składnik jest dostępny, co rozwiązuje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="75488-155">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="75488-156">Routing</span><span class="sxs-lookup"><span data-stu-id="75488-156">Routing</span></span>

<span data-ttu-id="75488-157">Routing w Blazor jest realizowany przez dostarczenie szablonu trasy do każdego dostępnego składnika w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="75488-157">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="75488-158">Po skompilowaniu pliku Razor z dyrektywą `@page`, wygenerowana Klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określania szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="75488-158">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="75488-159">W czasie wykonywania router szuka klas składników przy użyciu `RouteAttribute` i renderuje niezależnie składnik ma szablon trasy pasujący do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="75488-159">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="75488-160">Aby uzyskać więcej informacji, zobacz <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="75488-160">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="75488-161">Parametry</span><span class="sxs-lookup"><span data-stu-id="75488-161">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="75488-162">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="75488-162">Route parameters</span></span>

<span data-ttu-id="75488-163">Składniki mogą odbierać parametry tras z szablonu trasy dostarczonego w dyrektywie `@page`.</span><span class="sxs-lookup"><span data-stu-id="75488-163">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="75488-164">Router używa parametrów trasy, aby wypełnić odpowiednie parametry składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-164">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="75488-165">*Strony/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-165">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="75488-166">Parametry opcjonalne nie są obsługiwane, więc dwie dyrektywy `@page` są stosowane w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="75488-166">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="75488-167">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="75488-167">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="75488-168">Druga dyrektywa `@page` otrzymuje parametr trasy `{text}` i przypisuje wartość do właściwości `Text`.</span><span class="sxs-lookup"><span data-stu-id="75488-168">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="75488-169">Składnia *catch-all* (`*`/`**`), która przechwytuje ścieżkę między wieloma granicami folderów, **nie** jest obsługiwana w składnikach Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="75488-169">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="75488-170">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="75488-170">Component parameters</span></span>

<span data-ttu-id="75488-171">Składniki mogą mieć *Parametry składnika*, które są zdefiniowane przy użyciu właściwości publicznych w klasie składnika z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="75488-171">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="75488-172">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="75488-172">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="75488-173">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-173">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="75488-174">W poniższym przykładzie z przykładowej aplikacji `ParentComponent` ustawia wartość właściwości `Title` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="75488-174">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="75488-175">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-175">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="75488-176">Zawartość podrzędna</span><span class="sxs-lookup"><span data-stu-id="75488-176">Child content</span></span>

<span data-ttu-id="75488-177">Składniki mogą ustawiać zawartość innego składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-177">Components can set the content of another component.</span></span> <span data-ttu-id="75488-178">Składnik Assigner zawiera zawartość między tagami, które określają składnik do odbioru.</span><span class="sxs-lookup"><span data-stu-id="75488-178">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="75488-179">W poniższym przykładzie `ChildComponent` ma właściwość `ChildContent`, która reprezentuje `RenderFragment`, która reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="75488-179">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="75488-180">Wartość `ChildContent` jest umieszczana w znacznikach składnika, w którym powinna być renderowana zawartość.</span><span class="sxs-lookup"><span data-stu-id="75488-180">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="75488-181">Wartość `ChildContent` jest odbierana ze składnika nadrzędnego i renderowany w `panel-body`panelu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="75488-181">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="75488-182">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-182">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="75488-183">Właściwość, która otrzymuje `RenderFragment` zawartość, musi mieć nazwę `ChildContent` według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="75488-183">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="75488-184">`ParentComponent` w przykładowej aplikacji może udostępniać zawartość w celu renderowania `ChildComponent` przez umieszczenie zawartości wewnątrz tagów `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="75488-184">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="75488-185">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-185">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="75488-186">Korzystając atrybutów i dowolne parametry</span><span class="sxs-lookup"><span data-stu-id="75488-186">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="75488-187">Składniki mogą przechwytywać i renderować dodatkowe atrybuty oprócz zadeklarowanych parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-187">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="75488-188">Dodatkowe atrybuty mogą być przechwytywane w słowniku, a następnie *splatted* na element, gdy składnik jest renderowany przy użyciu dyrektywy [`@attributes`](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="75488-188">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="75488-189">Ten scenariusz jest przydatny podczas definiowania składnika, który generuje element znaczników, który obsługuje różne dostosowania.</span><span class="sxs-lookup"><span data-stu-id="75488-189">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="75488-190">Na przykład można żmudnym definiować atrybuty oddzielnie dla `<input>`, które obsługują wiele parametrów.</span><span class="sxs-lookup"><span data-stu-id="75488-190">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="75488-191">W poniższym przykładzie pierwszy element `<input>` (`id="useIndividualParams"`) używa pojedynczych parametrów składnika, podczas gdy drugi `<input>` elementu (`id="useAttributesDict"`) używa atrybutu korzystając:</span><span class="sxs-lookup"><span data-stu-id="75488-191">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
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

<span data-ttu-id="75488-192">Typ parametru musi implementować `IEnumerable<KeyValuePair<string, object>>` za pomocą kluczy ciągu.</span><span class="sxs-lookup"><span data-stu-id="75488-192">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="75488-193">Używanie `IReadOnlyDictionary<string, object>` jest również opcją w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="75488-193">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="75488-194">Renderowane `<input>` elementy używające obu metod są identyczne:</span><span class="sxs-lookup"><span data-stu-id="75488-194">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="75488-195">Aby zaakceptować dowolne atrybuty, zdefiniuj parametr składnika przy użyciu atrybutu `[Parameter]` z właściwością `CaptureUnmatchedValues` ustawioną na `true`:</span><span class="sxs-lookup"><span data-stu-id="75488-195">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="75488-196">Właściwość `CaptureUnmatchedValues` na `[Parameter]` umożliwia dopasowanie parametru do wszystkich atrybutów, które nie są zgodne z żadnym innym parametrem.</span><span class="sxs-lookup"><span data-stu-id="75488-196">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="75488-197">Składnik może definiować tylko jeden parametr z `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="75488-197">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="75488-198">Typ właściwości używany z `CaptureUnmatchedValues` musi być możliwy do przypisania z `Dictionary<string, object>` z kluczami ciągu.</span><span class="sxs-lookup"><span data-stu-id="75488-198">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="75488-199">w tym scenariuszu są również dostępne opcje `IEnumerable<KeyValuePair<string, object>>` lub `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="75488-199">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="75488-200">Pozycja `@attributes` odnosząca się do pozycji atrybutów elementu jest ważna.</span><span class="sxs-lookup"><span data-stu-id="75488-200">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="75488-201">Gdy `@attributes` są splatted na elemencie, atrybuty są przetwarzane od prawej do lewej (Ostatnia do).</span><span class="sxs-lookup"><span data-stu-id="75488-201">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="75488-202">Rozważmy następujący przykład składnika, który zużywa składnik `Child`:</span><span class="sxs-lookup"><span data-stu-id="75488-202">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="75488-203">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-203">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="75488-204">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-204">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="75488-205">Atrybut `extra` składnika `Child` jest ustawiony na prawo od `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="75488-205">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="75488-206">`<div>` renderowanego składnika `Parent` zawiera `extra="5"`, gdy jest on przeszukiwany za pomocą dodatkowego atrybutu, ponieważ atrybuty są przetwarzane od prawej do lewej (od ostatni do pierwszego):</span><span class="sxs-lookup"><span data-stu-id="75488-206">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="75488-207">W poniższym przykładzie porządek `extra` i `@attributes` jest odwrócony w `<div>`składnika `Child`:</span><span class="sxs-lookup"><span data-stu-id="75488-207">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="75488-208">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-208">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="75488-209">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-209">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="75488-210">Renderowane `<div>` w składniku `Parent` zawiera `extra="10"` w przypadku przechodzenia przez dodatkowy atrybut:</span><span class="sxs-lookup"><span data-stu-id="75488-210">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="75488-211">Przechwyć odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="75488-211">Capture references to components</span></span>

<span data-ttu-id="75488-212">Odwołania do składników zapewniają sposób odwoływania się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`.</span><span class="sxs-lookup"><span data-stu-id="75488-212">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="75488-213">Aby przechwycić odwołanie do składnika:</span><span class="sxs-lookup"><span data-stu-id="75488-213">To capture a component reference:</span></span>

* <span data-ttu-id="75488-214">Dodaj atrybut [`@ref`](xref:mvc/views/razor#ref) do składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="75488-214">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="75488-215">Zdefiniuj pole z tym samym typem co składnik podrzędny.</span><span class="sxs-lookup"><span data-stu-id="75488-215">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="75488-216">Gdy składnik jest renderowany, pole `_loginDialog` jest wypełniane za pomocą `MyLoginDialog` podrzędnego wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-216">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="75488-217">Następnie można wywołać metody .NET w wystąpieniu składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-217">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75488-218">Zmienna `_loginDialog` jest wypełniana tylko po wyrenderowaniu składnika, a jego wyjście zawiera element `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="75488-218">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="75488-219">Do tego momentu nie ma niczego do odwołania.</span><span class="sxs-lookup"><span data-stu-id="75488-219">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="75488-220">Aby manipulować odwołaniami do składników po zakończeniu renderowania składnika, należy użyć [metody OnAfterRenderAsync lub OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="75488-220">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="75488-221">Aby odwoływać się do składników w pętli, zobacz [przechwytywanie odwołań do wielu podobnych składników podrzędnych (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span><span class="sxs-lookup"><span data-stu-id="75488-221">To reference components in a loop, see [Capture references to multiple similar child-components (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span></span>

<span data-ttu-id="75488-222">Podczas przechwytywania odwołań do składników użycie podobnej składni do [przechwytywania odwołań do elementów](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements)nie jest funkcją międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="75488-222">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="75488-223">Odwołania do składników nie są przesyłane do kodu JavaScript,&mdash;są używane tylko w kodzie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="75488-223">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="75488-224">**Nie** należy używać odwołań do składników do mutacji stanu składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="75488-224">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="75488-225">Zamiast tego należy używać zwykłych parametrów deklaratywnych do przekazywania danych do składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="75488-225">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="75488-226">Użycie normalnych parametrów deklaratywnych powoduje, że składniki podrzędne, które automatycznie uruchamiają się w prawidłowym czasie.</span><span class="sxs-lookup"><span data-stu-id="75488-226">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="75488-227">Wywołaj metody składnika zewnętrznie, aby zaktualizować stan</span><span class="sxs-lookup"><span data-stu-id="75488-227">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="75488-228"> używa `SynchronizationContext` w celu wymuszenia pojedynczego wątku logicznego wykonywania.</span><span class="sxs-lookup"><span data-stu-id="75488-228"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="75488-229">W tym `SynchronizationContext`są wykonywane [metody cyklu życia](xref:blazor/lifecycle) składnika i wszystkie wywołania zwrotne zdarzeń, które są wywoływane przez Blazor.</span><span class="sxs-lookup"><span data-stu-id="75488-229">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="75488-230">W przypadku zdarzenia składnika należy zaktualizować na podstawie zdarzenia zewnętrznego, takiego jak czasomierz lub inne powiadomienia, użyj metody `InvokeAsync`, która będzie wysyłana do `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="75488-230">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="75488-231">Rozważmy na przykład *usługę powiadamiania* , która może powiadomić dowolny składnik nasłuchujący zaktualizowanego stanu:</span><span class="sxs-lookup"><span data-stu-id="75488-231">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="75488-232">Zarejestruj `NotifierService` jako singletion:</span><span class="sxs-lookup"><span data-stu-id="75488-232">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="75488-233">W programie Blazor webassembly Zarejestruj usługę w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="75488-233">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="75488-234">W programie Blazor Server Zarejestruj usługę w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="75488-234">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="75488-235">Użyj `NotifierService`, aby zaktualizować składnik:</span><span class="sxs-lookup"><span data-stu-id="75488-235">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="75488-236">W poprzednim przykładzie `NotifierService` wywołuje metodę `OnNotify` składnika poza `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="75488-236">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="75488-237">`InvokeAsync` jest używany do przełączania do poprawnego kontekstu i renderowania kolejki.</span><span class="sxs-lookup"><span data-stu-id="75488-237">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="75488-238">Użyj klucza \@, aby kontrolować zachowywanie elementów i składników</span><span class="sxs-lookup"><span data-stu-id="75488-238">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="75488-239">Podczas renderowania listy elementów lub składników oraz elementów lub składników, które następnie zmieniają się, algorytm różnicowania Blazormusi zdecydować, które z poprzednich elementów lub składników mogą być zachowywane i jak obiekty modelu powinny być mapowane na nie.</span><span class="sxs-lookup"><span data-stu-id="75488-239">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="75488-240">Zwykle ten proces jest automatyczny i można go zignorować, ale istnieją przypadki, w których może być konieczne sterowanie procesem.</span><span class="sxs-lookup"><span data-stu-id="75488-240">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="75488-241">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="75488-241">Consider the following example:</span></span>

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

<span data-ttu-id="75488-242">Zawartość kolekcji `People` może ulec zmianie z wstawionymi, usuniętymi lub z ponownymi zamówieniami.</span><span class="sxs-lookup"><span data-stu-id="75488-242">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="75488-243">Gdy składnik jest przerenderowany, składnik `<DetailsEditor>` może zmienić, aby otrzymywać różne `Details` wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="75488-243">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="75488-244">Może to spowodować bardziej złożone odwzorowanie niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="75488-244">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="75488-245">W niektórych przypadkach odzyskanie może prowadzić do zauważalnych różnic w zachowaniu, takich jak brak fokusu elementu.</span><span class="sxs-lookup"><span data-stu-id="75488-245">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="75488-246">Proces mapowania można kontrolować przy użyciu atrybutu dyrektywy `@key`.</span><span class="sxs-lookup"><span data-stu-id="75488-246">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="75488-247">`@key` powoduje, że algorytm różnicowania gwarantuje zachowywanie elementów lub składników na podstawie wartości klucza:</span><span class="sxs-lookup"><span data-stu-id="75488-247">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="75488-248">W przypadku zmiany kolekcji `People`, algorytm różnicowania zachowuje skojarzenie między wystąpieniami `<DetailsEditor>` i wystąpieniami `person`:</span><span class="sxs-lookup"><span data-stu-id="75488-248">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="75488-249">Jeśli `Person` zostanie usunięty z listy `People`, tylko odpowiednie wystąpienie `<DetailsEditor>` zostanie usunięte z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="75488-249">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="75488-250">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="75488-250">Other instances are left unchanged.</span></span>
* <span data-ttu-id="75488-251">Jeśli na liście zostanie wstawiony `Person`, jedno nowe wystąpienie `<DetailsEditor>` zostanie wstawione do odpowiedniego położenia.</span><span class="sxs-lookup"><span data-stu-id="75488-251">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="75488-252">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="75488-252">Other instances are left unchanged.</span></span>
* <span data-ttu-id="75488-253">W przypadku ponownego uporządkowania wpisów `Person` odpowiednie wystąpienia `<DetailsEditor>` są zachowywane i uporządkowane w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="75488-253">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="75488-254">W niektórych scenariuszach użycie `@key` minimalizuje złożoność operacji renderowania i pozwala uniknąć potencjalnych problemów związanych z zmianami stanowymi modelu DOM, takich jak pozycja fokusu.</span><span class="sxs-lookup"><span data-stu-id="75488-254">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75488-255">Klucze są lokalne dla każdego elementu kontenera lub składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-255">Keys are local to each container element or component.</span></span> <span data-ttu-id="75488-256">Klucze nie są porównywane globalnie w całym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="75488-256">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="75488-257">Kiedy używać klucza \@</span><span class="sxs-lookup"><span data-stu-id="75488-257">When to use \@key</span></span>

<span data-ttu-id="75488-258">Zazwyczaj warto używać `@key` zawsze, gdy lista jest renderowana (na przykład w bloku `@foreach`), a odpowiednia wartość istnieje do zdefiniowania `@key`.</span><span class="sxs-lookup"><span data-stu-id="75488-258">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="75488-259">Można również użyć `@key`, aby uniemożliwić Blazor zachowywania poddrzewa elementu lub składnika, gdy zmieniany jest obiekt:</span><span class="sxs-lookup"><span data-stu-id="75488-259">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="75488-260">W przypadku zmiany `@currentPerson` dyrektywa `@key` wymusza Blazor odrzucania całego `<div>` i jego obiektów podrzędnych i ponownej kompilacji poddrzewa w interfejsie użytkownika z nowymi elementami i składnikami.</span><span class="sxs-lookup"><span data-stu-id="75488-260">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="75488-261">Może to być przydatne, jeśli zachodzi konieczność zagwarantowania, że stan interfejsu użytkownika nie jest zachowywany po zmianie `@currentPerson`.</span><span class="sxs-lookup"><span data-stu-id="75488-261">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="75488-262">Kiedy nie używać klucza \@</span><span class="sxs-lookup"><span data-stu-id="75488-262">When not to use \@key</span></span>

<span data-ttu-id="75488-263">Różnica między `@key`ami jest kosztem wydajności.</span><span class="sxs-lookup"><span data-stu-id="75488-263">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="75488-264">Koszt wydajności nie jest duży, ale Określ `@key` tylko wtedy, gdy kontrolowanie reguł utrwalania elementów i składników korzyści dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="75488-264">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="75488-265">Nawet jeśli `@key` nie jest używany, Blazor zachowuje elementy podrzędne i wystąpienia składników tak dużo, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="75488-265">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="75488-266">Jedyną zaletą korzystania z `@key` jest kontrola nad *sposobem* , w jaki wystąpienia modelu są mapowane na zachowane wystąpienia składników, zamiast algorytmu różnicowego, wybierając mapowanie.</span><span class="sxs-lookup"><span data-stu-id="75488-266">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="75488-267">Wartości, które mają być używane dla klucza \@</span><span class="sxs-lookup"><span data-stu-id="75488-267">What values to use for \@key</span></span>

<span data-ttu-id="75488-268">Ogólnie rzecz biorąc, warto podać jeden z następujących rodzajów wartości dla `@key`:</span><span class="sxs-lookup"><span data-stu-id="75488-268">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="75488-269">Wystąpienia obiektów modelu (na przykład wystąpienie `Person` jak w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="75488-269">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="75488-270">Zapewnia to zachowywanie na podstawie równości odwołań do obiektów.</span><span class="sxs-lookup"><span data-stu-id="75488-270">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="75488-271">Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`lub `Guid`).</span><span class="sxs-lookup"><span data-stu-id="75488-271">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="75488-272">Upewnij się, że wartości używane dla `@key` nie kolidują.</span><span class="sxs-lookup"><span data-stu-id="75488-272">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="75488-273">Jeśli w tym samym elemencie nadrzędnym zostaną wykryte wartości powodujące konflikt, Blazor zgłasza wyjątek, ponieważ nie może on w sposób jednoznaczny mapować starych elementów lub składników na nowe elementy lub składniki.</span><span class="sxs-lookup"><span data-stu-id="75488-273">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="75488-274">Używaj tylko odrębnych wartości, takich jak wystąpienia obiektów lub wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="75488-274">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="75488-275">Obsługa klasy częściowej</span><span class="sxs-lookup"><span data-stu-id="75488-275">Partial class support</span></span>

<span data-ttu-id="75488-276">Składniki Razor są generowane jako klasy częściowe.</span><span class="sxs-lookup"><span data-stu-id="75488-276">Razor components are generated as partial classes.</span></span> <span data-ttu-id="75488-277">Składniki Razor są tworzone przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="75488-277">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="75488-278">C#kod jest zdefiniowany w bloku [`@code`](xref:mvc/views/razor#code) przy użyciu znaczników HTML i kodu Razor w pojedynczym pliku.</span><span class="sxs-lookup"><span data-stu-id="75488-278">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="75488-279">Szablony Blazor definiują ich składniki Razor przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="75488-279">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="75488-280">C#kod jest umieszczany w pliku związanym z kodem zdefiniowanym jako Klasa częściowa.</span><span class="sxs-lookup"><span data-stu-id="75488-280">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="75488-281">Poniższy przykład pokazuje domyślny składnik `Counter` z blokiem `@code` w aplikacji wygenerowanej na podstawie szablonu Blazor.</span><span class="sxs-lookup"><span data-stu-id="75488-281">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="75488-282">Znaczniki HTML, kod Razor i C# kod są w tym samym pliku:</span><span class="sxs-lookup"><span data-stu-id="75488-282">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="75488-283">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-283">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="75488-284">Składnik `Counter` można również utworzyć przy użyciu pliku związanego z kodem z klasą częściową:</span><span class="sxs-lookup"><span data-stu-id="75488-284">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="75488-285">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-285">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="75488-286">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="75488-286">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="75488-287">W razie potrzeby dodaj wszystkie wymagane przestrzenie nazw do pliku klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="75488-287">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="75488-288">Typowe przestrzenie nazw używane przez składniki Razor obejmują:</span><span class="sxs-lookup"><span data-stu-id="75488-288">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="75488-289">Określ klasę bazową</span><span class="sxs-lookup"><span data-stu-id="75488-289">Specify a base class</span></span>

<span data-ttu-id="75488-290">Dyrektywa [`@inherits`](xref:mvc/views/razor#inherits) może służyć do określania klasy bazowej dla składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-290">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="75488-291">Poniższy przykład pokazuje, jak składnik może dziedziczyć klasę bazową `BlazorRocksBase`, aby zapewnić właściwości i metody składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-291">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="75488-292">Klasa bazowa powinna pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="75488-292">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="75488-293">*Strony/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="75488-293">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="75488-294">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="75488-294">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a><span data-ttu-id="75488-295">Określ atrybut</span><span class="sxs-lookup"><span data-stu-id="75488-295">Specify an attribute</span></span>

<span data-ttu-id="75488-296">Atrybuty można określić w składnikach Razor za pomocą dyrektywy [`@attribute`](xref:mvc/views/razor#attribute) .</span><span class="sxs-lookup"><span data-stu-id="75488-296">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="75488-297">Poniższy przykład stosuje atrybut `[Authorize]` do klasy składnika:</span><span class="sxs-lookup"><span data-stu-id="75488-297">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="75488-298">Importuj składniki</span><span class="sxs-lookup"><span data-stu-id="75488-298">Import components</span></span>

<span data-ttu-id="75488-299">Przestrzeń nazw składnika utworzone przy użyciu Razor jest oparta na (w kolejności priorytetu):</span><span class="sxs-lookup"><span data-stu-id="75488-299">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="75488-300">Wyznaczanie [`@namespace`](xref:mvc/views/razor#namespace) w znaczniku pliku*Razor (`@namespace BlazorSample.MyNamespace`* ).</span><span class="sxs-lookup"><span data-stu-id="75488-300">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="75488-301">`RootNamespace` projektu w pliku projektu (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="75488-301">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="75488-302">Nazwa projektu, pobrana z nazwy pliku projektu ( *. csproj*) i ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="75488-302">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="75488-303">Na przykład struktura rozpoznaje *{Project root}/Pages/index.Razor* (*BlazorSample. csproj*) do przestrzeni nazw `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="75488-303">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="75488-304">Składniki przestrzegają C# reguł powiązań nazw.</span><span class="sxs-lookup"><span data-stu-id="75488-304">Components follow C# name binding rules.</span></span> <span data-ttu-id="75488-305">Dla składnika `Index` w tym przykładzie składniki należące do zakresu są wszystkich składników:</span><span class="sxs-lookup"><span data-stu-id="75488-305">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="75488-306">W tym samym folderze *strony*.</span><span class="sxs-lookup"><span data-stu-id="75488-306">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="75488-307">Składniki w katalogu głównym projektu, które nie określają jawnie innej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="75488-307">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="75488-308">Składniki zdefiniowane w innej przestrzeni nazw są wprowadzane do zakresu za pomocą dyrektywy [`@using`](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="75488-308">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="75488-309">Jeśli inny składnik, `NavMenu.razor`, istnieje w *BlazorSample/Shared/* folder, składnik może być używany w `Index.razor` z następującą instrukcją `@using`:</span><span class="sxs-lookup"><span data-stu-id="75488-309">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="75488-310">Do składników można także odwoływać się za pomocą ich w pełni kwalifikowanych nazw, które nie wymagają dyrektywy [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="75488-310">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="75488-311">Kwalifikacja `global::` nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="75488-311">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="75488-312">Importowanie składników za pomocą instrukcji `using` z aliasami (na przykład `@using Foo = Bar`) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="75488-312">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="75488-313">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="75488-313">Partially qualified names aren't supported.</span></span> <span data-ttu-id="75488-314">Na przykład dodawanie `@using BlazorSample` i odwoływanie się do `NavMenu.razor` za pomocą `<Shared.NavMenu></Shared.NavMenu>` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="75488-314">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="75488-315">Warunkowe atrybuty elementu HTML</span><span class="sxs-lookup"><span data-stu-id="75488-315">Conditional HTML element attributes</span></span>

<span data-ttu-id="75488-316">Atrybuty elementu HTML są warunkowo renderowane na podstawie wartości .NET.</span><span class="sxs-lookup"><span data-stu-id="75488-316">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="75488-317">Jeśli wartość jest `false` lub `null`, atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="75488-317">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="75488-318">Jeśli wartość jest `true`, atrybut jest renderowany jako zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="75488-318">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="75488-319">W poniższym przykładzie `IsCompleted` określa, czy `checked` jest renderowany w znacznikach elementu:</span><span class="sxs-lookup"><span data-stu-id="75488-319">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="75488-320">Jeśli `IsCompleted` jest `true`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="75488-320">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="75488-321">Jeśli `IsCompleted` jest `false`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="75488-321">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="75488-322">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="75488-322">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="75488-323">Niektóre atrybuty HTML, takie jak [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), nie działają prawidłowo, gdy typem .net jest `bool`.</span><span class="sxs-lookup"><span data-stu-id="75488-323">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="75488-324">W tych przypadkach Użyj typu `string` zamiast `bool`.</span><span class="sxs-lookup"><span data-stu-id="75488-324">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="75488-325">Nieprzetworzony kod HTML</span><span class="sxs-lookup"><span data-stu-id="75488-325">Raw HTML</span></span>

<span data-ttu-id="75488-326">Ciągi są zwykle renderowane przy użyciu węzłów tekstowych DOM, co oznacza, że wszystkie znaczniki, które mogą zawierać, są ignorowane i traktowane jako tekst literału.</span><span class="sxs-lookup"><span data-stu-id="75488-326">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="75488-327">Aby renderować nieprzetworzony kod HTML, zawiń zawartość HTML w `MarkupString` wartość.</span><span class="sxs-lookup"><span data-stu-id="75488-327">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="75488-328">Wartość jest analizowana jako plik HTML lub SVG i wstawiona do modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="75488-328">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="75488-329">Renderowanie nieprzetworzonego kodu HTML zbudowanego z dowolnego niezaufanego źródła stanowi **zagrożenie bezpieczeństwa** i należy je unikać!</span><span class="sxs-lookup"><span data-stu-id="75488-329">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="75488-330">Poniższy przykład ilustruje użycie typu `MarkupString`, aby dodać blok statycznej zawartości HTML do renderowanego danych wyjściowych składnika:</span><span class="sxs-lookup"><span data-stu-id="75488-330">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="75488-331">Wartości kaskadowe i parametry</span><span class="sxs-lookup"><span data-stu-id="75488-331">Cascading values and parameters</span></span>

<span data-ttu-id="75488-332">W niektórych scenariuszach nie można przepływać danych z składnika nadrzędnego do składnika potomnego przy użyciu [parametrów składnika](#component-parameters), zwłaszcza gdy istnieje kilka warstw składników.</span><span class="sxs-lookup"><span data-stu-id="75488-332">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="75488-333">Wartości kaskadowe i parametry rozwiązują ten problem, zapewniając wygodną metodę dla składnika nadrzędnego, aby zapewnić wartość wszystkim jej składnikom potomnym.</span><span class="sxs-lookup"><span data-stu-id="75488-333">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="75488-334">Kaskadowe wartości i parametry również zapewniają podejście do współrzędnych składników.</span><span class="sxs-lookup"><span data-stu-id="75488-334">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="75488-335">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="75488-335">Theme example</span></span>

<span data-ttu-id="75488-336">W poniższym przykładzie z przykładowej aplikacji Klasa `ThemeInfo` określa informacje o motywie, aby przetworzyć hierarchię składników w taki sposób, aby wszystkie przyciski w danej części aplikacji miały ten sam styl.</span><span class="sxs-lookup"><span data-stu-id="75488-336">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="75488-337">*UIThemeClasses/themeinfo wskazuje. cs*:</span><span class="sxs-lookup"><span data-stu-id="75488-337">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="75488-338">Składnik nadrzędny może zapewnić kaskadową wartość przy użyciu składnika wartości kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="75488-338">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="75488-339">Składnik `CascadingValue` zawija poddrzewo hierarchii składników i dostarcza jedną wartość do wszystkich składników w tym poddrzewie.</span><span class="sxs-lookup"><span data-stu-id="75488-339">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="75488-340">Przykładowo aplikacja Przykładowa określa informacje o motywie (`ThemeInfo`) w jednej z układów aplikacji jako parametr kaskadowy dla wszystkich składników, które tworzą treść układu właściwości `@Body`.</span><span class="sxs-lookup"><span data-stu-id="75488-340">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="75488-341">`ButtonClass` ma przypisaną wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="75488-341">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="75488-342">Każdy składnik podrzędny może wykorzystać tę właściwość za pomocą obiektu kaskadowego `ThemeInfo`.</span><span class="sxs-lookup"><span data-stu-id="75488-342">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="75488-343">składnik `CascadingValuesParametersLayout`:</span><span class="sxs-lookup"><span data-stu-id="75488-343">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="75488-344">Aby korzystać z wartości kaskadowych, składniki deklarują kaskadowe parametry przy użyciu atrybutu `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="75488-344">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="75488-345">Wartości kaskadowe są powiązane z parametrami kaskadowymi według typu.</span><span class="sxs-lookup"><span data-stu-id="75488-345">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="75488-346">W przykładowej aplikacji składnik `CascadingValuesParametersTheme` wiąże `ThemeInfo` wartość kaskadową z parametrem kaskadowym.</span><span class="sxs-lookup"><span data-stu-id="75488-346">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="75488-347">Parametr służy do ustawiania klasy CSS dla jednego z przycisków wyświetlanych przez składnik.</span><span class="sxs-lookup"><span data-stu-id="75488-347">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="75488-348">składnik `CascadingValuesParametersTheme`:</span><span class="sxs-lookup"><span data-stu-id="75488-348">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

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
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="75488-349">Aby przetworzyć kaskadowo wiele wartości tego samego typu w ramach tego samego poddrzewa, podaj unikatowy ciąg `Name` do każdego składnika `CascadingValue` i odpowiadający mu `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="75488-349">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="75488-350">W poniższym przykładzie dwa składniki `CascadingValue` są kaskadowo różne wystąpienia `MyCascadingType` według nazwy:</span><span class="sxs-lookup"><span data-stu-id="75488-350">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="75488-351">W składniku potomnym, kaskadowe parametry odbierają swoje wartości z odpowiednich kaskadowych wartości w składniku nadrzędnym według nazwy:</span><span class="sxs-lookup"><span data-stu-id="75488-351">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="75488-352">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="75488-352">TabSet example</span></span>

<span data-ttu-id="75488-353">Parametry kaskadowe umożliwiają również współdziałanie składników w hierarchii składników.</span><span class="sxs-lookup"><span data-stu-id="75488-353">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="75488-354">Rozważmy na przykład następujący przykład *TabSet* w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="75488-354">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="75488-355">Przykładowa aplikacja ma interfejs `ITab`, w którym znajdują się karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="75488-355">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="75488-356">Składnik `CascadingValuesParametersTabSet` używa składnika `TabSet`, który zawiera kilka `Tab` składników:</span><span class="sxs-lookup"><span data-stu-id="75488-356">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="75488-357">Podrzędne składniki `Tab` nie są jawnie przenoszone jako parametry do `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="75488-357">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="75488-358">Zamiast tego podrzędne składniki `Tab` są częścią zawartości podrzędnej `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="75488-358">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="75488-359">Jednak `TabSet` nadal muszą znać każdy składnik `Tab`, aby można było renderować nagłówki i aktywną kartę. Aby umożliwić tę koordynację bez konieczności stosowania dodatkowego kodu, składnik `TabSet` *może sam określić jako wartość kaskadową* , która jest następnie pobierana przez składniki `Tab` potomnych.</span><span class="sxs-lookup"><span data-stu-id="75488-359">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="75488-360">składnik `TabSet`:</span><span class="sxs-lookup"><span data-stu-id="75488-360">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="75488-361">Składniki `Tab` potomne przechwytują `TabSet` zawierający jako parametr kaskadowy, więc składniki `Tab` dodają same do `TabSet` i koordynują, na której karcie jest aktywna.</span><span class="sxs-lookup"><span data-stu-id="75488-361">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="75488-362">składnik `Tab`:</span><span class="sxs-lookup"><span data-stu-id="75488-362">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="75488-363">Szablony Razor</span><span class="sxs-lookup"><span data-stu-id="75488-363">Razor templates</span></span>

<span data-ttu-id="75488-364">Fragmenty renderowania można definiować przy użyciu składni szablonu Razor.</span><span class="sxs-lookup"><span data-stu-id="75488-364">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="75488-365">Szablony Razor są sposobem definiowania fragmentu interfejsu użytkownika i przyjmuje następujący format:</span><span class="sxs-lookup"><span data-stu-id="75488-365">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="75488-366">Poniższy przykład ilustruje sposób określania wartości `RenderFragment` i `RenderFragment<T>` oraz renderowania szablonów bezpośrednio w składniku.</span><span class="sxs-lookup"><span data-stu-id="75488-366">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="75488-367">Fragmenty renderowania mogą być również przekazane jako argumenty do [składników z szablonem](xref:blazor/templated-components).</span><span class="sxs-lookup"><span data-stu-id="75488-367">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="75488-368">Renderowane dane wyjściowe poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="75488-368">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="75488-369">Skalowalne obrazy wektorowe (SVG)</span><span class="sxs-lookup"><span data-stu-id="75488-369">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="75488-370">Ponieważ Blazor renderuje HTML, obrazy obsługiwane przez przeglądarkę, w tym obrazy*SVG (Scalable*Vector Graphics), są obsługiwane za pośrednictwem tagu `<img>`:</span><span class="sxs-lookup"><span data-stu-id="75488-370">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="75488-371">Podobnie Obrazy SVG są obsługiwane w regułach CSS pliku arkusza stylów (*CSS*):</span><span class="sxs-lookup"><span data-stu-id="75488-371">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="75488-372">Jednak wbudowane znaczniki SVG nie są obsługiwane we wszystkich scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="75488-372">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="75488-373">Jeśli umieścisz tag `<svg>` bezpośrednio w pliku składnika ( *. Razor*), podstawowe renderowanie obrazu jest obsługiwane, ale wiele scenariuszy zaawansowanych nie jest jeszcze obsługiwanych.</span><span class="sxs-lookup"><span data-stu-id="75488-373">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="75488-374">Na przykład Tagi `<use>` nie są obecnie przestrzegane i `@bind` nie mogą być używane w przypadku niektórych tagów SVG.</span><span class="sxs-lookup"><span data-stu-id="75488-374">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="75488-375">Oczekujemy, że te ograniczenia są opisane w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="75488-375">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75488-376">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="75488-376">Additional resources</span></span>

* <span data-ttu-id="75488-377"><xref:security/blazor/server> &ndash; zawiera wskazówki dotyczące tworzenia aplikacji Blazor Server, które muszą będą konkurować o z wyczerpaniem zasobów.</span><span class="sxs-lookup"><span data-stu-id="75488-377"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
