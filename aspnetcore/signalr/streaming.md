---
title: Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: ade2d6fb6e799d53ff3aaa69c641d0088acdee95
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837406"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="d6229-102">Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6229-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="d6229-103">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="d6229-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="d6229-104">SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracane metod serwera.</span><span class="sxs-lookup"><span data-stu-id="d6229-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="d6229-105">Jest to przydatne w scenariuszach, gdzie fragmenty danych będą pochodzić wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="d6229-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="d6229-106">Gdy wartość zwracana jest przesyłany strumieniowo do klienta, poszczególne fragmenty są wysyłane do klienta tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.</span><span class="sxs-lookup"><span data-stu-id="d6229-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="d6229-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6229-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="d6229-108">Konfigurowanie Centrum</span><span class="sxs-lookup"><span data-stu-id="d6229-108">Set up the hub</span></span>

<span data-ttu-id="d6229-109">Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>` lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="d6229-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="d6229-110">Poniżej przedstawiono przykład przedstawiający podstawy przesyłanie strumieniowe danych do klienta.</span><span class="sxs-lookup"><span data-stu-id="d6229-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="d6229-111">Zawsze, gdy obiekt jest zapisywany `ChannelReader` tego obiektu są natychmiast wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="d6229-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="d6229-112">Na koniec `ChannelReader` zakończeniu to sprawdzić klientowi strumień jest zamknięty.</span><span class="sxs-lookup"><span data-stu-id="d6229-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="d6229-113">Zapisać `ChannelReader` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="d6229-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="d6229-114">Inne wywołania koncentratora będzie zablokowany do momentu `ChannelReader` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="d6229-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="d6229-115">OPAKOWYWANIE logikę w `try ... catch` i ukończyć `Channel` w catch i na zewnątrz catch, aby upewnić się, że Centrum zakończyła wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="d6229-115">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="d6229-116">W programie ASP.NET Core 2.2 lub nowszej, przesyłanie strumieniowe metod koncentratora może akceptować `CancellationToken` parametr, który zostanie wyzwolony, gdy klient anuluje subskrypcje ze strumienia.</span><span class="sxs-lookup"><span data-stu-id="d6229-116">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="d6229-117">Używanie tego tokenu, aby zatrzymać działanie serwera i zwolnić wszystkie zasoby, jeśli klient odłączy się do końca strumienia.</span><span class="sxs-lookup"><span data-stu-id="d6229-117">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="d6229-118">Klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="d6229-118">.NET client</span></span>

<span data-ttu-id="d6229-119">`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="d6229-119">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="d6229-120">Przekaż nazwę metody koncentratora oraz argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="d6229-120">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="d6229-121">Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="d6229-121">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="d6229-122">Element `ChannelReader<T>` jest zwracany z wywołania usługi stream i reprezentuje strumienia na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="d6229-122">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="d6229-123">Do odczytywania danych, typowym wzorcem jest pętli `WaitToReadAsync` i wywołać `TryRead` kiedy dane są dostępne.</span><span class="sxs-lookup"><span data-stu-id="d6229-123">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="d6229-124">Pętla zakończy się po strumień został zamknięty przez serwer lub token anulowania jest przekazywany do `StreamAsChannelAsync` zostało anulowane.</span><span class="sxs-lookup"><span data-stu-id="d6229-124">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
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

## <a name="javascript-client"></a><span data-ttu-id="d6229-125">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="d6229-125">JavaScript client</span></span>

<span data-ttu-id="d6229-126">Klientów języka JavaScript pozwala wywoływać metody przesyłania strumieniowego dla koncentratorów, za pomocą `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="d6229-126">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="d6229-127">`stream` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="d6229-127">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="d6229-128">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="d6229-128">The name of the hub method.</span></span> <span data-ttu-id="d6229-129">W poniższym przykładzie nazwa metody koncentratora jest `Counter`.</span><span class="sxs-lookup"><span data-stu-id="d6229-129">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="d6229-130">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="d6229-130">Arguments defined in the hub method.</span></span> <span data-ttu-id="d6229-131">W poniższym przykładzie argumenty są: liczba, dla liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.</span><span class="sxs-lookup"><span data-stu-id="d6229-131">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="d6229-132">`connection.stream` Zwraca `IStreamResult` zawierającą `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="d6229-132">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="d6229-133">Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołań zwrotnych, aby otrzymywać powiadomienia z `stream` wywołania.</span><span class="sxs-lookup"><span data-stu-id="d6229-133">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="d6229-134">Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="d6229-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d6229-135">Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="d6229-135">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="d6229-136">Wywołanie tej metody spowoduje, że `CancellationToken` parametru metody koncentratora (jeśli zostały zapewnione jest jeden) zostaną anulowane.</span><span class="sxs-lookup"><span data-stu-id="d6229-136">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="d6229-137">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="d6229-137">Related resources</span></span>

* [<span data-ttu-id="d6229-138">Centra</span><span class="sxs-lookup"><span data-stu-id="d6229-138">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="d6229-139">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="d6229-139">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="d6229-140">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="d6229-140">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="d6229-141">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="d6229-141">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
