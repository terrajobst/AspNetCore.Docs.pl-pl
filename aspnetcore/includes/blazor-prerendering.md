<span data-ttu-id="8e89d-101">Gdy aplikacja po stronie serwera Blazor jest wstępnie renderowana, niektóre akcje, takie jak wywoływanie kodu JavaScript, nie są możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="8e89d-101">While a Blazor server-side app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="8e89d-102">Składniki mogą być konieczne w różny sposób, gdy są wstępnie renderowane.</span><span class="sxs-lookup"><span data-stu-id="8e89d-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="8e89d-103">Aby opóźnić wywołania międzyoperacyjne języka JavaScript do momentu ustanowienia połączenia z przeglądarką, można `OnAfterRenderAsync` użyć zdarzenia cyklu życia składnika.</span><span class="sxs-lookup"><span data-stu-id="8e89d-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="8e89d-104">To zdarzenie jest wywoływane tylko wtedy, gdy aplikacja jest w pełni renderowana, a połączenie z klientem zostanie nawiązane.</span><span class="sxs-lookup"><span data-stu-id="8e89d-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" @ref:suppressField value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender()
    {
        JSRuntime.InvokeAsync<object>(
            "setElementValue", myInput, "Value set after render");
    }
}
```

<span data-ttu-id="8e89d-105">Poniższy składnik pokazuje, jak używać międzyoperacyjności JavaScript jako części logiki inicjalizacji składnika w sposób, który jest zgodny z renderowaniem.</span><span class="sxs-lookup"><span data-stu-id="8e89d-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="8e89d-106">Składnik pokazuje, że można wyzwolić aktualizację renderowania z wewnątrz `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="8e89d-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="8e89d-107">Deweloper musi unikać tworzenia pętli nieskończonej w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="8e89d-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="8e89d-108">Gdzie `JSRuntime.InvokeAsync` jest wywoływana, `ElementRef` jest używana tylko w `OnAfterRenderAsync` , a nie w żadnej wcześniejszej metodzie cyklu życia, ponieważ nie istnieje element JavaScript do momentu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="8e89d-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="8e89d-109">`StateHasChanged`jest wywoływana, aby przetworzyć składnik z nowym stanem uzyskanym z wywołania międzyoperacyjnego języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8e89d-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="8e89d-110">Kod nie tworzy pętli nieskończonej, ponieważ `StateHasChanged` jest wywoływana tylko wtedy `infoFromJs` , `null`gdy jest.</span><span class="sxs-lookup"><span data-stu-id="8e89d-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IComponentContext ComponentContext
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" @ref:suppressField />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync()
    {
        // TEMPORARY: Currently we need this guard to avoid making the interop
        // call during prerendering. Soon this will be unnecessary because we
        // will change OnAfterRenderAsync so that it won't run during the
        // prerendering phase.
        if (!ComponentContext.IsConnected)
        {
            return;
        }

        if (infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```

<span data-ttu-id="8e89d-111">Aby warunkowo renderować inną zawartość w zależności od tego, czy aplikacja aktualnie renderuje zawartość, użyj `IsConnected` właściwości `IComponentContext` usługi.</span><span class="sxs-lookup"><span data-stu-id="8e89d-111">To conditionally render different content based on whether the app is currently prerendering content, use the `IsConnected` property on the `IComponentContext` service.</span></span> <span data-ttu-id="8e89d-112">Po uruchomieniu po stronie serwera program `IsConnected` zwraca `true` tylko wtedy, gdy istnieje aktywne połączenie z klientem.</span><span class="sxs-lookup"><span data-stu-id="8e89d-112">When running server-side, `IsConnected` only returns `true` if there's an active connection to the client.</span></span> <span data-ttu-id="8e89d-113">Zawsze jest on `true` zwracany podczas uruchamiania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8e89d-113">It always returns `true` when running client-side.</span></span>

```cshtml
@page "/isconnected-example"
@using Microsoft.AspNetCore.Components.Services
@inject IComponentContext ComponentContext

<h1>IsConnected Example</h1>

<p>
    Current state:
    <strong id="connected-state">
        @(ComponentContext.IsConnected ? "connected" : "not connected")
    </strong>
</p>

<p>
    Clicks:
    <strong id="count">@count</strong>
    <button id="increment-count" @onclick="@(() => count++)">Click me</button>
</p>

@code {
    private int count;
}
```
