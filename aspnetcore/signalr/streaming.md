---
title: Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 3ae9b83d60019eaa3196f35645bf9b4b03f6d8c6
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325643"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Korzystanie z przesyłaniem strumieniowym w biblioteki SignalR platformy ASP.NET Core

Przez [Brennan Conroy](https://github.com/BrennanConroy)

SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracane metod serwera. Jest to przydatne w scenariuszach, gdzie fragmenty danych będą pochodzić wraz z upływem czasu. Gdy wartość zwracana jest przesyłany strumieniowo do klienta, poszczególne fragmenty są wysyłane do klienta tak szybko, jak staje się dostępny, a nie oczekuje na wszystkie dane staną się dostępne.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Konfigurowanie Centrum

Metody koncentratora automatycznie wybrana zostaje pierwsza przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>` lub `Task<ChannelReader<T>>`. Poniżej przedstawiono przykład przedstawiający podstawy przesyłanie strumieniowe danych do klienta. Zawsze, gdy obiekt jest zapisywany `ChannelReader` tego obiektu są natychmiast wysyłane do klienta. Na koniec `ChannelReader` zakończeniu to sprawdzić klientowi strumień jest zamknięty.

> [!NOTE]
> Zapisać `ChannelReader` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe. Inne wywołania koncentratora będzie zablokowany do momentu `ChannelReader` jest zwracana.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>Klient modelu .NET

`StreamAsChannelAsync` Metody `HubConnection` służy do wywoływania metody przesyłania strumieniowego. Przekaż nazwę metody koncentratora oraz argumenty zdefiniowane w metody koncentratora `StreamAsChannelAsync`. Parametr generyczny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego. Element `ChannelReader<T>` jest zwracany z wywołania usługi stream i reprezentuje strumienia na komputerze klienckim. Do odczytywania danych, typowym wzorcem jest pętli `WaitToReadAsync` i wywołać `TryRead` kiedy dane są dostępne. Pętla zakończy się po strumień został zamknięty przez serwer lub token anulowania jest przekazywany do `StreamAsChannelAsync` zostało anulowane.

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

## <a name="javascript-client"></a>Klient JavaScript

Klientów języka JavaScript pozwala wywoływać metody przesyłania strumieniowego dla koncentratorów, za pomocą `connection.stream`. `stream` Metoda przyjmuje dwa argumenty:

* Nazwa metody koncentratora. W poniższym przykładzie nazwa metody koncentratora jest `Counter`.
* Argumenty zdefiniowane w metody koncentratora. W poniższym przykładzie argumenty są: liczba, dla liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.

`connection.stream` Zwraca `IStreamResult` zawierającą `subscribe` metody. Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołań zwrotnych, aby otrzymywać powiadomienia z `stream` wywołania.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Aby zakończyć strumień od klienta, należy wywołać `dispose` metody `ISubscription` zwrócone z `subscribe` metody.

## <a name="related-resources"></a>Powiązane zasoby

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
