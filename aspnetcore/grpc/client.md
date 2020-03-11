---
title: Wywoływanie usług gRPC za pomocą klienta platformy .NET
author: jamesnk
description: Dowiedz się, jak wywoływać usługi gRPC Services za pomocą programu .NET gRPC Client.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 6a6a649f7194354b16f3d67160be02428cc01170
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667175"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="ef9c2-103">Wywoływanie usług gRPC za pomocą klienta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="ef9c2-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="ef9c2-104">Biblioteka klienta .NET gRPC jest dostępna w pakiecie NuGet [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="ef9c2-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="ef9c2-105">W tym dokumencie wyjaśniono, jak:</span><span class="sxs-lookup"><span data-stu-id="ef9c2-105">This document explains how to:</span></span>

* <span data-ttu-id="ef9c2-106">Skonfiguruj klienta gRPC do wywoływania usług gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="ef9c2-107">GRPC wywołania jednoargumentowe, przesyłania strumieniowego serwera, przesyłania strumieniowego klienta i dwukierunkowych metod przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="ef9c2-108">Konfigurowanie klienta gRPC</span><span class="sxs-lookup"><span data-stu-id="ef9c2-108">Configure gRPC client</span></span>

<span data-ttu-id="ef9c2-109">gRPC klienci są konkretnymi typami klientów, które są [generowane na podstawie plików *\*. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="ef9c2-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="ef9c2-110">Konkretny klient gRPC ma metody, które są tłumaczone na usługę gRPC w pliku *\*. proto* .</span><span class="sxs-lookup"><span data-stu-id="ef9c2-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="ef9c2-111">Klient gRPC jest tworzony na podstawie kanału.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="ef9c2-112">Zacznij od użycia `GrpcChannel.ForAddress`, aby utworzyć kanał, a następnie użyj kanału, aby utworzyć klienta gRPC:</span><span class="sxs-lookup"><span data-stu-id="ef9c2-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="ef9c2-113">Kanał reprezentuje długotrwałe połączenie z usługą gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="ef9c2-114">Po utworzeniu kanału jest on konfigurowany z opcjami związanymi z wywoływaniem usługi.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="ef9c2-115">Na przykład `HttpClient` używany do wykonywania wywołań, maksymalnego rozmiaru wysyłania i odbierania wiadomości oraz rejestrowania można określić dla `GrpcChannelOptions` i użyć z `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="ef9c2-116">Aby uzyskać pełną listę opcji, zobacz [Opcje konfiguracji klienta](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="ef9c2-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="ef9c2-117">Wydajność i użycie kanału i klienta:</span><span class="sxs-lookup"><span data-stu-id="ef9c2-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="ef9c2-118">Tworzenie kanału może być kosztowną operacją.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="ef9c2-119">Użycie kanału dla wywołań gRPC zapewnia korzyści wynikające z wydajności.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="ef9c2-120">gRPC klienci są tworzone za pomocą kanałów.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="ef9c2-121">gRPC klienci są obiektami lekkimi i nie muszą być buforowane ani ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="ef9c2-122">Wielu klientów gRPC można utworzyć na podstawie kanału, w tym różnych typów klientów.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="ef9c2-123">Kanał i klienci utworzeni z kanału mogą być bezpiecznie używani przez wiele wątków.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="ef9c2-124">Klienci utworzeni z kanału mogą wykonywać wiele jednoczesnych wywołań.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="ef9c2-125">`GrpcChannel.ForAddress` nie jest jedyną opcją tworzenia klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="ef9c2-126">Jeśli wywołujesz usługi gRPC z aplikacji ASP.NET Core, weź pod uwagę [integrację klienta gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="ef9c2-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="ef9c2-127">Integracja gRPC z `HttpClientFactory` oferuje scentralizowaną alternatywę do tworzenia klientów gRPC.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="ef9c2-128">Dodatkowa konfiguracja jest wymagana do [wywołania niezabezpieczonych usług gRPC za pomocą klienta platformy .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="ef9c2-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="ef9c2-129">Wywołanie gRPC za pośrednictwem protokołu HTTP/2 z `Grpc.Net.Client` nie jest obecnie obsługiwane w oprogramowaniu Xamarin.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="ef9c2-130">Pracujemy nad ulepszeniem obsługi protokołu HTTP/2 w przyszłej wersji platformy Xamarin.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="ef9c2-131">[GRPC. Core](https://www.nuget.org/packages/Grpc.Core) i [GRPC-Web](xref:grpc/browser) to żywotne alternatywy, które działają dzisiaj.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="ef9c2-132">Utwórz wywołania gRPC</span><span class="sxs-lookup"><span data-stu-id="ef9c2-132">Make gRPC calls</span></span>

<span data-ttu-id="ef9c2-133">Wywołanie gRPC jest inicjowane przez wywołanie metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="ef9c2-134">Klient gRPC będzie obsługiwał serializację komunikatów i odnoszący się do wywołania gRPC do prawidłowej usługi.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="ef9c2-135">gRPC ma różne typy metod.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-135">gRPC has different types of methods.</span></span> <span data-ttu-id="ef9c2-136">Sposób użycia klienta do nawiązywania wywołania gRPC zależy od typu wywoływanej metody.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-136">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="ef9c2-137">Typy metod gRPC są następujące:</span><span class="sxs-lookup"><span data-stu-id="ef9c2-137">The gRPC method types are:</span></span>

* <span data-ttu-id="ef9c2-138">Jednostk</span><span class="sxs-lookup"><span data-stu-id="ef9c2-138">Unary</span></span>
* <span data-ttu-id="ef9c2-139">Przesyłanie strumieniowe serwera</span><span class="sxs-lookup"><span data-stu-id="ef9c2-139">Server streaming</span></span>
* <span data-ttu-id="ef9c2-140">Przesyłanie strumieniowe klienta</span><span class="sxs-lookup"><span data-stu-id="ef9c2-140">Client streaming</span></span>
* <span data-ttu-id="ef9c2-141">Dwukierunkowe przesyłanie strumieniowe</span><span class="sxs-lookup"><span data-stu-id="ef9c2-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="ef9c2-142">Wywołanie jednoargumentowe</span><span class="sxs-lookup"><span data-stu-id="ef9c2-142">Unary call</span></span>

<span data-ttu-id="ef9c2-143">Wywołanie jednoargumentowe rozpoczyna się od klienta wysyłającego komunikat żądania.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="ef9c2-144">Komunikat odpowiedzi jest zwracany po zakończeniu działania usługi.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="ef9c2-145">Każda metoda usługi jednoargumentowej w pliku *\*. proto* powoduje dwie metody .NET na konkretnym typie klienta gRPC do wywoływania metody: Metoda asynchroniczna i Metoda blokująca.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="ef9c2-146">Na przykład na `GreeterClient` istnieją dwa sposoby wywoływania `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="ef9c2-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="ef9c2-147">`GreeterClient.SayHelloAsync` — wywołania `Greeter.SayHello` usługa asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="ef9c2-148">Można oczekiwać.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-148">Can be awaited.</span></span>
* <span data-ttu-id="ef9c2-149">`GreeterClient.SayHello` — wywołania `Greeter.SayHello` usługi i bloków do momentu ukończenia.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="ef9c2-150">Nie używaj w kodzie asynchronicznym.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="ef9c2-151">Wywołanie przesyłania strumieniowego serwera</span><span class="sxs-lookup"><span data-stu-id="ef9c2-151">Server streaming call</span></span>

<span data-ttu-id="ef9c2-152">Wywołanie przesyłania strumieniowego serwera rozpoczyna się od klienta wysyłającego komunikat żądania.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="ef9c2-153">`ResponseStream.MoveNext()` odczytuje komunikaty przesyłane strumieniowo z usługi.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="ef9c2-154">Wywołanie przesyłania strumieniowego serwera jest ukończone, gdy `ResponseStream.MoveNext()` zwraca `false`.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="ef9c2-155">W przypadku używania C# 8 lub nowszych składni `await foreach` można używać do odczytywania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-155">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="ef9c2-156">Metoda rozszerzenia `IAsyncStreamReader<T>.ReadAllAsync()` odczytuje wszystkie komunikaty ze strumienia odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="ef9c2-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="ef9c2-157">Wywołanie przesyłania strumieniowego klienta</span><span class="sxs-lookup"><span data-stu-id="ef9c2-157">Client streaming call</span></span>

<span data-ttu-id="ef9c2-158">Wywołanie przesyłania strumieniowego klienta rozpoczyna się *bez* wysyłania komunikatu przez klienta.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="ef9c2-159">Klient może zdecydować się na wysyłanie komunikatów przy użyciu `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="ef9c2-160">Gdy klient zakończył wysyłanie komunikatów, `RequestStream.CompleteAsync` powinien zostać wywołany w celu powiadomienia usługi.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-160">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="ef9c2-161">Wywołanie jest zakończone, gdy usługa zwróci komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-161">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="ef9c2-162">Dwukierunkowe wywołanie przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="ef9c2-162">Bi-directional streaming call</span></span>

<span data-ttu-id="ef9c2-163">Dwukierunkowe wywołanie przesyłania strumieniowego rozpoczyna się *bez* wysyłania komunikatu przez klienta.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="ef9c2-164">Klient może zdecydować się na wysyłanie komunikatów przy użyciu `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="ef9c2-165">Komunikaty przesyłane strumieniowo z usługi są dostępne z użyciem `ResponseStream.MoveNext()` lub `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="ef9c2-166">Dwukierunkowe wywołanie przesyłania strumieniowego jest kompletne, gdy `ResponseStream` nie ma więcej komunikatów.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="ef9c2-167">W trakcie dwukierunkowego wywołania przesyłania strumieniowego klient i usługa mogą w dowolnym momencie wysyłać komunikaty do siebie.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="ef9c2-168">Najlepsza logika klienta do współpracy z dwukierunkowym wywołaniem różni się w zależności od logiki usługi.</span><span class="sxs-lookup"><span data-stu-id="ef9c2-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef9c2-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ef9c2-169">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
