---
title: Dodatkowe scenariusze zabezpieczeń ASP.NET Core Blazor webassembly
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: fe87ce76d8e181de788188b021616f2a09833585
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083889"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a>Dodatkowe scenariusze zabezpieczeń ASP.NET Core Blazor webassembly

Autor [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a>Obsługa błędów żądania tokenu

Gdy aplikacja jednostronicowa uwierzytelnia użytkownika za pomocą funkcji Open ID Connect (OIDC), stan uwierzytelniania jest obsługiwany lokalnie w ramach SPA i w postaci pliku cookie sesji, który jest ustawiany w wyniku użytkownika dostarczającego poświadczenia.

Tokeny, które są emitowane przez protokół IP dla użytkownika zwykle są ważne przez krótki okresy czasu, na ogół o godzinę, więc aplikacja kliencka musi regularnie pobierać nowe tokeny. W przeciwnym razie użytkownik zostanie wylogowany po wygaśnięciu przyznanych tokenów. W większości przypadków klienci OIDC mogą udostępniać nowe tokeny, nie wymagając ponownego uwierzytelnienia użytkownika w ramach stanu uwierzytelniania lub "sesji", który jest przechowywany w ramach adresu IP.

Istnieją sytuacje, w których klient nie może uzyskać tokenu bez interakcji użytkownika, na przykład gdy z jakiegoś powodu użytkownik jawnie wylogowuje się z adresu IP. Ten scenariusz występuje, gdy użytkownik odwiedza `https://login.microsoftonline.com` i wylogowuje się. W tych scenariuszach aplikacja nie wie od razu, że użytkownik wyloguje się. Każdy token przechowywany przez klienta może już nie być prawidłowy. Ponadto klient nie może zainicjować obsługi nowego tokenu bez interakcji użytkownika po wygaśnięciu bieżącego tokenu.

Te scenariusze nie są specyficzne dla uwierzytelniania opartego na tokenach. Są one częścią charakteru aplikacji jednostronicowych. SPA używający plików cookie również nie może wywołać interfejsu API serwera, jeśli plik cookie uwierzytelniania zostanie usunięty.

Gdy aplikacja wykonuje wywołania interfejsu API do chronionych zasobów, należy pamiętać o następujących kwestiach:

* Aby zainicjować obsługę nowego tokenu dostępu w celu wywołania interfejsu API, może być konieczne ponowne uwierzytelnienie użytkownika.
* Nawet jeśli klient ma token, który wydaje się być prawidłowy, wywołanie do serwera może się nie powieść, ponieważ token został odwołany przez użytkownika.

Gdy aplikacja żąda tokenu, istnieją dwa możliwe wyniki:

* Żądanie powiodło się, a aplikacja ma prawidłowy token.
* Żądanie nie powiedzie się, a aplikacja musi ponownie uwierzytelnić użytkownika w celu uzyskania nowego tokenu.

Gdy żądanie tokenu nie powiedzie się, należy zdecydować, czy chcesz zapisać dowolny bieżący stan przed przeprowadzeniem przekierowania. Istnieją różne podejścia z rosnącymi poziomami złożoności:

* Przechowuj bieżący stan strony w magazynie sesji. Podczas `OnInitializeAsync`Sprawdź, czy stan można przywrócić przed kontynuowaniem.
* Dodaj parametr ciągu zapytania i użyj go jako sposobu sygnalizowania aplikacji, którą potrzebuje do ponownego zapisu wcześniej zapisanego stanu.
* Dodaj parametr ciągu zapytania z unikatowym identyfikatorem w celu przechowywania danych w magazynie sesji bez ryzyka kolizji z innymi elementami.

W poniższym przykładzie przedstawiono sposób:

* Zachowaj stan przed przekierowaniem do strony logowania.
* Odzyskaj poprzedni stan, a następnie Uwierzytelnij przy użyciu parametru ciągu zapytania.

```razor
<EditForm Model="User" @onsubmit="OnSaveAsync">
    <label>User
        <InputText @bind-Value="User.Name" />
    </label>
    <label>Last name
        <InputText @bind-Value="User.LastName" />
    </label>
</EditForm>

@code {
    public class Profile
    {
        public string Name { get; set; }
        public string LastName { get; set; }
    }

    public Profile User { get; set; } = new Profile();

    protected async override Task OnInitializedAsync()
    {
        var currentQuery = new Uri(Navigation.Uri).Query;

        if (currentQuery.Contains("state=resumeSavingProfile"))
        {
            User = await JS.InvokeAsync<Profile>("sessionStorage.getState", 
                "resumeSavingProfile");
        }
    }

    public async Task OnSaveAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var resumeUri = Navigation.Uri + $"?state=resumeSavingProfile";

        var tokenResult = await AuthenticationService.RequestAccessToken(
            new AccessTokenRequestOptions
            {
                ReturnUrl = resumeUri
            });

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            await httpClient.PostJsonAsync("Save", User);
        }
        else
        {
            await JS.InvokeVoidAsync("sessionStorage.setState", 
                "resumeSavingProfile", User);
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }
    }
}
```

## <a name="save-app-state-before-an-authentication-operation"></a>Zapisz stan aplikacji przed operacją uwierzytelniania

Podczas operacji uwierzytelniania istnieją przypadki, w których chcesz zapisać stan aplikacji przed przekierowaniem przeglądarki do adresu IP. Taka sytuacja może wystąpić w przypadku korzystania z takiego elementu jak kontenera stanu i przywrócenia stanu po pomyślnym uwierzytelnieniu. Możesz użyć niestandardowego obiektu stanu uwierzytelniania, aby zachować stan specyficzny dla aplikacji lub odwołanie do niego, a następnie przywrócić ten stan po pomyślnym ukończeniu operacji uwierzytelniania.

składnik `Authentication` (*strony/uwierzytelnianie. Razor*):

```razor
@page "/authentication/{action}"
@inject JSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action" 
    AuthenticationState="AuthenticationState" OnLoginSucceded="RestoreState" 
    OnLogoutSucceded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public class ApplicationAuthenticationState : RemoteAuthenticationState
    {
        public string Id { get; set; }
    }

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn, 
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();
            await JS.InvokeVoidAsync("sessionStorage.setKey", 
                AuthenticationState.Id, State.Store());
        }
    }

    public async Task RestoreState(ApplicationAuthenticationState state)
    {
        var stored = await JS.InvokeAsync<string>("sessionStorage.getKey", 
            state.Id);
        State.FromStore(stored);
    }

    public ApplicationAuthenticationState AuthenticationState { get; set; } = 
        new ApplicationAuthenticationState();
}
```

## <a name="request-additional-access-tokens"></a>Żądaj dodatkowych tokenów dostępu

Większość aplikacji wymaga tylko tokenu dostępu, aby móc korzystać z chronionych zasobów, z których korzystają. W niektórych scenariuszach aplikacja może wymagać więcej niż jednego tokenu, aby można było korzystać z dwóch lub więcej zasobów. Metoda `IAccessTokenProvider.RequestToken` zapewnia Przeciążenie, które umożliwia aplikacji Inicjowanie obsługi tokenu z danym zestawem zakresów, jak pokazano w następującym przykładzie:

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a>Dostosowywanie tras aplikacji

Domyślnie Biblioteka `Microsoft.AspNetCore.Components.WebAssembly.Authentication` używa tras przedstawionych w poniższej tabeli w celu reprezentowania różnych stanów uwierzytelniania.

| Trasa                            | Cel |
| -------------------------------- | ------- |
| `authentication/login`           | Wyzwala operację logowania. |
| `authentication/login-callback`  | Obsługuje wynik operacji logowania. |
| `authentication/login-failed`    | Wyświetla komunikaty o błędach, gdy operacja logowania zakończy się niepowodzeniem z jakiegoś powodu. |
| `authentication/logout`          | Wyzwala operację wylogowania. |
| `authentication/logout-callback` | Obsługuje wynik operacji wylogowania. |
| `authentication/logout-failed`   | Wyświetla komunikaty o błędach, gdy operacja wylogowania nie powiedzie się z jakiegoś powodu. |
| `authentication/logged-out`      | Wskazuje, że użytkownik pomyślnie wylogować się. |
| `authentication/profile`         | Wyzwala operację edytowania profilu użytkownika. |
| `authentication/register`        | Wyzwala operację w celu zarejestrowania nowego użytkownika. |

Trasy pokazane w powyższej tabeli można konfigurować za pośrednictwem `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`. Podczas ustawiania opcji w celu zapewnienia tras niestandardowych upewnij się, że aplikacja ma trasę obsługującą każdą ścieżkę.

W poniższym przykładzie wszystkie ścieżki są poprzedzone prefiksem `/security`.

składnik `Authentication` (*strony/uwierzytelnianie. Razor*):

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`Program.Main` (*program.cs*):

```csharp
builder.Services.AddApiAuthorization(options => { 
    options.AuthenticationPaths.LogInPath = "security/login";
    options.AuthenticationPaths.LogInCallbackPath = "security/login-callback";
    options.AuthenticationPaths.LogInFailedPath = "security/login-failed";
    options.AuthenticationPaths.LogOutPath = "security/logout";
    options.AuthenticationPaths.LogOutCallbackPath = "security/logout-callback";
    options.AuthenticationPaths.LogOutFailedPath = "security/logout-failed";
    options.AuthenticationPaths.LogOutSucceededPath = "security/logged-out";
    options.AuthenticationPaths.ProfilePath = "security/profile";
    options.AuthenticationPaths.RegisterPath = "security/register";
});
```

Jeśli wymaganie wywołuje całkowicie różne ścieżki, ustaw trasy zgodnie z opisem wcześniej i Renderuj `RemoteAuthenticatorView` za pomocą jawnego parametru akcji:

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

Jeśli zdecydujesz się to zrobić, możesz przerwać interfejs użytkownika na różnych stronach.

## <a name="customize-the-authentication-user-interface"></a>Dostosowywanie interfejsu użytkownika uwierzytelniania

`RemoteAuthenticatorView` zawiera domyślny zestaw elementów interfejsu użytkownika dla każdego stanu uwierzytelniania. Każdy stan można dostosować przez przekazanie do niestandardowego `RenderFragment`. Aby dostosować wyświetlany tekst podczas początkowego procesu logowania, można zmienić `RemoteAuthenticatorView` w następujący sposób.

składnik `Authentication` (*strony/uwierzytelnianie. Razor*):

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action">
    <LoggingIn>
        You are about to be redirected to https://login.microsoftonline.com.
    </LoggingIn>
</RemoteAuthenticatorView>

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`RemoteAuthenticatorView` ma jeden fragment, którego można użyć dla trasy uwierzytelniania pokazanej w poniższej tabeli.

| Trasa                            | Fragment             |
| -------------------------------- | -------------------- |
| `authentication/login`           | `<LoggingIn>`        |
| `authentication/login-callback`  | `<CompletingLogIn>`  |
| `authentication/login-failed`    | `<LogInFailed>`      |
| `authentication/logout`          | `<LoggingOut>`       |
| `authentication/logout-callback` | `<CompletingLogOut>` |
| `authentication/logout-failed`   | `<LogOutFailed>`     |
| `authentication/logged-out`      | `<LogOutSucceeded>`  |
| `authentication/profile`         | `<UserProfile>`      |
| `authentication/register`        | `<Registering>`      |
