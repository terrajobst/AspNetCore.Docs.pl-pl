---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "Włączanie śledzenia SignalR | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ten dokument zawiera opis sposobu włączania i konfigurowania śledzenia dla SignalR serwerów i klientów. Śledzenie umożliwia przeglądanie informacji diagnostycznych dotyczących zdarzeń..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 2f01ab5d66e44cd82634f1b3df1ca6c78b7fd9d5
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
<a name="enabling-signalr-tracing"></a>Włączanie śledzenia SignalR
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten dokument zawiera opis sposobu włączania i konfigurowania śledzenia dla SignalR serwerów i klientów. Śledzenie można wyświetlić informacje diagnostyczne o zdarzeniach w aplikacji SignalR.
> 
> W tym temacie pierwotnie zapisał Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


Gdy śledzenie jest włączone, aplikacji SignalR tworzy wpisy dziennika zdarzeń. Zarówno klient, jak i serwera można rejestrować zdarzenia. Śledzenie na połączenie z serwerem dzienniki dostawcy skalowania i zdarzenia magistrali komunikatu. Śledzenie zdarzeń połączenia dzienników klienta. SignalR 2.1 i nowsze śledzenie na kliencie rejestruje pełną treść wiadomości wywołania koncentratora.

## <a name="contents"></a>Spis treści

- [Włączanie śledzenia na serwerze](#server)

    - [Rejestrowanie zdarzeń serwera plików](#server_text)
    - [Rejestrowanie zdarzeń serwera w dzienniku zdarzeń](#server_eventlog)
- [Włączanie śledzenia w kliencie programu .NET (aplikacje dla pulpitu systemu Windows)](#net_client)

    - [Rejestrowanie zdarzeń klienta w konsoli programu](#desktop_console)
    - [Rejestrowanie zdarzeń klienta do pliku tekstowego](#desktop_text)
- [Włączanie śledzenia w klientach Windows Phone 8](#phone)

    - [Rejestrowanie zdarzeń klienta Windows Phone do interfejsu użytkownika](#phone_ui)
    - [Rejestrowanie zdarzeń klienta Windows Phone do konsoli debugowania](#phone_debug)
- [Włączanie śledzenia w kliencie JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Włączanie śledzenia na serwerze

Włącz śledzenie na serwerze w pliku konfiguracji aplikacji (App.config lub Web.config, w zależności od typu projektu.) Określ, które kategorie zdarzeń, które mają być rejestrowane. W pliku konfiguracji, należy również określić, czy do rejestrowania zdarzeń do pliku tekstowego, dziennika zdarzeń systemu Windows lub dziennik niestandardowy przy użyciu implementacja [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Kategorie zdarzeń serwera obejmują następujące rodzaje komunikatów:

| Źródło | Komunikaty |
| --- | --- |
| SignalR.SqlMessageBus | Ustawienia dostawcy programu SQL magistrali komunikatów skalowania, operacji bazy danych, błąd i zdarzenia limitu czasu |
| SignalR.ServiceBusMessageBus | Tworzenie tematu dostawcy skalowania magistrali usługi i subskrypcji, błędów i zdarzeń komunikatów |
| SignalR.RedisMessageBus | Zdarzenia połączenia, rozłączenia i błąd dostawcy skalowania w poziomie redis |
| SignalR.ScaleoutMessageBus | Zdarzenia komunikatów skalowania w poziomie |
| SignalR.Transports.WebSocketTransport | Zdarzenia połączeń, rozłączenia, wiadomości i błąd transportu protokołu WebSocket |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents transportu połączenia, rozłączenia obsługi wiadomości i błąd zdarzeń |
| SignalR.Transports.ForeverFrameTransport | Zdarzenia połączeń, rozłączenia, wiadomości i błąd transportu ForeverFrame |
| SignalR.Transports.LongPollingTransport | Zdarzenia połączeń, rozłączenia, wiadomości i błąd transportu LongPolling |
| SignalR.Transports.TransportHeartBeat | Transport zdarzenia połączenia, rozłączenia i utrzymywania aktywności |
| SignalR.ReflectedHubDescriptorProvider | Centrum zdarzeń odnajdywania |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Rejestrowanie zdarzeń serwera plików

Poniższy kod przedstawia sposób włączania śledzenia dla każdej kategorii zdarzenia. Ten przykład konfiguruje aplikację do rejestrowania zdarzeń w plikach tekstowych.

**Kod XML serwera włączenie śledzenia**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

W powyższym kodzie `SignalRSwitch` wpis określa [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) używany do wysyłane do określonego dziennika zdarzeń. W takim przypadku ma ustawioną `Verbose` są rejestrowane, co oznacza wszystkie debugowania i śledzenia wiadomości.

Następujące dane wyjściowe zawiera wpisy z `transports.log.txt` pliku przy użyciu tego pliku konfiguracji aplikacji. Widoczny jest nowe połączenie, usunięte połączenie i zdarzenia Puls transportu.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Rejestrowanie zdarzeń serwera w dzienniku zdarzeń

Aby rejestrowanie zdarzeń w dzienniku zdarzeń, a nie plikiem tekstowym, zmień wartości dla pozycji w `sharedListeners` węzła. Poniższy kod przedstawia sposób rejestrowania zdarzeń serwera w dzienniku zdarzeń:

**Kod XML serwera do rejestrowania zdarzeń w dzienniku zdarzeń**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Zdarzenia są rejestrowane w dzienniku aplikacji i są dostępne w Podglądzie zdarzeń, jak pokazano poniżej:

![Wyświetlanie SignalR dzienniki Podglądu zdarzeń](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Korzystając z dziennika zdarzeń, należy ustawić **TraceLevel** do **błąd** do zachowania liczbę komunikatów o zarządzaniu.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Włączanie śledzenia w kliencie programu .NET (aplikacje dla pulpitu systemu Windows)

Klienta .NET można rejestrować zdarzenia konsoli, plik tekstowy lub dziennik niestandardowy przy użyciu implementacja [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).

Aby włączyć rejestrowanie w kliencie programu .NET, Ustaw połączenie `TraceLevel` właściwości [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) wartości i `TraceWriter` prawidłowej właściwości [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) wystąpienia.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Rejestrowanie zdarzeń klienta w konsoli programu

Poniższy kod C# pokazano, jak mają być rejestrowane zdarzenia w kliencie programu .NET do konsoli:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Rejestrowanie zdarzeń klienta do pliku tekstowego

Poniższy kod C# pokazano, jak mają być rejestrowane zdarzenia w kliencie programu .NET do pliku tekstowego:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Następujące dane wyjściowe zawiera wpisy z `ClientLog.txt` pliku przy użyciu tego pliku konfiguracji aplikacji. Zawiera klienta nawiązującego połączenie z serwerem i koncentrator wywołania metody klienta o nazwie `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Włączanie śledzenia w klientach Windows Phone 8

Aplikacji SignalR dla aplikacji Windows Phone, użyj tego samego klienta .NET jako aplikacje komputerowe, ale [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) i zapisywanie do plików z [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) nie są dostępne. Zamiast tego należy utworzyć niestandardową implementację [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) śledzenia. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Rejestrowanie zdarzeń klienta Windows Phone do interfejsu użytkownika

[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) zawiera przykładowe Windows Phone, który zapisuje dane wyjściowe śledzenia do [blok tekstu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) przy użyciu niestandardowego [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) wdrożenia o nazwie `TextBlockWriter`. Ta klasa znajduje się w **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projektu. Podczas tworzenia wystąpienia `TextBlockWriter`, należy przekazać w bieżącej [obiektu SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx), a [Panel stosu](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) których zostaną utworzone [blok tekstu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) do użycia na potrzeby śledzenia dane wyjściowe:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Następnie dane wyjściowe śledzenia będą zapisywane na nowy [blok tekstu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) utworzone w [Panel stosu](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) przekazany w:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Rejestrowanie zdarzeń klienta Windows Phone do konsoli debugowania

Aby wysłać dane wyjściowe do konsoli debugowania zamiast interfejsu użytkownika, Utwórz implementację [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) zapisuje do okna debugowania i przypisz je do tego połączenia [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) właściwości:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Informacje o śledzeniu następnie zostanie zapisany w oknie debugowania w programie Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Włączanie śledzenia w kliencie JavaScript

Aby włączyć rejestrowanie klienta połączenia, ustaw `logging` właściwości w obiekcie połączenia przed wywołaniem `start` metody do nawiązania połączenia.

**Kod języka JavaScript klienta do włączania śledzenia w konsoli przeglądarki (z wygenerowany serwer proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Kod języka JavaScript klienta do włączania śledzenia w konsoli przeglądarki (bez wygenerowany serwer proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Gdy śledzenie jest włączone, klient JavaScript rejestruje zdarzenia w konsoli przeglądarki. Aby uzyskać dostęp do konsoli przeglądarki, zobacz [monitorowania transportów](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Poniższy zrzut ekranu przedstawia klienta SignalR JavaScript z włączonym śledzeniem. Pokazuje połączenia i zdarzenia wywołania koncentratora w konsoli przeglądarki:

![Zdarzenia śledzenia SignalR w konsoli przeglądarki](enabling-signalr-tracing/_static/image4.png)
