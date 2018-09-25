---
title: Uwierzytelnianie i autoryzacja w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: fa5e8d4aea6100c54fd8b98411ef877d1dd8621c
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028208"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="b9417-103">Uwierzytelnianie i autoryzacja w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9417-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="b9417-104">Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="b9417-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="b9417-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="b9417-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="b9417-106">Uwierzytelnianie użytkowników łączących się z Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="b9417-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="b9417-107">SignalR mogą być używane z [uwierzytelnianie programu ASP.NET Core](xref:security/authentication/index) do skojarzenia użytkownika z każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="b9417-107">SignalR can be used with [ASP.NET Core Authentication](xref:security/authentication/index) to associate a user with each connection.</span></span> <span data-ttu-id="b9417-108">W centrum danych uwierzytelniania są dostępne z [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) właściwości.</span><span class="sxs-lookup"><span data-stu-id="b9417-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="b9417-109">Uwierzytelnianie umożliwia piaście aby wywoływać metody na wszystkie połączenia skojarzone z użytkownikiem (zobacz [zarządzania użytkownikami i grupami w SignalR](xref:signalr/groups) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="b9417-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="b9417-110">Wiele połączeń mogą być skojarzone z żadnym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="b9417-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="b9417-111">Plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="b9417-111">Cookie authentication</span></span>

<span data-ttu-id="b9417-112">W aplikacji opartej na przeglądarce uwierzytelniania plików cookie umożliwia istniejących poświadczeń użytkownika w taki sposób, aby automatycznie przekazywane z połączeniami SignalR.</span><span class="sxs-lookup"><span data-stu-id="b9417-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="b9417-113">Podczas korzystania z klienta przeglądarki, dodatkowa konfiguracja nie jest potrzebna.</span><span class="sxs-lookup"><span data-stu-id="b9417-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="b9417-114">Jeśli użytkownik jest zalogowany do aplikacji, połączenia SignalR automatycznie dziedziczy tego uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="b9417-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="b9417-115">Plik cookie uwierzytelniania nie jest zalecane, chyba że aplikacja musi się tylko do uwierzytelniania użytkowników z klienta przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b9417-115">Cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="b9417-116">Korzystając z [klient modelu .NET](xref:signalr/dotnet-client), `Cookies` właściwości można skonfigurować w `.WithUrl` wywołanie w celu zapewnienia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="b9417-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="b9417-117">Jednak przy użyciu uwierzytelniania plików cookie z klienta .NET wymaga aplikacji, aby dostarczać interfejs API w celu wymiany danych uwierzytelniania dla pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="b9417-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="b9417-118">Uwierzytelnianie przy użyciu tokenów elementu nośnego</span><span class="sxs-lookup"><span data-stu-id="b9417-118">Bearer token authentication</span></span>

<span data-ttu-id="b9417-119">Uwierzytelnianie przy użyciu tokenów elementu nośnego zaleca się podczas korzystania z klientów innych niż klient przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b9417-119">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span> <span data-ttu-id="b9417-120">W tym podejściu klienta zawiera token dostępu, która serwer sprawdza poprawność używana do identyfikowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b9417-120">In this approach, the client provides an access token that the server validates and uses to identify the user.</span></span> <span data-ttu-id="b9417-121">Szczegóły tokenu uwierzytelniania elementu nośnego wykraczają poza zakres tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b9417-121">The details of bearer token authentication are beyond the scope of this document.</span></span> <span data-ttu-id="b9417-122">Na serwerze uwierzytelniania tokenu elementu nośnego jest skonfigurowany przy użyciu [oprogramowanie pośredniczące elementu nośnego JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="b9417-122">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="b9417-123">W kliencie JavaScript, można określić token za pomocą [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) opcji.</span><span class="sxs-lookup"><span data-stu-id="b9417-123">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="b9417-124">W kliencie programu .NET jest podobny [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) właściwość, która może służyć do konfigurowania tokenu:</span><span class="sxs-lookup"><span data-stu-id="b9417-124">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="b9417-125">Funkcja tokenu dostępu, należy podać jest wywoływana przed **co** żądania HTTP przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="b9417-125">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="b9417-126">Jeśli musisz odnowić token, aby zachować połączenie jako aktywne (ponieważ mogą stracić ważność podczas nawiązywania połączenia), to zrobić w ramach tej funkcji i zwróć zaktualizowanego tokenu.</span><span class="sxs-lookup"><span data-stu-id="b9417-126">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="b9417-127">W standardowych interfejsów API sieci web tokenów elementu nośnego, są wysyłane w nagłówku HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9417-127">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="b9417-128">Nie można ustawić tych nagłówków w przeglądarkach, korzystając z niektórych transportu jest jednak SignalR.</span><span class="sxs-lookup"><span data-stu-id="b9417-128">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="b9417-129">Korzystając z funkcji WebSockets i zdarzenia Server-Sent, token jest przekazywany jako parametr ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="b9417-129">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="b9417-130">Aby to umożliwić, na serwerze, wymagana jest dodatkowa konfiguracja:</span><span class="sxs-lookup"><span data-stu-id="b9417-130">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="windows-authentication"></a><span data-ttu-id="b9417-131">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b9417-131">Windows authentication</span></span>

<span data-ttu-id="b9417-132">Jeśli [uwierzytelniania Windows](xref:security/authentication/windowsauth) jest skonfigurowana w Twojej aplikacji SignalR można użyć tej tożsamości do zabezpieczania koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="b9417-132">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="b9417-133">Jednak aby można było wysyłać komunikaty do poszczególnych użytkowników, należy dodać niestandardowego dostawcy Identyfikatora użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b9417-133">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="b9417-134">Jest to spowodowane uwierzytelniania systemu Windows nie zapewnia roszczenia "Identyfikator nazwy", która korzysta z biblioteki SignalR można ustalić nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b9417-134">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="b9417-135">Dodaj nową klasę, która implementuje `IUserIdProvider` i pobieranie jedno z oświadczeń użytkownika do wykorzystania jako identyfikator.</span><span class="sxs-lookup"><span data-stu-id="b9417-135">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="b9417-136">Na przykład, aby użyć oświadczenia "Name" (czyli nazwę użytkownika Windows w postaci `[Domain]\[Username]`), utworzyć następujące klasy:</span><span class="sxs-lookup"><span data-stu-id="b9417-136">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

<span data-ttu-id="b9417-137">Zamiast `ClaimTypes.Name`, możesz użyć dowolnej wartości z `User` (np. identyfikator Windows SID itd.).</span><span class="sxs-lookup"><span data-stu-id="b9417-137">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="b9417-138">Wartość, którą wybierzesz musi być unikatowa wśród wszystkich użytkowników w Twoim systemie.</span><span class="sxs-lookup"><span data-stu-id="b9417-138">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="b9417-139">W przeciwnym razie komunikat przeznaczony dla jednego użytkownika może się okazać, przechodząc do innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b9417-139">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="b9417-140">Zarejestruj ten składnik w usługi `Startup.ConfigureServices` metoda **po** wywołania `.AddSignalR`</span><span class="sxs-lookup"><span data-stu-id="b9417-140">Register this component in your `Startup.ConfigureServices` method **after** the call to `.AddSignalR`</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="b9417-141">W kliencie programu .NET, należy włączyć uwierzytelnianie Windows, ustawiając [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) właściwości:</span><span class="sxs-lookup"><span data-stu-id="b9417-141">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="b9417-142">Uwierzytelnianie Windows tylko jest obsługiwany przez klienta przeglądarki, korzystając z programu Microsoft Internet Explorer lub Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="b9417-142">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="b9417-143">Zezwolić użytkownikom na dostęp do koncentratorów i metod koncentratorów</span><span class="sxs-lookup"><span data-stu-id="b9417-143">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="b9417-144">Domyślnie wszystkie metody w Centrum można wywoływać za pomocą nieuwierzytelniony użytkownik.</span><span class="sxs-lookup"><span data-stu-id="b9417-144">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="b9417-145">Aby wymagać uwierzytelniania, należy zastosować [Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) atrybutu do Centrum:</span><span class="sxs-lookup"><span data-stu-id="b9417-145">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="b9417-146">Można użyć w argumentach konstruktora i właściwości działania `[Authorize]` atrybutu, aby ograniczyć dostęp do użytkowników tylko pasujących do określonych [zasady autoryzacji](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="b9417-146">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="b9417-147">Na przykład, jeśli masz niestandardowych zasad autoryzacji o nazwie `MyAuthorizationPolicy` upewnij się, tylko użytkownicy dopasowania tych zasad mają dostęp do Centrum, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="b9417-147">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="b9417-148">Może mieć metod koncentratora poszczególnych `[Authorize]` zastosowany także.</span><span class="sxs-lookup"><span data-stu-id="b9417-148">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="b9417-149">Jeśli bieżący użytkownik nie jest zgodny zasady zastosowane do metody, błąd jest zwracany do obiektu wywołującego:</span><span class="sxs-lookup"><span data-stu-id="b9417-149">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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
