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
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="348d1-102">Dodatkowe scenariusze zabezpieczeń ASP.NET Core Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="348d1-102">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="348d1-103">Autor [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="348d1-103">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a><span data-ttu-id="348d1-104">Obsługa błędów żądania tokenu</span><span class="sxs-lookup"><span data-stu-id="348d1-104">Handle token request errors</span></span>

<span data-ttu-id="348d1-105">Gdy aplikacja jednostronicowa uwierzytelnia użytkownika za pomocą funkcji Open ID Connect (OIDC), stan uwierzytelniania jest obsługiwany lokalnie w ramach SPA i w postaci pliku cookie sesji, który jest ustawiany w wyniku użytkownika dostarczającego poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="348d1-105">When a single page app (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="348d1-106">Tokeny, które są emitowane przez protokół IP dla użytkownika zwykle są ważne przez krótki okresy czasu, na ogół o godzinę, więc aplikacja kliencka musi regularnie pobierać nowe tokeny.</span><span class="sxs-lookup"><span data-stu-id="348d1-106">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="348d1-107">W przeciwnym razie użytkownik zostanie wylogowany po wygaśnięciu przyznanych tokenów.</span><span class="sxs-lookup"><span data-stu-id="348d1-107">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="348d1-108">W większości przypadków klienci OIDC mogą udostępniać nowe tokeny, nie wymagając ponownego uwierzytelnienia użytkownika w ramach stanu uwierzytelniania lub "sesji", który jest przechowywany w ramach adresu IP.</span><span class="sxs-lookup"><span data-stu-id="348d1-108">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="348d1-109">Istnieją sytuacje, w których klient nie może uzyskać tokenu bez interakcji użytkownika, na przykład gdy z jakiegoś powodu użytkownik jawnie wylogowuje się z adresu IP.</span><span class="sxs-lookup"><span data-stu-id="348d1-109">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="348d1-110">Ten scenariusz występuje, gdy użytkownik odwiedza `https://login.microsoftonline.com` i wylogowuje się. W tych scenariuszach aplikacja nie wie od razu, że użytkownik wyloguje się. Każdy token przechowywany przez klienta może już nie być prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="348d1-110">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="348d1-111">Ponadto klient nie może zainicjować obsługi nowego tokenu bez interakcji użytkownika po wygaśnięciu bieżącego tokenu.</span><span class="sxs-lookup"><span data-stu-id="348d1-111">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="348d1-112">Te scenariusze nie są specyficzne dla uwierzytelniania opartego na tokenach.</span><span class="sxs-lookup"><span data-stu-id="348d1-112">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="348d1-113">Są one częścią charakteru aplikacji jednostronicowych.</span><span class="sxs-lookup"><span data-stu-id="348d1-113">They are part of the nature of SPAs.</span></span> <span data-ttu-id="348d1-114">SPA używający plików cookie również nie może wywołać interfejsu API serwera, jeśli plik cookie uwierzytelniania zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="348d1-114">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="348d1-115">Gdy aplikacja wykonuje wywołania interfejsu API do chronionych zasobów, należy pamiętać o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="348d1-115">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="348d1-116">Aby zainicjować obsługę nowego tokenu dostępu w celu wywołania interfejsu API, może być konieczne ponowne uwierzytelnienie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="348d1-116">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="348d1-117">Nawet jeśli klient ma token, który wydaje się być prawidłowy, wywołanie do serwera może się nie powieść, ponieważ token został odwołany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="348d1-117">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="348d1-118">Gdy aplikacja żąda tokenu, istnieją dwa możliwe wyniki:</span><span class="sxs-lookup"><span data-stu-id="348d1-118">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="348d1-119">Żądanie powiodło się, a aplikacja ma prawidłowy token.</span><span class="sxs-lookup"><span data-stu-id="348d1-119">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="348d1-120">Żądanie nie powiedzie się, a aplikacja musi ponownie uwierzytelnić użytkownika w celu uzyskania nowego tokenu.</span><span class="sxs-lookup"><span data-stu-id="348d1-120">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="348d1-121">Gdy żądanie tokenu nie powiedzie się, należy zdecydować, czy chcesz zapisać dowolny bieżący stan przed przeprowadzeniem przekierowania.</span><span class="sxs-lookup"><span data-stu-id="348d1-121">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="348d1-122">Istnieją różne podejścia z rosnącymi poziomami złożoności:</span><span class="sxs-lookup"><span data-stu-id="348d1-122">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="348d1-123">Przechowuj bieżący stan strony w magazynie sesji.</span><span class="sxs-lookup"><span data-stu-id="348d1-123">Store the current page state in session storage.</span></span> <span data-ttu-id="348d1-124">Podczas `OnInitializeAsync`Sprawdź, czy stan można przywrócić przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="348d1-124">During `OnInitializeAsync`, check if state can be restored before continuing.</span></span>
* <span data-ttu-id="348d1-125">Dodaj parametr ciągu zapytania i użyj go jako sposobu sygnalizowania aplikacji, którą potrzebuje do ponownego zapisu wcześniej zapisanego stanu.</span><span class="sxs-lookup"><span data-stu-id="348d1-125">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="348d1-126">Dodaj parametr ciągu zapytania z unikatowym identyfikatorem w celu przechowywania danych w magazynie sesji bez ryzyka kolizji z innymi elementami.</span><span class="sxs-lookup"><span data-stu-id="348d1-126">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="348d1-127">W poniższym przykładzie przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="348d1-127">The following example shows how to:</span></span>

* <span data-ttu-id="348d1-128">Zachowaj stan przed przekierowaniem do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="348d1-128">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="348d1-129">Odzyskaj poprzedni stan, a następnie Uwierzytelnij przy użyciu parametru ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="348d1-129">Recover the previous state afterward authentication using the query string parameter.</span></span>

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

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="348d1-130">Zapisz stan aplikacji przed operacją uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="348d1-130">Save app state before an authentication operation</span></span>

<span data-ttu-id="348d1-131">Podczas operacji uwierzytelniania istnieją przypadki, w których chcesz zapisać stan aplikacji przed przekierowaniem przeglądarki do adresu IP.</span><span class="sxs-lookup"><span data-stu-id="348d1-131">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="348d1-132">Taka sytuacja może wystąpić w przypadku korzystania z takiego elementu jak kontenera stanu i przywrócenia stanu po pomyślnym uwierzytelnieniu.</span><span class="sxs-lookup"><span data-stu-id="348d1-132">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="348d1-133">Możesz użyć niestandardowego obiektu stanu uwierzytelniania, aby zachować stan specyficzny dla aplikacji lub odwołanie do niego, a następnie przywrócić ten stan po pomyślnym ukończeniu operacji uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="348d1-133">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="348d1-134">składnik `Authentication` (*strony/uwierzytelnianie. Razor*):</span><span class="sxs-lookup"><span data-stu-id="348d1-134">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

## <a name="request-additional-access-tokens"></a><span data-ttu-id="348d1-135">Żądaj dodatkowych tokenów dostępu</span><span class="sxs-lookup"><span data-stu-id="348d1-135">Request additional access tokens</span></span>

<span data-ttu-id="348d1-136">Większość aplikacji wymaga tylko tokenu dostępu, aby móc korzystać z chronionych zasobów, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="348d1-136">Most apps only require an access token to interact with the protected resources that they use.</span></span> <span data-ttu-id="348d1-137">W niektórych scenariuszach aplikacja może wymagać więcej niż jednego tokenu, aby można było korzystać z dwóch lub więcej zasobów.</span><span class="sxs-lookup"><span data-stu-id="348d1-137">In some scenarios, an app might require more than one token in order to interact with two or more resources.</span></span> <span data-ttu-id="348d1-138">Metoda `IAccessTokenProvider.RequestToken` zapewnia Przeciążenie, które umożliwia aplikacji Inicjowanie obsługi tokenu z danym zestawem zakresów, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="348d1-138">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision a token with a given set of scopes, as seen in the following example:</span></span>

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a><span data-ttu-id="348d1-139">Dostosowywanie tras aplikacji</span><span class="sxs-lookup"><span data-stu-id="348d1-139">Customize app routes</span></span>

<span data-ttu-id="348d1-140">Domyślnie Biblioteka `Microsoft.AspNetCore.Components.WebAssembly.Authentication` używa tras przedstawionych w poniższej tabeli w celu reprezentowania różnych stanów uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="348d1-140">By default, the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="348d1-141">Trasa</span><span class="sxs-lookup"><span data-stu-id="348d1-141">Route</span></span>                            | <span data-ttu-id="348d1-142">Cel</span><span class="sxs-lookup"><span data-stu-id="348d1-142">Purpose</span></span> |
| -------------------------------- | ------- |
| `authentication/login`           | <span data-ttu-id="348d1-143">Wyzwala operację logowania.</span><span class="sxs-lookup"><span data-stu-id="348d1-143">Triggers a sign-in operation.</span></span> |
| `authentication/login-callback`  | <span data-ttu-id="348d1-144">Obsługuje wynik operacji logowania.</span><span class="sxs-lookup"><span data-stu-id="348d1-144">Handles the result of any sign-in operation.</span></span> |
| `authentication/login-failed`    | <span data-ttu-id="348d1-145">Wyświetla komunikaty o błędach, gdy operacja logowania zakończy się niepowodzeniem z jakiegoś powodu.</span><span class="sxs-lookup"><span data-stu-id="348d1-145">Displays error messages when the sign-in operation fails for some reason.</span></span> |
| `authentication/logout`          | <span data-ttu-id="348d1-146">Wyzwala operację wylogowania.</span><span class="sxs-lookup"><span data-stu-id="348d1-146">Triggers a sign-out operation.</span></span> |
| `authentication/logout-callback` | <span data-ttu-id="348d1-147">Obsługuje wynik operacji wylogowania.</span><span class="sxs-lookup"><span data-stu-id="348d1-147">Handles the result of a sign-out operation.</span></span> |
| `authentication/logout-failed`   | <span data-ttu-id="348d1-148">Wyświetla komunikaty o błędach, gdy operacja wylogowania nie powiedzie się z jakiegoś powodu.</span><span class="sxs-lookup"><span data-stu-id="348d1-148">Displays error messages when the sign-out operation fails for some reason.</span></span> |
| `authentication/logged-out`      | <span data-ttu-id="348d1-149">Wskazuje, że użytkownik pomyślnie wylogować się.</span><span class="sxs-lookup"><span data-stu-id="348d1-149">Indicates that the user has successfully logout.</span></span> |
| `authentication/profile`         | <span data-ttu-id="348d1-150">Wyzwala operację edytowania profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="348d1-150">Triggers an operation to edit the user profile.</span></span> |
| `authentication/register`        | <span data-ttu-id="348d1-151">Wyzwala operację w celu zarejestrowania nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="348d1-151">Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="348d1-152">Trasy pokazane w powyższej tabeli można konfigurować za pośrednictwem `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span><span class="sxs-lookup"><span data-stu-id="348d1-152">The routes shown in the preceding table are configurable via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span></span> <span data-ttu-id="348d1-153">Podczas ustawiania opcji w celu zapewnienia tras niestandardowych upewnij się, że aplikacja ma trasę obsługującą każdą ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="348d1-153">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="348d1-154">W poniższym przykładzie wszystkie ścieżki są poprzedzone prefiksem `/security`.</span><span class="sxs-lookup"><span data-stu-id="348d1-154">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="348d1-155">składnik `Authentication` (*strony/uwierzytelnianie. Razor*):</span><span class="sxs-lookup"><span data-stu-id="348d1-155">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="348d1-156">`Program.Main` (*program.cs*):</span><span class="sxs-lookup"><span data-stu-id="348d1-156">`Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="348d1-157">Jeśli wymaganie wywołuje całkowicie różne ścieżki, ustaw trasy zgodnie z opisem wcześniej i Renderuj `RemoteAuthenticatorView` za pomocą jawnego parametru akcji:</span><span class="sxs-lookup"><span data-stu-id="348d1-157">If the requirement calls for completely different paths, set the routes as described previously and render the `RemoteAuthenticatorView` with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="348d1-158">Jeśli zdecydujesz się to zrobić, możesz przerwać interfejs użytkownika na różnych stronach.</span><span class="sxs-lookup"><span data-stu-id="348d1-158">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="348d1-159">Dostosowywanie interfejsu użytkownika uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="348d1-159">Customize the authentication user interface</span></span>

<span data-ttu-id="348d1-160">`RemoteAuthenticatorView` zawiera domyślny zestaw elementów interfejsu użytkownika dla każdego stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="348d1-160">`RemoteAuthenticatorView` includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="348d1-161">Każdy stan można dostosować przez przekazanie do niestandardowego `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="348d1-161">Each state can be customized by passing in a custom `RenderFragment`.</span></span> <span data-ttu-id="348d1-162">Aby dostosować wyświetlany tekst podczas początkowego procesu logowania, można zmienić `RemoteAuthenticatorView` w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="348d1-162">To customize the displayed text during the initial login process, can change the `RemoteAuthenticatorView` as follows.</span></span>

<span data-ttu-id="348d1-163">składnik `Authentication` (*strony/uwierzytelnianie. Razor*):</span><span class="sxs-lookup"><span data-stu-id="348d1-163">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

<span data-ttu-id="348d1-164">`RemoteAuthenticatorView` ma jeden fragment, którego można użyć dla trasy uwierzytelniania pokazanej w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="348d1-164">The `RemoteAuthenticatorView` has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="348d1-165">Trasa</span><span class="sxs-lookup"><span data-stu-id="348d1-165">Route</span></span>                            | <span data-ttu-id="348d1-166">Fragment</span><span class="sxs-lookup"><span data-stu-id="348d1-166">Fragment</span></span>             |
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
