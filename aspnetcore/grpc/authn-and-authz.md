---
title: Uwierzytelnianie i autoryzacja w programie gRPC for ASP.NET Core
author: jamesnk
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w programie gRPC for ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 07/26/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 34f7f8a5a22159329b3d6c4524943434c460c7fb
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602426"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a>Uwierzytelnianie i autoryzacja w programie gRPC for ASP.NET Core

Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-calling-a-grpc-service"></a>Uwierzytelnianie użytkowników wywołujących usługę gRPC

gRPC można użyć z [uwierzytelnianiem ASP.NET Core](xref:security/authentication/identity) , aby skojarzyć użytkownika z każdym wywołaniem.

Poniżej przedstawiono przykład `Startup.Configure` użycia gRPC i uwierzytelniania ASP.NET Core:

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
> Kolejność, w której zarejestrowano zagadnienia dotyczące oprogramowania pośredniczącego uwierzytelniania ASP.NET Core. Zawsze używaj `UseAuthentication` metody `UseAuthorization` Call `UseRouting` i After `UseEndpoints`i before.

Po skonfigurowaniu uwierzytelniania można uzyskać dostęp do użytkownika w metodach usługi gRPC za pośrednictwem `ServerCallContext`.

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a>Uwierzytelnianie tokenu okaziciela

Klient może udostępnić token dostępu do uwierzytelniania. Serwer sprawdza token i używa go do identyfikowania użytkownika.

Na serwerze uwierzytelnianie tokenu okaziciela jest konfigurowane przy użyciu [oprogramowania pośredniczącego okaziciela JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

W kliencie .NET gRPC token może być wysyłany z wywołaniami jako nagłówek:

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

### <a name="client-certificate-authentication"></a>Uwierzytelnianie certyfikatu klienta

Klient może również udostępnić certyfikat klienta na potrzeby uwierzytelniania. [Uwierzytelnianie certyfikatu](https://tools.ietf.org/html/rfc5246#section-7.4.4) odbywa się na poziomie protokołu TLS, o ile nie zostanie kiedykolwiek przeASP.NET Core. Gdy żądanie zostanie wprowadzone ASP.NET Core, [Pakiet uwierzytelniania certyfikatu klienta](xref:security/authentication/certauth) umożliwia rozpoznanie certyfikatu w usłudze `ClaimsPrincipal`.

> [!NOTE]
> Host musi być skonfigurowany do akceptowania certyfikatów klienta. Zobacz [Konfigurowanie hosta, aby wymagać certyfikatów](xref:security/authentication/certauth#configure-your-host-to-require-certificates) w celu uzyskania informacji na temat akceptowania certyfikatów klienta w usługach Kestrel, IIS i Azure.

W kliencie .NET gRPC certyfikat klienta zostaje dodany do `HttpClientHandler` programu, który jest następnie używany do tworzenia klienta gRPC:

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

### <a name="other-authentication-mechanisms"></a>Inne mechanizmy uwierzytelniania

Wiele ASP.NET Core obsługiwanych mechanizmów uwierzytelniania współpracuje z gRPC:

* Azure Active Directory
* Certyfikat klienta
* IdentityServer
* Token JWT
* OAuth 2,0
* OpenID Connect połączenie
* WS-Federation

Aby uzyskać więcej informacji na temat konfigurowania uwierzytelniania na serwerze, zobacz [ASP.NET Core Authentication](xref:security/authentication/identity).

Skonfigurowanie klienta gRPC do korzystania z uwierzytelniania będzie zależeć od używanego mechanizmu uwierzytelniania. W poprzednim tokenie okaziciela i certyfikatach klienta przedstawiono kilka sposobów skonfigurowania klienta gRPC do wysyłania metadanych uwierzytelniania z wywołaniami gRPC:

* Klienci z jednoznacznie określonym typem `HttpClient` gRPC używają wewnętrznie. Uwierzytelnianie można skonfigurować w [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)programie lub poprzez dodanie wystąpień niestandardowych [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) do programu. `HttpClient`
* Każde wywołanie gRPC ma opcjonalny `CallOptions` argument. Nagłówki niestandardowe można wysyłać przy użyciu kolekcji nagłówków opcji.

> [!NOTE]
> Uwierzytelnianie systemu Windows (NTLM/Kerberos/Negotiate) nie może być używane z gRPC. gRPC wymaga protokołu HTTP/2, a protokół HTTP/2 nie obsługuje uwierzytelniania systemu Windows.

## <a name="authorize-users-to-access-services-and-service-methods"></a>Autoryzuj użytkowników do uzyskiwania dostępu do usług i metod usług

Domyślnie wszystkie metody w usłudze mogą być wywoływane przez nieuwierzytelnionych użytkowników. Aby wymagać uwierzytelniania, należy zastosować atrybut [[autoryzuje]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) do usługi:

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

Można użyć argumentów konstruktora i właściwości `[Authorize]` atrybutu w celu ograniczenia dostępu tylko do użytkowników zgodnych z określonymi [zasadami autoryzacji](xref:security/authorization/policies). Na przykład jeśli masz zasady autoryzacji niestandardowej o nazwie `MyAuthorizationPolicy`, upewnij się, że tylko użytkownicy pasujący do tych zasad mogą uzyskać dostęp do usługi przy użyciu następującego kodu:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

Do `[Authorize]` poszczególnych metod usługi można zastosować atrybut. Jeśli bieżący użytkownik nie jest zgodny z zasadami stosowanymi **zarówno** w metodzie, jak i klasie, do obiektu wywołującego jest zwracany błąd:

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

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uwierzytelnianie tokenu okaziciela w ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [Konfigurowanie uwierzytelniania certyfikatu klienta w ASP.NET Core](xref:security/authentication/certauth)
