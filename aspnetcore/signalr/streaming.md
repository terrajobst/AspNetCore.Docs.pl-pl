---
title: Użyj przesyłania strumieniowego w ASP.NET Core SignalR
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: ae0e733dddfb48db07d77ea73f4673cf8f783b88
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275853"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="e25fc-102">Użyj przesyłania strumieniowego w ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e25fc-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="e25fc-103">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="e25fc-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="e25fc-104">SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracanych metody serwera.</span><span class="sxs-lookup"><span data-stu-id="e25fc-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="e25fc-105">Jest to przydatne w scenariuszach, skąd pochodzą będzie fragmentów danych czasie.</span><span class="sxs-lookup"><span data-stu-id="e25fc-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="e25fc-106">Gdy wartością zwracaną jest przesyłane strumieniowo do klienta, każdego fragmentu są wysyłane do klienta jak staje się dostępna, zamiast oczekiwania dla wszystkich danych stanie się dostępne.</span><span class="sxs-lookup"><span data-stu-id="e25fc-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="e25fc-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e25fc-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="e25fc-108">Konfigurowanie Centrum</span><span class="sxs-lookup"><span data-stu-id="e25fc-108">Set up the hub</span></span>

<span data-ttu-id="e25fc-109">Metody koncentratora automatycznie staje się przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>` lub `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="e25fc-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="e25fc-110">Poniżej przedstawiono przykładowe, która przedstawia podstawy strumieniowego przesyłania danych do klienta.</span><span class="sxs-lookup"><span data-stu-id="e25fc-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="e25fc-111">Zawsze, gdy obiekt jest zapisywany w `ChannelReader` tego obiektu jest natychmiast wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="e25fc-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="e25fc-112">Na koniec `ChannelReader` zakończeniu mówić klienta strumień jest zamknięty.</span><span class="sxs-lookup"><span data-stu-id="e25fc-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="e25fc-113">Zapis do `ChannelReader` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="e25fc-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="e25fc-114">Inne wywołania koncentratora będzie zablokowany do momentu `ChannelReader` jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="e25fc-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="e25fc-115">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="e25fc-115">.NET client</span></span>

<span data-ttu-id="e25fc-116">`StreamAsChannelAsync` Metoda `HubConnection` służy do wywoływania metody przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="e25fc-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="e25fc-117">Przekaż nazwę metody koncentratora i argumentów w metodzie koncentratora do `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="e25fc-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="e25fc-118">Parametr ogólny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="e25fc-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="e25fc-119">A `ChannelReader<T>` jest zwracany z wywołania strumienia i reprezentuje strumienia na kliencie.</span><span class="sxs-lookup"><span data-stu-id="e25fc-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="e25fc-120">Do odczytywania danych, ma pętlę wspólnego wzorca `WaitToReadAsync` i Wywołaj `TryRead` kiedy dane są dostępne.</span><span class="sxs-lookup"><span data-stu-id="e25fc-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="e25fc-121">Pętla zakończy strumień został zamknięty przez serwer lub przekazany token anulowania do `StreamAsChannelAsync` zostało anulowane.</span><span class="sxs-lookup"><span data-stu-id="e25fc-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="e25fc-122">JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="e25fc-122">JavaScript client</span></span>

<span data-ttu-id="e25fc-123">Klientów języka JavaScript wywoływać metod przesyłania strumieniowego na koncentratory przy użyciu `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="e25fc-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="e25fc-124">`stream` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="e25fc-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="e25fc-125">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="e25fc-125">The name of the hub method.</span></span> <span data-ttu-id="e25fc-126">W poniższym przykładzie nazwa metody koncentratora jest `Counter`.</span><span class="sxs-lookup"><span data-stu-id="e25fc-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="e25fc-127">Argumenty w metodzie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="e25fc-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="e25fc-128">W poniższym przykładzie argumentami są: liczba liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.</span><span class="sxs-lookup"><span data-stu-id="e25fc-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="e25fc-129">`connection.stream` Zwraca `IStreamResult` zawierającą `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="e25fc-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="e25fc-130">Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołania zwrotne otrzymywać powiadomień z `stream` wywołania.</span><span class="sxs-lookup"><span data-stu-id="e25fc-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="e25fc-131">Na koniec strumienia z wywołania klienta `dispose` metoda `ISubscription` zwracanego z `subscribe` metody.</span><span class="sxs-lookup"><span data-stu-id="e25fc-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="e25fc-132">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="e25fc-132">Related resources</span></span>

* [<span data-ttu-id="e25fc-133">Centra</span><span class="sxs-lookup"><span data-stu-id="e25fc-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e25fc-134">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="e25fc-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e25fc-135">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="e25fc-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e25fc-136">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="e25fc-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)