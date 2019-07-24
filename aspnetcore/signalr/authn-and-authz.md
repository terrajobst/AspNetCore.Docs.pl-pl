---
title: Uwierzytelnianie i autoryzacja w usłudze ASP.NET Core sygnalizujący
author: bradygaster
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w usłudze ASP.NET Core sygnalizujący.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 07/15/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e7e7a9fd537ba89b64c15594652a290357a00038
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412531"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>Uwierzytelnianie i autoryzacja w usłudze ASP.NET Core sygnalizujący

Według [Andrew Stanton-pielęgniarki](https://twitter.com/anurse)

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Uwierzytelnianie użytkowników łączących się z centrum sygnałów

Program sygnalizujący może być używany z [uwierzytelnianiem ASP.NET Core](xref:security/authentication/identity) , aby skojarzyć użytkownika z każdym połączeniem. W centrum dane uwierzytelniania są dostępne z [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) właściwości. Uwierzytelnianie umożliwia centrum wywoływanie metod we wszystkich połączeniach skojarzonych z użytkownikiem (zobacz [Zarządzanie użytkownikami i grupami w programie sygnalizującym](xref:signalr/groups) ), aby uzyskać więcej informacji. Wiele połączeń może być skojarzonych z pojedynczym użytkownikiem.

Poniżej przedstawiono przykład, w `Startup.Configure` którym jest używany program sygnalizujący i ASP.NET Core Authentication:

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();
    
    app.UseAuthentication();

    app.UseSignalR(hubs =>
    {
        hubs.MapHub<ChatHub>("/chat");
    });

    app.UseMvc(routes =>
    {
        routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

> [!NOTE]
> Kolejność, w której zarejestrowano sygnał i ASP.NET Core uwierzytelniania oprogramowania pośredniczącego. Zawsze wywołuj `UseAuthentication` przed `UseSignalR` tym, że sygnalizujący `HttpContext`ma użytkownika na.

### <a name="cookie-authentication"></a>Uwierzytelnianie plików cookie

W aplikacji opartej na przeglądarce uwierzytelnianie plików cookie umożliwia istniejące poświadczenia użytkownika w celu automatycznego przepływania do połączeń sygnałów. W przypadku korzystania z klienta przeglądarki nie jest wymagana dodatkowa konfiguracja. Jeśli użytkownik jest zalogowany do aplikacji, połączenie sygnalizujące automatycznie dziedziczy to uwierzytelnianie.

Pliki cookie to specyficzny dla przeglądarki sposób wysyłania tokenów dostępu, ale Klienci niebędący przeglądarkami mogą je wysyłać. W przypadku korzystania z `Cookies` [klienta platformy .NET](xref:signalr/dotnet-client)Właściwość można skonfigurować w `.WithUrl` wywołaniu w celu udostępnienia pliku cookie. Jednak używanie uwierzytelniania plików cookie z poziomu klienta platformy .NET wymaga, aby aplikacja zapewniała interfejs API do wymiany danych uwierzytelniania dla plików cookie.

### <a name="bearer-token-authentication"></a>Uwierzytelnianie tokenu okaziciela

Klient może dostarczyć token dostępu zamiast korzystać z pliku cookie. Serwer sprawdza token i używa go do identyfikowania użytkownika. Sprawdzanie poprawności jest wykonywane tylko wtedy, gdy połączenie zostało nawiązane. W trakcie trwania połączenia serwer nie zostanie automatycznie ponownie sprawdzony w celu sprawdzenia odwołania tokenu.

Na serwerze uwierzytelnianie tokenu okaziciela jest konfigurowane przy użyciu [oprogramowania pośredniczącego okaziciela JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

W kliencie JavaScript token może być dostarczony przy użyciu opcji [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) .

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

W kliencie .NET istnieje podobna Właściwość [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) , której można użyć do skonfigurowania tokenu:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> Funkcja tokenu dostępu dostarczana jest wywoływana przed **każdym** żądaniem http wykonywanym przez sygnalizujący. Jeśli musisz odnowić token, aby zachować aktywne połączenie (ponieważ może ono wygasnąć podczas połączenia), zrób to w ramach tej funkcji i zwróć zaktualizowany token.

W standardowym interfejsie API sieci Web tokeny okaziciela są wysyłane w nagłówku HTTP. Jednak sygnalizujący nie może ustawić tych nagłówków w przeglądarkach podczas korzystania z niektórych transportów. W przypadku korzystania z obiektów WebSockets i zdarzeń wysyłanych przez serwer token jest przekazywany jako parametr ciągu zapytania. Aby można było obsługiwać ten serwer na serwerze, wymagana jest dodatkowa konfiguracja:

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a>Pliki cookie a tokeny okaziciela 

Ponieważ pliki cookie są specyficzne dla przeglądarek, wysyłanie ich z innych rodzajów klientów zwiększa złożoność w porównaniu z wysyłaniem tokenów okaziciela. Z tego powodu uwierzytelnianie plików cookie nie jest zalecane, chyba że aplikacja wymaga tylko uwierzytelnienia użytkowników z poziomu klienta przeglądarki. Uwierzytelnianie tokenów okaziciela jest zalecanym rozwiązaniem w przypadku korzystania z klientów innych niż klient przeglądarki.

### <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

Jeśli w aplikacji skonfigurowano [uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth) , program sygnalizujący może użyć tej tożsamości do zabezpieczania centrów. Jednak w celu wysyłania komunikatów do poszczególnych użytkowników należy dodać niestandardowego dostawcę identyfikatora użytkownika. Wynika to z faktu, że system uwierzytelniania systemu Windows nie udostępnia "identyfikatora nazwy", którego używa program sygnalizujący do określenia nazwy użytkownika.

Dodaj nową klasę, która implementuje `IUserIdProvider` i pobiera jedno z oświadczeń od użytkownika do użycia jako identyfikator. Aby na przykład użyć żądania "name" (czyli nazwy użytkownika systemu Windows w formularzu `[Domain]\[Username]`), Utwórz następującą klasę:

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

`ClaimTypes.Name`Zamiast tego`User` można użyć dowolnej wartości z (na przykład identyfikatora SID systemu Windows itp.).

> [!NOTE]
> Wybrana wartość musi być unikatowa wśród wszystkich użytkowników w systemie. W przeciwnym razie komunikat przeznaczony dla jednego użytkownika może zostać zakończony przez innego użytkownika.

Zarejestruj ten składnik w `Startup.ConfigureServices` metodzie.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

W kliencie .NET należy włączyć uwierzytelnianie systemu Windows, ustawiając właściwość [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) :

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

Uwierzytelnianie systemu Windows jest obsługiwane tylko przez klienta przeglądarki tylko w przypadku korzystania z programu Microsoft Internet Explorer lub Microsoft Edge.

### <a name="use-claims-to-customize-identity-handling"></a>Używanie oświadczeń do dostosowywania obsługi tożsamości

Aplikacja, która uwierzytelnia użytkowników, może dziedziczyć identyfikatory użytkowników z oświadczeń użytkowników. Aby określić, jak program sygnalizujący tworzy identyfikatory użytkowników `IUserIdProvider` , zaimplementuj i zarejestruj implementację.

Przykładowy kod demonstruje, jak używać oświadczeń do wybierania adresu e-mail użytkownika jako właściwości identyfikującej. 

> [!NOTE]
> Wybrana wartość musi być unikatowa wśród wszystkich użytkowników w systemie. W przeciwnym razie komunikat przeznaczony dla jednego użytkownika może zostać zakończony przez innego użytkownika.

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

Rejestracja konta dodaje do bazy danych tożsamości ASP.NET `ClaimsTypes.Email` zgłoszenie typu.

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

Zarejestruj ten składnik w `Startup.ConfigureServices`.

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Autoryzuj użytkowników do uzyskiwania dostępu do centrów i metod centrów

Domyślnie wszystkie metody w koncentratorze mogą być wywoływane przez nieuwierzytelniony użytkownik. W celu wymagania uwierzytelniania należy zastosować atrybut [Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) do centrum:

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

Można użyć argumentów konstruktora i właściwości `[Authorize]` atrybutu w celu ograniczenia dostępu tylko do użytkowników zgodnych z określonymi [zasadami autoryzacji](xref:security/authorization/policies). Na przykład jeśli masz niestandardowe zasady autoryzacji o nazwie `MyAuthorizationPolicy` , możesz upewnić się, że tylko użytkownicy pasujący do tych zasad mogą uzyskać dostęp do centrum przy użyciu następującego kodu:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

Do `[Authorize]` poszczególnych metod centrów można również zastosować atrybut. Jeśli bieżący użytkownik nie jest zgodny z zasadami zastosowanymi do metody, zwracany jest błąd do obiektu wywołującego:

```csharp
[Authorize]
public class ChatHub : Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```

::: moniker range=">= aspnetcore-3.0"

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a>Używanie programów obsługi autoryzacji do dostosowywania autoryzacji metody centrum

Sygnalizujący udostępnia zasób niestandardowy do obsługi autoryzacji, gdy metoda centrum wymaga autoryzacji. Zasób jest wystąpieniem `HubInvocationContext`. `HubInvocationContext` Obejmuje,nazwęwywoływanejmetodycentrum`HubCallerContext`oraz argumenty metody centrum.

Rozważmy przykład pokoju czatu umożliwiającego logowanie do wielu organizacji za pośrednictwem Azure Active Directory. Każda osoba mająca konto Microsoft może zalogować się do programu chat, ale tylko członkowie organizacji będącej właścicielem będą mogli uniemożliwić użytkownikom lub wyświetlać historie rozmów użytkowników. Ponadto możemy chcieć ograniczyć niektóre funkcje do określonych użytkowników. Korzystanie z zaktualizowanych funkcji w ASP.NET Core 3,0 jest to w całości możliwe. Zwróć uwagę na `DomainRestrictedRequirement` to, jak służy `IAuthorizationRequirement`jako niestandardowy. Teraz, gdy parametr zasobujestprzesyłany,wewnętrznalogikamożesprawdzićkontekst,wktórymjestwywoływanacentrum,ipodjąćdecyzjedotycząceumożliwieniaużytkownikowiwykonywaniaposzczególnychmetodcentrów.`HubInvocationContext`

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}

public class DomainRestrictedRequirement : 
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>, 
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement, 
        HubInvocationContext resource)
    {
        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name) && 
            context.User.Identity.Name.EndsWith("@microsoft.com"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName,
        string currentUsername)
    {
        return !(currentUsername.Equals("asdf42@microsoft.com") && 
            hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase));
    }
}
```

W `Startup.ConfigureServices`programie Dodaj nowe zasady, dostarczając niestandardowe `DomainRestrictedRequirement` wymagania jako parametr do tworzenia `DomainRestricted` zasad.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services
        .AddAuthorization(options =>
        {
            options.AddPolicy("DomainRestricted", policy =>
            {
                policy.Requirements.Add(new DomainRestrictedRequirement());
            });
        });
}
```

W poprzednim przykładzie `DomainRestrictedRequirement` Klasa jest `IAuthorizationRequirement` zarówno, jak i `AuthorizationHandler` dla tego wymagania. Można podzielić te dwa składniki na osobne klasy w celu oddzielenia obaw. Zaletą podejścia przykładowego jest brak konieczności wstrzykiwania `AuthorizationHandler` podczas uruchamiania, ponieważ wymaganie i procedura obsługi są takie same.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uwierzytelnianie tokenu okaziciela w ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [Autoryzacja na podstawie zasobów](xref:security/authorization/resourcebased)
