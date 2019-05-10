---
title: Uwierzytelnianie i autoryzacja w biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 05/09/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e8f9dc48be780fb91bdec6ea4d579f5e4f16197b
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516939"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="5b625-103">Uwierzytelnianie i autoryzacja w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b625-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="5b625-104">Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="5b625-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="5b625-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="5b625-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="5b625-106">Uwierzytelnianie użytkowników łączących się z Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="5b625-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="5b625-107">SignalR mogą być używane z [uwierzytelnianie programu ASP.NET Core](xref:security/authentication/identity) do skojarzenia użytkownika z każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="5b625-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="5b625-108">W centrum danych uwierzytelniania są dostępne z [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) właściwości.</span><span class="sxs-lookup"><span data-stu-id="5b625-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="5b625-109">Uwierzytelnianie umożliwia piaście aby wywoływać metody na wszystkie połączenia skojarzone z użytkownikiem (zobacz [zarządzania użytkownikami i grupami w SignalR](xref:signalr/groups) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="5b625-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="5b625-110">Wiele połączeń mogą być skojarzone z żadnym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="5b625-110">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="5b625-111">Oto przykład `Startup.Configure` , który używa uwierzytelniania SignalR i ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5b625-111">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="5b625-112">Kolejność, w którym należy zarejestrować SignalR i ASP.NET Core spraw oprogramowania pośredniczącego uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="5b625-112">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="5b625-113">Zawsze wywołuj `UseAuthentication` przed `UseSignalR` tak, aby SignalR użytkownika `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="5b625-113">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="5b625-114">Plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="5b625-114">Cookie authentication</span></span>

<span data-ttu-id="5b625-115">W aplikacji opartej na przeglądarce uwierzytelniania plików cookie umożliwia istniejących poświadczeń użytkownika w taki sposób, aby automatycznie przekazywane z połączeniami SignalR.</span><span class="sxs-lookup"><span data-stu-id="5b625-115">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="5b625-116">Podczas korzystania z klienta przeglądarki, dodatkowa konfiguracja nie jest potrzebna.</span><span class="sxs-lookup"><span data-stu-id="5b625-116">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="5b625-117">Jeśli użytkownik jest zalogowany do aplikacji, połączenia SignalR automatycznie dziedziczy tego uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="5b625-117">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="5b625-118">Pliki cookie są specyficzne dla przeglądarki sposób przesyłania tokenów dostępu, ale klientów nie korzystających z przeglądarki mogą wysyłać je.</span><span class="sxs-lookup"><span data-stu-id="5b625-118">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="5b625-119">Korzystając z [klient modelu .NET](xref:signalr/dotnet-client), `Cookies` właściwości można skonfigurować w `.WithUrl` wywołanie w celu zapewnienia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="5b625-119">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="5b625-120">Jednak przy użyciu uwierzytelniania plików cookie z klienta .NET wymaga aplikacji, aby dostarczać interfejs API w celu wymiany danych uwierzytelniania dla pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="5b625-120">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="5b625-121">Uwierzytelnianie przy użyciu tokenów elementu nośnego</span><span class="sxs-lookup"><span data-stu-id="5b625-121">Bearer token authentication</span></span>

<span data-ttu-id="5b625-122">Klient może dostarczyć token dostępu, zamiast korzystać z pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="5b625-122">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="5b625-123">Serwer sprawdza poprawność tokenu i używa ich do identyfikacji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5b625-123">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="5b625-124">To jest sprawdzana tylko wtedy, gdy połączenie zostanie nawiązane.</span><span class="sxs-lookup"><span data-stu-id="5b625-124">This validation is done only when the connection is established.</span></span> <span data-ttu-id="5b625-125">W okresie obowiązywania połączenia serwera nie automatycznie ponownie sprawdź poprawność pod kątem tokenu odwołania.</span><span class="sxs-lookup"><span data-stu-id="5b625-125">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="5b625-126">Na serwerze uwierzytelniania tokenu elementu nośnego jest skonfigurowany przy użyciu [oprogramowanie pośredniczące elementu nośnego JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="5b625-126">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="5b625-127">W kliencie JavaScript, można określić token za pomocą [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) opcji.</span><span class="sxs-lookup"><span data-stu-id="5b625-127">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="5b625-128">W kliencie programu .NET jest podobny [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) właściwość, która może służyć do konfigurowania tokenu:</span><span class="sxs-lookup"><span data-stu-id="5b625-128">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="5b625-129">Funkcja tokenu dostępu, należy podać jest wywoływana przed **co** żądania HTTP przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="5b625-129">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="5b625-130">Jeśli musisz odnowić token, aby zachować połączenie jako aktywne (ponieważ mogą stracić ważność podczas nawiązywania połączenia), to zrobić w ramach tej funkcji i zwróć zaktualizowanego tokenu.</span><span class="sxs-lookup"><span data-stu-id="5b625-130">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="5b625-131">W standardowych interfejsów API sieci web tokenów elementu nośnego, są wysyłane w nagłówku HTTP.</span><span class="sxs-lookup"><span data-stu-id="5b625-131">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="5b625-132">Nie można ustawić tych nagłówków w przeglądarkach, korzystając z niektórych transportu jest jednak SignalR.</span><span class="sxs-lookup"><span data-stu-id="5b625-132">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="5b625-133">Korzystając z funkcji WebSockets i zdarzenia Server-Sent, token jest przekazywany jako parametr ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5b625-133">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="5b625-134">Aby to umożliwić, na serwerze, wymagana jest dodatkowa konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="5b625-134">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="5b625-135">Pliki cookie, a tokenów elementu nośnego</span><span class="sxs-lookup"><span data-stu-id="5b625-135">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="5b625-136">Ponieważ pliki cookie są specyficzne dla przeglądarek, wysyłając je od innych rodzajów klientów dodaje złożoności w porównaniu do wysyłania tokenów elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="5b625-136">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="5b625-137">Z tego powodu uwierzytelniania plików cookie nie jest zalecane, chyba że aplikacja musi się tylko do uwierzytelniania użytkowników z klienta przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="5b625-137">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="5b625-138">Uwierzytelnianie przy użyciu tokenów elementu nośnego zaleca się podczas korzystania z klientów innych niż klient przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="5b625-138">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="5b625-139">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="5b625-139">Windows authentication</span></span>

<span data-ttu-id="5b625-140">Jeśli [uwierzytelniania Windows](xref:security/authentication/windowsauth) jest skonfigurowana w Twojej aplikacji SignalR można użyć tej tożsamości do zabezpieczania koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="5b625-140">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="5b625-141">Jednak aby można było wysyłać komunikaty do poszczególnych użytkowników, należy dodać niestandardowego dostawcy Identyfikatora użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5b625-141">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="5b625-142">Jest to spowodowane uwierzytelniania systemu Windows nie zapewnia roszczenia "Identyfikator nazwy", która korzysta z biblioteki SignalR można ustalić nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5b625-142">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="5b625-143">Dodaj nową klasę, która implementuje `IUserIdProvider` i pobieranie jedno z oświadczeń użytkownika do wykorzystania jako identyfikator.</span><span class="sxs-lookup"><span data-stu-id="5b625-143">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="5b625-144">Na przykład, aby użyć oświadczenia "Name" (czyli nazwę użytkownika Windows w postaci `[Domain]\[Username]`), utworzyć następujące klasy:</span><span class="sxs-lookup"><span data-stu-id="5b625-144">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="5b625-145">Zamiast `ClaimTypes.Name`, możesz użyć dowolnej wartości z `User` (np. identyfikator Windows SID itd.).</span><span class="sxs-lookup"><span data-stu-id="5b625-145">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="5b625-146">Wartość, którą wybierzesz musi być unikatowa wśród wszystkich użytkowników w Twoim systemie.</span><span class="sxs-lookup"><span data-stu-id="5b625-146">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="5b625-147">W przeciwnym razie komunikat przeznaczony dla jednego użytkownika może się okazać, przechodząc do innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5b625-147">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="5b625-148">Zarejestruj ten składnik w usługi `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="5b625-148">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="5b625-149">W kliencie programu .NET, należy włączyć uwierzytelnianie Windows, ustawiając [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) właściwości:</span><span class="sxs-lookup"><span data-stu-id="5b625-149">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="5b625-150">Uwierzytelnianie Windows tylko jest obsługiwany przez klienta przeglądarki, korzystając z programu Microsoft Internet Explorer lub Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="5b625-150">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="5b625-151">Dostosuj obsługę tożsamości oświadczeń użycia</span><span class="sxs-lookup"><span data-stu-id="5b625-151">Use claims to customize identity handling</span></span>

<span data-ttu-id="5b625-152">Aplikacja, która uwierzytelnia użytkowników mogą dziedziczyć identyfikatory użytkowników SignalR oświadczenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5b625-152">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="5b625-153">Aby określić, jak SignalR tworzy identyfikatorów użytkowników, należy zaimplementować `IUserIdProvider` i zarejestruj wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="5b625-153">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="5b625-154">Przykładowy kod pokazuje, jak skorzystać z oświadczeń wybrać adres e-mail użytkownika jako właściwości identyfikującej.</span><span class="sxs-lookup"><span data-stu-id="5b625-154">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="5b625-155">Wartość, którą wybierzesz musi być unikatowa wśród wszystkich użytkowników w Twoim systemie.</span><span class="sxs-lookup"><span data-stu-id="5b625-155">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="5b625-156">W przeciwnym razie komunikat przeznaczony dla jednego użytkownika może się okazać, przechodząc do innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5b625-156">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="5b625-157">Rejestracja konta dodaje oświadczenie z typem `ClaimsTypes.Email` do bazy danych tożsamości ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b625-157">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="5b625-158">Zarejestruj ten składnik w usługi `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5b625-158">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="5b625-159">Zezwolić użytkownikom na dostęp do koncentratorów i metod koncentratorów</span><span class="sxs-lookup"><span data-stu-id="5b625-159">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="5b625-160">Domyślnie wszystkie metody w Centrum można wywoływać za pomocą nieuwierzytelniony użytkownik.</span><span class="sxs-lookup"><span data-stu-id="5b625-160">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="5b625-161">Aby wymagać uwierzytelniania, należy zastosować [Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) atrybutu do Centrum:</span><span class="sxs-lookup"><span data-stu-id="5b625-161">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="5b625-162">Można użyć w argumentach konstruktora i właściwości działania `[Authorize]` atrybutu, aby ograniczyć dostęp do użytkowników tylko pasujących do określonych [zasady autoryzacji](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="5b625-162">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="5b625-163">Na przykład, jeśli masz niestandardowych zasad autoryzacji o nazwie `MyAuthorizationPolicy` upewnij się, tylko użytkownicy dopasowania tych zasad mają dostęp do Centrum, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="5b625-163">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="5b625-164">Może mieć metod koncentratora poszczególnych `[Authorize]` zastosowany także.</span><span class="sxs-lookup"><span data-stu-id="5b625-164">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="5b625-165">Jeśli bieżący użytkownik nie jest zgodny zasady zastosowane do metody, błąd jest zwracany do obiektu wywołującego:</span><span class="sxs-lookup"><span data-stu-id="5b625-165">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub: Hub
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

## <a name="additional-resources"></a><span data-ttu-id="5b625-166">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5b625-166">Additional resources</span></span>

* [<span data-ttu-id="5b625-167">Uwierzytelnianie przy użyciu tokenów elementu nośnego w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b625-167">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
