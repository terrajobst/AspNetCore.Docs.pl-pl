<span data-ttu-id="18ec5-101">Podczas aplikacji po stronie serwera Blazor jest prerendering, nie są określone akcje, takie jak wywołanie JavaScript, z możliwe, ponieważ nie został utworzony połączenia za pośrednictwem przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="18ec5-101">While a Blazor server-side app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="18ec5-102">Składniki może być konieczne renderowania inaczej, gdy prerendered.</span><span class="sxs-lookup"><span data-stu-id="18ec5-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="18ec5-103">Opóźnienie JavaScript interop wywołuje aż po nawiązaniu połączenia za pośrednictwem przeglądarki, możesz użyć `OnAfterRenderAsync` zdarzenia cyklu życia składników.</span><span class="sxs-lookup"><span data-stu-id="18ec5-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="18ec5-104">To zdarzenie jest wywoływane tylko, po aplikacji jest w pełni renderowany i nawiązaniu połączenia klienta.</span><span class="sxs-lookup"><span data-stu-id="18ec5-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementRef myInput;

    protected override void OnAfterRender()
    {
        JSRuntime.InvokeAsync<object>(
            "setElementValue", myInput, "Value set after render");
    }
}
```

<span data-ttu-id="18ec5-105">Następujące części przedstawiono sposób użycia międzyoperacyjnego JavaScript jako część logiki inicjowania składnika w sposób, który jest zgodny z prerendering.</span><span class="sxs-lookup"><span data-stu-id="18ec5-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="18ec5-106">Składnik wskazuje, że jest możliwe do wyzwalania aktualizacji renderowanie z wewnątrz `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="18ec5-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="18ec5-107">Deweloper musi należy unikać tworzenia wejścia w nieskończoną pętlę w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="18ec5-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="18ec5-108">Gdzie `JSRuntime.InvokeAsync` jest wywoływana, `ElementRef` jest używana tylko w `OnAfterRenderAsync` a nie w dowolnej wcześniejszej metody cyklu życia, ponieważ nie ma żadnego elementu języka JavaScript, aż po składnika renderowania.</span><span class="sxs-lookup"><span data-stu-id="18ec5-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="18ec5-109">`StateHasChanged` jest wywoływana, aby rerender składnika za pomocą nowego Państwa uzyskany z wywołania międzyoperacyjnego JavaScript.</span><span class="sxs-lookup"><span data-stu-id="18ec5-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="18ec5-110">Kod nie tworzy wejścia w nieskończoną pętlę, ponieważ `StateHasChanged` tylko jest wywoływana, gdy `infoFromJs` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="18ec5-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

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
    <input id="val-set-by-interop" @ref="myElem" />
</p>

@code {
    private string infoFromJs;
    private ElementRef myElem;

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

<span data-ttu-id="18ec5-111">Aby warunkowo renderować różną zawartość w oparciu o tego, czy aplikacja jest obecnie prerendering zawartości, należy użyć `IsConnected` właściwość `IComponentContext` usługi.</span><span class="sxs-lookup"><span data-stu-id="18ec5-111">To conditionally render different content based on whether the app is currently prerendering content, use the `IsConnected` property on the `IComponentContext` service.</span></span> <span data-ttu-id="18ec5-112">Podczas uruchamiania po stronie serwera, `IsConnected` zwraca tylko `true` w przypadku aktywnego połączenia do klienta.</span><span class="sxs-lookup"><span data-stu-id="18ec5-112">When running server-side, `IsConnected` only returns `true` if there's an active connection to the client.</span></span> <span data-ttu-id="18ec5-113">Zawsze zwraca `true` podczas uruchamiania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="18ec5-113">It always returns `true` when running client-side.</span></span>

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
