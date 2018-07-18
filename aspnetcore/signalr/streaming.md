---
title: Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 0001eed830249ac46ba35331759187bb4e7e8fd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095265"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="b26fe-102">Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b26fe-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="b26fe-103">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="b26fe-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="b26fe-104">SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracane metod serwera.</span><span class="sxs-lookup"><span data-stu-id="b26fe-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="b26fe-105">Jest to przydatne w scenariuszach, gdzie fragmenty danych będą pochodzić wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="b26fe-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="b26fe-106">Gdy wartość zwracana jest przesyłany strumieniowo do klienta, poszczególne fragmenty są wysyłane do klienta tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.</span><span class="sxs-lookup"><span data-stu-id="b26fe-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="b26fe-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b26fe-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="b26fe-108">Konfigurowanie Centrum</span><span class="sxs-lookup"><span data-stu-id="b26fe-108">Set up the hub</span></span>

<span data-ttu-id="b26fe-109">Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>` lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="b26fe-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="b26fe-110">Poniżej przedstawiono przykład przedstawiający podstawy przesyłanie strumieniowe danych do klienta.</span><span class="sxs-lookup"><span data-stu-id="b26fe-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="b26fe-111">Zawsze, gdy obiekt jest zapisywany `ChannelReader` tego obiektu są natychmiast wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="b26fe-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="b26fe-112">Na koniec `ChannelReader` zakończeniu to sprawdzić klientowi strumień jest zamknięty.</span><span class="sxs-lookup"><span data-stu-id="b26fe-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="b26fe-113">Zapisać `ChannelReader` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="b26fe-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="b26fe-114">Inne wywołania koncentratora będzie zablokowany do momentu `ChannelReader` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="b26fe-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="b26fe-115">Klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="b26fe-115">.NET client</span></span>

<span data-ttu-id="b26fe-116">`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="b26fe-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="b26fe-117">Przekaż nazwę metody koncentratora oraz argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="b26fe-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="b26fe-118">Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="b26fe-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="b26fe-119">Element `ChannelReader<T>` jest zwracany z wywołania usługi stream i reprezentuje strumienia na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b26fe-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="b26fe-120">Do odczytywania danych, typowym wzorcem jest pętli `WaitToReadAsync` i wywołać `TryRead` kiedy dane są dostępne.</span><span class="sxs-lookup"><span data-stu-id="b26fe-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="b26fe-121">Pętla zakończy się po strumień został zamknięty przez serwer lub token anulowania jest przekazywany do `StreamAsChannelAsync` zostało anulowane.</span><span class="sxs-lookup"><span data-stu-id="b26fe-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

## <a name="javascript-client"></a><span data-ttu-id="b26fe-122">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="b26fe-122">JavaScript client</span></span>

<span data-ttu-id="b26fe-123">Klientów języka JavaScript pozwala wywoływać metody przesyłania strumieniowego dla koncentratorów, za pomocą `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="b26fe-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="b26fe-124">`stream` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="b26fe-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="b26fe-125">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b26fe-125">The name of the hub method.</span></span> <span data-ttu-id="b26fe-126">W poniższym przykładzie nazwa metody koncentratora jest `Counter`.</span><span class="sxs-lookup"><span data-stu-id="b26fe-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="b26fe-127">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b26fe-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="b26fe-128">W poniższym przykładzie argumenty są: liczba, dla liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.</span><span class="sxs-lookup"><span data-stu-id="b26fe-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="b26fe-129">`connection.stream` Zwraca `IStreamResult` zawierającą `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="b26fe-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="b26fe-130">Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołań zwrotnych, aby otrzymywać powiadomienia z `stream` wywołania.</span><span class="sxs-lookup"><span data-stu-id="b26fe-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="b26fe-131">Do końca strumienia z wywołania klienta `dispose` metody `ISubscription` zwrócone z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="b26fe-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="b26fe-132">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="b26fe-132">Related resources</span></span>

* [<span data-ttu-id="b26fe-133">Centra</span><span class="sxs-lookup"><span data-stu-id="b26fe-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b26fe-134">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="b26fe-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="b26fe-135">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="b26fe-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b26fe-136">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="b26fe-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
