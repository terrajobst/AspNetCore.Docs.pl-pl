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
ms.openlocfilehash: 7825beba55cefb6236fd8d8e332d030a7e4fc6df
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963884"
---
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a>Użyj przesyłania strumieniowego w ASP.NET Core SignalR

Autor [Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR obsługuje przesyłanie strumieniowe z klienta do serwera i z serwera do klienta. Jest to przydatne w scenariuszach, w których fragmenty danych docierają w czasie. Podczas przesyłania strumieniowego każdy fragment jest wysyłany do klienta lub serwera zaraz po jego udostępnieniu, a nie w celu uzyskania dostępu do wszystkich danych.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR obsługuje przesyłanie strumieniowe zwracanych wartości metod serwera. Jest to przydatne w scenariuszach, w których fragmenty danych docierają w czasie. Gdy wartość zwracana jest przesyłana strumieniowo do klienta, każdy fragment jest wysyłany do klienta natychmiast po jego udostępnieniu, a nie czeka na udostępnienie wszystkich danych.

::: moniker-end

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Konfigurowanie centrum do przesyłania strumieniowego

::: moniker range=">= aspnetcore-3.0"

Metoda centrum automatycznie zmienia metodę centrum przesyłania strumieniowego, gdy zwróci <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`lub `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Metoda centrum automatycznie zmienia metodę centrum przesyłania strumieniowego, gdy zwróci <xref:System.Threading.Channels.ChannelReader%601> lub `Task<ChannelReader<T>>`.

::: moniker-end

### <a name="server-to-client-streaming"></a>Przesyłanie strumieniowe między serwerami i klientami

::: moniker range=">= aspnetcore-3.0"

Metody centrum przesyłania strumieniowego mogą zwracać `IAsyncEnumerable<T>` oprócz `ChannelReader<T>`. Najprostszym sposobem zwrócenia `IAsyncEnumerable<T>` jest utworzenie metody centrum jako metody asynchronicznej iteratora, jak pokazano w poniższym przykładzie. Metody iteratorów asynchronicznych centrum mogą akceptować `CancellationToken` parametr, który jest wyzwalany, gdy klient anuluje subskrypcję ze strumienia. Metody iteratorów asynchronicznych unikają typowych problemów z kanałami, takich jak nie zwracają `ChannelReader` wystarczająco wcześnie lub opuszczają metodę bez wykonywania <xref:System.Threading.Channels.ChannelWriter`1>.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

Poniższy przykład przedstawia podstawy przesyłania strumieniowego danych do klienta przy użyciu kanałów. Za każdym razem, gdy obiekt jest zapisywana w <xref:System.Threading.Channels.ChannelWriter%601>, obiekt jest natychmiast wysyłany do klienta. Na koniec `ChannelWriter` zostanie zakończona, aby poinformować klienta, że strumień jest zamknięty.

> [!NOTE]
> Zapisuj `ChannelWriter<T>` w wątku w tle i zwracają `ChannelReader` najszybciej, jak to możliwe. Inne wywołania centrów są blokowane do momentu zwrócenia `ChannelReader`.
>
> Zawijanie logiki w `try ... catch`. Aby upewnić się, że wywołanie metody centrum zostało wykonane prawidłowo, Ukończ `Channel` w `catch` i poza `catch`.

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

Metody centrum przesyłania strumieniowego serwer-klient mogą akceptować `CancellationToken` parametr, który jest wyzwalany, gdy klient anulował subskrypcję ze strumienia. Użyj tego tokenu, aby zatrzymać operację serwera i zwolnić wszystkie zasoby, jeśli klient odłączy się przed końcem strumienia.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Przesyłanie strumieniowe klient-serwer

Metoda centrum automatycznie zmienia metodę centrum przesyłania strumieniowego klienta na serwer, gdy akceptuje jeden lub więcej obiektów typu <xref:System.Threading.Channels.ChannelReader%601> lub <xref:System.Collections.Generic.IAsyncEnumerable%601>. W poniższym przykładzie przedstawiono podstawowe informacje dotyczące odczytywania danych przesyłanych strumieniowo z klienta programu. Za każdym razem, gdy klient zapisuje do <xref:System.Threading.Channels.ChannelWriter%601>, dane są zapisywane do `ChannelReader` na serwerze, z którego jest odczytywana Metoda centrum.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

Poniżej znajduje się <xref:System.Collections.Generic.IAsyncEnumerable%601> wersja metody.

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

## <a name="net-client"></a>Klient .NET

### <a name="server-to-client-streaming"></a>Przesyłanie strumieniowe między serwerami i klientami


::: moniker range=">= aspnetcore-3.0"

Metody `StreamAsync` i `StreamAsChannelAsync` w `HubConnection` są używane do wywoływania metod przesyłania strumieniowego między serwerami i klientami. Przekaż nazwę i argumenty metody centrum zdefiniowane w metodzie Hub do `StreamAsync` lub `StreamAsChannelAsync`. Parametr generyczny w `StreamAsync<T>` i `StreamAsChannelAsync<T>` określa typ obiektów zwracanych przez metodę przesyłania strumieniowego. Obiekt typu `IAsyncEnumerable<T>` lub `ChannelReader<T>` jest zwracany ze strumienia wywołania i reprezentuje strumień na kliencie.

Przykład `StreamAsync`, który zwraca `IAsyncEnumerable<int>`:

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

Odpowiadający przykład `StreamAsChannelAsync`, który zwraca `ChannelReader<int>`:

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

Metoda `StreamAsChannelAsync` na `HubConnection` służy do wywoływania metody przesyłania strumieniowego między serwerami i klientami. Przekaż nazwę i argumenty metody centrum zdefiniowane w metodzie Hub, aby `StreamAsChannelAsync`. Parametr generyczny w `StreamAsChannelAsync<T>` określa typ obiektów zwracanych przez metodę przesyłania strumieniowego. `ChannelReader<T>` jest zwracana ze strumienia wywołania i reprezentuje strumień na kliencie.

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

Metoda `StreamAsChannelAsync` na `HubConnection` służy do wywoływania metody przesyłania strumieniowego między serwerami i klientami. Przekaż nazwę i argumenty metody centrum zdefiniowane w metodzie Hub, aby `StreamAsChannelAsync`. Parametr generyczny w `StreamAsChannelAsync<T>` określa typ obiektów zwracanych przez metodę przesyłania strumieniowego. `ChannelReader<T>` jest zwracana ze strumienia wywołania i reprezentuje strumień na kliencie.

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

### <a name="client-to-server-streaming"></a>Przesyłanie strumieniowe klient-serwer

Istnieją dwa sposoby wywołania metody centrum przesyłania strumieniowego klienta na serwer z poziomu klienta platformy .NET. W zależności od wywoływanej metody centrum można przekazać `IAsyncEnumerable<T>` lub `ChannelReader` jako argument do `SendAsync`, `InvokeAsync`lub `StreamAsChannelAsync`.

Za każdym razem, gdy dane są zapisywane w obiekcie `IAsyncEnumerable` lub `ChannelWriter`, Metoda Hub na serwerze otrzymuje nowy element z danymi od klienta.

Jeśli używasz obiektu `IAsyncEnumerable`, strumień kończy się po metodzie zwracającej elementy strumienia.

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

Lub jeśli używasz `ChannelWriter`, Ukończ kanał z `channel.Writer.Complete()`:

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>Klient JavaScript

### <a name="server-to-client-streaming"></a>Przesyłanie strumieniowe między serwerami i klientami

Klienci języka JavaScript wywołują metody przesyłania strumieniowego z serwera do klienta w centrach z `connection.stream`. Metoda `stream` akceptuje dwa argumenty:

* Nazwa metody centrum. W poniższym przykładzie nazwa metody centrum jest `Counter`.
* Argumenty zdefiniowane w metodzie centrum. W poniższym przykładzie argumenty są liczbami elementów strumienia do odebrania oraz opóźnieniem między elementami strumienia.

`connection.stream` zwraca `IStreamResult`, który zawiera metodę `subscribe`. Przekaż `IStreamSubscriber`, aby `subscribe` i ustawić wywołania zwrotne `next`, `error`i `complete`, aby otrzymywać powiadomienia z `stream` wywołania.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Aby zakończyć strumień z klienta, wywołaj metodę `dispose` na `ISubscription`, który jest zwracany z metody `subscribe`. Wywołanie tej metody powoduje anulowanie parametru `CancellationToken` metody Hub, jeśli został podany.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Aby zakończyć strumień z klienta, wywołaj metodę `dispose` na `ISubscription`, który jest zwracany z metody `subscribe`.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Przesyłanie strumieniowe klient-serwer

Klienci języka JavaScript wywołują metody przesyłania strumieniowego klient-serwer w centrach, przekazując `Subject` jako argument do `send`, `invoke`lub `stream`, w zależności od wywoływanej metody centrum. `Subject` jest klasą, która wygląda jak `Subject`. Na przykład w RxJS można użyć klasy [subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) z tej biblioteki.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

Wywołanie `subject.next(item)` z elementem zapisuje element do strumienia, a metoda centrum odbiera element na serwerze.

Aby zakończyć przesyłanie strumienia, wywołaj `subject.complete()`.

## <a name="java-client"></a>Klient Java

### <a name="server-to-client-streaming"></a>Przesyłanie strumieniowe między serwerami i klientami

Klient języka Java SignalR używa metody `stream` do wywoływania metod przesyłania strumieniowego. `stream` akceptuje trzy lub więcej argumentów:

* Oczekiwany typ elementów strumienia.
* Nazwa metody centrum.
* Argumenty zdefiniowane w metodzie centrum.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

Metoda `stream` na `HubConnection` zwraca widoczny typ elementu strumienia. Metoda `subscribe` typu zauważalnego to miejsce, w którym są zdefiniowane `onNext`, `onError` i `onCompleted` obsługi.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
