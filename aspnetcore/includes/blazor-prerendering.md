Podczas aplikacji po stronie serwera Blazor jest prerendering, nie są określone akcje, takie jak wywołanie JavaScript, z możliwe, ponieważ nie został utworzony połączenia za pośrednictwem przeglądarki. Składniki może być konieczne renderowania inaczej, gdy prerendered.

Opóźnienie JavaScript interop wywołuje aż po nawiązaniu połączenia za pośrednictwem przeglądarki, możesz użyć `OnAfterRenderAsync` zdarzenia cyklu życia składników. To zdarzenie jest wywoływane tylko, po aplikacji jest w pełni renderowany i nawiązaniu połączenia klienta.

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input ref="myInput" value="Value set during render" />

@functions {
    ElementRef myInput;

    protected override void OnAfterRender()
    {
        JSRuntime.InvokeAsync<object>(
            "setElementValue", myInput, "Value set after render");
    }
}
```

Ten składnik następujące pokazuje sposób użycia międzyoperacyjnego JavaScript jako część logiki inicjowania składnika w sposób, który jest zgodny z prerendering.

Składnik wskazuje, że jest możliwe do wyzwalania aktualizacji renderowanie z wewnątrz `OnAfterRenderAsync`. W tym scenariuszu. Deweloper musi należy unikać tworzenia wejścia w nieskończoną pętlę.

Gdzie `JSRuntime.InvokeAsync` jest wywoływana, `ElementRef` jest używana tylko w `OnAfterRenderAsync` i nie wszystkie wcześniejsze metody cyklu życia, ponieważ nie ma żadnego elementu języka JavaScript, do czasu, po składnik jest renderowany.

`StateHasChanged` jest wywoływana, aby rerender składnika za pomocą nowego Państwa uzyskany z wywołania międzyoperacyjnego JavaScript. Kod nie tworzy wejścia w nieskończoną pętlę, ponieważ `StateHasChanged` tylko jest wywoływana, gdy `infoFromJs` jest `null`.

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
    <input id="val-set-by-interop" ref="@myElem" />
</p>

@functions {
    string infoFromJs;
    ElementRef myElem;

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

Aby warunkowo renderować różną zawartość w oparciu o tego, czy aplikacja jest obecnie prerendering zawartości, należy użyć `IsConnected` właściwość `IComponentContext` usługi. Podczas uruchamiania po stronie serwera, `IsConnected` zwraca tylko `true` w przypadku aktywnego połączenia do klienta. Zawsze zwraca `true` podczas uruchamiania po stronie klienta.

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
    <button id="increment-count" onclick="@(() => count++)">Click me</button>
</p>

@functions {
    private int count;
}
```
