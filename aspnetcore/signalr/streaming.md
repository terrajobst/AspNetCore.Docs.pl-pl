---
title: Użyj przesyłania strumieniowego w ASP.NET Core SignalR
author: bradygaster
description: Dowiedz się, jak przesyłać strumieniowo dane między klientem a serwerem.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/streaming
ms.openlocfilehash: 21dd8180fe168f81ed68b01f02b81a6264d6e5a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667728"
---
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="48283-103">Użyj przesyłania strumieniowego w ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="48283-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="48283-104">Autor [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="48283-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="48283-105">ASP.NET Core SignalR obsługuje przesyłanie strumieniowe z klienta do serwera i z serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="48283-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="48283-106">Jest to przydatne w scenariuszach, w których fragmenty danych docierają w czasie.</span><span class="sxs-lookup"><span data-stu-id="48283-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="48283-107">Podczas przesyłania strumieniowego każdy fragment jest wysyłany do klienta lub serwera zaraz po jego udostępnieniu, a nie w celu uzyskania dostępu do wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="48283-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="48283-108">ASP.NET Core SignalR obsługuje przesyłanie strumieniowe zwracanych wartości metod serwera.</span><span class="sxs-lookup"><span data-stu-id="48283-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="48283-109">Jest to przydatne w scenariuszach, w których fragmenty danych docierają w czasie.</span><span class="sxs-lookup"><span data-stu-id="48283-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="48283-110">Gdy wartość zwracana jest przesyłana strumieniowo do klienta, każdy fragment jest wysyłany do klienta natychmiast po jego udostępnieniu, a nie czeka na udostępnienie wszystkich danych.</span><span class="sxs-lookup"><span data-stu-id="48283-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="48283-111">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="48283-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="48283-112">Konfigurowanie centrum do przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="48283-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="48283-113">Metoda centrum automatycznie zmienia metodę centrum przesyłania strumieniowego, gdy zwróci <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="48283-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="48283-114">Metoda centrum automatycznie zmienia metodę centrum przesyłania strumieniowego, gdy zwróci <xref:System.Threading.Channels.ChannelReader%601> lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="48283-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="48283-115">Przesyłanie strumieniowe między serwerami i klientami</span><span class="sxs-lookup"><span data-stu-id="48283-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="48283-116">Metody centrum przesyłania strumieniowego mogą zwracać `IAsyncEnumerable<T>` oprócz `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="48283-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="48283-117">Najprostszym sposobem zwrócenia `IAsyncEnumerable<T>` jest utworzenie metody centrum jako metody asynchronicznej iteratora, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="48283-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="48283-118">Metody iteratorów asynchronicznych centrum mogą akceptować `CancellationToken` parametr, który jest wyzwalany, gdy klient anuluje subskrypcję ze strumienia.</span><span class="sxs-lookup"><span data-stu-id="48283-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="48283-119">Metody iteratorów asynchronicznych unikają typowych problemów z kanałami, takich jak nie zwracają `ChannelReader` wystarczająco wcześnie lub opuszczają metodę bez wykonywania <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="48283-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="48283-120">Poniższy przykład przedstawia podstawy przesyłania strumieniowego danych do klienta przy użyciu kanałów.</span><span class="sxs-lookup"><span data-stu-id="48283-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="48283-121">Za każdym razem, gdy obiekt jest zapisywana w <xref:System.Threading.Channels.ChannelWriter%601>, obiekt jest natychmiast wysyłany do klienta.</span><span class="sxs-lookup"><span data-stu-id="48283-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="48283-122">Na koniec `ChannelWriter` zostanie zakończona, aby poinformować klienta, że strumień jest zamknięty.</span><span class="sxs-lookup"><span data-stu-id="48283-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="48283-123">Zapisuj `ChannelWriter<T>` w wątku w tle i zwracają `ChannelReader` najszybciej, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="48283-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="48283-124">Inne wywołania centrów są blokowane do momentu zwrócenia `ChannelReader`.</span><span class="sxs-lookup"><span data-stu-id="48283-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="48283-125">Zawijanie logiki w `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="48283-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="48283-126">Aby upewnić się, że wywołanie metody centrum zostało wykonane prawidłowo, Ukończ `Channel` w `catch` i poza `catch`.</span><span class="sxs-lookup"><span data-stu-id="48283-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="48283-127">Metody centrum przesyłania strumieniowego serwer-klient mogą akceptować `CancellationToken` parametr, który jest wyzwalany, gdy klient anulował subskrypcję ze strumienia.</span><span class="sxs-lookup"><span data-stu-id="48283-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="48283-128">Użyj tego tokenu, aby zatrzymać operację serwera i zwolnić wszystkie zasoby, jeśli klient odłączy się przed końcem strumienia.</span><span class="sxs-lookup"><span data-stu-id="48283-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="48283-129">Przesyłanie strumieniowe klient-serwer</span><span class="sxs-lookup"><span data-stu-id="48283-129">Client-to-server streaming</span></span>

<span data-ttu-id="48283-130">Metoda centrum automatycznie zmienia metodę centrum przesyłania strumieniowego klienta na serwer, gdy akceptuje jeden lub więcej obiektów typu <xref:System.Threading.Channels.ChannelReader%601> lub <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span><span class="sxs-lookup"><span data-stu-id="48283-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="48283-131">W poniższym przykładzie przedstawiono podstawowe informacje dotyczące odczytywania danych przesyłanych strumieniowo z klienta programu.</span><span class="sxs-lookup"><span data-stu-id="48283-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="48283-132">Za każdym razem, gdy klient zapisuje do <xref:System.Threading.Channels.ChannelWriter%601>, dane są zapisywane do `ChannelReader` na serwerze, z którego jest odczytywana Metoda centrum.</span><span class="sxs-lookup"><span data-stu-id="48283-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="48283-133">Poniżej znajduje się <xref:System.Collections.Generic.IAsyncEnumerable%601> wersja metody.</span><span class="sxs-lookup"><span data-stu-id="48283-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="48283-134">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="48283-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="48283-135">Przesyłanie strumieniowe między serwerami i klientami</span><span class="sxs-lookup"><span data-stu-id="48283-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="48283-136">Metody `StreamAsync` i `StreamAsChannelAsync` w `HubConnection` są używane do wywoływania metod przesyłania strumieniowego między serwerami i klientami.</span><span class="sxs-lookup"><span data-stu-id="48283-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="48283-137">Przekaż nazwę i argumenty metody centrum zdefiniowane w metodzie Hub do `StreamAsync` lub `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="48283-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="48283-138">Parametr generyczny w `StreamAsync<T>` i `StreamAsChannelAsync<T>` określa typ obiektów zwracanych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="48283-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="48283-139">Obiekt typu `IAsyncEnumerable<T>` lub `ChannelReader<T>` jest zwracany ze strumienia wywołania i reprezentuje strumień na kliencie.</span><span class="sxs-lookup"><span data-stu-id="48283-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="48283-140">Przykład `StreamAsync`, który zwraca `IAsyncEnumerable<int>`:</span><span class="sxs-lookup"><span data-stu-id="48283-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

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

<span data-ttu-id="48283-141">Odpowiadający przykład `StreamAsChannelAsync`, który zwraca `ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="48283-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

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

<span data-ttu-id="48283-142">Metoda `StreamAsChannelAsync` na `HubConnection` służy do wywoływania metody przesyłania strumieniowego między serwerami i klientami.</span><span class="sxs-lookup"><span data-stu-id="48283-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="48283-143">Przekaż nazwę i argumenty metody centrum zdefiniowane w metodzie Hub, aby `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="48283-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="48283-144">Parametr generyczny w `StreamAsChannelAsync<T>` określa typ obiektów zwracanych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="48283-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="48283-145">`ChannelReader<T>` jest zwracana ze strumienia wywołania i reprezentuje strumień na kliencie.</span><span class="sxs-lookup"><span data-stu-id="48283-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

<span data-ttu-id="48283-146">Metoda `StreamAsChannelAsync` na `HubConnection` służy do wywoływania metody przesyłania strumieniowego między serwerami i klientami.</span><span class="sxs-lookup"><span data-stu-id="48283-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="48283-147">Przekaż nazwę i argumenty metody centrum zdefiniowane w metodzie Hub, aby `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="48283-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="48283-148">Parametr generyczny w `StreamAsChannelAsync<T>` określa typ obiektów zwracanych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="48283-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="48283-149">`ChannelReader<T>` jest zwracana ze strumienia wywołania i reprezentuje strumień na kliencie.</span><span class="sxs-lookup"><span data-stu-id="48283-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="48283-150">Przesyłanie strumieniowe klient-serwer</span><span class="sxs-lookup"><span data-stu-id="48283-150">Client-to-server streaming</span></span>

<span data-ttu-id="48283-151">Istnieją dwa sposoby wywołania metody centrum przesyłania strumieniowego klienta na serwer z poziomu klienta platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="48283-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="48283-152">W zależności od wywoływanej metody centrum można przekazać `IAsyncEnumerable<T>` lub `ChannelReader` jako argument do `SendAsync`, `InvokeAsync`lub `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="48283-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="48283-153">Za każdym razem, gdy dane są zapisywane w obiekcie `IAsyncEnumerable` lub `ChannelWriter`, Metoda Hub na serwerze otrzymuje nowy element z danymi od klienta.</span><span class="sxs-lookup"><span data-stu-id="48283-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="48283-154">Jeśli używasz obiektu `IAsyncEnumerable`, strumień kończy się po metodzie zwracającej elementy strumienia.</span><span class="sxs-lookup"><span data-stu-id="48283-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

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

<span data-ttu-id="48283-155">Lub jeśli używasz `ChannelWriter`, Ukończ kanał z `channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="48283-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="48283-156">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="48283-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="48283-157">Przesyłanie strumieniowe między serwerami i klientami</span><span class="sxs-lookup"><span data-stu-id="48283-157">Server-to-client streaming</span></span>

<span data-ttu-id="48283-158">Klienci języka JavaScript wywołują metody przesyłania strumieniowego z serwera do klienta w centrach z `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="48283-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="48283-159">Metoda `stream` akceptuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="48283-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="48283-160">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="48283-160">The name of the hub method.</span></span> <span data-ttu-id="48283-161">W poniższym przykładzie nazwa metody centrum jest `Counter`.</span><span class="sxs-lookup"><span data-stu-id="48283-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="48283-162">Argumenty zdefiniowane w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="48283-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="48283-163">W poniższym przykładzie argumenty są liczbami elementów strumienia do odebrania oraz opóźnieniem między elementami strumienia.</span><span class="sxs-lookup"><span data-stu-id="48283-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="48283-164">`connection.stream` zwraca `IStreamResult`, który zawiera metodę `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="48283-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="48283-165">Przekaż `IStreamSubscriber`, aby `subscribe` i ustawić wywołania zwrotne `next`, `error`i `complete`, aby otrzymywać powiadomienia z `stream` wywołania.</span><span class="sxs-lookup"><span data-stu-id="48283-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="48283-166">Aby zakończyć strumień z klienta, wywołaj metodę `dispose` na `ISubscription`, który jest zwracany z metody `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="48283-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="48283-167">Wywołanie tej metody powoduje anulowanie parametru `CancellationToken` metody Hub, jeśli został podany.</span><span class="sxs-lookup"><span data-stu-id="48283-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="48283-168">Aby zakończyć strumień z klienta, wywołaj metodę `dispose` na `ISubscription`, który jest zwracany z metody `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="48283-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="48283-169">Przesyłanie strumieniowe klient-serwer</span><span class="sxs-lookup"><span data-stu-id="48283-169">Client-to-server streaming</span></span>

<span data-ttu-id="48283-170">Klienci języka JavaScript wywołują metody przesyłania strumieniowego klient-serwer w centrach, przekazując `Subject` jako argument do `send`, `invoke`lub `stream`, w zależności od wywoływanej metody centrum.</span><span class="sxs-lookup"><span data-stu-id="48283-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="48283-171">`Subject` jest klasą, która wygląda jak `Subject`.</span><span class="sxs-lookup"><span data-stu-id="48283-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="48283-172">Na przykład w RxJS można użyć klasy [subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) z tej biblioteki.</span><span class="sxs-lookup"><span data-stu-id="48283-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="48283-173">Wywołanie `subject.next(item)` z elementem zapisuje element do strumienia, a metoda centrum odbiera element na serwerze.</span><span class="sxs-lookup"><span data-stu-id="48283-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="48283-174">Aby zakończyć przesyłanie strumienia, wywołaj `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="48283-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="48283-175">Klient Java</span><span class="sxs-lookup"><span data-stu-id="48283-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="48283-176">Przesyłanie strumieniowe między serwerami i klientami</span><span class="sxs-lookup"><span data-stu-id="48283-176">Server-to-client streaming</span></span>

<span data-ttu-id="48283-177">Klient języka Java SignalR używa metody `stream` do wywoływania metod przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="48283-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="48283-178">`stream` akceptuje trzy lub więcej argumentów:</span><span class="sxs-lookup"><span data-stu-id="48283-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="48283-179">Oczekiwany typ elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="48283-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="48283-180">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="48283-180">The name of the hub method.</span></span>
* <span data-ttu-id="48283-181">Argumenty zdefiniowane w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="48283-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="48283-182">Metoda `stream` na `HubConnection` zwraca widoczny typ elementu strumienia.</span><span class="sxs-lookup"><span data-stu-id="48283-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="48283-183">Metoda `subscribe` typu zauważalnego to miejsce, w którym są zdefiniowane `onNext`, `onError` i `onCompleted` obsługi.</span><span class="sxs-lookup"><span data-stu-id="48283-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="48283-184">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48283-184">Additional resources</span></span>

* [<span data-ttu-id="48283-185">Centra</span><span class="sxs-lookup"><span data-stu-id="48283-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="48283-186">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="48283-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="48283-187">Klient środowiska JavaScript</span><span class="sxs-lookup"><span data-stu-id="48283-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="48283-188">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="48283-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
