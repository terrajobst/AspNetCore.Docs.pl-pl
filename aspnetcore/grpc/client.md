---
title: Wywoływanie usług gRPC za pomocą klienta platformy .NET
author: jamesnk
description: Dowiedz się, jak wywoływać usługi gRPC Services za pomocą programu .NET gRPC Client.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 56f79b303a8d53699e8eb6156d328c0da1259416
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011137"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="bb791-103">Wywoływanie usług gRPC za pomocą klienta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="bb791-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="bb791-104">Biblioteka klienta .NET gRPC jest dostępna w pakiecie NuGet [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="bb791-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="bb791-105">W tym dokumencie wyjaśniono, jak:</span><span class="sxs-lookup"><span data-stu-id="bb791-105">This document explains how to:</span></span>

* <span data-ttu-id="bb791-106">Skonfiguruj klienta gRPC do wywoływania usług gRPC.</span><span class="sxs-lookup"><span data-stu-id="bb791-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="bb791-107">GRPC wywołania jednoargumentowe, przesyłania strumieniowego serwera, przesyłania strumieniowego klienta i dwukierunkowych metod przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="bb791-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="bb791-108">Konfigurowanie klienta gRPC</span><span class="sxs-lookup"><span data-stu-id="bb791-108">Configure gRPC client</span></span>

<span data-ttu-id="bb791-109">gRPC klienci są konkretnymi typami klientów, które są [generowane z  *\*plików. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="bb791-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="bb791-110">Konkretny klient gRPC ma metody, które są tłumaczone na usługę gRPC w  *\*pliku. proto* .</span><span class="sxs-lookup"><span data-stu-id="bb791-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="bb791-111">Klient gRPC jest tworzony na podstawie kanału.</span><span class="sxs-lookup"><span data-stu-id="bb791-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="bb791-112">Uruchom polecenie, aby utworzyć kanał, a następnie użyj kanału, aby utworzyć klienta gRPC: `GrpcChannel.ForAddress`</span><span class="sxs-lookup"><span data-stu-id="bb791-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="bb791-113">Kanał reprezentuje długotrwałe połączenie z usługą gRPC.</span><span class="sxs-lookup"><span data-stu-id="bb791-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="bb791-114">Po utworzeniu kanału jest on skonfigurowany z opcjami związanymi z wywoływaniem usługi.</span><span class="sxs-lookup"><span data-stu-id="bb791-114">When a channel is created it is configured with options related to calling a service.</span></span> <span data-ttu-id="bb791-115">Na przykład `HttpClient` używane do wykonywania wywołań, maksymalnego rozmiaru wysyłania i odbierania wiadomości oraz rejestrowania można `GrpcChannelOptions` określić i użyć z `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="bb791-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="bb791-116">Aby uzyskać pełną listę opcji, zobacz [Opcje konfiguracji klienta](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="bb791-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

<span data-ttu-id="bb791-117">Utworzenie kanału może być kosztowną operacją i ponowne użycie kanału dla wywołań gRPC oferuje korzyści z wydajności.</span><span class="sxs-lookup"><span data-stu-id="bb791-117">Creating a channel can be an expensive operation and reusing a channel for gRPC calls offers performance benefits.</span></span> <span data-ttu-id="bb791-118">Wielu konkretnych klientów gRPC można utworzyć na podstawie kanału, w tym różnych typów klientów.</span><span class="sxs-lookup"><span data-stu-id="bb791-118">Multiple concrete gRPC clients can be created from a channel, including different types of clients.</span></span> <span data-ttu-id="bb791-119">Typy konkretnych klientów gRPC są obiektami lekkimi i można je tworzyć w razie konieczności.</span><span class="sxs-lookup"><span data-stu-id="bb791-119">Concrete gRPC client types are lightweight objects and can be created when needed.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="bb791-120">`GrpcChannel.ForAddress`nie jest jedyną opcją tworzenia klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="bb791-120">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="bb791-121">Jeśli wywołujesz usługi gRPC z aplikacji ASP.NET Core, weź pod uwagę [integrację klienta gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="bb791-121">If you are calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="bb791-122">Integracja gRPC z `HttpClientFactory` programem oferuje scentralizowaną alternatywę do tworzenia klientów gRPC.</span><span class="sxs-lookup"><span data-stu-id="bb791-122">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="bb791-123">Dodatkowa konfiguracja jest wymagana do [wywołania niezabezpieczonych usług gRPC za pomocą klienta platformy .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="bb791-123">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="bb791-124">Utwórz wywołania gRPC</span><span class="sxs-lookup"><span data-stu-id="bb791-124">Make gRPC calls</span></span>

<span data-ttu-id="bb791-125">Wywołanie gRPC jest inicjowane przez wywołanie metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="bb791-125">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="bb791-126">Klient gRPC będzie obsługiwał serializację komunikatów i odnoszący się do wywołania gRPC do prawidłowej usługi.</span><span class="sxs-lookup"><span data-stu-id="bb791-126">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="bb791-127">gRPC ma różne typy metod.</span><span class="sxs-lookup"><span data-stu-id="bb791-127">gRPC has different types of methods.</span></span> <span data-ttu-id="bb791-128">Sposób użycia klienta do nawiązywania wywołania gRPC zależy od typu wywoływanej metody.</span><span class="sxs-lookup"><span data-stu-id="bb791-128">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="bb791-129">Typy metod gRPC są następujące:</span><span class="sxs-lookup"><span data-stu-id="bb791-129">The gRPC method types are:</span></span>

* <span data-ttu-id="bb791-130">Jednostk</span><span class="sxs-lookup"><span data-stu-id="bb791-130">Unary</span></span>
* <span data-ttu-id="bb791-131">Przesyłanie strumieniowe serwera</span><span class="sxs-lookup"><span data-stu-id="bb791-131">Server streaming</span></span>
* <span data-ttu-id="bb791-132">Przesyłanie strumieniowe klienta</span><span class="sxs-lookup"><span data-stu-id="bb791-132">Client streaming</span></span>
* <span data-ttu-id="bb791-133">Dwukierunkowe przesyłanie strumieniowe</span><span class="sxs-lookup"><span data-stu-id="bb791-133">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="bb791-134">Wywołanie jednoargumentowe</span><span class="sxs-lookup"><span data-stu-id="bb791-134">Unary call</span></span>

<span data-ttu-id="bb791-135">Wywołanie jednoargumentowe rozpoczyna się od klienta wysyłającego komunikat żądania.</span><span class="sxs-lookup"><span data-stu-id="bb791-135">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="bb791-136">Komunikat odpowiedzi jest zwracany po zakończeniu działania usługi.</span><span class="sxs-lookup"><span data-stu-id="bb791-136">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="bb791-137">Każda Jednoargumentowa metoda usługi w  *\*pliku. proto* powoduje dwie metody .NET na konkretnym typie klienta gRPC do wywoływania metody: Metoda asynchroniczna i Metoda blokująca.</span><span class="sxs-lookup"><span data-stu-id="bb791-137">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="bb791-138">Na `GreeterClient` przykład istnieją dwa sposoby wywoływania `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="bb791-138">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="bb791-139">`GreeterClient.SayHelloAsync`-wywołuje `Greeter.SayHello` usługę asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="bb791-139">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="bb791-140">Można oczekiwać.</span><span class="sxs-lookup"><span data-stu-id="bb791-140">Can be awaited.</span></span>
* <span data-ttu-id="bb791-141">`GreeterClient.SayHello`-wywołuje `Greeter.SayHello` usługę i bloki do momentu ukończenia.</span><span class="sxs-lookup"><span data-stu-id="bb791-141">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="bb791-142">Nie używaj w kodzie asynchronicznym.</span><span class="sxs-lookup"><span data-stu-id="bb791-142">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="bb791-143">Wywołanie przesyłania strumieniowego serwera</span><span class="sxs-lookup"><span data-stu-id="bb791-143">Server streaming call</span></span>

<span data-ttu-id="bb791-144">Wywołanie przesyłania strumieniowego serwera rozpoczyna się od klienta wysyłającego komunikat żądania.</span><span class="sxs-lookup"><span data-stu-id="bb791-144">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="bb791-145">`ResponseStream.MoveNext()`odczytuje komunikaty przesyłane strumieniowo z usługi.</span><span class="sxs-lookup"><span data-stu-id="bb791-145">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="bb791-146">Wywołanie przesyłania strumieniowego serwera jest kompletne `ResponseStream.MoveNext()` , `false`gdy zwraca.</span><span class="sxs-lookup"><span data-stu-id="bb791-146">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="bb791-147">Jeśli używasz C# 8 lub nowszej, `await foreach` składnia może być używana do odczytywania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="bb791-147">If you are using C# 8 or later then the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="bb791-148">Metoda `IAsyncStreamReader<T>.ReadAllAsync()` rozszerzająca odczytuje wszystkie komunikaty ze strumienia odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="bb791-148">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="bb791-149">Wywołanie przesyłania strumieniowego klienta</span><span class="sxs-lookup"><span data-stu-id="bb791-149">Client streaming call</span></span>

<span data-ttu-id="bb791-150">Wywołanie przesyłania strumieniowego klienta rozpoczyna się *bez* wysyłania komunikatu przez klienta.</span><span class="sxs-lookup"><span data-stu-id="bb791-150">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="bb791-151">Klient może zdecydować się na wysłanie wysyłanych `RequestStream.WriteAsync`komunikatów z programu.</span><span class="sxs-lookup"><span data-stu-id="bb791-151">The client can choose to send sends messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="bb791-152">Po zakończeniu wysyłania komunikatów `RequestStream.CompleteAsync` przez klienta należy wywołać, aby powiadomić usługę.</span><span class="sxs-lookup"><span data-stu-id="bb791-152">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="bb791-153">Wywołanie jest zakończone, gdy usługa zwróci komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bb791-153">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="bb791-154">Dwukierunkowe wywołanie przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="bb791-154">Bi-directional streaming call</span></span>

<span data-ttu-id="bb791-155">Dwukierunkowe wywołanie przesyłania strumieniowego rozpoczyna się *bez* wysyłania komunikatu przez klienta.</span><span class="sxs-lookup"><span data-stu-id="bb791-155">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="bb791-156">Klient może wysyłać wiadomości za pomocą `RequestStream.WriteAsync`polecenia.</span><span class="sxs-lookup"><span data-stu-id="bb791-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="bb791-157">Komunikaty przesyłane strumieniowo z usługi są dostępne z `ResponseStream.MoveNext()` lub `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="bb791-157">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="bb791-158">Dwukierunkowe wywołanie przesyłania strumieniowego jest kompletne, gdy `ResponseStream` nie ma więcej komunikatów.</span><span class="sxs-lookup"><span data-stu-id="bb791-158">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="bb791-159">W trakcie dwukierunkowego wywołania przesyłania strumieniowego klient i usługa mogą w dowolnym momencie wysyłać komunikaty do siebie.</span><span class="sxs-lookup"><span data-stu-id="bb791-159">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="bb791-160">Najlepsza logika klienta do współpracy z dwukierunkowym wywołaniem różni się w zależności od logiki usługi.</span><span class="sxs-lookup"><span data-stu-id="bb791-160">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb791-161">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bb791-161">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
