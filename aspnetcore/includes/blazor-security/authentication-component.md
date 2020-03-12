<span data-ttu-id="1dd43-101">Strona utworzona przez składnik `Authentication` (*Pages/Authentication. Razor*) definiuje trasy wymagane do obsługi różnych etapów uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1dd43-101">The page produced by the `Authentication` component (*Pages/Authentication.razor*) defines the routes required for handling different authentication stages.</span></span>

<span data-ttu-id="1dd43-102">Składnik `RemoteAuthenticatorView`:</span><span class="sxs-lookup"><span data-stu-id="1dd43-102">The `RemoteAuthenticatorView` component:</span></span>

* <span data-ttu-id="1dd43-103">Jest dostarczany przez pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="1dd43-103">Is provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span>
* <span data-ttu-id="1dd43-104">Zarządza wykonywaniem odpowiednich czynności na każdym etapie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1dd43-104">Manages performing the appropriate actions at each stage of authentication.</span></span>

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
