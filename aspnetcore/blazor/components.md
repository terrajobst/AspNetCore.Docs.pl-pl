---
title: Tworzenie i używanie składników ASP.NET Core Razor
author: guardrex
description: Dowiedz się, jak tworzyć i używać składników Razor, w tym jak powiązać z danymi, obsługiwać zdarzenia i zarządzać cyklem życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
uid: blazor/components
ms.openlocfilehash: 267a6f5aa96feeecc280238abbef86949750b07e
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317213"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="4ac06-103">Tworzenie i używanie składników ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="4ac06-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="4ac06-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="4ac06-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="4ac06-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ac06-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4ac06-106">aplikacje Blazor są kompilowane przy użyciu *składników*programu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="4ac06-107">Składnik jest niezależnym fragmentem interfejsu użytkownika (UI), takim jak strona, okno dialogowe lub formularz.</span><span class="sxs-lookup"><span data-stu-id="4ac06-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="4ac06-108">Składnik zawiera znaczniki HTML i logikę przetwarzania wymagane do iniekcji danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="4ac06-109">Składniki są elastyczne i lekkie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="4ac06-110">Mogą być zagnieżdżane, ponownie używane i udostępniane między projektami.</span><span class="sxs-lookup"><span data-stu-id="4ac06-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="4ac06-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="4ac06-111">Component classes</span></span>

<span data-ttu-id="4ac06-112">Składniki są zaimplementowane w plikach składników [Razor](xref:mvc/views/razor) ( *. Razor*) przy użyciu kombinacji C# i znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="4ac06-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="4ac06-113">Składnik Blazor jest formalnie określany jako *składnik Razor*.</span><span class="sxs-lookup"><span data-stu-id="4ac06-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="4ac06-114">Nazwa składnika musi rozpoczynać się wielką literą.</span><span class="sxs-lookup"><span data-stu-id="4ac06-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="4ac06-115">Na przykład *MyCoolComponent. Razor* jest prawidłowy, a *MyCoolComponent. Razor* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="4ac06-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="4ac06-116">Interfejs użytkownika dla składnika jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="4ac06-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="4ac06-117">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="4ac06-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="4ac06-118">Po skompilowaniu aplikacji logika znaczników HTML i C# renderowania jest konwertowana na klasę składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="4ac06-119">Nazwa wygenerowanej klasy jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="4ac06-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="4ac06-120">Elementy członkowskie klasy składnika są zdefiniowane w bloku `@code`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="4ac06-121">W bloku `@code` stan składnika (właściwości, pola) jest określany przy użyciu metod obsługi zdarzeń lub definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="4ac06-122">Dozwolony jest więcej niż jeden blok `@code`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="4ac06-123">We wcześniejszych wersjach ASP.NET Core 3,0 bloki `@functions` były używane do tego samego celu co `@code` bloków w składnikach Razor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="4ac06-124">bloki `@functions` nadal działają w składnikach Razor, ale zalecamy używanie bloku `@code` w ASP.NET Core 3,0 w wersji zapoznawczej 6 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="4ac06-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="4ac06-125">Składowe składnika mogą być używane jako część logiki renderowania składnika przy użyciu C# wyrażeń, które zaczynają się od `@`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="4ac06-126">Na przykład C# pole jest renderowane przez utworzenie prefiksu `@` na nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="4ac06-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="4ac06-127">Poniższy przykład szacuje i renderuje:</span><span class="sxs-lookup"><span data-stu-id="4ac06-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="4ac06-128">`_headingFontStyle` wartość właściwości CSS dla `font-style`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="4ac06-129">`_headingText` do zawartości elementu `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="4ac06-130">Po pierwszym wyrenderowaniu składnika składnik generuje jego drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="4ac06-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="4ac06-131"> następnie porównuje nowe drzewo renderowania z poprzednią i zastosuje wszelkie modyfikacje Document Object Model (DOM) przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4ac06-131"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="4ac06-132">Składniki są zwykłymi C# klasami i mogą być umieszczane w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="4ac06-133">Składniki, które generują strony sieci Web, zwykle znajdują się w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="4ac06-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="4ac06-134">Składniki niestronicowe są często umieszczane w folderze *udostępnionym* lub w folderze niestandardowym dodanym do projektu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="4ac06-135">Aby użyć folderu niestandardowego, należy dodać przestrzeń nazw folderu niestandardowego do składnika nadrzędnego lub pliku *_Imports. Razor* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="4ac06-136">Na przykład następująca przestrzeń nazw sprawia, że składniki w folderze *Components* są dostępne, gdy główna przestrzeń nazw aplikacji jest `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="4ac06-137">Integrowanie składników w aplikacjach Razor Pages i MVC</span><span class="sxs-lookup"><span data-stu-id="4ac06-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="4ac06-138">Używaj składników z istniejącymi aplikacjami Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="4ac06-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="4ac06-139">Nie ma potrzeby ponownego zapisywania istniejących stron lub widoków w celu używania składników Razor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="4ac06-140">Gdy strona lub widok są renderowane, składniki są wstępnie renderowane w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="4ac06-141">Aby renderować składnik ze strony lub widoku, użyj pomocnika tagów `Component`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-141">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="4ac06-142">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="4ac06-142">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="4ac06-143">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-143">Is prerendered into the page.</span></span>
* <span data-ttu-id="4ac06-144">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia Blazor aplikacji z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-144">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="4ac06-145">Opis</span><span class="sxs-lookup"><span data-stu-id="4ac06-145">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="4ac06-146">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-146">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="4ac06-147">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-147">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="4ac06-148">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-148">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="4ac06-149">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-149">Output from the component isn't included.</span></span> <span data-ttu-id="4ac06-150">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-150">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="4ac06-151">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="4ac06-151">Renders the component into static HTML.</span></span> |

<span data-ttu-id="4ac06-152">Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true".</span><span class="sxs-lookup"><span data-stu-id="4ac06-152">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="4ac06-153">Składniki nie mogą używać scenariuszy dotyczących widoków i stron, takich jak częściowe widoki i sekcje.</span><span class="sxs-lookup"><span data-stu-id="4ac06-153">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="4ac06-154">Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.</span><span class="sxs-lookup"><span data-stu-id="4ac06-154">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="4ac06-155">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-155">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="4ac06-156">Aby uzyskać więcej informacji na temat sposobu renderowania składników, stanu składnika i pomocnika tagów `Component`, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-156">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

<span data-ttu-id="4ac06-157">Aby renderować składnik ze strony lub widoku, użyj metody pomocnika HTML `RenderComponentAsync<TComponent>`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-157">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
```

<span data-ttu-id="4ac06-158">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="4ac06-158">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="4ac06-159">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-159">Is prerendered into the page.</span></span>
* <span data-ttu-id="4ac06-160">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia Blazor aplikacji z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-160">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="4ac06-161">Opis</span><span class="sxs-lookup"><span data-stu-id="4ac06-161">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="4ac06-162">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-162">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="4ac06-163">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-163">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="4ac06-164">Parametry nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-164">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="4ac06-165">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-165">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="4ac06-166">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-166">Output from the component isn't included.</span></span> <span data-ttu-id="4ac06-167">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-167">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="4ac06-168">Parametry nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-168">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="4ac06-169">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="4ac06-169">Renders the component into static HTML.</span></span> <span data-ttu-id="4ac06-170">Parametry są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-170">Parameters are supported.</span></span> |

<span data-ttu-id="4ac06-171">Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true".</span><span class="sxs-lookup"><span data-stu-id="4ac06-171">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="4ac06-172">Składniki nie mogą używać scenariuszy dotyczących widoków i stron, takich jak częściowe widoki i sekcje.</span><span class="sxs-lookup"><span data-stu-id="4ac06-172">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="4ac06-173">Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.</span><span class="sxs-lookup"><span data-stu-id="4ac06-173">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="4ac06-174">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-174">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="4ac06-175">Aby uzyskać więcej informacji na temat sposobu renderowania składników, stanu składnika i pomocnika języka HTML `RenderComponentAsync`, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-175">For more information on how components are rendered, component state, and the `RenderComponentAsync` HTML Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

## <a name="use-components"></a><span data-ttu-id="4ac06-176">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="4ac06-176">Use components</span></span>

<span data-ttu-id="4ac06-177">Składniki mogą zawierać inne składniki, deklarując je za pomocą składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="4ac06-177">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="4ac06-178">Znaczniki użycia składnika wyglądają jak tag HTML, gdzie nazwa znacznika jest typem składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-178">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="4ac06-179">W powiązaniu atrybutu rozróżniana jest wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="4ac06-179">Attribute binding is case sensitive.</span></span> <span data-ttu-id="4ac06-180">Na przykład `@bind` jest prawidłowy, a `@Bind` jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="4ac06-180">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="4ac06-181">Poniższy znacznik w *indeksie. Razor* renderuje wystąpienie `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-181">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="4ac06-182">*Składniki/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-182">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="4ac06-183">Jeśli składnik zawiera element HTML z wielką literą, która nie jest zgodna z nazwą składnika, jest emitowane ostrzeżenie wskazujące, że element ma nieoczekiwaną nazwę.</span><span class="sxs-lookup"><span data-stu-id="4ac06-183">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="4ac06-184">Dodanie instrukcji `@using` dla przestrzeni nazw składnika sprawia, że składnik jest dostępny, co spowoduje usunięcie tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="4ac06-184">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="4ac06-185">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="4ac06-185">Component parameters</span></span>

<span data-ttu-id="4ac06-186">Składniki mogą mieć *Parametry składnika*, które są zdefiniowane przy użyciu właściwości publicznych w klasie składnika z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-186">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="4ac06-187">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="4ac06-187">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="4ac06-188">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-188">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="4ac06-189">W poniższym przykładzie `ParentComponent` ustawia wartość właściwości `Title` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-189">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="4ac06-190">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-190">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="4ac06-191">Zawartość podrzędna</span><span class="sxs-lookup"><span data-stu-id="4ac06-191">Child content</span></span>

<span data-ttu-id="4ac06-192">Składniki mogą ustawiać zawartość innego składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-192">Components can set the content of another component.</span></span> <span data-ttu-id="4ac06-193">Składnik Assigner zawiera zawartość między tagami, które określają składnik do odbioru.</span><span class="sxs-lookup"><span data-stu-id="4ac06-193">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="4ac06-194">W poniższym przykładzie `ChildComponent` ma właściwość `ChildContent`, która reprezentuje `RenderFragment`, która reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-194">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="4ac06-195">Wartość `ChildContent` jest umieszczana w znacznikach składnika, w którym powinna być renderowana zawartość.</span><span class="sxs-lookup"><span data-stu-id="4ac06-195">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="4ac06-196">Wartość `ChildContent` jest odbierana ze składnika nadrzędnego i renderowany w `panel-body`panelu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-196">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="4ac06-197">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-197">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="4ac06-198">Właściwość, która otrzymuje `RenderFragment` zawartość, musi mieć nazwę `ChildContent` według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-198">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="4ac06-199">Poniższe `ParentComponent` mogą zapewnić zawartość do renderowania `ChildComponent`, umieszczając zawartość wewnątrz tagów `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-199">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="4ac06-200">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-200">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="4ac06-201">Korzystając atrybutów i dowolne parametry</span><span class="sxs-lookup"><span data-stu-id="4ac06-201">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="4ac06-202">Składniki mogą przechwytywać i renderować dodatkowe atrybuty oprócz zadeklarowanych parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-202">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="4ac06-203">Dodatkowe atrybuty mogą być przechwytywane w słowniku, a następnie *splatted* na element, gdy składnik jest renderowany przy użyciu dyrektywy [@attributes](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-203">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="4ac06-204">Ten scenariusz jest przydatny podczas definiowania składnika, który generuje element znaczników, który obsługuje różne dostosowania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-204">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="4ac06-205">Na przykład można żmudnym definiować atrybuty oddzielnie dla `<input>`, które obsługują wiele parametrów.</span><span class="sxs-lookup"><span data-stu-id="4ac06-205">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="4ac06-206">W poniższym przykładzie pierwszy element `<input>` (`id="useIndividualParams"`) używa pojedynczych parametrów składnika, podczas gdy drugi `<input>` elementu (`id="useAttributesDict"`) używa atrybutu korzystając:</span><span class="sxs-lookup"><span data-stu-id="4ac06-206">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="4ac06-207">Typ parametru musi implementować `IEnumerable<KeyValuePair<string, object>>` za pomocą kluczy ciągu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-207">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="4ac06-208">Używanie `IReadOnlyDictionary<string, object>` jest również opcją w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-208">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="4ac06-209">Renderowane `<input>` elementy używające obu metod są identyczne:</span><span class="sxs-lookup"><span data-stu-id="4ac06-209">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="4ac06-210">Aby zaakceptować dowolne atrybuty, zdefiniuj parametr składnika przy użyciu atrybutu `[Parameter]` z właściwością `CaptureUnmatchedValues` ustawioną na `true`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-210">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="4ac06-211">Właściwość `CaptureUnmatchedValues` na `[Parameter]` umożliwia dopasowanie parametru do wszystkich atrybutów, które nie są zgodne z żadnym innym parametrem.</span><span class="sxs-lookup"><span data-stu-id="4ac06-211">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="4ac06-212">Składnik może definiować tylko jeden parametr z `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-212">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="4ac06-213">Typ właściwości używany z `CaptureUnmatchedValues` musi być możliwy do przypisania z `Dictionary<string, object>` z kluczami ciągu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-213">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="4ac06-214">w tym scenariuszu są również dostępne opcje `IEnumerable<KeyValuePair<string, object>>` lub `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-214">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="4ac06-215">Pozycja `@attributes` odnosząca się do pozycji atrybutów elementu jest ważna.</span><span class="sxs-lookup"><span data-stu-id="4ac06-215">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="4ac06-216">Gdy `@attributes` są splatted na elemencie, atrybuty są przetwarzane od prawej do lewej (Ostatnia do).</span><span class="sxs-lookup"><span data-stu-id="4ac06-216">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="4ac06-217">Rozważmy następujący przykład składnika, który zużywa składnik `Child`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-217">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="4ac06-218">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-218">*ParentComponent.razor*:</span></span>

```cshtml
<ChildComponent extra="10" />
```

<span data-ttu-id="4ac06-219">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-219">*ChildComponent.razor*:</span></span>

```cshtml
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="4ac06-220">Atrybut `extra` składnika `Child` jest ustawiony na prawo od `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-220">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="4ac06-221">`<div>` renderowanego składnika `Parent` zawiera `extra="5"`, gdy jest on przeszukiwany za pomocą dodatkowego atrybutu, ponieważ atrybuty są przetwarzane od prawej do lewej (od ostatni do pierwszego):</span><span class="sxs-lookup"><span data-stu-id="4ac06-221">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="4ac06-222">W poniższym przykładzie porządek `extra` i `@attributes` jest odwrócony w `<div>`składnika `Child`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-222">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="4ac06-223">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-223">*ParentComponent.razor*:</span></span>

```cshtml
<ChildComponent extra="10" />
```

<span data-ttu-id="4ac06-224">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-224">*ChildComponent.razor*:</span></span>

```cshtml
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="4ac06-225">Renderowane `<div>` w składniku `Parent` zawiera `extra="10"` w przypadku przechodzenia przez dodatkowy atrybut:</span><span class="sxs-lookup"><span data-stu-id="4ac06-225">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="4ac06-226">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="4ac06-226">Data binding</span></span>

<span data-ttu-id="4ac06-227">Powiązanie danych zarówno ze składnikami, jak i elementami modelu DOM jest realizowane przy użyciu atrybutu [@bind](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="4ac06-227">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="4ac06-228">Poniższy przykład wiąże Właściwość `CurrentValue` z wartością pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="4ac06-228">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="4ac06-229">Gdy pole tekstowe utraci fokus, wartość właściwości jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="4ac06-229">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="4ac06-230">Pole tekstowe jest aktualizowane w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, a nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="4ac06-230">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="4ac06-231">Ponieważ składniki renderują się po wykonaniu kodu procedury obsługi zdarzeń, aktualizacje właściwości są *zwykle* odzwierciedlane w interfejsie użytkownika natychmiast po wyzwoleniu programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4ac06-231">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="4ac06-232">Używanie `@bind` z właściwością `CurrentValue` (`<input @bind="CurrentValue" />`) jest zasadniczo równoważne z następującymi:</span><span class="sxs-lookup"><span data-stu-id="4ac06-232">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="4ac06-233">Gdy składnik jest renderowany, `value` elementu wejściowego pochodzi z właściwości `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-233">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="4ac06-234">Gdy użytkownik wpisze w polu tekstowym i zmieni fokus elementu, zdarzenie `onchange` jest wyzwalane, a właściwość `CurrentValue` jest ustawiona na wartość zmieniona.</span><span class="sxs-lookup"><span data-stu-id="4ac06-234">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="4ac06-235">W rzeczywistości generowanie kodu jest bardziej skomplikowane, ponieważ `@bind` obsługuje przypadki, w których są wykonywane konwersje typów.</span><span class="sxs-lookup"><span data-stu-id="4ac06-235">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="4ac06-236">W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia z atrybutem `value` i obsługuje zmiany przy użyciu zarejestrowanej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="4ac06-236">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="4ac06-237">Oprócz obsługi zdarzeń `onchange` ze składnią `@bind`, właściwość lub pole można powiązać przy użyciu innych zdarzeń, określając atrybut [@bind-value](xref:mvc/views/razor#bind) z `event` parametrem ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="4ac06-237">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="4ac06-238">Poniższy przykład wiąże `CurrentValue` właściwość dla zdarzenia `oninput`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-238">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="4ac06-239">W przeciwieństwie do `onchange`, które jest wyzwalane, gdy element utraci fokus, `oninput` uruchamiany, gdy wartość pola tekstowego ulegnie zmianie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-239">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="4ac06-240">**Wartości niemożliwy do przeanalizowania**</span><span class="sxs-lookup"><span data-stu-id="4ac06-240">**Unparsable values**</span></span>

<span data-ttu-id="4ac06-241">Gdy użytkownik dostarczy wartość niemożliwy do przeanalizowania do elementu powiązanego z danymi, wartość niemożliwy do przeanalizowania jest automatycznie przywracana do poprzedniej wartości po wyzwoleniu zdarzenia bind.</span><span class="sxs-lookup"><span data-stu-id="4ac06-241">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="4ac06-242">Rozważmy następujący scenariusz:</span><span class="sxs-lookup"><span data-stu-id="4ac06-242">Consider the following scenario:</span></span>

* <span data-ttu-id="4ac06-243">Element `<input>` jest powiązany z typem `int` z początkową wartością `123`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-243">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="4ac06-244">Użytkownik aktualizuje wartość elementu do `123.45` na stronie i zmienia fokus elementu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-244">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="4ac06-245">W poprzednim scenariuszu wartość elementu jest przywracana do `123`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-245">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="4ac06-246">Gdy wartość `123.45` zostanie odrzucona na korzyść oryginalnej wartości `123`, użytkownik rozumie, że ich wartość nie została zaakceptowana.</span><span class="sxs-lookup"><span data-stu-id="4ac06-246">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="4ac06-247">Domyślnie powiązanie dotyczy zdarzenia `onchange` elementu (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="4ac06-247">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="4ac06-248">Użyj `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`, aby ustawić inne zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-248">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="4ac06-249">W przypadku zdarzenia `oninput` (`@bind-value:event="oninput"`) następuje rewersja po naciśnięciu klawisza, które wprowadza niemożliwy do przeanalizowania wartość.</span><span class="sxs-lookup"><span data-stu-id="4ac06-249">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="4ac06-250">Podczas określania wartości docelowej zdarzenia `oninput` przy użyciu typu powiązanego z `int`użytkownik nie będzie wpisywać znaku `.`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-250">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="4ac06-251">Znak `.` zostanie natychmiast usunięty, więc użytkownik otrzymuje natychmiastową opinię, że dozwolone są tylko liczby całkowite.</span><span class="sxs-lookup"><span data-stu-id="4ac06-251">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="4ac06-252">Istnieją scenariusze, w których przywrócenie wartości w zdarzeniu `oninput` nie jest idealne, na przykład wtedy, gdy użytkownik powinien mieć możliwość wyczyszczenia wartości `<input>` niemożliwej do przeanalizowania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-252">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="4ac06-253">Alternatywy obejmują:</span><span class="sxs-lookup"><span data-stu-id="4ac06-253">Alternatives include:</span></span>

* <span data-ttu-id="4ac06-254">Nie używaj zdarzenia `oninput`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-254">Don't use the `oninput` event.</span></span> <span data-ttu-id="4ac06-255">Użyj domyślnego zdarzenia `onchange` (`@bind="{PROPERTY OR FIELD}"`), w którym niedozwolona wartość nie zostanie przywrócona, dopóki element nie utraci fokusu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-255">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="4ac06-256">Powiąż z typem dopuszczającym wartość null, takim jak `int?` lub `string`, i podaj logikę niestandardową do obsługi nieprawidłowych wpisów.</span><span class="sxs-lookup"><span data-stu-id="4ac06-256">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="4ac06-257">Użyj [składnika walidacji formularza](xref:blazor/forms-validation), takiego jak `InputNumber` lub `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-257">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="4ac06-258">Składniki walidacji formularza mają wbudowaną obsługę zarządzania nieprawidłowymi danymi wejściowymi.</span><span class="sxs-lookup"><span data-stu-id="4ac06-258">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="4ac06-259">Składniki walidacji formularza:</span><span class="sxs-lookup"><span data-stu-id="4ac06-259">Form validation components:</span></span>
  * <span data-ttu-id="4ac06-260">Zezwalaj użytkownikowi na dostarczenie nieprawidłowych danych wejściowych i otrzymywanie błędów walidacji w skojarzonym `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-260">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="4ac06-261">Wyświetlaj błędy walidacji w interfejsie użytkownika bez zakłócania wprowadzania dodatkowych danych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-261">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="4ac06-262">**Globalizacja**</span><span class="sxs-lookup"><span data-stu-id="4ac06-262">**Globalization**</span></span>

<span data-ttu-id="4ac06-263">wartości `@bind` są sformatowane do wyświetlania i analizowane przy użyciu reguł bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="4ac06-263">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="4ac06-264">Dostęp do bieżącej kultury można uzyskać ze właściwości <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-264">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="4ac06-265">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) jest używany dla następujących typów pól (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="4ac06-265">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="4ac06-266">Poprzednie typy pól:</span><span class="sxs-lookup"><span data-stu-id="4ac06-266">The preceding field types:</span></span>

* <span data-ttu-id="4ac06-267">Są wyświetlane przy użyciu odpowiednich reguł formatowania opartych na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="4ac06-267">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="4ac06-268">Nie może zawierać tekstu o dowolnym formacie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-268">Can't contain free-form text.</span></span>
* <span data-ttu-id="4ac06-269">Podaj charakterystykę interakcji użytkownika w oparciu o implementację przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4ac06-269">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="4ac06-270">Następujące typy pól mają określone wymagania dotyczące formatowania i nie są obecnie obsługiwane przez Blazor, ponieważ nie są obsługiwane przez wszystkie główne przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="4ac06-270">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="4ac06-271">`@bind` obsługuje parametr `@bind:culture`, aby zapewnić <xref:System.Globalization.CultureInfo?displayProperty=fullName> do analizy i formatowania wartości.</span><span class="sxs-lookup"><span data-stu-id="4ac06-271">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="4ac06-272">Określanie kultury nie jest zalecane w przypadku używania typów pól `date` i `number`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-272">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="4ac06-273">`date` i `number` mają wbudowaną obsługę Blazor, która udostępnia wymaganą kulturę.</span><span class="sxs-lookup"><span data-stu-id="4ac06-273">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="4ac06-274">Aby uzyskać informacje na temat sposobu ustawiania kultury użytkownika, zobacz sekcję [Lokalizacja](#localization) .</span><span class="sxs-lookup"><span data-stu-id="4ac06-274">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="4ac06-275">**Ciągi formatujące**</span><span class="sxs-lookup"><span data-stu-id="4ac06-275">**Format strings**</span></span>

<span data-ttu-id="4ac06-276">Powiązanie danych działa z ciągami formatu <xref:System.DateTime> przy użyciu [@bind:format](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="4ac06-276">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="4ac06-277">W tej chwili nie są dostępne inne wyrażenia formatu, takie jak formaty walutowe lub liczbowe.</span><span class="sxs-lookup"><span data-stu-id="4ac06-277">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="4ac06-278">W powyższym kodzie, typ pola `<input>` elementu (`type`) domyślnie `text`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-278">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="4ac06-279">`@bind:format` jest obsługiwana w celu powiązania następujących typów .NET:</span><span class="sxs-lookup"><span data-stu-id="4ac06-279">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="4ac06-280"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="4ac06-280"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="4ac06-281"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="4ac06-281"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="4ac06-282">Atrybut `@bind:format` określa format daty, który ma zostać zastosowany do `value` elementu `<input>`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-282">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="4ac06-283">Format jest również używany do analizowania wartości, gdy wystąpi zdarzenie `onchange`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-283">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="4ac06-284">Określanie formatu dla typu pola `date` nie jest zalecane, ponieważ Blazor ma wbudowaną obsługę formatowania dat.</span><span class="sxs-lookup"><span data-stu-id="4ac06-284">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="4ac06-285">**Parametry składnika**</span><span class="sxs-lookup"><span data-stu-id="4ac06-285">**Component parameters**</span></span>

<span data-ttu-id="4ac06-286">Powiązanie rozpoznaje parametry składnika, gdzie `@bind-{property}` może powiązać wartość właściwości między składnikami.</span><span class="sxs-lookup"><span data-stu-id="4ac06-286">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="4ac06-287">Poniższy składnik podrzędny (`ChildComponent`) ma parametr składnika `Year` i wywołanie zwrotne `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-287">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="4ac06-288">`EventCallback<T>` wyjaśniono w sekcji [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="4ac06-288">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="4ac06-289">Poniższy składnik nadrzędny używa `ChildComponent` i wiąże parametr `ParentYear` z elementu nadrzędnego z parametrem `Year` w składniku podrzędnym:</span><span class="sxs-lookup"><span data-stu-id="4ac06-289">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="4ac06-290">Załadowanie `ParentComponent` powoduje utworzenie następującej adjustacji:</span><span class="sxs-lookup"><span data-stu-id="4ac06-290">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="4ac06-291">Jeśli wartość właściwości `ParentYear` zostanie zmieniona przez wybranie przycisku w `ParentComponent`, zostanie zaktualizowana właściwość `Year` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-291">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="4ac06-292">Nowa wartość `Year` jest renderowana w interfejsie użytkownika podczas renderowania `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-292">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="4ac06-293">Parametr `Year` jest możliwy do powiązania, ponieważ zawiera zdarzenie pomocnika `YearChanged` pasujące do typu parametru `Year`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-293">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="4ac06-294">Zgodnie z Konwencją `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo równoważne zapisowi:</span><span class="sxs-lookup"><span data-stu-id="4ac06-294">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="4ac06-295">Ogólnie rzecz biorąc, właściwość może być powiązana z odpowiednią obsługą zdarzeń przy użyciu atrybutu `@bind-property:event`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-295">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="4ac06-296">Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` przy użyciu następujących dwóch atrybutów:</span><span class="sxs-lookup"><span data-stu-id="4ac06-296">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="4ac06-297">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="4ac06-297">Event handling</span></span>

<span data-ttu-id="4ac06-298">Składniki Razor zapewniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4ac06-298">Razor components provide event handling features.</span></span> <span data-ttu-id="4ac06-299">Dla atrybutu elementu HTML o nazwie `on{EVENT}` (na przykład `onclick` i `onsubmit`) z wartością typu delegata składniki Razor traktują wartość atrybutu jako procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4ac06-299">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="4ac06-300">Nazwa atrybutu jest zawsze sformatowana [@on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="4ac06-300">The attribute's name is always formatted [@on{EVENT}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="4ac06-301">Poniższy kod wywołuje metodę `UpdateHeading`, gdy przycisk zostanie wybrany w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="4ac06-301">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="4ac06-302">Poniższy kod wywołuje metodę `CheckChanged`, gdy pole wyboru zostanie zmienione w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="4ac06-302">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="4ac06-303">Procedury obsługi zdarzeń mogą również być asynchroniczne i zwracać <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-303">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="4ac06-304">Nie ma potrzeby ręcznego wywoływania `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-304">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="4ac06-305">Wyjątki są rejestrowane, gdy wystąpią.</span><span class="sxs-lookup"><span data-stu-id="4ac06-305">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="4ac06-306">W poniższym przykładzie `UpdateHeading` jest wywoływana asynchronicznie po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="4ac06-306">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="4ac06-307">Typy argumentów zdarzeń</span><span class="sxs-lookup"><span data-stu-id="4ac06-307">Event argument types</span></span>

<span data-ttu-id="4ac06-308">W przypadku niektórych zdarzeń dozwolone są typy argumentów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4ac06-308">For some events, event argument types are permitted.</span></span> <span data-ttu-id="4ac06-309">Jeśli dostęp do jednego z tych typów zdarzeń nie jest konieczny, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="4ac06-309">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="4ac06-310">Obsługiwane `EventArgs` przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ac06-310">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="4ac06-311">Wydarzenie</span><span class="sxs-lookup"><span data-stu-id="4ac06-311">Event</span></span>            | <span data-ttu-id="4ac06-312">Klasa</span><span class="sxs-lookup"><span data-stu-id="4ac06-312">Class</span></span>                | <span data-ttu-id="4ac06-313">Zdarzenia i uwagi dotyczące modelu DOM</span><span class="sxs-lookup"><span data-stu-id="4ac06-313">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="4ac06-314">Schowek</span><span class="sxs-lookup"><span data-stu-id="4ac06-314">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="4ac06-315">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="4ac06-315">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="4ac06-316">Przeciągnij</span><span class="sxs-lookup"><span data-stu-id="4ac06-316">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="4ac06-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="4ac06-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="4ac06-318">`DataTransfer` i `DataTransferItem` przechowywać przeciągane dane elementu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-318">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="4ac06-319">Błąd</span><span class="sxs-lookup"><span data-stu-id="4ac06-319">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="4ac06-320">Wydarzenie</span><span class="sxs-lookup"><span data-stu-id="4ac06-320">Event</span></span>            | `EventArgs`          | <span data-ttu-id="4ac06-321">*Ogólne*</span><span class="sxs-lookup"><span data-stu-id="4ac06-321">*General*</span></span><br><span data-ttu-id="4ac06-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="4ac06-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="4ac06-323">*Schowek*</span><span class="sxs-lookup"><span data-stu-id="4ac06-323">*Clipboard*</span></span><br><span data-ttu-id="4ac06-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="4ac06-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="4ac06-325">*Dane wejściowe*</span><span class="sxs-lookup"><span data-stu-id="4ac06-325">*Input*</span></span><br><span data-ttu-id="4ac06-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="4ac06-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="4ac06-327">*Multimedialny*</span><span class="sxs-lookup"><span data-stu-id="4ac06-327">*Media*</span></span><br><span data-ttu-id="4ac06-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="4ac06-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="4ac06-329">Fokus</span><span class="sxs-lookup"><span data-stu-id="4ac06-329">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="4ac06-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="4ac06-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="4ac06-331">Nie obejmuje obsługi `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-331">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="4ac06-332">Dane wejściowe</span><span class="sxs-lookup"><span data-stu-id="4ac06-332">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="4ac06-333">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="4ac06-333">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="4ac06-334">Klawiatury</span><span class="sxs-lookup"><span data-stu-id="4ac06-334">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="4ac06-335">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="4ac06-335">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="4ac06-336">Wskaźnik</span><span class="sxs-lookup"><span data-stu-id="4ac06-336">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="4ac06-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="4ac06-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="4ac06-338">Wskaźnik myszy</span><span class="sxs-lookup"><span data-stu-id="4ac06-338">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="4ac06-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="4ac06-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="4ac06-340">Kółko myszy</span><span class="sxs-lookup"><span data-stu-id="4ac06-340">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="4ac06-341">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="4ac06-341">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="4ac06-342">Postęp</span><span class="sxs-lookup"><span data-stu-id="4ac06-342">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="4ac06-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="4ac06-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="4ac06-344">Dotyk</span><span class="sxs-lookup"><span data-stu-id="4ac06-344">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="4ac06-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="4ac06-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="4ac06-346">`TouchPoint` reprezentuje pojedynczy punkt kontaktu na urządzeniu dotykowym.</span><span class="sxs-lookup"><span data-stu-id="4ac06-346">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="4ac06-347">Aby uzyskać informacje o zachowaniu właściwości i obsłudze zdarzeń zdarzeń w powyższej tabeli, zobacz [klasy EventArgs w źródle odwołań (gałąź ASPNET/AspNetCore Release/3.0)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="4ac06-347">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="4ac06-348">Wyrażenia lambda</span><span class="sxs-lookup"><span data-stu-id="4ac06-348">Lambda expressions</span></span>

<span data-ttu-id="4ac06-349">Wyrażenia lambda mogą być również używane:</span><span class="sxs-lookup"><span data-stu-id="4ac06-349">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="4ac06-350">Często wygodnie jest blisko dodatkowych wartości, na przykład podczas iteracji na zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="4ac06-350">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="4ac06-351">Poniższy przykład tworzy trzy przyciski, z których każdy wywołuje `UpdateHeading` przekazywaniem argumentu zdarzenia (`MouseEventArgs`) i numerem przycisku (`buttonNumber`), po wybraniu w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="4ac06-351">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="4ac06-352">**Nie** używaj zmiennej loop (`i`) w pętli `for` bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="4ac06-352">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="4ac06-353">W przeciwnym razie ta sama zmienna jest używana przez wszystkie wyrażenia lambda, co sprawia, że wartość `i`być taka sama we wszystkich lambdach.</span><span class="sxs-lookup"><span data-stu-id="4ac06-353">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="4ac06-354">Zawsze Przechwytuj wartość w zmiennej lokalnej (`buttonNumber` w poprzednim przykładzie), a następnie użyj jej.</span><span class="sxs-lookup"><span data-stu-id="4ac06-354">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="4ac06-355">EventCallback</span><span class="sxs-lookup"><span data-stu-id="4ac06-355">EventCallback</span></span>

<span data-ttu-id="4ac06-356">Typowym scenariuszem ze składnikami zagnieżdżonymi jest potrzeba uruchomienia metody składnika nadrzędnego w przypadku wystąpienia zdarzenia podrzędnego składnika&mdash;na przykład gdy zdarzenie `onclick` wystąpi w elemencie podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="4ac06-356">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="4ac06-357">Aby uwidocznić zdarzenia między składnikami, użyj `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-357">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="4ac06-358">Składnik nadrzędny może przypisać metodę wywołania zwrotnego do `EventCallback`składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4ac06-358">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="4ac06-359">`ChildComponent` w przykładowej aplikacji pokazuje, jak program obsługi `onclick` przycisku został skonfigurowany tak, aby otrzymać delegata `EventCallback` z `ParentComponent`próbki.</span><span class="sxs-lookup"><span data-stu-id="4ac06-359">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="4ac06-360">`EventCallback` jest wpisana z `MouseEventArgs`, która jest odpowiednia dla zdarzenia `onclick` z urządzenia peryferyjnego:</span><span class="sxs-lookup"><span data-stu-id="4ac06-360">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="4ac06-361">`ParentComponent` ustawia `EventCallback<T>` elementu podrzędnego na `ShowMessage` metody:</span><span class="sxs-lookup"><span data-stu-id="4ac06-361">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="4ac06-362">Gdy przycisk zostanie wybrany w `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-362">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="4ac06-363">Metoda `ShowMessage` `ParentComponent`jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="4ac06-363">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="4ac06-364">`messageText` jest aktualizowany i wyświetlany w `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-364">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="4ac06-365">Wywołanie `StateHasChanged` nie jest wymagane w metodzie wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="4ac06-365">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="4ac06-366">`StateHasChanged` jest automatycznie wywoływana w celu odrenderowania `ParentComponent`, podobnie jak zdarzenia podrzędne wyzwalają ponowne renderowanie składnika w obsłudze zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4ac06-366">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="4ac06-367">`EventCallback` i `EventCallback<T>` Zezwalaj na asynchroniczne Delegaty.</span><span class="sxs-lookup"><span data-stu-id="4ac06-367">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="4ac06-368">`EventCallback<T>` jest silnie wpisana i wymaga określonego typu argumentu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-368">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="4ac06-369">`EventCallback` jest słabo wpisywany i dopuszcza każdy typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-369">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="4ac06-370">Wywołaj `EventCallback` lub `EventCallback<T>` z `InvokeAsync` i oczekują na <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="4ac06-370">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="4ac06-371">Użyj `EventCallback` i `EventCallback<T>` do obsługi zdarzeń i parametrów składnika powiązania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-371">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="4ac06-372">Preferuj silnie wpisaną `EventCallback<T>` przez `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-372">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="4ac06-373">`EventCallback<T>` zapewnia lepszą opinię o błędach dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-373">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="4ac06-374">Podobnie jak w przypadku innych programów obsługi zdarzeń interfejsu użytkownika, określenie parametru zdarzenia jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="4ac06-374">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="4ac06-375">Użyj `EventCallback`, gdy nie zostanie przeniesiona wartość do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="4ac06-375">Use `EventCallback` when there's no value passed to the callback.</span></span>

::: moniker range=">= aspnetcore-3.1"

### <a name="prevent-default-actions"></a><span data-ttu-id="4ac06-376">Zapobiegaj akcjom domyślnym</span><span class="sxs-lookup"><span data-stu-id="4ac06-376">Prevent default actions</span></span>

<span data-ttu-id="4ac06-377">Użyj [@on{Event}:p](xref:mvc/views/razor#oneventpreventdefault) atrybucie dyrektywy reventdefault, aby zapobiec domyślnej akcji dla zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="4ac06-377">Use the [@on{EVENT}:preventDefault](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="4ac06-378">Po wybraniu klucza na urządzeniu wejściowym, gdy fokus elementu znajduje się w polu tekstowym, przeglądarka zwykle wyświetla znak klucza w polu tekstowym.</span><span class="sxs-lookup"><span data-stu-id="4ac06-378">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="4ac06-379">W poniższym przykładzie zachowanie domyślne jest blokowane przez określenie atrybutu dyrektywy `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-379">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="4ac06-380">Licznik przyrostu i klucz **+** nie są przechwytywane do wartości elementu `<input>`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-380">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```cshtml
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="4ac06-381">Określanie atrybutu `@on{EVENT}:preventDefault` bez wartości jest równoważne z `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-381">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="4ac06-382">Wartość atrybutu może również być wyrażeniem.</span><span class="sxs-lookup"><span data-stu-id="4ac06-382">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="4ac06-383">W poniższym przykładzie `_shouldPreventDefault` jest polem `bool` ustawionym na `true` lub `false`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-383">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```cshtml
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="4ac06-384">Procedura obsługi zdarzeń nie jest wymagana, aby zapobiec akcji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="4ac06-384">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="4ac06-385">Procedury obsługi zdarzeń i zapobiegania domyślnym scenariuszom akcji mogą być używane niezależnie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-385">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="4ac06-386">Zatrzymaj propagację zdarzeń</span><span class="sxs-lookup"><span data-stu-id="4ac06-386">Stop event propagation</span></span>

<span data-ttu-id="4ac06-387">Aby zatrzymać propagację zdarzeń, Użyj atrybutu dyrektywy [@on{Event}: stopPropagation](xref:mvc/views/razor#oneventstoppropagation) .</span><span class="sxs-lookup"><span data-stu-id="4ac06-387">Use the [@on{EVENT}:stopPropagation](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="4ac06-388">W poniższym przykładzie, zaznaczając pole wyboru, Zapobiegaj kliknięciu zdarzeń z drugiego elementu podrzędnego `<div>` od propagowania do `<div>`nadrzędnego:</span><span class="sxs-lookup"><span data-stu-id="4ac06-388">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```cshtml
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```

::: moniker-end

## <a name="chained-bind"></a><span data-ttu-id="4ac06-389">Powiązanie łańcuchowe</span><span class="sxs-lookup"><span data-stu-id="4ac06-389">Chained bind</span></span>

<span data-ttu-id="4ac06-390">Typowy scenariusz polega na łańcuchu parametru powiązanego z danymi do elementu strony w danych wyjściowych składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-390">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="4ac06-391">Ten scenariusz jest nazywany *powiązaniem łańcuchowym* , ponieważ wiele poziomów powiązań występuje jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-391">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="4ac06-392">Nie można zaimplementować powiązania łańcuchowego z składnią `@bind` w elemencie strony.</span><span class="sxs-lookup"><span data-stu-id="4ac06-392">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="4ac06-393">Program obsługi zdarzeń i wartość muszą być określone osobno.</span><span class="sxs-lookup"><span data-stu-id="4ac06-393">The event handler and value must be specified separately.</span></span> <span data-ttu-id="4ac06-394">Składnik nadrzędny, jednak może używać składni `@bind`ej z parametrem składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-394">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="4ac06-395">Następujący składnik `PasswordField` (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="4ac06-395">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="4ac06-396">Ustawia wartość elementu `<input>` na Właściwość `Password`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-396">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="4ac06-397">Uwidacznia zmiany właściwości `Password` w składniku nadrzędnym z [EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="4ac06-397">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

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

<span data-ttu-id="4ac06-398">Składnik `PasswordField` jest używany w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="4ac06-398">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="4ac06-399">Aby przeprowadzić sprawdzenia lub błędy pułapki dla hasła w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4ac06-399">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="4ac06-400">Utwórz pole zapasowe dla `Password` (`password` w poniższym przykładowym kodzie).</span><span class="sxs-lookup"><span data-stu-id="4ac06-400">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="4ac06-401">Wykonaj testy lub błędy pułapki w metodzie ustawiającej `Password`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-401">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="4ac06-402">Poniższy przykład przedstawia natychmiastową opinię dla użytkownika, jeśli w wartości hasła jest używana spacja:</span><span class="sxs-lookup"><span data-stu-id="4ac06-402">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

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

## <a name="capture-references-to-components"></a><span data-ttu-id="4ac06-403">Przechwyć odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="4ac06-403">Capture references to components</span></span>

<span data-ttu-id="4ac06-404">Odwołania do składników zapewniają sposób odwoływania się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-404">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="4ac06-405">Aby przechwycić odwołanie do składnika:</span><span class="sxs-lookup"><span data-stu-id="4ac06-405">To capture a component reference:</span></span>

* <span data-ttu-id="4ac06-406">Dodaj atrybut [@ref](xref:mvc/views/razor#ref) do składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4ac06-406">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="4ac06-407">Zdefiniuj pole z tym samym typem co składnik podrzędny.</span><span class="sxs-lookup"><span data-stu-id="4ac06-407">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="4ac06-408">Gdy składnik jest renderowany, pole `loginDialog` jest wypełniane za pomocą `MyLoginDialog` podrzędnego wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-408">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="4ac06-409">Następnie można wywołać metody .NET w wystąpieniu składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-409">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4ac06-410">Zmienna `loginDialog` jest wypełniana tylko po wyrenderowaniu składnika, a jego wyjście zawiera element `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-410">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="4ac06-411">Do tego momentu nie ma niczego do odwołania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-411">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="4ac06-412">Aby manipulować odwołaniami do składników po zakończeniu renderowania składnika, należy użyć [metody OnAfterRenderAsync lub OnAfterRender](#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="4ac06-412">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](#lifecycle-methods).</span></span>

<span data-ttu-id="4ac06-413">Podczas przechwytywania odwołań do składników użycie podobnej składni do [przechwytywania odwołań do elementów](xref:blazor/javascript-interop#capture-references-to-elements)nie jest funkcją [międzyoperacyjności języka JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="4ac06-413">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="4ac06-414">Odwołania do składników nie są przesyłane do kodu JavaScript,&mdash;są używane tylko w kodzie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4ac06-414">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="4ac06-415">**Nie** należy używać odwołań do składników do mutacji stanu składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="4ac06-415">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="4ac06-416">Zamiast tego należy używać zwykłych parametrów deklaratywnych do przekazywania danych do składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="4ac06-416">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="4ac06-417">Użycie normalnych parametrów deklaratywnych powoduje, że składniki podrzędne, które automatycznie uruchamiają się w prawidłowym czasie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-417">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="4ac06-418">Wywołaj metody składnika zewnętrznie, aby zaktualizować stan</span><span class="sxs-lookup"><span data-stu-id="4ac06-418">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="4ac06-419"> używa `SynchronizationContext` w celu wymuszenia pojedynczego wątku logicznego wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-419"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="4ac06-420">W tym `SynchronizationContext`są wykonywane metody cyklu życia składnika i wszystkie wywołania zwrotne zdarzeń, które są wywoływane przez Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-420">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="4ac06-421">W przypadku zdarzenia składnika należy zaktualizować na podstawie zdarzenia zewnętrznego, takiego jak czasomierz lub inne powiadomienia, użyj metody `InvokeAsync`, która będzie wysyłana do `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-421">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="4ac06-422">Rozważmy na przykład *usługę powiadamiania* , która może powiadomić dowolny składnik nasłuchujący zaktualizowanego stanu:</span><span class="sxs-lookup"><span data-stu-id="4ac06-422">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="4ac06-423">Użycie `NotifierService` do zaktualizowania składnika:</span><span class="sxs-lookup"><span data-stu-id="4ac06-423">Usage of the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="4ac06-424">W poprzednim przykładzie `NotifierService` wywołuje metodę `OnNotify` składnika poza `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-424">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="4ac06-425">`InvokeAsync` jest używany do przełączania do poprawnego kontekstu i renderowania kolejki.</span><span class="sxs-lookup"><span data-stu-id="4ac06-425">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="4ac06-426">Użyj klucza \@, aby kontrolować zachowywanie elementów i składników</span><span class="sxs-lookup"><span data-stu-id="4ac06-426">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="4ac06-427">Podczas renderowania listy elementów lub składników oraz elementów lub składników, które następnie zmieniają się, algorytm różnicowania Blazormusi zdecydować, które z poprzednich elementów lub składników mogą być zachowywane i jak obiekty modelu powinny być mapowane na nie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-427">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="4ac06-428">Zwykle ten proces jest automatyczny i można go zignorować, ale istnieją przypadki, w których może być konieczne sterowanie procesem.</span><span class="sxs-lookup"><span data-stu-id="4ac06-428">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="4ac06-429">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="4ac06-429">Consider the following example:</span></span>

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

<span data-ttu-id="4ac06-430">Zawartość kolekcji `People` może ulec zmianie z wstawionymi, usuniętymi lub z ponownymi zamówieniami.</span><span class="sxs-lookup"><span data-stu-id="4ac06-430">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="4ac06-431">Gdy składnik jest przerenderowany, składnik `<DetailsEditor>` może zmienić, aby otrzymywać różne `Details` wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="4ac06-431">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="4ac06-432">Może to spowodować bardziej złożone odwzorowanie niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="4ac06-432">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="4ac06-433">W niektórych przypadkach odzyskanie może prowadzić do zauważalnych różnic w zachowaniu, takich jak brak fokusu elementu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-433">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="4ac06-434">Proces mapowania można kontrolować przy użyciu atrybutu dyrektywy `@key`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-434">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="4ac06-435">`@key` powoduje, że algorytm różnicowania gwarantuje zachowywanie elementów lub składników na podstawie wartości klucza:</span><span class="sxs-lookup"><span data-stu-id="4ac06-435">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="4ac06-436">W przypadku zmiany kolekcji `People`, algorytm różnicowania zachowuje skojarzenie między wystąpieniami `<DetailsEditor>` i wystąpieniami `person`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-436">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="4ac06-437">Jeśli `Person` zostanie usunięty z listy `People`, tylko odpowiednie wystąpienie `<DetailsEditor>` zostanie usunięte z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-437">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="4ac06-438">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="4ac06-438">Other instances are left unchanged.</span></span>
* <span data-ttu-id="4ac06-439">Jeśli na liście zostanie wstawiony `Person`, jedno nowe wystąpienie `<DetailsEditor>` zostanie wstawione do odpowiedniego położenia.</span><span class="sxs-lookup"><span data-stu-id="4ac06-439">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="4ac06-440">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="4ac06-440">Other instances are left unchanged.</span></span>
* <span data-ttu-id="4ac06-441">W przypadku ponownego uporządkowania wpisów `Person` odpowiednie wystąpienia `<DetailsEditor>` są zachowywane i uporządkowane w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-441">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="4ac06-442">W niektórych scenariuszach użycie `@key` minimalizuje złożoność operacji renderowania i pozwala uniknąć potencjalnych problemów związanych z zmianami stanowymi modelu DOM, takich jak pozycja fokusu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-442">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4ac06-443">Klucze są lokalne dla każdego elementu kontenera lub składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-443">Keys are local to each container element or component.</span></span> <span data-ttu-id="4ac06-444">Klucze nie są porównywane globalnie w całym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-444">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="4ac06-445">Kiedy używać klucza \@</span><span class="sxs-lookup"><span data-stu-id="4ac06-445">When to use \@key</span></span>

<span data-ttu-id="4ac06-446">Zazwyczaj warto używać `@key` zawsze, gdy lista jest renderowana (na przykład w bloku `@foreach`), a odpowiednia wartość istnieje do zdefiniowania `@key`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-446">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="4ac06-447">Można również użyć `@key`, aby uniemożliwić Blazor zachowywania poddrzewa elementu lub składnika, gdy zmieniany jest obiekt:</span><span class="sxs-lookup"><span data-stu-id="4ac06-447">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="4ac06-448">W przypadku zmiany `@currentPerson` dyrektywa `@key` wymusza Blazor odrzucania całego `<div>` i jego obiektów podrzędnych i ponownej kompilacji poddrzewa w interfejsie użytkownika z nowymi elementami i składnikami.</span><span class="sxs-lookup"><span data-stu-id="4ac06-448">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="4ac06-449">Może to być przydatne, jeśli zachodzi konieczność zagwarantowania, że stan interfejsu użytkownika nie jest zachowywany po zmianie `@currentPerson`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-449">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="4ac06-450">Kiedy nie używać klucza \@</span><span class="sxs-lookup"><span data-stu-id="4ac06-450">When not to use \@key</span></span>

<span data-ttu-id="4ac06-451">Różnica między `@key`ami jest kosztem wydajności.</span><span class="sxs-lookup"><span data-stu-id="4ac06-451">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="4ac06-452">Koszt wydajności nie jest duży, ale Określ `@key` tylko wtedy, gdy kontrolowanie reguł utrwalania elementów i składników korzyści dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-452">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="4ac06-453">Nawet jeśli `@key` nie jest używany, Blazor zachowuje elementy podrzędne i wystąpienia składników tak dużo, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="4ac06-453">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="4ac06-454">Jedyną zaletą korzystania z `@key` jest kontrola nad *sposobem* , w jaki wystąpienia modelu są mapowane na zachowane wystąpienia składników, zamiast algorytmu różnicowego, wybierając mapowanie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-454">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="4ac06-455">Wartości, które mają być używane dla klucza \@</span><span class="sxs-lookup"><span data-stu-id="4ac06-455">What values to use for \@key</span></span>

<span data-ttu-id="4ac06-456">Ogólnie rzecz biorąc, warto podać jeden z następujących rodzajów wartości dla `@key`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-456">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="4ac06-457">Wystąpienia obiektów modelu (na przykład wystąpienie `Person` jak w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="4ac06-457">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="4ac06-458">Zapewnia to zachowywanie na podstawie równości odwołań do obiektów.</span><span class="sxs-lookup"><span data-stu-id="4ac06-458">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="4ac06-459">Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`lub `Guid`).</span><span class="sxs-lookup"><span data-stu-id="4ac06-459">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="4ac06-460">Upewnij się, że wartości używane dla `@key` nie kolidują.</span><span class="sxs-lookup"><span data-stu-id="4ac06-460">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="4ac06-461">Jeśli w tym samym elemencie nadrzędnym zostaną wykryte wartości powodujące konflikt, Blazor zgłasza wyjątek, ponieważ nie może on w sposób jednoznaczny mapować starych elementów lub składników na nowe elementy lub składniki.</span><span class="sxs-lookup"><span data-stu-id="4ac06-461">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="4ac06-462">Używaj tylko odrębnych wartości, takich jak wystąpienia obiektów lub wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="4ac06-462">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="4ac06-463">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="4ac06-463">Lifecycle methods</span></span>

<span data-ttu-id="4ac06-464">`OnInitializedAsync` i `OnInitialized` wykonywania kodu w celu zainicjowania składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-464">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="4ac06-465">Aby wykonać operację asynchroniczną, użyj `OnInitializedAsync` i słowa kluczowego `await` w operacji:</span><span class="sxs-lookup"><span data-stu-id="4ac06-465">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="4ac06-466">Asynchroniczne działanie podczas inicjowania składnika musi wystąpić w trakcie `OnInitializedAsync`go zdarzenia cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="4ac06-466">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="4ac06-467">W przypadku operacji synchronicznej Użyj `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-467">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="4ac06-468">`OnParametersSetAsync` i `OnParametersSet` są wywoływane, gdy składnik otrzymał parametry z jego elementu nadrzędnego, a wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="4ac06-468">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="4ac06-469">Te metody są wykonywane po zainicjowaniu składnika i za każdym razem, gdy składnik nadrzędny jest renderowany:</span><span class="sxs-lookup"><span data-stu-id="4ac06-469">These methods are executed after component initialization and each time the parent component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="4ac06-470">Asynchroniczne działanie, gdy stosowane są parametry i wartości właściwości w trakcie `OnParametersSetAsync`go zdarzenia cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="4ac06-470">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="4ac06-471">`OnAfterRenderAsync` i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-471">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="4ac06-472">Odwołania do elementów i składników są wypełniane w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-472">Element and component references are populated at this point.</span></span> <span data-ttu-id="4ac06-473">Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.</span><span class="sxs-lookup"><span data-stu-id="4ac06-473">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="4ac06-474">`OnAfterRender` *nie jest wywoływana podczas renderowania na serwerze.*</span><span class="sxs-lookup"><span data-stu-id="4ac06-474">`OnAfterRender` *isn't called when prerendering on the server.*</span></span>

<span data-ttu-id="4ac06-475">`firstRender` parametr `OnAfterRenderAsync` i `OnAfterRender` to:</span><span class="sxs-lookup"><span data-stu-id="4ac06-475">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="4ac06-476">Ustawiana na `true` podczas pierwszego wywołania wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-476">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="4ac06-477">Zapewnia, że operacja inicjowania jest wykonywana tylko raz.</span><span class="sxs-lookup"><span data-stu-id="4ac06-477">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="4ac06-478">Asynchroniczne działanie natychmiast po wyrenderowaniu musi wystąpić w trakcie zdarzenia cyklu życia `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-478">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="4ac06-479">Obsługuj niekompletne akcje asynchroniczne podczas renderowania</span><span class="sxs-lookup"><span data-stu-id="4ac06-479">Handle incomplete async actions at render</span></span>

<span data-ttu-id="4ac06-480">Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogą nie zostać zakończone przed renderowaniem składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-480">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="4ac06-481">Obiekty mogą być `null` lub uzupełniane z danymi podczas wykonywania metody cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="4ac06-481">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="4ac06-482">Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-482">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="4ac06-483">Renderowanie zastępczych elementów interfejsu użytkownika (na przykład komunikatów ładowania) podczas `null`obiektów.</span><span class="sxs-lookup"><span data-stu-id="4ac06-483">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="4ac06-484">W `FetchData` składniku szablonów Blazor `OnInitializedAsync` został zastąpiony asynchronicznie odbierania danych prognozy (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="4ac06-484">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="4ac06-485">Gdy `forecasts` jest `null`, zostanie wyświetlony komunikat ładowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-485">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="4ac06-486">Po `Task` zwrócone przez `OnInitializedAsync` zostanie wykonane, składnik zostanie przerenderowany ze zaktualizowanym stanem.</span><span class="sxs-lookup"><span data-stu-id="4ac06-486">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="4ac06-487">*Strony/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-487">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="4ac06-488">Wykonaj kod przed ustawieniem parametrów</span><span class="sxs-lookup"><span data-stu-id="4ac06-488">Execute code before parameters are set</span></span>

<span data-ttu-id="4ac06-489">`SetParameters` można zastąpić, aby wykonać kod przed ustawieniem parametrów:</span><span class="sxs-lookup"><span data-stu-id="4ac06-489">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="4ac06-490">Jeśli `base.SetParameters` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="4ac06-490">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="4ac06-491">Na przykład parametry przychodzące nie są wymagane do przypisania do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-491">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="4ac06-492">Pomiń odświeżanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="4ac06-492">Suppress refreshing of the UI</span></span>

<span data-ttu-id="4ac06-493">`ShouldRender` można zastąpić, aby pominąć odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-493">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="4ac06-494">Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony.</span><span class="sxs-lookup"><span data-stu-id="4ac06-494">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="4ac06-495">Nawet jeśli `ShouldRender` jest zastępowana, składnik jest zawsze początkowo renderowany.</span><span class="sxs-lookup"><span data-stu-id="4ac06-495">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="4ac06-496">Usuwanie składnika z interfejsem IDisposable</span><span class="sxs-lookup"><span data-stu-id="4ac06-496">Component disposal with IDisposable</span></span>

<span data-ttu-id="4ac06-497">Jeśli składnik implementuje <xref:System.IDisposable>, [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-497">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="4ac06-498">Poniższy składnik używa `@implements IDisposable` i metody `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-498">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

> [!NOTE]
> <span data-ttu-id="4ac06-499">Wywoływanie `StateHasChanged` w `Dispose` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-499">Calling `StateHasChanged` in `Dispose` isn't supported.</span></span> <span data-ttu-id="4ac06-500">`StateHasChanged` mogą być wywoływane w ramach rozłączania modułu renderowania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-500">`StateHasChanged` might be invoked as part of the renderer being torn down.</span></span> <span data-ttu-id="4ac06-501">Żądanie aktualizacji interfejsu użytkownika nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-501">Requesting UI updates at that point isn't supported.</span></span>

## <a name="routing"></a><span data-ttu-id="4ac06-502">Routing</span><span class="sxs-lookup"><span data-stu-id="4ac06-502">Routing</span></span>

<span data-ttu-id="4ac06-503">Routing w Blazor jest realizowany przez dostarczenie szablonu trasy do każdego dostępnego składnika w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-503">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="4ac06-504">Po skompilowaniu pliku Razor z dyrektywą `@page`, wygenerowana Klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określania szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="4ac06-504">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="4ac06-505">W czasie wykonywania router szuka klas składników przy użyciu `RouteAttribute` i renderuje niezależnie składnik ma szablon trasy pasujący do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4ac06-505">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="4ac06-506">Do składnika można zastosować wiele szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="4ac06-506">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="4ac06-507">Poniższy składnik odpowiada na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-507">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="4ac06-508">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="4ac06-508">Route parameters</span></span>

<span data-ttu-id="4ac06-509">Składniki mogą odbierać parametry tras z szablonu trasy dostarczonego w dyrektywie `@page`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-509">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="4ac06-510">Router używa parametrów trasy, aby wypełnić odpowiednie parametry składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-510">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="4ac06-511">*Składnik parametru trasy*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-511">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="4ac06-512">Parametry opcjonalne nie są obsługiwane, więc dwie dyrektywy `@page` są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-512">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="4ac06-513">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="4ac06-513">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="4ac06-514">Druga dyrektywa `@page` przyjmuje parametr trasy `{text}` i przypisuje wartość do właściwości `Text`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-514">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a><span data-ttu-id="4ac06-515">Obsługa klasy częściowej</span><span class="sxs-lookup"><span data-stu-id="4ac06-515">Partial class support</span></span>

<span data-ttu-id="4ac06-516">Składniki Razor są generowane jako klasy częściowe.</span><span class="sxs-lookup"><span data-stu-id="4ac06-516">Razor components are generated as partial classes.</span></span> <span data-ttu-id="4ac06-517">Składniki Razor są tworzone przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="4ac06-517">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="4ac06-518">C#kod jest zdefiniowany w bloku [@code](xref:mvc/views/razor#code) przy użyciu znaczników HTML i kodu Razor w pojedynczym pliku.</span><span class="sxs-lookup"><span data-stu-id="4ac06-518">C# code is defined in an [@code](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="4ac06-519">Szablony Blazor definiują ich składniki Razor przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="4ac06-519">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="4ac06-520">C#kod jest umieszczany w pliku związanym z kodem zdefiniowanym jako Klasa częściowa.</span><span class="sxs-lookup"><span data-stu-id="4ac06-520">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="4ac06-521">Poniższy przykład pokazuje domyślny składnik `Counter` z blokiem `@code` w aplikacji wygenerowanej na podstawie szablonu Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-521">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="4ac06-522">Znaczniki HTML, kod Razor i C# kod są w tym samym pliku:</span><span class="sxs-lookup"><span data-stu-id="4ac06-522">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="4ac06-523">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-523">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="4ac06-524">Składnik `Counter` można również utworzyć przy użyciu pliku związanego z kodem z klasą częściową:</span><span class="sxs-lookup"><span data-stu-id="4ac06-524">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="4ac06-525">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-525">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="4ac06-526">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-526">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

## <a name="specify-a-component-base-class"></a><span data-ttu-id="4ac06-527">Określ klasę bazową składnika</span><span class="sxs-lookup"><span data-stu-id="4ac06-527">Specify a component base class</span></span>

<span data-ttu-id="4ac06-528">Dyrektywa `@inherits` może służyć do określania klasy bazowej dla składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-528">The `@inherits` directive can be used to specify a base class for a component.</span></span>

<span data-ttu-id="4ac06-529">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasę bazową `BlazorRocksBase`, aby zapewnić właściwości i metody składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-529">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="4ac06-530">*Strony/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-530">*Pages/BlazorRocks.razor*:</span></span>

```cshtml
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="4ac06-531">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-531">*BlazorRocksBase.cs*:</span></span>

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

<span data-ttu-id="4ac06-532">Klasa bazowa powinna pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-532">The base class should derive from `ComponentBase`.</span></span>

::: moniker-end

## <a name="import-components"></a><span data-ttu-id="4ac06-533">Importuj składniki</span><span class="sxs-lookup"><span data-stu-id="4ac06-533">Import components</span></span>

<span data-ttu-id="4ac06-534">Przestrzeń nazw składnika utworzone przy użyciu Razor jest oparta na (w kolejności priorytetu):</span><span class="sxs-lookup"><span data-stu-id="4ac06-534">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="4ac06-535">Wyznaczanie [@namespace](xref:mvc/views/razor#namespace) w znaczniku pliku*Razor (`@namespace BlazorSample.MyNamespace`* ).</span><span class="sxs-lookup"><span data-stu-id="4ac06-535">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="4ac06-536">`RootNamespace` projektu w pliku projektu (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="4ac06-536">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="4ac06-537">Nazwa projektu, pobrana z nazwy pliku projektu ( *. csproj*) i ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-537">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="4ac06-538">Na przykład struktura rozpoznaje *{Project root}/Pages/index.Razor* (*BlazorSample. csproj*) do przestrzeni nazw `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-538">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="4ac06-539">Składniki przestrzegają C# reguł powiązań nazw.</span><span class="sxs-lookup"><span data-stu-id="4ac06-539">Components follow C# name binding rules.</span></span> <span data-ttu-id="4ac06-540">Dla składnika `Index` w tym przykładzie składniki należące do zakresu są wszystkich składników:</span><span class="sxs-lookup"><span data-stu-id="4ac06-540">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="4ac06-541">W tym samym folderze *strony*.</span><span class="sxs-lookup"><span data-stu-id="4ac06-541">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="4ac06-542">Składniki w katalogu głównym projektu, które nie określają jawnie innej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="4ac06-542">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="4ac06-543">Składniki zdefiniowane w innej przestrzeni nazw są wprowadzane do zakresu za pomocą dyrektywy [@using](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-543">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="4ac06-544">Jeśli inny składnik, `NavMenu.razor`, istnieje w *BlazorSample/Shared/* folder, składnik może być używany w `Index.razor` z następującą instrukcją `@using`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-544">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="4ac06-545">Do składników można także odwoływać się za pomocą ich w pełni kwalifikowanych nazw, które nie wymagają dyrektywy [@using](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="4ac06-545">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="4ac06-546">Kwalifikacja `global::` nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="4ac06-546">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="4ac06-547">Importowanie składników za pomocą instrukcji `using` z aliasami (na przykład `@using Foo = Bar`) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-547">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="4ac06-548">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-548">Partially qualified names aren't supported.</span></span> <span data-ttu-id="4ac06-549">Na przykład dodawanie `@using BlazorSample` i odwoływanie się do `NavMenu.razor` za pomocą `<Shared.NavMenu></Shared.NavMenu>` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-549">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="4ac06-550">Warunkowe atrybuty elementu HTML</span><span class="sxs-lookup"><span data-stu-id="4ac06-550">Conditional HTML element attributes</span></span>

<span data-ttu-id="4ac06-551">Atrybuty elementu HTML są warunkowo renderowane na podstawie wartości .NET.</span><span class="sxs-lookup"><span data-stu-id="4ac06-551">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="4ac06-552">Jeśli wartość jest `false` lub `null`, atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="4ac06-552">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="4ac06-553">Jeśli wartość jest `true`, atrybut jest renderowany jako zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="4ac06-553">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="4ac06-554">W poniższym przykładzie `IsCompleted` określa, czy `checked` jest renderowany w znacznikach elementu:</span><span class="sxs-lookup"><span data-stu-id="4ac06-554">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="4ac06-555">Jeśli `IsCompleted` jest `true`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="4ac06-555">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="4ac06-556">Jeśli `IsCompleted` jest `false`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="4ac06-556">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="4ac06-557">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-557">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="4ac06-558">Niektóre atrybuty HTML, takie jak [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), nie działają prawidłowo, gdy typem .net jest `bool`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-558">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="4ac06-559">W tych przypadkach Użyj typu `string` zamiast `bool`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-559">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="4ac06-560">Nieprzetworzony kod HTML</span><span class="sxs-lookup"><span data-stu-id="4ac06-560">Raw HTML</span></span>

<span data-ttu-id="4ac06-561">Ciągi są zwykle renderowane przy użyciu węzłów tekstowych DOM, co oznacza, że wszystkie znaczniki, które mogą zawierać, są ignorowane i traktowane jako tekst literału.</span><span class="sxs-lookup"><span data-stu-id="4ac06-561">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="4ac06-562">Aby renderować nieprzetworzony kod HTML, zawiń zawartość HTML w `MarkupString` wartość.</span><span class="sxs-lookup"><span data-stu-id="4ac06-562">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="4ac06-563">Wartość jest analizowana jako plik HTML lub SVG i wstawiona do modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="4ac06-563">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="4ac06-564">Renderowanie nieprzetworzonego kodu HTML zbudowanego z dowolnego niezaufanego źródła stanowi **zagrożenie bezpieczeństwa** i należy je unikać!</span><span class="sxs-lookup"><span data-stu-id="4ac06-564">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="4ac06-565">Poniższy przykład ilustruje użycie typu `MarkupString`, aby dodać blok statycznej zawartości HTML do renderowanego danych wyjściowych składnika:</span><span class="sxs-lookup"><span data-stu-id="4ac06-565">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="4ac06-566">Składniki z szablonami</span><span class="sxs-lookup"><span data-stu-id="4ac06-566">Templated components</span></span>

<span data-ttu-id="4ac06-567">Składniki z szablonami są składnikami, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które mogą być następnie używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-567">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="4ac06-568">Składniki z szablonami umożliwiają tworzenie składników wyższego poziomu, które są większe niż zwykłe składniki.</span><span class="sxs-lookup"><span data-stu-id="4ac06-568">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="4ac06-569">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="4ac06-569">A couple of examples include:</span></span>

* <span data-ttu-id="4ac06-570">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka, wierszy i stopki tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ac06-570">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="4ac06-571">Składnik listy, który umożliwia użytkownikowi określenie szablonu do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="4ac06-571">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="4ac06-572">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="4ac06-572">Template parameters</span></span>

<span data-ttu-id="4ac06-573">Składnik szablonu jest definiowany przez określenie co najmniej jednego parametru składnika typu `RenderFragment` lub `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-573">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="4ac06-574">Fragment renderowania reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-574">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="4ac06-575">`RenderFragment<T>` przyjmuje parametr typu, który można określić podczas wywoływania fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-575">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="4ac06-576">składnik `TableTemplate`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-576">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="4ac06-577">W przypadku korzystania z składnika z szablonem parametry szablonu można określić za pomocą elementów podrzędnych, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="4ac06-577">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="4ac06-578">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="4ac06-578">Template context parameters</span></span>

<span data-ttu-id="4ac06-579">Argumenty składnika typu `RenderFragment<T>` przekazane jako elementy mają niejawny parametr o nazwie `context` (na przykład z poprzedniego przykładu kodu, `@context.PetId`), ale można zmienić nazwę parametru przy użyciu atrybutu `Context` elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4ac06-579">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="4ac06-580">W poniższym przykładzie atrybut `Context` elementu `RowTemplate` określa `pet` parametr:</span><span class="sxs-lookup"><span data-stu-id="4ac06-580">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="4ac06-581">Alternatywnie można określić atrybut `Context` dla elementu składnika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-581">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="4ac06-582">Określony atrybut `Context` ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-582">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="4ac06-583">Może to być przydatne, jeśli chcesz określić nazwę parametru zawartości dla niejawnej zawartości podrzędnej (bez żadnego elementu podrzędnego otoki).</span><span class="sxs-lookup"><span data-stu-id="4ac06-583">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="4ac06-584">W poniższym przykładzie atrybut `Context` pojawia się na elemencie `TableTemplate` i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="4ac06-584">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="4ac06-585">Składniki typu rodzajowego</span><span class="sxs-lookup"><span data-stu-id="4ac06-585">Generic-typed components</span></span>

<span data-ttu-id="4ac06-586">Składniki z szablonami są często wpisywane ogólnie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-586">Templated components are often generically typed.</span></span> <span data-ttu-id="4ac06-587">Na przykład ogólny składnik `ListViewTemplate` może służyć do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="4ac06-587">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="4ac06-588">Aby zdefiniować składnik ogólny, użyj dyrektywy [@typeparam](xref:mvc/views/razor#typeparam) , aby określić parametry typu:</span><span class="sxs-lookup"><span data-stu-id="4ac06-588">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="4ac06-589">W przypadku używania składników o typie ogólnym parametr typu jest wnioskowany, jeśli jest to możliwe:</span><span class="sxs-lookup"><span data-stu-id="4ac06-589">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="4ac06-590">W przeciwnym razie parametr typu musi być jawnie określony przy użyciu atrybutu, który jest zgodny z nazwą parametru typu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-590">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="4ac06-591">W poniższym przykładzie `TItem="Pet"` określa typ:</span><span class="sxs-lookup"><span data-stu-id="4ac06-591">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="4ac06-592">Wartości kaskadowe i parametry</span><span class="sxs-lookup"><span data-stu-id="4ac06-592">Cascading values and parameters</span></span>

<span data-ttu-id="4ac06-593">W niektórych scenariuszach nie można przepływać danych z składnika nadrzędnego do składnika potomnego przy użyciu [parametrów składnika](#component-parameters), zwłaszcza gdy istnieje kilka warstw składników.</span><span class="sxs-lookup"><span data-stu-id="4ac06-593">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="4ac06-594">Wartości kaskadowe i parametry rozwiązują ten problem, zapewniając wygodną metodę dla składnika nadrzędnego, aby zapewnić wartość wszystkim jej składnikom potomnym.</span><span class="sxs-lookup"><span data-stu-id="4ac06-594">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="4ac06-595">Kaskadowe wartości i parametry również zapewniają podejście do współrzędnych składników.</span><span class="sxs-lookup"><span data-stu-id="4ac06-595">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="4ac06-596">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="4ac06-596">Theme example</span></span>

<span data-ttu-id="4ac06-597">W poniższym przykładzie z przykładowej aplikacji Klasa `ThemeInfo` określa informacje o motywie, aby przetworzyć hierarchię składników w taki sposób, aby wszystkie przyciski w danej części aplikacji miały ten sam styl.</span><span class="sxs-lookup"><span data-stu-id="4ac06-597">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="4ac06-598">*UIThemeClasses/themeinfo wskazuje. cs*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-598">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="4ac06-599">Składnik nadrzędny może zapewnić kaskadową wartość przy użyciu składnika wartości kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="4ac06-599">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="4ac06-600">Składnik `CascadingValue` zawija poddrzewo hierarchii składników i dostarcza jedną wartość do wszystkich składników w tym poddrzewie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-600">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="4ac06-601">Przykładowo aplikacja Przykładowa określa informacje o motywie (`ThemeInfo`) w jednej z układów aplikacji jako parametr kaskadowy dla wszystkich składników, które tworzą treść układu właściwości `@Body`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-601">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="4ac06-602">`ButtonClass` ma przypisaną wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-602">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="4ac06-603">Każdy składnik podrzędny może wykorzystać tę właściwość za pomocą obiektu kaskadowego `ThemeInfo`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-603">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="4ac06-604">składnik `CascadingValuesParametersLayout`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-604">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="4ac06-605">Aby korzystać z wartości kaskadowych, składniki deklarują kaskadowe parametry przy użyciu atrybutu `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-605">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="4ac06-606">Wartości kaskadowe są powiązane z parametrami kaskadowymi według typu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-606">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="4ac06-607">W przykładowej aplikacji składnik `CascadingValuesParametersTheme` wiąże `ThemeInfo` wartość kaskadową z parametrem kaskadowym.</span><span class="sxs-lookup"><span data-stu-id="4ac06-607">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="4ac06-608">Parametr służy do ustawiania klasy CSS dla jednego z przycisków wyświetlanych przez składnik.</span><span class="sxs-lookup"><span data-stu-id="4ac06-608">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="4ac06-609">składnik `CascadingValuesParametersTheme`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-609">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="4ac06-610">Aby przetworzyć kaskadowo wiele wartości tego samego typu w ramach tego samego poddrzewa, podaj unikatowy ciąg `Name` do każdego składnika `CascadingValue` i odpowiadający mu `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-610">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="4ac06-611">W poniższym przykładzie dwa składniki `CascadingValue` są kaskadowo różne wystąpienia `MyCascadingType` według nazwy:</span><span class="sxs-lookup"><span data-stu-id="4ac06-611">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="4ac06-612">W składniku potomnym, kaskadowe parametry odbierają swoje wartości z odpowiednich kaskadowych wartości w składniku nadrzędnym według nazwy:</span><span class="sxs-lookup"><span data-stu-id="4ac06-612">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="4ac06-613">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="4ac06-613">TabSet example</span></span>

<span data-ttu-id="4ac06-614">Parametry kaskadowe umożliwiają również współdziałanie składników w hierarchii składników.</span><span class="sxs-lookup"><span data-stu-id="4ac06-614">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="4ac06-615">Rozważmy na przykład następujący przykład *TabSet* w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="4ac06-615">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="4ac06-616">Przykładowa aplikacja ma interfejs `ITab`, w którym znajdują się karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="4ac06-616">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="4ac06-617">Składnik `CascadingValuesParametersTabSet` używa składnika `TabSet`, który zawiera kilka `Tab` składników:</span><span class="sxs-lookup"><span data-stu-id="4ac06-617">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="4ac06-618">Podrzędne składniki `Tab` nie są jawnie przenoszone jako parametry do `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-618">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="4ac06-619">Zamiast tego podrzędne składniki `Tab` są częścią zawartości podrzędnej `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-619">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="4ac06-620">Jednak `TabSet` nadal muszą znać każdy składnik `Tab`, aby można było renderować nagłówki i aktywną kartę. Aby umożliwić tę koordynację bez konieczności stosowania dodatkowego kodu, składnik `TabSet` *może sam określić jako wartość kaskadową* , która jest następnie pobierana przez składniki `Tab` potomnych.</span><span class="sxs-lookup"><span data-stu-id="4ac06-620">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="4ac06-621">składnik `TabSet`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-621">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="4ac06-622">Składniki `Tab` potomne przechwytują `TabSet` zawierający jako parametr kaskadowy, więc składniki `Tab` dodają same do `TabSet` i koordynują, na której karcie jest aktywna.</span><span class="sxs-lookup"><span data-stu-id="4ac06-622">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="4ac06-623">składnik `Tab`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-623">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="4ac06-624">Szablony Razor</span><span class="sxs-lookup"><span data-stu-id="4ac06-624">Razor templates</span></span>

<span data-ttu-id="4ac06-625">Fragmenty renderowania można definiować przy użyciu składni szablonu Razor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-625">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="4ac06-626">Szablony Razor są sposobem definiowania fragmentu interfejsu użytkownika i przyjmuje następujący format:</span><span class="sxs-lookup"><span data-stu-id="4ac06-626">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="4ac06-627">Poniższy przykład ilustruje sposób określania wartości `RenderFragment` i `RenderFragment<T>` oraz renderowania szablonów bezpośrednio w składniku.</span><span class="sxs-lookup"><span data-stu-id="4ac06-627">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="4ac06-628">Fragmenty renderowania mogą być również przekazane jako argumenty do [składników z szablonem](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="4ac06-628">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="4ac06-629">Renderowane dane wyjściowe poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="4ac06-629">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="4ac06-630">Ręczna logika RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="4ac06-630">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="4ac06-631">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` udostępnia metody manipulowania składnikami i elementami, w tym ręczne Kompilowanie C# składników w kodzie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-631">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="4ac06-632">Użycie `RenderTreeBuilder` do tworzenia składników jest zaawansowanym scenariuszem.</span><span class="sxs-lookup"><span data-stu-id="4ac06-632">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="4ac06-633">Nieprawidłowo sformułowany składnik (na przykład niezamknięty tag znacznika) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-633">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="4ac06-634">Rozważmy następujący składnik `PetDetails`, który można utworzyć ręcznie w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="4ac06-634">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="4ac06-635">W poniższym przykładzie pętla w metodzie `CreateComponent` generuje trzy składniki `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-635">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="4ac06-636">Podczas wywoływania `RenderTreeBuilder` metod tworzenia składników (`OpenComponent` i `AddAttribute`) numery sekwencji są numerami wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="4ac06-636">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="4ac06-637">Algorytm Blazor różnic polega na numerach sekwencji odpowiadających odrębnym wierszom kodu, nieodrębnym wywołaniu wywołań.</span><span class="sxs-lookup"><span data-stu-id="4ac06-637">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="4ac06-638">Podczas tworzenia składnika przy użyciu metod `RenderTreeBuilder` umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-638">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="4ac06-639">**Użycie obliczenia lub licznika do wygenerowania numeru sekwencji może prowadzić do niskiej wydajności.**</span><span class="sxs-lookup"><span data-stu-id="4ac06-639">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="4ac06-640">Aby uzyskać więcej informacji, zobacz sekcję [numery sekwencji powiązane z numerami wierszy kodu i kolejnością niewykonania](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="4ac06-640">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="4ac06-641">składnik `BuiltContent`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-641">`BuiltContent` component:</span></span>

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

> <span data-ttu-id="4ac06-642">! WYŚWIETLANIA Typy w `Microsoft.AspNetCore.Components.RenderTree` umożliwiają przetwarzanie *wyników* operacji renderowania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-642">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="4ac06-643">Są to wewnętrzne szczegóły implementacji platformy Blazor Framework.</span><span class="sxs-lookup"><span data-stu-id="4ac06-643">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="4ac06-644">Te typy powinny być uznawane za *niestabilne* i mogą ulec zmianie w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="4ac06-644">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="4ac06-645">Numery sekwencji odnoszą się do numerów wierszy kodu, a nie kolejności wykonywania</span><span class="sxs-lookup"><span data-stu-id="4ac06-645">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="4ac06-646">Pliki `.razor` Blazor są zawsze kompilowane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-646">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="4ac06-647">Jest to znakomita korzyść dla `.razor`, ponieważ krok Kompiluj może służyć do iniekcji informacji, które zwiększają wydajność aplikacji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-647">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="4ac06-648">Najważniejszym przykładem tych ulepszeń są *numery sekwencji*.</span><span class="sxs-lookup"><span data-stu-id="4ac06-648">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="4ac06-649">Numery sekwencji wskazują na środowisko uruchomieniowe, które pochodzą z różnych i uporządkowanych wierszy kodu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-649">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="4ac06-650">Środowisko uruchomieniowe używa tych informacji do generowania wydajnych różnic drzewa w czasie liniowym, które są znacznie szybsze niż zwykle jest to możliwe dla algorytmu różnicowego drzewa ogólnego.</span><span class="sxs-lookup"><span data-stu-id="4ac06-650">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="4ac06-651">Rozważmy następujący prosty plik `.razor`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-651">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="4ac06-652">Poprzedni kod kompiluje się w taki sposób, aby wyglądał następująco:</span><span class="sxs-lookup"><span data-stu-id="4ac06-652">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="4ac06-653">Gdy kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="4ac06-653">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="4ac06-654">Sekwencja</span><span class="sxs-lookup"><span data-stu-id="4ac06-654">Sequence</span></span> | <span data-ttu-id="4ac06-655">Typ</span><span class="sxs-lookup"><span data-stu-id="4ac06-655">Type</span></span>      | <span data-ttu-id="4ac06-656">Dane</span><span class="sxs-lookup"><span data-stu-id="4ac06-656">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="4ac06-657">0</span><span class="sxs-lookup"><span data-stu-id="4ac06-657">0</span></span>        | <span data-ttu-id="4ac06-658">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4ac06-658">Text node</span></span> | <span data-ttu-id="4ac06-659">pierwszego</span><span class="sxs-lookup"><span data-stu-id="4ac06-659">First</span></span>  |
| <span data-ttu-id="4ac06-660">1</span><span class="sxs-lookup"><span data-stu-id="4ac06-660">1</span></span>        | <span data-ttu-id="4ac06-661">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4ac06-661">Text node</span></span> | <span data-ttu-id="4ac06-662">Sekunda</span><span class="sxs-lookup"><span data-stu-id="4ac06-662">Second</span></span> |

<span data-ttu-id="4ac06-663">Załóżmy, że `someFlag` `false`, a znaczniki są renderowane ponownie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-663">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="4ac06-664">Tym razem Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="4ac06-664">This time, the builder receives:</span></span>

| <span data-ttu-id="4ac06-665">Sekwencja</span><span class="sxs-lookup"><span data-stu-id="4ac06-665">Sequence</span></span> | <span data-ttu-id="4ac06-666">Typ</span><span class="sxs-lookup"><span data-stu-id="4ac06-666">Type</span></span>       | <span data-ttu-id="4ac06-667">Dane</span><span class="sxs-lookup"><span data-stu-id="4ac06-667">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="4ac06-668">1</span><span class="sxs-lookup"><span data-stu-id="4ac06-668">1</span></span>        | <span data-ttu-id="4ac06-669">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4ac06-669">Text node</span></span>  | <span data-ttu-id="4ac06-670">Sekunda</span><span class="sxs-lookup"><span data-stu-id="4ac06-670">Second</span></span> |

<span data-ttu-id="4ac06-671">Gdy środowisko uruchomieniowe wykonuje różnicę, zobaczy, że element w sekwencji `0` został usunięty, więc generuje następujący skrypt uproszczonej *edycji*:</span><span class="sxs-lookup"><span data-stu-id="4ac06-671">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="4ac06-672">Usuń pierwszy węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-672">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="4ac06-673">Co się stało z błędami w przypadku wygenerowania numerów sekwencyjnych</span><span class="sxs-lookup"><span data-stu-id="4ac06-673">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="4ac06-674">Wyobraź sobie, że została zapisana następująca logika konstruktora drzewa renderowania:</span><span class="sxs-lookup"><span data-stu-id="4ac06-674">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="4ac06-675">Teraz pierwsze dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="4ac06-675">Now, the first output is:</span></span>

| <span data-ttu-id="4ac06-676">Sekwencja</span><span class="sxs-lookup"><span data-stu-id="4ac06-676">Sequence</span></span> | <span data-ttu-id="4ac06-677">Typ</span><span class="sxs-lookup"><span data-stu-id="4ac06-677">Type</span></span>      | <span data-ttu-id="4ac06-678">Dane</span><span class="sxs-lookup"><span data-stu-id="4ac06-678">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="4ac06-679">0</span><span class="sxs-lookup"><span data-stu-id="4ac06-679">0</span></span>        | <span data-ttu-id="4ac06-680">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4ac06-680">Text node</span></span> | <span data-ttu-id="4ac06-681">pierwszego</span><span class="sxs-lookup"><span data-stu-id="4ac06-681">First</span></span>  |
| <span data-ttu-id="4ac06-682">1</span><span class="sxs-lookup"><span data-stu-id="4ac06-682">1</span></span>        | <span data-ttu-id="4ac06-683">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4ac06-683">Text node</span></span> | <span data-ttu-id="4ac06-684">Sekunda</span><span class="sxs-lookup"><span data-stu-id="4ac06-684">Second</span></span> |

<span data-ttu-id="4ac06-685">Ten wynik jest identyczny z poprzednim przypadkiem, dlatego nie istnieją żadne negatywne problemy.</span><span class="sxs-lookup"><span data-stu-id="4ac06-685">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="4ac06-686">`someFlag` jest `false` podczas drugiego renderowania, a dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="4ac06-686">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="4ac06-687">Sekwencja</span><span class="sxs-lookup"><span data-stu-id="4ac06-687">Sequence</span></span> | <span data-ttu-id="4ac06-688">Typ</span><span class="sxs-lookup"><span data-stu-id="4ac06-688">Type</span></span>      | <span data-ttu-id="4ac06-689">Dane</span><span class="sxs-lookup"><span data-stu-id="4ac06-689">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="4ac06-690">0</span><span class="sxs-lookup"><span data-stu-id="4ac06-690">0</span></span>        | <span data-ttu-id="4ac06-691">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="4ac06-691">Text node</span></span> | <span data-ttu-id="4ac06-692">Sekunda</span><span class="sxs-lookup"><span data-stu-id="4ac06-692">Second</span></span> |

<span data-ttu-id="4ac06-693">Tym razem algorytm diff widzi, że pojawiły się *dwie* zmiany, a algorytm generuje następujący skrypt edycji:</span><span class="sxs-lookup"><span data-stu-id="4ac06-693">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="4ac06-694">Zmień wartość pierwszego węzła tekstowego na `Second`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-694">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="4ac06-695">Usuń drugi węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-695">Remove the second text node.</span></span>

<span data-ttu-id="4ac06-696">Generowanie numerów sekwencji utraciło wszystkie przydatne informacje o tym, gdzie `if/else` gałęzie i pętle były obecne w oryginalnym kodzie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-696">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="4ac06-697">Wynikiem tego jest różnica **dwa razy** , tak długo, jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4ac06-697">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="4ac06-698">Jest to prosty przykład.</span><span class="sxs-lookup"><span data-stu-id="4ac06-698">This is a trivial example.</span></span> <span data-ttu-id="4ac06-699">W bardziej realistycznych przypadkach ze złożonymi i głęboko zagnieżdżonymi strukturami, szczególnie w przypadku pętli, koszt wydajności jest bardziej poważny.</span><span class="sxs-lookup"><span data-stu-id="4ac06-699">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="4ac06-700">Zamiast natychmiastowego identyfikowania, które bloki lub gałęzie pętli zostały wstawione lub usunięte, algorytm różnicowy musi reprezentować się w drzewach renderowania i zwykle tworzyć dużo dłużej edytowane skrypty, ponieważ nie są w nim poinformowani o sposobie starych i nowych struktur odnoszą się do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="4ac06-700">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="4ac06-701">Wskazówki i wnioski</span><span class="sxs-lookup"><span data-stu-id="4ac06-701">Guidance and conclusions</span></span>

* <span data-ttu-id="4ac06-702">Wydajność aplikacji ma wpływ na to, że numery sekwencji są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-702">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="4ac06-703">Struktura nie może automatycznie tworzyć własnych numerów sekwencji w czasie wykonywania, ponieważ niezbędne informacje nie istnieją, chyba że są przechwytywane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-703">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="4ac06-704">Nie zapisuj długich bloków ręcznie zaimplementowanej logiki `RenderTreeBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4ac06-704">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="4ac06-705">Preferuj `.razor` pliki i Zezwalaj kompilatorowi na zaradzenie sobie z kolejnymi numerami.</span><span class="sxs-lookup"><span data-stu-id="4ac06-705">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="4ac06-706">Jeśli nie możesz uniknąć ręcznej logiki `RenderTreeBuilder`, Podziel długie bloki kodu na mniejsze fragmenty opakowane w `OpenRegion`/`CloseRegion` wywołania.</span><span class="sxs-lookup"><span data-stu-id="4ac06-706">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="4ac06-707">Każdy region ma własne oddzielne miejsce numerów sekwencyjnych, więc można uruchomić ponownie od zera (lub dowolnego innego numeru) w każdym regionie.</span><span class="sxs-lookup"><span data-stu-id="4ac06-707">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="4ac06-708">Jeśli numery sekwencji są stałee, algorytm diff wymaga tylko zwiększenia wartości sekwencji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-708">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="4ac06-709">Początkowa wartość i przerwy są nieistotne.</span><span class="sxs-lookup"><span data-stu-id="4ac06-709">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="4ac06-710">Jedną z wiarygodnych opcji jest użycie numeru wiersza kodu jako numeru sekwencyjnego lub rozpoczęcie od zera i zwiększenie według wartości lub setek (lub dowolnego preferowanego interwału).</span><span class="sxs-lookup"><span data-stu-id="4ac06-710">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="4ac06-711"> używa numerów sekwencji, podczas gdy inne struktury interfejsu użytkownika rozróżniania drzewa nie są używane.</span><span class="sxs-lookup"><span data-stu-id="4ac06-711"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="4ac06-712">Różnica jest znacznie szybsza, gdy są używane numery sekwencji, a Blazor ma zalety kroku kompilacji, który zajmuje się automatycznie numerami sekwencyjnymi dla deweloperów tworzących pliki *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="4ac06-712">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="4ac06-713">Lokalizacja</span><span class="sxs-lookup"><span data-stu-id="4ac06-713">Localization</span></span>

<span data-ttu-id="4ac06-714">aplikacje serwera Blazor są zlokalizowane przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="4ac06-714">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="4ac06-715">Oprogramowanie pośredniczące wybiera odpowiednią kulturę dla użytkowników żądających zasobów z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-715">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="4ac06-716">Kulturę można ustawić przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="4ac06-716">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="4ac06-717">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="4ac06-717">Cookies</span></span>](#cookies)
* [<span data-ttu-id="4ac06-718">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="4ac06-718">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="4ac06-719">Aby uzyskać więcej informacji i przykładów, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-719">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="4ac06-720">Konfigurowanie konsolidatora do użytku wieloplikowego (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="4ac06-720">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="4ac06-721">Domyślnie konfiguracja konsolidatora Blazordla Blazor aplikacji webassembly umożliwia rozłączenie informacji o danych wielojęzycznych z wyjątkiem lokalizacji lokalnych jawnie żądanych.</span><span class="sxs-lookup"><span data-stu-id="4ac06-721">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="4ac06-722">Aby uzyskać więcej informacji i wskazówek dotyczących kontrolowania zachowania konsolidatora, zobacz <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-722">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="4ac06-723">Cookie</span><span class="sxs-lookup"><span data-stu-id="4ac06-723">Cookies</span></span>

<span data-ttu-id="4ac06-724">Plik cookie kultury lokalizacji może utrzymywać kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-724">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="4ac06-725">Plik cookie jest tworzony przez metodę `OnGet` strony hosta aplikacji (*strony/hosta. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="4ac06-725">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="4ac06-726">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie na kolejnych żądaniach, aby ustawić kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-726">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="4ac06-727">Użycie pliku cookie zapewnia, że połączenie z użyciem protokołu WebSocket może prawidłowo propagować kulturę.</span><span class="sxs-lookup"><span data-stu-id="4ac06-727">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="4ac06-728">Jeśli schematy lokalizacji są oparte na ścieżce URL lub ciągu zapytania, schemat może nie być w stanie współdziałać z usługą WebSockets, więc nie będzie można zachować kultury.</span><span class="sxs-lookup"><span data-stu-id="4ac06-728">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="4ac06-729">W związku z tym zalecanym podejściem jest użycie pliku cookie kultury lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-729">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="4ac06-730">Każda technika może służyć do przypisywania kultury, jeśli kultura jest utrwalona w pliku cookie lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-730">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="4ac06-731">Jeśli aplikacja ma już ustalony schemat lokalizacji dla ASP.NET Core po stronie serwera, Kontynuuj korzystanie z istniejącej infrastruktury lokalizacji aplikacji i Ustaw plik cookie kultury lokalizacji w schemacie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-731">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="4ac06-732">Poniższy przykład pokazuje, jak ustawić bieżącą kulturę w pliku cookie, który może zostać odczytany przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-732">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="4ac06-733">Utwórz plik *Pages/hosta. cshtml. cs* z następującą zawartością w aplikacji Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="4ac06-733">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="4ac06-734">Lokalizacja jest obsługiwana w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4ac06-734">Localization is handled in the app:</span></span>

1. <span data-ttu-id="4ac06-735">Przeglądarka wysyła początkowe żądanie HTTP do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-735">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="4ac06-736">Kultura jest przypisana przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-736">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="4ac06-737">Metoda `OnGet` w *_Host. cshtml. cs* utrzymuje kulturę w pliku cookie jako część odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4ac06-737">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="4ac06-738">Przeglądarka otwiera połączenie WebSocket, aby utworzyć interakcyjną sesję serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-738">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="4ac06-739">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie i przypisuje kulturę.</span><span class="sxs-lookup"><span data-stu-id="4ac06-739">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="4ac06-740">Sesja serwera Blazor rozpoczyna się od poprawnej kultury.</span><span class="sxs-lookup"><span data-stu-id="4ac06-740">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="4ac06-741">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="4ac06-741">Provide UI to choose the culture</span></span>

<span data-ttu-id="4ac06-742">Aby zapewnić interfejs użytkownika, aby umożliwić użytkownikowi wybranie kultury, zalecane jest *podejście oparte na przekierowaniu* .</span><span class="sxs-lookup"><span data-stu-id="4ac06-742">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="4ac06-743">Ten proces jest podobny do tego, co się dzieje w aplikacji sieci Web, gdy użytkownik próbuje uzyskać dostęp do bezpiecznego zasobu&mdash;użytkownik zostanie przekierowany do strony logowania, a następnie przekierowany z powrotem do oryginalnego zasobu.</span><span class="sxs-lookup"><span data-stu-id="4ac06-743">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="4ac06-744">Aplikacja utrzymuje wybraną kulturę użytkownika za pośrednictwem przekierowania do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4ac06-744">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="4ac06-745">Kontroler ustawia wybraną kulturę użytkownika na plik cookie i przekierowuje użytkownika z powrotem do oryginalnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="4ac06-745">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="4ac06-746">Ustanów punkt końcowy HTTP na serwerze, aby ustawić kulturę wybraną przez użytkownika w pliku cookie i wykonać przekierowanie z powrotem do oryginalnego identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="4ac06-746">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="4ac06-747">Użyj wyniku działania `LocalRedirect`, aby zapobiec atakom typu "Open redirect".</span><span class="sxs-lookup"><span data-stu-id="4ac06-747">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="4ac06-748">Aby uzyskać więcej informacji, zobacz <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-748">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="4ac06-749">Poniższy składnik przedstawia przykład sposobu wykonywania wstępnego przekierowania, gdy użytkownik wybierze kulturę:</span><span class="sxs-lookup"><span data-stu-id="4ac06-749">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="4ac06-750">Korzystanie z scenariuszy lokalizacji platformy .NET w aplikacjach Blazor</span><span class="sxs-lookup"><span data-stu-id="4ac06-750">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="4ac06-751">W aplikacjach Blazor dostępne są następujące scenariusze dotyczące lokalizacji i globalizacji platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="4ac06-751">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="4ac06-752">. System zasobów netto</span><span class="sxs-lookup"><span data-stu-id="4ac06-752">.NET's resources system</span></span>
* <span data-ttu-id="4ac06-753">Formatowanie liczb i dat specyficznych dla kultury</span><span class="sxs-lookup"><span data-stu-id="4ac06-753">Culture-specific number and date formatting</span></span>

<span data-ttu-id="4ac06-754">Funkcja `@bind` Blazorwykonuje globalizację opartą na bieżącej kulturze użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ac06-754">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="4ac06-755">Aby uzyskać więcej informacji, zobacz sekcję [powiązanie danych](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="4ac06-755">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="4ac06-756">Obecnie obsługiwane są ograniczone zestawy ASP.NET Core scenariuszy lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="4ac06-756">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="4ac06-757">`IStringLocalizer<>` *jest obsługiwana* w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-757">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="4ac06-758">Lokalizacja `IHtmlLocalizer<>`, `IViewLocalizer<>`i adnotacji danych są ASP.NET Core scenariusze MVC i **nie są obsługiwane** w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="4ac06-758">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="4ac06-759">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="4ac06-759">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="4ac06-760">Skalowalne obrazy wektorowe (SVG)</span><span class="sxs-lookup"><span data-stu-id="4ac06-760">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="4ac06-761">Ponieważ Blazor renderuje HTML, obrazy obsługiwane przez przeglądarkę, w tym obrazy*SVG (Scalable*Vector Graphics), są obsługiwane za pośrednictwem tagu `<img>`:</span><span class="sxs-lookup"><span data-stu-id="4ac06-761">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="4ac06-762">Podobnie Obrazy SVG są obsługiwane w regułach CSS pliku arkusza stylów (*CSS*):</span><span class="sxs-lookup"><span data-stu-id="4ac06-762">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="4ac06-763">Jednak wbudowane znaczniki SVG nie są obsługiwane we wszystkich scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="4ac06-763">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="4ac06-764">Jeśli umieścisz tag `<svg>` bezpośrednio w pliku składnika ( *. Razor*), podstawowe renderowanie obrazu jest obsługiwane, ale wiele scenariuszy zaawansowanych nie jest jeszcze obsługiwanych.</span><span class="sxs-lookup"><span data-stu-id="4ac06-764">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="4ac06-765">Na przykład Tagi `<use>` nie są obecnie przestrzegane i `@bind` nie mogą być używane w przypadku niektórych tagów SVG.</span><span class="sxs-lookup"><span data-stu-id="4ac06-765">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="4ac06-766">Oczekujemy, że te ograniczenia są opisane w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="4ac06-766">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ac06-767">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4ac06-767">Additional resources</span></span>

* <span data-ttu-id="4ac06-768"><xref:security/blazor/server> &ndash; zawiera wskazówki dotyczące tworzenia aplikacji Blazor Server, które muszą będą konkurować o z wyczerpaniem zasobów.</span><span class="sxs-lookup"><span data-stu-id="4ac06-768"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
