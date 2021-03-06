---
title: Wprowadzenie do gRPC na platformie .NET Core
author: juntaoluo
description: Dowiedz się więcej o usługach gRPC Services z serwerem Kestrel i stosem ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/20/2019
uid: grpc/index
ms.openlocfilehash: d97eea1da28424680a3cfa38102637b1e20ff661
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667315"
---
# <a name="introduction-to-grpc-on-net-core"></a>Wprowadzenie do gRPC na platformie .NET Core

Przez [John Luo](https://github.com/juntaoluo) i [Kuba Kowalski-króla](https://twitter.com/jamesnk)

[gRPC](https://grpc.io/docs/guides/) to język niezależny od i środowisko zdalnego wywołania procedury (RPC) o wysokiej wydajności.

Główne zalety gRPC są następujące:
* Nowoczesne, wysoce wydajne i uproszczone środowisko RPC.
* Tworzenie aplikacji interfejsu API w pierwszej kolejności, domyślnie przy użyciu buforów protokołu, dzięki czemu można wdrażać implementacje języka niezależny od.
* Narzędzia dostępne dla wielu języków do generowania serwerów i klientów z jednoznacznie określonymi typami.
* Obsługuje wywołania przesyłania strumieniowego klienta, serwera i dwukierunkowego.
* Zredukowanie użycia sieci przy użyciu serializacji binarnej protobuf.

Te korzyści sprawiają, że gRPC doskonale nadaje się do:
* Lekkie mikrousługi, w których wydajność jest krytyczna.
* Systemy Polyglot, w których wiele języków jest wymaganych do programowania.
* Usługi w czasie rzeczywistym, które muszą obsługiwać żądania przesyłania strumieniowego lub odpowiedzi.

## <a name="c-tooling-support-for-proto-files"></a>C#Obsługa narzędzi dla plików. proto

gRPC używa podejścia pierwszego kontraktu do programowania interfejsu API. Usługi i komunikaty są zdefiniowane w plikach *\*. proto* :

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

Typy .NET dla usług, klientów i komunikatów są generowane automatycznie przez dołączenie plików *\*. proto* w projekcie:

* Dodaj odwołanie do pakietu do pakietu [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) .
* Dodaj pliki *\*. proto* do grupy `<Protobuf>` elementów.

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" />
</ItemGroup>
```

Aby uzyskać więcej informacji na temat pomocy technicznej dotyczącej narzędzi gRPC, zobacz <xref:grpc/basics>.

## <a name="grpc-services-on-aspnet-core"></a>usługi gRPC Services na ASP.NET Core

usługi gRPC Services mogą być hostowane na ASP.NET Core. Usługi mają pełną integrację z popularnymi funkcjami ASP.NET Core, takimi jak rejestrowanie, iniekcja zależności (DI), uwierzytelnianie i autoryzacja.

Szablon projektu usługi gRPC to usługa początkowa:

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

`GreeterService` dziedziczy z typu `GreeterBase`, który jest generowany przez usługę `Greeter` w pliku *\*. proto* . Usługa jest dostępna dla klientów w *Startup.cs*:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapGrpcService<GreeterService>();
});
```

Aby dowiedzieć się więcej na temat usług gRPC Services na ASP.NET Core, zobacz <xref:grpc/aspnetcore>.

## <a name="call-grpc-services-with-a-net-client"></a>Wywoływanie usług gRPC za pomocą klienta platformy .NET

gRPC klienci są konkretnymi typami klientów, które są [generowane na podstawie plików *\*. proto* ](xref:grpc/basics#generated-c-assets). Konkretny klient gRPC ma metody, które są tłumaczone na usługę gRPC w pliku *\*. proto* .

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greeter.GreeterClient(channel);

var response = await client.SayHelloAsync(
    new HelloRequest { Name = "World" });

Console.WriteLine(response.Message);
```

Klient gRPC jest tworzony przy użyciu kanału, który reprezentuje długotrwałe połączenie z usługą gRPC. Kanał można utworzyć przy użyciu `GrpcChannel.ForAddress`.

Aby uzyskać więcej informacji na temat tworzenia klientów i wywoływania różnych metod usługi, zobacz <xref:grpc/client>.

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/clientfactory>
* <xref:tutorials/grpc/grpc-start>
