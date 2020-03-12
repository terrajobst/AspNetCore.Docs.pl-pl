Składnik `App` (*App. Razor*) jest podobny do składnika `App` znalezionego w aplikacjach serwera Blazor:

* Składnik `CascadingAuthenticationState` zarządza uwidacznianiem `AuthenticationState` w pozostałej części aplikacji.
* Składnik `AuthorizeRouteView` upewnia się, że bieżący użytkownik ma uprawnienia dostępu do danej strony lub w inny sposób renderuje składnik `RedirectToLogin`.
* Składnik `RedirectToLogin` zarządza przekierowywaniem nieautoryzowanych użytkowników do strony logowania.

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
