Strona utworzona przez składnik `Authentication` (*Pages/Authentication. Razor*) definiuje trasy wymagane do obsługi różnych etapów uwierzytelniania.

Składnik `RemoteAuthenticatorView`:

* Jest dostarczany przez pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.
* Zarządza wykonywaniem odpowiednich czynności na każdym etapie uwierzytelniania.

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
