---
title: Uwierzytelnianie i autoryzacja w usłudze ASP.NET Core sygnalizujący
author: bradygaster
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w usłudze ASP.NET Core sygnalizujący.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 07/15/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: da226f4e192be8e34a0b2cec1493a1353c995279
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746528"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="6ba5e-103">Uwierzytelnianie i autoryzacja w usłudze ASP.NET Core sygnalizujący</span><span class="sxs-lookup"><span data-stu-id="6ba5e-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="6ba5e-104">Według [Andrew Stanton-pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="6ba5e-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="6ba5e-105">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="6ba5e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="6ba5e-106">Uwierzytelnianie użytkowników łączących się z centrum sygnałów</span><span class="sxs-lookup"><span data-stu-id="6ba5e-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="6ba5e-107">Program sygnalizujący może być używany z [uwierzytelnianiem ASP.NET Core](xref:security/authentication/identity) , aby skojarzyć użytkownika z każdym połączeniem.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="6ba5e-108">W centrum dane uwierzytelniania są dostępne z [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) właściwości.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="6ba5e-109">Uwierzytelnianie umożliwia centrum wywoływanie metod we wszystkich połączeniach skojarzonych z użytkownikiem (zobacz [Zarządzanie użytkownikami i grupami w programie sygnalizującym](xref:signalr/groups) ), aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="6ba5e-110">Wiele połączeń może być skojarzonych z pojedynczym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-110">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="6ba5e-111">Poniżej przedstawiono przykład, w `Startup.Configure` którym jest używany program sygnalizujący i ASP.NET Core Authentication:</span><span class="sxs-lookup"><span data-stu-id="6ba5e-111">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    app.UseRouting();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chat");
        endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

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
> <span data-ttu-id="6ba5e-112">Kolejność, w której zarejestrowano sygnał i ASP.NET Core uwierzytelniania oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-112">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="6ba5e-113">Zawsze wywołuj `UseAuthentication` przed `UseSignalR` tym, że sygnalizujący `HttpContext`ma użytkownika na.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-113">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

::: moniker-end

### <a name="cookie-authentication"></a><span data-ttu-id="6ba5e-114">Uwierzytelnianie plików cookie</span><span class="sxs-lookup"><span data-stu-id="6ba5e-114">Cookie authentication</span></span>

<span data-ttu-id="6ba5e-115">W aplikacji opartej na przeglądarce uwierzytelnianie plików cookie umożliwia istniejące poświadczenia użytkownika w celu automatycznego przepływania do połączeń sygnałów.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-115">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="6ba5e-116">W przypadku korzystania z klienta przeglądarki nie jest wymagana dodatkowa konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-116">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="6ba5e-117">Jeśli użytkownik jest zalogowany do aplikacji, połączenie sygnalizujące automatycznie dziedziczy to uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-117">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="6ba5e-118">Pliki cookie to specyficzny dla przeglądarki sposób wysyłania tokenów dostępu, ale Klienci niebędący przeglądarkami mogą je wysyłać.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-118">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="6ba5e-119">W przypadku korzystania z `Cookies` [klienta platformy .NET](xref:signalr/dotnet-client)Właściwość można skonfigurować w `.WithUrl` wywołaniu w celu udostępnienia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-119">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="6ba5e-120">Jednak używanie uwierzytelniania plików cookie z poziomu klienta platformy .NET wymaga, aby aplikacja zapewniała interfejs API do wymiany danych uwierzytelniania dla plików cookie.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-120">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="6ba5e-121">Uwierzytelnianie tokenu okaziciela</span><span class="sxs-lookup"><span data-stu-id="6ba5e-121">Bearer token authentication</span></span>

<span data-ttu-id="6ba5e-122">Klient może dostarczyć token dostępu zamiast korzystać z pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-122">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="6ba5e-123">Serwer sprawdza token i używa go do identyfikowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-123">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="6ba5e-124">Sprawdzanie poprawności jest wykonywane tylko wtedy, gdy połączenie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-124">This validation is done only when the connection is established.</span></span> <span data-ttu-id="6ba5e-125">W trakcie trwania połączenia serwer nie zostanie automatycznie ponownie sprawdzony w celu sprawdzenia odwołania tokenu.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-125">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="6ba5e-126">Na serwerze uwierzytelnianie tokenu okaziciela jest konfigurowane przy użyciu [oprogramowania pośredniczącego okaziciela JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="6ba5e-126">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="6ba5e-127">W kliencie JavaScript token może być dostarczony przy użyciu opcji [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) .</span><span class="sxs-lookup"><span data-stu-id="6ba5e-127">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="6ba5e-128">W kliencie .NET istnieje podobna Właściwość [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) , której można użyć do skonfigurowania tokenu:</span><span class="sxs-lookup"><span data-stu-id="6ba5e-128">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="6ba5e-129">Funkcja tokenu dostępu dostarczana jest wywoływana przed **każdym** żądaniem http wykonywanym przez sygnalizujący.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-129">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="6ba5e-130">Jeśli musisz odnowić token, aby zachować aktywne połączenie (ponieważ może ono wygasnąć podczas połączenia), zrób to w ramach tej funkcji i zwróć zaktualizowany token.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-130">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="6ba5e-131">W standardowym interfejsie API sieci Web tokeny okaziciela są wysyłane w nagłówku HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-131">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="6ba5e-132">Jednak sygnalizujący nie może ustawić tych nagłówków w przeglądarkach podczas korzystania z niektórych transportów.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-132">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="6ba5e-133">W przypadku korzystania z obiektów WebSockets i zdarzeń wysyłanych przez serwer token jest przekazywany jako parametr ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-133">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="6ba5e-134">Aby można było obsługiwać ten serwer na serwerze, wymagana jest dodatkowa konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="6ba5e-134">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="6ba5e-135">Pliki cookie a tokeny okaziciela</span><span class="sxs-lookup"><span data-stu-id="6ba5e-135">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="6ba5e-136">Ponieważ pliki cookie są specyficzne dla przeglądarek, wysyłanie ich z innych rodzajów klientów zwiększa złożoność w porównaniu z wysyłaniem tokenów okaziciela.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-136">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="6ba5e-137">Z tego powodu uwierzytelnianie plików cookie nie jest zalecane, chyba że aplikacja wymaga tylko uwierzytelnienia użytkowników z poziomu klienta przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-137">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="6ba5e-138">Uwierzytelnianie tokenów okaziciela jest zalecanym rozwiązaniem w przypadku korzystania z klientów innych niż klient przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-138">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="6ba5e-139">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="6ba5e-139">Windows authentication</span></span>

<span data-ttu-id="6ba5e-140">Jeśli w aplikacji skonfigurowano [uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth) , program sygnalizujący może użyć tej tożsamości do zabezpieczania centrów.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-140">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="6ba5e-141">Jednak w celu wysyłania komunikatów do poszczególnych użytkowników należy dodać niestandardowego dostawcę identyfikatora użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-141">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="6ba5e-142">Wynika to z faktu, że system uwierzytelniania systemu Windows nie udostępnia "identyfikatora nazwy", którego używa program sygnalizujący do określenia nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-142">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="6ba5e-143">Dodaj nową klasę, która implementuje `IUserIdProvider` i pobiera jedno z oświadczeń od użytkownika do użycia jako identyfikator.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-143">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="6ba5e-144">Aby na przykład użyć żądania "name" (czyli nazwy użytkownika systemu Windows w formularzu `[Domain]\[Username]`), Utwórz następującą klasę:</span><span class="sxs-lookup"><span data-stu-id="6ba5e-144">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="6ba5e-145">`ClaimTypes.Name`Zamiast tego`User` można użyć dowolnej wartości z (na przykład identyfikatora SID systemu Windows itp.).</span><span class="sxs-lookup"><span data-stu-id="6ba5e-145">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="6ba5e-146">Wybrana wartość musi być unikatowa wśród wszystkich użytkowników w systemie.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-146">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="6ba5e-147">W przeciwnym razie komunikat przeznaczony dla jednego użytkownika może zostać zakończony przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-147">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="6ba5e-148">Zarejestruj ten składnik w `Startup.ConfigureServices` metodzie.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-148">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="6ba5e-149">W kliencie .NET należy włączyć uwierzytelnianie systemu Windows, ustawiając właściwość [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) :</span><span class="sxs-lookup"><span data-stu-id="6ba5e-149">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="6ba5e-150">Uwierzytelnianie systemu Windows jest obsługiwane tylko przez klienta przeglądarki tylko w przypadku korzystania z programu Microsoft Internet Explorer lub Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-150">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="6ba5e-151">Używanie oświadczeń do dostosowywania obsługi tożsamości</span><span class="sxs-lookup"><span data-stu-id="6ba5e-151">Use claims to customize identity handling</span></span>

<span data-ttu-id="6ba5e-152">Aplikacja, która uwierzytelnia użytkowników, może dziedziczyć identyfikatory użytkowników z oświadczeń użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-152">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="6ba5e-153">Aby określić, jak program sygnalizujący tworzy identyfikatory użytkowników `IUserIdProvider` , zaimplementuj i zarejestruj implementację.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-153">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="6ba5e-154">Przykładowy kod demonstruje, jak używać oświadczeń do wybierania adresu e-mail użytkownika jako właściwości identyfikującej.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-154">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="6ba5e-155">Wybrana wartość musi być unikatowa wśród wszystkich użytkowników w systemie.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-155">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="6ba5e-156">W przeciwnym razie komunikat przeznaczony dla jednego użytkownika może zostać zakończony przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-156">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="6ba5e-157">Rejestracja konta dodaje do bazy danych tożsamości ASP.NET `ClaimsTypes.Email` zgłoszenie typu.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-157">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="6ba5e-158">Zarejestruj ten składnik w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-158">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="6ba5e-159">Autoryzuj użytkowników do uzyskiwania dostępu do centrów i metod centrów</span><span class="sxs-lookup"><span data-stu-id="6ba5e-159">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="6ba5e-160">Domyślnie wszystkie metody w koncentratorze mogą być wywoływane przez nieuwierzytelniony użytkownik.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-160">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="6ba5e-161">W celu wymagania uwierzytelniania należy zastosować atrybut [Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) do centrum:</span><span class="sxs-lookup"><span data-stu-id="6ba5e-161">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="6ba5e-162">Można użyć argumentów konstruktora i właściwości `[Authorize]` atrybutu w celu ograniczenia dostępu tylko do użytkowników zgodnych z określonymi [zasadami autoryzacji](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="6ba5e-162">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="6ba5e-163">Na przykład jeśli masz niestandardowe zasady autoryzacji o nazwie `MyAuthorizationPolicy` , możesz upewnić się, że tylko użytkownicy pasujący do tych zasad mogą uzyskać dostęp do centrum przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="6ba5e-163">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

<span data-ttu-id="6ba5e-164">Do `[Authorize]` poszczególnych metod centrów można również zastosować atrybut.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-164">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="6ba5e-165">Jeśli bieżący użytkownik nie jest zgodny z zasadami zastosowanymi do metody, zwracany jest błąd do obiektu wywołującego:</span><span class="sxs-lookup"><span data-stu-id="6ba5e-165">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a><span data-ttu-id="6ba5e-166">Używanie programów obsługi autoryzacji do dostosowywania autoryzacji metody centrum</span><span class="sxs-lookup"><span data-stu-id="6ba5e-166">Use authorization handlers to customize hub method authorization</span></span>

<span data-ttu-id="6ba5e-167">Sygnalizujący udostępnia zasób niestandardowy do obsługi autoryzacji, gdy metoda centrum wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-167">SignalR provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="6ba5e-168">Zasób jest wystąpieniem `HubInvocationContext`.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-168">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="6ba5e-169">`HubInvocationContext` Obejmuje,nazwęwywoływanejmetodycentrum`HubCallerContext`oraz argumenty metody centrum.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-169">The `HubInvocationContext` includes the `HubCallerContext`, the name of the hub method being invoked, and the arguments to the hub method.</span></span>

<span data-ttu-id="6ba5e-170">Rozważmy przykład pokoju czatu umożliwiającego logowanie do wielu organizacji za pośrednictwem Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-170">Consider the example of a chat room allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="6ba5e-171">Każda osoba mająca konto Microsoft może zalogować się do programu chat, ale tylko członkowie organizacji będącej właścicielem będą mogli uniemożliwić użytkownikom lub wyświetlać historie rozmów użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-171">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization should be able to ban users or view users' chat histories.</span></span> <span data-ttu-id="6ba5e-172">Ponadto możemy chcieć ograniczyć niektóre funkcje do określonych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-172">Furthermore, we might want to restrict certain functionality from certain users.</span></span> <span data-ttu-id="6ba5e-173">Korzystanie z zaktualizowanych funkcji w ASP.NET Core 3,0 jest to w całości możliwe.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-173">Using the updated features in ASP.NET Core 3.0, this is entirely possible.</span></span> <span data-ttu-id="6ba5e-174">Zwróć uwagę na `DomainRestrictedRequirement` to, jak służy `IAuthorizationRequirement`jako niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-174">Note how the `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="6ba5e-175">Teraz, gdy parametr zasobujestprzesyłany,wewnętrznalogikamożesprawdzićkontekst,wktórymjestwywoływanacentrum,ipodjąćdecyzjedotycząceumożliwieniaużytkownikowiwykonywaniaposzczególnychmetodcentrów.`HubInvocationContext`</span><span class="sxs-lookup"><span data-stu-id="6ba5e-175">Now that the `HubInvocationContext` resource parameter is being passed in, the internal logic can inspect the context in which the Hub is being called and make decisions on allowing the user to execute individual Hub methods.</span></span>

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

<span data-ttu-id="6ba5e-176">W `Startup.ConfigureServices`programie Dodaj nowe zasady, dostarczając niestandardowe `DomainRestrictedRequirement` wymagania jako parametr do tworzenia `DomainRestricted` zasad.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-176">In `Startup.ConfigureServices`, add the new policy, providing the custom `DomainRestrictedRequirement` requirement as a parameter to create the `DomainRestricted` policy.</span></span>

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

<span data-ttu-id="6ba5e-177">W poprzednim przykładzie `DomainRestrictedRequirement` Klasa jest `IAuthorizationRequirement` zarówno, jak i `AuthorizationHandler` dla tego wymagania.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-177">In the preceding example, the `DomainRestrictedRequirement` class is both an `IAuthorizationRequirement` and its own `AuthorizationHandler` for that requirement.</span></span> <span data-ttu-id="6ba5e-178">Można podzielić te dwa składniki na osobne klasy w celu oddzielenia obaw.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-178">It's acceptable to split these two components into separate classes to separate concerns.</span></span> <span data-ttu-id="6ba5e-179">Zaletą podejścia przykładowego jest brak konieczności wstrzykiwania `AuthorizationHandler` podczas uruchamiania, ponieważ wymaganie i procedura obsługi są takie same.</span><span class="sxs-lookup"><span data-stu-id="6ba5e-179">A benefit of the example's approach is there's no need to inject the `AuthorizationHandler` during startup, as the requirement and the handler are the same thing.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6ba5e-180">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6ba5e-180">Additional resources</span></span>

* [<span data-ttu-id="6ba5e-181">Uwierzytelnianie tokenu okaziciela w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ba5e-181">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="6ba5e-182">Autoryzacja na podstawie zasobów</span><span class="sxs-lookup"><span data-stu-id="6ba5e-182">Resource-based Authorization</span></span>](xref:security/authorization/resourcebased)
