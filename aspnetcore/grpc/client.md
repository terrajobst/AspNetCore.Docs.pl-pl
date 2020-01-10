---
title: Wywoływanie usług gRPC za pomocą klienta platformy .NET
author: jamesnk
description: Dowiedz się, jak wywoływać usługi gRPC Services za pomocą programu .NET gRPC Client.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 1e7887388a752fb35d00e65db210c3924c6ab192
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829104"
---
# <a name="call-grpc-services-with-the-net-client"></a>Wywoływanie usług gRPC za pomocą klienta platformy .NET

Biblioteka klienta .NET gRPC jest dostępna w pakiecie NuGet [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) . W tym dokumencie wyjaśniono, jak:

* Skonfiguruj klienta gRPC do wywoływania usług gRPC.
* GRPC wywołania jednoargumentowe, przesyłania strumieniowego serwera, przesyłania strumieniowego klienta i dwukierunkowych metod przesyłania strumieniowego.

## <a name="configure-grpc-client"></a>Konfigurowanie klienta gRPC

gRPC klienci są konkretnymi typami klientów, które są [generowane na podstawie plików *\*. proto* ](xref:grpc/basics#generated-c-assets). Konkretny klient gRPC ma metody, które są tłumaczone na usługę gRPC w pliku *\*. proto* .

Klient gRPC jest tworzony na podstawie kanału. Zacznij od użycia `GrpcChannel.ForAddress`, aby utworzyć kanał, a następnie użyj kanału, aby utworzyć klienta gRPC:

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

Kanał reprezentuje długotrwałe połączenie z usługą gRPC. Po utworzeniu kanału jest on konfigurowany z opcjami związanymi z wywoływaniem usługi. Na przykład `HttpClient` używany do wykonywania wywołań, maksymalnego rozmiaru wysyłania i odbierania wiadomości oraz rejestrowania można określić dla `GrpcChannelOptions` i użyć z `GrpcChannel.ForAddress`. Aby uzyskać pełną listę opcji, zobacz [Opcje konfiguracji klienta](xref:grpc/configuration#configure-client-options).

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

Wydajność i użycie kanału i klienta:

* Tworzenie kanału może być kosztowną operacją. Użycie kanału dla wywołań gRPC zapewnia korzyści wynikające z wydajności.
* gRPC klienci są tworzone za pomocą kanałów. gRPC klienci są obiektami lekkimi i nie muszą być buforowane ani ponownie używane.
* Wielu klientów gRPC można utworzyć na podstawie kanału, w tym różnych typów klientów.
* Kanał i klienci utworzeni z kanału mogą być bezpiecznie używani przez wiele wątków.
* Klienci utworzeni z kanału mogą wykonywać wiele jednoczesnych wywołań.

`GrpcChannel.ForAddress` nie jest jedyną opcją tworzenia klienta gRPC. Jeśli wywołujesz usługi gRPC z aplikacji ASP.NET Core, weź pod uwagę [integrację klienta gRPC](xref:grpc/clientfactory). Integracja gRPC z `HttpClientFactory` oferuje scentralizowaną alternatywę do tworzenia klientów gRPC.

> [!NOTE]
> Dodatkowa konfiguracja jest wymagana do [wywołania niezabezpieczonych usług gRPC za pomocą klienta platformy .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).

## <a name="make-grpc-calls"></a>Utwórz wywołania gRPC

Wywołanie gRPC jest inicjowane przez wywołanie metody na kliencie. Klient gRPC będzie obsługiwał serializację komunikatów i odnoszący się do wywołania gRPC do prawidłowej usługi.

gRPC ma różne typy metod. Sposób użycia klienta do nawiązywania wywołania gRPC zależy od typu wywoływanej metody. Typy metod gRPC są następujące:

* Jednoargumentowy
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

Każda metoda usługi jednoargumentowej w pliku *\*. proto* powoduje dwie metody .NET na konkretnym typie klienta gRPC do wywoływania metody: Metoda asynchroniczna i Metoda blokująca. Na przykład na `GreeterClient` istnieją dwa sposoby wywoływania `SayHello`:

* `GreeterClient.SayHelloAsync` — wywołania `Greeter.SayHello` usługa asynchronicznie. Można oczekiwać.
* `GreeterClient.SayHello` — wywołania `Greeter.SayHello` usługi i bloków do momentu ukończenia. Nie używaj w kodzie asynchronicznym.

### <a name="server-streaming-call"></a>Wywołanie przesyłania strumieniowego serwera

Wywołanie przesyłania strumieniowego serwera rozpoczyna się od klienta wysyłającego komunikat żądania. `ResponseStream.MoveNext()` odczytuje komunikaty przesyłane strumieniowo z usługi. Wywołanie przesyłania strumieniowego serwera jest ukończone, gdy `ResponseStream.MoveNext()` zwraca `false`.

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

W przypadku używania C# 8 lub nowszych składni `await foreach` można używać do odczytywania wiadomości. Metoda rozszerzenia `IAsyncStreamReader<T>.ReadAllAsync()` odczytuje wszystkie komunikaty ze strumienia odpowiedzi:

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

Wywołanie przesyłania strumieniowego klienta rozpoczyna się *bez* wysyłania komunikatu przez klienta. Klient może zdecydować się na wysyłanie komunikatów przy użyciu `RequestStream.WriteAsync`. Gdy klient zakończył wysyłanie komunikatów, `RequestStream.CompleteAsync` powinien zostać wywołany w celu powiadomienia usługi. Wywołanie jest zakończone, gdy usługa zwróci komunikat odpowiedzi.

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

Dwukierunkowe wywołanie przesyłania strumieniowego rozpoczyna się *bez* wysyłania komunikatu przez klienta. Klient może zdecydować się na wysyłanie komunikatów przy użyciu `RequestStream.WriteAsync`. Komunikaty przesyłane strumieniowo z usługi są dostępne z użyciem `ResponseStream.MoveNext()` lub `ResponseStream.ReadAllAsync()`. Dwukierunkowe wywołanie przesyłania strumieniowego jest kompletne, gdy `ResponseStream` nie ma więcej komunikatów.

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
