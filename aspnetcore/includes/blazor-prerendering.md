Gdy aplikacja serwera Blazor jest wstępnie renderowana, niektóre akcje, takie jak wywoływanie kodu JavaScript, nie są możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane. Składniki mogą być konieczne w różny sposób, gdy są wstępnie renderowane.

Aby opóźnić wywołania międzyoperacyjne języka JavaScript do momentu ustanowienia połączenia z przeglądarką, można `OnAfterRenderAsync` użyć zdarzenia cyklu życia składnika. To zdarzenie jest wywoływane tylko wtedy, gdy aplikacja jest w pełni renderowana, a połączenie z klientem zostanie nawiązane.

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
            JSRuntime.InvokeAsync<object>(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

Poniższy składnik pokazuje, jak używać międzyoperacyjności JavaScript jako części logiki inicjalizacji składnika w sposób, który jest zgodny z renderowaniem. Składnik pokazuje, że można wyzwolić aktualizację renderowania z wewnątrz `OnAfterRenderAsync`. Deweloper musi unikać tworzenia pętli nieskończonej w tym scenariuszu.

Gdzie `JSRuntime.InvokeAsync` jest wywoływana, `ElementRef` jest używana tylko w `OnAfterRenderAsync` , a nie w żadnej wcześniejszej metodzie cyklu życia, ponieważ nie istnieje element JavaScript do momentu renderowania składnika.

`StateHasChanged`jest wywoływana, aby przetworzyć składnik z nowym stanem uzyskanym z wywołania międzyoperacyjnego języka JavaScript. Kod nie tworzy pętli nieskończonej, ponieważ `StateHasChanged` jest wywoływana tylko wtedy `infoFromJs` , `null`gdy jest.

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

Aby warunkowo renderować inną zawartość w zależności od tego, czy aplikacja aktualnie renderuje zawartość, użyj `IsConnected` właściwości `IComponentContext` usługi. W przypadku aplikacji serwera Blazor `IsConnected` funkcja zwraca `true` tylko wtedy, gdy istnieje aktywne połączenie z klientem. Zawsze jest on `true` zwracany w aplikacjach Blazor webassembly.

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
