---
title: Wywoływanie usług gRPC za pomocą klienta platformy .NET
author: jamesnk
description: Dowiedz się, jak wywoływać usługi gRPC Services za pomocą programu .NET gRPC Client.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 27e4b3e7307ae49bacb01a46fbc1b55b6967c7a0
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773692"
---
# <a name="call-grpc-services-with-the-net-client"></a>Wywoływanie usług gRPC za pomocą klienta platformy .NET

Biblioteka klienta .NET gRPC jest dostępna w pakiecie NuGet [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) . W tym dokumencie wyjaśniono, jak:

* Skonfiguruj klienta gRPC do wywoływania usług gRPC.
* GRPC wywołania jednoargumentowe, przesyłania strumieniowego serwera, przesyłania strumieniowego klienta i dwukierunkowych metod przesyłania strumieniowego.

## <a name="configure-grpc-client"></a>Konfigurowanie klienta gRPC

gRPC klienci są konkretnymi typami klientów, które są [generowane z  *\*plików. proto* ](xref:grpc/basics#generated-c-assets). Konkretny klient gRPC ma metody, które są tłumaczone na usługę gRPC w  *\*pliku. proto* .

Klient gRPC jest tworzony na podstawie kanału. Uruchom polecenie, aby utworzyć kanał, a następnie użyj kanału, aby utworzyć klienta gRPC: `GrpcChannel.ForAddress`

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

Kanał reprezentuje długotrwałe połączenie z usługą gRPC. Po utworzeniu kanału jest on skonfigurowany z opcjami związanymi z wywoływaniem usługi. Na przykład `HttpClient` używane do wykonywania wywołań, maksymalnego rozmiaru wysyłania i odbierania wiadomości oraz rejestrowania można `GrpcChannelOptions` określić i użyć z `GrpcChannel.ForAddress`. Aby uzyskać pełną listę opcji, zobacz [Opcje konfiguracji klienta](xref:grpc/configuration#configure-client-options).

Utworzenie kanału może być kosztowną operacją i ponowne użycie kanału dla wywołań gRPC oferuje korzyści z wydajności. Wielu konkretnych klientów gRPC można utworzyć na podstawie kanału, w tym różnych typów klientów. Typy konkretnych klientów gRPC są obiektami lekkimi i można je tworzyć w razie konieczności.

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

`GrpcChannel.ForAddress`nie jest jedyną opcją tworzenia klienta gRPC. Jeśli wywołujesz usługi gRPC z aplikacji ASP.NET Core, weź pod uwagę [integrację klienta gRPC](xref:grpc/clientfactory). Integracja gRPC z `HttpClientFactory` programem oferuje scentralizowaną alternatywę do tworzenia klientów gRPC.

## <a name="make-grpc-calls"></a>Utwórz wywołania gRPC

Wywołanie gRPC jest inicjowane przez wywołanie metody na kliencie. Klient gRPC będzie obsługiwał serializację komunikatów i odnoszący się do wywołania gRPC do prawidłowej usługi.

gRPC ma różne typy metod. Sposób użycia klienta do nawiązywania wywołania gRPC zależy od typu wywoływanej metody. Typy metod gRPC są następujące:

* Jednostk
* Przesyłanie strumieniowe serwera
* Przesyłanie strumieniowe klienta
* Dwukierunkowe przesyłanie strumieniowe

### <a name="unary-call"></a>Wywołanie jednoargumentowe

Wywołanie jednoargumentowe rozpoczyna się od klienta wysyłającego komunikat żądania. Komunikat odpowiedzi jest zwracany po zakończeniu działania usługi.

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

Każda Jednoargumentowa metoda usługi w  *\*pliku. proto* powoduje dwie metody .NET na konkretnym typie klienta gRPC do wywoływania metody: Metoda asynchroniczna i Metoda blokująca. Na `GreeterClient` przykład istnieją dwa sposoby wywoływania `SayHello`:

* `GreeterClient.SayHelloAsync`-wywołuje `Greeter.SayHello` usługę asynchronicznie. Można oczekiwać.
* `GreeterClient.SayHello`-wywołuje `Greeter.SayHello` usługę i bloki do momentu ukończenia. Nie używaj w kodzie asynchronicznym.

### <a name="server-streaming-call"></a>Wywołanie przesyłania strumieniowego serwera

Wywołanie przesyłania strumieniowego serwera rozpoczyna się od klienta wysyłającego komunikat żądania. `ResponseStream.MoveNext()`odczytuje komunikaty przesyłane strumieniowo z usługi. Wywołanie przesyłania strumieniowego serwera jest kompletne `ResponseStream.MoveNext()` , `false`gdy zwraca.

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

Jeśli używasz C# 8 lub nowszej, `await foreach` składnia może być używana do odczytywania wiadomości. Metoda `IAsyncStreamReader<T>.ReadAllAsync()` rozszerzająca odczytuje wszystkie komunikaty ze strumienia odpowiedzi:

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

### <a name="client-streaming-call"></a>Wywołanie przesyłania strumieniowego klienta

Wywołanie przesyłania strumieniowego klienta rozpoczyna się *bez* wysyłania komunikatu przez klienta. Klient może zdecydować się na wysłanie wysyłanych `RequestStream.WriteAsync`komunikatów z programu. Po zakończeniu wysyłania komunikatów `RequestStream.CompleteAsync` przez klienta należy wywołać, aby powiadomić usługę. Wywołanie jest zakończone, gdy usługa zwróci komunikat odpowiedzi.

```csharp
var client = new Counter.CounterClient(channel);
using (var call = client.AccumulateCount())
{
    for (var i = 0; i < 3; i++)
    {
        await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
    }
    await call.RequestStream.CompleteAsync();

    var response = await call;
    Console.WriteLine($"Count: {response.Count}");
    // Count: 3
}
```

### <a name="bi-directional-streaming-call"></a>Dwukierunkowe wywołanie przesyłania strumieniowego

Dwukierunkowe wywołanie przesyłania strumieniowego rozpoczyna się *bez* wysyłania komunikatu przez klienta. Klient może wysyłać wiadomości za pomocą `RequestStream.WriteAsync`polecenia. Komunikaty przesyłane strumieniowo z usługi są dostępne z `ResponseStream.MoveNext()` lub `ResponseStream.ReadAllAsync()`. Dwukierunkowe wywołanie przesyłania strumieniowego jest kompletne, gdy `ResponseStream` nie ma więcej komunikatów.

```csharp
using (var call = client.Echo())
{
    Console.WriteLine("Starting background task to receive messages");
    var readTask = Task.Run(async () =>
    {
        await foreach (var response in call.ResponseStream.ReadAllAsync())
        {
            Console.WriteLine(response.Message);
            // Echo messages sent to the service
        }
    });

    Console.WriteLine("Starting to send messages");
    Console.WriteLine("Type a message to echo then press enter.");
    while (true)
    {
        var result = Console.ReadLine();
        if (string.IsNullOrEmpty(result))
        {
            break;
        }

        await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
    }

    Console.WriteLine("Disconnecting");
    await call.RequestStream.CompleteAsync();
    await readTask;
}
```

W trakcie dwukierunkowego wywołania przesyłania strumieniowego klient i usługa mogą w dowolnym momencie wysyłać komunikaty do siebie. Najlepsza logika klienta do współpracy z dwukierunkowym wywołaniem różni się w zależności od logiki usługi.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
