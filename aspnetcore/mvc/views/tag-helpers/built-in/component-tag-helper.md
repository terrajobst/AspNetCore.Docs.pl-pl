---
title: Pomocnik tagu składnika w ASP.NET Core
author: guardrex
ms.author: riande
description: Dowiedz się, jak używać pomocnika tagów składnika ASP.NET Core, aby renderować składniki Razor na stronach i widokach.
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 801ceb73de5bb4ef7500624e1fbddbf96d1ab89c
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226397"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="42f8c-103">Pomocnik tagu składnika w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42f8c-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="42f8c-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="42f8c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="42f8c-105">Aby renderować składnik ze strony lub widoku, użyj [pomocnika tagów składnika](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span><span class="sxs-lookup"><span data-stu-id="42f8c-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

<span data-ttu-id="42f8c-106">Poniższy pomocnik tagów składnika renderuje składnik `Counter` na stronie lub w widoku:</span><span class="sxs-lookup"><span data-stu-id="42f8c-106">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="42f8c-107">Pomocnik tagów składnika może również przekazywać parametry do składników.</span><span class="sxs-lookup"><span data-stu-id="42f8c-107">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="42f8c-108">Rozważmy następujący składnik `ColorfulCheckbox`, który ustawia kolor i rozmiar etykiety pola wyboru:</span><span class="sxs-lookup"><span data-stu-id="42f8c-108">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

```razor
<label style="font-size:@(Size)px;color:@Color">
    <input @bind="Value"
           id="survey" 
           name="blazor" 
           type="checkbox" />
    Enjoying Blazor?
</label>

@code {
    [Parameter]
    public bool Value { get; set; }

    [Parameter]
    public int Size { get; set; } = 8;

    [Parameter]
    public string Color { get; set; }

    protected override void OnInitialized()
    {
        Size += 10;
    }
}
```

<span data-ttu-id="42f8c-109">[Parametry składnika](xref:blazor/components#component-parameters) `Size` (`int`) i `Color` (`string`) mogą być ustawiane przez pomocnika tagów składnika:</span><span class="sxs-lookup"><span data-stu-id="42f8c-109">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="42f8c-110">Następujący kod HTML jest renderowany na stronie lub w widoku:</span><span class="sxs-lookup"><span data-stu-id="42f8c-110">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="42f8c-111">Przekazywanie ciągu w cudzysłowie wymaga [jawnego wyrażenia Razor](xref:mvc/views/razor#explicit-razor-expressions), jak pokazano dla `param-Color` w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="42f8c-111">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="42f8c-112">Zachowanie analizy Razor dla wartości typu `string` nie ma zastosowania do atrybutu `param-*`, ponieważ atrybut jest typem `object`.</span><span class="sxs-lookup"><span data-stu-id="42f8c-112">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="42f8c-113">Typ parametru musi być możliwy do serializacji JSON, co oznacza, że typ musi mieć domyślny Konstruktor i właściwości settable.</span><span class="sxs-lookup"><span data-stu-id="42f8c-113">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="42f8c-114">Na przykład można określić wartość `Size` i `Color` w poprzednim przykładzie, ponieważ typy `Size` i `Color` są typami pierwotnymi (`int` i `string`), które są obsługiwane przez serializator JSON.</span><span class="sxs-lookup"><span data-stu-id="42f8c-114">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="42f8c-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="42f8c-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="42f8c-116">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="42f8c-116">Is prerendered into the page.</span></span>
* <span data-ttu-id="42f8c-117">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="42f8c-117">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="42f8c-118">Tryb renderowania</span><span class="sxs-lookup"><span data-stu-id="42f8c-118">Render Mode</span></span> | <span data-ttu-id="42f8c-119">Opis</span><span class="sxs-lookup"><span data-stu-id="42f8c-119">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="42f8c-120">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="42f8c-120">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="42f8c-121">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="42f8c-121">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="42f8c-122">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="42f8c-122">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="42f8c-123">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="42f8c-123">Output from the component isn't included.</span></span> <span data-ttu-id="42f8c-124">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="42f8c-124">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="42f8c-125">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="42f8c-125">Renders the component into static HTML.</span></span> |

<span data-ttu-id="42f8c-126">Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true".</span><span class="sxs-lookup"><span data-stu-id="42f8c-126">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="42f8c-127">Składniki nie mogą korzystać z funkcji specjalnych, takich jak widoki częściowe i sekcje.</span><span class="sxs-lookup"><span data-stu-id="42f8c-127">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="42f8c-128">Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.</span><span class="sxs-lookup"><span data-stu-id="42f8c-128">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="42f8c-129">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="42f8c-129">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="42f8c-130">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="42f8c-130">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
