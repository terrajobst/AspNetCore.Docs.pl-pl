Składnik `FetchData` pokazuje, jak:

* Inicjowanie obsługi administracyjnej tokenu dostępu.
* Użyj tokenu dostępu, aby wywołać interfejs API zasobów chronionych w aplikacji *serwera* .

Dyrektywa `@attribute [Authorize]` wskazuje system autoryzacji zestawu webassembly Blazor, który użytkownik musi mieć autoryzację, aby móc odwiedzić ten składnik. Obecność atrybutu w aplikacji *klienckiej* nie zapobiega WYWOŁYWANIU interfejsu API na serwerze bez poprawnych poświadczeń. Aplikacja *serwera* musi również używać `[Authorize]` na odpowiednich punktach końcowych, aby prawidłowo je chronić.

`AuthenticationService.RequestAccessToken();` bierze pod uwagę żądanie tokenu dostępu, który można dodać do żądania, aby wywołać interfejs API. Jeśli token jest buforowany lub usługa jest w stanie zainicjować nowy token dostępu bez interakcji z użytkownikiem, żądanie tokenu powiedzie się. W przeciwnym razie żądanie tokenu kończy się niepowodzeniem.

Aby uzyskać rzeczywisty token do uwzględnienia w żądaniu, aplikacja musi sprawdzić, czy żądanie powiodło się, wywołując `tokenResult.TryGetToken(out var token)`. 

Jeśli żądanie zakończyło się pomyślnie, zmienna tokenu jest wypełniana tokenem dostępu. Właściwość `Value` tokenu ujawnia ciąg literału, który ma zostać uwzględniony w nagłówku żądania `Authorization`.

Jeśli żądanie nie powiodło się, ponieważ nie można zainicjować obsługi administracyjnej tokenu bez interakcji z użytkownikiem, wynik tokenu zawiera adres URL przekierowania. Przechodzenie do tego adresu URL powoduje, że użytkownik jest zalogowany na stronie logowania i z powrotem do bieżącej strony po pomyślnym uwierzytelnieniu.

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

Aby uzyskać więcej informacji, zobacz [Zapisywanie stanu aplikacji przed operacją uwierzytelniania](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).
