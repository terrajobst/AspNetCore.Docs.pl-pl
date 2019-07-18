---
title: Uwierzytelnianie i autoryzacja w programie gRPC for ASP.NET Core
author: jamesnk
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w programie gRPC for ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 06/07/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 49024295e4db7976924397bb24567d92d6298562
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308816"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="3cfb1-103">Uwierzytelnianie i autoryzacja w programie gRPC for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3cfb1-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="3cfb1-104">Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="3cfb1-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="3cfb1-105">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="3cfb1-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="3cfb1-106">Uwierzytelnianie użytkowników wywołujących usługę gRPC</span><span class="sxs-lookup"><span data-stu-id="3cfb1-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="3cfb1-107">gRPC można użyć z [uwierzytelnianiem ASP.NET Core](xref:security/authentication/identity) , aby skojarzyć użytkownika z każdym wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="3cfb1-108">Poniżej przedstawiono przykład `Startup.Configure` użycia gRPC i uwierzytelniania ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3cfb1-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        routes.MapGrpcService<GreeterService>();
    });
}
```

> [!NOTE]
> <span data-ttu-id="3cfb1-109">Kolejność, w której zarejestrowano zagadnienia dotyczące oprogramowania pośredniczącego uwierzytelniania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="3cfb1-110">Zawsze używaj `UseAuthentication` metody `UseAuthorization` Call `UseRouting` i After `UseEndpoints`i before.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="3cfb1-111">Po skonfigurowaniu uwierzytelniania można uzyskać dostęp do użytkownika w metodach usługi gRPC za pośrednictwem `ServerCallContext`.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="3cfb1-112">Uwierzytelnianie tokenu okaziciela</span><span class="sxs-lookup"><span data-stu-id="3cfb1-112">Bearer token authentication</span></span>

<span data-ttu-id="3cfb1-113">Klient może udostępnić token dostępu do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="3cfb1-114">Serwer sprawdza token i używa go do identyfikowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="3cfb1-115">Na serwerze uwierzytelnianie tokenu okaziciela jest konfigurowane przy użyciu [oprogramowania pośredniczącego okaziciela JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="3cfb1-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="3cfb1-116">W kliencie .NET gRPC token może być wysyłany z wywołaniami jako nagłówek:</span><span class="sxs-lookup"><span data-stu-id="3cfb1-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

```csharp
public bool DoAuthenticatedCall(
    Ticketer.TicketerClient client, string token)
{
    var headers = new Metadata();
    headers.Add("Authorization", $"Bearer {token}");

    var request = new BuyTicketsRequest { Count = 1 };
    var response = await client.BuyTicketsAsync(request, headers);

    return response.Success;
}
```

### <a name="client-certificate-authentication"></a><span data-ttu-id="3cfb1-117">Uwierzytelnianie certyfikatu klienta</span><span class="sxs-lookup"><span data-stu-id="3cfb1-117">Client certificate authentication</span></span>

<span data-ttu-id="3cfb1-118">Klient może również udostępnić certyfikat klienta na potrzeby uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="3cfb1-119">[Uwierzytelnianie certyfikatu](https://tools.ietf.org/html/rfc5246#section-7.4.4) odbywa się na poziomie protokołu TLS, o ile nie zostanie kiedykolwiek przeASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="3cfb1-120">Gdy żądanie zostanie wprowadzone ASP.NET Core, [Pakiet uwierzytelniania certyfikatu klienta](xref:security/authentication/certauth) umożliwia rozpoznanie certyfikatu w usłudze `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="3cfb1-121">Host musi być skonfigurowany do akceptowania certyfikatów klienta.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="3cfb1-122">Zobacz [Konfigurowanie hosta, aby wymagać certyfikatów](xref:security/authentication/certauth#configure-your-host-to-require-certificates) w celu uzyskania informacji na temat akceptowania certyfikatów klienta w usługach Kestrel, IIS i Azure.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="3cfb1-123">W kliencie .NET gRPC certyfikat klienta zostaje dodany do `HttpClientHandler` programu, który jest następnie używany do tworzenia klienta gRPC:</span><span class="sxs-lookup"><span data-stu-id="3cfb1-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC client
    var httpClient = new HttpClient(handler);
    httpClient.BaseAddress = new Uri(baseAddress);

    return GrpcClient.Create<Ticketer.TicketerClient>(httpClient);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="3cfb1-124">Inne mechanizmy uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="3cfb1-124">Other authentication mechanisms</span></span>

<span data-ttu-id="3cfb1-125">Oprócz tokenu okaziciela i uwierzytelniania certyfikatu klienta wszystkie ASP.NET Core obsługiwane mechanizmy uwierzytelniania, takie jak OAuth, OpenID Connect i Negotiate, powinny współpracować z gRPC.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-125">In addition to bearer token and client certificate authentication, all ASP.NET Core supported authentication mechanisms such as OAuth, OpenID and Negotiate should work with gRPC.</span></span> <span data-ttu-id="3cfb1-126">Aby uzyskać więcej informacji na temat konfigurowania uwierzytelniania po stronie serwera, odwiedź stronę [ASP.NET Core Authentication](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="3cfb1-126">Visit [ASP.NET Core authentication](xref:security/authentication/identity) for more information for configuring authentication on the server side.</span></span>

<span data-ttu-id="3cfb1-127">Konfiguracja po stronie klienta będzie zależeć od używanego mechanizmu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-127">Client side configuration will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="3cfb1-128">W poprzednim tokenie okaziciela i uwierzytelniania certyfikatu klienta przedstawiono kilka sposobów skonfigurowania klienta gRPC do wysyłania metadanych uwierzytelniania z wywołaniami gRPC:</span><span class="sxs-lookup"><span data-stu-id="3cfb1-128">The previous bearer token and client certificate authentication examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="3cfb1-129">Klienci z jednoznacznie określonym typem `HttpClient` gRPC używają wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-129">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="3cfb1-130">Uwierzytelnianie można skonfigurować w [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)programie lub poprzez dodanie wystąpień niestandardowych [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) do programu. `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="3cfb1-130">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="3cfb1-131">Każde wywołanie gRPC ma opcjonalny `CallOptions` argument.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-131">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="3cfb1-132">Nagłówki niestandardowe można wysyłać przy użyciu kolekcji nagłówków opcji.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-132">Custom headers can be sent using the option's headers collection.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="3cfb1-133">Autoryzuj użytkowników do uzyskiwania dostępu do usług i metod usług</span><span class="sxs-lookup"><span data-stu-id="3cfb1-133">Authorize users to access services and service methods</span></span>

<span data-ttu-id="3cfb1-134">Domyślnie wszystkie metody w usłudze mogą być wywoływane przez nieuwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-134">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="3cfb1-135">Aby wymagać uwierzytelniania, należy zastosować atrybut [[autoryzuje]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) do usługi:</span><span class="sxs-lookup"><span data-stu-id="3cfb1-135">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="3cfb1-136">Można użyć argumentów konstruktora i właściwości `[Authorize]` atrybutu w celu ograniczenia dostępu tylko do użytkowników zgodnych z określonymi [zasadami autoryzacji](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="3cfb1-136">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="3cfb1-137">Na przykład jeśli masz zasady autoryzacji niestandardowej o nazwie `MyAuthorizationPolicy`, upewnij się, że tylko użytkownicy pasujący do tych zasad mogą uzyskać dostęp do usługi przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="3cfb1-137">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="3cfb1-138">Do `[Authorize]` poszczególnych metod usługi można zastosować atrybut.</span><span class="sxs-lookup"><span data-stu-id="3cfb1-138">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="3cfb1-139">Jeśli bieżący użytkownik nie jest zgodny z zasadami stosowanymi **zarówno** w metodzie, jak i klasie, do obiektu wywołującego jest zwracany błąd:</span><span class="sxs-lookup"><span data-stu-id="3cfb1-139">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
    public override Task<AvailableTicketsResponse> GetAvailableTickets(
        Empty request, ServerCallContext context)
    {
        // ... buy tickets for the current user ...
    }

    [Authorize("Administrators")]
    public override Task<BuyTicketsResponse> RefundTickets(
        BuyTicketsRequest request, ServerCallContext context)
    {
        // ... refund tickets (something only Administrators can do) ..
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="3cfb1-140">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3cfb1-140">Additional resources</span></span>

* [<span data-ttu-id="3cfb1-141">Uwierzytelnianie tokenu okaziciela w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3cfb1-141">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="3cfb1-142">Konfigurowanie uwierzytelniania certyfikatu klienta w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3cfb1-142">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
