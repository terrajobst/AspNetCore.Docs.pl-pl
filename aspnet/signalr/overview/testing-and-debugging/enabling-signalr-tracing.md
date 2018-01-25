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
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="66a12-104">Włączanie śledzenia SignalR</span><span class="sxs-lookup"><span data-stu-id="66a12-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="66a12-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="66a12-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="66a12-106">Ten dokument zawiera opis sposobu włączania i konfigurowania śledzenia dla SignalR serwerów i klientów.</span><span class="sxs-lookup"><span data-stu-id="66a12-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="66a12-107">Śledzenie można wyświetlić informacje diagnostyczne o zdarzeniach w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="66a12-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="66a12-108">W tym temacie pierwotnie zapisał Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="66a12-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="66a12-109">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="66a12-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="66a12-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="66a12-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="66a12-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="66a12-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="66a12-112">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="66a12-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="66a12-113">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="66a12-113">Questions and comments</span></span>
> 
> <span data-ttu-id="66a12-114">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="66a12-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="66a12-115">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="66a12-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="66a12-116">Gdy śledzenie jest włączone, aplikacji SignalR tworzy wpisy dziennika zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="66a12-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="66a12-117">Zarówno klient, jak i serwera można rejestrować zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="66a12-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="66a12-118">Śledzenie na połączenie z serwerem dzienniki dostawcy skalowania i zdarzenia magistrali komunikatu.</span><span class="sxs-lookup"><span data-stu-id="66a12-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="66a12-119">Śledzenie zdarzeń połączenia dzienników klienta.</span><span class="sxs-lookup"><span data-stu-id="66a12-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="66a12-120">SignalR 2.1 i nowsze śledzenie na kliencie rejestruje pełną treść wiadomości wywołania koncentratora.</span><span class="sxs-lookup"><span data-stu-id="66a12-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="66a12-121">Spis treści</span><span class="sxs-lookup"><span data-stu-id="66a12-121">Contents</span></span>

- [<span data-ttu-id="66a12-122">Włączanie śledzenia na serwerze</span><span class="sxs-lookup"><span data-stu-id="66a12-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="66a12-123">Rejestrowanie zdarzeń serwera plików</span><span class="sxs-lookup"><span data-stu-id="66a12-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="66a12-124">Rejestrowanie zdarzeń serwera w dzienniku zdarzeń</span><span class="sxs-lookup"><span data-stu-id="66a12-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="66a12-125">Włączanie śledzenia w kliencie programu .NET (aplikacje dla pulpitu systemu Windows)</span><span class="sxs-lookup"><span data-stu-id="66a12-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="66a12-126">Rejestrowanie zdarzeń klienta w konsoli programu</span><span class="sxs-lookup"><span data-stu-id="66a12-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="66a12-127">Rejestrowanie zdarzeń klienta do pliku tekstowego</span><span class="sxs-lookup"><span data-stu-id="66a12-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="66a12-128">Włączanie śledzenia w klientach Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="66a12-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="66a12-129">Rejestrowanie zdarzeń klienta Windows Phone do interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="66a12-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="66a12-130">Rejestrowanie zdarzeń klienta Windows Phone do konsoli debugowania</span><span class="sxs-lookup"><span data-stu-id="66a12-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="66a12-131">Włączanie śledzenia w kliencie JavaScript</span><span class="sxs-lookup"><span data-stu-id="66a12-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="66a12-132">Włączanie śledzenia na serwerze</span><span class="sxs-lookup"><span data-stu-id="66a12-132">Enabling tracing on the server</span></span>

<span data-ttu-id="66a12-133">Włącz śledzenie na serwerze w pliku konfiguracji aplikacji (App.config lub Web.config, w zależności od typu projektu.) Określ, które kategorie zdarzeń, które mają być rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="66a12-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="66a12-134">W pliku konfiguracji, należy również określić, czy do rejestrowania zdarzeń do pliku tekstowego, dziennika zdarzeń systemu Windows lub dziennik niestandardowy przy użyciu implementacja [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="66a12-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="66a12-135">Kategorie zdarzeń serwera obejmują następujące rodzaje komunikatów:</span><span class="sxs-lookup"><span data-stu-id="66a12-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="66a12-136">Źródło</span><span class="sxs-lookup"><span data-stu-id="66a12-136">Source</span></span> | <span data-ttu-id="66a12-137">Komunikaty</span><span class="sxs-lookup"><span data-stu-id="66a12-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="66a12-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="66a12-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="66a12-139">Ustawienia dostawcy programu SQL magistrali komunikatów skalowania, operacji bazy danych, błąd i zdarzenia limitu czasu</span><span class="sxs-lookup"><span data-stu-id="66a12-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="66a12-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="66a12-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="66a12-141">Tworzenie tematu dostawcy skalowania magistrali usługi i subskrypcji, błędów i zdarzeń komunikatów</span><span class="sxs-lookup"><span data-stu-id="66a12-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="66a12-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="66a12-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="66a12-143">Zdarzenia połączenia, rozłączenia i błąd dostawcy skalowania w poziomie redis</span><span class="sxs-lookup"><span data-stu-id="66a12-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="66a12-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="66a12-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="66a12-145">Zdarzenia komunikatów skalowania w poziomie</span><span class="sxs-lookup"><span data-stu-id="66a12-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="66a12-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="66a12-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="66a12-147">Zdarzenia połączeń, rozłączenia, wiadomości i błąd transportu protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="66a12-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="66a12-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="66a12-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="66a12-149">ServerSentEvents transportu połączenia, rozłączenia obsługi wiadomości i błąd zdarzeń</span><span class="sxs-lookup"><span data-stu-id="66a12-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="66a12-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="66a12-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="66a12-151">Zdarzenia połączeń, rozłączenia, wiadomości i błąd transportu ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="66a12-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="66a12-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="66a12-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="66a12-153">Zdarzenia połączeń, rozłączenia, wiadomości i błąd transportu LongPolling</span><span class="sxs-lookup"><span data-stu-id="66a12-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="66a12-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="66a12-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="66a12-155">Transport zdarzenia połączenia, rozłączenia i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="66a12-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="66a12-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="66a12-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="66a12-157">Centrum zdarzeń odnajdywania</span><span class="sxs-lookup"><span data-stu-id="66a12-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="66a12-158">Rejestrowanie zdarzeń serwera plików</span><span class="sxs-lookup"><span data-stu-id="66a12-158">Logging server events to text files</span></span>

<span data-ttu-id="66a12-159">Poniższy kod przedstawia sposób włączania śledzenia dla każdej kategorii zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="66a12-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="66a12-160">Ten przykład konfiguruje aplikację do rejestrowania zdarzeń w plikach tekstowych.</span><span class="sxs-lookup"><span data-stu-id="66a12-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="66a12-161">**Kod XML serwera włączenie śledzenia**</span><span class="sxs-lookup"><span data-stu-id="66a12-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="66a12-162">W powyższym kodzie `SignalRSwitch` wpis określa [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) używany do wysyłane do określonego dziennika zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="66a12-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="66a12-163">W takim przypadku ma ustawioną `Verbose` są rejestrowane, co oznacza wszystkie debugowania i śledzenia wiadomości.</span><span class="sxs-lookup"><span data-stu-id="66a12-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="66a12-164">Następujące dane wyjściowe zawiera wpisy z `transports.log.txt` pliku przy użyciu tego pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="66a12-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="66a12-165">Widoczny jest nowe połączenie, usunięte połączenie i zdarzenia Puls transportu.</span><span class="sxs-lookup"><span data-stu-id="66a12-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="66a12-166">Rejestrowanie zdarzeń serwera w dzienniku zdarzeń</span><span class="sxs-lookup"><span data-stu-id="66a12-166">Logging server events to the event log</span></span>

<span data-ttu-id="66a12-167">Aby rejestrowanie zdarzeń w dzienniku zdarzeń, a nie plikiem tekstowym, zmień wartości dla pozycji w `sharedListeners` węzła.</span><span class="sxs-lookup"><span data-stu-id="66a12-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="66a12-168">Poniższy kod przedstawia sposób rejestrowania zdarzeń serwera w dzienniku zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="66a12-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="66a12-169">**Kod XML serwera do rejestrowania zdarzeń w dzienniku zdarzeń**</span><span class="sxs-lookup"><span data-stu-id="66a12-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="66a12-170">Zdarzenia są rejestrowane w dzienniku aplikacji i są dostępne w Podglądzie zdarzeń, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="66a12-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Wyświetlanie SignalR dzienniki Podglądu zdarzeń](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="66a12-172">Korzystając z dziennika zdarzeń, należy ustawić **TraceLevel** do **błąd** do zachowania liczbę komunikatów o zarządzaniu.</span><span class="sxs-lookup"><span data-stu-id="66a12-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="66a12-173">Włączanie śledzenia w kliencie programu .NET (aplikacje dla pulpitu systemu Windows)</span><span class="sxs-lookup"><span data-stu-id="66a12-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="66a12-174">Klienta .NET można rejestrować zdarzenia konsoli, plik tekstowy lub dziennik niestandardowy przy użyciu implementacja [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="66a12-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="66a12-175">Aby włączyć rejestrowanie w kliencie programu .NET, Ustaw połączenie `TraceLevel` właściwości [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) wartości i `TraceWriter` prawidłowej właściwości [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="66a12-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="66a12-176">Rejestrowanie zdarzeń klienta w konsoli programu</span><span class="sxs-lookup"><span data-stu-id="66a12-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="66a12-177">Poniższy kod C# pokazano, jak mają być rejestrowane zdarzenia w kliencie programu .NET do konsoli:</span><span class="sxs-lookup"><span data-stu-id="66a12-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="66a12-178">Rejestrowanie zdarzeń klienta do pliku tekstowego</span><span class="sxs-lookup"><span data-stu-id="66a12-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="66a12-179">Poniższy kod C# pokazano, jak mają być rejestrowane zdarzenia w kliencie programu .NET do pliku tekstowego:</span><span class="sxs-lookup"><span data-stu-id="66a12-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="66a12-180">Następujące dane wyjściowe zawiera wpisy z `ClientLog.txt` pliku przy użyciu tego pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="66a12-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="66a12-181">Zawiera klienta nawiązującego połączenie z serwerem i koncentrator wywołania metody klienta o nazwie `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="66a12-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="66a12-182">Włączanie śledzenia w klientach Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="66a12-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="66a12-183">Aplikacji SignalR dla aplikacji Windows Phone, użyj tego samego klienta .NET jako aplikacje komputerowe, ale [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) i zapisywanie do plików z [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="66a12-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="66a12-184">Zamiast tego należy utworzyć niestandardową implementację [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) śledzenia.</span><span class="sxs-lookup"><span data-stu-id="66a12-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="66a12-185">Rejestrowanie zdarzeń klienta Windows Phone do interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="66a12-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="66a12-186">[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) zawiera przykładowe Windows Phone, który zapisuje dane wyjściowe śledzenia do [blok tekstu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) przy użyciu niestandardowego [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) wdrożenia o nazwie `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="66a12-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="66a12-187">Ta klasa znajduje się w **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projektu.</span><span class="sxs-lookup"><span data-stu-id="66a12-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="66a12-188">Podczas tworzenia wystąpienia `TextBlockWriter`, należy przekazać w bieżącej [obiektu SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), a [Panel stosu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) których zostaną utworzone [blok tekstu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) do użycia na potrzeby śledzenia dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="66a12-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="66a12-189">Następnie dane wyjściowe śledzenia będą zapisywane na nowy [blok tekstu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) utworzone w [Panel stosu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) przekazany w:</span><span class="sxs-lookup"><span data-stu-id="66a12-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="66a12-190">Rejestrowanie zdarzeń klienta Windows Phone do konsoli debugowania</span><span class="sxs-lookup"><span data-stu-id="66a12-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="66a12-191">Aby wysłać dane wyjściowe do konsoli debugowania zamiast interfejsu użytkownika, Utwórz implementację [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) zapisuje do okna debugowania i przypisz je do tego połączenia [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) właściwości:</span><span class="sxs-lookup"><span data-stu-id="66a12-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="66a12-192">Informacje o śledzeniu następnie zostanie zapisany w oknie debugowania w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="66a12-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="66a12-193">Włączanie śledzenia w kliencie JavaScript</span><span class="sxs-lookup"><span data-stu-id="66a12-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="66a12-194">Aby włączyć rejestrowanie klienta połączenia, ustaw `logging` właściwości w obiekcie połączenia przed wywołaniem `start` metody do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="66a12-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="66a12-195">**Kod języka JavaScript klienta do włączania śledzenia w konsoli przeglądarki (z wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="66a12-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="66a12-196">**Kod języka JavaScript klienta do włączania śledzenia w konsoli przeglądarki (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="66a12-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="66a12-197">Gdy śledzenie jest włączone, klient JavaScript rejestruje zdarzenia w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="66a12-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="66a12-198">Aby uzyskać dostęp do konsoli przeglądarki, zobacz [monitorowania transportów](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="66a12-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="66a12-199">Poniższy zrzut ekranu przedstawia klienta SignalR JavaScript z włączonym śledzeniem.</span><span class="sxs-lookup"><span data-stu-id="66a12-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="66a12-200">Pokazuje połączenia i zdarzenia wywołania koncentratora w konsoli przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="66a12-200">It shows connection and hub invocation events in the browser console:</span></span>

![Zdarzenia śledzenia SignalR w konsoli przeglądarki](enabling-signalr-tracing/_static/image4.png)
