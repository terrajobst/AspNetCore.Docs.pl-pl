---
uid: signalr/overview/performance/signalr-performance
title: "SignalR wydajności | Dokumentacja firmy Microsoft"
author: pfletcher
description: "SignalR wydajności"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: dec2602e47fbcb838643a506a7e3feebda9d9c81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance"></a><span data-ttu-id="f4b76-103">SignalR wydajności</span><span class="sxs-lookup"><span data-stu-id="f4b76-103">SignalR Performance</span></span>
====================
<span data-ttu-id="f4b76-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f4b76-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="f4b76-105">W tym temacie opisano, jak projektować pod kątem pomiaru i zwiększyć wydajność w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="f4b76-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f4b76-106">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="f4b76-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="f4b76-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f4b76-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f4b76-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f4b76-108">.NET 4.5</span></span>
> - <span data-ttu-id="f4b76-109">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="f4b76-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f4b76-110">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="f4b76-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="f4b76-111">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="f4b76-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f4b76-112">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="f4b76-112">Questions and comments</span></span>
> 
> <span data-ttu-id="f4b76-113">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="f4b76-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f4b76-114">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f4b76-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f4b76-115">Ostatnie prezentacji na SignalR wydajności i skalowania, zobacz [skalowanie sieci Web w czasie rzeczywistym za pomocą programu ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="f4b76-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="f4b76-116">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="f4b76-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f4b76-117">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="f4b76-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="f4b76-118">Dostrajanie wydajności serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="f4b76-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="f4b76-119">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="f4b76-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="f4b76-120">Za pomocą liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="f4b76-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="f4b76-121">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="f4b76-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="f4b76-122">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="f4b76-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="f4b76-123">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="f4b76-123">Design considerations</span></span>

<span data-ttu-id="f4b76-124">W tej sekcji opisano wzorców, które można zastosować podczas projektowania aplikacji SignalR, aby upewnić się, że wydajność jest nie utrudniona, generując ruchu sieciowego niepotrzebne.</span><span class="sxs-lookup"><span data-stu-id="f4b76-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="f4b76-125">Ograniczanie częstotliwość komunikatów</span><span class="sxs-lookup"><span data-stu-id="f4b76-125">Throttling message frequency</span></span>

<span data-ttu-id="f4b76-126">Nawet w przypadku aplikacji, która wysyła komunikaty wysoka częstotliwość (na przykład w czasie rzeczywistym gry aplikacji) większość aplikacji, nie trzeba więcej niż kilka wiadomości na sekundę.</span><span class="sxs-lookup"><span data-stu-id="f4b76-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="f4b76-127">Aby zmniejszyć ilość ruchu sieciowego, który generuje każdego klienta, można zaimplementować pętli komunikatów wiadomości nie częściej niż stały szybkość kolejek i wysyła się (to znaczy do określonej liczby wiadomości zostaną wysłane co sekundę w przypadku komunikatów w tym czasie w terval do wysłania).</span><span class="sxs-lookup"><span data-stu-id="f4b76-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="f4b76-128">Przykładową aplikację, która ogranicza wiadomości do niektórych wartości (od klienta i serwera), zobacz [czasu rzeczywistego wysokiej częstotliwości z SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="f4b76-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="f4b76-129">Zmniejszenie rozmiaru wiadomości</span><span class="sxs-lookup"><span data-stu-id="f4b76-129">Reducing message size</span></span>

<span data-ttu-id="f4b76-130">Rozmiar obiektów serializowanych, można zmniejszyć rozmiar komunikatu SignalR.</span><span class="sxs-lookup"><span data-stu-id="f4b76-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="f4b76-131">W kodzie serwera wysyłasz obiekt, który zawiera właściwości, które nie muszą być przekazywane, uniemożliwi te właściwości poddany serializacji przy użyciu `JsonIgnore` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f4b76-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="f4b76-132">Nazwy właściwości są także przechowywane w komunikacie; nazwy właściwości może zostać skrócony przy użyciu `JsonProperty` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f4b76-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="f4b76-133">W poniższym przykładzie kodu pokazano wykluczać właściwość zostały wysłane do klienta oraz skrócić nazwy właściwości:</span><span class="sxs-lookup"><span data-stu-id="f4b76-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="f4b76-134">**Kod serwera .NET, który demonstruje atrybutu JsonIgnore wykluczanie danych zostały wysłane do klienta, a atrybut JsonProperty, aby zmniejszyć rozmiar wiadomości**</span><span class="sxs-lookup"><span data-stu-id="f4b76-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="f4b76-135">Aby zachować czytelność / utrzymanie kodu klienta, nazwy skróconej właściwości mogą być ponownie zmapowany do personelu przyjaznych nazw po odebraniu komunikatu.</span><span class="sxs-lookup"><span data-stu-id="f4b76-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="f4b76-136">W poniższym przykładzie kodu pokazano możliwe sposób ponownego mapowania skrócone nazwy do tych dłużej, definiując kontraktu komunikatu (mapowanie) i przy użyciu `reMap` funkcja dotyczyć kontrakt klasy zoptymalizowane wiadomości:</span><span class="sxs-lookup"><span data-stu-id="f4b76-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="f4b76-137">**Kod JavaScript po stronie klienta, który ponownie mapuje skrócony nazw właściwości do nazwy zrozumiałą dla użytkownika**</span><span class="sxs-lookup"><span data-stu-id="f4b76-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="f4b76-138">Nazwy mogą skrócony w wiadomościach z klienta do serwera, jak również za pomocą tej samej metody.</span><span class="sxs-lookup"><span data-stu-id="f4b76-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="f4b76-139">Zmniejszenie zużycia pamięci (to znaczy ilość pamięci użytej wiadomości) komunikatu obiektu mogą także podnieść wydajność.</span><span class="sxs-lookup"><span data-stu-id="f4b76-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="f4b76-140">Na przykład jeśli pełny zakres `int` nie jest potrzebny, `short` lub `byte` może być użyty.</span><span class="sxs-lookup"><span data-stu-id="f4b76-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="f4b76-141">Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie jego rozmiar wiadomości, można także rozwiązać problemy pamięci serwera.</span><span class="sxs-lookup"><span data-stu-id="f4b76-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="f4b76-142">Dostrajanie wydajności serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="f4b76-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="f4b76-143">Następujące ustawienia konfiguracji może służyć do dostrojenia serwera w celu poprawy wydajności w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="f4b76-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="f4b76-144">Aby uzyskać ogólne informacje na temat sposobu zwiększenia wydajności w aplikacji ASP.NET, zobacz [poprawę wydajności ASP.NET](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="f4b76-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span></span>

<span data-ttu-id="f4b76-145">**Ustawienia konfiguracji SignalR**</span><span class="sxs-lookup"><span data-stu-id="f4b76-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="f4b76-146">**DefaultMessageBufferSize**: domyślnie SignalR zachowuje 1000 komunikatów w pamięci dla koncentratora dla każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="f4b76-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="f4b76-147">Jeśli są używane duże wiadomości, może to spowodować problemy z pamięcią, które mogą być złagodzone przez zmniejszenie tej wartości.</span><span class="sxs-lookup"><span data-stu-id="f4b76-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="f4b76-148">To ustawienie można ustawić w `Application_Start` obsługi zdarzeń w aplikacji ASP.NET lub `Configuration` metody klasy początkowej OWIN w siebie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4b76-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="f4b76-149">W poniższym przykładzie pokazano sposób Zmniejsz tę wartość, aby zmniejszyć ilość pamięci zajmowaną przez aplikację, aby zmniejszyć ilość używanej pamięci serwera:</span><span class="sxs-lookup"><span data-stu-id="f4b76-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="f4b76-150">**.NET server kod w pliku Startup.cs zmniejszenie domyślny rozmiar buforu komunikatu**</span><span class="sxs-lookup"><span data-stu-id="f4b76-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="f4b76-151">**Ustawienia konfiguracji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="f4b76-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="f4b76-152">**Maksymalna liczba jednoczesnych żądań na aplikację**: zwiększenie liczby równoczesnych usługi IIS żądań spowoduje zwiększenie zasobów serwera dostępne dla obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="f4b76-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="f4b76-153">Wartość domyślna to 5000; Aby zwiększyć to ustawienie, uruchom następujące polecenia w wierszu polecenia z pełnymi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="f4b76-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="f4b76-154">**ApplicationPool QueueLength**: jest to maksymalna liczba żądań, że sterownik Http.sys umieszcza w kolejce dla puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4b76-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="f4b76-155">Gdy kolejka jest zapełniona, nowe żądania otrzymują odpowiedź 503 "Usługa niedostępna".</span><span class="sxs-lookup"><span data-stu-id="f4b76-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="f4b76-156">Wartość domyślna to 1000.</span><span class="sxs-lookup"><span data-stu-id="f4b76-156">The default value is 1000.</span></span>

    <span data-ttu-id="f4b76-157">Skrócić długość kolejki dla procesu roboczego w puli aplikacji będą zaoszczędzenia zasobów pamięci.</span><span class="sxs-lookup"><span data-stu-id="f4b76-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="f4b76-158">Aby uzyskać więcej informacji, zobacz [dostrajania, konfigurowanie pul aplikacji i zarządzanie nimi](https://technet.microsoft.com/en-us/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="f4b76-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/en-us/library/cc745955.aspx).</span></span>

<span data-ttu-id="f4b76-159">**Ustawienia konfiguracji programu ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="f4b76-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="f4b76-160">Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w `aspnet.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="f4b76-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="f4b76-161">Ten plik znajduje się w dwóch miejscach, w zależności od platformy:</span><span class="sxs-lookup"><span data-stu-id="f4b76-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="f4b76-162">Ustawienia programu ASP.NET, które może poprawić wydajność SignalR są następujące:</span><span class="sxs-lookup"><span data-stu-id="f4b76-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="f4b76-163">**Maksymalna liczba równoczesnych żądań dla każdego procesora CPU**: zwiększenie tego ustawienia może zlikwidować wąskie gardła wydajności.</span><span class="sxs-lookup"><span data-stu-id="f4b76-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="f4b76-164">Aby zwiększyć to ustawienie, Dodaj następujące ustawienie konfiguracji do `aspnet.config` pliku:</span><span class="sxs-lookup"><span data-stu-id="f4b76-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="f4b76-165">**Ograniczenie kolejki żądań**: gdy przekracza całkowitą liczbę połączeń `maxConcurrentRequestsPerCPU` ustawienie ASP.NET rozpocznie ograniczania przy użyciu kolejki żądań.</span><span class="sxs-lookup"><span data-stu-id="f4b76-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="f4b76-166">Aby zwiększyć rozmiar kolejki, można zwiększyć `requestQueueLimit` ustawienie.</span><span class="sxs-lookup"><span data-stu-id="f4b76-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="f4b76-167">Aby to zrobić, Dodaj następujące ustawienie konfiguracji, aby `processModel` w węźle `config/machine.config` (zamiast `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="f4b76-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="f4b76-168">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="f4b76-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="f4b76-169">W tej sekcji opisano sposoby znajdowania wąskich gardeł zmniejszających wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4b76-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="f4b76-170">Weryfikowanie, że używany jest protokół WebSocket</span><span class="sxs-lookup"><span data-stu-id="f4b76-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="f4b76-171">Podczas SignalR można używać różnych transportu do komunikacji między klientem a serwerem, WebSocket zapewnia korzyści znaczących wydajności i powinien być używany, jeśli klient i serwer obsługuje.</span><span class="sxs-lookup"><span data-stu-id="f4b76-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="f4b76-172">Aby określić, jeśli Twoje klienta i serwera spełniają wymagania protokołu WebSocket, zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f4b76-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="f4b76-173">Aby ustalić, jakie transportu jest używany w aplikacji, można użyć narzędzi deweloperskich przeglądarki i sprawdź dzienniki, aby zobaczyć, jakie transportu jest używany przez połączenie.</span><span class="sxs-lookup"><span data-stu-id="f4b76-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="f4b76-174">Aby uzyskać informacje na temat używania narzędzia programistyczne przeglądarki w programie Internet Explorer i Chrome, zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f4b76-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="f4b76-175">Za pomocą liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="f4b76-175">Using SignalR performance counters</span></span>

<span data-ttu-id="f4b76-176">W tej sekcji opisano, jak włączanie i używanie liczniki wydajności SignalR, w `Microsoft.AspNet.SignalR.Utils` pakietu.</span><span class="sxs-lookup"><span data-stu-id="f4b76-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="f4b76-177">Instalowanie signalr.exe</span><span class="sxs-lookup"><span data-stu-id="f4b76-177">Installing signalr.exe</span></span>

<span data-ttu-id="f4b76-178">Liczniki wydajności można dodać do serwera przy użyciu narzędzia, nazywany SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="f4b76-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="f4b76-179">Aby zainstalować to narzędzie, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="f4b76-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="f4b76-180">W aplikacji Visual Studio, wybierz **narzędzia**, **Menedżer pakietów biblioteki**, **Zarządzaj pakietami NuGet rozwiązania...**</span><span class="sxs-lookup"><span data-stu-id="f4b76-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="f4b76-181">Wyszukaj **signalr.utils**i wybierz opcję instalacji.</span><span class="sxs-lookup"><span data-stu-id="f4b76-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="f4b76-182">Zaakceptuj Umowę licencyjną, aby zainstalować pakiet.</span><span class="sxs-lookup"><span data-stu-id="f4b76-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="f4b76-183">Zostanie zainstalowana SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="f4b76-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="f4b76-184">Instalowanie liczników wydajności z SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="f4b76-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="f4b76-185">Aby zainstalować liczniki wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, z następującym parametrem:</span><span class="sxs-lookup"><span data-stu-id="f4b76-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="f4b76-186">Aby usunąć liczniki wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, z następującym parametrem:</span><span class="sxs-lookup"><span data-stu-id="f4b76-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="f4b76-187">Liczniki wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="f4b76-187">SignalR Performance counters</span></span>

<span data-ttu-id="f4b76-188">Pakiet narzędzi instaluje następujących liczników wydajności.</span><span class="sxs-lookup"><span data-stu-id="f4b76-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="f4b76-189">"Całkowita" liczniki mierzenia liczby zdarzeń od czasu ostatniego puli aplikacji lub serwera ponownego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="f4b76-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="f4b76-190">**Metryki połączenia**</span><span class="sxs-lookup"><span data-stu-id="f4b76-190">**Connection metrics**</span></span>

<span data-ttu-id="f4b76-191">Następujące metryki pomiaru połączenia okres istnienia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="f4b76-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="f4b76-192">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługi zdarzeń okres istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="f4b76-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="f4b76-193">**Połączeń**</span><span class="sxs-lookup"><span data-stu-id="f4b76-193">**Connections Connected**</span></span>
- <span data-ttu-id="f4b76-194">**Ponowne łączenie zakończone połączeń**</span><span class="sxs-lookup"><span data-stu-id="f4b76-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="f4b76-195">**Połączenia rozłączony**</span><span class="sxs-lookup"><span data-stu-id="f4b76-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="f4b76-196">**Bieżące połączenia**</span><span class="sxs-lookup"><span data-stu-id="f4b76-196">**Connections Current**</span></span>

<span data-ttu-id="f4b76-197">**Metryki wiadomości**</span><span class="sxs-lookup"><span data-stu-id="f4b76-197">**Message metrics**</span></span>

<span data-ttu-id="f4b76-198">Następujące metryki pomiaru ruch wiadomości generowany przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="f4b76-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="f4b76-199">**Odebrane komunikaty połączenia danych całkowitych**</span><span class="sxs-lookup"><span data-stu-id="f4b76-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="f4b76-200">**Całkowita liczba wysyłane komunikaty połączenia**</span><span class="sxs-lookup"><span data-stu-id="f4b76-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="f4b76-201">**Komunikaty połączeń odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="f4b76-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="f4b76-202">**Połączenie komunikaty wysłane/s**</span><span class="sxs-lookup"><span data-stu-id="f4b76-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="f4b76-203">**Metryki magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="f4b76-203">**Message bus metrics**</span></span>

<span data-ttu-id="f4b76-204">Następujące metryki pomiaru ruchu za pośrednictwem wewnętrznego magistrali komunikatu SignalR, kolejki, w którym znajdują się wszystkie komunikaty przychodzące i wychodzące SignalR.</span><span class="sxs-lookup"><span data-stu-id="f4b76-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="f4b76-205">Komunikat jest **opublikowano** po jest wysyłana lub emisji.</span><span class="sxs-lookup"><span data-stu-id="f4b76-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="f4b76-206">A **subskrybenta** w tym kontekście jest subskrypcji na magistrali komunikatu; ta powinna być równa liczbę klientów oraz sam serwer.</span><span class="sxs-lookup"><span data-stu-id="f4b76-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="f4b76-207">**Przydzielone procesu roboczego** jest składnikiem, który wysyła dane do aktywnych połączeń; **zajęty procesu roboczego** to taki, który jest aktywnie wysłanie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="f4b76-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="f4b76-208">**Całkowita liczba odebranych komunikatów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="f4b76-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="f4b76-209">**Magistrala komunikatów wiadomości odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="f4b76-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="f4b76-210">**Komunikatów magistrali komunikatów publikowanych danych całkowitych**</span><span class="sxs-lookup"><span data-stu-id="f4b76-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="f4b76-211">**Magistrala komunikatu komunikaty opublikowane na sekundę**</span><span class="sxs-lookup"><span data-stu-id="f4b76-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="f4b76-212">**Bieżąca subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="f4b76-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="f4b76-213">**Łączna liczba subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="f4b76-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="f4b76-214">**Subskrybentów magistrali komunikatów na sekundę**</span><span class="sxs-lookup"><span data-stu-id="f4b76-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="f4b76-215">**Magistrala komunikatów przydzielonych pracowników**</span><span class="sxs-lookup"><span data-stu-id="f4b76-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="f4b76-216">**Zajętych pracowników magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="f4b76-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="f4b76-217">**Bieżąca tematy magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="f4b76-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="f4b76-218">**Błąd metryk**</span><span class="sxs-lookup"><span data-stu-id="f4b76-218">**Error metrics**</span></span>

<span data-ttu-id="f4b76-219">Następujące metryki pomiaru błędy generowane przez ruch komunikatów SignalR.</span><span class="sxs-lookup"><span data-stu-id="f4b76-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="f4b76-220">**Rozpoznawania koncentratora** błędy występują, gdy nie można ustalić koncentratora lub metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="f4b76-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="f4b76-221">**Wywołania koncentratora** błędy są wyjątki zgłaszane podczas wywoływania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="f4b76-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="f4b76-222">**Transport** wyjątek podczas żądania lub odpowiedzi HTTP błędów połączeń są błędy.</span><span class="sxs-lookup"><span data-stu-id="f4b76-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="f4b76-223">**Błędów: Suma wszystkich**</span><span class="sxs-lookup"><span data-stu-id="f4b76-223">**Errors: All Total**</span></span>
- <span data-ttu-id="f4b76-224">**Błędy: All/s**</span><span class="sxs-lookup"><span data-stu-id="f4b76-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="f4b76-225">**Błędy: Całkowita liczba rozpoznawania koncentratora**</span><span class="sxs-lookup"><span data-stu-id="f4b76-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="f4b76-226">**Błędy: Rozpoznawania koncentratora na sekundę**</span><span class="sxs-lookup"><span data-stu-id="f4b76-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="f4b76-227">**Błędy: Całkowita liczba wywołania koncentratora**</span><span class="sxs-lookup"><span data-stu-id="f4b76-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="f4b76-228">**Błędy: Wywołania koncentratora na sekundę**</span><span class="sxs-lookup"><span data-stu-id="f4b76-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="f4b76-229">**Błędy: Całkowita liczba transportu**</span><span class="sxs-lookup"><span data-stu-id="f4b76-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="f4b76-230">**Błędy: Transportu na sekundę**</span><span class="sxs-lookup"><span data-stu-id="f4b76-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="f4b76-231">**Metryki skalowania**</span><span class="sxs-lookup"><span data-stu-id="f4b76-231">**Scaleout metrics**</span></span>

<span data-ttu-id="f4b76-232">Następujące metryki pomiaru ruchu i błędów wygenerowanych przez dostawcę skalowania w poziomie.</span><span class="sxs-lookup"><span data-stu-id="f4b76-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="f4b76-233">A **strumienia** w tym kontekście jest jednostką skalowania jest używany przez dostawcę skalowania, co jest tabeli, jeśli jest używany program SQL Server, tematu, jeśli jest używana usługa Service Bus i subskrypcji, użycie pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="f4b76-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="f4b76-234">Zapewnia każdego strumienia uporządkowanej odczytu i zapisu; jednego strumienia jest potencjalne wąskie gardło skali, tak aby pomóc w zmniejszeniu tego "wąskie gardło" można zwiększyć liczbę strumieni.</span><span class="sxs-lookup"><span data-stu-id="f4b76-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="f4b76-235">Użycie wielu strumieni SignalR automatycznej dystrybucji między tych strumieni w sposób, który gwarantuje, że wiadomości wysłane z dowolnego danego połączenia są w kolejności wiadomości (niezależnego fragmentu).</span><span class="sxs-lookup"><span data-stu-id="f4b76-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="f4b76-236">[MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) kontroluje długość kolejki wysyłania skalowania obsługiwanego przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="f4b76-236">The [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="f4b76-237">Zostanie ustawiona na wartość większą niż 0 zostaną umieszczone wszystkie wiadomości w kolejce wysyłania do wysłania pojedynczo skonfigurowanych płyty montażowej obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="f4b76-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="f4b76-238">Rozmiar kolejki przekracza skonfigurowany limit, wezwań do wysyłania będzie działać natychmiast z [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) dopóki liczba wiadomości w kolejce jest mniejsza niż wartość ustawienia ponownie.</span><span class="sxs-lookup"><span data-stu-id="f4b76-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="f4b76-239">Kolejkowanie jest domyślnie wyłączona, ponieważ implementowane montażowych mają zwykle własne usługi kolejkowania wiadomości lub Sterowanie przepływem w miejscu.</span><span class="sxs-lookup"><span data-stu-id="f4b76-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="f4b76-240">W przypadku programu SQL Server efektywnie puli połączeń ogranicza liczbę wysyła przejściem w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="f4b76-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="f4b76-241">Domyślnie tylko jeden strumień jest używany do programu SQL Server i pamięci podręcznej Redis, pięć strumienie są używane dla usługi Service Bus i Kolejkowanie jest wyłączona, ale te ustawienia można zmienić za pomocą konfiguracji na serwerze SQL i usługi Service Bus:</span><span class="sxs-lookup"><span data-stu-id="f4b76-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="f4b76-242">**Kod serwera .NET do konfigurowania tabeli długość kolejki i liczba dla środowiska IDE programu SQL Server**</span><span class="sxs-lookup"><span data-stu-id="f4b76-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="f4b76-243">**Konfigurowanie długość kolejki i liczba tematu usługi Service Bus płyty montażowej kod serwera .NET**</span><span class="sxs-lookup"><span data-stu-id="f4b76-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="f4b76-244">A **buforowanie** strumienia, który przeszedł w stan; gdy strumień jest w stanie błędnej, wszystkie komunikaty wysyłane na płytę montażową zakończy się niepowodzeniem aż do strumienia nie spowodował błąd.</span><span class="sxs-lookup"><span data-stu-id="f4b76-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="f4b76-245">**Długość kolejki wysyłania** jest liczba komunikatów, które zostały przesłane, ale nie zostały jeszcze wysłane.</span><span class="sxs-lookup"><span data-stu-id="f4b76-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="f4b76-246">**Magistrali komunikatów skalowania wiadomości odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="f4b76-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="f4b76-247">**Całkowita liczba strumieni skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="f4b76-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="f4b76-248">**Otwórz strumieni skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="f4b76-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="f4b76-249">**Strumienie skalowania buforowania**</span><span class="sxs-lookup"><span data-stu-id="f4b76-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="f4b76-250">**Całkowita liczba błędów skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="f4b76-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="f4b76-251">**Błędów skalowania na sekundę**</span><span class="sxs-lookup"><span data-stu-id="f4b76-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="f4b76-252">**Długość kolejki wysyłania skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="f4b76-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="f4b76-253">Aby uzyskać więcej informacji na jakie te liczniki są pomiaru, zobacz [skalowania SignalR z usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="f4b76-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="f4b76-254">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="f4b76-254">Using other performance counters</span></span>

<span data-ttu-id="f4b76-255">Następujące liczniki wydajności mogą być również przydatne w ramach monitorowania wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4b76-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="f4b76-256">**Pamięci**</span><span class="sxs-lookup"><span data-stu-id="f4b76-256">**Memory**</span></span>

- <span data-ttu-id="f4b76-257">.NET CLR pamięci\\liczba bajtów we wszystkich sterty (dla w3wp)</span><span class="sxs-lookup"><span data-stu-id="f4b76-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="f4b76-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="f4b76-258">**ASP.NET**</span></span>

- <span data-ttu-id="f4b76-259">Bieżąca ASP.NET\Requests</span><span class="sxs-lookup"><span data-stu-id="f4b76-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="f4b76-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="f4b76-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="f4b76-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="f4b76-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="f4b76-262">**PROCESOR CPU**</span><span class="sxs-lookup"><span data-stu-id="f4b76-262">**CPU**</span></span>

- <span data-ttu-id="f4b76-263">Czas procesora Information\Processor</span><span class="sxs-lookup"><span data-stu-id="f4b76-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="f4b76-264">**PROTOKÓŁ TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="f4b76-264">**TCP/IP**</span></span>

- <span data-ttu-id="f4b76-265">TCPv6/połączeń</span><span class="sxs-lookup"><span data-stu-id="f4b76-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="f4b76-266">TCPv4/połączeń</span><span class="sxs-lookup"><span data-stu-id="f4b76-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="f4b76-267">**Usługi sieci Web**</span><span class="sxs-lookup"><span data-stu-id="f4b76-267">**Web Service**</span></span>

- <span data-ttu-id="f4b76-268">Połączenia Service\Current sieci Web</span><span class="sxs-lookup"><span data-stu-id="f4b76-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="f4b76-269">Połączenia Service\Maximum sieci Web</span><span class="sxs-lookup"><span data-stu-id="f4b76-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="f4b76-270">**Wątkowość**</span><span class="sxs-lookup"><span data-stu-id="f4b76-270">**Threading**</span></span>

- <span data-ttu-id="f4b76-271">.NET CLR blokuje i wątków\\# aktualna liczba wątków logicznych</span><span class="sxs-lookup"><span data-stu-id="f4b76-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="f4b76-272">.NET CLR blokuje i wątków\\# aktualna liczba wątków fizycznych</span><span class="sxs-lookup"><span data-stu-id="f4b76-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="f4b76-273">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="f4b76-273">Other Resources</span></span>

<span data-ttu-id="f4b76-274">Aby uzyskać więcej informacji na temat wydajności programu ASP.NET, monitorowania i dostrajania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="f4b76-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="f4b76-275">[Omówienie wydajności programu ASP.NET](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="f4b76-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="f4b76-276">Użycie wątku ASP.NET usług IIS 7.5, usługi IIS 7.0 i IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="f4b76-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="f4b76-277">&lt;applicationPool&gt; elementu (ustawienia sieci Web)</span><span class="sxs-lookup"><span data-stu-id="f4b76-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
