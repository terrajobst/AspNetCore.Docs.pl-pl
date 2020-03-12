<span data-ttu-id="25bad-101">Składnik `FetchData` pokazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="25bad-101">The `FetchData` component shows how to:</span></span>

* <span data-ttu-id="25bad-102">Inicjowanie obsługi administracyjnej tokenu dostępu.</span><span class="sxs-lookup"><span data-stu-id="25bad-102">Provision an access token.</span></span>
* <span data-ttu-id="25bad-103">Użyj tokenu dostępu, aby wywołać interfejs API zasobów chronionych w aplikacji *serwera* .</span><span class="sxs-lookup"><span data-stu-id="25bad-103">Use the access token to call a protected resource API in the *Server* app.</span></span>

<span data-ttu-id="25bad-104">Dyrektywa `@attribute [Authorize]` wskazuje system autoryzacji zestawu webassembly Blazor, który użytkownik musi mieć autoryzację, aby móc odwiedzić ten składnik.</span><span class="sxs-lookup"><span data-stu-id="25bad-104">The `@attribute [Authorize]` directive indicates to the Blazor WebAssembly authorization system that the user must be authorized in order to visit this component.</span></span> <span data-ttu-id="25bad-105">Obecność atrybutu w aplikacji *klienckiej* nie zapobiega WYWOŁYWANIU interfejsu API na serwerze bez poprawnych poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="25bad-105">The presence of the attribute in the *Client* app doesn't prevent the API on the server from being called without proper credentials.</span></span> <span data-ttu-id="25bad-106">Aplikacja *serwera* musi również używać `[Authorize]` na odpowiednich punktach końcowych, aby prawidłowo je chronić.</span><span class="sxs-lookup"><span data-stu-id="25bad-106">The *Server* app also must use `[Authorize]` on the appropriate endpoints to correctly protect them.</span></span>

<span data-ttu-id="25bad-107">`AuthenticationService.RequestAccessToken();` bierze pod uwagę żądanie tokenu dostępu, który można dodać do żądania, aby wywołać interfejs API.</span><span class="sxs-lookup"><span data-stu-id="25bad-107">`AuthenticationService.RequestAccessToken();` takes care of requesting an access token that can be added to the request to call the API.</span></span> <span data-ttu-id="25bad-108">Jeśli token jest buforowany lub usługa jest w stanie zainicjować nowy token dostępu bez interakcji z użytkownikiem, żądanie tokenu powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="25bad-108">If the token is cached or the service is able to provision a new access token without user interaction, the token request succeeds.</span></span> <span data-ttu-id="25bad-109">W przeciwnym razie żądanie tokenu kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="25bad-109">Otherwise, the token request fails.</span></span>

<span data-ttu-id="25bad-110">Aby uzyskać rzeczywisty token do uwzględnienia w żądaniu, aplikacja musi sprawdzić, czy żądanie powiodło się, wywołując `tokenResult.TryGetToken(out var token)`.</span><span class="sxs-lookup"><span data-stu-id="25bad-110">In order to obtain the actual token to include in the request, the app must check that the request succeeded by calling `tokenResult.TryGetToken(out var token)`.</span></span> 

<span data-ttu-id="25bad-111">Jeśli żądanie zakończyło się pomyślnie, zmienna tokenu jest wypełniana tokenem dostępu.</span><span class="sxs-lookup"><span data-stu-id="25bad-111">If the request was successful, the token variable is populated with the access token.</span></span> <span data-ttu-id="25bad-112">Właściwość `Value` tokenu ujawnia ciąg literału, który ma zostać uwzględniony w nagłówku żądania `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="25bad-112">The `Value` property of the token exposes the literal string to include in the `Authorization` request header.</span></span>

<span data-ttu-id="25bad-113">Jeśli żądanie nie powiodło się, ponieważ nie można zainicjować obsługi administracyjnej tokenu bez interakcji z użytkownikiem, wynik tokenu zawiera adres URL przekierowania.</span><span class="sxs-lookup"><span data-stu-id="25bad-113">If the request failed because the token couldn't be provisioned without user interaction, the token result contains a redirect URL.</span></span> <span data-ttu-id="25bad-114">Przechodzenie do tego adresu URL powoduje, że użytkownik jest zalogowany na stronie logowania i z powrotem do bieżącej strony po pomyślnym uwierzytelnieniu.</span><span class="sxs-lookup"><span data-stu-id="25bad-114">Navigating to this URL takes the user to the login page and back to the current page after a successful authentication.</span></span>

```razor
@page "/fetchdata"
...
@attribute [Authorize]

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var tokenResult = await AuthenticationService.RequestAccessToken();

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            forecasts = await httpClient.GetJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        else
        {
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }

    }
}
```

<span data-ttu-id="25bad-115">Aby uzyskać więcej informacji, zobacz [Zapisywanie stanu aplikacji przed operacją uwierzytelniania](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).</span><span class="sxs-lookup"><span data-stu-id="25bad-115">For more information, see [Save app state before an authentication operation](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).</span></span>
