---
title: Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się, jak zwracanie strumieni wartości z metod koncentratora serwera oraz wykorzystują strumienie przy użyciu klientów platformy .NET i języka JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 7c176e3f21ffca7b97d9d3c2e8861032f22587b8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264306"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="29b07-103">Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29b07-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="29b07-104">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="29b07-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="29b07-105">SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracane metod serwera.</span><span class="sxs-lookup"><span data-stu-id="29b07-105">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="29b07-106">Jest to przydatne w scenariuszach, gdzie fragmenty danych będą pochodzić wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="29b07-106">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="29b07-107">Gdy wartość zwracana jest przesyłany strumieniowo do klienta, poszczególne fragmenty są wysyłane do klienta tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.</span><span class="sxs-lookup"><span data-stu-id="29b07-107">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="29b07-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="29b07-108">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="29b07-109">Konfigurowanie Centrum</span><span class="sxs-lookup"><span data-stu-id="29b07-109">Set up the hub</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="29b07-110">Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, lub `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="29b07-110">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="29b07-111">Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>` lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="29b07-111">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="29b07-112">W programie ASP.NET Core 3.0 lub nowszej, przesyłanie strumieniowe metod koncentratora może zwrócić `IAsyncEnumerable<T>` oprócz `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="29b07-112">In ASP.NET Core 3.0 or later, streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="29b07-113">Najprostszym sposobem, aby zwrócić `IAsyncEnumerable<T>` energii jest upewnienie metody iteratora asynchronicznej metody koncentratora, tak jak pokazano w następującym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="29b07-113">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="29b07-114">Metody iteratora asynchroniczne Centrum może akceptować `CancellationToken` parametr, który zostanie wyzwolony, gdy klient anuluje subskrypcje ze strumienia.</span><span class="sxs-lookup"><span data-stu-id="29b07-114">Hub async iterator methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="29b07-115">Asynchroniczne metody iteracyjne łatwo problemom, typowe kanały takich jak nie zwraca `ChannelReader` wystarczająco wczesne tryb konserwacji lub wychodzenia z metodą przerywając `ChannelWriter`.</span><span class="sxs-lookup"><span data-stu-id="29b07-115">Async iterator methods easily avoid problems common with Channels such as not returning the `ChannelReader` early enough or exiting the method without completing the `ChannelWriter`.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="29b07-116">Poniższy przykład pokazuje podstawy przesyłanie strumieniowe danych do klienta za pomocą kanałów.</span><span class="sxs-lookup"><span data-stu-id="29b07-116">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="29b07-117">Zawsze, gdy obiekt jest zapisywany `ChannelWriter` tego obiektu są natychmiast wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="29b07-117">Whenever an object is written to the `ChannelWriter` that object is immediately sent to the client.</span></span> <span data-ttu-id="29b07-118">Na koniec `ChannelWriter` zakończeniu to sprawdzić klientowi strumień jest zamknięty.</span><span class="sxs-lookup"><span data-stu-id="29b07-118">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="29b07-119">Zapisać `ChannelWriter` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="29b07-119">Write to the `ChannelWriter` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="29b07-120">Inne wywołania koncentratora będzie zablokowany do momentu `ChannelReader` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="29b07-120">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="29b07-121">OPAKOWYWANIE logikę w `try ... catch` i ukończyć `Channel` w catch i na zewnątrz catch, aby upewnić się, że Centrum zakończyła wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="29b07-121">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="29b07-122">W programie ASP.NET Core 2.2 lub nowszej, przesyłanie strumieniowe metod koncentratora może akceptować `CancellationToken` parametr, który zostanie wyzwolony, gdy klient anuluje subskrypcje ze strumienia.</span><span class="sxs-lookup"><span data-stu-id="29b07-122">In ASP.NET Core 2.2 or later, streaming hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="29b07-123">Używanie tego tokenu, aby zatrzymać działanie serwera i zwolnić wszystkie zasoby, jeśli klient odłączy się do końca strumienia.</span><span class="sxs-lookup"><span data-stu-id="29b07-123">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="29b07-124">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="29b07-124">.NET client</span></span>

<span data-ttu-id="29b07-125">`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="29b07-125">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="29b07-126">Przekaż nazwę metody koncentratora oraz argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="29b07-126">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="29b07-127">Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="29b07-127">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="29b07-128">Element `ChannelReader<T>` jest zwracany z wywołania usługi stream i reprezentuje strumienia na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="29b07-128">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="29b07-129">Do odczytywania danych, typowym wzorcem jest pętli `WaitToReadAsync` i wywołać `TryRead` kiedy dane są dostępne.</span><span class="sxs-lookup"><span data-stu-id="29b07-129">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="29b07-130">Pętla zakończy się po strumień został zamknięty przez serwer lub token anulowania jest przekazywany do `StreamAsChannelAsync` zostało anulowane.</span><span class="sxs-lookup"><span data-stu-id="29b07-130">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

## <a name="javascript-client"></a><span data-ttu-id="29b07-131">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="29b07-131">JavaScript client</span></span>

<span data-ttu-id="29b07-132">Klientów języka JavaScript pozwala wywoływać metody przesyłania strumieniowego dla koncentratorów, za pomocą `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="29b07-132">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="29b07-133">`stream` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="29b07-133">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="29b07-134">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="29b07-134">The name of the hub method.</span></span> <span data-ttu-id="29b07-135">W poniższym przykładzie nazwa metody koncentratora jest `Counter`.</span><span class="sxs-lookup"><span data-stu-id="29b07-135">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="29b07-136">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="29b07-136">Arguments defined in the hub method.</span></span> <span data-ttu-id="29b07-137">W poniższym przykładzie argumenty są: liczba, dla liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.</span><span class="sxs-lookup"><span data-stu-id="29b07-137">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="29b07-138">`connection.stream` Zwraca `IStreamResult` zawierającą `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="29b07-138">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="29b07-139">Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołań zwrotnych, aby otrzymywać powiadomienia z `stream` wywołania.</span><span class="sxs-lookup"><span data-stu-id="29b07-139">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="29b07-140">Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="29b07-140">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="29b07-141">Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="29b07-141">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="29b07-142">Wywołanie tej metody spowoduje, że `CancellationToken` parametru metody koncentratora (jeśli zostały zapewnione jest jeden) zostaną anulowane.</span><span class="sxs-lookup"><span data-stu-id="29b07-142">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="java-client"></a><span data-ttu-id="29b07-143">Klient Java</span><span class="sxs-lookup"><span data-stu-id="29b07-143">Java client</span></span>

<span data-ttu-id="29b07-144">Klient SignalR Java używa `stream` metody do wywołania metody przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="29b07-144">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="29b07-145">Akceptuje ona co najmniej trzech argumentów:</span><span class="sxs-lookup"><span data-stu-id="29b07-145">It accepts three or more arguments:</span></span>

* <span data-ttu-id="29b07-146">Oczekiwany typ elementów strumienia</span><span class="sxs-lookup"><span data-stu-id="29b07-146">The expected type of the stream items</span></span>
* <span data-ttu-id="29b07-147">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="29b07-147">The name of the hub method.</span></span>
* <span data-ttu-id="29b07-148">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="29b07-148">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="29b07-149">`stream` Metody `HubConnection` zwraca zauważalny typu elementu strumienia.</span><span class="sxs-lookup"><span data-stu-id="29b07-149">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="29b07-150">Typ dostrzegalnych `subscribe` określa się za pomocą metody swoje `onNext`, `onError` i `onCompleted` programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="29b07-150">The Observable type's `subscribe` method is where you define your `onNext`,  `onError` and  `onCompleted` handlers.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="29b07-151">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="29b07-151">Related resources</span></span>

* [<span data-ttu-id="29b07-152">Centra</span><span class="sxs-lookup"><span data-stu-id="29b07-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="29b07-153">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="29b07-153">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="29b07-154">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="29b07-154">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="29b07-155">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="29b07-155">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
