---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Włączanie śledzenia SignalR | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym dokumencie opisano, jak włączyć i skonfigurować śledzenie SignalR serwerów i klientów. Śledzenie umożliwia wyświetlanie informacji diagnostycznych dotyczących zdarzeń...
ms.author: riande
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 733163699f83cbf68d6d5a27e86d208c3922272c
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287666"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="142ad-104">Włączanie śledzenia usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="142ad-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="142ad-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="142ad-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="142ad-106">W tym dokumencie opisano, jak włączyć i skonfigurować śledzenie SignalR serwerów i klientów.</span><span class="sxs-lookup"><span data-stu-id="142ad-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="142ad-107">Śledzenie umożliwia wyświetlanie informacji diagnostycznych o zdarzeniach w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="142ad-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="142ad-108">W tym temacie pierwotnie został napisany przez Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="142ad-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="142ad-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="142ad-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="142ad-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="142ad-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="142ad-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="142ad-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="142ad-112">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="142ad-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="142ad-113">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="142ad-113">Questions and comments</span></span>
>
> <span data-ttu-id="142ad-114">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="142ad-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="142ad-115">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="142ad-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="142ad-116">Po włączeniu funkcji śledzenia aplikacji SignalR tworzy wpisy dziennika zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="142ad-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="142ad-117">Można rejestrować zdarzenia z zarówno klient, jak i z serwera.</span><span class="sxs-lookup"><span data-stu-id="142ad-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="142ad-118">Śledzenie na połączenie z serwerem dzienniki dostawcy skalowania i komunikatów bus zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="142ad-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="142ad-119">Śledzenie zdarzeń połączenia dzienniki klienta.</span><span class="sxs-lookup"><span data-stu-id="142ad-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="142ad-120">SignalR 2.1 i nowsze śledzenia na kliencie rejestruje pełnej zawartości wiadomości wywołania koncentratora.</span><span class="sxs-lookup"><span data-stu-id="142ad-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="142ad-121">Spis treści</span><span class="sxs-lookup"><span data-stu-id="142ad-121">Contents</span></span>

- [<span data-ttu-id="142ad-122">Włączanie śledzenia na serwerze</span><span class="sxs-lookup"><span data-stu-id="142ad-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="142ad-123">Rejestrowanie zdarzeń serwera w plikach tekstowych</span><span class="sxs-lookup"><span data-stu-id="142ad-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="142ad-124">Rejestrowanie zdarzeń serwera do dziennika zdarzeń</span><span class="sxs-lookup"><span data-stu-id="142ad-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="142ad-125">Włączanie śledzenia w kliencie programu .NET (aplikacje pulpitu Windows)</span><span class="sxs-lookup"><span data-stu-id="142ad-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="142ad-126">Rejestrowanie zdarzeń klienta stacjonarnego do konsoli</span><span class="sxs-lookup"><span data-stu-id="142ad-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="142ad-127">Rejestrowanie zdarzeń klienta stacjonarnego do pliku tekstowego</span><span class="sxs-lookup"><span data-stu-id="142ad-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="142ad-128">Włączanie śledzenia w klientach Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="142ad-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="142ad-129">Rejestrowanie zdarzeń klienta Windows Phone w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="142ad-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="142ad-130">Rejestrowanie zdarzeń klienta Windows Phone do konsoli debugowania</span><span class="sxs-lookup"><span data-stu-id="142ad-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="142ad-131">Włączanie śledzenia w klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="142ad-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="142ad-132">Włączanie śledzenia na serwerze</span><span class="sxs-lookup"><span data-stu-id="142ad-132">Enabling tracing on the server</span></span>

<span data-ttu-id="142ad-133">Włącz śledzenie na serwerze w pliku konfiguracji aplikacji (App.config lub Web.config, w zależności od typu projektu.) Należy określić, które kategorie zdarzeń, które mają być rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="142ad-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="142ad-134">W pliku konfiguracji, można również określić, czy do rejestrowania zdarzeń do pliku tekstowego, w dzienniku zdarzeń Windows lub dziennika niestandardowego przy użyciu implementacji [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="142ad-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="142ad-135">Kategorie zdarzeń serwera obejmują następujące rodzaje komunikatów:</span><span class="sxs-lookup"><span data-stu-id="142ad-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="142ad-136">Źródło</span><span class="sxs-lookup"><span data-stu-id="142ad-136">Source</span></span> | <span data-ttu-id="142ad-137">Komunikaty</span><span class="sxs-lookup"><span data-stu-id="142ad-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="142ad-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="142ad-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="142ad-139">Instalacji dostawcy usługi SQL magistrali komunikatów skalowania w poziomie, operacji bazy danych, błędów i zdarzeń limitu czasu</span><span class="sxs-lookup"><span data-stu-id="142ad-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="142ad-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="142ad-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="142ad-141">Tworzenie tematu dostawcy usługi Service bus ze skalowaniem i subskrypcji, błędów i zdarzeń komunikatów</span><span class="sxs-lookup"><span data-stu-id="142ad-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="142ad-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="142ad-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="142ad-143">Zdarzenia połączeń, rozłączenia i błąd dostawcy skalowania w poziomie redis</span><span class="sxs-lookup"><span data-stu-id="142ad-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="142ad-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="142ad-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="142ad-145">Zdarzenia przesyłania komunikatów skalowania w poziomie</span><span class="sxs-lookup"><span data-stu-id="142ad-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="142ad-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="142ad-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="142ad-147">Zdarzenia połączeń, rozłączenia, wiadomości błyskawiczne i błąd transportu WebSocket</span><span class="sxs-lookup"><span data-stu-id="142ad-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="142ad-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="142ad-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="142ad-149">ServerSentEvents transportu zdarzenia połączenia, rozłączenia, wiadomości błyskawiczne i błędów</span><span class="sxs-lookup"><span data-stu-id="142ad-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="142ad-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="142ad-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="142ad-151">Zdarzenia połączeń, rozłączenia, wiadomości błyskawiczne i błąd transportu ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="142ad-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="142ad-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="142ad-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="142ad-153">Zdarzenia połączeń, rozłączenia, wiadomości błyskawiczne i błąd transportu LongPolling</span><span class="sxs-lookup"><span data-stu-id="142ad-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="142ad-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="142ad-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="142ad-155">Transportu zdarzenia połączenia, rozłączenia i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="142ad-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="142ad-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="142ad-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="142ad-157">Centrum zdarzeń usługi odnajdywania</span><span class="sxs-lookup"><span data-stu-id="142ad-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="142ad-158">Rejestrowanie zdarzeń serwera w plikach tekstowych</span><span class="sxs-lookup"><span data-stu-id="142ad-158">Logging server events to text files</span></span>

<span data-ttu-id="142ad-159">Poniższy kod przedstawia sposób włączania śledzenia dla każdej kategorii zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="142ad-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="142ad-160">Ten przykład umożliwia skonfigurowanie aplikacji do rejestrowania zdarzeń w plikach tekstowych.</span><span class="sxs-lookup"><span data-stu-id="142ad-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="142ad-161">**Włączanie śledzenia kodu serwera XML**</span><span class="sxs-lookup"><span data-stu-id="142ad-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="142ad-162">W powyższym kodzie `SignalRSwitch` wpis określa [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) używany do zdarzeń wysyłanych do określonego dziennika.</span><span class="sxs-lookup"><span data-stu-id="142ad-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="142ad-163">W takich przypadkach jest równa `Verbose` oznacza wszystkie debugowania i śledzenia komunikaty są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="142ad-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="142ad-164">Następujące dane wyjściowe zawierają wpisy z `transports.log.txt` pliku dla aplikacji przy użyciu powyższych pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="142ad-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="142ad-165">Pokazuje nowe połączenie, usunięte połączenie i zdarzenia pulsu transportu.</span><span class="sxs-lookup"><span data-stu-id="142ad-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="142ad-166">Rejestrowanie zdarzeń serwera do dziennika zdarzeń</span><span class="sxs-lookup"><span data-stu-id="142ad-166">Logging server events to the event log</span></span>

<span data-ttu-id="142ad-167">Aby rejestrować zdarzenia w dzienniku zdarzeń, a nie plikiem tekstowym, zmienić wartości wpisów w `sharedListeners` węzła.</span><span class="sxs-lookup"><span data-stu-id="142ad-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="142ad-168">Poniższy kod przedstawia sposób rejestrowania serwera zdarzeń w dzienniku zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="142ad-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="142ad-169">**Kod serwera XML do rejestrowania zdarzeń w dzienniku zdarzeń**</span><span class="sxs-lookup"><span data-stu-id="142ad-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="142ad-170">Zdarzenia są rejestrowane w dzienniku aplikacji i są dostępne za pomocą Podglądu zdarzeń, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="142ad-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Podgląd zdarzeń, wyświetlanie dzienników SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="142ad-172">Korzystając z dziennika zdarzeń, należy ustawić **TraceLevel** do **błąd** zapewnienie liczbę komunikatów w zarządzaniu.</span><span class="sxs-lookup"><span data-stu-id="142ad-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="142ad-173">Włączanie śledzenia w kliencie programu .NET (aplikacje pulpitu Windows)</span><span class="sxs-lookup"><span data-stu-id="142ad-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="142ad-174">Klient modelu .NET można rejestrować zdarzenia w konsoli, plikiem tekstowym lub dziennik niestandardowy przy użyciu implementacji [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="142ad-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="142ad-175">Aby włączyć rejestrowanie w kliencie programu .NET, należy ustawić połączenie `TraceLevel` właściwości [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) wartości i `TraceWriter` właściwości do prawidłowego [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="142ad-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="142ad-176">Rejestrowanie zdarzeń klienta stacjonarnego do konsoli</span><span class="sxs-lookup"><span data-stu-id="142ad-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="142ad-177">Poniższy kod C# pokazuje, jak rejestrować zdarzenia w klienta .NET do konsoli:</span><span class="sxs-lookup"><span data-stu-id="142ad-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="142ad-178">Rejestrowanie zdarzeń klienta stacjonarnego do pliku tekstowego</span><span class="sxs-lookup"><span data-stu-id="142ad-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="142ad-179">Poniższy kod C# pokazuje, jak rejestrować zdarzenia w klienta .NET do pliku tekstowego:</span><span class="sxs-lookup"><span data-stu-id="142ad-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="142ad-180">Następujące dane wyjściowe zawierają wpisy z `ClientLog.txt` pliku dla aplikacji przy użyciu powyższych pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="142ad-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="142ad-181">Pokazuje klienta nawiązującego połączenie z serwerem i Centrum wywołania metody klienta o nazwie `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="142ad-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="142ad-182">Włączanie śledzenia w klientach Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="142ad-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="142ad-183">Aplikacji SignalR dla aplikacji Windows Phone używać tego samego klienta .NET jako aplikacje komputerowe, ale [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) oraz zapisywanie do pliku za pomocą [StreamWriter —](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="142ad-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="142ad-184">Zamiast tego należy utworzyć niestandardową implementację [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) śledzenia.</span><span class="sxs-lookup"><span data-stu-id="142ad-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="142ad-185">Rejestrowanie zdarzeń klienta Windows Phone w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="142ad-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="142ad-186">[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) przedstawiono przykładowe Windows Phone, która zapisuje dane wyjściowe śledzenia [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) przy użyciu niestandardowego [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) wdrożenia o nazwie `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="142ad-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="142ad-187">Ta klasa znajduje się w **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projektu.</span><span class="sxs-lookup"><span data-stu-id="142ad-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="142ad-188">Podczas tworzenia wystąpienia obiektu `TextBlockWriter`, przekazać w bieżącym [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)i [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) gdzie utworzy [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) na potrzeby śledzenia dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="142ad-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="142ad-189">Dane wyjściowe śledzenia będą następnie zapisywane w nową [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) utworzone w [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) przekazywane w:</span><span class="sxs-lookup"><span data-stu-id="142ad-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="142ad-190">Rejestrowanie zdarzeń klienta Windows Phone do konsoli debugowania</span><span class="sxs-lookup"><span data-stu-id="142ad-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="142ad-191">Aby wysłać dane wyjściowe do konsoli debugowania, a nie w Interfejsie użytkownika, należy utworzyć implementację [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) zapisuje okna debugowania i przypisać ją do tego połączenia [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) właściwości:</span><span class="sxs-lookup"><span data-stu-id="142ad-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="142ad-192">Informacje o śledzeniu będą następnie zapisywane do okna debugowania w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="142ad-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="142ad-193">Włączanie śledzenia w klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="142ad-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="142ad-194">Aby włączyć połączenia logowania po stronie klienta, należy ustawić `logging` właściwości w obiekcie połączenia przed wywołaniem `start` metodę, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="142ad-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="142ad-195">**Kod języka JavaScript klienta umożliwiające śledzenie z konsolą przeglądarki (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="142ad-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="142ad-196">**Kod języka JavaScript klienta umożliwiające śledzenie z konsolą przeglądarki (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="142ad-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="142ad-197">Gdy śledzenie jest włączone, klient JavaScript rejestruje zdarzenia w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="142ad-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="142ad-198">Aby uzyskać dostęp do konsoli przeglądarki, zobacz [monitorowania transportów](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="142ad-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="142ad-199">Poniższy zrzut ekranu przedstawia klienta SignalR JavaScript z włączonym śledzeniem.</span><span class="sxs-lookup"><span data-stu-id="142ad-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="142ad-200">Pokazuje połączenia i Centrum zdarzeń usługi wywołania w konsoli przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="142ad-200">It shows connection and hub invocation events in the browser console:</span></span>

![Zdarzenia śledzenia SignalR w konsoli przeglądarki](enabling-signalr-tracing/_static/image4.png)
