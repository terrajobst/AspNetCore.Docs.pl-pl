---
title: ASP.NET Core Blazor cyklu życia
author: guardrex
description: Dowiedz się, jak używać metod cyklu życia składnika Razor w aplikacjach Blazor ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/lifecycle
ms.openlocfilehash: e600e7c7a6a8c646a655520bd5c127f2cd662753
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944034"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="ff9e1-103">ASP.NET Core Blazor cyklu życia</span><span class="sxs-lookup"><span data-stu-id="ff9e1-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="ff9e1-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ff9e1-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="ff9e1-105">Platforma Blazor obejmuje metody cyklu życia synchronicznego i asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="ff9e1-106">Zastąp metody cyklu życia, aby wykonać dodatkowe operacje na składnikach podczas inicjowania i renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="ff9e1-107">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="ff9e1-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="ff9e1-108">Metody inicjujące składniki</span><span class="sxs-lookup"><span data-stu-id="ff9e1-108">Component initialization methods</span></span>

<span data-ttu-id="ff9e1-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> wykonywania kodu, który inicjuje składnik.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> execute code that initializes a component.</span></span> <span data-ttu-id="ff9e1-110">Te metody są wywoływane tylko raz podczas pierwszego wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-110">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="ff9e1-111">Aby wykonać operację asynchroniczną, użyj `OnInitializedAsync` i słowa kluczowego `await` w operacji:</span><span class="sxs-lookup"><span data-stu-id="ff9e1-111">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="ff9e1-112">Asynchroniczne działanie podczas inicjowania składnika musi wystąpić w trakcie `OnInitializedAsync`go zdarzenia cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-112">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="ff9e1-113">W przypadku operacji synchronicznej Użyj `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="ff9e1-113">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="ff9e1-114">Przed ustawieniem parametrów</span><span class="sxs-lookup"><span data-stu-id="ff9e1-114">Before parameters are set</span></span>

<span data-ttu-id="ff9e1-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> ustawia parametry dostarczone przez element nadrzędny składnika w drzewie renderowania:</span><span class="sxs-lookup"><span data-stu-id="ff9e1-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="ff9e1-116"><xref:Microsoft.AspNetCore.Components.ParameterView> zawiera cały zbiór wartości parametrów każdorazowo po wywołaniu metody `SetParametersAsync`.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="ff9e1-117">Domyślna implementacja `SetParametersAsync` ustawia wartość każdej właściwości z atrybutem `[Parameter]` lub `[CascadingParameter]`, który ma odpowiednią wartość w `ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-117">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="ff9e1-118">Parametry, które nie mają odpowiadającej wartości w `ParameterView` są pozostawione bez zmian.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="ff9e1-119">Jeśli `base.SetParametersAync` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="ff9e1-120">Na przykład nie jest wymagane przypisanie parametrów przychodzących do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="ff9e1-121">Po ustawieniu parametrów</span><span class="sxs-lookup"><span data-stu-id="ff9e1-121">After parameters are set</span></span>

<span data-ttu-id="ff9e1-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> są wywoływane, gdy składnik otrzymał parametry z jego elementu nadrzędnego, a wartości są przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="ff9e1-123">Te metody są wykonywane po zainicjowaniu składnika i każdej chwili, gdy są określone nowe wartości parametrów:</span><span class="sxs-lookup"><span data-stu-id="ff9e1-123">These methods are executed after component initialization and each time new parameter values are specified:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="ff9e1-124">Asynchroniczne działanie, gdy stosowane są parametry i wartości właściwości w trakcie `OnParametersSetAsync`go zdarzenia cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-124">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="ff9e1-125">Po renderowania składników</span><span class="sxs-lookup"><span data-stu-id="ff9e1-125">After component render</span></span>

<span data-ttu-id="ff9e1-126"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-126"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="ff9e1-127">Odwołania do elementów i składników są wypełniane w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-127">Element and component references are populated at this point.</span></span> <span data-ttu-id="ff9e1-128">Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-128">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="ff9e1-129">`firstRender` parametr `OnAfterRenderAsync` i `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="ff9e1-129">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="ff9e1-130">Jest ustawiony na `true` podczas pierwszego renderowania wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-130">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="ff9e1-131">Można go użyć, aby upewnić się, że zadania inicjowania są wykonywane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-131">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="ff9e1-132">Asynchroniczne działanie natychmiast po wyrenderowaniu musi wystąpić w trakcie zdarzenia cyklu życia `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-132">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="ff9e1-133">Nawet jeśli zwracasz <xref:System.Threading.Tasks.Task> z `OnAfterRenderAsync`, struktura nie zaplanuje dalszych cykli renderowania dla składnika po zakończeniu tego zadania.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-133">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="ff9e1-134">Ma to na celu uniknięcie nieskończonej pętli renderowania.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-134">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="ff9e1-135">Różnią się one od innych metod cyklu życia, które Zaplanuj kolejny cykl renderowania po zakończeniu zwracanego zadania.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-135">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="ff9e1-136">`OnAfterRender` i `OnAfterRenderAsync` *nie są wywoływane podczas renderowania na serwerze.*</span><span class="sxs-lookup"><span data-stu-id="ff9e1-136">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="ff9e1-137">Pomiń odświeżanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="ff9e1-137">Suppress UI refreshing</span></span>

<span data-ttu-id="ff9e1-138">Zastąp <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>, aby pominąć odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-138">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="ff9e1-139">Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony:</span><span class="sxs-lookup"><span data-stu-id="ff9e1-139">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="ff9e1-140">`ShouldRender` jest wywoływana za każdym razem, gdy składnik jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-140">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="ff9e1-141">Nawet jeśli `ShouldRender` jest zastępowana, składnik jest zawsze początkowo renderowany.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-141">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="ff9e1-142">Zmiany stanu</span><span class="sxs-lookup"><span data-stu-id="ff9e1-142">State changes</span></span>

<span data-ttu-id="ff9e1-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> powiadamia składnik o zmianie jego stanu.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="ff9e1-144">Jeśli ma to zastosowanie, wywołanie `StateHasChanged` powoduje, że składnik zostanie przerenderowany.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-144">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="ff9e1-145">Obsługuj niekompletne akcje asynchroniczne podczas renderowania</span><span class="sxs-lookup"><span data-stu-id="ff9e1-145">Handle incomplete async actions at render</span></span>

<span data-ttu-id="ff9e1-146">Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogły nie zostać ukończone przed renderowaniem składnika.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-146">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="ff9e1-147">Obiekty mogą być `null` lub uzupełniane z danymi podczas wykonywania metody cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-147">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="ff9e1-148">Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-148">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="ff9e1-149">Renderowanie zastępczych elementów interfejsu użytkownika (na przykład komunikatów ładowania) podczas `null`obiektów.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-149">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="ff9e1-150">W `FetchData` składniku szablonów Blazor `OnInitializedAsync` został zastąpiony asynchronicznie odbierania danych prognozy (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="ff9e1-150">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="ff9e1-151">Gdy `forecasts` jest `null`, zostanie wyświetlony komunikat ładowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-151">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="ff9e1-152">Po `Task` zwrócone przez `OnInitializedAsync` zostanie wykonane, składnik zostanie przerenderowany ze zaktualizowanym stanem.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-152">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="ff9e1-153">*Pages/FetchData. Razor* w szablonie serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="ff9e1-153">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="ff9e1-154">Usuwanie składnika z interfejsem IDisposable</span><span class="sxs-lookup"><span data-stu-id="ff9e1-154">Component disposal with IDisposable</span></span>

<span data-ttu-id="ff9e1-155">Jeśli składnik implementuje <xref:System.IDisposable>, [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-155">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="ff9e1-156">Poniższy składnik używa `@implements IDisposable` i metody `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="ff9e1-156">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="ff9e1-157">Wywoływanie <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> w `Dispose` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-157">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="ff9e1-158">`StateHasChanged` mogą być wywoływane w ramach rozrywania modułu renderowania, dlatego żądanie aktualizacji interfejsu użytkownika nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-158">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="ff9e1-159">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="ff9e1-159">Handle errors</span></span>

<span data-ttu-id="ff9e1-160">Aby uzyskać informacje na temat obsługi błędów podczas wykonywania metody cyklu życia, zobacz <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="ff9e1-160">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
