---
title: Tworzenie i używanie składników ASP.NET Core Razor
author: guardrex
description: Dowiedz się, jak tworzyć i używać składników Razor, w tym jak powiązać z danymi, obsługiwać zdarzenia i zarządzać cyklem życia składników.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: 0da0d83a4fde7b753a84bf05d3a9284776f2881f
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213353"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="cd982-103">Tworzenie i używanie składników ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="cd982-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="cd982-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="cd982-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="cd982-105">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cd982-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cd982-106">Aplikacje Blazor są kompilowane przy użyciu *składników*programu.</span><span class="sxs-lookup"><span data-stu-id="cd982-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="cd982-107">Składnik jest niezależnym fragmentem interfejsu użytkownika (UI), takim jak strona, okno dialogowe lub formularz.</span><span class="sxs-lookup"><span data-stu-id="cd982-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="cd982-108">Składnik zawiera znaczniki HTML i logikę przetwarzania wymagane do iniekcji danych lub reagowania na zdarzenia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd982-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="cd982-109">Składniki są elastyczne i lekkie.</span><span class="sxs-lookup"><span data-stu-id="cd982-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="cd982-110">Mogą być zagnieżdżane, ponownie używane i udostępniane między projektami.</span><span class="sxs-lookup"><span data-stu-id="cd982-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="cd982-111">Klasy składników</span><span class="sxs-lookup"><span data-stu-id="cd982-111">Component classes</span></span>

<span data-ttu-id="cd982-112">Składniki są zaimplementowane w plikach składników [Razor](xref:mvc/views/razor) ( *. Razor*) przy użyciu kombinacji C# i znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="cd982-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="cd982-113">Składnik w Blazor jest formalnie określany jako *składnik Razor*.</span><span class="sxs-lookup"><span data-stu-id="cd982-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="cd982-114">Nazwa składnika musi rozpoczynać się wielką literą.</span><span class="sxs-lookup"><span data-stu-id="cd982-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="cd982-115">Na przykład *MyCoolComponent. Razor* jest prawidłowy, a *MyCoolComponent. Razor* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="cd982-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="cd982-116">Interfejs użytkownika dla składnika jest definiowany przy użyciu języka HTML.</span><span class="sxs-lookup"><span data-stu-id="cd982-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="cd982-117">Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="cd982-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="cd982-118">Po skompilowaniu aplikacji logika znaczników HTML i C# renderowania jest konwertowana na klasę składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="cd982-119">Nazwa wygenerowanej klasy jest zgodna z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="cd982-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="cd982-120">Elementy członkowskie klasy składnika są zdefiniowane w bloku `@code`.</span><span class="sxs-lookup"><span data-stu-id="cd982-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="cd982-121">W bloku `@code` stan składnika (właściwości, pola) jest określany przy użyciu metod obsługi zdarzeń lub definiowania innej logiki składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="cd982-122">Dozwolony jest więcej niż jeden blok `@code`.</span><span class="sxs-lookup"><span data-stu-id="cd982-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="cd982-123">Składowe składnika mogą być używane jako część logiki renderowania składnika przy użyciu C# wyrażeń, które zaczynają się od `@`.</span><span class="sxs-lookup"><span data-stu-id="cd982-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="cd982-124">Na przykład C# pole jest renderowane przez utworzenie prefiksu `@` na nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="cd982-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="cd982-125">Poniższy przykład szacuje i renderuje:</span><span class="sxs-lookup"><span data-stu-id="cd982-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="cd982-126">`_headingFontStyle` wartość właściwości CSS dla `font-style`.</span><span class="sxs-lookup"><span data-stu-id="cd982-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="cd982-127">`_headingText` do zawartości elementu `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="cd982-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="cd982-128">Po pierwszym wyrenderowaniu składnika składnik generuje jego drzewo renderowania w odpowiedzi na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cd982-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="cd982-129">Blazor następnie porównuje nowe drzewo renderowania z poprzednią i zastosuje wszelkie modyfikacje Document Object Model przeglądarki (DOM).</span><span class="sxs-lookup"><span data-stu-id="cd982-129">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="cd982-130">Składniki są zwykłymi C# klasami i mogą być umieszczane w dowolnym miejscu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="cd982-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="cd982-131">Składniki, które generują strony sieci Web, zwykle znajdują się w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="cd982-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="cd982-132">Składniki niestronicowe są często umieszczane w folderze *udostępnionym* lub w folderze niestandardowym dodanym do projektu.</span><span class="sxs-lookup"><span data-stu-id="cd982-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="cd982-133">Zazwyczaj przestrzeń nazw składnika pochodzi od głównej przestrzeni nazw aplikacji i lokalizacji składnika (folderu) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="cd982-134">Jeśli główna przestrzeń nazw aplikacji jest `BlazorApp` a składnik `Counter` znajduje się w folderze *strony* :</span><span class="sxs-lookup"><span data-stu-id="cd982-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="cd982-135">Przestrzeń nazw składnika `Counter` jest `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="cd982-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="cd982-136">W pełni kwalifikowana nazwa typu składnika jest `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="cd982-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="cd982-137">Aby uzyskać więcej informacji, zobacz sekcję [Importowanie składników](#import-components) .</span><span class="sxs-lookup"><span data-stu-id="cd982-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="cd982-138">Aby użyć folderu niestandardowego, należy dodać przestrzeń nazw folderu niestandardowego do składnika nadrzędnego lub pliku *_Imports. Razor* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="cd982-139">Na przykład następująca przestrzeń nazw sprawia, że składniki w folderze *Components* są dostępne, gdy główna przestrzeń nazw aplikacji jest `BlazorApp`:</span><span class="sxs-lookup"><span data-stu-id="cd982-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="cd982-140">Integrowanie składników w aplikacjach Razor Pages i MVC</span><span class="sxs-lookup"><span data-stu-id="cd982-140">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="cd982-141">Składniki Razor można zintegrować z aplikacjami Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="cd982-141">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="cd982-142">Gdy strona lub widok jest renderowany, składniki mogą być wstępnie renderowane w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="cd982-142">When the page or view is rendered, components can be prerendered at the same time.</span></span>

<span data-ttu-id="cd982-143">Aby przygotować aplikację Razor Pages lub MVC do hostowania składników Razor, postępuj zgodnie ze wskazówkami zawartymi w sekcji *integrowanie składników Razor z aplikacjami Razor Pages i MVC* w artykule <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="cd982-143">To prepare a Razor Pages or MVC app to host Razor components, follow the guidance in the *Integrate Razor components into Razor Pages and MVC apps* section of the <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps> article.</span></span>

<span data-ttu-id="cd982-144">W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do strony/widoku lub pliku *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="cd982-144">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="cd982-145">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cd982-145">In the following example:</span></span>

* <span data-ttu-id="cd982-146">Zmień `MyAppNamespace` na przestrzeń nazw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-146">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="cd982-147">Jeśli folder o nazwie *Components* nie jest używany do przechowywania składników, należy zmienić `Components` do folderu, w którym znajdują się składniki.</span><span class="sxs-lookup"><span data-stu-id="cd982-147">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```csharp
@using MyAppNamespace.Components
```

<span data-ttu-id="cd982-148">Plik *_ViewImports. cshtml* znajduje się w folderze *strony* aplikacji Razor Pages lub folderu *widoki* aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="cd982-148">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="cd982-149">Aby renderować składnik ze strony lub widoku, użyj pomocnika tagów `Component`:</span><span class="sxs-lookup"><span data-stu-id="cd982-149">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="cd982-150">Typ parametru musi być możliwy do serializacji JSON, co oznacza, że typ musi mieć domyślny Konstruktor i właściwości settable.</span><span class="sxs-lookup"><span data-stu-id="cd982-150">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="cd982-151">Na przykład można określić wartość `IncrementAmount`, ponieważ typ `IncrementAmount` jest `int`, który jest typem pierwotnym obsługiwanym przez serializator JSON.</span><span class="sxs-lookup"><span data-stu-id="cd982-151">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="cd982-152">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="cd982-152">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="cd982-153">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="cd982-153">Is prerendered into the page.</span></span>
* <span data-ttu-id="cd982-154">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd982-154">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="cd982-155">Opis</span><span class="sxs-lookup"><span data-stu-id="cd982-155">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="cd982-156">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-156">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="cd982-157">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-157">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="cd982-158">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-158">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="cd982-159">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="cd982-159">Output from the component isn't included.</span></span> <span data-ttu-id="cd982-160">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-160">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="cd982-161">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="cd982-161">Renders the component into static HTML.</span></span> |

<span data-ttu-id="cd982-162">Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true".</span><span class="sxs-lookup"><span data-stu-id="cd982-162">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="cd982-163">Składniki nie mogą używać scenariuszy dotyczących widoków i stron, takich jak częściowe widoki i sekcje.</span><span class="sxs-lookup"><span data-stu-id="cd982-163">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="cd982-164">Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.</span><span class="sxs-lookup"><span data-stu-id="cd982-164">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="cd982-165">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cd982-165">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="cd982-166">Aby uzyskać więcej informacji na temat sposobu renderowania składników, stanu składnika i pomocnika tagów `Component`, zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="cd982-166">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>

## <a name="tag-helpers-arent-used-in-components"></a><span data-ttu-id="cd982-167">Pomocnicy tagów nie są używani w składnikach</span><span class="sxs-lookup"><span data-stu-id="cd982-167">Tag Helpers aren't used in components</span></span>

<span data-ttu-id="cd982-168">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) nie są obsługiwani w składnikach Razor (pliki *. Razor* ).</span><span class="sxs-lookup"><span data-stu-id="cd982-168">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="cd982-169">Aby zapewnić funkcje podobne do pomocnika tagów w Blazor, należy utworzyć składnik o tej samej funkcji, co pomocnik tagów i użyć składnika zamiast.</span><span class="sxs-lookup"><span data-stu-id="cd982-169">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="cd982-170">Używanie składników</span><span class="sxs-lookup"><span data-stu-id="cd982-170">Use components</span></span>

<span data-ttu-id="cd982-171">Składniki mogą zawierać inne składniki, deklarując je za pomocą składni elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="cd982-171">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="cd982-172">Znaczniki użycia składnika wyglądają jak tag HTML, gdzie nazwa znacznika jest typem składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-172">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="cd982-173">W powiązaniu atrybutu rozróżniana jest wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="cd982-173">Attribute binding is case sensitive.</span></span> <span data-ttu-id="cd982-174">Na przykład `@bind` jest prawidłowy, a `@Bind` jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="cd982-174">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="cd982-175">Poniższy znacznik w *indeksie. Razor* renderuje wystąpienie `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="cd982-175">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="cd982-176">*Składniki/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-176">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="cd982-177">Jeśli składnik zawiera element HTML z wielką literą, która nie jest zgodna z nazwą składnika, jest emitowane ostrzeżenie wskazujące, że element ma nieoczekiwaną nazwę.</span><span class="sxs-lookup"><span data-stu-id="cd982-177">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="cd982-178">Dodanie instrukcji `@using` dla przestrzeni nazw składnika sprawia, że składnik jest dostępny, co spowoduje usunięcie tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="cd982-178">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="cd982-179">Parametry składnika</span><span class="sxs-lookup"><span data-stu-id="cd982-179">Component parameters</span></span>

<span data-ttu-id="cd982-180">Składniki mogą mieć *Parametry składnika*, które są zdefiniowane przy użyciu właściwości publicznych w klasie składnika z atrybutem `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="cd982-180">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="cd982-181">Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="cd982-181">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="cd982-182">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-182">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="cd982-183">W poniższym przykładzie z przykładowej aplikacji `ParentComponent` ustawia wartość właściwości `Title` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="cd982-183">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="cd982-184">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-184">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="child-content"></a><span data-ttu-id="cd982-185">Zawartość podrzędna</span><span class="sxs-lookup"><span data-stu-id="cd982-185">Child content</span></span>

<span data-ttu-id="cd982-186">Składniki mogą ustawiać zawartość innego składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-186">Components can set the content of another component.</span></span> <span data-ttu-id="cd982-187">Składnik Assigner zawiera zawartość między tagami, które określają składnik do odbioru.</span><span class="sxs-lookup"><span data-stu-id="cd982-187">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="cd982-188">W poniższym przykładzie `ChildComponent` ma właściwość `ChildContent`, która reprezentuje `RenderFragment`, która reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="cd982-188">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="cd982-189">Wartość `ChildContent` jest umieszczana w znacznikach składnika, w którym powinna być renderowana zawartość.</span><span class="sxs-lookup"><span data-stu-id="cd982-189">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="cd982-190">Wartość `ChildContent` jest odbierana ze składnika nadrzędnego i renderowany w `panel-body`panelu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="cd982-190">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="cd982-191">*Składniki/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-191">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="cd982-192">Właściwość, która otrzymuje `RenderFragment` zawartość, musi mieć nazwę `ChildContent` według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="cd982-192">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="cd982-193">`ParentComponent` w przykładowej aplikacji może udostępniać zawartość w celu renderowania `ChildComponent` przez umieszczenie zawartości wewnątrz tagów `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="cd982-193">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="cd982-194">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-194">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="cd982-195">Korzystając atrybutów i dowolne parametry</span><span class="sxs-lookup"><span data-stu-id="cd982-195">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="cd982-196">Składniki mogą przechwytywać i renderować dodatkowe atrybuty oprócz zadeklarowanych parametrów składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-196">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="cd982-197">Dodatkowe atrybuty mogą być przechwytywane w słowniku, a następnie *splatted* na element, gdy składnik jest renderowany przy użyciu dyrektywy [`@attributes`](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="cd982-197">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="cd982-198">Ten scenariusz jest przydatny podczas definiowania składnika, który generuje element znaczników, który obsługuje różne dostosowania.</span><span class="sxs-lookup"><span data-stu-id="cd982-198">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="cd982-199">Na przykład można żmudnym definiować atrybuty oddzielnie dla `<input>`, które obsługują wiele parametrów.</span><span class="sxs-lookup"><span data-stu-id="cd982-199">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="cd982-200">W poniższym przykładzie pierwszy element `<input>` (`id="useIndividualParams"`) używa pojedynczych parametrów składnika, podczas gdy drugi `<input>` elementu (`id="useAttributesDict"`) używa atrybutu korzystając:</span><span class="sxs-lookup"><span data-stu-id="cd982-200">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="cd982-201">Typ parametru musi implementować `IEnumerable<KeyValuePair<string, object>>` za pomocą kluczy ciągu.</span><span class="sxs-lookup"><span data-stu-id="cd982-201">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="cd982-202">Używanie `IReadOnlyDictionary<string, object>` jest również opcją w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="cd982-202">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="cd982-203">Renderowane `<input>` elementy używające obu metod są identyczne:</span><span class="sxs-lookup"><span data-stu-id="cd982-203">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="cd982-204">Aby zaakceptować dowolne atrybuty, zdefiniuj parametr składnika przy użyciu atrybutu `[Parameter]` z właściwością `CaptureUnmatchedValues` ustawioną na `true`:</span><span class="sxs-lookup"><span data-stu-id="cd982-204">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="cd982-205">Właściwość `CaptureUnmatchedValues` na `[Parameter]` umożliwia dopasowanie parametru do wszystkich atrybutów, które nie są zgodne z żadnym innym parametrem.</span><span class="sxs-lookup"><span data-stu-id="cd982-205">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="cd982-206">Składnik może definiować tylko jeden parametr z `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="cd982-206">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="cd982-207">Typ właściwości używany z `CaptureUnmatchedValues` musi być możliwy do przypisania z `Dictionary<string, object>` z kluczami ciągu.</span><span class="sxs-lookup"><span data-stu-id="cd982-207">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="cd982-208">w tym scenariuszu są również dostępne opcje `IEnumerable<KeyValuePair<string, object>>` lub `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="cd982-208">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="cd982-209">Pozycja `@attributes` odnosząca się do pozycji atrybutów elementu jest ważna.</span><span class="sxs-lookup"><span data-stu-id="cd982-209">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="cd982-210">Gdy `@attributes` są splatted na elemencie, atrybuty są przetwarzane od prawej do lewej (Ostatnia do).</span><span class="sxs-lookup"><span data-stu-id="cd982-210">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="cd982-211">Rozważmy następujący przykład składnika, który zużywa składnik `Child`:</span><span class="sxs-lookup"><span data-stu-id="cd982-211">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="cd982-212">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-212">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="cd982-213">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-213">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="cd982-214">Atrybut `extra` składnika `Child` jest ustawiony na prawo od `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="cd982-214">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="cd982-215">`<div>` renderowanego składnika `Parent` zawiera `extra="5"`, gdy jest on przeszukiwany za pomocą dodatkowego atrybutu, ponieważ atrybuty są przetwarzane od prawej do lewej (od ostatni do pierwszego):</span><span class="sxs-lookup"><span data-stu-id="cd982-215">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="cd982-216">W poniższym przykładzie porządek `extra` i `@attributes` jest odwrócony w `<div>`składnika `Child`:</span><span class="sxs-lookup"><span data-stu-id="cd982-216">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="cd982-217">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-217">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="cd982-218">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-218">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="cd982-219">Renderowane `<div>` w składniku `Parent` zawiera `extra="10"` w przypadku przechodzenia przez dodatkowy atrybut:</span><span class="sxs-lookup"><span data-stu-id="cd982-219">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="cd982-220">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="cd982-220">Data binding</span></span>

<span data-ttu-id="cd982-221">Powiązanie danych zarówno ze składnikami, jak i elementami modelu DOM jest realizowane przy użyciu atrybutu [`@bind`](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="cd982-221">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="cd982-222">Poniższy przykład wiąże Właściwość `CurrentValue` z wartością pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="cd982-222">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="cd982-223">Gdy pole tekstowe utraci fokus, wartość właściwości jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="cd982-223">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="cd982-224">Pole tekstowe jest aktualizowane w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, a nie w odpowiedzi na zmianę wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="cd982-224">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="cd982-225">Ponieważ składniki renderują się po wykonaniu kodu procedury obsługi zdarzeń, aktualizacje właściwości są *zwykle* odzwierciedlane w interfejsie użytkownika natychmiast po wyzwoleniu programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cd982-225">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="cd982-226">Używanie `@bind` z właściwością `CurrentValue` (`<input @bind="CurrentValue" />`) jest zasadniczo równoważne z następującymi:</span><span class="sxs-lookup"><span data-stu-id="cd982-226">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="cd982-227">Gdy składnik jest renderowany, `value` elementu wejściowego pochodzi z właściwości `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="cd982-227">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="cd982-228">Gdy użytkownik wpisze w polu tekstowym i zmieni fokus elementu, zdarzenie `onchange` jest wyzwalane, a właściwość `CurrentValue` jest ustawiona na wartość zmieniona.</span><span class="sxs-lookup"><span data-stu-id="cd982-228">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="cd982-229">W rzeczywistości generowanie kodu jest bardziej skomplikowane, ponieważ `@bind` obsługuje przypadki, w których są wykonywane konwersje typów.</span><span class="sxs-lookup"><span data-stu-id="cd982-229">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="cd982-230">W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia z atrybutem `value` i obsługuje zmiany przy użyciu zarejestrowanej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="cd982-230">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="cd982-231">Oprócz obsługi zdarzeń `onchange` ze składnią `@bind`, właściwość lub pole można powiązać przy użyciu innych zdarzeń, określając atrybut [`@bind-value`](xref:mvc/views/razor#bind) z `event` parametrem ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="cd982-231">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="cd982-232">Poniższy przykład wiąże `CurrentValue` właściwość dla zdarzenia `oninput`:</span><span class="sxs-lookup"><span data-stu-id="cd982-232">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="cd982-233">W przeciwieństwie do `onchange`, które jest wyzwalane, gdy element utraci fokus, `oninput` uruchamiany, gdy wartość pola tekstowego ulegnie zmianie.</span><span class="sxs-lookup"><span data-stu-id="cd982-233">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="cd982-234">`@bind-value` w powyższym przykładzie powiązania:</span><span class="sxs-lookup"><span data-stu-id="cd982-234">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="cd982-235">Określone wyrażenie (`CurrentValue`) do atrybutu `value` elementu.</span><span class="sxs-lookup"><span data-stu-id="cd982-235">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="cd982-236">Delegowanie zdarzenia zmiany do zdarzenia określonego przez `@bind-value:event`.</span><span class="sxs-lookup"><span data-stu-id="cd982-236">A change event delegate to the event specified by `@bind-value:event`.</span></span>

### <a name="unparsable-values"></a><span data-ttu-id="cd982-237">Wartości niemożliwy do przeanalizowania</span><span class="sxs-lookup"><span data-stu-id="cd982-237">Unparsable values</span></span>

<span data-ttu-id="cd982-238">Gdy użytkownik dostarczy wartość niemożliwy do przeanalizowania do elementu powiązanego z danymi, wartość niemożliwy do przeanalizowania jest automatycznie przywracana do poprzedniej wartości po wyzwoleniu zdarzenia bind.</span><span class="sxs-lookup"><span data-stu-id="cd982-238">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="cd982-239">Poniżej przedstawiono przykładowy scenariusz:</span><span class="sxs-lookup"><span data-stu-id="cd982-239">Consider the following scenario:</span></span>

* <span data-ttu-id="cd982-240">Element `<input>` jest powiązany z typem `int` z początkową wartością `123`:</span><span class="sxs-lookup"><span data-stu-id="cd982-240">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="cd982-241">Użytkownik aktualizuje wartość elementu do `123.45` na stronie i zmienia fokus elementu.</span><span class="sxs-lookup"><span data-stu-id="cd982-241">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="cd982-242">W poprzednim scenariuszu wartość elementu jest przywracana do `123`.</span><span class="sxs-lookup"><span data-stu-id="cd982-242">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="cd982-243">Gdy wartość `123.45` zostanie odrzucona na korzyść oryginalnej wartości `123`, użytkownik rozumie, że ich wartość nie została zaakceptowana.</span><span class="sxs-lookup"><span data-stu-id="cd982-243">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="cd982-244">Domyślnie powiązanie dotyczy zdarzenia `onchange` elementu (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="cd982-244">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="cd982-245">Użyj `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`, aby ustawić inne zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="cd982-245">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="cd982-246">W przypadku zdarzenia `oninput` (`@bind-value:event="oninput"`) następuje rewersja po naciśnięciu klawisza, które wprowadza niemożliwy do przeanalizowania wartość.</span><span class="sxs-lookup"><span data-stu-id="cd982-246">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="cd982-247">Podczas określania wartości docelowej zdarzenia `oninput` przy użyciu typu powiązanego z `int`użytkownik nie będzie wpisywać znaku `.`.</span><span class="sxs-lookup"><span data-stu-id="cd982-247">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="cd982-248">Znak `.` zostanie natychmiast usunięty, więc użytkownik otrzymuje natychmiastową opinię, że dozwolone są tylko liczby całkowite.</span><span class="sxs-lookup"><span data-stu-id="cd982-248">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="cd982-249">Istnieją scenariusze, w których przywrócenie wartości w zdarzeniu `oninput` nie jest idealne, na przykład wtedy, gdy użytkownik powinien mieć możliwość wyczyszczenia wartości `<input>` niemożliwej do przeanalizowania.</span><span class="sxs-lookup"><span data-stu-id="cd982-249">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="cd982-250">Alternatywy obejmują:</span><span class="sxs-lookup"><span data-stu-id="cd982-250">Alternatives include:</span></span>

* <span data-ttu-id="cd982-251">Nie używaj zdarzenia `oninput`.</span><span class="sxs-lookup"><span data-stu-id="cd982-251">Don't use the `oninput` event.</span></span> <span data-ttu-id="cd982-252">Użyj domyślnego zdarzenia `onchange` (`@bind="{PROPERTY OR FIELD}"`), w którym niedozwolona wartość nie zostanie przywrócona, dopóki element nie utraci fokusu.</span><span class="sxs-lookup"><span data-stu-id="cd982-252">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="cd982-253">Powiąż z typem dopuszczającym wartość null, takim jak `int?` lub `string`, i podaj logikę niestandardową do obsługi nieprawidłowych wpisów.</span><span class="sxs-lookup"><span data-stu-id="cd982-253">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="cd982-254">Użyj [składnika walidacji formularza](xref:blazor/forms-validation), takiego jak `InputNumber` lub `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="cd982-254">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="cd982-255">Składniki walidacji formularza mają wbudowaną obsługę zarządzania nieprawidłowymi danymi wejściowymi.</span><span class="sxs-lookup"><span data-stu-id="cd982-255">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="cd982-256">Składniki walidacji formularza:</span><span class="sxs-lookup"><span data-stu-id="cd982-256">Form validation components:</span></span>
  * <span data-ttu-id="cd982-257">Zezwalaj użytkownikowi na dostarczenie nieprawidłowych danych wejściowych i otrzymywanie błędów walidacji w skojarzonym `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="cd982-257">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="cd982-258">Wyświetlaj błędy walidacji w interfejsie użytkownika bez zakłócania wprowadzania dodatkowych danych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd982-258">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

### <a name="globalization"></a><span data-ttu-id="cd982-259">Globalizacja</span><span class="sxs-lookup"><span data-stu-id="cd982-259">Globalization</span></span>

<span data-ttu-id="cd982-260">wartości `@bind` są sformatowane do wyświetlania i analizowane przy użyciu reguł bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="cd982-260">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="cd982-261">Dostęp do bieżącej kultury można uzyskać ze właściwości <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="cd982-261">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="cd982-262">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) jest używany dla następujących typów pól (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="cd982-262">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="cd982-263">Poprzednie typy pól:</span><span class="sxs-lookup"><span data-stu-id="cd982-263">The preceding field types:</span></span>

* <span data-ttu-id="cd982-264">Są wyświetlane przy użyciu odpowiednich reguł formatowania opartych na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="cd982-264">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="cd982-265">Nie może zawierać tekstu o dowolnym formacie.</span><span class="sxs-lookup"><span data-stu-id="cd982-265">Can't contain free-form text.</span></span>
* <span data-ttu-id="cd982-266">Podaj charakterystykę interakcji użytkownika w oparciu o implementację przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cd982-266">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="cd982-267">Następujące typy pól mają określone wymagania dotyczące formatowania i nie są obecnie obsługiwane przez Blazor, ponieważ nie są obsługiwane przez wszystkie główne przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="cd982-267">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="cd982-268">`@bind` obsługuje parametr `@bind:culture`, aby zapewnić <xref:System.Globalization.CultureInfo?displayProperty=fullName> do analizy i formatowania wartości.</span><span class="sxs-lookup"><span data-stu-id="cd982-268">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="cd982-269">Określanie kultury nie jest zalecane w przypadku używania typów pól `date` i `number`.</span><span class="sxs-lookup"><span data-stu-id="cd982-269">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="cd982-270">`date` i `number` mają wbudowaną obsługę Blazor, która udostępnia wymaganą kulturę.</span><span class="sxs-lookup"><span data-stu-id="cd982-270">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="cd982-271">Aby uzyskać informacje na temat sposobu ustawiania kultury użytkownika, zobacz sekcję [Lokalizacja](#localization) .</span><span class="sxs-lookup"><span data-stu-id="cd982-271">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

### <a name="format-strings"></a><span data-ttu-id="cd982-272">Ciągi formatujące</span><span class="sxs-lookup"><span data-stu-id="cd982-272">Format strings</span></span>

<span data-ttu-id="cd982-273">Powiązanie danych działa z ciągami formatu <xref:System.DateTime> przy użyciu [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="cd982-273">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="cd982-274">W tej chwili nie są dostępne inne wyrażenia formatu, takie jak formaty walutowe lub liczbowe.</span><span class="sxs-lookup"><span data-stu-id="cd982-274">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="cd982-275">W powyższym kodzie, typ pola `<input>` elementu (`type`) domyślnie `text`.</span><span class="sxs-lookup"><span data-stu-id="cd982-275">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="cd982-276">`@bind:format` jest obsługiwana w celu powiązania następujących typów .NET:</span><span class="sxs-lookup"><span data-stu-id="cd982-276">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="cd982-277"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="cd982-277"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="cd982-278"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="cd982-278"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="cd982-279">Atrybut `@bind:format` określa format daty, który ma zostać zastosowany do `value` elementu `<input>`.</span><span class="sxs-lookup"><span data-stu-id="cd982-279">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="cd982-280">Format jest również używany do analizowania wartości, gdy wystąpi zdarzenie `onchange`.</span><span class="sxs-lookup"><span data-stu-id="cd982-280">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="cd982-281">Określanie formatu dla typu pola `date` nie jest zalecane, ponieważ Blazor ma wbudowaną obsługę formatowania dat.</span><span class="sxs-lookup"><span data-stu-id="cd982-281">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="cd982-282">Pomimo zalecenia, należy używać tylko formatu daty `yyyy-MM-dd`, aby powiązania działały prawidłowo, jeśli format jest dostarczany z typem pola `date`:</span><span class="sxs-lookup"><span data-stu-id="cd982-282">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="cd982-283">Powiązanie element nadrzędny-to-Child z parametrami składnika</span><span class="sxs-lookup"><span data-stu-id="cd982-283">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="cd982-284">Powiązanie rozpoznaje parametry składnika, gdzie `@bind-{property}` może powiązać wartość właściwości z składnika nadrzędnego w dół ze składnikiem podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="cd982-284">Binding recognizes component parameters, where `@bind-{property}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="cd982-285">Powiązanie z elementu podrzędnego z elementem nadrzędnym jest omówione w [powiązaniu podrzędnie-to-Parent z częściowym powiązaniem powiązania](#child-to-parent-binding-with-chained-bind) .</span><span class="sxs-lookup"><span data-stu-id="cd982-285">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="cd982-286">Poniższy składnik podrzędny (`ChildComponent`) ma parametr składnika `Year` i wywołanie zwrotne `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="cd982-286">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="cd982-287">`EventCallback<T>` wyjaśniono w sekcji [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="cd982-287">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="cd982-288">Poniższy składnik nadrzędny używa:</span><span class="sxs-lookup"><span data-stu-id="cd982-288">The following parent component uses:</span></span>

* <span data-ttu-id="cd982-289">`ChildComponent` i wiąże parametr `ParentYear` z elementu nadrzędnego z parametrem `Year` w składniku podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="cd982-289">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="cd982-290">Zdarzenie `onclick` służy do wyzwalania metody `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="cd982-290">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="cd982-291">Aby uzyskać więcej informacji, zobacz sekcję [Obsługa zdarzeń](#event-handling) .</span><span class="sxs-lookup"><span data-stu-id="cd982-291">For more information, see the [Event handling](#event-handling) section.</span></span>

```razor
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

<span data-ttu-id="cd982-292">Załadowanie `ParentComponent` powoduje utworzenie następującej adjustacji:</span><span class="sxs-lookup"><span data-stu-id="cd982-292">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="cd982-293">Jeśli wartość właściwości `ParentYear` zostanie zmieniona przez wybranie przycisku w `ParentComponent`, zostanie zaktualizowana właściwość `Year` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="cd982-293">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="cd982-294">Nowa wartość `Year` jest renderowana w interfejsie użytkownika podczas renderowania `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="cd982-294">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="cd982-295">Parametr `Year` jest możliwy do powiązania, ponieważ zawiera zdarzenie pomocnika `YearChanged` pasujące do typu parametru `Year`.</span><span class="sxs-lookup"><span data-stu-id="cd982-295">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="cd982-296">Zgodnie z Konwencją `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo równoważne zapisowi:</span><span class="sxs-lookup"><span data-stu-id="cd982-296">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="cd982-297">Ogólnie rzecz biorąc, właściwość może być powiązana z odpowiednią obsługą zdarzeń przy użyciu atrybutu `@bind-property:event`.</span><span class="sxs-lookup"><span data-stu-id="cd982-297">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="cd982-298">Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` przy użyciu następujących dwóch atrybutów:</span><span class="sxs-lookup"><span data-stu-id="cd982-298">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="cd982-299">Powiązanie elementu podrzędnego z elementem nadrzędnym z powiązaniem łańcuchowym</span><span class="sxs-lookup"><span data-stu-id="cd982-299">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="cd982-300">Typowy scenariusz polega na łańcuchu parametru powiązanego z danymi do elementu strony w danych wyjściowych składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-300">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="cd982-301">Ten scenariusz jest nazywany *powiązaniem łańcuchowym* , ponieważ wiele poziomów powiązań występuje jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="cd982-301">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="cd982-302">Nie można zaimplementować powiązania łańcuchowego z składnią `@bind` w elemencie strony.</span><span class="sxs-lookup"><span data-stu-id="cd982-302">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="cd982-303">Program obsługi zdarzeń i wartość muszą być określone osobno.</span><span class="sxs-lookup"><span data-stu-id="cd982-303">The event handler and value must be specified separately.</span></span> <span data-ttu-id="cd982-304">Składnik nadrzędny, jednak może używać składni `@bind`ej z parametrem składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-304">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="cd982-305">Następujący składnik `PasswordField` (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="cd982-305">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="cd982-306">Ustawia wartość elementu `<input>` na Właściwość `Password`.</span><span class="sxs-lookup"><span data-stu-id="cd982-306">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="cd982-307">Uwidacznia zmiany właściwości `Password` w składniku nadrzędnym z [EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="cd982-307">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>
* <span data-ttu-id="cd982-308">Używa zdarzenia `onclick` służy do wyzwalania metody `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="cd982-308">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="cd982-309">Aby uzyskać więcej informacji, zobacz sekcję [Obsługa zdarzeń](#event-handling) .</span><span class="sxs-lookup"><span data-stu-id="cd982-309">For more information, see the [Event handling](#event-handling) section.</span></span>

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

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
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="cd982-310">Składnik `PasswordField` jest używany w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="cd982-310">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="cd982-311">Aby przeprowadzić sprawdzenia lub błędy pułapki dla hasła w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cd982-311">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="cd982-312">Utwórz pole zapasowe dla `Password` (`_password` w poniższym przykładowym kodzie).</span><span class="sxs-lookup"><span data-stu-id="cd982-312">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="cd982-313">Wykonaj testy lub błędy pułapki w metodzie ustawiającej `Password`.</span><span class="sxs-lookup"><span data-stu-id="cd982-313">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="cd982-314">Poniższy przykład przedstawia natychmiastową opinię dla użytkownika, jeśli w wartości hasła jest używana spacja:</span><span class="sxs-lookup"><span data-stu-id="cd982-314">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
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
        _showPassword = !_showPassword;
    }
}
```

### <a name="radio-buttons"></a><span data-ttu-id="cd982-315">Przyciski radiowe</span><span class="sxs-lookup"><span data-stu-id="cd982-315">Radio buttons</span></span>

<span data-ttu-id="cd982-316">Aby uzyskać informacje na temat tworzenia powiązań z przyciskami radiowymi w formularzu, zobacz <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="cd982-316">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>

## <a name="event-handling"></a><span data-ttu-id="cd982-317">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="cd982-317">Event handling</span></span>

<span data-ttu-id="cd982-318">Składniki Razor zapewniają funkcje obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cd982-318">Razor components provide event handling features.</span></span> <span data-ttu-id="cd982-319">Dla atrybutu elementu HTML o nazwie `on{EVENT}` (na przykład `onclick` i `onsubmit`) z wartością typu delegata składniki Razor traktują wartość atrybutu jako procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cd982-319">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="cd982-320">Nazwa atrybutu jest zawsze sformatowana [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="cd982-320">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="cd982-321">Poniższy kod wywołuje metodę `UpdateHeading`, gdy przycisk zostanie wybrany w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="cd982-321">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
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

<span data-ttu-id="cd982-322">Poniższy kod wywołuje metodę `CheckChanged`, gdy pole wyboru zostanie zmienione w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="cd982-322">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="cd982-323">Procedury obsługi zdarzeń mogą również być asynchroniczne i zwracać <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="cd982-323">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="cd982-324">Nie ma potrzeby ręcznego wywoływania [StateHasChanged](xref:blazor/lifecycle#state-changes).</span><span class="sxs-lookup"><span data-stu-id="cd982-324">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="cd982-325">Wyjątki są rejestrowane, gdy wystąpią.</span><span class="sxs-lookup"><span data-stu-id="cd982-325">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="cd982-326">W poniższym przykładzie `UpdateHeading` jest wywoływana asynchronicznie po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="cd982-326">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
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

### <a name="event-argument-types"></a><span data-ttu-id="cd982-327">Typy argumentów zdarzeń</span><span class="sxs-lookup"><span data-stu-id="cd982-327">Event argument types</span></span>

<span data-ttu-id="cd982-328">W przypadku niektórych zdarzeń dozwolone są typy argumentów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cd982-328">For some events, event argument types are permitted.</span></span> <span data-ttu-id="cd982-329">Jeśli dostęp do jednego z tych typów zdarzeń nie jest konieczny, nie jest to wymagane w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="cd982-329">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="cd982-330">Obsługiwane `EventArgs` przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="cd982-330">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="cd982-331">Wydarzenie</span><span class="sxs-lookup"><span data-stu-id="cd982-331">Event</span></span>            | <span data-ttu-id="cd982-332">Klasa</span><span class="sxs-lookup"><span data-stu-id="cd982-332">Class</span></span>                | <span data-ttu-id="cd982-333">Zdarzenia i uwagi dotyczące modelu DOM</span><span class="sxs-lookup"><span data-stu-id="cd982-333">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="cd982-334">Schowek</span><span class="sxs-lookup"><span data-stu-id="cd982-334">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="cd982-335">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="cd982-335">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="cd982-336">Przeciągnij</span><span class="sxs-lookup"><span data-stu-id="cd982-336">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="cd982-337">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="cd982-337">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="cd982-338">`DataTransfer` i `DataTransferItem` przechowywać przeciągane dane elementu.</span><span class="sxs-lookup"><span data-stu-id="cd982-338">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="cd982-339">Błąd</span><span class="sxs-lookup"><span data-stu-id="cd982-339">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="cd982-340">Wydarzenie</span><span class="sxs-lookup"><span data-stu-id="cd982-340">Event</span></span>            | `EventArgs`          | <span data-ttu-id="cd982-341">*Ogólne*</span><span class="sxs-lookup"><span data-stu-id="cd982-341">*General*</span></span><br><span data-ttu-id="cd982-342">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="cd982-342">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="cd982-343">*Schowek*</span><span class="sxs-lookup"><span data-stu-id="cd982-343">*Clipboard*</span></span><br><span data-ttu-id="cd982-344">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="cd982-344">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="cd982-345">*Dane wejściowe*</span><span class="sxs-lookup"><span data-stu-id="cd982-345">*Input*</span></span><br><span data-ttu-id="cd982-346">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart``onsubmit`</span><span class="sxs-lookup"><span data-stu-id="cd982-346">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="cd982-347">*Multimedialny*</span><span class="sxs-lookup"><span data-stu-id="cd982-347">*Media*</span></span><br><span data-ttu-id="cd982-348">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="cd982-348">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="cd982-349">Fokus</span><span class="sxs-lookup"><span data-stu-id="cd982-349">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="cd982-350">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="cd982-350">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="cd982-351">Nie obejmuje obsługi `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="cd982-351">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="cd982-352">Dane wejściowe</span><span class="sxs-lookup"><span data-stu-id="cd982-352">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="cd982-353">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="cd982-353">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="cd982-354">Klawiatura</span><span class="sxs-lookup"><span data-stu-id="cd982-354">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="cd982-355">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="cd982-355">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="cd982-356">Wskaźnik</span><span class="sxs-lookup"><span data-stu-id="cd982-356">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="cd982-357">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="cd982-357">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="cd982-358">Wskaźnik myszy</span><span class="sxs-lookup"><span data-stu-id="cd982-358">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="cd982-359">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="cd982-359">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="cd982-360">Kółko myszy</span><span class="sxs-lookup"><span data-stu-id="cd982-360">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="cd982-361">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="cd982-361">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="cd982-362">Postęp</span><span class="sxs-lookup"><span data-stu-id="cd982-362">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="cd982-363">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress``ontimeout`</span><span class="sxs-lookup"><span data-stu-id="cd982-363">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="cd982-364">Dotyk</span><span class="sxs-lookup"><span data-stu-id="cd982-364">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="cd982-365">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave``ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="cd982-365">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="cd982-366">`TouchPoint` reprezentuje pojedynczy punkt kontaktu na urządzeniu dotykowym.</span><span class="sxs-lookup"><span data-stu-id="cd982-366">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="cd982-367">Więcej informacji zawierają następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="cd982-367">For more information, see the following resources:</span></span>

* <span data-ttu-id="cd982-368">[Klasy EventArgs w źródle odwołania ASP.NET Core (gałąź dotnet/aspnetcore Release/3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="cd982-368">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="cd982-369">[Powiadomienia MDN Web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; zawiera informacje o tym, które elementy HTML obsługują każde wydarzenie dom.</span><span class="sxs-lookup"><span data-stu-id="cd982-369">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="cd982-370">Wyrażenia lambda</span><span class="sxs-lookup"><span data-stu-id="cd982-370">Lambda expressions</span></span>

<span data-ttu-id="cd982-371">Wyrażenia lambda mogą być również używane:</span><span class="sxs-lookup"><span data-stu-id="cd982-371">Lambda expressions can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="cd982-372">Często wygodnie jest blisko dodatkowych wartości, na przykład podczas iteracji na zestawie elementów.</span><span class="sxs-lookup"><span data-stu-id="cd982-372">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="cd982-373">Poniższy przykład tworzy trzy przyciski, z których każdy wywołuje `UpdateHeading` przekazywaniem argumentu zdarzenia (`MouseEventArgs`) i numerem przycisku (`buttonNumber`), po wybraniu w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="cd982-373">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="cd982-374">**Nie** używaj zmiennej loop (`i`) w pętli `for` bezpośrednio w wyrażeniu lambda.</span><span class="sxs-lookup"><span data-stu-id="cd982-374">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="cd982-375">W przeciwnym razie ta sama zmienna jest używana przez wszystkie wyrażenia lambda, co sprawia, że wartość `i`być taka sama we wszystkich lambdach.</span><span class="sxs-lookup"><span data-stu-id="cd982-375">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="cd982-376">Zawsze Przechwytuj wartość w zmiennej lokalnej (`buttonNumber` w poprzednim przykładzie), a następnie użyj jej.</span><span class="sxs-lookup"><span data-stu-id="cd982-376">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="cd982-377">EventCallback</span><span class="sxs-lookup"><span data-stu-id="cd982-377">EventCallback</span></span>

<span data-ttu-id="cd982-378">Typowym scenariuszem ze składnikami zagnieżdżonymi jest potrzeba uruchomienia metody składnika nadrzędnego w przypadku wystąpienia zdarzenia podrzędnego składnika&mdash;na przykład gdy zdarzenie `onclick` wystąpi w elemencie podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="cd982-378">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="cd982-379">Aby uwidocznić zdarzenia między składnikami, użyj `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="cd982-379">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="cd982-380">Składnik nadrzędny może przypisać metodę wywołania zwrotnego do `EventCallback`składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="cd982-380">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="cd982-381">`ChildComponent` w przykładowej aplikacji (*Components/ChildComponent. Razor*) pokazuje, jak program obsługi `onclick` przycisku został skonfigurowany tak, aby otrzymać delegata `EventCallback` z `ParentComponent`próbki.</span><span class="sxs-lookup"><span data-stu-id="cd982-381">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="cd982-382">`EventCallback` jest wpisana z `MouseEventArgs`, która jest odpowiednia dla zdarzenia `onclick` z urządzenia peryferyjnego:</span><span class="sxs-lookup"><span data-stu-id="cd982-382">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="cd982-383">`ParentComponent` ustawia `EventCallback<T>` elementu podrzędnego (`OnClickCallback`) na jego metodę `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="cd982-383">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="cd982-384">*Strony/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-384">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="cd982-385">Gdy przycisk zostanie wybrany w `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="cd982-385">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="cd982-386">Metoda `ShowMessage` `ParentComponent`jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="cd982-386">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="cd982-387">`_messageText` jest aktualizowany i wyświetlany w `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="cd982-387">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="cd982-388">Wywołanie [StateHasChanged](xref:blazor/lifecycle#state-changes) nie jest wymagane w metodzie wywołania zwrotnego (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="cd982-388">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="cd982-389">`StateHasChanged` jest automatycznie wywoływana w celu odrenderowania `ParentComponent`, podobnie jak zdarzenia podrzędne wyzwalają ponowne renderowanie składnika w obsłudze zdarzeń, które są wykonywane w ramach elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="cd982-389">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="cd982-390">`EventCallback` i `EventCallback<T>` Zezwalaj na asynchroniczne Delegaty.</span><span class="sxs-lookup"><span data-stu-id="cd982-390">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="cd982-391">`EventCallback<T>` jest silnie wpisana i wymaga określonego typu argumentu.</span><span class="sxs-lookup"><span data-stu-id="cd982-391">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="cd982-392">`EventCallback` jest słabo wpisywany i dopuszcza każdy typ argumentu.</span><span class="sxs-lookup"><span data-stu-id="cd982-392">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="cd982-393">Wywołaj `EventCallback` lub `EventCallback<T>` z `InvokeAsync` i oczekują na <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="cd982-393">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="cd982-394">Użyj `EventCallback` i `EventCallback<T>` do obsługi zdarzeń i parametrów składnika powiązania.</span><span class="sxs-lookup"><span data-stu-id="cd982-394">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="cd982-395">Preferuj silnie wpisaną `EventCallback<T>` przez `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="cd982-395">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="cd982-396">`EventCallback<T>` zapewnia lepszą opinię o błędach dla użytkowników składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-396">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="cd982-397">Podobnie jak w przypadku innych programów obsługi zdarzeń interfejsu użytkownika, określenie parametru zdarzenia jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="cd982-397">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="cd982-398">Użyj `EventCallback`, gdy nie zostanie przeniesiona wartość do wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="cd982-398">Use `EventCallback` when there's no value passed to the callback.</span></span>

### <a name="prevent-default-actions"></a><span data-ttu-id="cd982-399">Zapobiegaj akcjom domyślnym</span><span class="sxs-lookup"><span data-stu-id="cd982-399">Prevent default actions</span></span>

<span data-ttu-id="cd982-400">Użyj atrybutu dyrektywy [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) , aby zapobiec domyślnej akcji dla zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cd982-400">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="cd982-401">Po wybraniu klucza na urządzeniu wejściowym, gdy fokus elementu znajduje się w polu tekstowym, przeglądarka zwykle wyświetla znak klucza w polu tekstowym.</span><span class="sxs-lookup"><span data-stu-id="cd982-401">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="cd982-402">W poniższym przykładzie zachowanie domyślne jest blokowane przez określenie atrybutu dyrektywy `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="cd982-402">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="cd982-403">Licznik przyrostu i klucz **+** nie są przechwytywane do wartości elementu `<input>`:</span><span class="sxs-lookup"><span data-stu-id="cd982-403">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
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

<span data-ttu-id="cd982-404">Określanie atrybutu `@on{EVENT}:preventDefault` bez wartości jest równoważne z `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="cd982-404">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="cd982-405">Wartość atrybutu może również być wyrażeniem.</span><span class="sxs-lookup"><span data-stu-id="cd982-405">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="cd982-406">W poniższym przykładzie `_shouldPreventDefault` jest polem `bool` ustawionym na `true` lub `false`:</span><span class="sxs-lookup"><span data-stu-id="cd982-406">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="cd982-407">Procedura obsługi zdarzeń nie jest wymagana, aby zapobiec akcji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="cd982-407">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="cd982-408">Procedury obsługi zdarzeń i zapobiegania domyślnym scenariuszom akcji mogą być używane niezależnie.</span><span class="sxs-lookup"><span data-stu-id="cd982-408">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="cd982-409">Zatrzymaj propagację zdarzeń</span><span class="sxs-lookup"><span data-stu-id="cd982-409">Stop event propagation</span></span>

<span data-ttu-id="cd982-410">Użyj atrybutu dyrektywy [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) , aby zatrzymać propagację zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cd982-410">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="cd982-411">W poniższym przykładzie, zaznaczając pole wyboru, Zapobiegaj kliknięciu zdarzeń z drugiego elementu podrzędnego `<div>` od propagowania do `<div>`nadrzędnego:</span><span class="sxs-lookup"><span data-stu-id="cd982-411">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
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

## <a name="capture-references-to-components"></a><span data-ttu-id="cd982-412">Przechwyć odwołania do składników</span><span class="sxs-lookup"><span data-stu-id="cd982-412">Capture references to components</span></span>

<span data-ttu-id="cd982-413">Odwołania do składników zapewniają sposób odwoływania się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`.</span><span class="sxs-lookup"><span data-stu-id="cd982-413">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="cd982-414">Aby przechwycić odwołanie do składnika:</span><span class="sxs-lookup"><span data-stu-id="cd982-414">To capture a component reference:</span></span>

* <span data-ttu-id="cd982-415">Dodaj atrybut [`@ref`](xref:mvc/views/razor#ref) do składnika podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="cd982-415">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="cd982-416">Zdefiniuj pole z tym samym typem co składnik podrzędny.</span><span class="sxs-lookup"><span data-stu-id="cd982-416">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="cd982-417">Gdy składnik jest renderowany, pole `_loginDialog` jest wypełniane za pomocą `MyLoginDialog` podrzędnego wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-417">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="cd982-418">Następnie można wywołać metody .NET w wystąpieniu składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-418">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd982-419">Zmienna `_loginDialog` jest wypełniana tylko po wyrenderowaniu składnika, a jego wyjście zawiera element `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="cd982-419">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="cd982-420">Do tego momentu nie ma niczego do odwołania.</span><span class="sxs-lookup"><span data-stu-id="cd982-420">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="cd982-421">Aby manipulować odwołaniami do składników po zakończeniu renderowania składnika, należy użyć [metody OnAfterRenderAsync lub OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="cd982-421">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="cd982-422">Podczas przechwytywania odwołań do składników użycie podobnej składni do [przechwytywania odwołań do elementów](xref:blazor/javascript-interop#capture-references-to-elements)nie jest funkcją [międzyoperacyjności języka JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="cd982-422">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="cd982-423">Odwołania do składników nie są przesyłane do kodu JavaScript,&mdash;są używane tylko w kodzie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="cd982-423">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="cd982-424">**Nie** należy używać odwołań do składników do mutacji stanu składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="cd982-424">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="cd982-425">Zamiast tego należy używać zwykłych parametrów deklaratywnych do przekazywania danych do składników podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="cd982-425">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="cd982-426">Użycie normalnych parametrów deklaratywnych powoduje, że składniki podrzędne, które automatycznie uruchamiają się w prawidłowym czasie.</span><span class="sxs-lookup"><span data-stu-id="cd982-426">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="cd982-427">Wywołaj metody składnika zewnętrznie, aby zaktualizować stan</span><span class="sxs-lookup"><span data-stu-id="cd982-427">Invoke component methods externally to update state</span></span>

<span data-ttu-id="cd982-428">Blazor używa `SynchronizationContext` do wymuszania pojedynczego logicznego wątku wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cd982-428">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="cd982-429">W tym `SynchronizationContext`są wykonywane [metody cyklu życia](xref:blazor/lifecycle) składnika i wszystkie wywołania zwrotne zdarzeń wywoływane przez Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-429">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="cd982-430">W przypadku zdarzenia składnika należy zaktualizować na podstawie zdarzenia zewnętrznego, takiego jak czasomierz lub inne powiadomienia, użyj metody `InvokeAsync`, która będzie wysyłana do `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-430">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="cd982-431">Rozważmy na przykład *usługę powiadamiania* , która może powiadomić dowolny składnik nasłuchujący zaktualizowanego stanu:</span><span class="sxs-lookup"><span data-stu-id="cd982-431">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="cd982-432">Zarejestruj `NotifierService` jako singletion:</span><span class="sxs-lookup"><span data-stu-id="cd982-432">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="cd982-433">W programie Blazor webassembly Zarejestruj usługę w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="cd982-433">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="cd982-434">Na serwerze Blazor Zarejestruj usługę w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cd982-434">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="cd982-435">Użyj `NotifierService`, aby zaktualizować składnik:</span><span class="sxs-lookup"><span data-stu-id="cd982-435">Use the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="cd982-436">W poprzednim przykładzie `NotifierService` wywołuje metodę `OnNotify` składnika poza `SynchronizationContext`em Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-436">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="cd982-437">`InvokeAsync` jest używany do przełączania do poprawnego kontekstu i renderowania kolejki.</span><span class="sxs-lookup"><span data-stu-id="cd982-437">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="cd982-438">Użyj klucza \@, aby kontrolować zachowywanie elementów i składników</span><span class="sxs-lookup"><span data-stu-id="cd982-438">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="cd982-439">Podczas renderowania listy elementów lub składników oraz elementów lub składników, które następnie zmieniają się, algorytm diff Blazor musi zdecydować, które z poprzednich elementów lub składników mogą być zachowywane i jak obiekty modelu powinny być mapowane na nie.</span><span class="sxs-lookup"><span data-stu-id="cd982-439">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="cd982-440">Zwykle ten proces jest automatyczny i można go zignorować, ale istnieją przypadki, w których może być konieczne sterowanie procesem.</span><span class="sxs-lookup"><span data-stu-id="cd982-440">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="cd982-441">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="cd982-441">Consider the following example:</span></span>

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

<span data-ttu-id="cd982-442">Zawartość kolekcji `People` może ulec zmianie z wstawionymi, usuniętymi lub z ponownymi zamówieniami.</span><span class="sxs-lookup"><span data-stu-id="cd982-442">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="cd982-443">Gdy składnik jest przerenderowany, składnik `<DetailsEditor>` może zmienić, aby otrzymywać różne `Details` wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="cd982-443">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="cd982-444">Może to spowodować bardziej złożone odwzorowanie niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="cd982-444">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="cd982-445">W niektórych przypadkach odzyskanie może prowadzić do zauważalnych różnic w zachowaniu, takich jak brak fokusu elementu.</span><span class="sxs-lookup"><span data-stu-id="cd982-445">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="cd982-446">Proces mapowania można kontrolować przy użyciu atrybutu dyrektywy `@key`.</span><span class="sxs-lookup"><span data-stu-id="cd982-446">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="cd982-447">`@key` powoduje, że algorytm różnicowania gwarantuje zachowywanie elementów lub składników na podstawie wartości klucza:</span><span class="sxs-lookup"><span data-stu-id="cd982-447">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="cd982-448">W przypadku zmiany kolekcji `People`, algorytm różnicowania zachowuje skojarzenie między wystąpieniami `<DetailsEditor>` i wystąpieniami `person`:</span><span class="sxs-lookup"><span data-stu-id="cd982-448">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="cd982-449">Jeśli `Person` zostanie usunięty z listy `People`, tylko odpowiednie wystąpienie `<DetailsEditor>` zostanie usunięte z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd982-449">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="cd982-450">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="cd982-450">Other instances are left unchanged.</span></span>
* <span data-ttu-id="cd982-451">Jeśli na liście zostanie wstawiony `Person`, jedno nowe wystąpienie `<DetailsEditor>` zostanie wstawione do odpowiedniego położenia.</span><span class="sxs-lookup"><span data-stu-id="cd982-451">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="cd982-452">Inne wystąpienia pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="cd982-452">Other instances are left unchanged.</span></span>
* <span data-ttu-id="cd982-453">W przypadku ponownego uporządkowania wpisów `Person` odpowiednie wystąpienia `<DetailsEditor>` są zachowywane i uporządkowane w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd982-453">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="cd982-454">W niektórych scenariuszach użycie `@key` minimalizuje złożoność operacji renderowania i pozwala uniknąć potencjalnych problemów związanych z zmianami stanowymi modelu DOM, takich jak pozycja fokusu.</span><span class="sxs-lookup"><span data-stu-id="cd982-454">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd982-455">Klucze są lokalne dla każdego elementu kontenera lub składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-455">Keys are local to each container element or component.</span></span> <span data-ttu-id="cd982-456">Klucze nie są porównywane globalnie w całym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="cd982-456">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="cd982-457">Kiedy używać klucza \@</span><span class="sxs-lookup"><span data-stu-id="cd982-457">When to use \@key</span></span>

<span data-ttu-id="cd982-458">Zazwyczaj warto używać `@key` zawsze, gdy lista jest renderowana (na przykład w bloku `@foreach`), a odpowiednia wartość istnieje do zdefiniowania `@key`.</span><span class="sxs-lookup"><span data-stu-id="cd982-458">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="cd982-459">Można również użyć `@key`, aby uniemożliwić Blazor z zachowaniem poddrzewa elementu lub składnika, gdy zmieniany jest obiekt:</span><span class="sxs-lookup"><span data-stu-id="cd982-459">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="cd982-460">W przypadku zmiany `@currentPerson` dyrektywa `@key` wymusza Blazor całego `<div>` i jego obiektów podrzędnych i ponownie kompiluje poddrzewo w interfejsie użytkownika z nowymi elementami i składnikami.</span><span class="sxs-lookup"><span data-stu-id="cd982-460">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="cd982-461">Może to być przydatne, jeśli zachodzi konieczność zagwarantowania, że stan interfejsu użytkownika nie jest zachowywany po zmianie `@currentPerson`.</span><span class="sxs-lookup"><span data-stu-id="cd982-461">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="cd982-462">Kiedy nie używać klucza \@</span><span class="sxs-lookup"><span data-stu-id="cd982-462">When not to use \@key</span></span>

<span data-ttu-id="cd982-463">Różnica między `@key`ami jest kosztem wydajności.</span><span class="sxs-lookup"><span data-stu-id="cd982-463">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="cd982-464">Koszt wydajności nie jest duży, ale Określ `@key` tylko wtedy, gdy kontrolowanie reguł utrwalania elementów i składników korzyści dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-464">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="cd982-465">Nawet jeśli `@key` nie jest używany, Blazor zachowuje elementy podrzędne i wystąpienia składników tak dużo, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="cd982-465">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="cd982-466">Jedyną zaletą korzystania z `@key` jest kontrola nad *sposobem* , w jaki wystąpienia modelu są mapowane na zachowane wystąpienia składników, zamiast algorytmu różnicowego, wybierając mapowanie.</span><span class="sxs-lookup"><span data-stu-id="cd982-466">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="cd982-467">Wartości, które mają być używane dla klucza \@</span><span class="sxs-lookup"><span data-stu-id="cd982-467">What values to use for \@key</span></span>

<span data-ttu-id="cd982-468">Ogólnie rzecz biorąc, warto podać jeden z następujących rodzajów wartości dla `@key`:</span><span class="sxs-lookup"><span data-stu-id="cd982-468">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="cd982-469">Wystąpienia obiektów modelu (na przykład wystąpienie `Person` jak w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="cd982-469">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="cd982-470">Zapewnia to zachowywanie na podstawie równości odwołań do obiektów.</span><span class="sxs-lookup"><span data-stu-id="cd982-470">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="cd982-471">Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`lub `Guid`).</span><span class="sxs-lookup"><span data-stu-id="cd982-471">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="cd982-472">Upewnij się, że wartości używane dla `@key` nie kolidują.</span><span class="sxs-lookup"><span data-stu-id="cd982-472">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="cd982-473">Jeśli w tym samym elemencie nadrzędnym zostaną wykryte wartości powodujące konflikt, Blazor zgłasza wyjątek, ponieważ nie może on w sposób jednoznaczny mapować starych elementów lub składników na nowe elementy lub składniki.</span><span class="sxs-lookup"><span data-stu-id="cd982-473">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="cd982-474">Używaj tylko odrębnych wartości, takich jak wystąpienia obiektów lub wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="cd982-474">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="cd982-475">Routing</span><span class="sxs-lookup"><span data-stu-id="cd982-475">Routing</span></span>

<span data-ttu-id="cd982-476">Routing w Blazor jest realizowany przez dostarczenie szablonu trasy do każdego dostępnego składnika w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-476">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="cd982-477">Po skompilowaniu pliku Razor z dyrektywą `@page`, wygenerowana Klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określania szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="cd982-477">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="cd982-478">W czasie wykonywania router szuka klas składników przy użyciu `RouteAttribute` i renderuje niezależnie składnik ma szablon trasy pasujący do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="cd982-478">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="cd982-479">Do składnika można zastosować wiele szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="cd982-479">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="cd982-480">Poniższy składnik odpowiada na żądania `/BlazorRoute` i `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="cd982-480">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="cd982-481">*Strony/BlazorRoute. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-481">*Pages/BlazorRoute.razor*:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a><span data-ttu-id="cd982-482">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="cd982-482">Route parameters</span></span>

<span data-ttu-id="cd982-483">Składniki mogą odbierać parametry tras z szablonu trasy dostarczonego w dyrektywie `@page`.</span><span class="sxs-lookup"><span data-stu-id="cd982-483">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="cd982-484">Router używa parametrów trasy, aby wypełnić odpowiednie parametry składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-484">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="cd982-485">*Strony/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-485">*Pages/RouteParameter.razor*:</span></span>

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

<span data-ttu-id="cd982-486">Parametry opcjonalne nie są obsługiwane, więc dwie dyrektywy `@page` są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="cd982-486">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="cd982-487">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="cd982-487">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="cd982-488">Druga dyrektywa `@page` przyjmuje parametr trasy `{text}` i przypisuje wartość do właściwości `Text`.</span><span class="sxs-lookup"><span data-stu-id="cd982-488">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="cd982-489">Składnia *catch-all* (`*`/`**`), która przechwytuje ścieżkę między wieloma granicami folderów, **nie** jest obsługiwana w składnikach Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="cd982-489">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="cd982-490">Obsługa klasy częściowej</span><span class="sxs-lookup"><span data-stu-id="cd982-490">Partial class support</span></span>

<span data-ttu-id="cd982-491">Składniki Razor są generowane jako klasy częściowe.</span><span class="sxs-lookup"><span data-stu-id="cd982-491">Razor components are generated as partial classes.</span></span> <span data-ttu-id="cd982-492">Składniki Razor są tworzone przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="cd982-492">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="cd982-493">C#kod jest zdefiniowany w bloku [`@code`](xref:mvc/views/razor#code) przy użyciu znaczników HTML i kodu Razor w pojedynczym pliku.</span><span class="sxs-lookup"><span data-stu-id="cd982-493">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="cd982-494">Szablony Blazor definiują ich składniki Razor przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="cd982-494">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="cd982-495">C#kod jest umieszczany w pliku związanym z kodem zdefiniowanym jako Klasa częściowa.</span><span class="sxs-lookup"><span data-stu-id="cd982-495">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="cd982-496">Poniższy przykład pokazuje domyślny składnik `Counter` z blokiem `@code` w aplikacji wygenerowanej na podstawie szablonu Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-496">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="cd982-497">Znaczniki HTML, kod Razor i C# kod są w tym samym pliku:</span><span class="sxs-lookup"><span data-stu-id="cd982-497">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="cd982-498">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-498">*Counter.razor*:</span></span>

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

<span data-ttu-id="cd982-499">Składnik `Counter` można również utworzyć przy użyciu pliku związanego z kodem z klasą częściową:</span><span class="sxs-lookup"><span data-stu-id="cd982-499">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="cd982-500">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-500">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="cd982-501">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd982-501">*Counter.razor.cs*:</span></span>

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

<span data-ttu-id="cd982-502">W razie potrzeby dodaj wszystkie wymagane przestrzenie nazw do pliku klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="cd982-502">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="cd982-503">Typowe przestrzenie nazw używane przez składniki Razor obejmują:</span><span class="sxs-lookup"><span data-stu-id="cd982-503">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="cd982-504">Określ klasę bazową</span><span class="sxs-lookup"><span data-stu-id="cd982-504">Specify a base class</span></span>

<span data-ttu-id="cd982-505">Dyrektywa [`@inherits`](xref:mvc/views/razor#inherits) może służyć do określania klasy bazowej dla składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-505">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="cd982-506">Poniższy przykład pokazuje, jak składnik może dziedziczyć klasę bazową `BlazorRocksBase`, aby zapewnić właściwości i metody składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-506">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="cd982-507">Klasa bazowa powinna pochodzić od `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="cd982-507">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="cd982-508">*Strony/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cd982-508">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="cd982-509">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd982-509">*BlazorRocksBase.cs*:</span></span>

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

## <a name="specify-an-attribute"></a><span data-ttu-id="cd982-510">Określ atrybut</span><span class="sxs-lookup"><span data-stu-id="cd982-510">Specify an attribute</span></span>

<span data-ttu-id="cd982-511">Atrybuty można określić w składnikach Razor za pomocą dyrektywy [`@attribute`](xref:mvc/views/razor#attribute) .</span><span class="sxs-lookup"><span data-stu-id="cd982-511">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="cd982-512">Poniższy przykład stosuje atrybut `[Authorize]` do klasy składnika:</span><span class="sxs-lookup"><span data-stu-id="cd982-512">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="cd982-513">Importuj składniki</span><span class="sxs-lookup"><span data-stu-id="cd982-513">Import components</span></span>

<span data-ttu-id="cd982-514">Przestrzeń nazw składnika utworzone przy użyciu Razor jest oparta na (w kolejności priorytetu):</span><span class="sxs-lookup"><span data-stu-id="cd982-514">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="cd982-515">Wyznaczanie [`@namespace`](xref:mvc/views/razor#namespace) w znaczniku pliku*Razor (`@namespace BlazorSample.MyNamespace`* ).</span><span class="sxs-lookup"><span data-stu-id="cd982-515">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="cd982-516">`RootNamespace` projektu w pliku projektu (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="cd982-516">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="cd982-517">Nazwa projektu, pobrana z nazwy pliku projektu ( *. csproj*) i ścieżka z katalogu głównego projektu do składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-517">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="cd982-518">Na przykład struktura rozpoznaje *{Project root}/Pages/index.Razor* (*BlazorSample. csproj*) do przestrzeni nazw `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="cd982-518">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="cd982-519">Składniki przestrzegają C# reguł powiązań nazw.</span><span class="sxs-lookup"><span data-stu-id="cd982-519">Components follow C# name binding rules.</span></span> <span data-ttu-id="cd982-520">Dla składnika `Index` w tym przykładzie składniki należące do zakresu są wszystkich składników:</span><span class="sxs-lookup"><span data-stu-id="cd982-520">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="cd982-521">W tym samym folderze *strony*.</span><span class="sxs-lookup"><span data-stu-id="cd982-521">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="cd982-522">Składniki w katalogu głównym projektu, które nie określają jawnie innej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="cd982-522">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="cd982-523">Składniki zdefiniowane w innej przestrzeni nazw są wprowadzane do zakresu za pomocą dyrektywy [`@using`](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="cd982-523">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="cd982-524">Jeśli inny składnik, `NavMenu.razor`, istnieje w *BlazorSample/Shared/* folder, składnik może być używany w `Index.razor` z następującą instrukcją `@using`:</span><span class="sxs-lookup"><span data-stu-id="cd982-524">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="cd982-525">Do składników można także odwoływać się za pomocą ich w pełni kwalifikowanych nazw, które nie wymagają dyrektywy [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="cd982-525">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="cd982-526">Kwalifikacja `global::` nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="cd982-526">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="cd982-527">Importowanie składników za pomocą instrukcji `using` z aliasami (na przykład `@using Foo = Bar`) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cd982-527">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="cd982-528">Częściowo kwalifikowane nazwy nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cd982-528">Partially qualified names aren't supported.</span></span> <span data-ttu-id="cd982-529">Na przykład dodawanie `@using BlazorSample` i odwoływanie się do `NavMenu.razor` za pomocą `<Shared.NavMenu></Shared.NavMenu>` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cd982-529">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="cd982-530">Warunkowe atrybuty elementu HTML</span><span class="sxs-lookup"><span data-stu-id="cd982-530">Conditional HTML element attributes</span></span>

<span data-ttu-id="cd982-531">Atrybuty elementu HTML są warunkowo renderowane na podstawie wartości .NET.</span><span class="sxs-lookup"><span data-stu-id="cd982-531">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="cd982-532">Jeśli wartość jest `false` lub `null`, atrybut nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="cd982-532">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="cd982-533">Jeśli wartość jest `true`, atrybut jest renderowany jako zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="cd982-533">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="cd982-534">W poniższym przykładzie `IsCompleted` określa, czy `checked` jest renderowany w znacznikach elementu:</span><span class="sxs-lookup"><span data-stu-id="cd982-534">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="cd982-535">Jeśli `IsCompleted` jest `true`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="cd982-535">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="cd982-536">Jeśli `IsCompleted` jest `false`, pole wyboru jest renderowane jako:</span><span class="sxs-lookup"><span data-stu-id="cd982-536">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="cd982-537">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="cd982-537">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="cd982-538">Niektóre atrybuty HTML, takie jak [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), nie działają prawidłowo, gdy typem .net jest `bool`.</span><span class="sxs-lookup"><span data-stu-id="cd982-538">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="cd982-539">W tych przypadkach Użyj typu `string` zamiast `bool`.</span><span class="sxs-lookup"><span data-stu-id="cd982-539">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="cd982-540">Nieprzetworzony kod HTML</span><span class="sxs-lookup"><span data-stu-id="cd982-540">Raw HTML</span></span>

<span data-ttu-id="cd982-541">Ciągi są zwykle renderowane przy użyciu węzłów tekstowych DOM, co oznacza, że wszystkie znaczniki, które mogą zawierać, są ignorowane i traktowane jako tekst literału.</span><span class="sxs-lookup"><span data-stu-id="cd982-541">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="cd982-542">Aby renderować nieprzetworzony kod HTML, zawiń zawartość HTML w `MarkupString` wartość.</span><span class="sxs-lookup"><span data-stu-id="cd982-542">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="cd982-543">Wartość jest analizowana jako plik HTML lub SVG i wstawiona do modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="cd982-543">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="cd982-544">Renderowanie nieprzetworzonego kodu HTML zbudowanego z dowolnego niezaufanego źródła stanowi **zagrożenie bezpieczeństwa** i należy je unikać!</span><span class="sxs-lookup"><span data-stu-id="cd982-544">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="cd982-545">Poniższy przykład ilustruje użycie typu `MarkupString`, aby dodać blok statycznej zawartości HTML do renderowanego danych wyjściowych składnika:</span><span class="sxs-lookup"><span data-stu-id="cd982-545">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="cd982-546">Składniki z szablonami</span><span class="sxs-lookup"><span data-stu-id="cd982-546">Templated components</span></span>

<span data-ttu-id="cd982-547">Składniki z szablonami są składnikami, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które mogą być następnie używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-547">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="cd982-548">Składniki z szablonami umożliwiają tworzenie składników wyższego poziomu, które są większe niż zwykłe składniki.</span><span class="sxs-lookup"><span data-stu-id="cd982-548">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="cd982-549">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="cd982-549">A couple of examples include:</span></span>

* <span data-ttu-id="cd982-550">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka, wierszy i stopki tabeli.</span><span class="sxs-lookup"><span data-stu-id="cd982-550">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="cd982-551">Składnik listy, który umożliwia użytkownikowi określenie szablonu do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="cd982-551">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="cd982-552">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="cd982-552">Template parameters</span></span>

<span data-ttu-id="cd982-553">Składnik szablonu jest definiowany przez określenie co najmniej jednego parametru składnika typu `RenderFragment` lub `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="cd982-553">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="cd982-554">Fragment renderowania reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="cd982-554">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="cd982-555">`RenderFragment<T>` przyjmuje parametr typu, który można określić podczas wywoływania fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="cd982-555">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="cd982-556">składnik `TableTemplate`:</span><span class="sxs-lookup"><span data-stu-id="cd982-556">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="cd982-557">W przypadku korzystania z składnika z szablonem parametry szablonu można określić za pomocą elementów podrzędnych, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="cd982-557">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```razor
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

### <a name="template-context-parameters"></a><span data-ttu-id="cd982-558">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="cd982-558">Template context parameters</span></span>

<span data-ttu-id="cd982-559">Argumenty składnika typu `RenderFragment<T>` przekazane jako elementy mają niejawny parametr o nazwie `context` (na przykład z poprzedniego przykładu kodu, `@context.PetId`), ale można zmienić nazwę parametru przy użyciu atrybutu `Context` elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="cd982-559">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="cd982-560">W poniższym przykładzie atrybut `Context` elementu `RowTemplate` określa `pet` parametr:</span><span class="sxs-lookup"><span data-stu-id="cd982-560">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```razor
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

<span data-ttu-id="cd982-561">Alternatywnie można określić atrybut `Context` dla elementu składnika.</span><span class="sxs-lookup"><span data-stu-id="cd982-561">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="cd982-562">Określony atrybut `Context` ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="cd982-562">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="cd982-563">Może to być przydatne, jeśli chcesz określić nazwę parametru zawartości dla niejawnej zawartości podrzędnej (bez żadnego elementu podrzędnego otoki).</span><span class="sxs-lookup"><span data-stu-id="cd982-563">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="cd982-564">W poniższym przykładzie atrybut `Context` pojawia się na elemencie `TableTemplate` i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="cd982-564">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```razor
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

### <a name="generic-typed-components"></a><span data-ttu-id="cd982-565">Składniki typu rodzajowego</span><span class="sxs-lookup"><span data-stu-id="cd982-565">Generic-typed components</span></span>

<span data-ttu-id="cd982-566">Składniki z szablonami są często wpisywane ogólnie.</span><span class="sxs-lookup"><span data-stu-id="cd982-566">Templated components are often generically typed.</span></span> <span data-ttu-id="cd982-567">Na przykład ogólny składnik `ListViewTemplate` może służyć do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="cd982-567">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="cd982-568">Aby zdefiniować składnik ogólny, użyj dyrektywy [`@typeparam`](xref:mvc/views/razor#typeparam) , aby określić parametry typu:</span><span class="sxs-lookup"><span data-stu-id="cd982-568">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="cd982-569">W przypadku używania składników o typie ogólnym parametr typu jest wnioskowany, jeśli jest to możliwe:</span><span class="sxs-lookup"><span data-stu-id="cd982-569">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="cd982-570">W przeciwnym razie parametr typu musi być jawnie określony przy użyciu atrybutu, który jest zgodny z nazwą parametru typu.</span><span class="sxs-lookup"><span data-stu-id="cd982-570">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="cd982-571">W poniższym przykładzie `TItem="Pet"` określa typ:</span><span class="sxs-lookup"><span data-stu-id="cd982-571">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="cd982-572">Wartości kaskadowe i parametry</span><span class="sxs-lookup"><span data-stu-id="cd982-572">Cascading values and parameters</span></span>

<span data-ttu-id="cd982-573">W niektórych scenariuszach nie można przepływać danych z składnika nadrzędnego do składnika potomnego przy użyciu [parametrów składnika](#component-parameters), zwłaszcza gdy istnieje kilka warstw składników.</span><span class="sxs-lookup"><span data-stu-id="cd982-573">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="cd982-574">Wartości kaskadowe i parametry rozwiązują ten problem, zapewniając wygodną metodę dla składnika nadrzędnego, aby zapewnić wartość wszystkim jej składnikom potomnym.</span><span class="sxs-lookup"><span data-stu-id="cd982-574">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="cd982-575">Kaskadowe wartości i parametry również zapewniają podejście do współrzędnych składników.</span><span class="sxs-lookup"><span data-stu-id="cd982-575">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="cd982-576">Przykład motywu</span><span class="sxs-lookup"><span data-stu-id="cd982-576">Theme example</span></span>

<span data-ttu-id="cd982-577">W poniższym przykładzie z przykładowej aplikacji Klasa `ThemeInfo` określa informacje o motywie, aby przetworzyć hierarchię składników w taki sposób, aby wszystkie przyciski w danej części aplikacji miały ten sam styl.</span><span class="sxs-lookup"><span data-stu-id="cd982-577">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="cd982-578">*UIThemeClasses/themeinfo wskazuje. cs*:</span><span class="sxs-lookup"><span data-stu-id="cd982-578">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="cd982-579">Składnik nadrzędny może zapewnić kaskadową wartość przy użyciu składnika wartości kaskadowych.</span><span class="sxs-lookup"><span data-stu-id="cd982-579">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="cd982-580">Składnik `CascadingValue` zawija poddrzewo hierarchii składników i dostarcza jedną wartość do wszystkich składników w tym poddrzewie.</span><span class="sxs-lookup"><span data-stu-id="cd982-580">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="cd982-581">Przykładowo aplikacja Przykładowa określa informacje o motywie (`ThemeInfo`) w jednej z układów aplikacji jako parametr kaskadowy dla wszystkich składników, które tworzą treść układu właściwości `@Body`.</span><span class="sxs-lookup"><span data-stu-id="cd982-581">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="cd982-582">`ButtonClass` ma przypisaną wartość `btn-success` w składniku układu.</span><span class="sxs-lookup"><span data-stu-id="cd982-582">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="cd982-583">Każdy składnik podrzędny może wykorzystać tę właściwość za pomocą obiektu kaskadowego `ThemeInfo`.</span><span class="sxs-lookup"><span data-stu-id="cd982-583">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="cd982-584">składnik `CascadingValuesParametersLayout`:</span><span class="sxs-lookup"><span data-stu-id="cd982-584">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="cd982-585">Aby korzystać z wartości kaskadowych, składniki deklarują kaskadowe parametry przy użyciu atrybutu `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="cd982-585">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="cd982-586">Wartości kaskadowe są powiązane z parametrami kaskadowymi według typu.</span><span class="sxs-lookup"><span data-stu-id="cd982-586">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="cd982-587">W przykładowej aplikacji składnik `CascadingValuesParametersTheme` wiąże `ThemeInfo` wartość kaskadową z parametrem kaskadowym.</span><span class="sxs-lookup"><span data-stu-id="cd982-587">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="cd982-588">Parametr służy do ustawiania klasy CSS dla jednego z przycisków wyświetlanych przez składnik.</span><span class="sxs-lookup"><span data-stu-id="cd982-588">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="cd982-589">składnik `CascadingValuesParametersTheme`:</span><span class="sxs-lookup"><span data-stu-id="cd982-589">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="cd982-590">Aby przetworzyć kaskadowo wiele wartości tego samego typu w ramach tego samego poddrzewa, podaj unikatowy ciąg `Name` do każdego składnika `CascadingValue` i odpowiadający mu `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="cd982-590">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="cd982-591">W poniższym przykładzie dwa składniki `CascadingValue` są kaskadowo różne wystąpienia `MyCascadingType` według nazwy:</span><span class="sxs-lookup"><span data-stu-id="cd982-591">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="cd982-592">W składniku potomnym, kaskadowe parametry odbierają swoje wartości z odpowiednich kaskadowych wartości w składniku nadrzędnym według nazwy:</span><span class="sxs-lookup"><span data-stu-id="cd982-592">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="cd982-593">Przykład TabSet</span><span class="sxs-lookup"><span data-stu-id="cd982-593">TabSet example</span></span>

<span data-ttu-id="cd982-594">Parametry kaskadowe umożliwiają również współdziałanie składników w hierarchii składników.</span><span class="sxs-lookup"><span data-stu-id="cd982-594">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="cd982-595">Rozważmy na przykład następujący przykład *TabSet* w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="cd982-595">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="cd982-596">Przykładowa aplikacja ma interfejs `ITab`, w którym znajdują się karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="cd982-596">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="cd982-597">Składnik `CascadingValuesParametersTabSet` używa składnika `TabSet`, który zawiera kilka `Tab` składników:</span><span class="sxs-lookup"><span data-stu-id="cd982-597">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

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

<span data-ttu-id="cd982-598">Podrzędne składniki `Tab` nie są jawnie przenoszone jako parametry do `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="cd982-598">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="cd982-599">Zamiast tego podrzędne składniki `Tab` są częścią zawartości podrzędnej `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="cd982-599">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="cd982-600">Jednak `TabSet` nadal muszą znać każdy składnik `Tab`, aby można było renderować nagłówki i aktywną kartę. Aby umożliwić tę koordynację bez konieczności stosowania dodatkowego kodu, składnik `TabSet` *może sam określić jako wartość kaskadową* , która jest następnie pobierana przez składniki `Tab` potomnych.</span><span class="sxs-lookup"><span data-stu-id="cd982-600">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="cd982-601">składnik `TabSet`:</span><span class="sxs-lookup"><span data-stu-id="cd982-601">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="cd982-602">Składniki `Tab` potomne przechwytują `TabSet` zawierający jako parametr kaskadowy, więc składniki `Tab` dodają same do `TabSet` i koordynują, na której karcie jest aktywna.</span><span class="sxs-lookup"><span data-stu-id="cd982-602">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="cd982-603">składnik `Tab`:</span><span class="sxs-lookup"><span data-stu-id="cd982-603">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="cd982-604">Szablony Razor</span><span class="sxs-lookup"><span data-stu-id="cd982-604">Razor templates</span></span>

<span data-ttu-id="cd982-605">Fragmenty renderowania można definiować przy użyciu składni szablonu Razor.</span><span class="sxs-lookup"><span data-stu-id="cd982-605">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="cd982-606">Szablony Razor są sposobem definiowania fragmentu interfejsu użytkownika i przyjmuje następujący format:</span><span class="sxs-lookup"><span data-stu-id="cd982-606">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="cd982-607">Poniższy przykład ilustruje sposób określania wartości `RenderFragment` i `RenderFragment<T>` oraz renderowania szablonów bezpośrednio w składniku.</span><span class="sxs-lookup"><span data-stu-id="cd982-607">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="cd982-608">Fragmenty renderowania mogą być również przekazane jako argumenty do [składników z szablonem](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="cd982-608">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="cd982-609">Renderowane dane wyjściowe poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="cd982-609">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="cd982-610">Ręczna logika RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="cd982-610">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="cd982-611">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` udostępnia metody manipulowania składnikami i elementami, w tym ręczne Kompilowanie C# składników w kodzie.</span><span class="sxs-lookup"><span data-stu-id="cd982-611">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="cd982-612">Użycie `RenderTreeBuilder` do tworzenia składników jest zaawansowanym scenariuszem.</span><span class="sxs-lookup"><span data-stu-id="cd982-612">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="cd982-613">Nieprawidłowo sformułowany składnik (na przykład niezamknięty tag znacznika) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="cd982-613">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="cd982-614">Rozważmy następujący składnik `PetDetails`, który można utworzyć ręcznie w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="cd982-614">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="cd982-615">W poniższym przykładzie pętla w metodzie `CreateComponent` generuje trzy składniki `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="cd982-615">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="cd982-616">Podczas wywoływania `RenderTreeBuilder` metod tworzenia składników (`OpenComponent` i `AddAttribute`) numery sekwencji są numerami wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="cd982-616">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="cd982-617">Algorytm Blazor różnica polega na numerach sekwencji odpowiadających odrębnym wierszom kodu, a nie odrębnym wywoływaniu wywołań.</span><span class="sxs-lookup"><span data-stu-id="cd982-617">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="cd982-618">Podczas tworzenia składnika przy użyciu metod `RenderTreeBuilder` umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="cd982-618">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="cd982-619">**Użycie obliczenia lub licznika do wygenerowania numeru sekwencji może prowadzić do niskiej wydajności.**</span><span class="sxs-lookup"><span data-stu-id="cd982-619">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="cd982-620">Aby uzyskać więcej informacji, zobacz sekcję [numery sekwencji powiązane z numerami wierszy kodu i kolejnością niewykonania](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="cd982-620">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="cd982-621">składnik `BuiltContent`:</span><span class="sxs-lookup"><span data-stu-id="cd982-621">`BuiltContent` component:</span></span>

```razor
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

> [!WARNING]
> <span data-ttu-id="cd982-622">Typy w `Microsoft.AspNetCore.Components.RenderTree` umożliwiają przetwarzanie *wyników* operacji renderowania.</span><span class="sxs-lookup"><span data-stu-id="cd982-622">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="cd982-623">Są to wewnętrzne szczegóły implementacji platformy Blazor Framework.</span><span class="sxs-lookup"><span data-stu-id="cd982-623">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="cd982-624">Te typy powinny być uznawane za *niestabilne* i mogą ulec zmianie w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="cd982-624">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="cd982-625">Numery sekwencji odnoszą się do numerów wierszy kodu, a nie kolejności wykonywania</span><span class="sxs-lookup"><span data-stu-id="cd982-625">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="cd982-626">Blazor pliki `.razor` są zawsze kompilowane.</span><span class="sxs-lookup"><span data-stu-id="cd982-626">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="cd982-627">Jest to znakomita korzyść dla `.razor`, ponieważ krok Kompiluj może służyć do iniekcji informacji, które zwiększają wydajność aplikacji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cd982-627">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="cd982-628">Najważniejszym przykładem tych ulepszeń są *numery sekwencji*.</span><span class="sxs-lookup"><span data-stu-id="cd982-628">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="cd982-629">Numery sekwencji wskazują na środowisko uruchomieniowe, które pochodzą z różnych i uporządkowanych wierszy kodu.</span><span class="sxs-lookup"><span data-stu-id="cd982-629">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="cd982-630">Środowisko uruchomieniowe używa tych informacji do generowania wydajnych różnic drzewa w czasie liniowym, które są znacznie szybsze niż zwykle jest to możliwe dla algorytmu różnicowego drzewa ogólnego.</span><span class="sxs-lookup"><span data-stu-id="cd982-630">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="cd982-631">Rozważmy następujący plik składnika Razor (*Razor*):</span><span class="sxs-lookup"><span data-stu-id="cd982-631">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="cd982-632">Poprzedni kod kompiluje się w taki sposób, aby wyglądał następująco:</span><span class="sxs-lookup"><span data-stu-id="cd982-632">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="cd982-633">Gdy kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="cd982-633">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="cd982-634">Sequence</span><span class="sxs-lookup"><span data-stu-id="cd982-634">Sequence</span></span> | <span data-ttu-id="cd982-635">Typ</span><span class="sxs-lookup"><span data-stu-id="cd982-635">Type</span></span>      | <span data-ttu-id="cd982-636">Dane</span><span class="sxs-lookup"><span data-stu-id="cd982-636">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="cd982-637">0</span><span class="sxs-lookup"><span data-stu-id="cd982-637">0</span></span>        | <span data-ttu-id="cd982-638">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="cd982-638">Text node</span></span> | <span data-ttu-id="cd982-639">Pierwszego</span><span class="sxs-lookup"><span data-stu-id="cd982-639">First</span></span>  |
| <span data-ttu-id="cd982-640">1</span><span class="sxs-lookup"><span data-stu-id="cd982-640">1</span></span>        | <span data-ttu-id="cd982-641">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="cd982-641">Text node</span></span> | <span data-ttu-id="cd982-642">Sekunda</span><span class="sxs-lookup"><span data-stu-id="cd982-642">Second</span></span> |

<span data-ttu-id="cd982-643">Załóżmy, że `someFlag` `false`, a znaczniki są renderowane ponownie.</span><span class="sxs-lookup"><span data-stu-id="cd982-643">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="cd982-644">Tym razem Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="cd982-644">This time, the builder receives:</span></span>

| <span data-ttu-id="cd982-645">Sequence</span><span class="sxs-lookup"><span data-stu-id="cd982-645">Sequence</span></span> | <span data-ttu-id="cd982-646">Typ</span><span class="sxs-lookup"><span data-stu-id="cd982-646">Type</span></span>       | <span data-ttu-id="cd982-647">Dane</span><span class="sxs-lookup"><span data-stu-id="cd982-647">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="cd982-648">1</span><span class="sxs-lookup"><span data-stu-id="cd982-648">1</span></span>        | <span data-ttu-id="cd982-649">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="cd982-649">Text node</span></span>  | <span data-ttu-id="cd982-650">Sekunda</span><span class="sxs-lookup"><span data-stu-id="cd982-650">Second</span></span> |

<span data-ttu-id="cd982-651">Gdy środowisko uruchomieniowe wykonuje różnicę, zobaczy, że element w sekwencji `0` został usunięty, więc generuje następujący skrypt uproszczonej *edycji*:</span><span class="sxs-lookup"><span data-stu-id="cd982-651">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="cd982-652">Usuń pierwszy węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="cd982-652">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="cd982-653">Co się stało z błędami w przypadku wygenerowania numerów sekwencyjnych</span><span class="sxs-lookup"><span data-stu-id="cd982-653">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="cd982-654">Wyobraź sobie, że została zapisana następująca logika konstruktora drzewa renderowania:</span><span class="sxs-lookup"><span data-stu-id="cd982-654">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="cd982-655">Teraz pierwsze dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="cd982-655">Now, the first output is:</span></span>

| <span data-ttu-id="cd982-656">Sequence</span><span class="sxs-lookup"><span data-stu-id="cd982-656">Sequence</span></span> | <span data-ttu-id="cd982-657">Typ</span><span class="sxs-lookup"><span data-stu-id="cd982-657">Type</span></span>      | <span data-ttu-id="cd982-658">Dane</span><span class="sxs-lookup"><span data-stu-id="cd982-658">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="cd982-659">0</span><span class="sxs-lookup"><span data-stu-id="cd982-659">0</span></span>        | <span data-ttu-id="cd982-660">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="cd982-660">Text node</span></span> | <span data-ttu-id="cd982-661">Pierwszego</span><span class="sxs-lookup"><span data-stu-id="cd982-661">First</span></span>  |
| <span data-ttu-id="cd982-662">1</span><span class="sxs-lookup"><span data-stu-id="cd982-662">1</span></span>        | <span data-ttu-id="cd982-663">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="cd982-663">Text node</span></span> | <span data-ttu-id="cd982-664">Sekunda</span><span class="sxs-lookup"><span data-stu-id="cd982-664">Second</span></span> |

<span data-ttu-id="cd982-665">Ten wynik jest identyczny z poprzednim przypadkiem, dlatego nie istnieją żadne negatywne problemy.</span><span class="sxs-lookup"><span data-stu-id="cd982-665">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="cd982-666">`someFlag` jest `false` podczas drugiego renderowania, a dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cd982-666">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="cd982-667">Sequence</span><span class="sxs-lookup"><span data-stu-id="cd982-667">Sequence</span></span> | <span data-ttu-id="cd982-668">Typ</span><span class="sxs-lookup"><span data-stu-id="cd982-668">Type</span></span>      | <span data-ttu-id="cd982-669">Dane</span><span class="sxs-lookup"><span data-stu-id="cd982-669">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="cd982-670">0</span><span class="sxs-lookup"><span data-stu-id="cd982-670">0</span></span>        | <span data-ttu-id="cd982-671">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="cd982-671">Text node</span></span> | <span data-ttu-id="cd982-672">Sekunda</span><span class="sxs-lookup"><span data-stu-id="cd982-672">Second</span></span> |

<span data-ttu-id="cd982-673">Tym razem algorytm diff widzi, że pojawiły się *dwie* zmiany, a algorytm generuje następujący skrypt edycji:</span><span class="sxs-lookup"><span data-stu-id="cd982-673">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="cd982-674">Zmień wartość pierwszego węzła tekstowego na `Second`.</span><span class="sxs-lookup"><span data-stu-id="cd982-674">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="cd982-675">Usuń drugi węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="cd982-675">Remove the second text node.</span></span>

<span data-ttu-id="cd982-676">Generowanie numerów sekwencji utraciło wszystkie przydatne informacje o tym, gdzie `if/else` gałęzie i pętle były obecne w oryginalnym kodzie.</span><span class="sxs-lookup"><span data-stu-id="cd982-676">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="cd982-677">Wynikiem tego jest różnica **dwa razy** , tak długo, jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="cd982-677">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="cd982-678">Jest to prosty przykład.</span><span class="sxs-lookup"><span data-stu-id="cd982-678">This is a trivial example.</span></span> <span data-ttu-id="cd982-679">W bardziej realistycznych przypadkach ze złożonymi i głęboko zagnieżdżonymi strukturami, szczególnie w przypadku pętli, koszt wydajności jest bardziej poważny.</span><span class="sxs-lookup"><span data-stu-id="cd982-679">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="cd982-680">Zamiast natychmiastowego identyfikowania, które bloki lub gałęzie pętli zostały wstawione lub usunięte, algorytm różnicowy musi reprezentować się w drzewach renderowania i zwykle tworzyć dużo dłużej edytowane skrypty, ponieważ nie są w nim poinformowani o sposobie starych i nowych struktur odnoszą się do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="cd982-680">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="cd982-681">Wskazówki i wnioski</span><span class="sxs-lookup"><span data-stu-id="cd982-681">Guidance and conclusions</span></span>

* <span data-ttu-id="cd982-682">Wydajność aplikacji ma wpływ na to, że numery sekwencji są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="cd982-682">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="cd982-683">Struktura nie może automatycznie tworzyć własnych numerów sekwencji w czasie wykonywania, ponieważ niezbędne informacje nie istnieją, chyba że są przechwytywane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-683">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="cd982-684">Nie zapisuj długich bloków ręcznie zaimplementowanej logiki `RenderTreeBuilder`.</span><span class="sxs-lookup"><span data-stu-id="cd982-684">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="cd982-685">Preferuj `.razor` pliki i Zezwalaj kompilatorowi na zaradzenie sobie z kolejnymi numerami.</span><span class="sxs-lookup"><span data-stu-id="cd982-685">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="cd982-686">Jeśli nie możesz uniknąć ręcznej logiki `RenderTreeBuilder`, Podziel długie bloki kodu na mniejsze fragmenty opakowane w `OpenRegion`/`CloseRegion` wywołania.</span><span class="sxs-lookup"><span data-stu-id="cd982-686">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="cd982-687">Każdy region ma własne oddzielne miejsce numerów sekwencyjnych, więc można uruchomić ponownie od zera (lub dowolnego innego numeru) w każdym regionie.</span><span class="sxs-lookup"><span data-stu-id="cd982-687">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="cd982-688">Jeśli numery sekwencji są stałee, algorytm diff wymaga tylko zwiększenia wartości sekwencji.</span><span class="sxs-lookup"><span data-stu-id="cd982-688">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="cd982-689">Początkowa wartość i przerwy są nieistotne.</span><span class="sxs-lookup"><span data-stu-id="cd982-689">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="cd982-690">Jedną z wiarygodnych opcji jest użycie numeru wiersza kodu jako numeru sekwencyjnego lub rozpoczęcie od zera i zwiększenie według wartości lub setek (lub dowolnego preferowanego interwału).</span><span class="sxs-lookup"><span data-stu-id="cd982-690">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="cd982-691"> używa numerów sekwencji, podczas gdy inne struktury interfejsu użytkownika rozróżniania drzewa nie są używane.</span><span class="sxs-lookup"><span data-stu-id="cd982-691"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="cd982-692">Różnica jest znacznie szybsza, gdy są używane numery sekwencji, a Blazor ma zalety kroku kompilacji, który zajmuje się automatycznie numerami sekwencyjnymi dla deweloperów tworzących pliki *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="cd982-692">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="cd982-693">Lokalizacja</span><span class="sxs-lookup"><span data-stu-id="cd982-693">Localization</span></span>

<span data-ttu-id="cd982-694">aplikacje serwera Blazor są zlokalizowane przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="cd982-694">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="cd982-695">Oprogramowanie pośredniczące wybiera odpowiednią kulturę dla użytkowników żądających zasobów z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-695">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="cd982-696">Kulturę można ustawić przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="cd982-696">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="cd982-697">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="cd982-697">Cookies</span></span>](#cookies)
* [<span data-ttu-id="cd982-698">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="cd982-698">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="cd982-699">Aby uzyskać więcej informacji i przykładów, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="cd982-699">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="cd982-700">Konfigurowanie konsolidatora do użytku wieloplikowego (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="cd982-700">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="cd982-701">Domyślnie konfiguracja konsolidatora Blazordla Blazor aplikacji webassembly umożliwia rozłączenie informacji o danych wielojęzycznych z wyjątkiem lokalizacji lokalnych jawnie żądanych.</span><span class="sxs-lookup"><span data-stu-id="cd982-701">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="cd982-702">Aby uzyskać więcej informacji i wskazówek dotyczących kontrolowania zachowania konsolidatora, zobacz <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="cd982-702">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="cd982-703">Cookie</span><span class="sxs-lookup"><span data-stu-id="cd982-703">Cookies</span></span>

<span data-ttu-id="cd982-704">Plik cookie kultury lokalizacji może utrzymywać kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd982-704">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="cd982-705">Plik cookie jest tworzony przez metodę `OnGet` strony hosta aplikacji (*strony/hosta. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="cd982-705">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="cd982-706">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie na kolejnych żądaniach, aby ustawić kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd982-706">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="cd982-707">Użycie pliku cookie zapewnia, że połączenie z użyciem protokołu WebSocket może prawidłowo propagować kulturę.</span><span class="sxs-lookup"><span data-stu-id="cd982-707">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="cd982-708">Jeśli schematy lokalizacji są oparte na ścieżce URL lub ciągu zapytania, schemat może nie być w stanie współdziałać z usługą WebSockets, więc nie będzie można zachować kultury.</span><span class="sxs-lookup"><span data-stu-id="cd982-708">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="cd982-709">W związku z tym zalecanym podejściem jest użycie pliku cookie kultury lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-709">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="cd982-710">Każda technika może służyć do przypisywania kultury, jeśli kultura jest utrwalona w pliku cookie lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-710">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="cd982-711">Jeśli aplikacja ma już ustalony schemat lokalizacji dla ASP.NET Core po stronie serwera, Kontynuuj korzystanie z istniejącej infrastruktury lokalizacji aplikacji i Ustaw plik cookie kultury lokalizacji w schemacie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-711">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="cd982-712">Poniższy przykład pokazuje, jak ustawić bieżącą kulturę w pliku cookie, który może zostać odczytany przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-712">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="cd982-713">Utwórz plik *Pages/hosta. cshtml. cs* z następującą zawartością w aplikacji Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="cd982-713">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="cd982-714">Lokalizacja jest obsługiwana w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cd982-714">Localization is handled in the app:</span></span>

1. <span data-ttu-id="cd982-715">Przeglądarka wysyła początkowe żądanie HTTP do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-715">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="cd982-716">Kultura jest przypisana przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="cd982-716">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="cd982-717">Metoda `OnGet` w *_Host. cshtml. cs* utrzymuje kulturę w pliku cookie jako część odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="cd982-717">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="cd982-718">Przeglądarka otwiera połączenie WebSocket, aby utworzyć interakcyjną sesję serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-718">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="cd982-719">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie i przypisuje kulturę.</span><span class="sxs-lookup"><span data-stu-id="cd982-719">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="cd982-720">Sesja serwera Blazor rozpoczyna się od poprawnej kultury.</span><span class="sxs-lookup"><span data-stu-id="cd982-720">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="cd982-721">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="cd982-721">Provide UI to choose the culture</span></span>

<span data-ttu-id="cd982-722">Aby zapewnić interfejs użytkownika, aby umożliwić użytkownikowi wybranie kultury, zalecane jest *podejście oparte na przekierowaniu* .</span><span class="sxs-lookup"><span data-stu-id="cd982-722">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="cd982-723">Ten proces jest podobny do tego, co się dzieje w aplikacji sieci Web, gdy użytkownik próbuje uzyskać dostęp do bezpiecznego zasobu&mdash;użytkownik zostanie przekierowany do strony logowania, a następnie przekierowany z powrotem do oryginalnego zasobu.</span><span class="sxs-lookup"><span data-stu-id="cd982-723">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="cd982-724">Aplikacja utrzymuje wybraną kulturę użytkownika za pośrednictwem przekierowania do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="cd982-724">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="cd982-725">Kontroler ustawia wybraną kulturę użytkownika na plik cookie i przekierowuje użytkownika z powrotem do oryginalnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="cd982-725">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="cd982-726">Ustanów punkt końcowy HTTP na serwerze, aby ustawić kulturę wybraną przez użytkownika w pliku cookie i wykonać przekierowanie z powrotem do oryginalnego identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="cd982-726">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="cd982-727">Użyj wyniku działania `LocalRedirect`, aby zapobiec atakom typu "Open redirect".</span><span class="sxs-lookup"><span data-stu-id="cd982-727">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="cd982-728">Aby uzyskać więcej informacji, zobacz <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="cd982-728">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="cd982-729">Poniższy składnik przedstawia przykład sposobu wykonywania wstępnego przekierowania, gdy użytkownik wybierze kulturę:</span><span class="sxs-lookup"><span data-stu-id="cd982-729">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="cd982-730">Korzystanie z scenariuszy lokalizacji platformy .NET w aplikacjach Blazor</span><span class="sxs-lookup"><span data-stu-id="cd982-730">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="cd982-731">W aplikacjach Blazor dostępne są następujące scenariusze dotyczące lokalizacji i globalizacji platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="cd982-731">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="cd982-732">. System zasobów netto</span><span class="sxs-lookup"><span data-stu-id="cd982-732">.NET's resources system</span></span>
* <span data-ttu-id="cd982-733">Formatowanie liczb i dat specyficznych dla kultury</span><span class="sxs-lookup"><span data-stu-id="cd982-733">Culture-specific number and date formatting</span></span>

<span data-ttu-id="cd982-734">Funkcja `@bind` Blazorwykonuje globalizację opartą na bieżącej kulturze użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd982-734">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="cd982-735">Aby uzyskać więcej informacji, zobacz sekcję [powiązanie danych](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="cd982-735">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="cd982-736">Obecnie obsługiwane są ograniczone zestawy ASP.NET Core scenariuszy lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="cd982-736">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="cd982-737">`IStringLocalizer<>` *jest obsługiwana* w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-737">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="cd982-738">Lokalizacja `IHtmlLocalizer<>`, `IViewLocalizer<>`i adnotacji danych są ASP.NET Core scenariusze MVC i **nie są obsługiwane** w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd982-738">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="cd982-739">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="cd982-739">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="cd982-740">Skalowalne obrazy wektorowe (SVG)</span><span class="sxs-lookup"><span data-stu-id="cd982-740">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="cd982-741">Ponieważ Blazor renderuje HTML, obrazy obsługiwane przez przeglądarkę, w tym obrazy*SVG (Scalable*Vector Graphics), są obsługiwane za pośrednictwem tagu `<img>`:</span><span class="sxs-lookup"><span data-stu-id="cd982-741">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="cd982-742">Podobnie Obrazy SVG są obsługiwane w regułach CSS pliku arkusza stylów (*CSS*):</span><span class="sxs-lookup"><span data-stu-id="cd982-742">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="cd982-743">Jednak wbudowane znaczniki SVG nie są obsługiwane we wszystkich scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="cd982-743">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="cd982-744">Jeśli umieścisz tag `<svg>` bezpośrednio w pliku składnika ( *. Razor*), podstawowe renderowanie obrazu jest obsługiwane, ale wiele scenariuszy zaawansowanych nie jest jeszcze obsługiwanych.</span><span class="sxs-lookup"><span data-stu-id="cd982-744">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="cd982-745">Na przykład Tagi `<use>` nie są obecnie przestrzegane i `@bind` nie mogą być używane w przypadku niektórych tagów SVG.</span><span class="sxs-lookup"><span data-stu-id="cd982-745">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="cd982-746">Oczekujemy, że te ograniczenia są opisane w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="cd982-746">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd982-747">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cd982-747">Additional resources</span></span>

* <span data-ttu-id="cd982-748"><xref:security/blazor/server> &ndash; zawiera wskazówki dotyczące tworzenia aplikacji Blazor Server, które muszą będą konkurować o z wyczerpaniem zasobów.</span><span class="sxs-lookup"><span data-stu-id="cd982-748"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
