---
title: gRPC integrację klienta w programie .NET Core
author: jamesnk
description: Dowiedz się, jak tworzyć klientów gRPC przy użyciu fabryki klienta.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 3042bb61367f8b9a9f3142217ad329270ab2cca5
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963682"
---
# <a name="grpc-client-factory-integration-in-net-core"></a>gRPC integrację klienta w programie .NET Core

Integracja gRPC z `HttpClientFactory` oferuje scentralizowany sposób tworzenia klientów gRPC. Może służyć jako alternatywa do [konfigurowania autonomicznych wystąpień klienta gRPC](xref:grpc/client). Integracja z fabryką jest dostępna w pakiecie NuGet [GRPC .NET. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) .

Fabryka oferuje następujące korzyści:

* Zapewnia centralną lokalizację do konfigurowania wystąpień klienta logicznego gRPC
* Zarządza okresem istnienia bazowego `HttpClientMessageHandler`
* Automatyczne propagowanie terminu ostatecznego i anulowanie w ASP.NET Core usłudze gRPC

## <a name="register-grpc-clients"></a>Rejestrowanie klientów gRPC

Aby zarejestrować klienta gRPC, Metoda rozszerzenia generycznego `AddGrpcClient` może być używana w ramach `Startup.ConfigureServices`, określając klasy klienta i adres usługi z wpisaną gRPC:

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

Typ klienta gRPC jest rejestrowany jako przejściowy z iniekcją zależności (DI). Klient może teraz zostać dodany i wykorzystany bezpośrednio w typach utworzonych przez DI. ASP.NET Core kontrolery MVC, SignalR Hub i usługi gRPC są miejsca, w których można automatycznie dodawać klientów gRPC:

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a>Konfigurowanie HttpClient

`HttpClientFactory` tworzy `HttpClient` używany przez klienta gRPC. Przy użyciu standardowych metod `HttpClientFactory` można dodać wychodzące oprogramowanie pośredniczące lub skonfigurować bazowe `HttpClientHandler` `HttpClient`:

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

Aby uzyskać więcej informacji, zobacz [Tworzenie żądań HTTP przy użyciu IHttpClientFactory](xref:fundamentals/http-requests).

## <a name="configure-channel-and-interceptors"></a>Konfigurowanie kanałów i przechwyceń

metody specyficzne dla gRPC są dostępne dla:

* Skonfiguruj kanał bazowy klienta gRPC.
* Dodaj wystąpienia `Interceptor`, których klient będzie używać podczas wykonywania wywołań gRPC.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a>Termin ostateczny i Propagacja anulowania

gRPC klientów utworzonych przez fabrykę w usłudze gRPC można skonfigurować przy użyciu `EnableCallContextPropagation()` do automatycznego propagowania terminu ostatecznego i tokenu anulowania do wywołań podrzędnych. Metoda rozszerzenia `EnableCallContextPropagation()` jest dostępna w pakiecie NuGet [GRPC. AspNetCore. Server. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) .

Propagacja kontekstu wywołania działa, odczytując termin ostateczny i token anulowania z bieżącego kontekstu żądania gRPC i automatycznie propaguje je do wywołań wychodzących wykonywanych przez klienta gRPC. Propagacja kontekstu wywołania jest doskonałym sposobem zapewnienia, że złożone, zagnieżdżone scenariusze gRPC zawsze propagują termin i anulowanie.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

Aby uzyskać więcej informacji na temat terminów i anulowania wywołania RPC, zobacz [cykl życia usługi RPC](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
