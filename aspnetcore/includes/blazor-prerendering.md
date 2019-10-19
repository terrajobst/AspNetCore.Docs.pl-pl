<span data-ttu-id="e1258-101">Gdy aplikacja serwera Blazor jest wstępnie renderowana, niektóre akcje, takie jak wywoływanie kodu JavaScript, nie są możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="e1258-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="e1258-102">Składniki mogą być konieczne w różny sposób, gdy są wstępnie renderowane.</span><span class="sxs-lookup"><span data-stu-id="e1258-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="e1258-103">Aby opóźnić wywołania międzyoperacyjne języka JavaScript do momentu ustanowienia połączenia z przeglądarką, można użyć zdarzenia cyklu życia składnika `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="e1258-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="e1258-104">To zdarzenie jest wywoływane tylko wtedy, gdy aplikacja jest w pełni renderowana, a połączenie z klientem zostanie nawiązane.</span><span class="sxs-lookup"><span data-stu-id="e1258-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            JSRuntime.InvokeVoidAsync(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

<span data-ttu-id="e1258-105">Poniższy składnik pokazuje, jak używać międzyoperacyjności JavaScript jako części logiki inicjalizacji składnika w sposób, który jest zgodny z renderowaniem.</span><span class="sxs-lookup"><span data-stu-id="e1258-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="e1258-106">Składnik pokazuje, że można wyzwolić aktualizację renderowania z poziomu `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="e1258-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="e1258-107">Deweloper musi unikać tworzenia pętli nieskończonej w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="e1258-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="e1258-108">Gdzie `JSRuntime.InvokeAsync` jest wywoływana, `ElementRef` jest używana tylko w `OnAfterRenderAsync` i nie w żadnej starszej metodzie cyklu życia, ponieważ nie istnieje element JavaScript do momentu renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="e1258-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="e1258-109">`StateHasChanged` jest wywoływana, aby przetworzyć składnik z nowym stanem uzyskanym z wywołania międzyoperacyjnego języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e1258-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="e1258-110">Kod nie tworzy pętli nieskończonej, ponieważ `StateHasChanged` jest wywoływana tylko wtedy, gdy `infoFromJs` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="e1258-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
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
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```
