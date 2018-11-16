---
title: Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 6d5f707bd2a37e1999c6e87e3cfc369aa0301207
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708442"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="223b5-102">Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="223b5-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="223b5-103">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="223b5-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="223b5-104">SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracane metod serwera.</span><span class="sxs-lookup"><span data-stu-id="223b5-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="223b5-105">Jest to przydatne w scenariuszach, gdzie fragmenty danych będą pochodzić wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="223b5-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="223b5-106">Gdy wartość zwracana jest przesyłany strumieniowo do klienta, poszczególne fragmenty są wysyłane do klienta tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.</span><span class="sxs-lookup"><span data-stu-id="223b5-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="223b5-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="223b5-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="223b5-108">Konfigurowanie Centrum</span><span class="sxs-lookup"><span data-stu-id="223b5-108">Set up the hub</span></span>

<span data-ttu-id="223b5-109">Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>` lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="223b5-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="223b5-110">Poniżej przedstawiono przykład przedstawiający podstawy przesyłanie strumieniowe danych do klienta.</span><span class="sxs-lookup"><span data-stu-id="223b5-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="223b5-111">Zawsze, gdy obiekt jest zapisywany `ChannelReader` tego obiektu są natychmiast wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="223b5-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="223b5-112">Na koniec `ChannelReader` zakończeniu to sprawdzić klientowi strumień jest zamknięty.</span><span class="sxs-lookup"><span data-stu-id="223b5-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="223b5-113">Zapisać `ChannelReader` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="223b5-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="223b5-114">Inne wywołania koncentratora będzie zablokowany do momentu `ChannelReader` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="223b5-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?range=12-36)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=11-35)]

> [!NOTE]
> <span data-ttu-id="223b5-115">W programie ASP.NET Core 2.2 lub nowszej, przesyłanie strumieniowe metod koncentratora może akceptować `CancellationToken` parametr, który zostanie wyzwolony, gdy klient anuluje subskrypcje ze strumienia.</span><span class="sxs-lookup"><span data-stu-id="223b5-115">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="223b5-116">Używanie tego tokenu, aby zatrzymać działanie serwera i zwolnić wszystkie zasoby, jeśli klient odłączy się do końca strumienia.</span><span class="sxs-lookup"><span data-stu-id="223b5-116">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="223b5-117">Klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="223b5-117">.NET client</span></span>

<span data-ttu-id="223b5-118">`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="223b5-118">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="223b5-119">Przekaż nazwę metody koncentratora oraz argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="223b5-119">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="223b5-120">Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="223b5-120">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="223b5-121">Element `ChannelReader<T>` jest zwracany z wywołania usługi stream i reprezentuje strumienia na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="223b5-121">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="223b5-122">Do odczytywania danych, typowym wzorcem jest pętli `WaitToReadAsync` i wywołać `TryRead` kiedy dane są dostępne.</span><span class="sxs-lookup"><span data-stu-id="223b5-122">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="223b5-123">Pętla zakończy się po strumień został zamknięty przez serwer lub token anulowania jest przekazywany do `StreamAsChannelAsync` zostało anulowane.</span><span class="sxs-lookup"><span data-stu-id="223b5-123">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="223b5-124">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="223b5-124">JavaScript client</span></span>

<span data-ttu-id="223b5-125">Klientów języka JavaScript pozwala wywoływać metody przesyłania strumieniowego dla koncentratorów, za pomocą `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="223b5-125">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="223b5-126">`stream` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="223b5-126">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="223b5-127">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="223b5-127">The name of the hub method.</span></span> <span data-ttu-id="223b5-128">W poniższym przykładzie nazwa metody koncentratora jest `Counter`.</span><span class="sxs-lookup"><span data-stu-id="223b5-128">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="223b5-129">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="223b5-129">Arguments defined in the hub method.</span></span> <span data-ttu-id="223b5-130">W poniższym przykładzie argumenty są: liczba, dla liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.</span><span class="sxs-lookup"><span data-stu-id="223b5-130">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="223b5-131">`connection.stream` Zwraca `IStreamResult` zawierającą `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="223b5-131">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="223b5-132">Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołań zwrotnych, aby otrzymywać powiadomienia z `stream` wywołania.</span><span class="sxs-lookup"><span data-stu-id="223b5-132">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="223b5-133">Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="223b5-133">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="223b5-134">Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="223b5-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="223b5-135">Wywołanie tej metody spowoduje, że `CancellationToken` parametru metody koncentratora (jeśli zostały zapewnione jest jeden) zostaną anulowane.</span><span class="sxs-lookup"><span data-stu-id="223b5-135">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="223b5-136">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="223b5-136">Related resources</span></span>

* [<span data-ttu-id="223b5-137">Centra</span><span class="sxs-lookup"><span data-stu-id="223b5-137">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="223b5-138">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="223b5-138">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="223b5-139">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="223b5-139">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="223b5-140">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="223b5-140">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
