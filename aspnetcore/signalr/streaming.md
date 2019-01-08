---
title: Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: e0d201a7ffebbbe387a874c6d788994faa2be7a5
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098808"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core

Przez [Brennan Conroy](https://github.com/BrennanConroy)

SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracane metod serwera. Jest to przydatne w scenariuszach, gdzie fragmenty danych będą pochodzić wraz z upływem czasu. Gdy wartość zwracana jest przesyłany strumieniowo do klienta, poszczególne fragmenty są wysyłane do klienta tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Konfigurowanie Centrum

Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>` lub `Task<ChannelReader<T>>`. Poniżej przedstawiono przykład przedstawiający podstawy przesyłanie strumieniowe danych do klienta. Zawsze, gdy obiekt jest zapisywany `ChannelReader` tego obiektu są natychmiast wysyłane do klienta. Na koniec `ChannelReader` zakończeniu to sprawdzić klientowi strumień jest zamknięty.

> [!NOTE]
> * Zapisać `ChannelReader` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe. Inne wywołania koncentratora będzie zablokowany do momentu `ChannelReader` jest zwracana.
> * OPAKOWYWANIE logikę w `try ... catch` i ukończyć `Channel` w catch i na zewnątrz catch, aby upewnić się, że Centrum zakończyła wywołania metody.

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

W programie ASP.NET Core 2.2 lub nowszej, przesyłanie strumieniowe metod koncentratora może akceptować `CancellationToken` parametr, który zostanie wyzwolony, gdy klient anuluje subskrypcje ze strumienia. Używanie tego tokenu, aby zatrzymać działanie serwera i zwolnić wszystkie zasoby, jeśli klient odłączy się do końca strumienia.

::: moniker-end

## <a name="net-client"></a>Klient modelu .NET

`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego. Przekaż nazwę metody koncentratora oraz argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`. Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego. Element `ChannelReader<T>` jest zwracany z wywołania usługi stream i reprezentuje strumienia na komputerze klienckim. Do odczytywania danych, typowym wzorcem jest pętli `WaitToReadAsync` i wywołać `TryRead` kiedy dane są dostępne. Pętla zakończy się po strumień został zamknięty przez serwer lub token anulowania jest przekazywany do `StreamAsChannelAsync` zostało anulowane.

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

## <a name="javascript-client"></a>Klient JavaScript

Klientów języka JavaScript pozwala wywoływać metody przesyłania strumieniowego dla koncentratorów, za pomocą `connection.stream`. `stream` Metoda przyjmuje dwa argumenty:

* Nazwa metody koncentratora. W poniższym przykładzie nazwa metody koncentratora jest `Counter`.
* Argumenty zdefiniowane w metody koncentratora. W poniższym przykładzie argumenty są: liczba, dla liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.

`connection.stream` Zwraca `IStreamResult` zawierającą `subscribe` metody. Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołań zwrotnych, aby otrzymywać powiadomienia z `stream` wywołania.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody. Wywołanie tej metody spowoduje, że `CancellationToken` parametru metody koncentratora (jeśli zostały zapewnione jest jeden) zostaną anulowane.

::: moniker-end

## <a name="related-resources"></a>Powiązane zasoby

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
