---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Włączanie śledzenia SignalR | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym dokumencie opisano, jak włączyć i skonfigurować śledzenie SignalR serwerów i klientów. Śledzenie umożliwia wyświetlanie informacji diagnostycznych dotyczących zdarzeń...
ms.author: aspnetcontent
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 3bc8e4c81a6efa1b02d2b3e633ddb92c8ff6c2e3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809200"
---
<a name="enabling-signalr-tracing"></a>Włączanie śledzenia SignalR
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym dokumencie opisano, jak włączyć i skonfigurować śledzenie SignalR serwerów i klientów. Śledzenie umożliwia wyświetlanie informacji diagnostycznych o zdarzeniach w aplikacji SignalR.
> 
> W tym temacie pierwotnie został napisany przez Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
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
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


Po włączeniu funkcji śledzenia aplikacji SignalR tworzy wpisy dziennika zdarzeń. Można rejestrować zdarzenia z zarówno klient, jak i z serwera. Śledzenie na połączenie z serwerem dzienniki dostawcy skalowania i komunikatów bus zdarzenia. Śledzenie zdarzeń połączenia dzienniki klienta. SignalR 2.1 i nowsze śledzenia na kliencie rejestruje pełnej zawartości wiadomości wywołania koncentratora.

## <a name="contents"></a>Spis treści

- [Włączanie śledzenia na serwerze](#server)

    - [Rejestrowanie zdarzeń serwera w plikach tekstowych](#server_text)
    - [Rejestrowanie zdarzeń serwera do dziennika zdarzeń](#server_eventlog)
- [Włączanie śledzenia w kliencie programu .NET (aplikacje pulpitu Windows)](#net_client)

    - [Rejestrowanie zdarzeń klienta stacjonarnego do konsoli](#desktop_console)
    - [Rejestrowanie zdarzeń klienta stacjonarnego do pliku tekstowego](#desktop_text)
- [Włączanie śledzenia w klientach Windows Phone 8](#phone)

    - [Rejestrowanie zdarzeń klienta Windows Phone w interfejsie użytkownika](#phone_ui)
    - [Rejestrowanie zdarzeń klienta Windows Phone do konsoli debugowania](#phone_debug)
- [Włączanie śledzenia w klient JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Włączanie śledzenia na serwerze

Włącz śledzenie na serwerze w pliku konfiguracji aplikacji (App.config lub Web.config, w zależności od typu projektu.) Należy określić, które kategorie zdarzeń, które mają być rejestrowane. W pliku konfiguracji, można również określić, czy do rejestrowania zdarzeń do pliku tekstowego, w dzienniku zdarzeń Windows lub dziennika niestandardowego przy użyciu implementacji [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Kategorie zdarzeń serwera obejmują następujące rodzaje komunikatów:

| Źródło | Komunikaty |
| --- | --- |
| SignalR.SqlMessageBus | Instalacji dostawcy usługi SQL magistrali komunikatów skalowania w poziomie, operacji bazy danych, błędów i zdarzeń limitu czasu |
| SignalR.ServiceBusMessageBus | Tworzenie tematu dostawcy usługi Service bus ze skalowaniem i subskrypcji, błędów i zdarzeń komunikatów |
| SignalR.RedisMessageBus | Zdarzenia połączeń, rozłączenia i błąd dostawcy skalowania w poziomie redis |
| SignalR.ScaleoutMessageBus | Zdarzenia przesyłania komunikatów skalowania w poziomie |
| SignalR.Transports.WebSocketTransport | Zdarzenia połączeń, rozłączenia, wiadomości błyskawiczne i błąd transportu WebSocket |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents transportu zdarzenia połączenia, rozłączenia, wiadomości błyskawiczne i błędów |
| SignalR.Transports.ForeverFrameTransport | Zdarzenia połączeń, rozłączenia, wiadomości błyskawiczne i błąd transportu ForeverFrame |
| SignalR.Transports.LongPollingTransport | Zdarzenia połączeń, rozłączenia, wiadomości błyskawiczne i błąd transportu LongPolling |
| SignalR.Transports.TransportHeartBeat | Transportu zdarzenia połączenia, rozłączenia i utrzymywania aktywności |
| SignalR.ReflectedHubDescriptorProvider | Centrum zdarzeń usługi odnajdywania |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Rejestrowanie zdarzeń serwera w plikach tekstowych

Poniższy kod przedstawia sposób włączania śledzenia dla każdej kategorii zdarzenia. Ten przykład umożliwia skonfigurowanie aplikacji do rejestrowania zdarzeń w plikach tekstowych.

**Włączanie śledzenia kodu serwera XML**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

W powyższym kodzie `SignalRSwitch` wpis określa [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) używany do zdarzeń wysyłanych do określonego dziennika. W takich przypadkach jest równa `Verbose` oznacza wszystkie debugowania i śledzenia komunikaty są rejestrowane.

Następujące dane wyjściowe zawierają wpisy z `transports.log.txt` pliku dla aplikacji przy użyciu powyższych pliku konfiguracji. Pokazuje nowe połączenie, usunięte połączenie i zdarzenia pulsu transportu.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Rejestrowanie zdarzeń serwera do dziennika zdarzeń

Aby rejestrować zdarzenia w dzienniku zdarzeń, a nie plikiem tekstowym, zmienić wartości wpisów w `sharedListeners` węzła. Poniższy kod przedstawia sposób rejestrowania serwera zdarzeń w dzienniku zdarzeń:

**Kod serwera XML do rejestrowania zdarzeń w dzienniku zdarzeń**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Zdarzenia są rejestrowane w dzienniku aplikacji i są dostępne za pomocą Podglądu zdarzeń, jak pokazano poniżej:

![Podgląd zdarzeń, wyświetlanie dzienników SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Korzystając z dziennika zdarzeń, należy ustawić **TraceLevel** do **błąd** zapewnienie liczbę komunikatów w zarządzaniu.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Włączanie śledzenia w kliencie programu .NET (aplikacje pulpitu Windows)

Klient modelu .NET można rejestrować zdarzenia w konsoli, plikiem tekstowym lub dziennik niestandardowy przy użyciu implementacji [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Aby włączyć rejestrowanie w kliencie programu .NET, należy ustawić połączenie `TraceLevel` właściwości [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) wartości i `TraceWriter` właściwości do prawidłowego [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) wystąpienia.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Rejestrowanie zdarzeń klienta stacjonarnego do konsoli

Poniższy kod C# pokazuje, jak rejestrować zdarzenia w klienta .NET do konsoli:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Rejestrowanie zdarzeń klienta stacjonarnego do pliku tekstowego

Poniższy kod C# pokazuje, jak rejestrować zdarzenia w klienta .NET do pliku tekstowego:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Następujące dane wyjściowe zawierają wpisy z `ClientLog.txt` pliku dla aplikacji przy użyciu powyższych pliku konfiguracji. Pokazuje klienta nawiązującego połączenie z serwerem i Centrum wywołania metody klienta o nazwie `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Włączanie śledzenia w klientach Windows Phone 8

Aplikacji SignalR dla aplikacji Windows Phone używać tego samego klienta .NET jako aplikacje komputerowe, ale [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) oraz zapisywanie do pliku za pomocą [StreamWriter —](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nie są dostępne. Zamiast tego należy utworzyć niestandardową implementację [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) śledzenia. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Rejestrowanie zdarzeń klienta Windows Phone w interfejsie użytkownika

[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) przedstawiono przykładowe Windows Phone, która zapisuje dane wyjściowe śledzenia [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) przy użyciu niestandardowego [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) wdrożenia o nazwie `TextBlockWriter`. Ta klasa znajduje się w **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projektu. Podczas tworzenia wystąpienia obiektu `TextBlockWriter`, przekazać w bieżącym [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)i [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) gdzie utworzy [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) na potrzeby śledzenia dane wyjściowe:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Dane wyjściowe śledzenia będą następnie zapisywane w nową [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) utworzone w [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) przekazywane w:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Rejestrowanie zdarzeń klienta Windows Phone do konsoli debugowania

Aby wysłać dane wyjściowe do konsoli debugowania, a nie w Interfejsie użytkownika, należy utworzyć implementację [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) zapisuje okna debugowania i przypisać ją do tego połączenia [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) właściwości:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Informacje o śledzeniu będą następnie zapisywane do okna debugowania w programie Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Włączanie śledzenia w klient JavaScript

Aby włączyć połączenia logowania po stronie klienta, należy ustawić `logging` właściwości w obiekcie połączenia przed wywołaniem `start` metodę, aby nawiązać połączenie.

**Kod języka JavaScript klienta umożliwiające śledzenie z konsolą przeglądarki (z wygenerowany serwer proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Kod języka JavaScript klienta umożliwiające śledzenie z konsolą przeglądarki (bez wygenerowany serwer proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Gdy śledzenie jest włączone, klient JavaScript rejestruje zdarzenia w konsoli przeglądarki. Aby uzyskać dostęp do konsoli przeglądarki, zobacz [monitorowania transportów](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Poniższy zrzut ekranu przedstawia klienta SignalR JavaScript z włączonym śledzeniem. Pokazuje połączenia i Centrum zdarzeń usługi wywołania w konsoli przeglądarki:

![Zdarzenia śledzenia SignalR w konsoli przeglądarki](enabling-signalr-tracing/_static/image4.png)
