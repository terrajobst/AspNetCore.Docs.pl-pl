---
title: ASP.NET Core Blazor cyklu życia
author: guardrex
description: Dowiedz się, jak używać metod cyklu życia składnika Razor w aplikacjach Blazor ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: 831f575afa6ce11d06c016d43ecd1bb59d09eab6
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218911"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="e247c-103">ASP.NET Core Blazor cyklu życia</span><span class="sxs-lookup"><span data-stu-id="e247c-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="e247c-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="e247c-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="e247c-105">Platforma Blazor obejmuje metody cyklu życia synchronicznego i asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="e247c-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="e247c-106">Zastąp metody cyklu życia, aby wykonać dodatkowe operacje na składnikach podczas inicjowania i renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="e247c-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="e247c-107">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="e247c-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="e247c-108">Metody inicjujące składniki</span><span class="sxs-lookup"><span data-stu-id="e247c-108">Component initialization methods</span></span>

<span data-ttu-id="e247c-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> są wywoływane, gdy składnik zostanie zainicjowany po odebraniu początkowych parametrów z jego składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="e247c-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="e247c-110">Użyj `OnInitializedAsync`, gdy składnik wykonuje operację asynchroniczną i powinien być odświeżany po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="e247c-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span>

<span data-ttu-id="e247c-111">W przypadku operacji synchronicznej Przesłoń `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="e247c-111">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="e247c-112">Aby wykonać operację asynchroniczną, Przesłoń `OnInitializedAsync` i użyj słowa kluczowego `await` w operacji:</span><span class="sxs-lookup"><span data-stu-id="e247c-112">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor<span data-ttu-id="e247c-113"> aplikacji serwerowych [, które](xref:blazor/hosting-model-configuration#render-mode) `OnInitializedAsync` **_dwa razy_** :</span><span class="sxs-lookup"><span data-stu-id="e247c-113"> Server apps that [prerender their content](xref:blazor/hosting-model-configuration#render-mode) call `OnInitializedAsync` **_twice_**:</span></span>

* <span data-ttu-id="e247c-114">Gdy składnik jest początkowo renderowany statycznie jako część strony.</span><span class="sxs-lookup"><span data-stu-id="e247c-114">Once when the component is initially rendered statically as part of the page.</span></span>
* <span data-ttu-id="e247c-115">Drugi raz, gdy przeglądarka nawiąże połączenie z serwerem.</span><span class="sxs-lookup"><span data-stu-id="e247c-115">A second time when the browser establishes a connection back to the server.</span></span>

<span data-ttu-id="e247c-116">Aby zapobiec dwukrotnemu uruchomieniu kodu dewelopera w `OnInitializedAsync`, zobacz sekcję [stan ponownego połączenia po przeprowadzeniu prerenderowania](#stateful-reconnection-after-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="e247c-116">To prevent developer code in `OnInitializedAsync` from running twice, see the [Stateful reconnection after prerendering](#stateful-reconnection-after-prerendering) section.</span></span>

<span data-ttu-id="e247c-117">Gdy aplikacja serwera Blazor jest wstępnie renderowana, niektóre akcje, takie jak wywoływanie kodu JavaScript, nie są możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="e247c-117">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="e247c-118">Składniki mogą być konieczne w różny sposób, gdy są wstępnie renderowane.</span><span class="sxs-lookup"><span data-stu-id="e247c-118">Components may need to render differently when prerendered.</span></span> <span data-ttu-id="e247c-119">Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, gdy aplikacja jest prerenderowana](#detect-when-the-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="e247c-119">For more information, see the [Detect when the app is prerendering](#detect-when-the-app-is-prerendering) section.</span></span>

<span data-ttu-id="e247c-120">W przypadku skonfigurowania dowolnych programów obsługi zdarzeń odłączanie ich do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="e247c-120">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="e247c-121">Aby uzyskać więcej informacji, zobacz sekcję [Usuwanie składnika z](#component-disposal-with-idisposable) interfejsem IDisposable.</span><span class="sxs-lookup"><span data-stu-id="e247c-121">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="before-parameters-are-set"></a><span data-ttu-id="e247c-122">Przed ustawieniem parametrów</span><span class="sxs-lookup"><span data-stu-id="e247c-122">Before parameters are set</span></span>

<span data-ttu-id="e247c-123"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> ustawia parametry dostarczone przez element nadrzędny składnika w drzewie renderowania:</span><span class="sxs-lookup"><span data-stu-id="e247c-123"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="e247c-124"><xref:Microsoft.AspNetCore.Components.ParameterView> zawiera cały zbiór wartości parametrów każdorazowo po wywołaniu metody `SetParametersAsync`.</span><span class="sxs-lookup"><span data-stu-id="e247c-124"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="e247c-125">Domyślna implementacja `SetParametersAsync` ustawia wartość każdej właściwości z atrybutem `[Parameter]` lub `[CascadingParameter]`, który ma odpowiednią wartość w `ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="e247c-125">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="e247c-126">Parametry, które nie mają odpowiadającej wartości w `ParameterView` są pozostawione bez zmian.</span><span class="sxs-lookup"><span data-stu-id="e247c-126">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="e247c-127">Jeśli `base.SetParametersAync` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="e247c-127">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="e247c-128">Na przykład nie jest wymagane przypisanie parametrów przychodzących do właściwości w klasie.</span><span class="sxs-lookup"><span data-stu-id="e247c-128">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

<span data-ttu-id="e247c-129">W przypadku skonfigurowania dowolnych programów obsługi zdarzeń odłączanie ich do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="e247c-129">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="e247c-130">Aby uzyskać więcej informacji, zobacz sekcję [Usuwanie składnika z](#component-disposal-with-idisposable) interfejsem IDisposable.</span><span class="sxs-lookup"><span data-stu-id="e247c-130">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="e247c-131">Po ustawieniu parametrów</span><span class="sxs-lookup"><span data-stu-id="e247c-131">After parameters are set</span></span>

<span data-ttu-id="e247c-132"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> są wywoływane:</span><span class="sxs-lookup"><span data-stu-id="e247c-132"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="e247c-133">Gdy składnik jest zainicjowany i odebrał swój pierwszy zestaw parametrów z jego składnika nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="e247c-133">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="e247c-134">Po ponownym wyrenderowaniu i zaopatrzeniu składnika nadrzędnego:</span><span class="sxs-lookup"><span data-stu-id="e247c-134">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="e247c-135">Tylko znane niezmienne typy pierwotne, których co najmniej jeden parametr został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="e247c-135">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="e247c-136">Wszystkie parametry złożone z typem.</span><span class="sxs-lookup"><span data-stu-id="e247c-136">Any complex-typed parameters.</span></span> <span data-ttu-id="e247c-137">Struktura nie może wiedzieć, czy wartości parametru złożonego są mutacją wewnętrznie, dlatego traktuje zestaw parametrów jako zmieniony.</span><span class="sxs-lookup"><span data-stu-id="e247c-137">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="e247c-138">Asynchroniczne działanie, gdy stosowane są parametry i wartości właściwości w trakcie `OnParametersSetAsync`go zdarzenia cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="e247c-138">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="e247c-139">W przypadku skonfigurowania dowolnych programów obsługi zdarzeń odłączanie ich do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="e247c-139">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="e247c-140">Aby uzyskać więcej informacji, zobacz sekcję [Usuwanie składnika z](#component-disposal-with-idisposable) interfejsem IDisposable.</span><span class="sxs-lookup"><span data-stu-id="e247c-140">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="after-component-render"></a><span data-ttu-id="e247c-141">Po renderowania składników</span><span class="sxs-lookup"><span data-stu-id="e247c-141">After component render</span></span>

<span data-ttu-id="e247c-142"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> są wywoływane po zakończeniu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="e247c-142"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="e247c-143">Odwołania do elementów i składników są wypełniane w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="e247c-143">Element and component references are populated at this point.</span></span> <span data-ttu-id="e247c-144">Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.</span><span class="sxs-lookup"><span data-stu-id="e247c-144">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="e247c-145">`firstRender` parametr `OnAfterRenderAsync` i `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="e247c-145">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="e247c-146">Jest ustawiony na `true` podczas pierwszego renderowania wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="e247c-146">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="e247c-147">Można go użyć, aby upewnić się, że zadania inicjowania są wykonywane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="e247c-147">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="e247c-148">Asynchroniczne działanie natychmiast po wyrenderowaniu musi wystąpić w trakcie zdarzenia cyklu życia `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="e247c-148">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="e247c-149">Nawet jeśli zwracasz <xref:System.Threading.Tasks.Task> z `OnAfterRenderAsync`, struktura nie zaplanuje dalszych cykli renderowania dla składnika po zakończeniu tego zadania.</span><span class="sxs-lookup"><span data-stu-id="e247c-149">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="e247c-150">Ma to na celu uniknięcie nieskończonej pętli renderowania.</span><span class="sxs-lookup"><span data-stu-id="e247c-150">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="e247c-151">Różnią się one od innych metod cyklu życia, które Zaplanuj kolejny cykl renderowania po zakończeniu zwracanego zadania.</span><span class="sxs-lookup"><span data-stu-id="e247c-151">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="e247c-152">`OnAfterRender` i `OnAfterRenderAsync` *nie są wywoływane podczas renderowania na serwerze.*</span><span class="sxs-lookup"><span data-stu-id="e247c-152">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

<span data-ttu-id="e247c-153">W przypadku skonfigurowania dowolnych programów obsługi zdarzeń odłączanie ich do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="e247c-153">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="e247c-154">Aby uzyskać więcej informacji, zobacz sekcję [Usuwanie składnika z](#component-disposal-with-idisposable) interfejsem IDisposable.</span><span class="sxs-lookup"><span data-stu-id="e247c-154">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="e247c-155">Pomiń odświeżanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="e247c-155">Suppress UI refreshing</span></span>

<span data-ttu-id="e247c-156">Zastąp <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>, aby pominąć odświeżanie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e247c-156">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="e247c-157">Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony:</span><span class="sxs-lookup"><span data-stu-id="e247c-157">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="e247c-158">`ShouldRender` jest wywoływana za każdym razem, gdy składnik jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="e247c-158">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="e247c-159">Nawet jeśli `ShouldRender` jest zastępowana, składnik jest zawsze początkowo renderowany.</span><span class="sxs-lookup"><span data-stu-id="e247c-159">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="e247c-160">Zmiany stanu</span><span class="sxs-lookup"><span data-stu-id="e247c-160">State changes</span></span>

<span data-ttu-id="e247c-161"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> powiadamia składnik o zmianie jego stanu.</span><span class="sxs-lookup"><span data-stu-id="e247c-161"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="e247c-162">Jeśli ma to zastosowanie, wywołanie `StateHasChanged` powoduje, że składnik zostanie przerenderowany.</span><span class="sxs-lookup"><span data-stu-id="e247c-162">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="e247c-163">Obsługuj niekompletne akcje asynchroniczne podczas renderowania</span><span class="sxs-lookup"><span data-stu-id="e247c-163">Handle incomplete async actions at render</span></span>

<span data-ttu-id="e247c-164">Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogły nie zostać ukończone przed renderowaniem składnika.</span><span class="sxs-lookup"><span data-stu-id="e247c-164">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="e247c-165">Obiekty mogą być `null` lub uzupełniane z danymi podczas wykonywania metody cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="e247c-165">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="e247c-166">Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane.</span><span class="sxs-lookup"><span data-stu-id="e247c-166">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="e247c-167">Renderowanie zastępczych elementów interfejsu użytkownika (na przykład komunikatów ładowania) podczas `null`obiektów.</span><span class="sxs-lookup"><span data-stu-id="e247c-167">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="e247c-168">W `FetchData` składniku szablonów Blazor `OnInitializedAsync` został zastąpiony asynchronicznie odbierania danych prognozy (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="e247c-168">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="e247c-169">Gdy `forecasts` jest `null`, zostanie wyświetlony komunikat ładowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e247c-169">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="e247c-170">Po `Task` zwrócone przez `OnInitializedAsync` zostanie wykonane, składnik zostanie przerenderowany ze zaktualizowanym stanem.</span><span class="sxs-lookup"><span data-stu-id="e247c-170">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="e247c-171">*Pages/FetchData. Razor* w szablonie serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="e247c-171">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="e247c-172">Usuwanie składnika z interfejsem IDisposable</span><span class="sxs-lookup"><span data-stu-id="e247c-172">Component disposal with IDisposable</span></span>

<span data-ttu-id="e247c-173">Jeśli składnik implementuje <xref:System.IDisposable>, [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e247c-173">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="e247c-174">Poniższy składnik używa `@implements IDisposable` i metody `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="e247c-174">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="e247c-175">Wywoływanie <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> w `Dispose` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="e247c-175">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="e247c-176">`StateHasChanged` mogą być wywoływane w ramach rozrywania modułu renderowania, dlatego żądanie aktualizacji interfejsu użytkownika nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="e247c-176">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

<span data-ttu-id="e247c-177">Procedury obsługi zdarzeń anulowania subskrypcji z zdarzeń platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="e247c-177">Unsubscribe event handlers from .NET events.</span></span> <span data-ttu-id="e247c-178">W poniższych przykładach [formularzyBlazor](xref:blazor/forms-validation) pokazano, jak odpiąć procedurę obsługi zdarzeń w metodzie `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="e247c-178">The following [Blazor form](xref:blazor/forms-validation) examples show how to unhook an event handler in the `Dispose` method:</span></span>

* <span data-ttu-id="e247c-179">Pole prywatne i podejście lambda</span><span class="sxs-lookup"><span data-stu-id="e247c-179">Private field and lambda approach</span></span>

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-1.razor?highlight=23,28)]

* <span data-ttu-id="e247c-180">Podejście metody prywatnej</span><span class="sxs-lookup"><span data-stu-id="e247c-180">Private method approach</span></span>

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-2.razor?highlight=16,26)]

## <a name="handle-errors"></a><span data-ttu-id="e247c-181">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="e247c-181">Handle errors</span></span>

<span data-ttu-id="e247c-182">Aby uzyskać informacje na temat obsługi błędów podczas wykonywania metody cyklu życia, zobacz <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="e247c-182">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>

## <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="e247c-183">Stanowe Ponowne nawiązywanie połączenia po przeprowadzeniu prerenderowania</span><span class="sxs-lookup"><span data-stu-id="e247c-183">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="e247c-184">W aplikacji Blazor Server, gdy `RenderMode` jest `ServerPrerendered`, składnik jest początkowo renderowany statycznie jako część strony.</span><span class="sxs-lookup"><span data-stu-id="e247c-184">In a Blazor Server app when `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="e247c-185">Gdy przeglądarka nawiąże połączenie z serwerem, składnik jest renderowany *ponownie*, a składnik jest teraz interaktywny.</span><span class="sxs-lookup"><span data-stu-id="e247c-185">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="e247c-186">Jeśli istnieje metoda cyklu życia " [OnInitialized {Async}](xref:blazor/lifecycle#component-initialization-methods) " dla inicjowania składnika, metoda jest wykonywana *dwukrotnie*:</span><span class="sxs-lookup"><span data-stu-id="e247c-186">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="e247c-187">Gdy składnik jest wstępnie renderowany statycznie.</span><span class="sxs-lookup"><span data-stu-id="e247c-187">When the component is prerendered statically.</span></span>
* <span data-ttu-id="e247c-188">Po nawiązaniu połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="e247c-188">After the server connection has been established.</span></span>

<span data-ttu-id="e247c-189">Może to spowodować zauważalną zmianę danych wyświetlanych w interfejsie użytkownika, gdy składnik jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="e247c-189">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="e247c-190">Aby uniknąć podwójnego renderowania w aplikacji serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="e247c-190">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="e247c-191">Przekaż identyfikator, który może służyć do buforowania stanu podczas wykonywania prerenderowania i pobierania stanu po ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e247c-191">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="e247c-192">Użyj identyfikatora podczas renderowania, aby zapisać stan składnika.</span><span class="sxs-lookup"><span data-stu-id="e247c-192">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="e247c-193">Użyj identyfikatora po włączeniu, aby pobrać buforowany stan.</span><span class="sxs-lookup"><span data-stu-id="e247c-193">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="e247c-194">Poniższy kod ilustruje zaktualizowany `WeatherForecastService` w aplikacji serwerowej Blazor opartej na szablonach, która pozwala uniknąć podwójnego renderowania:</span><span class="sxs-lookup"><span data-stu-id="e247c-194">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

<span data-ttu-id="e247c-195">Aby uzyskać więcej informacji na `RenderMode`, zobacz <xref:blazor/hosting-model-configuration#render-mode>.</span><span class="sxs-lookup"><span data-stu-id="e247c-195">For more information on the `RenderMode`, see <xref:blazor/hosting-model-configuration#render-mode>.</span></span>

## <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="e247c-196">Wykryj, kiedy aplikacja jest przedrenderowana</span><span class="sxs-lookup"><span data-stu-id="e247c-196">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]
