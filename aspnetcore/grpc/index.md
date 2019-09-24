---
title: Wprowadzenie do gRPC na platformie .NET Core
author: juntaoluo
description: Dowiedz się więcej o usługach gRPC Services z serwerem Kestrel i stosem ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/20/2019
uid: grpc/index
ms.openlocfilehash: 88ceeba329ff2c7d764b7a5eabd5413da6ace765
ms.sourcegitcommit: 8a36be1bfee02eba3b07b7a86085ec25c38bae6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219122"
---
# <a name="introduction-to-grpc-on-net-core"></a><span data-ttu-id="692e4-103">Wprowadzenie do gRPC na platformie .NET Core</span><span class="sxs-lookup"><span data-stu-id="692e4-103">Introduction to gRPC on .NET Core</span></span>

<span data-ttu-id="692e4-104">Przez [John Luo](https://github.com/juntaoluo) i [Kuba Kowalski-króla](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="692e4-104">By [John Luo](https://github.com/juntaoluo) and [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="692e4-105">[gRPC](https://grpc.io/docs/guides/) to język niezależny od i środowisko zdalnego wywołania procedury (RPC) o wysokiej wydajności.</span><span class="sxs-lookup"><span data-stu-id="692e4-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span>

<span data-ttu-id="692e4-106">Główne zalety gRPC są następujące:</span><span class="sxs-lookup"><span data-stu-id="692e4-106">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="692e4-107">Nowoczesne środowisko uproszczonej usługi RPC o wysokiej wydajności.</span><span class="sxs-lookup"><span data-stu-id="692e4-107">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="692e4-108">Tworzenie aplikacji interfejsu API w pierwszej kolejności, domyślnie przy użyciu buforów protokołu, dzięki czemu można wdrażać implementacje języka niezależny od.</span><span class="sxs-lookup"><span data-stu-id="692e4-108">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="692e4-109">Narzędzia dostępne dla wielu języków do generowania serwerów i klientów z jednoznacznie określonymi typami.</span><span class="sxs-lookup"><span data-stu-id="692e4-109">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="692e4-110">Obsługuje wywołania przesyłania strumieniowego klienta, serwera i dwukierunkowego.</span><span class="sxs-lookup"><span data-stu-id="692e4-110">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="692e4-111">Zredukowanie użycia sieci przy użyciu serializacji binarnej protobuf.</span><span class="sxs-lookup"><span data-stu-id="692e4-111">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="692e4-112">Te korzyści sprawiają, że gRPC doskonale nadaje się do:</span><span class="sxs-lookup"><span data-stu-id="692e4-112">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="692e4-113">Lekkie mikrousługi, w których wydajność jest krytyczna.</span><span class="sxs-lookup"><span data-stu-id="692e4-113">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="692e4-114">Systemy Polyglot, w których wiele języków jest wymaganych do programowania.</span><span class="sxs-lookup"><span data-stu-id="692e4-114">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="692e4-115">Usługi w czasie rzeczywistym, które muszą obsługiwać żądania przesyłania strumieniowego lub odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="692e4-115">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="692e4-116">C#Obsługa narzędzi dla plików. proto</span><span class="sxs-lookup"><span data-stu-id="692e4-116">C# Tooling support for .proto files</span></span>

<span data-ttu-id="692e4-117">gRPC używa podejścia pierwszego kontraktu do programowania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="692e4-117">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="692e4-118">Usługi i komunikaty są zdefiniowane w  *\*plikach. proto* :</span><span class="sxs-lookup"><span data-stu-id="692e4-118">Services and messages are defined in *\*.proto* files:</span></span>

```protobuf
syntax = "proto3";

service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply);
}

message HelloRequest {
  string name = 1;
}

message HelloReply {
  string message = 1;
}
```

<span data-ttu-id="692e4-119">Typy .NET dla usług, klientów i komunikatów są generowane automatycznie przez dołączenie  *\*plików PROTO* w projekcie:</span><span class="sxs-lookup"><span data-stu-id="692e4-119">.NET types for services, clients and messages are automatically generated by including *\*.proto* files in a project:</span></span>

* <span data-ttu-id="692e4-120">Dodaj odwołanie do pakietu do pakietu [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="692e4-120">Add a package reference to [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) package.</span></span>
* <span data-ttu-id="692e4-121">`<Protobuf>` Dodaj  *\*pliki. proto* do grupy elementów.</span><span class="sxs-lookup"><span data-stu-id="692e4-121">Add *\*.proto* files to the `<Protobuf>` item group.</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" />
</ItemGroup>
```

<span data-ttu-id="692e4-122">Więcej informacji o obsłudze narzędzi gRPC można znaleźć w <xref:grpc/basics>temacie.</span><span class="sxs-lookup"><span data-stu-id="692e4-122">For more information on gRPC tooling support, see <xref:grpc/basics>.</span></span>

## <a name="grpc-services-on-aspnet-core"></a><span data-ttu-id="692e4-123">usługi gRPC Services na ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="692e4-123">gRPC services on ASP.NET Core</span></span>

<span data-ttu-id="692e4-124">usługi gRPC Services mogą być hostowane na ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="692e4-124">gRPC services can be hosted on ASP.NET Core.</span></span> <span data-ttu-id="692e4-125">Usługi mają pełną integrację z popularnymi funkcjami ASP.NET Core, takimi jak rejestrowanie, iniekcja zależności (DI), uwierzytelnianie i autoryzacja.</span><span class="sxs-lookup"><span data-stu-id="692e4-125">Services have full integration with popular ASP.NET Core features such as logging, dependency injection (DI), authentication and authorization.</span></span>

<span data-ttu-id="692e4-126">Szablon projektu usługi gRPC to usługa początkowa:</span><span class="sxs-lookup"><span data-stu-id="692e4-126">The gRPC service project template provides a starter service:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    private readonly ILogger<GreeterService> _logger;

    public GreeterService(ILogger<GreeterService> logger)
    {
        _logger = logger;
    }

    public override Task<HelloReply> SayHello(HelloRequest request,
        ServerCallContext context)
    {
        _logger.LogInformation("Saying hello to {Name}", request.Name);
        return Task.FromResult(new HelloReply 
        {
            Message = "Hello " + request.Name
        });
    }
}
```

<span data-ttu-id="692e4-127">`GreeterService`dziedziczy z `GreeterBase` typu, który jest generowany na `Greeter` podstawie usługi w  *\*pliku. proto* .</span><span class="sxs-lookup"><span data-stu-id="692e4-127">`GreeterService` inherits from the `GreeterBase` type, which is generated from the `Greeter` service in the *\*.proto* file.</span></span> <span data-ttu-id="692e4-128">Usługa jest dostępna dla klientów w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="692e4-128">The service is made accessible to clients in *Startup.cs*:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapGrpcService<GreeterService>();
});
```

<span data-ttu-id="692e4-129">Aby dowiedzieć się więcej na temat usług gRPC Services <xref:grpc/aspnetcore>na ASP.NET Core, zobacz.</span><span class="sxs-lookup"><span data-stu-id="692e4-129">To learn more about gRPC services on ASP.NET Core, see <xref:grpc/aspnetcore>.</span></span>

## <a name="call-grpc-services-with-a-net-client"></a><span data-ttu-id="692e4-130">Wywoływanie usług gRPC za pomocą klienta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="692e4-130">Call gRPC services with a .NET client</span></span>

<span data-ttu-id="692e4-131">gRPC klienci są konkretnymi typami klientów, które są [generowane z  *\*plików. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="692e4-131">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="692e4-132">Konkretny klient gRPC ma metody, które są tłumaczone na usługę gRPC w  *\*pliku. proto* .</span><span class="sxs-lookup"><span data-stu-id="692e4-132">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greeter.GreeterClient(channel);

var response = await client.SayHello(
    new HelloRequest { Name = "World" });

Console.WriteLine(response.Message);
```

<span data-ttu-id="692e4-133">Klient gRPC jest tworzony przy użyciu kanału, który reprezentuje długotrwałe połączenie z usługą gRPC.</span><span class="sxs-lookup"><span data-stu-id="692e4-133">A gRPC client is created using a channel, which represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="692e4-134">Kanał można utworzyć przy użyciu `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="692e4-134">A channel can be created using `GrpcChannel.ForAddress`.</span></span>

<span data-ttu-id="692e4-135">Aby uzyskać więcej informacji na temat tworzenia klientów i wywoływania różnych metod usługi, <xref:grpc/client>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="692e4-135">For more information on creating clients, and calling different service methods, see <xref:grpc/client>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="692e4-136">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="692e4-136">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/clientfactory>
* <xref:tutorials/grpc/grpc-start>
