---
title: Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się, jak przesłać strumień danych między klientem a serwerem.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2019
uid: signalr/streaming
ms.openlocfilehash: d185056d3bdda089eaa46ae9b8e13ab7a4354f93
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165079"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core

Przez [Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego od klienta do serwera i z serwera do klienta. Jest to przydatne w scenariuszach, gdzie fragmenty danych pojawić się wraz z upływem czasu. Podczas przesyłania strumieniowego, poszczególne fragmenty są wysyłane do klienta lub serwera tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracane metod serwera. Jest to przydatne w scenariuszach, gdzie fragmenty danych pojawić się wraz z upływem czasu. Gdy wartość zwracana jest przesyłany strumieniowo do klienta, poszczególne fragmenty są wysyłane do klienta tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.

::: moniker-end

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Konfigurowanie Centrum do przesyłania strumieniowego

::: moniker range=">= aspnetcore-3.0"

Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, lub `Task<IAsyncEnumerable<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca <xref:System.Threading.Channels.ChannelReader%601> lub `Task<ChannelReader<T>>`.

::: moniker-end

### <a name="server-to-client-streaming"></a>Przesyłanie strumieniowe serwera do klienta

::: moniker range=">= aspnetcore-3.0"

Przesyłanie strumieniowe metod koncentratora może zwrócić `IAsyncEnumerable<T>` oprócz `ChannelReader<T>`. Najprostszym sposobem, aby zwrócić `IAsyncEnumerable<T>` energii jest upewnienie metody iteratora asynchronicznej metody koncentratora, tak jak pokazano w następującym przykładzie. Metody iteratora asynchroniczne Centrum może akceptować `CancellationToken` parametr, który jest wyzwalany, gdy klient anuluje subskrypcje ze strumienia. Asynchroniczne metody iteracyjne problemom, typowe kanałów, takich jak nie zwraca `ChannelReader` wystarczająco wczesne tryb konserwacji lub wychodzenia z metodą przerywając <xref:System.Threading.Channels.ChannelWriter`1>.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

Poniższy przykład pokazuje podstawy przesyłanie strumieniowe danych do klienta za pomocą kanałów. Zawsze, gdy obiekt jest zapisywany <xref:System.Threading.Channels.ChannelWriter%601>, natychmiast wysyła obiekt do klienta. Na koniec `ChannelWriter` zakończeniu to sprawdzić klientowi strumień jest zamknięty.

> [!NOTE]
> Zapisać `ChannelWriter<T>` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe. Inne wywołania koncentratora są blokowane, aż do `ChannelReader` jest zwracana.
>
> OPAKOWYWANIE logiki `try ... catch`. Wykonaj `Channel` w `catch` i na zewnątrz `catch` się upewnić się, że Centrum wywołania metody jest wykonany prawidłowo.

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

Można zaakceptować przesyłania strumieniowego metod koncentratora klienta serwera `CancellationToken` parametr, który jest wyzwalany, gdy klient anuluje subskrypcje ze strumienia. Używanie tego tokenu, aby zatrzymać działanie serwera i zwolnić wszystkie zasoby, jeśli klient odłączy się do końca strumienia.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Klient serwer przesyłania strumieniowego

Metody koncentratora automatycznie wybrana zostaje pierwsza klient serwer przesyłania strumieniowego metody koncentratora po przyjmuje jeden lub więcej <xref:System.Threading.Channels.ChannelReader`1>s. Poniższy przykład pokazuje podstawy odczytywanie danych przesyłanych strumieniowo wysłanych z klienta. Zawsze, gdy klient zapisuje <xref:System.Threading.Channels.ChannelWriter`1>, dane są zapisywane do `ChannelReader` na serwerze, który odczytuje z metody koncentratora.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

::: moniker-end

## <a name="net-client"></a>Klient .NET

### <a name="server-to-client-streaming"></a>Przesyłanie strumieniowe serwera do klienta

`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego serwera do klienta. Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`. Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego. Element `ChannelReader<T>` jest zwracany z wywołania strumienia i reprezentuje strumienia na komputerze klienckim.

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

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Klient serwer przesyłania strumieniowego

Aby wywołać przesyłania strumieniowego metody koncentratora klienta z serwerem z klienta .NET, należy utworzyć `Channel` i przekazać `ChannelReader` jako argument do `SendAsync`, `InvokeAsync`, lub `StreamAsChannelAsync`, w zależności od wywoływane metody koncentratora.

Zawsze, gdy dane są zapisywane do `ChannelWriter`, metody koncentratora na serwerze odbiera nowy element z danymi od klienta.

Do końca strumienia, wykonaj kanału z `channel.Writer.Complete()`.

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>Klient JavaScript

### <a name="server-to-client-streaming"></a>Przesyłanie strumieniowe serwera do klienta

Klientów JavaScript wywoływać metody przesyłania strumieniowego serwera do klienta dla centrów z `connection.stream`. `stream` Metoda przyjmuje dwa argumenty:

* Nazwa metody koncentratora. W poniższym przykładzie nazwa metody koncentratora jest `Counter`.
* Argumenty zdefiniowane w metody koncentratora. W poniższym przykładzie argumenty są licznik liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.

`connection.stream` Zwraca `IStreamResult`, który zawiera `subscribe` metody. Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołań zwrotnych, aby otrzymywać powiadomienia z `stream` wywołania.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody. Wywołanie tej metody powoduje unieważnienie `CancellationToken` parametru metody koncentratora, jeśli została podana.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Klient serwer przesyłania strumieniowego

Klientów języka JavaScript wywołania klient serwer przesyłania strumieniowego metod koncentratorów, przekazując `Subject` jako argument do `send`, `invoke`, lub `stream`, w zależności od wywoływane metody koncentratora. `Subject` Jest klasa, która wygląda podobnie `Subject`. Na przykład w RxJS, można użyć [podmiotu](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) klasy w bibliotece.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

Wywoływanie `subject.next(item)` przy użyciu elementu zapisuje elementu w strumieniu i metody koncentratora otrzyma elementu na serwerze.

Aby zakończyć strumień, należy wywołać `subject.complete()`.

## <a name="java-client"></a>Klient Java

### <a name="server-to-client-streaming"></a>Przesyłanie strumieniowe serwera do klienta

Klient SignalR Java używa `stream` metody do wywołania metody przesyłania strumieniowego. `stream` akceptuje co najmniej trzech argumentów:

* Oczekiwany typ elementów strumienia.
* Nazwa metody koncentratora.
* Argumenty zdefiniowane w metody koncentratora.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

`stream` Metody `HubConnection` zwraca zauważalny typu elementu strumienia. Typ zauważalne `subscribe` metodą jest gdzie `onNext`, `onError` i `onCompleted` obsługi są zdefiniowane.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
