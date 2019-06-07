---
title: Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się, jak przesłać strumień danych między klientem a serwerem.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/05/2019
uid: signalr/streaming
ms.openlocfilehash: a75156f398e113393ddb891d16eec3f09de80c09
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750185"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="ce500-103">Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce500-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="ce500-104">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="ce500-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce500-105">SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego od klienta do serwera i z serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="ce500-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="ce500-106">Jest to przydatne w scenariuszach, gdzie fragmenty danych pojawić się wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="ce500-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="ce500-107">Podczas przesyłania strumieniowego, poszczególne fragmenty są wysyłane do klienta lub serwera tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.</span><span class="sxs-lookup"><span data-stu-id="ce500-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce500-108">SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracane metod serwera.</span><span class="sxs-lookup"><span data-stu-id="ce500-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="ce500-109">Jest to przydatne w scenariuszach, gdzie fragmenty danych pojawić się wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="ce500-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="ce500-110">Gdy wartość zwracana jest przesyłany strumieniowo do klienta, poszczególne fragmenty są wysyłane do klienta tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.</span><span class="sxs-lookup"><span data-stu-id="ce500-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="ce500-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce500-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="ce500-112">Konfigurowanie Centrum do przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="ce500-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce500-113">Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="ce500-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce500-114">Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca <xref:System.Threading.Channels.ChannelReader%601> lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="ce500-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="ce500-115">Przesyłanie strumieniowe serwera do klienta</span><span class="sxs-lookup"><span data-stu-id="ce500-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce500-116">Przesyłanie strumieniowe metod koncentratora może zwrócić `IAsyncEnumerable<T>` oprócz `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="ce500-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="ce500-117">Najprostszym sposobem, aby zwrócić `IAsyncEnumerable<T>` energii jest upewnienie metody iteratora asynchronicznej metody koncentratora, tak jak pokazano w następującym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="ce500-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="ce500-118">Metody iteratora asynchroniczne Centrum może akceptować `CancellationToken` parametr, który jest wyzwalany, gdy klient anuluje subskrypcje ze strumienia.</span><span class="sxs-lookup"><span data-stu-id="ce500-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="ce500-119">Asynchroniczne metody iteracyjne problemom, typowe kanałów, takich jak nie zwraca `ChannelReader` wystarczająco wczesne tryb konserwacji lub wychodzenia z metodą przerywając <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="ce500-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="ce500-120">Poniższy przykład pokazuje podstawy przesyłanie strumieniowe danych do klienta za pomocą kanałów.</span><span class="sxs-lookup"><span data-stu-id="ce500-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="ce500-121">Zawsze, gdy obiekt jest zapisywany <xref:System.Threading.Channels.ChannelWriter%601>, natychmiast wysyła obiekt do klienta.</span><span class="sxs-lookup"><span data-stu-id="ce500-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="ce500-122">Na koniec `ChannelWriter` zakończeniu to sprawdzić klientowi strumień jest zamknięty.</span><span class="sxs-lookup"><span data-stu-id="ce500-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="ce500-123">Zapisać `ChannelWriter<T>` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="ce500-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="ce500-124">Inne wywołania koncentratora są blokowane, aż do `ChannelReader` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="ce500-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="ce500-125">OPAKOWYWANIE logiki `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="ce500-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="ce500-126">Wykonaj `Channel` w `catch` i na zewnątrz `catch` się upewnić się, że Centrum wywołania metody jest wykonany prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="ce500-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Streaming hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/samples/2.2/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/samples/2.1/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ce500-127">Można zaakceptować przesyłania strumieniowego metod koncentratora klienta serwera `CancellationToken` parametr, który jest wyzwalany, gdy klient anuluje subskrypcje ze strumienia.</span><span class="sxs-lookup"><span data-stu-id="ce500-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="ce500-128">Używanie tego tokenu, aby zatrzymać działanie serwera i zwolnić wszystkie zasoby, jeśli klient odłączy się do końca strumienia.</span><span class="sxs-lookup"><span data-stu-id="ce500-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="ce500-129">Klient serwer przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="ce500-129">Client-to-server streaming</span></span>

<span data-ttu-id="ce500-130">Metody koncentratora automatycznie wybrana zostaje pierwsza klient serwer przesyłania strumieniowego metody koncentratora po przyjmuje jeden lub więcej obiektów typu <xref:System.Threading.Channels.ChannelReader%601> lub <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span><span class="sxs-lookup"><span data-stu-id="ce500-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="ce500-131">Poniższy przykład pokazuje podstawy odczytywanie danych przesyłanych strumieniowo wysłanych z klienta.</span><span class="sxs-lookup"><span data-stu-id="ce500-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="ce500-132">Zawsze, gdy klient zapisuje <xref:System.Threading.Channels.ChannelWriter%601>, dane są zapisywane do `ChannelReader` na serwerze, z którego odczytuje metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ce500-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="ce500-133"><xref:System.Collections.Generic.IAsyncEnumerable%601> Zgodna wersja metody.</span><span class="sxs-lookup"><span data-stu-id="ce500-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<Stream> stream) 
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="ce500-134">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="ce500-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="ce500-135">Przesyłanie strumieniowe serwera do klienta</span><span class="sxs-lookup"><span data-stu-id="ce500-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce500-136">`StreamAsync` i `StreamAsChannelAsync` metod `HubConnection` są używane do wywołania metody przesyłania strumieniowego serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="ce500-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="ce500-137">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `StreamAsync` lub `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="ce500-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="ce500-138">Parametr generyczny na `StreamAsync<T>` i `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="ce500-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="ce500-139">Obiekt typu `IAsyncEnumerable<T>` lub `ChannelReader<T>` jest zwracany z wywołania strumienia i reprezentuje strumienia na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="ce500-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="ce500-140">A `StreamAsync` przykładu, który zwraca `IAsyncEnumerable<int>`:</span><span class="sxs-lookup"><span data-stu-id="ce500-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var stream = await hubConnection.StreamAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

await foreach (var count in stream)
{
    Console.WriteLine($"{count}");
}

Console.WriteLine("Streaming completed");
```

<span data-ttu-id="ce500-141">Odpowiedni `StreamAsChannelAsync` przykładu, który zwraca `ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="ce500-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ce500-142">`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="ce500-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="ce500-143">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="ce500-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="ce500-144">Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="ce500-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="ce500-145">Element `ChannelReader<T>` jest zwracany z wywołania strumienia i reprezentuje strumienia na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="ce500-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="ce500-146">`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="ce500-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="ce500-147">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="ce500-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="ce500-148">Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="ce500-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="ce500-149">Element `ChannelReader<T>` jest zwracany z wywołania strumienia i reprezentuje strumienia na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="ce500-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="ce500-150">Klient serwer przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="ce500-150">Client-to-server streaming</span></span>

<span data-ttu-id="ce500-151">Istnieją dwa sposoby, aby wywołać przesyłania strumieniowego metody koncentratora klienta z serwerem z klienta .NET.</span><span class="sxs-lookup"><span data-stu-id="ce500-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="ce500-152">Możesz albo Przekaż `IAsyncEnumerable<T>` lub `ChannelReader` jako argument do `SendAsync`, `InvokeAsync`, lub `StreamAsChannelAsync`, w zależności od wywoływane metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ce500-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="ce500-153">Zawsze, gdy dane są zapisywane do `IAsyncEnumerable` lub `ChannelWriter` obiektu metody koncentratora na serwerze odbiera nowy element z danymi od klienta.</span><span class="sxs-lookup"><span data-stu-id="ce500-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="ce500-154">Jeśli przy użyciu `IAsyncEnumerable` obiektu strumienia, który kończy się po metoda zwracania strumienia elementy wyjść.</span><span class="sxs-lookup"><span data-stu-id="ce500-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
    //After the for loop has completed and the local function exits the stream completion will be sent.
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="ce500-155">Czy używasz `ChannelWriter`, wykonaniu kanale przy użyciu `channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="ce500-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="ce500-156">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="ce500-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="ce500-157">Przesyłanie strumieniowe serwera do klienta</span><span class="sxs-lookup"><span data-stu-id="ce500-157">Server-to-client streaming</span></span>

<span data-ttu-id="ce500-158">Klientów JavaScript wywoływać metody przesyłania strumieniowego serwera do klienta dla centrów z `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="ce500-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="ce500-159">`stream` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="ce500-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="ce500-160">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ce500-160">The name of the hub method.</span></span> <span data-ttu-id="ce500-161">W poniższym przykładzie nazwa metody koncentratora jest `Counter`.</span><span class="sxs-lookup"><span data-stu-id="ce500-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="ce500-162">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ce500-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="ce500-163">W poniższym przykładzie argumenty są licznik liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.</span><span class="sxs-lookup"><span data-stu-id="ce500-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="ce500-164">`connection.stream` Zwraca `IStreamResult`, który zawiera `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="ce500-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="ce500-165">Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołań zwrotnych, aby otrzymywać powiadomienia z `stream` wywołania.</span><span class="sxs-lookup"><span data-stu-id="ce500-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="ce500-166">Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="ce500-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="ce500-167">Wywołanie tej metody powoduje unieważnienie `CancellationToken` parametru metody koncentratora, jeśli została podana.</span><span class="sxs-lookup"><span data-stu-id="ce500-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="ce500-168">Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="ce500-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="ce500-169">Klient serwer przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="ce500-169">Client-to-server streaming</span></span>

<span data-ttu-id="ce500-170">Klientów języka JavaScript wywołania klient serwer przesyłania strumieniowego metod koncentratorów, przekazując `Subject` jako argument do `send`, `invoke`, lub `stream`, w zależności od wywoływane metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ce500-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="ce500-171">`Subject` Jest klasa, która wygląda podobnie `Subject`.</span><span class="sxs-lookup"><span data-stu-id="ce500-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="ce500-172">Na przykład w RxJS, można użyć [podmiotu](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) klasy w bibliotece.</span><span class="sxs-lookup"><span data-stu-id="ce500-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="ce500-173">Wywoływanie `subject.next(item)` przy użyciu elementu zapisuje elementu w strumieniu i metody koncentratora otrzyma elementu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="ce500-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="ce500-174">Aby zakończyć strumień, należy wywołać `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="ce500-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="ce500-175">Klient Java</span><span class="sxs-lookup"><span data-stu-id="ce500-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="ce500-176">Przesyłanie strumieniowe serwera do klienta</span><span class="sxs-lookup"><span data-stu-id="ce500-176">Server-to-client streaming</span></span>

<span data-ttu-id="ce500-177">Klient SignalR Java używa `stream` metody do wywołania metody przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="ce500-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="ce500-178">`stream` akceptuje co najmniej trzech argumentów:</span><span class="sxs-lookup"><span data-stu-id="ce500-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="ce500-179">Oczekiwany typ elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="ce500-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="ce500-180">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ce500-180">The name of the hub method.</span></span>
* <span data-ttu-id="ce500-181">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ce500-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="ce500-182">`stream` Metody `HubConnection` zwraca zauważalny typu elementu strumienia.</span><span class="sxs-lookup"><span data-stu-id="ce500-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="ce500-183">Typ zauważalne `subscribe` metodą jest gdzie `onNext`, `onError` i `onCompleted` obsługi są zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="ce500-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ce500-184">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ce500-184">Additional resources</span></span>

* [<span data-ttu-id="ce500-185">Centra</span><span class="sxs-lookup"><span data-stu-id="ce500-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ce500-186">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="ce500-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ce500-187">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="ce500-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ce500-188">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="ce500-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
