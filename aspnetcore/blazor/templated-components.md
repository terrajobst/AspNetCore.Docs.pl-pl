---
title: ASP.NET Core Blazor elementy szablonu
author: guardrex
description: Dowiedz się, w jaki sposób składniki szablonu mogą akceptować jeden lub więcej szablonów interfejsu użytkownika jako parametry, które mogą być następnie używane jako część logiki renderowania składnika.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templated-components
ms.openlocfilehash: b64d6a731e540b13c50b2c6108f75efd0ac9290c
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453227"
---
# <a name="aspnet-core-opno-locblazor-templated-components"></a><span data-ttu-id="d3486-103">ASP.NET Core Blazor elementy szablonu</span><span class="sxs-lookup"><span data-stu-id="d3486-103">ASP.NET Core Blazor templated components</span></span>

<span data-ttu-id="d3486-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d3486-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="d3486-105">Składniki z szablonami są składnikami, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które mogą być następnie używane jako część logiki renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="d3486-105">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="d3486-106">Składniki z szablonami umożliwiają tworzenie składników wyższego poziomu, które są większe niż zwykłe składniki.</span><span class="sxs-lookup"><span data-stu-id="d3486-106">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="d3486-107">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="d3486-107">A couple of examples include:</span></span>

* <span data-ttu-id="d3486-108">Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka, wierszy i stopki tabeli.</span><span class="sxs-lookup"><span data-stu-id="d3486-108">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="d3486-109">Składnik listy, który umożliwia użytkownikowi określenie szablonu do renderowania elementów na liście.</span><span class="sxs-lookup"><span data-stu-id="d3486-109">A list component that allows a user to specify a template for rendering items in a list.</span></span>

## <a name="template-parameters"></a><span data-ttu-id="d3486-110">Parametry szablonu</span><span class="sxs-lookup"><span data-stu-id="d3486-110">Template parameters</span></span>

<span data-ttu-id="d3486-111">Składnik szablonu jest definiowany przez określenie co najmniej jednego parametru składnika typu `RenderFragment` lub `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="d3486-111">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="d3486-112">Fragment renderowania reprezentuje segment interfejsu użytkownika do renderowania.</span><span class="sxs-lookup"><span data-stu-id="d3486-112">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="d3486-113">`RenderFragment<T>` przyjmuje parametr typu, który można określić podczas wywoływania fragmentu renderowania.</span><span class="sxs-lookup"><span data-stu-id="d3486-113">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="d3486-114">składnik `TableTemplate`:</span><span class="sxs-lookup"><span data-stu-id="d3486-114">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="d3486-115">W przypadku korzystania z składnika z szablonem parametry szablonu można określić za pomocą elementów podrzędnych, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):</span><span class="sxs-lookup"><span data-stu-id="d3486-115">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

## <a name="template-context-parameters"></a><span data-ttu-id="d3486-116">Parametry kontekstu szablonu</span><span class="sxs-lookup"><span data-stu-id="d3486-116">Template context parameters</span></span>

<span data-ttu-id="d3486-117">Argumenty składnika typu `RenderFragment<T>` przekazane jako elementy mają niejawny parametr o nazwie `context` (na przykład z poprzedniego przykładu kodu, `@context.PetId`), ale można zmienić nazwę parametru przy użyciu atrybutu `Context` elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d3486-117">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="d3486-118">W poniższym przykładzie atrybut `Context` elementu `RowTemplate` określa `pet` parametr:</span><span class="sxs-lookup"><span data-stu-id="d3486-118">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="d3486-119">Alternatywnie można określić atrybut `Context` dla elementu składnika.</span><span class="sxs-lookup"><span data-stu-id="d3486-119">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="d3486-120">Określony atrybut `Context` ma zastosowanie do wszystkich parametrów określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="d3486-120">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="d3486-121">Może to być przydatne, jeśli chcesz określić nazwę parametru zawartości dla niejawnej zawartości podrzędnej (bez żadnego elementu podrzędnego otoki).</span><span class="sxs-lookup"><span data-stu-id="d3486-121">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="d3486-122">W poniższym przykładzie atrybut `Context` pojawia się na elemencie `TableTemplate` i ma zastosowanie do wszystkich parametrów szablonu:</span><span class="sxs-lookup"><span data-stu-id="d3486-122">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

## <a name="generic-typed-components"></a><span data-ttu-id="d3486-123">Składniki typu rodzajowego</span><span class="sxs-lookup"><span data-stu-id="d3486-123">Generic-typed components</span></span>

<span data-ttu-id="d3486-124">Składniki z szablonami są często wpisywane ogólnie.</span><span class="sxs-lookup"><span data-stu-id="d3486-124">Templated components are often generically typed.</span></span> <span data-ttu-id="d3486-125">Na przykład ogólny składnik `ListViewTemplate` może służyć do renderowania `IEnumerable<T>` wartości.</span><span class="sxs-lookup"><span data-stu-id="d3486-125">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="d3486-126">Aby zdefiniować składnik ogólny, użyj dyrektywy [`@typeparam`](xref:mvc/views/razor#typeparam) , aby określić parametry typu:</span><span class="sxs-lookup"><span data-stu-id="d3486-126">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="d3486-127">W przypadku używania składników o typie ogólnym parametr typu jest wnioskowany, jeśli jest to możliwe:</span><span class="sxs-lookup"><span data-stu-id="d3486-127">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="d3486-128">W przeciwnym razie parametr typu musi być jawnie określony przy użyciu atrybutu, który jest zgodny z nazwą parametru typu.</span><span class="sxs-lookup"><span data-stu-id="d3486-128">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="d3486-129">W poniższym przykładzie `TItem="Pet"` określa typ:</span><span class="sxs-lookup"><span data-stu-id="d3486-129">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
