<span data-ttu-id="2518e-101">Składnik `App` (*App. Razor*) jest podobny do składnika `App` znalezionego w aplikacjach serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="2518e-101">The `App` component (*App.razor*) is similar to the `App` component found in Blazor Server apps:</span></span>

* <span data-ttu-id="2518e-102">Składnik `CascadingAuthenticationState` zarządza uwidacznianiem `AuthenticationState` w pozostałej części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2518e-102">The `CascadingAuthenticationState` component manages exposing the `AuthenticationState` to the rest of the app.</span></span>
* <span data-ttu-id="2518e-103">Składnik `AuthorizeRouteView` upewnia się, że bieżący użytkownik ma uprawnienia dostępu do danej strony lub w inny sposób renderuje składnik `RedirectToLogin`.</span><span class="sxs-lookup"><span data-stu-id="2518e-103">The `AuthorizeRouteView` component makes sure that the current user is authorized to access a given page or otherwise renders the `RedirectToLogin` component.</span></span>
* <span data-ttu-id="2518e-104">Składnik `RedirectToLogin` zarządza przekierowywaniem nieautoryzowanych użytkowników do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="2518e-104">The `RedirectToLogin` component manages redirecting unauthorized users to the login page.</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    <RedirectToLogin />
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
