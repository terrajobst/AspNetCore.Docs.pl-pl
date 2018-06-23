---
title: Użyj przesyłania strumieniowego w ASP.NET Core SignalR
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 08ddea4fb83150bab27a9e2685c75ff34565606b
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327496"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Użyj przesyłania strumieniowego w ASP.NET Core SignalR

Przez [Brennan Conroy](https://github.com/BrennanConroy)

SignalR platformy ASP.NET Core obsługuje przesyłania strumieniowego wartości zwracanych metody serwera. Jest to przydatne w scenariuszach, skąd pochodzą będzie fragmentów danych czasie. Gdy wartością zwracaną jest przesyłane strumieniowo do klienta, każdego fragmentu są wysyłane do klienta jak staje się dostępna, zamiast oczekiwania dla wszystkich danych stanie się dostępne.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Konfigurowanie Centrum

Metody koncentratora automatycznie staje się przesyłania strumieniowego metody koncentratora po zwraca `ChannelReader<T>` lub `Task<ChannelReader<T>>`. Poniżej przedstawiono przykładowe, która przedstawia podstawy strumieniowego przesyłania danych do klienta. Zawsze, gdy obiekt jest zapisywany w `ChannelReader` tego obiektu jest natychmiast wysyłane do klienta. Na koniec `ChannelReader` zakończeniu mówić klienta strumień jest zamknięty.

> [!NOTE]
> Zapis do `ChannelReader` w wątku w tle i wróć `ChannelReader` tak szybko, jak to możliwe. Inne wywołania koncentratora będzie zablokowany do momentu `ChannelReader` jest zwracany.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>Klient .NET

`StreamAsChannelAsync` Metoda `HubConnection` służy do wywoływania metody przesyłania strumieniowego. Przekaż nazwę metody koncentratora i argumentów w metodzie koncentratora do `StreamAsChannelAsync`. Parametr ogólny na `StreamAsChannelAsync<T>` Określa typ obiektów zwróconych przez metodę przesyłania strumieniowego. A `ChannelReader<T>` jest zwracany z wywołania strumienia i reprezentuje strumienia na kliencie. Do odczytywania danych, ma pętlę wspólnego wzorca `WaitToReadAsync` i Wywołaj `TryRead` kiedy dane są dostępne. Pętla zakończy strumień został zamknięty przez serwer lub przekazany token anulowania do `StreamAsChannelAsync` zostało anulowane.

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

## <a name="javascript-client"></a>JavaScript klienta

Klientów języka JavaScript wywoływać metod przesyłania strumieniowego na koncentratory przy użyciu `connection.stream`. `stream` Metoda przyjmuje dwa argumenty:

* Nazwa metody koncentratora. W poniższym przykładzie nazwa metody koncentratora jest `Counter`.
* Argumenty w metodzie koncentratora. W poniższym przykładzie argumentami są: liczba liczby elementów strumienia do odbierania i opóźnienie między elementami strumienia.

`connection.stream` Zwraca `IStreamResult` zawierającą `subscribe` metody. Przekaż `IStreamSubscriber` do `subscribe` i ustaw `next`, `error`, i `complete` wywołania zwrotne otrzymywać powiadomień z `stream` wywołania.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Na koniec strumienia z wywołania klienta `dispose` metoda `ISubscription` zwracanego z `subscribe` metody.

## <a name="related-resources"></a>Zasoby pokrewne

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
