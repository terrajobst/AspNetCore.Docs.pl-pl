---
title: ASP.NET Core Blazor cyklu życia
author: guardrex
description: Dowiedz się, jak używać metod cyklu życia składnika Razor w aplikacjach Blazor ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: df5bb676df59b538179a69978040521c4ee78ed1
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146371"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="589bf-103">ASP.NET Core Blazor cyklu życia</span><span class="sxs-lookup"><span data-stu-id="589bf-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="589bf-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="589bf-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="589bf-105">Platforma Blazor obejmuje metody cyklu życia synchronicznego i asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="589bf-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="589bf-106">Zastąp metody cyklu życia, aby wykonać dodatkowe operacje na składnikach podczas inicjowania i renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="589bf-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="589bf-107">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="589bf-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="589bf-108">Metody inicjujące składniki</span><span class="sxs-lookup"><span data-stu-id="589bf-108">Component initialization methods</span></span>

<span data-ttu-id="589bf-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> są wywoływane, gdy składnik zostanie zainicjowany po odebraniu początkowych parametrów z jego składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="589bf-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="589bf-110">Użyj `OnInitializedAsync`, gdy składnik wykonuje operację asynchroniczną i powinien być odświeżany po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="589bf-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span> <span data-ttu-id="589bf-111">Te metody są wywoływane tylko raz podczas pierwszego wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="589bf-111">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="589bf-112">W przypadku operacji synchronicznej Przesłoń `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="589bf-112">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="589bf-113">Aby wykonać operację asynchroniczną, Przesłoń `OnInitializedAsync` i użyj słowa kluczowego `await` w operacji:</span><span class="sxs-lookup"><span data-stu-id="589bf-113">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="589bf-114">Przed ustawieniem parametrów</span><span class="sxs-lookup"><span data-stu-id="589bf-114">Before parameters are set</span></span>

<span data-ttu-id="589bf-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> ustawia parametry dostarczone przez element nadrzędny składnika w drzewie renderowania:</span><span class="sxs-lookup"><span data-stu-id="589bf-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="589bf-116"><xref:Microsoft.AspNetCore.Components.ParameterView> zawiera cały zbiór wartości parametrów każdorazowo po wywołaniu metody `SetParametersAsync`.</span><span class="sxs-lookup"><span data-stu-id="589bf-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="589bf-117">Domyślna implementacja `SetParametersAsync` ustawia wartość każdej właściwości z atrybutem `[Parameter]` lub `[CascadingParameter]`, który ma odpowiednią wartość w `ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="589bf-117">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="589bf-118">Parametry, które nie mają odpowiadającej wartości w `ParameterView` są pozostawione bez zmian.</span><span class="sxs-lookup"><span data-stu-id="589bf-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="589bf-119">Jeśli `base.SetParametersAync` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="589bf-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="589bf-120">Na przykład nie jest wymagane przypisanie parametrów przychodzących do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="589bf-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="589bf-121">Po ustawieniu parametrów</span><span class="sxs-lookup"><span data-stu-id="589bf-121">After parameters are set</span></span>

<span data-ttu-id="589bf-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> są wywoływane:</span><span class="sxs-lookup"><span data-stu-id="589bf-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="589bf-123">Gdy składnik jest zainicjowany i odebrał swój pierwszy zestaw parametrów z jego składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="589bf-123">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="589bf-124">Po ponownym wyrenderowaniu i zaopatrzeniu składnika nadrzędnego:</span><span class="sxs-lookup"><span data-stu-id="589bf-124">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="589bf-125">Tylko znane niezmienne typy pierwotne, których co najmniej jeden parametr został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="589bf-125">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="589bf-126">Wszystkie parametry złożone z typem.</span><span class="sxs-lookup"><span data-stu-id="589bf-126">Any complex-typed parameters.</span></span> <span data-ttu-id="589bf-127">Struktura nie może wiedzieć, czy wartości parametru złożonego są mutacją wewnętrznie, dlatego traktuje zestaw parametrów jako zmieniony.</span><span class="sxs-lookup"><span data-stu-id="589bf-127">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="589bf-128">Asynchroniczne działanie, gdy stosowane są parametry i wartości właściwości w trakcie `OnParametersSetAsync`go zdarzenia cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="589bf-128">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="589bf-129">Po renderowania składników</span><span class="sxs-lookup"><span data-stu-id="589bf-129">After component render</span></span>

<span data-ttu-id="589bf-130"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="589bf-130"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="589bf-131">Odwołania do elementów i składników są wypełniane w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="589bf-131">Element and component references are populated at this point.</span></span> <span data-ttu-id="589bf-132">Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.</span><span class="sxs-lookup"><span data-stu-id="589bf-132">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="589bf-133">`firstRender` parametr `OnAfterRenderAsync` i `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="589bf-133">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="589bf-134">Jest ustawiony na `true` podczas pierwszego renderowania wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="589bf-134">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="589bf-135">Można go użyć, aby upewnić się, że zadania inicjowania są wykonywane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="589bf-135">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="589bf-136">Asynchroniczne działanie natychmiast po wyrenderowaniu musi wystąpić w trakcie zdarzenia cyklu życia `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="589bf-136">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="589bf-137">Nawet jeśli zwracasz <xref:System.Threading.Tasks.Task> z `OnAfterRenderAsync`, struktura nie zaplanuje dalszych cykli renderowania dla składnika po zakończeniu tego zadania.</span><span class="sxs-lookup"><span data-stu-id="589bf-137">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="589bf-138">Ma to na celu uniknięcie nieskończonej pętli renderowania.</span><span class="sxs-lookup"><span data-stu-id="589bf-138">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="589bf-139">Różnią się one od innych metod cyklu życia, które Zaplanuj kolejny cykl renderowania po zakończeniu zwracanego zadania.</span><span class="sxs-lookup"><span data-stu-id="589bf-139">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="589bf-140">`OnAfterRender` i `OnAfterRenderAsync` *nie są wywoływane podczas renderowania na serwerze.*</span><span class="sxs-lookup"><span data-stu-id="589bf-140">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="589bf-141">Pomiń odświeżanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="589bf-141">Suppress UI refreshing</span></span>

<span data-ttu-id="589bf-142">Zastąp <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>, aby pominąć odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="589bf-142">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="589bf-143">Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony:</span><span class="sxs-lookup"><span data-stu-id="589bf-143">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="589bf-144">`ShouldRender` jest wywoływana za każdym razem, gdy składnik jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="589bf-144">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="589bf-145">Nawet jeśli `ShouldRender` jest zastępowana, składnik jest zawsze początkowo renderowany.</span><span class="sxs-lookup"><span data-stu-id="589bf-145">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="589bf-146">Zmiany stanu</span><span class="sxs-lookup"><span data-stu-id="589bf-146">State changes</span></span>

<span data-ttu-id="589bf-147"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> powiadamia składnik o zmianie jego stanu.</span><span class="sxs-lookup"><span data-stu-id="589bf-147"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="589bf-148">Jeśli ma to zastosowanie, wywołanie `StateHasChanged` powoduje, że składnik zostanie przerenderowany.</span><span class="sxs-lookup"><span data-stu-id="589bf-148">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="589bf-149">Obsługuj niekompletne akcje asynchroniczne podczas renderowania</span><span class="sxs-lookup"><span data-stu-id="589bf-149">Handle incomplete async actions at render</span></span>

<span data-ttu-id="589bf-150">Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogły nie zostać ukończone przed renderowaniem składnika.</span><span class="sxs-lookup"><span data-stu-id="589bf-150">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="589bf-151">Obiekty mogą być `null` lub uzupełniane z danymi podczas wykonywania metody cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="589bf-151">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="589bf-152">Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane.</span><span class="sxs-lookup"><span data-stu-id="589bf-152">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="589bf-153">Renderowanie zastępczych elementów interfejsu użytkownika (na przykład komunikatów ładowania) podczas `null`obiektów.</span><span class="sxs-lookup"><span data-stu-id="589bf-153">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="589bf-154">W `FetchData` składniku szablonów Blazor `OnInitializedAsync` został zastąpiony asynchronicznie odbierania danych prognozy (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="589bf-154">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="589bf-155">Gdy `forecasts` jest `null`, zostanie wyświetlony komunikat ładowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="589bf-155">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="589bf-156">Po `Task` zwrócone przez `OnInitializedAsync` zostanie wykonane, składnik zostanie przerenderowany ze zaktualizowanym stanem.</span><span class="sxs-lookup"><span data-stu-id="589bf-156">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="589bf-157">*Pages/FetchData. Razor* w szablonie serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="589bf-157">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="589bf-158">Usuwanie składnika z interfejsem IDisposable</span><span class="sxs-lookup"><span data-stu-id="589bf-158">Component disposal with IDisposable</span></span>

<span data-ttu-id="589bf-159">Jeśli składnik implementuje <xref:System.IDisposable>, [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="589bf-159">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="589bf-160">Poniższy składnik używa `@implements IDisposable` i metody `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="589bf-160">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```razor
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
> <span data-ttu-id="589bf-161">Wywoływanie <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> w `Dispose` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="589bf-161">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="589bf-162">`StateHasChanged` mogą być wywoływane w ramach rozrywania modułu renderowania, dlatego żądanie aktualizacji interfejsu użytkownika nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="589bf-162">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="589bf-163">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="589bf-163">Handle errors</span></span>

<span data-ttu-id="589bf-164">Aby uzyskać informacje na temat obsługi błędów podczas wykonywania metody cyklu życia, zobacz <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="589bf-164">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
